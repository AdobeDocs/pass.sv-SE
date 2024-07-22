---
title: MVPD-utbyte av användarmetadata
description: MVPD-utbyte av användarmetadata
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# MVPD-utbyte av användarmetadata

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro-user-metadata-exchange}

MVPD-program bevarar användarspecifika metadata om sina kunder som i vissa fall delas med programmerare. Syftet med Adobe Pass-autentisering är att förmedla ett utbyte av dessa &quot;användarmetadata&quot;, men inte att genomdriva några regler för utbytet. Utbytesreglerna är avsedda för programmeringsföretag att samarbeta med sina programmeringspartners.

Följande metadatatyper för användare finns för utbyte:

* Postnummer
* Högsta betyg (VChift eller MPAA)
* Användar-ID
* Hushållets-ID
* Kanal-ID

Med den här funktionen kan programmerare och programmerare implementera specialfall som föräldrakontroll. En MVPD kan till exempel skicka föräldraklassificeringsdata till en programmerare som sedan använder dessa data för att filtrera de tillgängliga visningsalternativen för en användare.

Nyckelpunkter för användarmetadata:

* MVPD skickar användarmetadata till programmerarens program under autentiserings- och auktoriseringsflödena
* Adobe Pass Authentication sparar metadatavärdena i AuthN- och AuthZ-tokens
* Adobe Pass Authentication kan normalisera värden för MVPD-filer som tillhandahåller användarmetadata i olika format
* Vissa parametrar kan krypteras med programmerarens nyckel
* Specifika värden är tillgängliga av Adobe via en konfigurationsändring

>[!NOTE]
>
>Användarmetadata är ett tillägg till de statiska metadata (autentiseringstoken TTL, auktoriseringstoken TTL och enhets-ID) som tidigare fanns i Adobe Pass Authentication.

## Exempel {#example-mvpd-user-metadata-exch}

### Föräldrakontroll {#example-parental-control}

I det här exemplet visas utbytet av följande:

* [Programmer till MVPD-metadatautbyte](#progr-mvpd-metadata-exch)

* [MVPD till programmerarens metadatautbytesflöde](#mvpd-progr-exchange-flow)

### Programmer till MVPD-metadatautbyte {#progr-mvpd-metadata-exch}

För närvarande stöder programmerings-API, Adobe Pass Authentication och MVPD Authorizers endast auktorisering på kanalnivå. Kanalen anges som en ren textsträng i programmerarens getAuthorization()-API-anrop. Strängen sprids ända till MVPD:s auktoriserande serverdel:

Från programmerarens app eller webbplats väljer användaren ett MVPD-program som stöder XACML (i det här exemplet&quot;TNT&quot;). Mer information om XACML finns i [eXtensible Access Control Markup Language](https://en.wikipedia.org/wiki/XACML){target=_blank}.
Programmerarens app utgör en AuthZ-begäran som innehåller resursen och dess metadata.  I det här exemplet ingår en MPAA-klassificering av &quot;pg&quot; i medieattributet för kanalelementet:

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

Adobe Pass Authentication har faktiskt stöd för mer detaljerad auktorisering, ända ned till tillgångsnivå, när det stöds av både MVPD och Programmer. Resursen och dess metadata är ogenomskinliga för Adobe. Avsikten är att skapa ett standardformat för att ange resurs-ID och metadata på ett normaliserat sätt och skicka resurs-ID:n till olika MVPD-filer.

>[!NOTE]
>
>Om användaren väljer ett kanalspecifikt kompatibelt MVPD extraherar Adobe Pass Authentication ENDAST kanaltiteln (&quot;TNT&quot; i exemplet ovan) och skickar endast titeln till MVPD.

### MVPD till programmerarens metadatautbytesflöde {#mvpd-progr-exchange-flow}

Adobe Pass Authentication gör följande antaganden:

* MVPD skickar den maximala graderingen som en del av SAML-svaret
* Den här informationen sparas som en del av autentiseringstoken
* Adobe Pass Authentication tillhandahåller ett API som gör det möjligt för programmerare att hämta denna information
* Programmerare implementerar den här funktionen på sin webbplats eller i sin app (t.ex. för att dölja videoklipp som är högre än användarens maxklassificering)

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### Anteckningar {#notes-mvpd-progr-metadata-exch-flow}

**Resursens normalisering och validering.** resurs-ID:n kan skickas som en oformaterad sträng eller en MRSS-sträng. En programmerare kan välja att använda antingen det rena strängformatet eller MRSS, men måste ha ett förhandsavtal med MVPD så att MVPD vet hur resursen ska hanteras.

**Ange resurs-ID och metadata.** Adobe Pass Authentication använder RSS-standarden med Media RSS-tillägget för att ange en resurs och dess metadata. I kombination med Media RSS-tillägget stöder Adobe Pass Authentication en mängd olika metadata, till exempel föräldrakontroller (via `<media:rating>`) eller geopositionering (`<media:location>`).

Adobe Pass Authentication kan även hantera genomskinlig konvertering från den äldre kanalsträngen till motsvarande RSS-resurs för MVPD-filer som kräver RSS. I den andra riktningen har Adobe Pass Authentication stöd för konvertering från RSS+MRSS till vanlig kanaltitel, för kanalspecifika MVPD-program.

**Adobe Pass-autentisering garanterar fullständig bakåtkompatibilitet med befintliga integreringar.** För programmerare som använder autentisering på kanalnivå, tar Adobe Pass Authentication hand om att paketera channel ID:t i det format som krävs innan det skickas till en MVPD som förstår det formatet. Det motsatta gäller också: om en programmerare anger alla resurser i ett nytt format, översätter Adobe Pass Authentication det nya formatet till en enkel kanalsträng om auktorisering görs mot en MVPD som bara gör kanalnivåauktorisering.

## Användningsexempel för användarmetadata {#user-metadata-use-cases}

Användningsexemplen förändras hela tiden och blir allt fler när fler PDF-filer skapar juridiska arrangemang och lägger till funktionalitet. Nedan följer exempel på vilka användarmetadata kan användas.

* [MVPD-användar-ID](#mvpd-user-id)
* [Hushållets-ID](#household-user-id)
* [Postnummer](#zip-code)
* [Maximal klassificering (Föräldrakontroll)](#max-rating-parental-control)
* [Kanallinje](#channel-line-up)

### MVPD-användar-ID {#mvpd-user-id}

* Enligt MVPD
* Inte användarens inloggningsinformation, eftersom den hashas av MVPD
* Kan användas för att indikera problem med eller för specifika användare
* Krypterad
* Stöd för MVPD: Alla MVPD

### Hushållets användar-ID {#household-user-id}

* Tillåter god måttinformation
* Krypterad
* Stöd för MVPD: Vissa MVPD

### Postnummer {#zip-code}

* Användarens postnummer för fakturering
* Används främst för att framtvinga regler för frysningsperiod för sportevenemang
* Kan anges med AuthZ-svaret för snabba uppdateringar
* Stöd för MVPD: Vissa MVPD

### Maximal klassificering (Föräldrakontroll) {#max-rating-parental-control}

* AuthN initialt, plus AuthZ-uppdatering
* Filtrera innehåll från användargränssnittet
* MPAA- eller VChift-klassificeringar
* Stöd för MVPD: Vissa MVPD

### Kanallinje {#channel-line-up}

* MVPD-program kan tillhandahålla en lista över kanaler som användaren har rätt att visa
* Gör det möjligt att snabbt måla i användargränssnittet
* OLCA-specifikationen tillåter detta som en AttributeStatement i AuthN-svaret
* Stöd för MVPD: Vissa MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
