---
title: REST API V2 - frågor och svar
description: REST API V2 - frågor och svar
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '9611'
ht-degree: 0%

---

# REST API V2 - frågor och svar {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Det här dokumentet innehåller omfattande översiktssvar på vanliga frågor om användningen av Adobe Pass Authentication REST API V2.

Mer information om REST API V2 som helhet finns i [REST API V2 Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) -dokumentationen.

## Allmänna frågor och svar {#general-faqs}

Börja med det här avsnittet om du arbetar med ett program som behöver integrera REST API V2, oavsett om det är ett nytt program eller ett befintligt som migreras från [REST API V1](#migration-rest-api-v1-to-rest-api-v2) eller [SDK](#migration-sdk-to-rest-api-v2).

Mer information om migreringsinformation och -steg finns även i nästa avsnitt.

### Vanliga frågor om registreringsfasen {#registration-phase-faqs-general}

+++Vanliga frågor om registreringsfasen

Se [DCR-dokumentationen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs) med vanliga frågor om registrering av dynamiska klienter.

+++

### Vanliga frågor om konfigurationsfasen {#configuration-phase-faqs-general}

+++Vanliga frågor om konfigurationsfasen

#### &#x200B;1. Vad är syftet med konfigurationsfasen? {#configuration-phase-faq1}

Syftet med konfigurationsfasen är att ge klientprogrammet en lista över de MVPD-filer som det är aktivt integrerat med konfigurationsinformation (t.ex. `id`, `displayName`, `logoUrl` osv.) som sparats av Adobe Pass Authentication för varje MVPD.

Konfigurationsfasen fungerar som ett nödvändigt steg för autentiseringsfasen när klientprogrammet måste be användaren att välja sin TV-leverantör.

#### &#x200B;2. Är konfigurationsfasen obligatorisk? {#configuration-phase-faq2}

Konfigurationsfasen är inte obligatorisk. Klientprogrammet måste hämta konfigurationen först när användaren måste välja sin MVPD för att autentisera eller återautentisera.

Klientprogrammet kan hoppa över den här fasen i följande scenarier:

* Användaren är redan autentiserad.
* Användaren erbjuds tillfällig åtkomst via grundläggande eller kampanjanpassad [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md)-funktion.
* Användarautentiseringen har upphört att gälla, men klientprogrammet har cachelagrat den tidigare valda MVPD som ett motiverat val av användarupplevelse och uppmanar bara användaren att bekräfta att han/hon fortfarande prenumererar på denna MVPD.

#### &#x200B;3. Vad är en konfiguration och hur länge gäller den? {#configuration-phase-faq3}

Konfigurationen är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

Konfigurationen innehåller en lista med MVPD-filer som definieras av följande attribut, `id`, `displayName`, `logoUrl` osv., som kan hämtas från [Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)-slutpunkten.

Klientprogrammet måste hämta konfigurationen först när användaren måste välja sin MVPD för att kunna autentisera eller autentisera igen.

Klientprogrammet kan använda konfigurationssvaret för att visa en UI-väljare med tillgängliga MVPD-alternativ när användaren behöver välja sin TV-leverantör.

Klientprogrammet måste lagra användarens valda MVPD-identifierare, enligt MVPD konfigurationsnivåattribut `id`, för att kunna fortsätta med autentiserings-, förauktoriserings-, auktoriserings- eller utloggningsfaserna.

Mer information finns i dokumentationen för [Hämta konfiguration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### &#x200B;4. Är konfigurationen specifik för en tjänsteleverantör, plattform eller användare? {#configuration-phase-faq4}

Konfigurationen är specifik för en [tjänstleverantör](rest-api-v2-glossary.md#service-provider).

Konfigurationen är specifik för en plattformstyp.

Konfigurationen är inte specifik för en användare.

För klientprogram som använder en server-till-server-arkitektur rekommenderas att konfigurationssvaret cachelagras (t.ex. med en TTL på 2 minuter) för varje plattformstyp i minneslagring på serversidan. Detta minskar antalet onödiga förfrågningar för varje användare och förbättrar den övergripande användarupplevelsen.

#### &#x200B;5. Ska klientprogrammet cachelagra konfigurationssvarsinformationen i en beständig lagring? {#configuration-phase-faq5}

>[!IMPORTANT]
> 
> För klientprogram som använder en server-till-server-arkitektur rekommenderas att konfigurationssvaret cachelagras (t.ex. med en TTL på 2 minuter) för varje plattformstyp i minneslagring på serversidan. Detta minskar antalet onödiga förfrågningar för varje användare och förbättrar den övergripande användarupplevelsen.

Klientprogrammet måste hämta konfigurationen först när användaren måste välja sin MVPD för att kunna autentisera eller autentisera igen.

Klientprogrammet bör cachelagra konfigurationssvarsinformationen i en minneslagring för att undvika onödiga begäranden och förbättra användarupplevelsen när:

* Användaren är redan autentiserad.
* Användaren erbjuds tillfällig åtkomst via grundläggande eller kampanjanpassad [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md)-funktion.
* Användarautentiseringen har upphört att gälla, men klientprogrammet har cachelagrat den tidigare valda MVPD som ett motiverat val av användarupplevelse och uppmanar bara användaren att bekräfta att han/hon fortfarande prenumererar på denna MVPD.

#### &#x200B;6. Kan klientapplikationen hantera sin egen lista över MVPD? {#configuration-phase-faq6}

Klientprogrammet kan hantera sin egen lista över MVPD-program, men det skulle kräva att MVPD-identifierarna hålls synkroniserade med Adobe Pass Authentication. Vi rekommenderar därför att du använder konfigurationen från Adobe Pass Authentication för att se till att listan är aktuell och korrekt.

Klientprogrammet skulle få ett [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) från Adobe Pass Authentication REST API V2 om den angivna MVPD-identifieraren är ogiltig eller om den inte har en aktiv integrering med den angivna [tjänstprovidern](rest-api-v2-glossary.md#service-provider).

#### &#x200B;7. Kan klientprogrammet filtrera listan över MVPD? {#configuration-phase-faq7}

Klientprogrammet kan filtrera listan över MVPD-program som anges i konfigurationssvaret genom att implementera en anpassad mekanism som bygger på dess egen affärslogik och krav som användarplats eller användarhistorik för det tidigare urvalet.

Klientprogrammet kan filtrera listan över [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md)-MVPD-filer eller MVPD-filer som fortfarande är integrerade i utveckling eller testning.

#### &#x200B;8. Vad händer om integreringen med en MVPD är inaktiverad och markerad som inaktiv? {#configuration-phase-faq8}

När integreringen med en MVPD är inaktiverad och markerad som inaktiv tas MVPD bort från listan över MVPD-program som finns i ytterligare konfigurationssvar, och det finns två viktiga följder att tänka på:

* De oautentiserade användarna av denna MVPD kommer inte längre att kunna slutföra autentiseringsfasen med denna MVPD.
* De autentiserade användarna av denna MVPD kommer inte längre att kunna slutföra förauktoriserings-, auktoriserings- eller utloggningsfaserna med denna MVPD.

Klientprogrammet skulle få ett [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) från Adobe Pass Authentication REST API V2 om användaren valde MVPD inte längre har någon aktiv integrering med den angivna [tjänstprovidern](rest-api-v2-glossary.md#service-provider).

#### &#x200B;9. Vad händer om integreringen med en MVPD är aktiverad och markerad som aktiv? {#configuration-phase-faq9}

När integreringen med en MVPD är aktiverad och markerad som aktiv, tas MVPD med i listan över distributörer av videoprogrammeringstjänster som finns i ytterligare konfigurationssvar, och det finns två viktiga konsekvenser att tänka på:

* De oautentiserade användarna av denna MVPD kan slutföra autentiseringsfasen igen med denna MVPD.
* De autentiserade användarna av denna MVPD kommer att kunna slutföra förauktoriserings-, auktoriserings- eller utloggningsfaserna med denna MVPD.

#### &#x200B;10. Hur aktiverar eller inaktiverar jag integreringen med en MVPD? {#configuration-phase-faq10}

Den här åtgärden kan utföras via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration) .

+++

### Vanliga frågor om autentiseringsfasen {#authentication-phase-faqs-general}

+++Vanliga frågor om autentiseringsfasen

#### &#x200B;1. Vad är syftet med autentiseringsfasen? {#authentication-phase-faq1}

Syftet med autentiseringsfasen är att ge klientprogrammet möjlighet att verifiera användarens identitet och få information om användarens metadata.

Autentiseringsfasen fungerar som ett nödvändigt steg för förauktoriseringsfasen eller auktoriseringsfasen när klientprogrammet behöver spela upp innehåll.

#### &#x200B;2. Är autentiseringsfasen obligatorisk? {#authentication-phase-faq2}

Autentiseringsfasen är obligatorisk. Klientprogrammet måste autentisera användaren när det inte har en giltig profil inom Adobe Pass-autentisering.

Klientprogrammet kan hoppa över den här fasen i följande scenarier:

* Användaren är redan autentiserad och profilen är fortfarande giltig.
* Användaren erbjuds tillfällig åtkomst via grundläggande eller kampanjanpassad [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md)-funktion.

Felhanteringen i klientprogrammet kräver att [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)-koderna (t.ex. `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated` osv.) hanteras, vilket anger att klientprogrammet kräver att användaren autentiserar.

#### &#x200B;3. Vad är en autentiseringssession och hur länge gäller den? {#authentication-phase-faq3}

Autentiseringssessionen är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

Autentiseringssessionen lagrar information om den initierade autentiseringsprocessen som kan hämtas från sessionens slutpunkt.

Autentiseringssessionen är giltig under en begränsad och kort tidsram som anges vid utfärdandetillfället av tidsstämpeln `notAfter`, vilket anger hur lång tid användaren måste slutföra autentiseringsprocessen innan flödet måste startas om.

Klientprogrammet kan använda autentiseringssessionssvaret för att veta hur autentiseringsprocessen ska fortsätta. Observera att det finns fall där användaren inte behöver autentisera, till exempel temporär åtkomst, begränsad åtkomst eller när användaren redan är autentiserad.

Mer information finns i följande dokument:

* [Skapa API för autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Återuppta autentiseringssessions-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundläggande autentiseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundläggande autentiseringsflöde som utförs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. Vad är en autentiseringskod och hur länge gäller den? {#authentication-phase-faq4}

Autentiseringskoden är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

Autentiseringskoden lagrar ett unikt värde som genereras när en användare initierar autentiseringsprocessen och identifierar unikt användarens autentiseringssession tills processen är slutförd eller tills autentiseringssessionen upphör.

Autentiseringskoden är giltig under en begränsad och kort tidsperiod som anges när autentiseringssessionen initieras av tidsstämpeln `notAfter`, vilket anger hur lång tid användaren måste slutföra autentiseringsprocessen innan flödet måste startas om.

Klientprogrammet kan använda autentiseringskoden för att verifiera om användaren har slutfört autentiseringen och hämta användarens profilinformation, vanligtvis via en avsökningsmekanism.

Klientprogrammet kan också använda autentiseringskoden för att tillåta användaren att slutföra eller återuppta autentiseringsprocessen antingen på samma enhet eller på en andra (skärm), med tanke på att autentiseringssessionen inte gick ut.

Mer information finns i följande dokument:

* [Skapa API för autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Återuppta autentiseringssessions-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Grundläggande autentiseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundläggande autentiseringsflöde som utförs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;5. Hur vet klientprogrammet om användaren har skrivit en giltig autentiseringskod och att autentiseringssessionen inte har gått ut än? {#authentication-phase-faq5}

Klientprogrammet kan validera den autentiseringskod som användaren skriver i ett sekundärt (skärm) program genom att skicka en begäran till någon av sessionens slutpunkter som ansvarar för att återuppta autentiseringssessionen eller hämta autentiseringssessionsinformation som är kopplad till autentiseringskoden.

Klientprogrammet skulle få ett [fel](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) om den angivna autentiseringskoden skrivits fel eller om autentiseringssessionen skulle gå ut.

Mer information finns i dokumenten för [Återuppta autentiseringssessionen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) och [Hämta autentiseringssessionen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### &#x200B;6. Hur vet klientprogrammet om användaren redan är autentiserad? {#authentication-phase-faq6}

Klientprogrammet kan fråga någon av följande slutpunkter som kan verifiera om en användare redan är autentiserad och returnera profilinformation:

* [API för profilslutpunkt](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profilslutpunkt för specifikt MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profilslutpunkt för specifik (autentisering) kod-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Mer information finns i följande dokument:

* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. Vad är en profil och hur länge gäller den? {#authentication-phase-faq7}

Profilen är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

Profilen lagrar information om användarens autentiseringsgiltighet, metadatainformation och mycket mer som kan hämtas från profilslutpunkten.

Klientprogrammet kan använda profilen för att känna till användarens autentiseringsstatus, komma åt användarens metadatainformation, hitta den metod som används för att autentisera eller enheten som används för att ange identitet.

Mer information finns i följande dokument:

* [API för profilslutpunkt](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profilslutpunkt för specifikt MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profilslutpunkt för specifik (autentisering) kod-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Profilen är giltig under en begränsad tidsperiod som anges när den efterfrågas av tidsstämpeln `notAfter`, vilket anger hur lång tid användaren har en giltig autentisering innan han eller hon måste gå igenom autentiseringsfasen igen.

Den här begränsade tidsramen som kallas autentisering (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) kan visas och ändras via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

#### &#x200B;8. Ska klientprogrammet cachelagra användarens profilinformation i ett beständigt lagringsutrymme? {#authentication-phase-faq8}

Klientprogrammet bör cachelagra delar av användarens profilinformation i en beständig lagringsplats för att undvika onödiga begäranden och förbättra användarupplevelsen med tanke på följande:

| Attribut | Användarupplevelse |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | Klientprogrammet kan använda detta för att hålla reda på användarens valda TV-leverantör och fortsätta att använda det under förauktoriserings- eller auktoriseringsfaserna.<br/><br/>När den aktuella användarprofilen förfaller kan klientprogrammet använda det sparade MVPD-valet och be användaren bekräfta. |
| `attributes` | Klientprogrammet kan använda detta för att anpassa användarupplevelsen baserat på olika [användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)-nycklar (t.ex. `zip`, `maxRating` osv.).<br/><br/>Användarmetadata blir tillgängliga när autentiseringsflödet har slutförts. Klientprogrammet behöver därför inte fråga en separat slutpunkt för att hämta informationen för [användarens metadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) eftersom den redan ingår i profilinformationen.<br/><br/>Vissa metadataattribut kan uppdateras under auktoriseringsflödet, beroende på MVPD och det specifika metadataattributet. Därför kan klientprogrammet behöva fråga Profiles-API:erna igen för att hämta de senaste användarens metadata. |
| `notAfter` | Klientprogrammet kan använda detta för att hålla reda på utgångsdatumet för användarprofilen.<br/><br/>Felhanteringen i klientprogrammet kräver att [ error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) -koderna (t.ex. `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated` osv.) hanteras, vilket anger att klientprogrammet kräver att användaren autentiserar. |

#### &#x200B;9. Kan klientprogrammet utöka användarens profil utan att omautentisering krävs? {#authentication-phase-faq9}

Nej.

Användarprofilen kan inte förlängas utanför sin giltighet utan användarinteraktion, eftersom dess förfallotid bestäms av autentiserings-TTL:en som upprättats med MVPD:er.

Klientprogrammet måste därför uppmana användaren att autentisera igen och interagera med MVPD inloggningssida för att uppdatera sin profil på vårt system.

För MVPD-program som stöder [hembaserad autentisering](/help/premium-workflow/hba-access/home-based-authentication.md) (HBA) behöver användaren inte ange några autentiseringsuppgifter.

#### &#x200B;10. Vilka är användningsexemplen för de tillgängliga profilslutpunkterna? {#authentication-phase-faq10}

De grundläggande profilslutpunkterna är utformade för att ge klientprogrammet möjlighet att känna till användarens autentiseringsstatus, få åtkomst till användarens metadatainformation, hitta den metod som används för att autentisera eller den enhet som används för att ange identitet.

Varje slutpunkt passar ett specifikt användningsfall, enligt följande:

| API | Beskrivning | Använd skiftläge |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Profilslutpunkts-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Hämta alla användarprofiler. | **Användaren öppnar klientprogrammet för första gången**<br/><br/> I det här scenariot har klientprogrammet inte användarens valda MVPD-identifierare cachelagrad i beständig lagring.<br/><br/>Det innebär att programmet skickar en begäran om att hämta alla tillgängliga användarprofiler. |
| [Profilslutpunkt för specifikt MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Hämta användarprofilen som är kopplad till en viss MVPD. | **Användaren återgår till klientprogrammet efter att ha autentiserats vid ett tidigare besök**<br/><br/> I det här fallet måste användarens tidigare valda MVPD-identifierare cachelagras i det beständiga lagringsutrymmet.<br/><br/>Det innebär att programmet skickar en enda begäran om att hämta användarens profil för den specifika MVPD:n. |
| [Profilslutpunkt för specifikt (autentisering) kods-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Hämta användarprofilen som är associerad med en viss autentiseringskod. | **Användaren initierar autentiseringsprocessen**<br/><br/> I det här scenariot måste klientprogrammet avgöra om användaren har slutfört autentiseringen och hämta profilinformationen.<br/><br/>Detta resulterar i att en avsökningsmekanism startas för att hämta användarens profil som är associerad med autentiseringskoden. |

Mer information finns i det [grundläggande profilflödet som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) och i det [grundläggande profilflödet som utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md).

Profilens SSO-slutpunkt har ett annat syfte och ger klientprogrammet möjlighet att skapa en användarprofil med partnerautentiseringssvaret och hämta den i en enda engångsåtgärd.

| API | Beskrivning | Använd skiftläge |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Profiler för SSO-slutpunkts-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | Skapa och hämta användarprofil med partnerautentiseringssvar. | **Användaren tillåter programmet att använda enkel inloggning från partner för att autentisera**<br/><br/> I det här scenariot måste klientprogrammet skapa en användarprofil efter att ha tagit emot partnerautentiseringssvaret och hämta den i en enda engångsåtgärd. |

För efterföljande frågor måste de grundläggande profilslutpunkterna användas för att fastställa användarens autentiseringsstatus, få åtkomst till användarens metadatainformation, hitta den metod som används för att autentisera eller den enhet som används för att ange identitet.

Mer information finns i dokumenten för [enkel inloggning med partnerflöden](/help/premium-workflow/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

#### &#x200B;11. Vad ska klientprogrammet göra om användaren har flera MVPD-profiler? {#authentication-phase-faq11}

Beslutet att ge stöd för flera profiler beror på klientprogrammets affärskrav.

De flesta användare har bara en profil. I de fall där det finns flera profiler (som beskrivs nedan) ansvarar klientprogrammet för att fastställa den bästa användarupplevelsen för val av profil.

Klientprogrammet kan välja att uppmana användaren att välja önskad MVPD-profil eller göra urvalet automatiskt, till exempel välja den första användarprofilen i svaret eller den som har längst giltighetsperiod.

REST API v2 har stöd för flera profiler:

* Användare som kan behöva välja mellan en vanlig MVPD-profil och en som skaffats via enkel inloggning (SSO).
* Användare som erbjuds tillfällig åtkomst utan att behöva logga ut från sin vanliga MVPD.
* Användare med MVPD prenumeration kombinerat med DTC-tjänster (Direct-to-Consumer).
* Användare med flera MVPD-prenumerationer.

#### &#x200B;12. Vad händer när användarprofiler upphör att gälla? {#authentication-phase-faq12}

När användarprofiler förfaller inkluderas de inte längre i svaret från profilslutpunkten.

Om slutpunkten för profiler returnerar ett tomt profilmappningssvar måste klientprogrammet skapa en ny autentiseringssession och uppmana användaren att autentisera igen.

Mer information finns i dokumentationen för [Skapa autentiseringssessions-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

#### &#x200B;13. När blir användarprofiler ogiltiga? {#authentication-phase-faq13}

Användarprofiler blir ogiltiga i följande scenarier:

* När autentiserings-TTL upphör att gälla, vilket anges av tidsstämpeln `notAfter` i slutpunktssvaret för profiler.
* När klientprogrammet ändrar rubrikvärdet [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).
* När klientprogrammet uppdaterar klientautentiseringsuppgifterna som används för att hämta rubrikvärdet [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md).
* När klientprogrammet återkallar eller uppdaterar programsatsen som används för att hämta klientautentiseringsuppgifter.

#### &#x200B;14. När ska klientprogrammet starta avsökningsmekanismen? {#authentication-phase-faq14}

För att säkerställa effektivitet och undvika onödiga förfrågningar måste klientprogrammet starta avsökningsfunktionen på följande villkor:

**Autentisering utförd i det primära (skärm) programmet**

Det primära (direktuppspelande) programmet ska starta avsökningen när användaren kommer till den sista målsidan, efter att webbläsarkomponenten har läst in den URL som angetts för parametern `redirectUrl` i [ Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) -slutpunktsbegäran.

**Autentisering utförd i ett sekundärt (skärm) program**

Det primära (direktuppspelande) programmet bör starta avsökningen så snart användaren initierar autentiseringsprocessen, direkt efter att ha tagit emot slutpunktssvaret för [sessionerna](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) och visat autentiseringskoden för användaren.

#### &#x200B;15. När ska klientprogrammet stoppa avsökningsmekanismen? {#authentication-phase-faq15}

För att säkerställa effektivitet och undvika onödiga förfrågningar måste klientprogrammet stoppa avsökningsfunktionen under följande förhållanden:

**Autentiseringen har slutförts**

Användarens profilinformation har hämtats och verifierar autentiseringsstatusen. I nuläget behövs ingen avsökning längre.

**Autentiseringssession och kodutgång**

Autentiseringssessionen och koden upphör att gälla, vilket anges av tidsstämpeln `notAfter` (t.ex. 30 minuter) i slutpunktssvaret för [sessionerna](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Om detta inträffar måste användaren starta om autentiseringsprocessen och avsökningen med den tidigare autentiseringskoden ska stoppas omedelbart.

**Ny autentiseringskod genererad**

Om användaren begär en ny autentiseringskod på den primära (skärm) enheten är den befintliga sessionen inte längre giltig och avsökningen med den föregående autentiseringskoden bör stoppas omedelbart.

#### &#x200B;16. Vilket intervall mellan anrop ska klientprogrammet använda för avsökningsmekanismen? {#authentication-phase-faq16}

För att säkerställa effektivitet och undvika onödiga förfrågningar måste klientprogrammet konfigurera avsökningsmekanismens frekvens enligt följande villkor:

| **Autentisering utförd i det primära (skärm) programmet** | **Autentisering utförd i ett sekundärt (skärm) program** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| Det primära programmet (direktuppspelning) ska avsöka var 3-5:e sekund eller mer. | Det primära programmet (direktuppspelning) ska avsöka var 3-5:e sekund eller mer. |

#### &#x200B;17. Hur många avsökningsbegäranden kan klientprogrammet skicka? {#authentication-phase-faq17}

Klientprogrammet måste följa de aktuella gränserna som definieras av Adobe Pass-autentiseringsmekanismen [Throttling Mechanism](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits).

Felhanteringen i klientprogrammet måste kunna hantera felkoden [429 för många begäranden](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response) , vilket anger att klientprogrammet har överskridit det högsta antalet tillåtna begäranden.

Mer information finns i dokumentationen för [Begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

#### &#x200B;18. Hur kan klientprogrammet hämta användarens metadatainformation? {#authentication-phase-faq18}

Klientprogrammet kan fråga någon av följande slutpunkter som kan returnera [användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) som en del av profilinformationen:

* [API för profilslutpunkt](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Profilslutpunkt för specifikt MVPD API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Profilslutpunkt för specifik (autentisering) kod-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Användarmetadata blir tillgängliga när autentiseringsflödet har slutförts och klientprogrammet behöver därför inte fråga en separat slutpunkt för att hämta informationen för [användarens metadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) eftersom den redan ingår i profilinformationen.

Mer information finns i följande dokument:

* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Vissa metadataattribut kan uppdateras under auktoriseringsflödet beroende på MVPD och det specifika metadataattributet. Därför kan klientprogrammet behöva fråga API:erna ovan igen för att hämta de senaste användarmetadata.

#### &#x200B;19. Hur ska klientprogrammet hantera försämrad åtkomst? {#authentication-phase-faq19}

[Försämringsfunktionen](/help/premium-workflow/degraded-access/degradation-feature.md) gör att klientprogrammet kan upprätthålla en sömlös direktuppspelning för användare, även när deras MVPD autentiserings- eller auktoriseringstjänster stöter på problem.

Sammanfattningsvis kan detta säkerställa oavbruten åtkomst till innehåll trots att MVPD tillfälligt upphör med tjänsten.

Med tanke på att din organisation har för avsikt att använda funktionen för premiumförsämring måste klientprogrammet hantera nedbrutna åtkomstflöden, som beskriver hur REST API v2-slutpunkter fungerar i sådana scenarier.

Mer information finns i dokumentationen för [Försämrade åtkomstflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).

#### &#x200B;20. Hur ska klientprogrammet hantera temporär åtkomst? {#authentication-phase-faq20}

Med funktionen [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md) kan klientprogrammet ge användaren tillfällig åtkomst.

Sammanfattningsvis kan detta engagera användarna genom att ge dem tidsbegränsad tillgång till materialet eller ett fördefinierat antal VOD-titlar under en viss tidsperiod.

Med tanke på att din organisation har för avsikt att använda premiumfunktionen TempPass måste klientprogrammet hantera tillfälliga åtkomstflöden, som beskriver hur REST API v2-slutpunkter fungerar i sådana scenarier.

I tidigare API-versioner var klientprogrammet tvunget att logga ut en användare som autentiserats med sin vanliga MVPD för att ge temporär åtkomst.

Med REST API v2 kan klientprogrammet smidigt växla mellan en vanlig MVPD och en TempPass MVPD vid auktorisering av en direktuppspelning, vilket eliminerar behovet av utloggning.

Mer information finns i dokumentationen för [Tillfälliga åtkomstflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

#### &#x200B;21. Hur ska klientprogrammet hantera åtkomst för enkel inloggning mellan olika enheter? {#authentication-phase-faq21}

REST API v2 kan aktivera enkel inloggning på olika enheter om klientprogrammet ger en konsekvent unik användaridentifierare på olika enheter.

Den här identifieraren, som kallas tjänsttoken, måste genereras av klientprogrammet genom implementering eller integrering av en extern identitetstjänst som du väljer.

Mer information finns i dokumentationen för [enkel inloggning med tjänsttokenflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

+++

### Vanliga frågor om förauktoriseringsfasen {#preauthorization-phase-faqs-general}

+++Vanliga frågor om förauktoriseringsfasen

#### &#x200B;1. Vad är syftet med förauktoriseringsfasen? {#preauthorization-phase-faq1}

Syftet med förauktoriseringsfasen är att ge klientprogrammet möjlighet att presentera en delmängd av resurser från sin katalog som användaren skulle ha rätt till.

Fas för förhandsauktorisering kan förbättra användarupplevelsen när användaren öppnar klientprogrammet för första gången eller navigerar till ett nytt avsnitt.

#### &#x200B;2. Är förhandsauktoriseringsfasen obligatorisk? {#preauthorization-phase-faq2}

Förhandsauktoriseringsfasen är inte obligatorisk. Klientprogrammet kan hoppa över den här fasen om det vill visa en katalog med resurser utan att först filtrera dem baserat på användarens tillstånd.

#### &#x200B;3. Vad är ett beslut om förhandstillstånd? {#preauthorization-phase-faq3}

Förhandsauktoriseringen är en term som definieras i [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization) , medan beslutstermen också finns i [ordlistan](rest-api-v2-glossary.md#decision).

Förhandsauktoriseringsbeslutet lagrar information om MVPD-förauktoriseringsprocessförfrågningar som kan hämtas från slutpunkten för förauktorisering av beslut.

Klientprogrammet kan använda förauktoriseringsbesluten för att presentera en delmängd av resurser som tv-leverantören (informativt) skulle tillåta användaren att komma åt.

Mer information finns i följande dokument:

* [Hämta API för förauktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. Ska klientprogrammet cachelagra förauktoriseringsbesluten i en beständig lagring? {#preauthorization-phase-faq4}

Klientprogrammet behövs inte för att lagra förauktoriseringsbeslut i beständig lagring. Vi rekommenderar dock att du cachelagrar tillståndsbeslut i minnet för att förbättra användarupplevelsen. Detta bidrar till att undvika onödiga anrop till slutpunkten för beslut Förauktorisera för resurser som redan har förauktoriserats, vilket minskar latensen och förbättrar prestandan.

#### &#x200B;5. Hur kan klientprogrammet avgöra varför ett beslut om förauktorisering nekades? {#preauthorization-phase-faq5}

Klientprogrammet kan fastställa orsaken till ett beslut om förauktorisering som nekas genom att granska [felkoden och meddelandet](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) som ingår i svaret från slutpunkten för förauktorisering av beslut. Dessa detaljer ger insikt i den specifika anledningen till att förauktoriseringsbegäran nekades, vilket kan hjälpa användaren att informera om användarupplevelsen eller utlösa nödvändig hantering i programmet.

Se till att alla återförsöksmetoder som implementeras för att hämta beslut om förauktorisering inte resulterar i en oändlig slinga om beslutet om förauktorisering nekas.

Överväg att begränsa antalet försök till ett rimligt antal och hantera nekanden på ett enkelt sätt genom att ge användaren tydlig feedback.

#### &#x200B;6. Varför saknas en medietoken i beslutet om förhandsauktorisering? {#preauthorization-phase-faq6}

Förauktoriseringsbeslutet saknar en medietoken eftersom förauktoriseringsfasen inte får användas för att spela upp resurser, eftersom det är syftet med auktoriseringsfasen.

#### &#x200B;7. Kan auktoriseringsfasen hoppas över om det redan finns ett beslut om förhandsgodkännande? {#preauthorization-phase-faq7}

Nej.

Auktoriseringsfasen kan inte hoppas över även om ett beslut om förhandsgodkännande finns tillgängligt. Besluten om förhandsgodkännande är endast informativa och ger inte faktisk uppspelningsbehörighet. Förhandsauktoriseringsfasen är avsedd att ge tidig vägledning, men auktoriseringsfasen krävs fortfarande innan något innehåll spelas upp.

#### &#x200B;8. Vad är en resurs och vilka format stöds? {#preauthorization-phase-faq8}

Resursen är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

Resursen är en unik identifierare som avtalas med MVPD-program och som är associerad med ett innehåll som klientprogrammet kan direktuppspela.

Den unika identifieraren för resursen kan ha två format:

* Ett enkelt strängformat, till exempel en unik identifierare för en kanal (varumärke).
* Ett medie-RSS-format (MRSS) som innehåller ytterligare information som titel, gradering och metadata för föräldrakontroll.

Mer information finns i dokumentationen för [Skyddade resurser](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;9. För hur många resurser kan klientprogrammet få ett beslut om förauktorisering åt gången? {#preauthorization-phase-faq9}

Klientprogrammet kan få ett beslut om förhandsauktorisering för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 5, på grund av villkor som anges av distributörer av videoprogrammeringstjänster.

Det maximala antalet resurser kan visas och ändras efter att du har godkänt PDF-filer via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av organisationens administratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) .

+++

### Vanliga frågor om auktoriseringsfasen {#authorization-phase-faqs-general}

+++Vanliga frågor om auktoriseringsfasen

#### &#x200B;1. Vad är syftet med auktoriseringsfasen? {#authorization-phase-faq1}

Syftet med auktoriseringsfasen är att ge klientprogrammet möjlighet att spela upp resurser som användaren begär efter att ha verifierat sina rättigheter med MVPD.

#### &#x200B;2. Är auktoriseringsfasen obligatorisk? {#authorization-phase-faq2}

Auktoriseringsfasen är obligatorisk. Klientprogrammet kan inte hoppa över den här fasen om det vill spela upp resurser som användaren begär, eftersom det kräver verifiering med MVPD att användaren har rätt innan strömmen släpps.

#### &#x200B;3. Vad är ett auktoriseringsbeslut och hur länge gäller det? {#authorization-phase-faq3}

Auktoriseringen är en term som definieras i [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization) , medan beslutstermen också finns i [ordlistan](rest-api-v2-glossary.md#decision).

I auktoriseringsbeslutet lagras information om förfrågningsresultatet för MVPD-auktoriseringsprocessen som kan hämtas från slutpunkten för beslutsauktorisering.

Klientprogrammet kan använda auktoriseringsbeslutet som innehåller en medietoken för att spela upp resursströmmen om tv-providerns (auktoritativa) beslut skulle tillåta användaren att komma åt den.

Mer information finns i följande dokument:

* [Hämta API för auktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Auktoriseringsbeslutet gäller under en begränsad och kort tidsperiod som anges vid tidpunkten för utfärdandet, vilket anger hur lång tid det kommer att cachas av Adobe Pass Authentication innan det krävs att fråga MVPD igen.

Den här begränsade tidsramen som kallas auktorisering (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) kan visas och ändras via Adobe Pass [TVE Dashboard](rest-api-v2-glossary.md#tve-dashboard) av en av organisationens administratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

#### &#x200B;4. Ska klientprogrammet cachelagra auktoriseringsbesluten i en beständig lagring? {#authorization-phase-faq4}

Klientprogrammet behövs inte för att lagra auktoriseringsbeslut i beständig lagring.

#### &#x200B;5. Hur kan klientprogrammet avgöra varför ett auktoriseringsbeslut nekades? {#authorization-phase-faq5}

Klientprogrammet kan fastställa orsaken till ett beslut om nekad auktorisering genom att granska [felkoden och meddelandet ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) som ingår i svaret från slutpunkten för auktorisering av beslut. Dessa uppgifter ger insikt i varför auktoriseringsbegäran nekades, vilket kan bidra till att informera användaren eller utlösa nödvändig hantering i programmet.

Se till att alla återförsöksmetoder som implementeras för att hämta auktoriseringsbeslut inte resulterar i en oändlig slinga om auktoriseringsbeslutet nekas.

Överväg att begränsa antalet försök till ett rimligt antal och hantera nekanden på ett enkelt sätt genom att ge användaren tydlig feedback.

#### &#x200B;6. Vad är en medietoken och hur länge gäller den? {#authorization-phase-faq6}

Medietoken är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

Medietoken består av en signerad sträng som skickas i klartext och som kan hämtas från slutpunkten Beslutsauktorisering.

Mer information finns i dokumentationen för [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

Medietoken är giltig under en begränsad och kort tidsperiod som anges vid tidpunkten för utfärdandet, vilket anger tidsgränsen innan den måste verifieras och användas av klientprogrammet.

Klientprogrammet kan använda medietoken för att spela upp en resursström om TV-providerns (auktoritativa) beslut skulle tillåta användaren att komma åt den.

Mer information finns i följande dokument:

* [Hämta API för auktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. Ska klientprogrammet validera medietoken innan resursströmmen spelas upp? {#authorization-phase-faq7}

Ja.

Klientprogrammet måste validera medietoken innan uppspelningen av resursströmmen startar. Verifieringen bör utföras med [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Genom att verifiera `serializedToken` från `token` som returnerats hjälper klientprogrammet till att förhindra obehörig åtkomst, som strömrippning, och ser till att endast korrekt auktoriserade användare kan spela upp innehållet.

#### &#x200B;8. Ska klientprogrammet uppdatera en medietoken som har gått ut under uppspelningen? {#authorization-phase-faq8}

Nej.

Klientprogrammet behöver inte uppdatera en medietoken som har gått ut när strömmen spelas upp. Om medietoken upphör att gälla under uppspelning bör strömmen kunna fortsätta utan avbrott. Klienten måste dock begära ett nytt auktoriseringsbeslut - och få en ny medietoken - nästa gång användaren försöker spela upp en resurs.

#### &#x200B;9. Vad är syftet med varje tidsstämpelattribut i auktoriseringsbeslutet? {#authorization-phase-faq9}

Auktoriseringsbeslutet innehåller flera tidsstämpelattribut som ger ett viktigt sammanhang när det gäller giltighetsperioden för själva auktoriseringen och tillhörande medietoken. Dessa tidsstämplar har olika syften, beroende på om de avser auktoriseringsbeslutet eller medietoken.

**Tidsstämplar på beslutsnivå**

Dessa tidsstämplar beskriver giltighetsperioden för det övergripande beslutet om godkännande:

| Attribut | Beskrivning | Anteckningar |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Tiden i millisekunder då auktoriseringsbeslutet utfärdades. | Detta är början på giltighetsfönstret för auktoriseringen. |
| `notAfter` | Tiden i millisekunder när auktoriseringsbeslutet upphör att gälla. | [TTL (Time-to-Live)](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) för auktorisering avgör hur länge auktoriseringen ska vara giltig innan omauktorisering krävs. Denna TTL förhandlas fram med MVPD representanter. |

**Tidsstämplar på tokennivå**

Dessa tidsstämplar beskriver giltighetsperioden för den medietoken som är knuten till auktoriseringsbeslutet:

| Attribut | Beskrivning | Anteckningar |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Tiden i millisekunder när medietoken utfärdades. | Detta anger när token blir giltig för uppspelning. |
| `notAfter` | Tiden i millisekunder när medietoken upphör att gälla. | Medietoken har en avsiktligt kort livslängd (vanligtvis 7 minuter) för att minimera risken för missbruk och ta hänsyn till potentiella klockskillnader mellan den tokengenererande servern och den tokenverifierande servern. |

#### &#x200B;10. Vad är en resurs och vilka format stöds? {#authorization-phase-faq10}

Resursen är en term som definieras i dokumentationen för [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

Resursen är en unik identifierare som avtalas med MVPD-program och som är associerad med ett innehåll som klientprogrammet kan direktuppspela.

Den unika identifieraren för resursen kan ha två format:

* Ett enkelt strängformat, till exempel en unik identifierare för en kanal (varumärke).
* Ett medie-RSS-format (MRSS) som innehåller ytterligare information som titel, gradering och metadata för föräldrakontroll.

Mer information finns i dokumentationen för [Skyddade resurser](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;11. För hur många resurser kan klientprogrammet få ett auktoriseringsbeslut åt gången? {#authorization-phase-faq11}

Klientprogrammet kan få ett auktoriseringsbeslut för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 1, på grund av villkor som anges av distributörerna.

+++

### Vanliga frågor om utloggningsfasen {#logout-phase-faqs-general}

+++Vanliga frågor om utloggningsfasen

#### &#x200B;1. Vad är syftet med utloggningsfasen? {#logout-phase-faq1}

Syftet med utloggningsfasen är att ge klientprogrammet möjlighet att avsluta användarens autentiserade profil inom Adobe Pass-autentisering på användarens begäran.

#### &#x200B;2. Är utloggningsfasen obligatorisk? {#logout-phase-faq2}

Utloggningsfasen är obligatorisk. Klientprogrammet måste ge användaren möjlighet att logga ut.

+++

### Vanliga frågor om rubriker {#headers-faqs-general}

+++Vanliga frågor om rubriker

#### &#x200B;1. Hur beräknar man värdet för auktoriseringshuvudet? {#headers-faq1}

>[!IMPORTANT]
>
> Om klientprogrammet migrerar från REST API V1 till REST API V2, kan klientprogrammet fortsätta använda samma metod för att hämta `Bearer`-åtkomsttokenvärdet som tidigare.

Huvudet [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) innehåller åtkomsttoken `Bearer` som krävs för att klientprogrammet ska få åtkomst till Adobe Pass-skyddade API:er.

Huvudvärdet för auktorisering måste hämtas från Adobe Pass-autentisering under registreringsfasen.

Mer information finns i följande dokument:

* [Översikt över registrering av dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Hämta klientens autentiserings-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Hämta API för åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamiskt klientregistreringsflöde](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### &#x200B;2. Hur beräknas värdet för rubriken AP-Device-Identifier? {#headers-faq2}

>[!IMPORTANT]
>
> Om klientprogrammet migrerar från REST API V1 till REST API V2 kan klientprogrammet fortsätta att använda samma metod för att beräkna enhetsidentifierarvärdet som tidigare.

Huvudet för begäran [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) innehåller identifieraren för direktuppspelningsenheten som den skapades av klientprogrammet.

Huvuddokumentationen för [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) innehåller exempel på större plattformar för hur värdet beräknas, men klientprogrammet kan välja att använda en annan metod baserat på sin egen affärslogik och sina egna krav.

#### &#x200B;3. Hur beräknar man värdet för X-Device-Info-huvudet? {#headers-faq3}

>[!IMPORTANT]
>
> Om klientprogrammet migrerar från REST API V1 till REST API V2 kan klientprogrammet fortsätta att använda samma metod för att beräkna enhetsinformationsvärdet som tidigare.

Huvudet [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) innehåller klientinformationen (enhet, anslutning och program) som är relaterad till den faktiska direktuppspelningsenheten och används för att fastställa plattformsspecifika regler som MVPD-program kan tillämpa.

Huvuddokumentationen för [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) innehåller exempel på större plattformar för hur värdet beräknas, men klientprogrammet kan välja att använda en annan metod baserat på sin egen affärslogik och sina egna krav.

Om rubriken [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) saknas eller innehåller felaktiga värden kan begäran klassificeras som om den kommer från en `unknown`-plattform.

Detta kan leda till att begäran behandlas som osäker och omfattas av mer restriktiva regler, till exempel kortare autentiserings-TTL. Dessutom är vissa fält, till exempel direktuppspelningsenheten `connectionIp` och `connectionPort`, obligatoriska för funktioner som spektrumets [Home Base Authentication](/help/premium-workflow/hba-access/home-based-authentication.md).

Även när begäran kommer från en server för en enhets räkning måste rubrikvärdet [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) återspegla den faktiska informationen om direktuppspelningsenheten.

+++

### Vanliga frågor och svar {#misc-faqs-general}

+++Vanliga frågor och svar

#### &#x200B;1. Kan jag utforska REST API V2-begäranden och -svar och testa API:t? {#misc-faq1}

Ja.

Du kan utforska REST API V2 via vår dedikerade [Adobe Developer](https://developer.adobe.com/adobe-pass/)-webbplats. Adobe Developer webbplats ger obegränsad tillgång till

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Om du vill interagera med [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) måste du inkludera rubriken [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) med en `Bearer` åtkomsttoken som hämtas via [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Om du vill använda [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) krävs en programsats med REST API V2-scope. Mer information finns i dokumentet [Dynamic Client Registration (DCR) - frågor och svar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### &#x200B;2. Kan jag utforska REST API V2-begäranden och svar med ett API-utvecklingsverktyg med stöd för OpenAPI? {#misc-faq2}

Ja.

Du kan hämta OpenAPI-specifikationsfiler för [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) och [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) från webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass/).

Om du vill hämta OpenAPI-specifikationsfilerna klickar du på hämtningsknapparna och sparar följande filer på den lokala datorn:

* [DCR API JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

Du kan sedan importera dessa filer till det API-utvecklingsverktyg du föredrar för att utforska REST API V2-begäranden och -svar och testa API:t.

#### &#x200B;3. Kan jag fortfarande använda det befintliga API-testverktyget på https://sp.auth-staging.adobe.com/apitest/api.html? {#misc-faq3}

Nej.

Klientprogrammen som migrerar till REST API V2 bör använda det nya testverktyget som finns på https://developer.adobe.com/adobe-pass/. Adobe Developer webbplats ger obegränsad tillgång till

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Om du vill interagera med [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) måste du inkludera rubriken [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) med en `Bearer` åtkomsttoken som hämtas via [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Om du vill använda [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) krävs en programsats med REST API V2-scope. Mer information finns i dokumentet [Dynamic Client Registration (DCR) - frågor och svar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

## Vanliga frågor om migrering {#migration-faqs}

Fortsätt med det här avsnittet om du arbetar med ett program som behöver migrera ett befintligt program till REST API V2.

>[!MORELIKETHIS]
>
> * [Vanliga frågor om registrering av dynamisk klient (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Vanliga frågor om migrering {#general-migration-faqs}

+++Vanliga frågor om migrering

#### &#x200B;1. Måste jag köra ett nytt klientprogram som migrerats till REST API V2 för alla användare samtidigt? {#migration-faq1}

Nej.

Klientprogrammet behöver inte ha en ny version som integrerar REST API V2 för alla användare samtidigt.

Adobe Pass Authentication kommer att ha fortsatt stöd för äldre klientprogramversioner som integrerar REST API V1 eller SDK fram till slutet av 2025.

#### &#x200B;2. Måste jag köra ett nytt klientprogram som migrerats till REST API V2 för alla API:er och flöden samtidigt? {#migration-faq2}

Ja.

Klientprogrammet krävs för att lansera en ny version som integrerar REST API V2 i alla Adobe Pass Authentication API:er och flöden samtidigt.

Om autentiseringsflödet för den andra skärmen ska fungera måste klientprogrammet köra en ny version som integrerar REST API V2 för både [primära](rest-api-v2-glossary.md#primary-application) - och [sekundära](rest-api-v2-glossary.md#secondary-application) -program samtidigt.

Adobe Pass Authentication stöder inte hybridimplementeringar som integrerar både REST API V2 och REST API V1/SDK mellan API:er och flöden.

#### &#x200B;3. Bevaras användarautentiseringen vid uppdatering till ett nytt klientprogram som migrerats till REST API V2? {#migration-faq3}

Nej.

Användarautentiseringen som erhållits i äldre klientprogramversioner som integrerar REST API V1 eller SDK bevaras inte.

Därför måste användaren autentisera igen i det nya klientprogrammet som migrerats till REST API V2.

#### &#x200B;4. Är de förbättrade felkoderna aktiverade som standard i REST API V2? {#migration-faq4}

Ja.

Klientprogrammen som migrerar till REST API V2 drar automatiskt nytta av den här funktionen som standard, vilket ger mer detaljerad och exakt felinformation.

Mer information finns i dokumentationen för [Förbättrade felkoder](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

#### &#x200B;5. Kräver befintliga integreringar konfigurationsändringar vid migrering till REST API V2? {#migration-faq5}

Nej.

Klientprogrammen som migreras till REST API V2 kräver inga konfigurationsändringar för befintliga MVPD-integreringar. De kommer även fortsättningsvis att använda samma identifierare för befintliga [tjänstleverantörer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) och [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd).

Dessutom ändras inte de protokoll som används av Adobe Pass Authentication för att kommunicera med MVPD slutpunkter.

+++

### Migrering från REST API V1 till REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Fortsätt med det här underavsnittet om du arbetar med ett program som behöver migrera från REST API V1 till REST API V2.

#### Vanliga frågor om registreringsfasen {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Vanliga frågor om registreringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API:er som krävs för registreringsfasen? {#registration-phase-v1-to-v2-faq1}

I övergången från REST API V1 till REST API V2 finns det inga omfattande förändringar i registreringsfasen.

Klientprogrammet kan fortsätta att använda samma flöde för att registrera sig mot Adobe Pass-autentisering via processen [Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) och få en åtkomsttoken.

Mer information finns i följande dokument:

* [Översikt över registrering av dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Hämta klientens autentiserings-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Hämta API för åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Dynamiskt klientregistreringsflöde](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### Vanliga frågor om konfigurationsfasen {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Vanliga frågor om konfigurationsfasen

##### &#x200B;1. Vilka är de högnivå-API-migreringar som krävs för konfigurationsfasen? {#configuration-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Vanliga frågor om autentiseringsfasen {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Vanliga frågor om autentiseringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API som krävs för autentiseringsfasen? {#authentication-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta registreringskod (autentiseringskod) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera registreringskod (autentiseringskod) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Återuppta (MVPD) autentisering på andra skärmen (program) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiera (MVPD) autentisering | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [GET <br/> /api/v1/checkauthn (första skärmen)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (andra skärmen)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta token för användarautentisering (profil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Vanliga frågor om förauktoriseringsfasen {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Vanliga frågor om förauktoriseringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API:er som krävs för fasen för förhandsauktorisering? {#preauthorization-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [GET <br/> /api/v1/preauthorized (first screen)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorized (second screen)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preAuthze/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Vanliga frågor om auktoriseringsfasen {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Vanliga frågor om auktoriseringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API som krävs för auktoriseringsfasen? {#authorization-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD) auktorisering | [GET <br/> /api/v1/authorized](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD) auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Hämta auktoriseringstoken (auktoriseringsbeslut) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD) auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Hämta kort auktoriseringstoken (medietoken) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD) auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Vanliga frågor om utloggningsfasen {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Vanliga frågor om utloggningsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API:er som krävs för utloggningsfasen? {#logout-phase-v1-to-v2-faq1}

I migreringen från REST API V1 till REST API V2 finns det stora förändringar som kan övervägas i följande tabell:

| Omfång | REST API V1 | REST API V2 | Observationer |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera utloggning | [GET <br/> /api/v1/logOut](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migrering från SDK till REST API V2 {#migration-sdk-to-rest-api-v2}

Fortsätt med det här underavsnittet om du arbetar med ett program som behöver migreras från SDK till REST API V2.

#### Vanliga frågor om registreringsfasen {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Vanliga frågor om registreringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API:er som krävs för registreringsfasen? {#registration-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

###### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fullständig DCR (Dynamic Client Registration) | Tillhandahåller programsats till konstruktorn | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Registreringsflöde för dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### Vanliga frågor om konfigurationsfasen {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Vanliga frågor om konfigurationsfasen

##### &#x200B;1. Vilka är de högnivå-API-migreringar som krävs för konfigurationsfasen? {#configuration-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

###### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Hämta en lista över MVPD-filer med aktiv integrering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Vanliga frågor om autentiseringsfasen {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++Vanliga frågor om autentiseringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API som krävs för autentiseringsfasen? {#authentication-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

###### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta registreringskod (autentiseringskod) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera registreringskod (autentiseringskod) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Återuppta (MVPD) autentisering på andra skärmen (program) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta registreringskod (autentiseringskod) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera registreringskod (autentiseringskod) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Återuppta (MVPD) autentisering på andra skärmen (program) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Initiera (MVPD) autentisering | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande autentiseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Grundläggande autentiseringsflöde som har utförts i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Kontrollera autentiseringsstatus för användare | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Hämta information om användarmetadata | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Använd något av följande: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Klientprogrammet kan använda svaret från dessa API:er för flera syften samtidigt: <br/> <ul><li>Kontrollera autentiseringsstatus för användare</li><li>Hämta användarprofil</li><li>Hämta information om användarmetadata</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande profiler utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Grundläggande profiler utförs i det sekundära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Vanliga frågor om förauktoriseringsfasen {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Vanliga frågor om förauktoriseringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API:er som krävs för fasen för förhandsauktorisering? {#preauthorization-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

###### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorized](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preAuthze/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorized](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preAuthze/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorized](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decision/preAuthze/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Omfång | SDK | REST API V2 | Observationer |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta förauktoriserade resurser (beslut om förauktorisering) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/Decision/preAuthze/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande förauktoriseringsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Vanliga frågor om auktoriseringsfasen {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Vanliga frågor om auktoriseringsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API som krävs för auktoriseringsfasen? {#authorization-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

###### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD) auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD) auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD) auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hämta kort auktoriseringstoken (medietoken) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decision/authorized/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | Klientprogrammet kan använda svaret från detta API för flera syften samtidigt: <br/> <ul><li>Initiera (MVPD) auktorisering</li><li>Hämta auktoriseringsbeslut</li><li>Hämta kort medietoken</li></ul> <br/> Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande auktoriseringsflöde har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Vanliga frågor om utloggningsfasen {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++Vanliga frågor om utloggningsfasen

##### &#x200B;1. Vilka är de högnivåmigreringar av API:er som krävs för utloggningsfasen? {#logout-phase-sdk-to-v2-faq1}

I migreringen från SDK:er till REST API V2 finns det stora förändringar som kan övervägas i följande tabeller:

###### AccessEnabler JavaScript SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Omfång | SDK | REST API V2 | Observationer |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initiera utloggning | [AccessEnabler.logOut](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/log](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Mer information finns i följande dokument: <br/> <ul><li>[Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
