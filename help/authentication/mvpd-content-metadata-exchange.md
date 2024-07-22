---
title: MVPD Content Metadata Exchange
description: MVPD Content Metadata Exchange
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# MVPD Content Metadata Exchange

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#content-metadat-exchange-overview}

På den här sidan beskrivs två standardimplementeringar som Adobe Pass Authentication använder för att skicka strukturerade data till MVPD på auktoriseringsbegäran.  De strukturerade data representerar den resurs (programmeraren) som gör begäran och, eventuellt, ytterligare data, till exempel klassificering av innehåll.

På programmerarsidan har Adobe Pass Authentication stöd för strukturerade MRSS-dataresurser enligt följande:

1. Programmeraren skickar resursen som en MRSS-sträng. Adobe Pass Authentication kodar inte den på klientsidan för webb- eller inbyggda enheter. MRSS skickas som en vanlig sträng till Adobe Pass-autentiseringsservern.
1. På serversidan valideras MRSS mot det fördefinierade schemat (http://search.yahoo.com/mrss/).  Om valideringen godkänns extraherar Adobe Pass Authentication informationen från MRSS-fälten, inklusive:
   * kanaltitel
   * objekttitel
   * resursidentifierare
   * Värderingsvärde och typ
1. De värden som extraheras från MRSS används för att skapa den auktoriseringsbegäran som skickas till MVPD.

Adobe Pass Authentication stöder två metoder för översättning av MRSS till format som stöds av MVPD:

* **XML**.  Den första metoden är anpassad efter OLCA-standarden.  Den använder XACML, där MRSS-värdena extraheras för att skapa en XACMLResource med attribut som mappar till MRSS-elementen.  Detta skickas sedan till MVPD.
* **REST**.  Den andra metoden är REST-baserad.  MRSS är base64-kodad och skickas som en URL-parameter i REST-anropet.

I båda metoderna behandlar det virtuella dokumentationsdokumentet tillståndsansökan genom att inkludera de extraherade värdena i sitt eget logiska flöde och returnera ett auktoriseringssvar.

## Integreringsinformation {#integration-details}

* OLCA-baserad strukturerad XACML-resurs
* REST-baserad strukturerad resurs

### OLCA-baserad strukturerad XACML-resurs {#olca-based-xacml-struc-resource}

De flesta kabelorienterade videobandspelare använder den XACML-baserade metoden, men stöder ännu inte den fullständiga strukturerade datainställningen.  Andra MVPD-program som stöder XACML tar kanaltiteln och godkänner den för attributet ResourceID. I exemplet nedan visas den fullständiga strukturerade XACML-baserade metoden. Adobe Pass Authentication-teamet rekommenderar att de för MVPD-program som använder XACML, men ännu inte har stöd för funktioner som föräldrakontroll, ska anpassa sin XACML-integrering till följande exempel:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### REST-baserad strukturerad resurs {#rest-based-struct-resource}

Vissa MVPD har standardiserat med följande REST-baserade protokoll för auktorisering. Den här metoden är lika komplett som XACML-metoden, men erbjuder en implementering med&quot;lättare vikt&quot;.

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
