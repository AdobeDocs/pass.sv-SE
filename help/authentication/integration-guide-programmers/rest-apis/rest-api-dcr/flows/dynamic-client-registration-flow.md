---
title: Dynamiskt klientregistreringsflöde
description: Dynamiskt klientregistreringsflöde
exl-id: d881cf0a-de09-4b1d-a094-d5490f944796
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Dynamiskt klientregistreringsflöde {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Implementeringen av API:t för registrering av dynamiska klienter begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Använd Adobe Pass-skyddade API:er {#access-adobe-pass-protected-apis}

### Förutsättningar {#prerequisites-access-adobe-pass-protected-apis}

Innan du får åtkomst till Adobe Pass-skyddade API:er måste du kontrollera att följande krav är uppfyllda:

* En klientrepresentant måste skapa ett registrerat program enligt beskrivningen i avsnittet [Hantera registrerade program](../dynamic-client-registration-overview.md#manage-registered-applications).
* En klientrepresentant måste hämta och bädda in en programsats enligt beskrivningen i avsnittet [Hantera programsatser](../dynamic-client-registration-overview.md#manage-software-statements).

>[!IMPORTANT]
>
> Adobe Pass SDK för autentisering hämtar och uppdaterar klientens inloggningsuppgifter och åtkomsttoken för klientprogrammets räkning.
> 
> För alla andra Adobe Pass-skyddade API:er måste klientprogrammet följa arbetsflödet nedan.

### Arbetsflöde {#workflow-access-adobe-pass-protected-apis}

Följ de angivna stegen för att få åtkomst till Adobe Pass-skyddade API:er enligt bilden nedan.

![Få åtkomst till Adobe Pass-skyddade API:er](../../../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Få åtkomst till Adobe Pass-skyddade API:er*

1. **Hämta klientautentiseringsuppgifter:** Klientprogrammet samlar in alla nödvändiga data för att hämta klientautentiseringsuppgifter genom att anropa slutpunkten för klientregistret.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta klientautentiseringsuppgifter](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API-dokumentationen:
   >
   > * Alla _obligatoriska_-parametrar, som `software_statement`
   > * Alla _obligatoriska_ rubriker, som `Content-Type`, `X-Device-Info`
   > * Alla _valfria_ parametrar och rubriker

1. **Returnera klientautentiseringsuppgifter:** Svaret på klientregisterslutpunkten innehåller information om klientautentiseringsuppgifterna som är associerade med de mottagna parametrarna och rubrikerna.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett svar på klientautentiseringsuppgifter finns i [Hämta klientautentiseringsuppgifter](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API-dokumentationen.
   >
   > <br/>
   >
   > Klientregistret validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer [Hämta klientautentiseringsuppgifter](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API-dokumentationen.

   >[!TIP]
   >
   > Klientens autentiseringsuppgifter måste cachelagras och användas i oändlighet.

1. **Hämta åtkomsttoken:** Klientprogrammet samlar in alla nödvändiga data för att hämta åtkomsttoken genom att anropa klienttokenslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta åtkomsttoken](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API-dokumentationen:
   >
   > * Alla _obligatoriska_-parametrar, som `client_id`, `client_secret` och `grant_type`
   > * Alla _obligatoriska_ rubriker, som `Content-Type`, `X-Device-Info`
   > * Alla _valfria_ parametrar och rubriker

1. **Returåtkomsttoken:** Slutpunktssvaret för klienttoken innehåller information om åtkomsttoken som är associerad med de mottagna parametrarna och rubrikerna.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett åtkomsttoken-svar finns i [Hämta åtkomsttoken](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API-dokumentationen.
   >
   > <br/>
   >
   > Klienttoken validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer [Hämta åtkomsttoken](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API-dokumentationen.

   >[!TIP]
   >
   > Åtkomsttoken måste cachelagras och användas endast inom den angivna varaktigheten (t.ex. 24 timmars time-to-live). När det har gått ut måste klientprogrammet begära en ny åtkomsttoken.

1. **Fortsätt med åtkomst till skyddade API:er:** Klientprogrammet använder åtkomsttoken för åtkomst till andra Adobe Pass-skyddade API:er. Klientprogrammet måste inkludera åtkomsttoken i begärandehuvudet `Authorization` med autentiseringsschemat `Bearer` (d.v.s. `Authorization: Bearer <access_token>`).

   >[!IMPORTANT]
   >
   > Adobe Pass-skyddade API:er validerar åtkomsttoken för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * _access_token_ måste vara giltig.
   > * _access_token_ måste associeras med en giltig _client_id_ och _client_secrets_.
   > * _access_token_ måste associeras med en giltig _software_statement_.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../features-standard/error-reporting/enhanced-error-codes.md).
