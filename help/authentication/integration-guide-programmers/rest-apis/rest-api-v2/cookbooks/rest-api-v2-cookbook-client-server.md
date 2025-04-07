---
title: REST API V2 Cookbook (klient-till-server)
description: REST API V2 Cookbook (klient-till-server)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '1846'
ht-degree: 0%

---

# REST API V2 Cookbook (klient-till-server) {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Dokumentet är avsett för utvecklare som integrerar [Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) i sina strömningsprogram med en klient-till-server-arkitektur (C2S).

## Förutsättningar {#prerequisites}

Termer och definitioner finns i [REST API V2 Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) -dokumentationen.

Information om obligatoriska krav och rekommenderade metoder finns i dokumentationen för [REST API V2 Checklist](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) .

Vanliga frågor och svar finns i [REST API V2 FAQs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) -dokumentationen.

## A. Registreringsfas {#registration-phase}

Syftet med registreringsfasen är att registrera direktuppspelningsprogrammet mot Adobe Pass-autentisering genom DCR-processen (Dynamic Client Registration).

DCR-processen (Dynamic Client Registration) kräver att direktuppspelningsprogrammet hämtar ett par klientautentiseringsuppgifter och hämtar en åtkomsttoken som slutmål för registreringsfasen.

Registreringsfasen är obligatorisk, men direktuppspelningsprogrammet kan hoppa över den här fasen om det har ett cachelagrat par med klientautentiseringsuppgifter och en åtkomsttoken som fortfarande är giltig.

+++Relaterade artiklar

**API:er:**

