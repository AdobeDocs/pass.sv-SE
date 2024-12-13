---
title: Produktmeddelanden
description: Produktmeddelanden
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# Produktmeddelanden {#product-announcements}

## Slutet av livscykeln (EOL) {#eol}

I det här avsnittet beskrivs slutdatum för support och slutdatum för vissa Adobe Pass-autentiseringsfunktioner och produkter som ska avvecklas.

Se till att du håller dig informerad om tidslinjerna för avveckling och att du tänker vidta de åtgärder som beskrivs nedan.

| Meddelande | Anteckningsdatum | Sista supportdatum | Slutdatum | Beskrivning |
|-----------------------------------------------------------------|-------------------|---------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe Pass Authentication AccessAktivera JavaScript SDK 3.5 EOL | 12/11/2024 | Ej tillämpligt | 01/08/2025 | Vi planerar att upphöra med livscykeln för Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 den **8 januari 2025**. Efter detta datum kommer funktioner och tjänster som tillhandahålls av AccessEnabler JavaScript SDK 3.5 inte längre att fungera, inklusive användarautentisering och behörighet. |
| Adobe Pass Authentication Non-DCR Security Mechanism EOL | 12/11/2024 | Ej tillämpligt | 01/20/2025 | Vi planerar att upphöra med livscykeln för säkerhetsmekanismen Adobe Pass Authentication Non-DCR den **20 januari 2025**. Den här mekanismen användes för att skydda åtkomsten till följande Adobe Pass Authentication API:er:<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md">Återställ API för tillfälligt pass</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md">Försämring av API</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">Proxy-MVPD API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">Tillståndstjänstens övervaknings-API</a></li></ul>Efter detta datum kommer funktioner och tjänster som tillhandahålls av ovanstående API:er som nås med denna säkerhetsmekanism inte längre att fungera.</br></br>För att du ska kunna fortsätta använda tjänsten måste du migrera alla program för att kunna använda funktionen [Dynamisk klientregistrering](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md). |
| Adobe Pass Authentication REST API v1 EOL | 12/11/2024 | 12/31/2025 | Slutet av 2026 (TBD) | Vi planerar att upphöra med stödet för Adobe Pass Authentication REST API v1 den **31 december 2025**. Efter detta datum kommer uppdateringar eller korrigeringar inte längre att tillhandahållas.</br></br>Om du vill ha fortsatt stöd måste du migrera alla program till <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>.</br></br>Slutet av livscykeln för Adobe Pass Authentication REST API v1 planeras till **slutet av 2026**. Efter detta datum kommer funktioner och tjänster som tillhandahålls av REST API v1 inte längre att fungera, inklusive användarautentisering och auktorisering.</br></br>Om du vill fortsätta använda tjänsten måste du migrera alla program till <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>. |
| Adobe Pass Authentication AccessAktivera SDK:er EOL | 12/11/2024 | 05/31/2026 | Slutet av 2026 (TBD) | Vi planerar att upphöra med stödet för Adobe Pass Authentication AccessEnabler SDK:er den **31 maj 2026**. Efter detta datum kommer uppdateringar eller korrigeringar inte längre att tillhandahållas.</br></br>Om du vill ha fortsatt stöd måste du migrera alla program till <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>.</br></br>Vi planerar slutet på livscykeln för Adobe Pass Authentication AccessEnabler SDK:er till **slutet av 2026**. Efter detta datum kommer funktioner och tjänster som tillhandahålls av AccessEnabler SDK:er inte längre att fungera, inklusive användarautentisering och behörighet.</br></br>Om du vill fortsätta använda tjänsten måste du migrera alla program till <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>. |

## Produktreleaser {#product-releases}

I det här avsnittet sammanställs referenser till versionshistoriken och motsvarande versionsinformation för Adobe Pass-autentisering.

### 2024 {#product-releases-2024}

| Versionsinformation | Datum |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Versionsinformation för Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| [Versionsinformation för Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| [Versionsinformation om Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md) | 04/23/2024 - 04/25/2024 |
| [Versionsinformation om Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md) | 02/27/2024 - 02/29/2024 |
| [Versionsinformation om Adobe Pass-autentisering för JavaScript SDK 4.7.0](notes-releases/authn-rn-javascript-470.md) | 02/27/2024 - 02/29/2024 |
| [Versionsinformation om Adobe Pass Authentication iOS/tvOS SDK 3.9.2](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Versionsinformation om Adobe Pass Authentication iOS/tvOS SDK 3.8.4](notes-releases/authn-rn-ios-tvos-384.md) | 01/26/2024 |

### 2023 {#product-releases-2023}

| Versionsinformation | Datum |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Versionskommentarer för Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| [Versionskommentarer för Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| [Versionsinformation om Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md) | 07/11/2023 - 07/13/2023 |
| [Versionsinformation om Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md) | 06/20/2023 - 06/22/2023 |
| [Versionsinformation om Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Versionsinformation om Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md) | 01/31/2023 - 02/02/2023 |
| [Versionsinformation om Adobe Pass-autentisering, iOS/tvOS SDK 3.8.3](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Versionsinformation om Adobe Pass Authentication iOS/tvOS SDK 3.8.2](notes-releases/authn-rn-ios-tvos-382.md) | 02/10/2023 |
| [Versionsinformation om Adobe Pass-autentisering, iOS/tvOS SDK 3.8.1](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Versionsinformation om Adobe Pass-autentisering Android SDK 3.7.3](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| Versionsinformation | Datum |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Versionsinformation om Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| [Versionsinformation om Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md) | 09/20/2022 - 09/22/2022 |
| [Versionsinformation om Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md) | 08/02/2022 - 08/04/2022 |
| [Versionsinformation om Adobe Pass-autentisering för JavaScript SDK 4.6.0](notes-releases/authn-rn-javascript-460.md) | 09/20/2022 - 09/22/2022 |

### 2021 {#product-releases-2021}

| Versionsinformation | Datum |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Versionsinformation om Adobe Pass-autentisering för JavaScript SDK 4.4.0](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Versionsinformation för Adobe Pass Authentication iOS/tvOS SDK 3.7.0](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
