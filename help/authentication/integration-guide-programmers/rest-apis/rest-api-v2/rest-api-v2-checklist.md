---
title: REST API V2-checklista
description: REST API V2-checklista
source-git-commit: f0001d86f595040f4be74f357c95bd2919dadf15
workflow-type: tm+mt
source-wordcount: '2535'
ht-degree: 0%

---

# REST API V2-checklista {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Det här dokumentet samlar på ett ställe de obligatoriska kraven och rekommenderade metoderna för programmerare som implementerar klientprogram som använder Adobe Pass-autentisering [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Följande dokument måste ingå i ditt acceptanskriterier när du implementerar REST API V2 och måste användas som en checklista för att säkerställa att alla nödvändiga åtgärder har vidtagits för att uppnå en lyckad integrering.

## Obligatoriska krav {#mandatory-requirements}

### 1. Registreringsfas {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registrerad programomfång</i></td>
      <td>Använd ett registrerat program med omfattningen REST API v2.</td>
      <td>Risker som utlöser HTTP 401 "Obehöriga" felsvar, överbelastar systemresurser och ökar fördröjningen.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>Cachelagring av klientautentiseringsuppgifter</i></td>
      <td>Lagra klientens autentiseringsuppgifter i beständig lagring och återanvänd dem för varje åtkomsttokenbegäran.</td>
      <td>Riskerar att autentiseringen går förlorad när klientens autentiseringsuppgifter genereras om.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Åtkomsttoken-cachelagring</i></td>
      <td>Lagra åtkomsttoken i beständig lagring och återanvänd dem tills de upphör att gälla - begär inte en ny token för varje REST API v2-anrop.</td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
</table>

### 2. Konfigurationsfas {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfigurationshämtning</i></td>
      <td>Hämta bara konfigurationssvaret när du begär att användaren ska välja sin MVPD (TV-leverantör) före autentiseringsfasen.<br/><br/>Du behöver inte hämta konfigurationssvaret när:<ul><li>Användaren är redan autentiserad.</li><li>Användaren erbjuds tillfällig åtkomst.</li><li>Användarautentiseringen har upphört att gälla, men användaren kan uppmanas att bekräfta att han/hon fortfarande är prenumerant på den tidigare valda MVPD.</li></ul></td>
      <td>Risker överbelastar systemresurser och ökar fördröjningen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Cachelagring av tv-leverantör</i></td>
      <td>Lagra användarens val av Pay TV-leverantör (MVPD) i beständigt lagringsutrymme och använd detta i alla efterföljande faser:<ul><li>I konfigurationssvarsarkivet har användaren valt MVPD "id".</li><li>I konfigurationssvarsarkivet har användaren valt MVPD "displayName".</li><li>I konfigurationssvarsarkivet har användaren valt MVPD "logoUrl".</li></ul></td>
      <td>Risker överbelastar systemresurser och ökar fördröjningen.</td>
   </tr>
</table>

