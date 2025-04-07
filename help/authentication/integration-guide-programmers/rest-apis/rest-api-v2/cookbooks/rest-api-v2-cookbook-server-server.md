---
title: REST API V2 Cookbook (Server-to-Server)
description: REST API V2 Cookbook (Server-to-Server)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2510'
ht-degree: 0%

---

# REST API V2 Cookbook (Server-to-Server) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Dokumentet är avsett för utvecklare som integrerar [Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) i sina direktuppspelningsprogram med en Server-to-Server-arkitektur (S2S).

## Förutsättningar {#prerequisites}

Termer och definitioner finns i [REST API V2 Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) -dokumentationen.

Information om obligatoriska krav och rekommenderade metoder finns i dokumentationen för [REST API V2 Checklist](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) .

Vanliga frågor och svar finns i [REST API V2 FAQs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) -dokumentationen.

### Komponenter {#components}

Innan du börjar bör du se till att du är insatt i följande komponenter och termer som används i arbetsflödet:

| Typ | Komponent | Beskrivning |
|---------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe Infrastructure | Adobe Pass Service | Integreras med MVPD IdP- och AuthZ-tjänsten för att ge autentiserings- och auktoriseringsbeslut. |
| Programmeringsinfrastruktur | Programmerartjänst | Ansluter direktuppspelningsenheten till Adobe Pass-tjänsten för att erhålla autentiserade profiler och auktoriseringsbeslut. |
| MVPD Infrastructure | MVPD IdP-tjänst | MVPD-slutpunkt som ansvarar för inloggningsbaserad autentisering validerar användarens identitet. |
|                           | MVPD AuthZ-tjänst | MVPD-slutpunkt som bestämmer auktoriseringsbeslut baserat på användarabonnemang, föräldrakontroll och andra behörighetsregler. |
| Strömmande enhet | Strömmande app | Programmerarens program som körs på användarens direktuppspelningsenhet och som spelar upp autentiserat videoinnehåll. |
|                           | (Valfritt) AuthN-modul | Om direktuppspelningsenheten har en användaragent (t.ex. en webbläsare) hanterar AuthN-modulen användarautentisering på MVPD IdP. |
| (Valfritt) AuthN-enhet | AuthN-app | Om strömningsenheten saknar en användaragent (t.ex. webbläsare) är AuthN-programmet ett webbprogram som nås från en separat enhet via en webbläsare. |

### Krav {#requirements}

I Server-to-Server-implementeringar (S2S) måste Streaming App och Programmer Service upprätta ett protokoll som gör det möjligt för Programmer-tjänsten att:

* Kommunicera med Adobe Pass Service för Streaming App.

* Samla in och skicka en unik enhets-ID för direktuppspelningsenheten enligt vad som krävs i huvudet [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).

* Samla in och skicka korrekt information om direktuppspelningsenhet, inklusive källporten och enhetsspecifik information, enligt vad som krävs i rubriken [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) .

* Samla in och skicka IP-adressen för direktuppspelningsenheten enligt vad som krävs i huvudet X-Forwarded-For.

* Lagra parametrar som enhets-ID, klient-ID och klienthemlighet säkert i antingen Streaming App eller Programmer Service.

* Formatera och skicka data i enlighet med MVPD-program och integrerade program, inklusive enhets-IP, källport, enhetsspecifik information, MRSS och valfria identifierare som ECID.

* Underhåll och hantera säkert certifikat som delas med Adobe för krypterad överföring av användarmetadata.

* Respektera autentiseringsprofilen och giltigheten för auktoriseringsbeslut vid cachelagring, och se till att autentiserings- och auktoriseringstillstånden ogiltigförklaras när de meddelas.

* Returnera auktoriseringsbeslut och relevanta instruktioner till Streaming App.

### Miljö {#environments}

Kontrollera att du har minst två miljöer innan du går in i arbetsflödet: Produktion och Förproduktion.

**Produktion**

