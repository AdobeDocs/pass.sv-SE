---
title: Vanliga frågor om registrering av dynamisk klient (DCR)
description: Vanliga frågor om registrering av dynamisk klient (DCR)
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Vanliga frågor om registrering av dynamisk klient (DCR) {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

I det här dokumentet hittar du svar på vanliga frågor om hur Adobe Pass Authentication Dynamic Client Registration (DCR) används.

Mer information om den övergripande DCR-registreringen (Dynamic Client Registration) finns i dokumentationen [Översikt över den dynamiska klientregistreringen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

>[!MORELIKETHIS]
>
> * [REST API v2 - frågor och svar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)

## Allmänna frågor och svar {#general-faqs}

Börja med det här avsnittet om du arbetar med ett program som behöver integrera DCR-registreringen (Dynamic Client Registration), oavsett om det är ett nytt program eller ett befintligt som migreras från en av de tidigare mekanismerna.

### Vanliga frågor om registreringsfasen {#registration-phase-faqs-general}

+++Vanliga frågor om registreringsfasen

#### 1. Vad är syftet med registreringsfasen? {#registration-phase-faq1}

Syftet med registreringsfasen är att registrera klientprogrammet mot Adobe Pass-autentisering genom processen [Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr).

DCR-processen (Dynamic Client Registration) kräver att klientprogrammet hämtar ett par klientautentiseringsuppgifter och en åtkomsttoken som slutmål för registreringsfasen.

Mer information finns i dokumentationen [Översikt över registrering av dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 2. Är registreringsfasen obligatorisk? {#registration-phase-faq2}

Registreringsfasen är obligatorisk, men klientprogrammet kan hoppa över den här fasen om det har ett cachelagrat par med klientautentiseringsuppgifter och en åtkomsttoken som fortfarande är giltig.

#### 3. Vad är en programsats och hur länge gäller den? {#registration-phase-faq3}

Programsatsen är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement).

Programsatsen består av en JSON Web Token (JWT) som kan genereras och hämtas från Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Programsatsen gäller under obegränsad tid, men du kan när som helst be en Adobe Pass-representant om att återkalla den.

Klientprogrammet måste lagra programsatsen och använda den när klientautentiseringsuppgifterna behöver hämtas.

Mer information finns i dokumentationen [Översikt över registrering av dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 4. Hur skapar och laddar man ned en programsats? {#registration-phase-faq4}

Den här åtgärden kan utföras via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i användarhandboken för [TVE Dashboard Channels](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) eller [TVE Dashboard Programmers ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) .

#### 5. Vad händer om en programsats återkallas? {#registration-phase-faq5}

När programsatsen återkallas finns det en viktig konsekvens att tänka på:

* Klientprogrammen som använder den spärrade programsatsen kan inte längre gå igenom [berättigandeflödena](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement), vilket innebär att användare blockeras från att spela upp innehåll.

#### 6. Vad är klientautentiseringsuppgifter och hur länge gäller de? {#registration-phase-faq6}

Klientens autentiseringsuppgifter är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials).

Klientens autentiseringsuppgifter består av ett klientidentifierarpar och klienthemligt par som kan hämtas från klientregisterslutpunkten.

Klientens autentiseringsuppgifter är giltiga för en obegränsad tidsram.

Klientprogrammet måste lagra klientautentiseringsuppgifterna på obestämd tid och använda dem när en åtkomsttoken behöver hämtas.

Mer information finns i dokumentationen för [Hämta klientautentiseringsuppgifter](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

#### 7. Hur hanterar jag klientens inloggningsuppgifter? {#registration-phase-faq7}

Vi rekommenderar att klientprogrammet hanterar ett unikt par klientautentiseringsuppgifter för varje instans av användarprogrammet om både klient-till-server- och server-till-server-integreringar med Adobe Pass Authentication.

Klientprogrammet måste lagra klientautentiseringsuppgifterna på obestämd tid och använda dem när en åtkomsttoken behöver hämtas.

#### 8. Vad händer om cachelagrade klientautentiseringsuppgifter går förlorade? {#registration-phase-faq8}

När cachelagrade klientautentiseringsuppgifter försvinner finns det tre viktiga följder att tänka på:

* Klientprogrammet måste erhålla ett nytt par klientautentiseringsuppgifter.
* Klientprogrammet måste hämta en ny åtkomsttoken med hjälp av det nya klientautentiseringsuppgifterna.
* Klientprogrammet måste be användaren att autentisera igen eftersom klientprogrammet förlorar åtkomsten till de autentiserade profilerna som erhållits tidigare.

#### 9. Vad är en åtkomsttoken och hur länge gäller den? {#registration-phase-faq9}

Åtkomsttoken är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token).

Åtkomsttoken består av en [innehavartoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) som kan hämtas från klienttokenslutpunkten.

Åtkomsttoken är giltig under en begränsad och kort tidsperiod som anges vid tidpunkten för utfärdandet.

Klientprogrammet måste lagra åtkomsttoken och använda den tills den upphör att gälla när REST API V2 används.

Klientprogrammet måste erhålla en ny åtkomsttoken innan den aktuella åtkomsttoken upphör att gälla för att förhindra obehöriga begäranden.

Mer information finns i dokumentationen för [Hämta åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

#### 10. Hur uppdaterar klientprogrammet en åtkomsttoken? {#registration-phase-faq10}

Klientprogrammet måste uppdatera en åtkomsttoken på samma sätt som när en ny åtkomsttoken hämtas, men med cachelagrade klientautentiseringsuppgifter.

Klientprogrammet får inte registrera om för att uppdatera en åtkomsttoken, utan måste använda de lagrade klientinloggningsuppgifterna, annars måste användarna autentisera igen.

Mer information finns i dokumentationen för [Hämta åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