* [Hämta klientautentiseringsuppgifter](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Hämta åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flöden:**

* [Dynamiskt klientregistreringsflöde](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**Vanliga frågor:**

* [Vanliga frågor om registreringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Steg 1: Registrera ditt program {#step-1-register-your-application}

* **Hämta klientautentiseringsuppgifter:** Direktuppspelningsprogrammet hämtar klientautentiseringsuppgifter genom att anropa slutpunkten [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

   * Strömningsprogrammet måste lagra klientens autentiseringsuppgifter och använda dem i oändlighet när en åtkomsttoken behöver hämtas.


* **Hämta åtkomsttoken:** Direktuppspelningsprogrammet hämtar åtkomsttoken genom att anropa slutpunkten [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

   * Strömningsprogrammet måste lagra och använda åtkomsttoken tills den upphör att gälla, sedan ta bort den och skaffa en ny.

## B. Autentiseringsfas {#authentication-phase}

Syftet med autentiseringsfasen är att ge direktuppspelningsprogrammet möjlighet att verifiera användarens identitet och få information om användarens metadata.

Autentiseringsfasen fungerar som ett nödvändigt steg för förauktoriseringsfasen eller auktoriseringsfasen när direktuppspelningsprogrammet behöver spela upp innehåll.

+++Relaterade artiklar

**API:er**

* [Skapa autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Återuppta autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Hämta autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Utför autentisering i användaragent](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Hämta profiler](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Hämta profil för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Hämta profil för specifik kod](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Flöden**

* [Grundläggande autentiseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundläggande autentiseringsflöde som utförs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om autentiseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Steg 2: Kontrollera om det finns befintliga autentiserade profiler {#step-2-check-for-existing-authenticated-profiles}

* **Hämta profiler:** Direktuppspelningsprogrammet söker efter befintliga profiler genom att anropa slutpunkten [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) .


* **Scenario 1:** Det finns befintliga profiler. Strömningsprogrammet kan fortsätta till [förauktoriseringsfasen](#preauthorization-phase) eller [auktoriseringsfasen](#authorization-phase).


* **Scenario 2:** Det finns inga befintliga profiler. Direktuppspelningsprogrammet kan fortsätta till nästa steg för att [autentisera användaren](#step-3-authenticate-the-user).


* **Scenario 3:** Det finns inga befintliga profiler. Direktuppspelningsprogrammet kan fortsätta att ge användaren tillfällig åtkomst via funktionen [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md).

   * Det här scenariot ligger utanför det här dokumentets omfång. Mer information finns i dokumentationen för [Tillfälliga åtkomstflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

### Steg 3: Autentisera användaren {#step-3-authenticate-the-user}

* **Hämta konfiguration:** Direktuppspelningsprogrammet hämtar listan över tillgängliga MVPD-filer genom att anropa slutpunkten [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) .

   * Strömningsprogrammet kan implementera en anpassad filtreringsmekanism för att förfina listan över MVPD-program från konfigurationssvaret och endast visa avsedda providers medan andra döljs (t.ex. MVPD-program under utveckling, test MVPD-program, TempPass). Detta garanterar att användarna får ett välstrukturerat urval när de väljer sin tv-leverantör.


* **Skapa autentiseringssession:** Direktuppspelningsprogrammet initierar en autentiseringssession genom att anropa slutpunkten [**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).


* **Scenario 1:** Direktuppspelningsprogrammet kan öppna en webbläsare eller webbvy, och därför måste autentiseringen `url` läsas in.

   * Användaren anger sitt användarnamn och lösenord på inloggningssidan för MVPD. När autentiseringen är klar visas en sida för slutförd omdirigering.


* **Scenario 2:** Direktuppspelningsprogrammet kan inte öppna en webbläsare och autentiseringen `code` måste därför visas. Ett separat webbprogram krävs för att uppmana användaren att ange `code`, konstruera autentiseringen `url` och öppna: [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * Användaren anger sitt användarnamn och lösenord på inloggningssidan för MVPD. När autentiseringen är klar visas en sida för slutförd omdirigering.

### Steg 4: Kontrollera om det finns autentiserade profiler {#step-4-check-for-authenticated-profiles}

* **Hämta profil för specifik kod:** Direktuppspelningsprogrammet måste implementera en avsökningsmekanism med `code` för att kontrollera om profilen genererades och sparades genom att anropa slutpunkten [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).

   * Direktuppspelningsprogrammet måste **starta avsökningsfunktionen** under följande förhållanden:

      * **Autentisering som utförs i det primära (skärm) programmet:** Det primära (direktuppspelande) programmet ska starta avsökningen när användaren kommer till den sista målsidan, efter att webbläsarkomponenten har läst in den URL som angetts för parametern `redirectUrl` i [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) -slutpunktsbegäran.

      * **Autentisering som utförs i ett sekundärt (skärm) program:** Det primära (direktuppspelande) programmet ska starta avsökningen så snart användaren initierar autentiseringsprocessen - direkt efter att ha tagit emot [Sessioner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) -slutpunktssvaret och visar autentiseringskoden för användaren.

   * Direktuppspelningsprogrammet måste **stoppa avsökningsfunktionen** under följande förhållanden:

      * **Autentiseringen är klar:** Användarens profilinformation har hämtats och autentiseringsstatusen har bekräftats. I nuläget behövs ingen avsökning längre.

      * **Autentiseringssession och förfallodatum för kod:** Autentiseringssessionen och koden förfaller, vilket anges av tidsstämpeln `notAfter` (t.ex. 30 minuter) i slutpunktssvaret för [sessioner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Om detta inträffar måste användaren starta om autentiseringsprocessen och avsökningen med den tidigare autentiseringskoden ska stoppas omedelbart.

      * **Ny autentiseringskod har genererats:** Om användaren begär en ny autentiseringskod på den primära (skärm) enheten är den befintliga sessionen inte längre giltig och avsökningen med den tidigare autentiseringskoden bör stoppas omedelbart.

   * Direktuppspelningsprogrammet måste **konfigurera avsökningsfrekvensen** under följande förhållanden:

      * **Autentisering som utförs i det primära (skärm) programmet:** Det primära (direktuppspelande) programmet ska avsöka var 3:e till 5:e sekund eller mer.

      * **Autentisering som utförs i ett sekundärt (skärm) program:** Det primära (direktuppspelande) programmet ska avsöka var 3-5:e sekund eller mer.

   * Strömningsprogrammet bör cachelagra delar av användarens profilinformation i en beständig lagringsplats för att undvika onödiga begäranden och förbättra användarupplevelsen.

## C. (Valfritt) Förhandsauktoriseringsfas {#preauthorization-phase}

Syftet med förauktoriseringsfasen är att ge direktuppspelningsprogrammet möjlighet att presentera en delmängd av resurser från sin katalog som användaren skulle ha rätt till.

Fas för förhandsauktorisering kan förbättra användarupplevelsen när användaren öppnar direktuppspelningsprogrammet för första gången eller navigerar till ett nytt avsnitt.

Förhandsauktoriseringsfasen är inte obligatorisk, men direktuppspelningsprogrammet kan hoppa över den här fasen om det vill visa en katalog med resurser utan att först filtrera dem baserat på användarens tillstånd.

+++Relaterade artiklar

**API:er**

* [Hämta förauktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flöden**

* [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om förauktoriseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Steg 5: Sök efter förauktoriserade resurser {#step-5-check-for-preauthorized-resources}

* **Hämta beslut om förauktorisering:** Direktuppspelningsprogrammet hämtar beslut om förauktorisering för en lista med resurser genom att anropa slutpunkten [**/api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) .

   * Strömningsprogrammet behövs inte för att lagra förauktoriseringsbeslut i beständig lagring. Vi rekommenderar dock att du cachelagrar tillståndsbeslut i minnet för att förbättra användarupplevelsen. På så sätt undviker du onödiga krav på resurser som redan har förauktoriserats, vilket minskar latensen och förbättrar prestandan.

   * Direktuppspelningsprogrammet kan fastställa orsaken till ett beslut om förauktorisering som nekas genom att granska [felkoden och meddelandet](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) som ingår i svaret från slutpunkten för förauktorisering av beslut. Dessa detaljer ger insikt i den specifika anledningen till att förauktoriseringsbegäran nekades, vilket kan hjälpa användaren att informera om användarupplevelsen eller utlösa nödvändig hantering i programmet. Se till att alla återförsöksmetoder som implementeras för att hämta beslut om förauktorisering inte resulterar i en oändlig slinga om beslutet om förauktorisering nekas. Överväg att begränsa antalet försök till ett rimligt antal och hantera nekanden på ett enkelt sätt genom att ge användaren tydlig feedback.

   * Strömningsprogrammet kan få ett beslut om förhandsgodkännande för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 5, på grund av villkor som anges av distributörer av videoprogrammeringstjänster. Det maximala antalet resurser kan visas och ändras efter att du har godkänt PDF-filer via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av organisationens administratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.


## D. Auktoriseringsfas {#authorization-phase}

Syftet med auktoriseringsfasen är att ge direktuppspelningsprogrammet möjlighet att spela upp resurser som användaren begär efter att ha verifierat sina rättigheter med MVPD.

Auktoriseringsfasen är obligatorisk, men direktuppspelningsprogrammet kan inte hoppa över den här fasen om det vill spela upp resurser som användaren begär, eftersom det kräver verifiering med MVPD att användaren har rätt innan direktuppspelningen släpps.

+++Relaterade artiklar

**API:er**

* [Hämta auktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flöden**

* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om auktoriseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Steg 6: Kontrollera om det finns auktoriserade resurser {#step-6-check-for-authorized-resources}

* **Hämta auktoriseringsbeslut:** Direktuppspelningsprogrammet hämtar auktoriseringsbeslut för en specifik resurs genom att anropa slutpunkten [**/api/v2/{serviceProvider}/Decision/authorized/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).

   * Strömningsprogrammet behövs inte för att lagra auktoriseringsbeslut i beständig lagring.

   * Direktuppspelningsprogrammet kan fastställa orsaken till ett beslut om nekad auktorisering genom att granska [felkoden och meddelandet](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) som ingår i svaret från verifieringsslutpunkten för beslut. Dessa uppgifter ger insikt i varför auktoriseringsbegäran nekades, vilket kan bidra till att informera användaren eller utlösa nödvändig hantering i programmet. Se till att alla återförsöksmetoder som implementeras för att hämta auktoriseringsbeslut inte resulterar i en oändlig slinga om auktoriseringsbeslutet nekas. Överväg att begränsa antalet försök till ett rimligt antal och hantera nekanden på ett enkelt sätt genom att ge användaren tydlig feedback.

   * Direktuppspelningsprogrammet behöver inte uppdatera en medietoken som har upphört att gälla medan direktuppspelningen pågår. Om medietoken upphör att gälla under uppspelning bör strömmen kunna fortsätta utan avbrott. Klienten måste dock begära ett nytt auktoriseringsbeslut - och få en ny medietoken - nästa gång användaren försöker spela upp en resurs.

   * Strömningsprogrammet kan erhålla ett auktoriseringsbeslut för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 1, på grund av villkor som anges av distributörer av videoprogrammeringstjänster.

## E. Utloggningsfas {#logout-phase}

Syftet med utloggningsfasen är att ge direktuppspelningsprogrammet möjlighet att avsluta användarens autentiserade profil inom Adobe Pass-autentisering på användarens begäran.

Utloggningsfasen är obligatorisk. Strömningsprogrammet måste ge användaren möjlighet att logga ut.

+++Relaterade artiklar

**API:er**

* [Initiera utloggning för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flöden**

* [Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om utloggningsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Steg 7: Logga ut {#step-7-logout}

* **Initiera Adobe Pass-utloggning:** Strömningsprogrammet initierar utloggningsflödet genom att anropa slutpunkten [**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).

   * Direktuppspelningsprogrammet måste följa instruktionerna i attributen `actionName` och `actionType` för slutpunktssvaret för utloggning för att säkerställa att utloggningsprocessen slutförs korrekt.

      * Om attributet `actionType` i svaret är inställt på &quot;interaktiv&quot;:

         * **Scenario 1:** Direktuppspelningsprogrammet kan öppna en webbläsare eller webbvy och måste därför läsa in utloggningen `url`.

         * **Scenario 2:** Direktuppspelningsprogrammet kan inte öppna en webbläsare, och därför kan utloggningsprocessen stoppas eftersom MVPD-sessionen inte sparades i en webbläsarcache för direktuppspelningsenheten.