Produktionsmiljön måste vara mycket tillgänglig och skalad på ett lämpligt sätt för att hantera stora eller oväntade trafiktoppar, t.ex. sådana som orsakas av live-sportevenemang eller nyheter.

* Adobe Pass-tjänsten fungerar över flera geografiskt spridda datacenter i hela USA för att optimera prestanda och minimera latenstiden.

   * Programmeringstjänsten bör anta en liknande infrastrukturstrategi som säkerställer svarstider med låg fördröjning från Adobe Pass.

* Programmeraren måste tillhandahålla det offentliga IP-intervallet för sin produktionsmiljö.

   * Dessa IP-adresser läggs till i ett tillåtelselista i Adobe Pass infrastruktur.

* Programmerartjänsten måste begränsa DNS-cachning till högst 30 sekunder för att tillåta dynamisk omdirigering om Adobe behöver omdirigera trafik på grund av att ett datacenter inte är tillgängligt.

**Förproduktion**

Mellanlagringsmiljön kan vara minimal men bör spegla produktionen genom att inkludera alla kritiska systemkomponenter och affärslogik.

* Det måste vara möjligt för testversioner innan driftsättning till produktion.

* Det bör i sin tur i sin funktion vara likartat med produktionen, vilket möjliggör realistisk testning.

* I idealfallet bör testmiljön anslutas till Adobe Pass testmiljöer för att

   * Tillåt programmerare att testa mot Adobe infrastruktur.

   * Gör det möjligt för Adobe att vid behov hjälpa till med testning och felsökning.

## Arbetsflöde {#workflow}

Utför stegen nedan enligt bilden nedan.