### 3. Autentiseringsfas {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Avsökningsmekanismen börjar</i></td>
      <td>Starta avsökningsfunktionen under följande villkor:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autentisering utförd i det primära (skärm) programmet</a></b><ul><li>Det primära (direktuppspelande) programmet ska starta avsökningen när användaren kommer till den sista målsidan, efter att webbläsarkomponenten har läst in den URL som angetts för parametern "redirectUrl" i Sessions-slutpunktsbegäran.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autentisering utförd i ett sekundärt (skärm) program</a></b><ul><li>Det primära (direktuppspelande) programmet bör starta avsökningen så snart användaren initierar autentiseringsprocessen, direkt efter att ha tagit emot Sessionernas slutpunktssvar och visat autentiseringskoden för användaren.</li></ul></td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Avsökningsmekanismen stoppas</i></td>
      <td>Stoppa avsökningsmekanismen under följande villkor:<br/><br/><b>Autentisering slutförd</b><ul><li>Användarens profilinformation har hämtats, vilket bekräftar autentiseringsstatusen och därför behövs ingen avsökning längre.</li></ul><br/><b>Autentiseringssession och kodutgång</b><ul><li>Autentiseringssessionen och koden upphör att gälla, användaren måste starta om autentiseringsprocessen och avsökningen med den tidigare autentiseringskoden bör stoppas omedelbart.</li></ul><br/><b>Ny autentiseringskod genererad</b><ul><li>Om användaren begär en ny autentiseringskod ogiltigförklaras den befintliga sessionen och avsökningen med den tidigare autentiseringskoden stoppas omedelbart.</li></ul></td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfigurering av avsökningsmekanism</i></td>
      <td>Konfigurera avsökningsmekanismens frekvens under följande villkor:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autentisering utförd i det primära (skärm) programmet</a></b><ul><li>Det primära programmet (direktuppspelning) ska avsöka var 3:e till 5:e sekund.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autentisering utförd i ett sekundärt (skärm) program</a></b><ul><li>Det primära programmet (direktuppspelning) ska avsöka var 3:e till 5:e sekund.</li></ul></td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Cachelagring av profiler</i></td>
      <td>Lagra delar av användarens profilinformation i beständig lagring för att förbättra prestandan och minimera onödiga REST API v2-anrop.<br/><br/>Cachelagring bör fokusera på följande profilsvarsfält:<br/><br/><b>mvpd</b><ul><li>Klientprogrammet kan använda detta för att hålla reda på användarens valda TV-leverantör och fortsätta att använda det under förauktoriserings- eller auktoriseringsfaserna.</li><li>När den aktuella användarprofilen går ut kan klientprogrammet använda det sparade MVPD-valet och be användaren bekräfta.</li></ul><br/><b>attribut</b><ul><li>Används för att anpassa användarupplevelsen baserat på olika användarmetadatanycklar (t.ex. zip, maxRating osv.).</li><li>Användarmetadata blir tillgängliga när autentiseringsflödet har slutförts, och därför behöver klientprogrammet inte fråga en separat slutpunkt för att hämta användarmetadatainformation, eftersom den redan ingår i profilinformationen.</li><li>Vissa metadataattribut kan uppdateras under auktoriseringsfasen beroende på MVPD (t.ex. charta) och det specifika metadataattributet (t.ex. houseID). Därför kan klientprogrammet behöva fråga Profiles-API:erna igen efter auktoriseringen för att hämta de senaste användarens metadata.</li></ul></td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
</table>

### 4. (Valfritt) Förhandsauktoriseringsfas {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Återhämtning av förauktoriseringsbeslut</i></td>
      <td>Använd förauktoriseringsbeslut för innehållsfiltrering och aldrig för uppspelningsbeslut.</td>
      <td>Risker som bryter mot avtal mellan Programmer, MVPD och Adobe.<br/><br/>Risker genom att våra övervaknings- och varningssystem kringgås.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Återförsök av förauktoriseringsbeslut</i></td>
      <td>Hantera de <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">förbättrade felkoderna</a> korrekt och använd åtgärdsfältet för att avgöra vilka reparationssteg som krävs.<br/><br/>Endast ett begränsat antal utökade felkoder kräver ett nytt försök, medan de flesta kräver alternativa upplösningar som anges i åtgärdsfältet.<br/><br/>Kontrollera att alla återförsöksmetoder som implementeras för att hämta beslut om förauktorisering inte resulterar i en oändlig loop och att den begränsar återförsök till ett rimligt tal (dvs. 2-3).</td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Cachelagring av förauktoriseringsbeslut</i></td>
      <td>Cachelagra lyckade tillståndsbeslut i minnet för att förbättra prestanda och minimera onödiga REST API v2-anrop, eftersom prenumerationsuppdateringar när programmet körs är ovanliga.</td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
</table>

