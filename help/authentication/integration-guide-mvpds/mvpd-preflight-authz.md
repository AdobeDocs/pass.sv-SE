---
title: MVPD Preflight-auktorisering
description: MVPD Preflight-auktorisering
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# MVPD Preflight-auktorisering

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#mvpd-preflight-authz-intro}

&quot;Preflight-auktorisering&quot; är en enkel behörighetskontroll för flera resurser. Programmerare använder den främst för att dekorera sina användargränssnitt (till exempel för att ange åtkomststatus med lås- och upplåsningsikoner).

Adobe Pass Authentication har för närvarande stöd för Preflight Authorization på två sätt för MVPD, antingen via AuthN-svarsattribut eller via en AuthZ-begäran i flera kanaler.  I följande scenarier beskrivs kostnaden/nyttan av olika sätt att implementera preflight-auktorisering:

* **Best Case Scenario** - MVPD tillhandahåller en lista över förauktoriserade resurser under auktoriseringsfasen (Multi-channel AuthZ).
* **Sämsta scenariot** - Om en MVPD inte har stöd för någon form av auktorisering med flera resurser, utför Adobe Pass autentiseringsserver ett auktoriseringsanrop till MVPD för varje resurs i resurslistan. Det här scenariot påverkar svarstiden för preflight-auktoriseringsbegäran (i förhållande till antalet resurser). Det kan öka belastningen på både Adobe- och MVPD-servrar vilket kan orsaka prestandaproblem. Dessutom genereras auktoriseringsförfrågningar/svarshändelser utan behov av en uppspelning.
* **Föråldrat** - MVPD tillhandahåller en lista med förauktoriserade resurser under autentiseringsfasen, så det behövs inga nätverksanrop, inte ens preflight-begäran, eftersom listan cachelagras på klienten.

Även om programmeringsgränssnitten inte behöver stödja preflight-auktorisering, beskriver följande avsnitt några metoder för preflight-auktorisering som Adobe Pass Authentication kan stödja, innan de återgår till det värsta scenariot ovan.

## Preflight i AuthN {#preflight-authn}

Det här preflight-scenariot är OLCA-kompatibelt (Cableabs). I avsnitt 7.5.2 i specifikationen för Authentication and Authorization Interface 1.0 beskrivs hur ett SAML-autentiseringssvar kan innehålla en lista med förauktoriserade resurser. Om en IdP har stöd för detta kan Adobe Pass-autentiseringsservern generera den förfyllda resurslistan vid autentiseringen och cachelagra den på klienten tillsammans med autentiseringstoken. Den här metoden uppnår också det bästa scenariot och inga nätverksanrop utförs när programmeraren anropar checkPreauthorizedResources(), eftersom allt redan finns på klienten.

### Anpassad resurslista i SAML-attribututtryck {#custom-res-saml-attr}

IdP:s SAML-autentiseringssvar ska innehålla en AttributeStatement som innehåller resursnamn som AdobePass ska godkänna.  Vissa MVPD har följande format:

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

Exemplet ovan innehåller en lista med två förauktoriserade resurser:&quot;MMOD&quot; och&quot;Olympic2012&quot;.

Detta uppnår det bästa scenariot och inga nätverksanrop utförs när programmeraren anropar checkPreauthorizedResources() eftersom allt redan finns på klienten.

## Preflight för flera kanaler i AuthZ {#preflight-multich-authz}

Den här preflight-implementeringen är även kompatibel med OLCA (Cablelabs).  I specifikationen för Authentication and Authorization Interface 1.0 (avsnitten 7.5.3 och 7.5.4) beskrivs metoder för att begära auktoriseringsinformation från en MVPD med antingen SAML Assertions eller XACML. Det här är det rekommenderade sättet att fråga efter auktoriseringsstatus för MVPD som inte stöder detta som en del av autentiseringsflödet. Adobe Pass Authentication utfärdar ett enda nätverksanrop till MVPD för att hämta listan över auktoriserade resurser.


Adobe Pass Authentication tar emot listan över resurser från programmerarens program. Integreringen av Adobe Pass Authentication med MVPD kan sedan göra ett AuthZ-anrop med alla dessa resurser, analysera svaret och extrahera de olika besluten om tillstånd/neka.  Flödet för preflight med AuthZ-scenario med flera kanaler fungerar enligt följande:

1. Programmerarens app skickar en kommaavgränsad lista med resurser via preflight-klientens API, t.ex. &quot;TestChannel1,TestChannel2,TestChannel3&quot;.
1. MVPD preflight AuthZ-begäran innehåller flera resurser och har följande struktur:

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## Anpassad auktorisering för flera resurser {#custom-authz}

Vissa MVPD-filer har auktoriseringsslutpunkter som stöder auktorisering för flera resurser i en begäran, men de omfattas inte av scenariot som beskrivs i AuthZ för flera kanaler. Dessa specifika PDF-filer kräver anpassat arbete.

Adobe kan även stödja flerkanalsauktorisering utan ändringar i den befintliga implementeringen.  Detta tillvägagångssätt måste ses över mellan Adobe och MVPD tekniska team för att se till att det fungerar som förväntat.

## MVPD-filer som stöder Preflight-auktorisering {#mvpds-supp-preflight-authz}

I följande tabell visas de MVPD-filer som stöder Preflight-auktorisering, tillsammans med vilken typ av preflight de stöder och kända begränsningar:

| Preflight-metod | MVPD | Anteckningar |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| AuthZ med flera kanaler | Comcast AT&amp;T Proxy Clearleap Chart_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| Kanalindelning i användarmetadata | Suddenlink HTC | Alla Synacor Direct-integreringar har även stöd för den här metoden. |
| Förgrening | Alla andra som inte listas ovan | Det högsta tillåtna standardantalet resurser som kontrollerats = 5. |

