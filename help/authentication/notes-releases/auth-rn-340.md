---
title: Versionsinformation om Adobe Pass Authentication 3.4.0
description: Versionsinformation om Adobe Pass Authentication 3.4.0
exl-id: ad572617-f607-419d-a085-70c025465080
source-git-commit: c9958a17ad9dfb518bab1d24087c85fdcb6fd057
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.4.0 {#authn-340-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-340}

* [Byggnummer](#build-number-340)
* [Versionsöversikt](#release-overview-340)

### Byggnummer {#build-number-340}

Adobe Pass-autentisering: adobe-pass-**3.4.0**
Releasedatum: **09/16/2025 - 09/18/2025**

### Versionsöversikt {#release-overview-340}

#### REST API v2

* Stöd för [Experience Cloud ID (ECID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md) har lagts till för att förbättra användaridentifierings- och spårningsfunktionerna.
* Sidhuvudena AP-Device-Identifier och X-Device-Info har ändrats till valfria för REST API V2 [Configuration API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### Felkorrigeringar

* Ett problem har korrigerats där MVPD-id och Proxy-MVPD-id återanvändes i token från REST API V2 [Decision](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -svaret.
* Ett problem har korrigerats där begärande-ID inte fanns i token från REST API V2 [Decision](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) -svaret.
* Ett problem har korrigerats där överföring av för mycket resurser till API:t för REST V2 [Förhandsauktorisera beslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) resulterade i en tom beslutslista i svaret i stället för [Förbättrad felkod](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **för_många_resurser** .

#### Diverse

* Patched security vulnerabilities.