![REST API V2 Cookbook (Server-to-Server)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

*REST API V2 Cookbook (Server-to-Server)*

## A. Registreringsfas {#registration-phase}

Syftet med registreringsfasen är att registrera direktuppspelningsprogrammet mot Adobe Pass-autentisering genom DCR-processen (Dynamic Client Registration).

DCR-processen (Dynamic Client Registration) kräver att direktuppspelningsprogrammet hämtar ett par klientautentiseringsuppgifter och hämtar en åtkomsttoken som slutmål för registreringsfasen.

Registreringsfasen är obligatorisk, men direktuppspelningsprogrammet kan hoppa över den här fasen om det har ett cachelagrat par med klientautentiseringsuppgifter och en åtkomsttoken som fortfarande är giltig.

+++Relaterade artiklar

API:

* [Hämta klientautentiseringsuppgifter](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Hämta åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

Flöden:

* [Dynamiskt klientregistreringsflöde](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Frågor och svar:

* [Vanliga frågor om registreringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Steg 1: Registrera ditt program {#step-1-register-your-application}

* Hämta klientautentiseringsuppgifter: Programmeringstjänsten hämtar klientautentiseringsuppgifter genom att anropa slutpunkten [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

   * Programmerartjänsten eller programmerarappen måste lagra klientens autentiseringsuppgifter och använda dem i oändlighet när en åtkomsttoken behöver hämtas.


* Hämta åtkomsttoken: Programmerartjänsten hämtar åtkomsttoken genom att anropa slutpunkten [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

   * Programmerartjänsten eller programmerarappen måste lagra och använda åtkomsttoken tills den upphör att gälla, sedan ta bort den och skaffa en ny.

## B. Autentiseringsfas {#authentication-phase}

Syftet med autentiseringsfasen är att ge direktuppspelningsprogrammet möjlighet att verifiera användarens identitet och få information om användarens metadata.

Autentiseringsfasen fungerar som ett nödvändigt steg för förauktoriseringsfasen eller auktoriseringsfasen när direktuppspelningsprogrammet behöver spela upp innehåll.

+++Relaterade artiklar

API:er

* [Skapa autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Återuppta autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Hämta autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Utför autentisering i användaragent](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Hämta profiler](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Hämta profil för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Hämta profil för specifik kod](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Flöden

* [Grundläggande autentiseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundläggande autentiseringsflöde som utförs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Vanliga frågor

* [Vanliga frågor om autentiseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Steg 2: Kontrollera om det finns befintliga autentiserade profiler {#step-2-check-for-existing-authenticated-profiles}

* **Hämta profiler:** Programmerartjänsten söker efter befintliga profiler för direktuppspelningsappen genom att anropa slutpunkten [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) .


* **Scenario 1:** Det finns befintliga profiler. Programmeringstjänsten kan fortsätta till [förauktoriseringsfasen](#preauthorization-phase) eller [auktoriseringsfasen](#authorization-phase).


* **Scenario 2:** Det finns inga befintliga profiler. Programmeringstjänsten kan gå vidare till nästa steg för att [autentisera användaren](#step-3-authenticate-the-user).


* **Scenario 3:** Det finns inga befintliga profiler. Programmeringstjänsten kan fortsätta att ge användaren tillfällig åtkomst via funktionen [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md).

   * Det här scenariot ligger utanför det här dokumentets omfång. Mer information finns i dokumentationen för [Tillfälliga åtkomstflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

### Steg 3: Autentisera användaren {#step-3-authenticate-the-user}

* **Hämta konfiguration:** Programmeringstjänsten hämtar listan över tillgängliga MVPD-filer genom att anropa slutpunkten [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) .

   * Programmeringstjänsten kan implementera en anpassad filtreringsmekanism för att förfina listan över MVPD-program från konfigurationssvaret, så att Streaming App endast visar de avsedda leverantörerna medan andra döljs (t.ex. MVPD-program som håller på att utvecklas, testar MVPD-program, TempPass). Detta garanterar att användarna får ett välstrukturerat urval när de väljer sin tv-leverantör.


* **Skapa autentiseringssession:** Programmeringstjänsten initierar en autentiseringssession genom att anropa slutpunkten [**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

   * Programmerartjänsten måste returnera `code` och `url` till direktuppspelningsappen.


* **Scenario 1:** Direktuppspelningsappen kan öppna en webbläsare eller webbvy och måste därför läsa in autentiseringen `url`.

   * Användaren anger sitt användarnamn och lösenord på inloggningssidan för MVPD. När autentiseringen är klar visas en sida för slutförd omdirigering.


* **Scenario 2:** Direktuppspelningsappen kan inte öppna en webbläsare och måste därför visa autentiseringen `code`. Ett separat webbprogram krävs för att uppmana användaren att ange `code`, konstruera autentiseringen `url` och öppna: [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * Användaren anger sitt användarnamn och lösenord på inloggningssidan för MVPD. När autentiseringen är klar visas en sida för slutförd omdirigering.

### Steg 4: Kontrollera om det finns autentiserade profiler {#step-4-check-for-authenticated-profiles}

* **Hämta profil för specifik kod:** Programmeringstjänsten måste implementera en avsökningsmekanism med `code` för att kontrollera om profilen genererades och sparades genom att anropa slutpunkten [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).

   * Programmerartjänsten måste **starta avsökningsfunktionen** på följande villkor:

      * **Autentisering som utförs i det primära (skärm) programmet:** Programmerartjänsten ska starta avsökningen när användaren kommer till den sista målsidan, efter att webbläsarkomponenten har läst in den URL som angetts för parametern `redirectUrl` i [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) -slutpunktsbegäran.

      * **Autentisering som utförs i ett sekundärt (skärm) program:** Programmerartjänstprogrammet bör börja avfråga så snart användaren initierar autentiseringsprocessen - direkt efter att ha tagit emot [Sessioner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) -slutpunktssvaret och visar autentiseringskoden för användaren.

   * Programmerartjänsten måste **stoppa avsökningsfunktionen** under följande villkor:

      * **Autentiseringen är klar:** Användarens profilinformation har hämtats och autentiseringsstatusen har bekräftats. I nuläget behövs ingen avsökning längre.

      * **Autentiseringssession och förfallodatum för kod:** Autentiseringssessionen och koden förfaller, vilket anges av tidsstämpeln `notAfter` (t.ex. 30 minuter) i slutpunktssvaret för [sessioner](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Om detta inträffar måste användaren starta om autentiseringsprocessen och avsökningen med den tidigare autentiseringskoden ska stoppas omedelbart.

      * **Ny autentiseringskod har genererats:** Om användaren begär en ny autentiseringskod på den primära (skärm) enheten är den befintliga sessionen inte längre giltig och avsökningen med den tidigare autentiseringskoden bör stoppas omedelbart.

   * Programmerartjänsten måste **konfigurera avsökningsfrekvensen** under följande förhållanden:

      * **Autentisering som utförs i det primära (skärm) programmet:** Programmeringstjänsten bör avsöka var 3-5:e sekund eller mer.

      * **Autentisering som utförs i ett sekundärt (skärm) program:** Programmeringstjänsten bör avsöka var 3-5:e sekund eller mer.

   * Programmerartjänsten bör cachelagra delar av användarens profilinformation i en beständig lagringsplats för att undvika onödiga begäranden och förbättra användarupplevelsen.

## C. (Valfritt) Förhandsauktoriseringsfas {#preauthorization-phase}

Syftet med förauktoriseringsfasen är att ge direktuppspelningsprogrammet möjlighet att presentera en delmängd av resurser från sin katalog som användaren skulle ha rätt till.

Fas för förhandsauktorisering kan förbättra användarupplevelsen när användaren öppnar direktuppspelningsprogrammet för första gången eller navigerar till ett nytt avsnitt.

Förhandsauktoriseringsfasen är inte obligatorisk, men direktuppspelningsprogrammet kan hoppa över den här fasen om det vill visa en katalog med resurser utan att först filtrera dem baserat på användarens tillstånd.

+++Relaterade artiklar

API:er

* [Hämta förauktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Flöden

* [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

Vanliga frågor

* [Vanliga frågor om förauktoriseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Steg 5: Sök efter förauktoriserade resurser {#step-5-check-for-preauthorized-resources}

* **Hämta beslut om förhandsauktorisering:** Programmeringstjänsten hämtar beslut om förhandsauktorisering för en lista med resurser genom att anropa slutpunkten [**/api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) .

   * Programmeringstjänsten måste skicka listan över tillstånd och neka förauktoriseringsbeslut till Streaming App.

   * Programmerartjänsten behövs inte för att lagra förauktoriseringsbeslut i beständig lagring. Vi rekommenderar dock att du cachelagrar tillståndsbeslut i minnet för att förbättra användarupplevelsen. På så sätt undviker du onödiga krav på resurser som redan har förauktoriserats, vilket minskar latensen och förbättrar prestandan.

   * Programmerartjänsten kan fastställa orsaken till ett beslut om förauktorisering som nekats genom att granska [felkoden och meddelandet](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) som ingår i svaret från slutpunkten för förauktorisering av beslut. Dessa detaljer ger insikt i den specifika anledningen till att förauktoriseringsbegäran nekades, vilket kan hjälpa användaren att informera om användarupplevelsen eller utlösa nödvändig hantering i programmet. Se till att alla återförsöksmetoder som implementeras för att hämta beslut om förauktorisering inte resulterar i en oändlig slinga om beslutet om förauktorisering nekas. Överväg att begränsa antalet försök till ett rimligt antal och hantera nekanden på ett enkelt sätt genom att ge användaren tydlig feedback.

   * Programmeringstjänsten kan få ett beslut om förhandsbehörighet för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 5, på grund av villkor som anges av distributörer av videoprogrammeringstjänster. Det maximala antalet resurser kan visas och ändras efter att du har godkänt PDF-filer via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av organisationens administratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

## D. Auktoriseringsfas {#authorization-phase}

Syftet med auktoriseringsfasen är att ge direktuppspelningsprogrammet möjlighet att spela upp resurser som användaren begär efter att ha verifierat sina rättigheter med MVPD.

Auktoriseringsfasen är obligatorisk, men direktuppspelningsprogrammet kan inte hoppa över den här fasen om det vill spela upp resurser som användaren begär, eftersom det kräver verifiering med MVPD att användaren har rätt innan direktuppspelningen släpps.

+++Relaterade artiklar

API:er

* [Hämta auktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Flöden

* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Vanliga frågor

* [Vanliga frågor om auktoriseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Steg 6: Kontrollera om det finns auktoriserade resurser {#step-6-check-for-authorized-resources}

* **Hämta auktoriseringsbeslut:** Programmerartjänsten hämtar auktoriseringsbeslut för en specifik resurs som skickas av direktuppspelningsappen genom att anropa slutpunkten [**/api/v2/{serviceProvider}/Decision/authorized/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).

   * Programmerartjänsten behövs inte för att lagra auktoriseringsbeslut i beständig lagring.

   * Programmerartjänsten kan fastställa orsaken till ett beslut om nekad auktorisering genom att granska [felkoden och meddelandet](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) som ingår i svaret från slutpunkten för auktorisering av beslut. Dessa uppgifter ger insikt i varför auktoriseringsbegäran nekades, vilket kan bidra till att informera användaren eller utlösa nödvändig hantering i Streaming App. Se till att alla återförsöksmetoder som implementeras för att hämta auktoriseringsbeslut inte resulterar i en oändlig slinga om auktoriseringsbeslutet nekas. Överväg att begränsa antalet försök till ett rimligt antal och hantera nekanden på ett enkelt sätt genom att ge användaren tydlig feedback.

   * Programmeringstjänsten kan utvärdera andra affärsregler och returnera ett lämpligt auktoriseringsbeslut till Streaming App.

   * Programmerartjänsten behöver inte uppdatera en medietoken som har upphört att gälla medan strömmen spelas upp. Om medietoken upphör att gälla under uppspelning bör strömmen kunna fortsätta utan avbrott. Klienten måste dock begära ett nytt auktoriseringsbeslut - och få en ny medietoken - nästa gång användaren försöker spela upp en resurs.

   * Programmeringstjänsten kan få ett auktoriseringsbeslut för ett begränsat antal resurser i en enda API-begäran, vanligtvis upp till 1, på grund av villkor som anges av distributörer av videoprogrammeringstjänster.

## E. Utloggningsfas {#logout-phase}

Syftet med utloggningsfasen är att ge direktuppspelningsprogrammet möjlighet att avsluta användarens autentiserade profil inom Adobe Pass-autentisering på användarens begäran.

Utloggningsfasen är obligatorisk. Strömningsprogrammet måste ge användaren möjlighet att logga ut.

+++Relaterade artiklar

API:er

* [Initiera utloggning för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

Flöden

* [Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

Vanliga frågor

* [Vanliga frågor om utloggningsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Steg 7: Logga ut {#step-7-logout}

* Initiera Adobe Pass-utloggning: Programmeringstjänsten initierar utloggningsflödet enligt Streaming App-programmets begäran genom att anropa slutpunkten [/api/v2/{serviceProvider}/logout/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).

   * Programmerartjänsten kan rensa all information som lagras om den autentiserade användaren.

   * Programmeringstjänsten måste följa instruktionerna i attributen `actionName` och `actionType` för utloggningsslutpunktssvaret för att säkerställa att utloggningsprocessen slutförs korrekt.

      * Om attributet `actionType` i svaret är inställt på &quot;interaktiv&quot; måste programmeringstjänsten returnera attributvärdet `url` till direktuppspelningsappen.

         * **Scenario 1:** Direktuppspelningsappen kan öppna en webbläsare eller webbvy och måste därför läsa in utloggningen `url`.

         * **Scenario 2:** Direktuppspelningsappen kan inte öppna en webbläsare, och därför kan utloggningsprocessen stoppas eftersom MVPD-sessionen inte sparades i en webbläsarcache för direktuppspelningsenheten.