### 5. Auktoriseringsfas {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Återhämtning av auktoriseringsbeslut</i></td>
      <td>Inhämta auktoriseringsbeslut före uppspelning - oavsett om ett förhandsauktoriseringsbeslut finns.<br/><br/>Tillåt att strömmar fortsätter utan avbrott även om medietoken förfaller under uppspelningen och begär ett nytt auktoriseringsbeslut som innehåller en (ny) medietoken när användaren gör sin nästa uppspelningsbegäran, oavsett om det är för samma eller en annan resurs.<br/><br/>Liveströmmar som körs under längre perioder kan välja att begära ett nytt auktoriseringsbeslut efter videoåtgärder som att pausa innehåll, initiera kommersiella avbrott eller ändra konfigurationer på tillgångsnivå när MRSS-strömmen ändras.</td>
      <td>Risker som bryter mot avtal mellan Programmer, MVPD och Adobe.<br/><br/>Risker genom att våra övervaknings- och varningssystem kringgås.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Återförsök av auktoriseringsbeslut</i></td>
      <td>Hantera de <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">förbättrade felkoderna</a> korrekt och använd åtgärdsfältet för att avgöra vilka reparationssteg som krävs.<br/><br/>Endast ett begränsat antal utökade felkoder kräver ett nytt försök, medan de flesta kräver alternativa upplösningar som anges i åtgärdsfältet.<br/><br/>Kontrollera att alla återförsöksmetoder som implementeras för att hämta auktoriseringsbeslut inte resulterar i en oändlig loop och att den begränsar återförsök till ett rimligt tal (dvs. 2-3).</td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".</td>
   </tr>
</table>

### 6. Utloggningsfas {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Utloggningssupport</i></td>
      <td>Implementera utloggnings-API:t så att användare manuellt kan logga ut, avsluta sin autentiserade profil och följa det REST API v2-åtgärdsnamn som anges för varje borttagen profil:<ul><li>För MVPD-program som stöder en utloggningsslutpunkt måste klientprogrammet navigera till angiven url i en användaragent.</li><li>För profiler av typen"appleSSO" måste klientprogrammet hjälpa användaren att även logga ut från partnernivån (Apple systeminställningar).</li></ul></td>
      <td>Risker med att klientprogrammet inte fungerar som det ska på grund av att det saknas stöd på klientprogramsidan.</td>
   </tr>
</table>

### 7. Parametrar och rubriker {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Skicka auktoriseringshuvud</i></td>
      <td>Skicka huvudet <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> för varje REST API v2-begäran.</td>
      <td>Risker som utlöser HTTP 401 "Obehöriga" felsvar, överbelastar systemresurser och ökar fördröjningen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Skicka AP-Device-Identifier-huvud</i></td>
      <td>Skicka huvudet <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> för varje REST API v2-begäran.<br/><br/>Även om begäran kommer från en server för en enhets räkning måste rubrikvärdet för AP-Device-Identifier återspegla den faktiska identifieraren för direktuppspelningsenheten.</td>
      <td>Risker som utlöser HTTP 400-felsvar om felaktig begäran, överlagrar systemresurser och ökar fördröjningen.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Skicka X-Device-Info Header</i></td>
      <td>Skicka rubriken <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> för varje REST API v2-begäran.<br/><br/>Även när begäran kommer från en server för en enhets räkning måste X-Device-Info-rubrikvärdet återspegla den faktiska informationen om direktuppspelningsenheten.</td>
      <td>Risker som klassificeras som att de härrör från en okänd plattform och behandlas som osäkra, blir föremål för mer restriktiva regler, t.ex. kortare TTL-värden för autentisering.<br/><br/>Dessutom är vissa fält, till exempel direktuppspelningsenhetens connectionIp och connectionPort, obligatoriska för funktioner som Spectrum’s Home Base Authentication.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Stabil enhetsidentifierare</i></td>
      <td>Beräkna och lagra en stabil enhets-ID som inte ändras över uppdateringar eller omstarter för huvudet <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.<br/><br/>För plattformar som saknar en maskinvaruidentifierare skapar du en unik identifierare från programattributen och behåller den.</td>
      <td>Riskerar att autentiseringen går förlorad när enhetsidentifieraren ändras.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Följ API-referenser</i></td>
      <td>Kontrollera att du bara skickar förväntade parametrar och rubriker för REST API v2.</td>
      <td>Risker som utlöser HTTP 400-felsvar om felaktig begäran, överlagrar systemresurser och ökar fördröjningen.</td>
   </tr>
