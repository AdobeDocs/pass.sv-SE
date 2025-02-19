---
title: Versionsinformation om Adobe Pass Authentication 3.1.0
description: Versionsinformation om Adobe Pass Authentication 3.1.0
source-git-commit: 4ad5ea619f64a78a72f69228c9ae3c83a7b66f24
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.1.0 {#authn-310-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-310}

* [Byggnummer](#build-number-310)
* [Versionsöversikt](#release-overview-310)

### Byggnummer {#build-number-310}

Adobe Pass-autentisering: adobe-pass-**3.1.0**

Releasedatum: **02/25/2025 - 02/27/2025**

### Versionsöversikt {#release-overview-310}

#### REST API v2

* Nytt `partner_logout`-åtgärdsnamn och ny `partner_interactive`-åtgärdstyp för att skilja mellan vanlig utloggning och enkel inloggning från partner i REST API v2 [Utloggnings-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)-svar.
* Nytt `reason`-fält om du vill ha mer information om åtgärdsnamnet i REST API v2 [Sessions-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) och [Sessions-SSO API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) -svar.

#### Felkorrigeringar

* Korrigerade ett problem som förhindrade spektrumprenumeranter att autentisera via REST API v2 [Authenticate API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).
* Korrigerade ett problem som förhindrade att händelser som genererats via REST API V2 [Authenticate API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) aggregerades korrekt i ESM.
* Korrigerade ett problem som orsakade fel beräkning av `notBefore`-tidsstämpeln för användarprofilerna i REST API v2 [Profiles API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) -svaret.

#### JavaScript SDK

* Borttagen version 3.5.0 av [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md).