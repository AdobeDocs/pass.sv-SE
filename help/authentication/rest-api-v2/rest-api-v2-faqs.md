---
title: REST API V2 - frågor och svar
description: REST API V2 - frågor och svar
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# REST API V2 - frågor och svar {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Det här dokumentet innehåller omfattande översiktssvar på vanliga frågor om användningen av Adobe Pass Authentication REST API V2.

Mer information om REST API V2 som helhet finns i [REST API V2 Overview](./rest-api-v2-overview.md) -dokumentationen.

## Allmänna frågor och svar {#general-faqs}

Börja med det här avsnittet om du arbetar med ett program som behöver integrera REST API V2, oavsett om det är ett nytt eller befintligt program som migreras från [REST API V1](#migrate-rest-api-v1-to-rest-api-v2) eller [SDK](#migrate-sdk-to-rest-api-v2).

Mer information om migreringsinformation och -steg finns även i nästa avsnitt.

+++Vanliga frågor om registreringsfasen

### 1. Vad är syftet med registreringsfasen? {#registration-phase-faq1}

Syftet med registreringsfasen är att registrera klientprogrammet mot Adobe Pass-autentisering genom processen [Dynamic Client Registration (DCR)](./rest-api-v2-glossary.md#dcr).

DCR-processen (Dynamic Client Registration) kräver att klientprogrammet hämtar ett par klientautentiseringsuppgifter och en åtkomsttoken som slutmål för registreringsfasen.

Mer information finns i dokumentationen [Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md).

### 2. Är registreringsfasen obligatorisk? {#registration-phase-faq2}

Registreringsfasen är obligatorisk, men klientprogrammet kan hoppa över den här fasen om det har ett cachelagrat par med klientautentiseringsuppgifter och en åtkomsttoken som fortfarande är giltig.

### 3. Vad är en programsats och hur länge gäller den? {#registration-phase-faq3}

Programsatsen är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#software-statement).

Programsatsen består av en JSON Web Token (JWT) som kan genereras och hämtas från Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Programsatsen gäller under obegränsad tid, men du kan när som helst be en Adobe Pass-representant om att återkalla den.

Klientprogrammet måste lagra programsatsen och använda den när klientautentiseringsuppgifterna behöver hämtas.

Mer information finns i dokumentationen [Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md).

### 4. Hur skapar och laddar man ned en programsats? {#registration-phase-faq4}

Den här åtgärden kan utföras via Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i användarhandboken för [TVE Dashboard Channels](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) eller [TVE Dashboard Programmers ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) .

### 5. Vad händer om en programsats återkallas? {#registration-phase-faq5}

När programsatsen återkallas finns det en viktig konsekvens att tänka på:

* Klientprogrammen som använder den spärrade programsatsen kan inte längre gå igenom [berättigandeflödena](./rest-api-v2-glossary.md#entitlement), vilket innebär att användare blockeras från att spela upp innehåll.

### 6. Vad är klientautentiseringsuppgifter och hur länge gäller de? {#registration-phase-faq6}

Klientens autentiseringsuppgifter är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#client-credentials).

Klientens autentiseringsuppgifter består av ett klientidentifierarpar och klienthemligt par som kan hämtas från klientregisterslutpunkten.

Klientens autentiseringsuppgifter är giltiga för en obegränsad tidsram.

Klientprogrammet måste lagra klientautentiseringsuppgifterna på obestämd tid och använda dem när en åtkomsttoken behöver hämtas.

Mer information finns i dokumentationen för [Hämta klientautentiseringsuppgifter](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

### 7. Hur hanterar jag klientens inloggningsuppgifter? {#registration-phase-faq7}

Vi rekommenderar att klientprogrammet hanterar ett unikt par klientautentiseringsuppgifter för varje instans av användarprogrammet om både klient-till-server- och server-till-server-integreringar med Adobe Pass Authentication.

Klientprogrammet måste lagra klientautentiseringsuppgifterna på obestämd tid och använda dem när en åtkomsttoken behöver hämtas.

### 8. Vad händer om cachelagrade klientautentiseringsuppgifter går förlorade? {#registration-phase-faq8}

När cachelagrade klientautentiseringsuppgifter försvinner finns det tre viktiga följder att tänka på:

* Klientprogrammet måste erhålla ett nytt par klientautentiseringsuppgifter.
* Klientprogrammet måste hämta en ny åtkomsttoken med hjälp av det nya klientautentiseringsuppgifterna.
* Klientprogrammet måste be användaren att autentisera igen eftersom klientprogrammet förlorar åtkomsten till de autentiserade profilerna som erhållits tidigare.

### 9. Vad är en åtkomsttoken och hur länge gäller den? {#registration-phase-faq9}

Åtkomsttoken är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#access-token).

Åtkomsttoken består av en [innehavartoken](./appendix/headers/rest-api-v2-appendix-headers-authorization.md) som kan hämtas från klienttokenslutpunkten.

Åtkomsttoken är giltig under en begränsad och kort tidsperiod som anges vid tidpunkten för utfärdandet.

Klientprogrammet måste lagra åtkomsttoken och använda den tills den upphör att gälla när REST API V2 används.

Klientprogrammet måste erhålla en ny åtkomsttoken innan den aktuella åtkomsttoken upphör att gälla för att förhindra obehöriga begäranden.

Mer information finns i dokumentationen för [Hämta åtkomsttoken](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).

### 10. Hur uppdaterar klientprogrammet en åtkomsttoken? {#registration-phase-faq10}

Klientprogrammet måste uppdatera en åtkomsttoken på samma sätt som när en ny åtkomsttoken hämtas, men med cachelagrade klientautentiseringsuppgifter.

Klientprogrammet får inte registrera om för att uppdatera en åtkomsttoken, utan måste använda de lagrade klientinloggningsuppgifterna, annars måste användarna autentisera igen.

Mer information finns i dokumentationen för [Hämta åtkomsttoken](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).

+++

+++Vanliga frågor om konfigurationsfasen

### 1. Vad är syftet med konfigurationsfasen? {#configuration-phase-faq1}

Syftet med konfigurationsfasen är att ge klientprogrammet en lista över de MVPD-program som det är aktivt integrerat med tillsammans med konfigurationsinformation som sparats av Adobe Pass Authentication för varje MVPD.

Konfigurationsfasen fungerar som ett nödvändigt steg för autentiseringsfasen när klientprogrammet måste be användaren att välja sin TV-leverantör.

### 2. Är konfigurationsfasen obligatorisk? {#configuration-phase-faq2}

Konfigurationsfasen är inte obligatorisk, klientprogrammet kan hoppa över den här fasen i följande scenarier:

* Användaren är redan autentiserad.
* Användaren erbjuds tillfällig åtkomst via en vanlig eller kampanjanpassad TempPass-funktion.
* Användarautentiseringen har upphört att gälla, men klientprogrammet har cachelagrat det tidigare valda MVPD som ett motiverat val av användarupplevelse och uppmanar bara användaren att bekräfta att han/hon fortfarande prenumererar på det MVPD-programmet.

### 3. Vad är en konfiguration och hur länge gäller den? {#configuration-phase-faq3}

Konfigurationen är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#configuration).

Konfigurationen består av en lista med MVPD-filer som kan hämtas från Configuration-slutpunkten.

Klientprogrammet kan använda konfigurationen för att presentera en UI-komponent som kallas för&quot;väljaren&quot; när användaren måste välja sitt MVPD-program.

Klientprogrammet bör uppdatera konfigurationen innan användaren går igenom autentiseringsfasen.

Mer information finns i dokumentationen för [Hämta konfiguration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

### 4. Kan klientapplikationen hantera sin egen lista över MVPD? {#configuration-phase-faq4}

Klientprogrammet kan hantera sin egen lista över MVPD, men vi rekommenderar att du använder konfigurationen som tillhandahålls av Adobe Pass Authentication för att se till att listan är aktuell och korrekt.

Klientprogrammet skulle få ett [fel](/help/authentication/enhanced-error-codes.md) från Adobe Pass Authentication REST API V2 om användaren valde MVPD inte har någon aktiv integrering med den angivna [tjänstprovidern](./rest-api-v2-glossary.md#service-provider).

### 5. Kan klientprogrammet filtrera listan över MVPD? {#configuration-phase-faq5}

Klientprogrammet kan filtrera listan över MVPD-program som anges i konfigurationssvaret genom att implementera en anpassad mekanism som bygger på egen affärslogik och krav som användarplats eller användarhistorik för tidigare val.

### 6. Vad händer om integreringen med ett PDF-dokument är inaktiverad och markerad som inaktiv? {#configuration-phase-faq6}

När integreringen med ett MVPD-dokument är inaktiverad och markerad som inaktiv, tas MVPD bort från listan över MVPD-program som finns i ytterligare konfigurationssvar och det finns två viktiga konsekvenser att tänka på:

* De oautentiserade användarna av detta MVPD kommer inte längre att kunna slutföra autentiseringsfasen med detta MVPD.
* De autentiserade användarna av detta MVPD kommer inte längre att kunna slutföra förauktoriserings-, auktoriserings- eller utloggningsfaserna med detta MVPD.

Klientprogrammet skulle få ett [fel](/help/authentication/enhanced-error-codes.md) från Adobe Pass Authentication REST API V2 om användaren valde MVPD inte längre har någon aktiv integrering med den angivna [tjänstprovidern](./rest-api-v2-glossary.md#service-provider).

### 7. Vad händer om integreringen med ett PDF-dokument är aktiverad och markerad som aktiv? {#configuration-phase-faq7}

När integreringen med ett MVPD-program är aktiverad och markerad som aktiv, tas det MVPD med i listan över MVPD-program som finns i ytterligare konfigurationssvar och det finns två viktiga konsekvenser att tänka på:

* De oautentiserade användarna av detta MVPD kommer att kunna slutföra autentiseringsfasen igen med detta MVPD.
* De autentiserade användarna av detta MVPD kommer att kunna slutföra förauktoriserings-, auktoriserings- eller utloggningsfaserna igen med detta MVPD.

### 8. Hur aktiverar eller inaktiverar man integreringen med ett MVPD-program? {#configuration-phase-faq8}

Den här åtgärden kan utföras via Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration) .

+++

+++Vanliga frågor om autentiseringsfasen

### 1. Vad är syftet med autentiseringsfasen? {#authentication-phase-faq1}

Syftet med autentiseringsfasen är att ge klientprogrammet möjlighet att verifiera användarens identitet och få information om användarens metadata.

Autentiseringsfasen fungerar som ett nödvändigt steg för förauktoriseringsfasen eller auktoriseringsfasen när klientprogrammet behöver spela upp innehåll.

### 2. Hur vet klientprogrammet om användaren redan är autentiserad? {#authentication-phase-faq2}

Klientprogrammet kan fråga någon av följande slutpunkter som kan verifiera om en användare redan är autentiserad och returnera profilinformation:

* [API för profilslutpunkt](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profilslutpunkt för specifikt MVPD API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profilslutpunkt för specifik (autentisering) kod-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Mer information finns i följande dokument:

* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3. Hur kan klientprogrammet hämta användarens metadatainformation? {#authentication-phase-faq3}

Klientprogrammet kan fråga någon av följande slutpunkter som kan returnera [användarmetadata](/help/authentication/user-metadata-feature.md) som en del av profilinformationen:

* [API för profilslutpunkt](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profilslutpunkt för specifikt MVPD API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profilslutpunkt för specifik (autentisering) kod-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Mer information finns i följande dokument:

* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4. Vad är en autentiseringssession och hur länge gäller den? {#authentication-phase-faq4}

Autentiseringssessionen är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#session).

Autentiseringssessionen lagrar information om den initierade autentiseringsprocessen som kan hämtas från sessionens slutpunkt.

Autentiseringssessionen är giltig under en begränsad och kort tidsram som anges vid tidpunkten för utfärdandet, vilket anger hur lång tid användaren måste slutföra autentiseringsprocessen innan flödet måste startas om.

Klientprogrammet kan använda autentiseringssessionssvaret för att veta hur autentiseringsprocessen ska fortsätta. Observera att det finns fall där användaren inte behöver autentisera, till exempel temporär åtkomst, begränsad åtkomst eller när användaren redan är autentiserad.

Mer information finns i följande dokument:

* [Skapa API för autentiseringssession](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Återuppta autentiseringssessions-API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundläggande autentiseringsflöde som utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundläggande autentiseringsflöde som utförs i sekundärt program](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5. Vad är en autentiseringskod och hur länge gäller den? {#authentication-phase-faq5}

Autentiseringskoden är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#code).

Autentiseringskoden lagrar ett unikt värde som genereras när en användare initierar autentiseringsprocessen och identifierar unikt användarens autentiseringssession tills processen är slutförd eller tills autentiseringssessionen upphör.

Autentiseringskoden gäller under en begränsad och kort tidsperiod som anges när autentiseringssessionen initieras, vilket anger hur lång tid användaren måste slutföra autentiseringsprocessen innan flödet måste startas om.

Klientprogrammet kan använda autentiseringskoden för att tillåta användaren att slutföra eller återuppta autentiseringsprocessen antingen på samma enhet eller på en andra, med tanke på att autentiseringssessionen inte gick ut.

Mer information finns i följande dokument:

* [Skapa API för autentiseringssession](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Återuppta autentiseringssessions-API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundläggande autentiseringsflöde som utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundläggande autentiseringsflöde som utförs i sekundärt program](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6. Hur vet klientprogrammet om användaren har skrivit en giltig autentiseringskod och att autentiseringssessionen inte har gått ut än? {#authentication-phase-faq6}

Klientprogrammet kan validera den autentiseringskod som användaren skriver i ett sekundärt (skärm) program genom att skicka en begäran till sessionens slutpunkt som ansvarar för att hämta autentiseringssessionsinformation som är kopplad till autentiseringskoden.

Klientprogrammet skulle få ett [fel](/help/authentication/enhanced-error-codes.md) om den angivna autentiseringskoden skulle skrivas in fel eller om autentiseringssessionen skulle gå ut.

Mer information finns i dokumentationen för [Hämta autentiseringssession](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

### 7. Vad är en profil och hur länge gäller den? {#authentication-phase-faq7}

Profilen är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#profile).

Profilen lagrar information om användarens autentiseringsgiltighet, metadatainformation och mycket mer som kan hämtas från profilslutpunkten.

Klientprogrammet kan använda profilen för att känna till användarens autentiseringsstatus, komma åt användarens metadatainformation eller hitta den metod som används för att autentisera.

Mer information finns i följande dokument:

* [API för profilslutpunkt](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profilslutpunkt för specifikt MVPD API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profilslutpunkt för specifik (autentisering) kod-API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Profilen gäller under en begränsad tidsperiod som anges när användaren tillfrågas, vilket anger hur lång tid användaren har en giltig autentisering innan han eller hon måste gå igenom autentiseringsfasen igen.

Den här begränsade tidsramen som kallas autentisering (authN) [TTL](./rest-api-v2-glossary.md#ttl) kan visas och ändras via Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

+++

+++Vanliga frågor om förauktoriseringsfasen

### 1. Vad är syftet med förauktoriseringsfasen? {#preauthorization-phase-faq1}

Syftet med förauktoriseringsfasen är att ge klientprogrammet möjlighet att presentera en delmängd av resurser från sin katalog som användaren skulle ha rätt till.

Fas för förhandsauktorisering kan förbättra användarupplevelsen när användaren öppnar klientprogrammet för första gången eller navigerar till ett nytt avsnitt.

### 2. Är förhandsauktoriseringsfasen obligatorisk? {#preauthorization-phase-faq2}

Förhandsauktoriseringsfasen är inte obligatorisk. Klientprogrammet kan hoppa över den här fasen om det vill visa en katalog med resurser utan att först filtrera dem baserat på användarens tillstånd.

### 3. Vad är ett beslut om förhandstillstånd? {#preauthorization-phase-faq3}

Förhandsauktoriseringen är en term som definieras i [ordlistan](./rest-api-v2-glossary.md#preauthorization) , medan beslutstermen också finns i [ordlistan](./rest-api-v2-glossary.md#decision).

Förhandsauktoriseringsbeslutet lagrar information om förfrågningsresultatet för MVPD-förauktoriseringsprocessen som kan hämtas från slutpunkten för beslut och förauktorisering.

Klientprogrammet kan använda förauktoriseringsbesluten för att presentera en delmängd av resurser som tv-leverantören (informativt) skulle tillåta användaren att komma åt.

Mer information finns i följande dokument:

* [Hämta API för förauktoriseringsbeslut](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4. Varför saknas en medietoken i beslutet om förhandsauktorisering? {#preauthorization-phase-faq4}

Förauktoriseringsbeslutet saknar en medietoken eftersom förauktoriseringsfasen inte får användas för att spela upp resurser, eftersom det är syftet med auktoriseringsfasen.

### 5. Vad är en resurs och vilka format stöds? {#preauthorization-phase-faq5}

Resursen är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#resource).

Resursen är en unik identifierare som avtalas med MVPD-program och som är associerad med ett innehåll som klientprogrammet kan direktuppspela.

Den unika identifieraren för resursen kan ha två format:

* Ett enkelt strängformat, till exempel en unik identifierare för en kanal (varumärke).
* Ett medie-RSS-format (MRSS) som innehåller ytterligare information som titel, gradering och metadata för föräldrakontroll.

Mer information finns i dokumentationen för [Identifiera skyddade resurser](/help/authentication/identify-protected-resources.md).

### 6. För hur många resurser kan klientprogrammet få ett beslut om förauktorisering åt gången? {#preauthorization-phase-faq6}

Klientprogrammet kan få ett beslut om förhandsauktorisering för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 5, på grund av villkor som anges av distributörer av videoprogrammeringstjänster.

Det maximala antalet resurser kan visas och ändras efter att du har godkänt PDF-filer via Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) av en av organisationens administratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) .

+++

+++Vanliga frågor om auktoriseringsfasen

### 1. Vad är syftet med auktoriseringsfasen? {#authorization-phase-faq1}

Syftet med auktoriseringsfasen är att ge klientprogrammet möjlighet att spela upp resurser som användaren begär efter att ha verifierat sina rättigheter med det mobila dokumentationsprogrammet.

### 2. Är auktoriseringsfasen obligatorisk? {#authorization-phase-faq2}

Auktoriseringsfasen är obligatorisk. Klientprogrammet kan inte hoppa över den här fasen om det vill spela upp resurser som användaren begär, eftersom det måste verifiera med MVPD att användaren är berättigad innan strömmen släpps.

### 3. Vad är ett auktoriseringsbeslut och hur länge gäller det? {#authorization-phase-faq3}

Auktoriseringen är en term som definieras i [ordlistan](./rest-api-v2-glossary.md#authorization) , medan beslutstermen också finns i [ordlistan](./rest-api-v2-glossary.md#decision).

I auktoriseringsbeslutet lagras information om förfrågningsresultatet från MVPD-auktoriseringsprocessen som kan hämtas från slutpunkten för beslutsauktorisering.

Klientprogrammet kan använda auktoriseringsbeslutet som innehåller en medietoken för att spela upp resursströmmen om tv-providerns (auktoritativa) beslut skulle tillåta användaren att komma åt den.

Mer information finns i följande dokument:

* [Hämta API för auktoriseringsbeslut](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Auktoriseringsbeslutet gäller under en begränsad och kort tidsperiod som anges vid tidpunkten för utfärdandet, vilket anger hur lång tid det kommer att cachas av Adobe Pass Authentication innan det krävs att du frågar igenom PDF-filen igen.

Den här begränsade tidsramen som kallas auktorisering (authZ) [TTL](./rest-api-v2-glossary.md#ttl) kan visas och ändras via Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) av en av organisationens administratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

### 4. Vad är en medietoken och hur länge gäller den? {#authorization-phase-faq4}

Medietoken är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#media-token).

Medietoken består av en signerad sträng som skickas i klartext och som kan hämtas från slutpunkten Beslutsauktorisering.

Mer information finns i dokumentationen för [Integrating the Media Token Verifier](/help/authentication/media-token-verifier-int.md).

Medietoken är giltig under en begränsad och kort tidsperiod som anges vid tidpunkten för utfärdandet, vilket anger hur lång tid det måste användas av klientprogrammet innan det krävs att fråga slutpunkten för beslutsauktorisering igen.

Klientprogrammet kan använda medietoken för att spela upp en resursström om TV-providerns (auktoritativa) beslut skulle tillåta användaren att komma åt den.

Mer information finns i följande dokument:

* [Hämta API för auktoriseringsbeslut](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5. Vad är en resurs och vilka format stöds? {#authorization-phase-faq5}

Resursen är en term som definieras i dokumentationen för [ordlistan](./rest-api-v2-glossary.md#resource).

Resursen är en unik identifierare som avtalas med MVPD-program och som är associerad med ett innehåll som klientprogrammet kan direktuppspela.

Den unika identifieraren för resursen kan ha två format:

* Ett enkelt strängformat, till exempel en unik identifierare för en kanal (varumärke).
* Ett medie-RSS-format (MRSS) som innehåller ytterligare information som titel, gradering och metadata för föräldrakontroll.

Mer information finns i dokumentationen för [Identifiera skyddade resurser](/help/authentication/identify-protected-resources.md).

### 6. För hur många resurser kan klientprogrammet få ett auktoriseringsbeslut åt gången? {#authorization-phase-faq6}

Klientprogrammet kan få ett auktoriseringsbeslut för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 1, på grund av villkor som anges av distributörerna.

+++

+++Vanliga frågor om utloggningsfasen

### 1. Vad är syftet med utloggningsfasen? {#logout-phase-faq1}

Syftet med utloggningsfasen är att ge klientprogrammet möjlighet att avsluta användarens autentiserade profil inom Adobe Pass-autentisering på användarens begäran.

+++

+++Vanliga frågor om rubriker

### 1. Hur beräknar man värdet för auktoriseringshuvudet? {#headers-faq1}

Huvudet [Authorization](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) innehåller åtkomsttoken `Bearer` som krävs för att klientprogrammet ska få åtkomst till Adobe Pass-skyddade API:er.

Huvudvärdet för auktorisering måste hämtas från Adobe Pass-autentisering under registreringsfasen.

Mer information finns i följande dokument:

* [Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Hämta klientens autentiserings-API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Hämta API för åtkomsttoken](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamiskt klientregistreringsflöde](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

Om klientprogrammet migrerar från REST API V1 till REST API V2 kan klientprogrammet fortsätta använda samma metod för att hämta åtkomsttoken för `Bearer` som tidigare.

### 2. Hur beräknas värdet för rubriken AP-Device-Identifier? {#headers-faq2}

Huvudet för begäran [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) innehåller identifieraren för direktuppspelningsenheten som den skapades av klientprogrammet.

Huvuddokumentationen för [AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) innehåller exempel på hur du beräknar värdet för olika plattformar, men klientprogrammet kan välja att använda en annan metod baserat på sin egen affärslogik och sina egna krav.

Om klientprogrammet migrerar från REST API V1 till REST API V2 kan klientprogrammet fortsätta att använda samma metod för att beräkna enhetsidentifieraren som tidigare.

### 3. Hur beräknar man värdet för X-Device-Info-huvudet? {#headers-faq3}

Huvudet [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) innehåller klientinformationen (enhet, anslutning och program) som hör till den faktiska direktuppspelningsenheten.

I rubrikdokumentationen för [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) finns exempel på hur du beräknar värdet för olika plattformar, men klientprogrammet kan välja att använda en annan metod baserat på sin egen affärslogik och sina egna krav.

Om klientprogrammet migrerar från REST API V1 till REST API V2 kan klientprogrammet fortsätta att använda samma metod för att beräkna enhetsinformationen som tidigare.

+++

## Vanliga frågor om migrering {#migration-faqs}

Fortsätt med det här avsnittet om du arbetar med ett program som behöver migrera ett befintligt program till REST API V2.

+++Vanliga frågor om migrering

### 1. Måste jag köra ett nytt klientprogram som migrerats till REST API V2 för alla användare samtidigt? {#migration-faq1}

Nej.

Klientprogrammet behöver inte ha en ny version som integrerar REST API V2 för alla användare samtidigt.

Adobe Pass Authentication kommer att ha fortsatt stöd för äldre klientprogramversioner som integrerar REST API V1 eller SDK till och med slutet av 2025.

### 2. Måste jag köra ett nytt klientprogram som migrerats till REST API V2 för alla API:er och flöden samtidigt? {#migration-faq2}

Ja.

Klientprogrammet krävs för att lansera en ny version som integrerar REST API V2 i alla Adobe Pass Authentication API:er och flöden samtidigt.

Om autentiseringsflödet för den andra skärmen ska fungera måste klientprogrammet köra en ny version som integrerar REST API V2 för både [primära](./rest-api-v2-glossary.md#primary-application) - och [sekundära](./rest-api-v2-glossary.md#secondary-application) -program samtidigt.

Adobe Pass Authentication stöder inte hybridimplementeringar som integrerar både REST API V2 och REST API V1/SDK mellan API:er och flöden.

### 3. Bevaras användarautentiseringen vid uppdatering till ett nytt klientprogram som migrerats till REST API V2? {#migration-faq3}

Nej.

Användarautentiseringen som erhållits i äldre klientprogramversioner som integrerar REST API V1 eller SDK bevaras inte.

Därför måste användaren autentisera igen i det nya klientprogrammet som migrerats till REST API V2.

### 4. Kan klientprogrammet använda de befintliga registrerade programmen (programsatser)? {#migration-faq4}

Klientprogrammet kan inte återanvända de befintliga registrerade programmen (programsatser), och därför måste ett nytt registrerat program (programsatser) som är dedikerat för att använda REST API V2 genereras och hämtas.

Den här åtgärden kan utföras via Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i användarhandboken för [TVE Dashboard Channels](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) eller [TVE Dashboard Programmers ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) .

För tillfället måste du be en Adobe Pass-autentiseringsrepresentant om att aktivera användningen av REST API V2 för dina nya registrerade program (programsatser) tills Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) uppdateras för att tillåta självhantering för den här åtgärden.

För att kunna skilja på registrerade program (programsatser) som används i klientprogram som använder REST API V2, måste du lägga till ett specifikt suffix till det registrerade programnamnet, till exempel&quot;RESTV2&quot;.

### 5. Kan klientprogrammet använda befintliga anpassade scheman? {#migration-faq5}

Klientprogrammet kan återanvända befintliga anpassade scheman som genererats via Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard).

Mer information finns i användarhandboken för [TVE Dashboard Channels](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes) eller [TVE Dashboard Programmers ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) .

### 6. Är de förbättrade felkoderna aktiverade som standard i REST API V2? {#migration-faq6}

Ja.

Klientprogrammen som integrerar REST API V2 drar nytta av den förbättrade felkodsfunktionen som är aktiverad som standard.

Mer information finns i dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migrering från REST API V1 till REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Fortsätt med det här underavsnittet om du arbetar med ett program som behöver migrera från REST API V1 till REST API V2.

+++Vanliga frågor om registreringsfasen

#### 1. Vilka är de högnivåmigreringar av API:er som krävs för registreringsfasen? {#registration-phase-v1-to-v2-faq1}

I övergången från REST API V1 till REST API V2 finns det inga omfattande förändringar i registreringsfasen.

Klientprogrammet kan fortsätta att använda samma flöde för att registrera sig mot Adobe Pass-autentisering via processen [Dynamic Client Registration (DCR)](./rest-api-v2-glossary.md#dcr) och få en åtkomsttoken.

Mer information finns i följande dokument:

* [Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [Hämta klientens autentiserings-API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Hämta API för åtkomsttoken](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamiskt klientregistreringsflöde](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

+++Vanliga frågor om konfigurationsfasen

#### 1. Vilka är de högnivå-API-migreringar som krävs för konfigurationsfasen? {#configuration-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++Vanliga frågor om autentiseringsfasen

#### 1. Vilka är de högnivåmigreringar av API som krävs för autentiseringsfasen? {#authentication-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta registreringskod (autentiseringskod) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera registreringskod (autentiseringskod) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioner/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Återuppta MVPD-autentisering på andra skärmen (program) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessioner/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiera (MVPD) autentisering | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [GET <br/> /api/v1/checkauthn (första skärmen)](/help/authentication/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (andra skärmen)](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta token för användarautentisering (profil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Vanliga frågor om förauktoriseringsfasen

#### 1. Vilka är de högnivåmigreringar av API:er som krävs för fasen för förhandsauktorisering? {#preauthorization-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [GET <br/> /api/v1/förauktorisera (första skärmen)](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorized (second screen)](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++Vanliga frågor om auktoriseringsfasen

#### 1. Vilka är de högnivåmigreringar av API som krävs för auktoriseringsfasen? {#authorization-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD)-auktorisering | [GET <br/> /api/v1/authorized](/help/authentication/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD)-auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Hämta auktoriseringstoken (auktoriseringsbeslut) | [GET <br/> /api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD)-auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Hämta kort auktoriseringstoken (medietoken) | [GET <br/> /api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD)-auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++Vanliga frågor om utloggningsfasen

#### 1. Vilka är de högnivåmigreringar av API:er som krävs för utloggningsfasen? {#logout-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera utloggning | [GET <br/> /api/v1/logOut](/help/authentication/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migrera från SDK till REST API V2 {#migrate-sdk-to-rest-api-v2}

Fortsätt med det här underavsnittet om du arbetar med ett program som behöver migreras från SDK till REST API V2.

+++Vanliga frågor om registreringsfasen

#### 1. Vilka är de högnivåmigreringar av API:er som krävs för registreringsfasen? {#registration-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

##### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Mer information finns i följande dokument: <br/> <ul><li>[Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Mer information finns i följande dokument: <br/> <ul><li>[Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Mer information finns i följande dokument: <br/> <ul><li>[Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Mer information finns i följande dokument: <br/> <ul><li>[Översikt över registrering av dynamisk klient](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

+++Vanliga frågor om konfigurationsfasen

#### 1. Vilka är de högnivå-API-migreringar som krävs för konfigurationsfasen? {#configuration-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

##### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++Vanliga frågor om autentiseringsfasen

#### 1. Vilka är de högnivåmigreringar av API som krävs för autentiseringsfasen? {#authentication-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

##### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/javascript-sdk-api-reference.md#getMeta) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler iOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta registreringskod (autentiseringskod) | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera registreringskod (autentiseringskod) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioner/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Återuppta MVPD-autentisering på andra skärmen (program) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessioner/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/android-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/android-sdk-api-reference.md#getMetadata) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta registreringskod (autentiseringskod) | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera registreringskod (autentiseringskod) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessioner/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Återuppta MVPD-autentisering på andra skärmen (program) | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessioner/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessioner](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++Vanliga frågor om förauktoriseringsfasen

#### 1. Vilka är de högnivåmigreringar av API:er som krävs för fasen för förhandsauktorisering? {#preauthorization-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

##### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorized](/help/authentication/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorized](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorized](/help/authentication/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++Vanliga frågor om auktoriseringsfasen

#### 1. Vilka är de högnivåmigreringar av API som krävs för auktoriseringsfasen? {#authorization-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

##### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD)-auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD)-auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD)-auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD)-auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++Vanliga frågor om utloggningsfasen

#### 1. Vilka är de högnivåmigreringar av API:er som krävs för utloggningsfasen? {#logout-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

##### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++