</table>

### 8. Felhantering {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Förbättrat stöd för felkodshantering</i></td>
      <td>Hantera de <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">förbättrade felkoderna</a> korrekt och använd åtgärdsfältet för att avgöra vilka reparationssteg som krävs.<br/><br/>Endast ett begränsat antal utökade felkoder kräver ett nytt försök, medan de flesta kräver alternativa upplösningar som anges i åtgärdsfältet.<br/><br/>De mest förbättrade felkoderna i <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">Förbättrade felkoder - REST API V2</a> -dokumentationen kan förhindras helt om de hanteras korrekt under utvecklingsfasen innan programmet startas.</td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".<br/><br/>Riskerar att klientprogrammet fungerar som det ska på grund av att hantering av de förbättrade felkoderna saknas, vilket orsakar otydliga felmeddelanden, felaktig användarvägledning eller felaktigt reservbeteende.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Stöd för HTTP-felhantering</i></td>
      <td>Differentiera mellan hantering av HTTP-felsvar (t.ex. 400, 401, 403, 404, 405, 500) och svar på lyckade åtgärder (t.ex. 200, 2011) som innehåller utökade felkodsnyttolaster, vilket beskrivs ovan.<br/><br/>Endast ett begränsat antal HTTP-felkoder kräver ett nytt försök, medan de flesta kräver alternativa upplösningar.<br/><br/>De flesta HTTP-felsvar kan förhindras helt om de hanteras korrekt under utvecklingsfasen innan programmet startas.</td>
      <td>Risker överbelastar systemresurser, ökar fördröjningen och kan utlösa HTTP 429-felsvar"För många begäranden".<br/><br/>Riskerar att klientprogrammet fungerar som det ska på grund av att hantering av de förbättrade felkoderna saknas, vilket orsakar otydliga felmeddelanden, felaktig användarvägledning eller felaktigt reservbeteende.</td>
   </tr>
</table>

### 9. Testning {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Krav</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Livscykeltestning</i></td>
      <td>Utveckla och testa programmet i de officiella icke-produktionsmiljöerna för Adobe Pass Authentication:<ul><li>Förhandsproduktion</li><li>Frigör mellanlagring</li></ul><br/>Utför en grundlig kvalitetssäkring i dessa miljöer innan du startar produktionen.<br/><br/>Klientprogram får inte fortsätta till versionsproduktion utan att först slutföra en fullständig validering i icke-produktionsmiljöer.</td>
      <td>Risker med allvarliga och allvarliga fel.<br/><br/>Om du saknar en kort och effektiv felsökningsväg kan det hindra Adobe Support och Engineering från att snabbt komma in.</td>
   </tr>
</table>

## Rekommenderad praxis {#recommended-practices}

### 1. Registreringsfas {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Åtkomsttoken, validering</i></td>
      <td>Kontrollera giltigheten för åtkomsttoken och uppdatera den när den har gått ut.<br/><br/>Kontrollera att alla återförsöksmetoder för hantering av HTTP 401-fel av typen "Obehörig" först uppdaterar åtkomsttoken innan du försöker utföra den ursprungliga begäran igen.</td>
      <td>Risker som utlöser HTTP 401 "Obehöriga" felsvar, överbelastar systemresurser och ökar fördröjningen.</td>
   </tr>
</table>

