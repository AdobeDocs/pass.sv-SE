---
title: Standardmetadataattribut
description: Standardmetadataattribut
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# Standardmetadataattribut {#std-metadata-attributes}

Den här sidan innehåller en fullständig lista över metadataattribut som tjänsten för övervakning av samtidig användning kan bearbeta och som kan användas som grund för policyer som kan implementeras. Standardmetadataattributen kan kategoriseras så här:

* Attribut som ingår i design (skickas vid varje sessionsinitieringsanrop, så som de krävs i URL-sökvägen). Inga giltiga anrop kan utföras utan dessa värden.
* Metadataattribut: värden som måste skickas som formulärdata under sessionsinitieringsanropet (om serverdelsprinciperna kräver sina värden).

## Attribut som krävs för design {#attr-req-by-design}

API:t för övervakning av samtidig användning tvingar klienter att skicka följande värden som en del av ett giltigt initieringsanrop: [initieringsanrop för session](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Fältnamn | Exempelvärde | Var den ska användas | Hämtat från |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Rubrik för auktorisering | Zendesk-biljett vid integration |
| mvpdName | Sample_MVPD | URI-sökväg | Adobe Pass Authentication från config endpoint när användaren väljer MVPD |
| accountId | 12345 | URI-sökväg | Adobe Pass Authentication upstreamUserID metadata after user login [User Metadata upstreamUserID - Adobe Pass Authentication](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## Metadataattribut {#metadata-attr}

Fälten i tabellen nedan kan användas av programmerare och distributörer av videoprogrammeringstjänster för att skapa profiler som ska implementeras i övervakning av samtidig användning.

Om något av dessa attribut krävs för de definierade profilerna med [API v2.0](http://docs.adobeptime.io/cm-api-v2/) resulterar ett försök att starta en session utan det attributet i en 400-felaktig begäran.


| Entitet | Attributnamn | Datatyp | Beskrivning | Extern referens (t.ex. EIDR, OATC) | Exempelvärde | Valideringsregler |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Medieföretag | programmerName | string | Programmerarens namn |                                                   | ProgrammerX |                                                                                   |
| Resurs | kanal | string | TV-kanalen |                                                   | ChannelY |                                                                                   |
|                 | assetId | string | Den&quot;vänliga&quot; eller läsbara titel som ska presenteras för innehållet | [Referens för EIDR 2.0-datafält](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | uppräkning | Ett värde som beskriver den allmänna innehållstypen som representeras av TveItem. De uppräknade värdena är: movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow clip concurrt Conference newsEvent sportevent trailer | [Rekommenderad praxis för OATC-metadatafeed](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | Fältet måste motsvara ett av objekten i uppräkningen |
|                 | contentType | string | Det här fältet avgör om det begärda innehållet är live eller VOD | Ej tillämpligt | live, vod | live eller vod |
|                 | genre | string | Innehållets genre som direktuppspelas. Beskriver den allmänna programmeringstypen | [Metadatafeed för OATC rekommenderas](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | Komedi | Giltig genre-typ |
|                 | varaktighet | tal | Medieobjektets varaktighet i sekunder | [Rekommenderad praxis för OATC-metadatafeed](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | nummersekvens |
| Enhet/webbläsare | deviceId | string | Unik enhets-ID. | [Egenskaper för enhetskarta](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | string | Enhetens egna namn. |                                                   | Joe&#39;s iPad |                                                                                   |
|                 | marketingName | string | Marknadsföringsnamnet (eller det kundvänliga namnet) för en enhet | [Egenskaper för enhetskarta](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | giltigt marknadsföringsnamn |
|                 | mobileDevice | boolesk | True om enheten är avsedd att användas på resande fot | [Egenskaper för enhetskarta](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | string | Modellnamnet för enheten, webbläsaren eller den andra komponenten | [Egenskaper för enhetskarta](https://deviceatlas.com/device-data/properties){target=_blank} | tablet, telefon, xbox. digitalbox | giltigt enhetsmodellnamn |
|                 | osName | string | Operativsystemet som enheten körs på | [Enhetskarta - fördefinierade egenskapsvärden för operativsystemet](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Annat: Du måste vara inloggad med ett användarnamn och lösenord i Device Atlas för att kunna visa egenskapsvärdena | det förväntade värdet är ett av värdena i de fördefinierade egenskaperna för enhetskartan |
|                 | browserName | string | Namnet eller typen av webbläsare på enheten | [Enhetskarta - fördefinierade egenskapsvärden för webbläsaren](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | Namnet på eller typen av webbläsare på enheten.  Obs! Du måste vara inloggad med ett användarnamn och lösenord i enhetskartan för att kunna visa egenskapsvärden | det förväntade värdet är ett av värdena i de fördefinierade egenskaperna för enhetskartan |
|                 | browserVersion | string | Webbläsarversionen på enheten | [Egenskaper för enhetskarta](https://deviceatlas.com/device-data/properties){target=_blank} | Webbläsarversionen på enheten |                                                                                   |
| Program | applicationName | string | Det användarvänliga eller konsumentläsbara namnet på programmet | Ej tillämpligt | Sample_Application |                                                                                   |
|                 | applicationId | string | Program-ID som unikt identifierar ett klientprogram. | Ej tillämpligt | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | string | Programmets interna plattform | Ej tillämpligt | ios, android |                                                                                   |
|                 | applicationVersion | string | Det här värdet kan användas i analyssyfte | Ej tillämpligt | 1.0, 2.0 |                                                                                   |
| Ämne | accountId | string | Konto-ID för Concurrency Monitoring-ämnet (i MVPD-området) | Ej tillämpligt | test-account |                                                                                   |
|                 | contractType | string | Premium, grundläggande. Kunderna kan själva lägga till detta som anpassade metadata och använda det i sina egna domäner | Ej tillämpligt | premie, grundläggande |                                                                                   |
| Användare | name | string | Vissa MVPD-program tillhandahåller information om den specifika användaren som spelar upp innehåll. | Ej tillämpligt |                                                                                                                                                         |                                                                                   |
|                 | hba | boolesk | Identifierar om användaren försöker initiera strömmen från sin hemplats | Ej tillämpligt | true, false | true eller false |
| Plats | kontinent | string | Den kontinent där det deviceID som skickar uppspelningsbegäran kommer från | Ej tillämpligt | Nordamerika | giltigt namn på kontinenten |
|                 | land | string | Landet som det deviceID som skickar uppspelningsbegäran kommer från | Ej tillämpligt | USA | giltigt land |
|                 | läge | string | Tillståndet som det deviceID som skickar uppspelningsbegäran kommer från | Ej tillämpligt | CA | giltigt tillståndsnamn |
|                 | stad | string | Ort som det deviceID som skickar uppspelningsbegäran kommer från | Ej tillämpligt | Cupertino | giltigt ort |
|                 | zipcode | tal | Postnumret som det deviceID som skickar uppspelningsbegäran kommer från | Ej tillämpligt | 95014 | giltig zipcode |
| Strömma | streamId | string | Genereras av CM-tjänsten, inte av kundkontroll. Används implicit när regler av typen maxstreams definieras. | Ej tillämpligt | Ej tillämpligt | Ej tillämpligt |
|                 | streamCDN | string | anger det CDN från vilket strömmen hämtades | Ej tillämpligt | Ej tillämpligt | Ej tillämpligt |

## Exempel på hur du använder metadataattribut för att skapa principer {#examples-metadata-attr}

Standardmetadatafälten kan användas för att definiera serversidans principer baserat på deras fältvärden:

* Du kan konfigurera en princip som bara ska gälla för specifika fältvärden (till exempel en dedikerad iOS-princip: där `osType` är `iOS`)
* Du kan begränsa antalet distinkta värden för ett visst fält. Nedan följer några exempel:
   * inte fler än X olika enheter: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * inte fler än X distinkta postnummer: `HAVING DISTINCT COUNT(zipcode) <= 3`
* Du kan begränsa antalet aktiva strömmar per fältvärde. Nedan följer några exempel:
   * inte fler än X aktiva strömmar för en enskild enhetstyp: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * inte fler än X aktiva strömmar för strömmar med liveinnehåll: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Kontakta teamet för övervakning av samtidig användning (Concurrency Monitoring) genom att [skapa en biljett i Zendesk](mailto:tve-support@adobe.com) och ange vilka principer du vill ha implementerat.

Här finns fler exempel på principer och integreringscookbooks:

* [Policybeslutspunkt](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API-konsol - Adobe Concurrency Monitoring](http://docs.adobeptime.io/cm-api-v2/)
