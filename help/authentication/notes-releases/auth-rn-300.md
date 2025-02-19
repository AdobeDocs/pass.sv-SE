---
title: Versionsinformation om Adobe Pass Authentication 3.0
description: Versionsinformation om Adobe Pass Authentication 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.0 {#authn-300-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-300}

* [Byggnummer](#build-number-300)
* [Versionsöversikt](#release-overview-300)

### Byggnummer {#build-number-300}

Adobe Pass-autentisering: adobe-pass-**3.0**

Releasedatum: **09/10/2024 - 09/12/2024**

### Versionsöversikt {#release-overview-300}

#### REST API v2

##### Code

* Från och med adobe-pass-**3.0** kan nya och befintliga klientprogram integrera eller migrera till det nya REST API v2 för att dra nytta av de senaste Adobe Pass-funktionerna.

##### Dokumentation

* Börja med det nya REST API v2 i följande dokument:
   * [REST API v2 - API:er - översikt](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - Flöden - översikt](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* URL:erna för offentliga dokument i REST API v1 har ändrats, se följande dokument:
   * [REST API v1 - API:er - översikt](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
   * [REST API v1 - API:er - referens](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### verktyg

* Om du vill testa det nya REST API v2 kan du gå till den nya Adobe Pass-autentiseringssidan från webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass).

#### Felkorrigeringar

* Korrigerade ett problem med att parametern för omdirigerings-URL inte användes när den fanns i utloggningsbegäran.