### 2. Konfigurationsfas {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfigurationscache</i></td>
      <td>Lagra konfigurationssvaret i minnet eller i det beständiga lagringsutrymmet under en kort tidsperiod (t.ex. 3-5 minuter) för att förbättra prestandan och minimera onödiga REST API v2-anrop.</td>
      <td>Risker överbelastar systemresurser och ökar fördröjningen.</td>
   </tr>
</table>

### 3. Autentiseringsfas {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Verifiering av autentiseringskod (autentisering på andra skärmen)</i></td>
      <td>Validera autentiseringskoden som skickats via användarindata i det sekundära (andra) programmet (skärmen) innan du anropar /api/v2/authenticate API under följande villkor:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">Autentisering utförd i det sekundära (skärmen) programmet med förvalt mvpd</a></b><ul><li>Använd <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">Återuppta autentiseringssession</a> - POST /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">Autentisering utförd i sekundärt (skärm) program utan förval av mvpd</a></b><ul><li>Använd <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">Hämta autentiseringssession</a> - GET /api/v2/{serviceProvider}/sessioner/{code}</li></ul><br/>Klientprogrammet skulle få ett fel om den angivna autentiseringskoden skrivits fel eller om autentiseringssessionen skulle gå ut.</td>
      <td>Riktar olika felsvar och arbetsflödesproblem under autentisering.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Stöd för flera profiler</i></td>
      <td>Se till att klientprogrammet kan hantera flera profiler genom att antingen uppmana användaren att välja en profil eller använda anpassad logik, till exempel automatiskt välja profilen med den längsta giltighetsperioden.</td>
      <td>Risker med att klientprogrammet inte fungerar som det ska på grund av att det saknas stöd på klientprogramsidan.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(Valfritt) Stöd för icke-grundläggande flöden</i></td>
      <td>Stöd för icke-grundläggande flöden om klientapplikationsverksamheten kräver:<ul><li>Försämrade åtkomstflöden (premiumfunktion)</li><li>Tillfälliga åtkomstflöden (premiumfunktion)</li><li>Åtkomstflöden för enkel inloggning (standardfunktion)</li></ul></td>
      <td>Risker som skapar en användarupplevelse som inte är perfekt.</td>
   </tr>
</table>

### 4. (Valfritt) Förhandsauktoriseringsfas {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Användarupplevelse</i></td>
      <td>Visa tydlig användarfeedback om ett beslut om förauktorisering nekas, med meddelanden från distributörer av videoprogrammeringstjänster eller Adobe via förbättrade felkoder.</td>
      <td>Risker som skapar en användarupplevelse som inte är perfekt.</td>
   </tr>
</table>

### 5. Auktoriseringsfas {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validering av medietoken</i></td>
      <td>Validera medietoken med biblioteket <a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">Media Token Verifier</a>.</td>
      <td>Risker för bedrägerischeman, t.ex. strörippning.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Användarupplevelse</i></td>
      <td>Visa tydlig användarfeedback om ett auktoriseringsbeslut nekas med hjälp av meddelanden från distributörer av videoprogrammeringstjänster eller Adobe via förbättrade felkoder.</td>
      <td>Risker som skapar en användarupplevelse som inte är perfekt.</td>
   </tr>
</table>

### 6. Utloggningsfas {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Användarupplevelse</i></td>
      <td>Undvik att anropa inloggnings-API automatiskt (programmatiskt) i scenarier som nekad förauktorisering eller auktorisering, eftersom utloggnings-API bara ska anropas som svar på en begäran från en direkt användare.</td>
      <td>Risker som förvirrar användaren om att autentiseringen misslyckas.</td>
   </tr>
</table>

### 7. Parametrar och rubriker {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Återanvänd kod</i></td>
      <td>Återanvänd kod från REST API v1 för att beräkna enhets-ID och enhetsinformation med mindre justeringar, men kontrollera att du bara skickar förväntade parametrar och rubriker för REST API v2.<br/><br/>Återanvänd kod från REST API v1 för att anropa DCR-API:t för att hämta en åtkomsttoken.</td>
      <td>-</td>
   </tr>
