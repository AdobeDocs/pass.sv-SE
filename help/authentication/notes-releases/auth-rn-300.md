---
title: Versionsinformation om Adobe Pass Authentication 3.0
description: Versionsinformation om Adobe Pass Authentication 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.0 {#pt-authn-300-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-300}

* [Byggnummer](#build-number-300)
* [Versionsöversikt](#release-overview-300)

### Byggnummer {#build-number-2651}

Adobe Pass-autentisering: adobe-pass-**3.0**
Releasedatum: **09/10/2024 - 09/12/2024**

### Nya funktioner {#new-features-300}

#### REST API v2 {#rest-apis}

##### Code

* Från och med adobe-pass-**3.0** kan nya och befintliga klientprogram integrera eller migrera till det nya REST API v2 för att dra nytta av de senaste Adobe Pass-funktionerna.

##### Dokumentation

* Börja med det nya REST API v2 i följande dokument:
   * [REST API v2 - API:er - översikt](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - Flöden - översikt](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* URL:erna för offentliga dokument i REST API v1 har ändrats, se följande dokument:
   * [REST API v1 - API:er - översikt](../integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
   * [REST API v1 - API:er - referens](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### verktyg

* Om du vill testa det nya REST API v2 kan du gå till den nya Adobe Pass-autentiseringssidan från webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass).

### Felkorrigeringar {#bug-fixes-300}

* Korrigerade ett problem med att parametern för omdirigerings-URL inte användes när den fanns i utloggningsbegäran.