</table>

### 8. Testning {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Praktik</th>
      <th style="background-color: #EFF2F7;">Risker</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Testtäckning</i></td>
      <td>Kontrollera att följande grundläggande flöden har testats på olika enheter och plattformar:<br/><br/><b>Autentiseringsflöden</b><ul><li>Autentiseringsscenario för primärt program (skärm)</li><li>Sekundärt autentiseringsscenario för program (skärm)</li></ul><br/><b>(Valfritt) Förhandsauktoriseringsflöden</b><ul><li>Testa scenariot för tillståndsbeslut</li><li>Testa scenariot för att neka beslut</li></ul><br/><b>Behörighetsflöden</b><ul><li>Testa scenariot för tillståndsbeslut</li><li>Testa scenariot för att neka beslut</li></ul><br/><b>Utloggningsflöden</b><br/><br/>Testa andra åtkomstflöden om tillämpligt:<br/><br/><ul><li>Försämrade åtkomstflöden (premiumfunktion)</li><li>Tillfälliga åtkomstflöden (premiumfunktion)</li><li>Åtkomstflöden för enkel inloggning (standardfunktion)</li></ul><br/>Täck de vanligaste MVPD-integreringarna (omfattar de mest använda leverantörerna).</td>
      <td>Riskerar oförutsedda produktionsfel, särskilt på mindre frekvent testade plattformar eller sällsynta flöden som negativa scenarier.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Testverktyg</i></td>
      <td>Använd webbplatsen <a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>.</td>
      <td>-</td>
   </tr>
</table>

## Sammanfattning {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Fas</th>
      <th style="background-color: #EFF2F7;">Obligatoriskt</th>
      <th style="background-color: #EFF2F7;">(Stabil) Rekommenderas</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registrering</i></td>
      <td>Cachelagra klientautentiseringsuppgifter<br/><br/>Cacheåtkomsttoken</td>
      <td>Validera och uppdatera åtkomsttoken</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Konfiguration</i></td>
      <td>Minimera hämtningar av konfigurationssvar</td>
      <td>Konfigurationssvar för cache</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autentisering</i></td>
      <td>Avsökningsmekanism finjusterar<br/><br/>Cachelagra delar av profiler</td>
      <td>Stöd för flera profiler<br/><br/>Funktion för supportdegradering (om verksamheten kräver det)<br/><br/>Stöd för TempPass-funktionen (om verksamheten kräver det)<br/><br/>Stöd för enkel inloggning (om verksamheten kräver det)</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Förhandsauktorisering</i></td>
      <td>Cachelagra beslut om förhandsauktorisering<br/><br/>Finjustera återförsöksmekanismen</td>
      <td>Förbättra användarupplevelsen med hjälp av felkoder för nekade förauktoriseringsbeslut</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Behörighet</i></td>
      <td>Hämta auktoriseringsbeslut när användaren begär uppspelning<br/><br/>Finjustera återförsöksmekanism</td>
      <td>Förbättra användarupplevelsen med felkoder för beslut om nekad auktorisering<br/><br/>Medietokenvalidering</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Utloggning</i></td>
      <td>Implementera utloggnings-API så att användare kan logga ut manuellt</td>
      <td>Undvik att anropa utloggnings-API automatiskt</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Obligatoriskt</th>
      <th style="background-color: #EFF2F7;">(Stabil) Rekommenderas</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Parametrar och rubriker</i></td>
      <td>Följ obligatoriska rubrikspecifikationer</td>
      <td>Återanvänd kod från REST API v1</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Felhantering</i></td>
      <td>Implementera förbättrad felhantering<br/><br/>Implementera HTTP-felhantering</td>
      <td>-</td>
   </tr>
</table>
