---
title: REST API V2 - kokbok - implementeringssteg för klient/server
description: REST API V2 - kokbok - implementeringssteg för klient/server
source-git-commit: 5ba538bdb13d121ba27005df82d4ae604f912241
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# REST API V2 - kokbok - implementeringssteg för klient/server {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/throttling-mechanism.md).

## Steg för att implementera REST API V2 i klientprogram {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

För att implementera Adobe Pass REST API V2 måste du följa stegen nedan grupperade i faser.

## A. Registreringsfas {#registration-phase}

### Steg 1: Registrera ditt program {#step-1-register-your-application}

För att programmet ska kunna anropa Adobe Pass REST API V2 krävs en åtkomsttoken som krävs för API:ts säkerhetslager.
För att få åtkomst-token måste programmet följa de steg som beskrivs:
[Registrering av dynamisk klient](./dynamic-client-registration.md)

## B. Autentiseringsfas {#authentication-phase}

### Steg 2: Kontrollera om det finns befintliga autentiserade profiler {#step-2-check-for-existing-authenticated-profiles}

Strömmande programkontroller för befintliga autentiserade profiler: <b>/api/v2/{serviceProvider}/profiles</b><br>
([Hämta autentiserade profiler](./apis/profiles-apis/rest-api-v2-retrieve-authenticated-profiles.md))

* om ingen profil hittas och Streaming-programmet implementerar ett TempPass-flöde
   * Följ dokumentationen om hur du implementerar [Tillfälliga åtkomstflöden](./temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* om ingen profil hittas implementerar direktuppspelningsprogrammet ett autentiseringsflöde
   * Direktuppspelningsprogrammet hämtar listan över MVPD:er som är tillgängliga för serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Hämta lista över tillgängliga MVPD:er ](./apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Strömningsprogram kan implementera filtrering i listan över MVPD-program och endast visa MVPD-program som är avsedda att dölja andra (TempPass, test MVPDs, MVPDs under utveckling osv.)
   * Visningsväljaren för direktuppspelande program, användaren väljer MVPD
   * Direktuppspelningsprogram skapar en session: <b>/api/v2/{serviceProvider}/sessions </b><br>
([Skapa autentiseringssession ](./apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * en CODE och URL som ska användas för autentisering returneras
      * Om en profil hittas kan direktuppspelningsprogrammet fortsätta till <a href="#preauthorization-phase">C. Förauktoriseringsfas </a> eller <a href="#authorization-phase">D. Auktoriseringsfas </a>

### Steg 3: Autentisera användaren {#step-3-authenticate-the-user}

Använda en webbläsare eller ett webbaserat program för en andra skärm:

* Alternativ 1. Direktuppspelningsprogrammet kan öppna en webbläsare eller en webbvy, läsa in URL:en för autentisering och användaren går till inloggningssidan för MVPD där inloggningsuppgifter måste skickas
   * användarens inloggning/lösenord, den slutliga omdirigeringen visar en sida som slutfördes
* Alternativ 2. Direktuppspelningsprogrammet kan inte öppna en webbläsare utan bara visa CODE. <b>Ett separat webbprogram måste utvecklas</b> för att användaren ska kunna ange CODE, skapa och öppna URL: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * användarens inloggning/lösenord, den slutliga omdirigeringen visar en sida som slutfördes

### Steg 4: Kontrollera om det finns autentiserade profiler {#step-4-check-for-authenticated-profiles}

Strömmande applikationskontroller för autentisering med MVPD som slutförs i webbläsaren eller på andra skärmen

* avsökning var femtonde sekund rekommenderas för <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Hämta autentiserade profiler för specifikt MVPD](.apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Om MVPD-valet inte görs i direktuppspelningsprogrammet eftersom MVPD-väljaren presenteras i programmet för sekundär skärm, ska avsökningen göras med CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Hämta autentiserade profiler för specifik KOD ](./apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* avsökningen får inte överskrida 30 minuter, om 30 minuter har uppnåtts och direktuppspelningsprogrammet fortfarande är aktivt, måste en ny session startas och en ny CODE och URL returneras
* när autentiseringen är klar är returen 200 med autentiserad profil
* Direktuppspelningsprogrammet kan fortsätta till <a href="#preauthorization-phase">C. Förauktoriseringsfas </a> eller <a href="#authorization-phase">D. Auktoriseringsfas </a>

## C. Förhandsauktoriseringsfas {#preauthorization-phase}

### Steg 5: Sök efter förauktoriserade resurser {#step-5-check-for-preauthorized-resources}

Direktuppspelningsprogram förbereds för visning av videor som är tillgängliga för den autentiserade användaren och har möjlighet att kontrollera
åtkomst till dessa resurser.

* är valfritt och körs om programmet vill filtrera resurserna som inte är tillgängliga i det autentiserade användarpaketet
* anrop till <b>/api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}</b><br>
([Hämta förauktoriseringsbeslut med hjälp av specifikt MVPD ](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))


## D. Auktoriseringsfas {#authorization-phase}

### Steg 6: Kontrollera om det finns auktoriserade resurser {#step-6-check-for-authorized-resources}

Direktuppspelningsprogrammet förbereds för uppspelning av en video/resurs/resurs som valts av användaren.

* steg krävs för varje uppspelningsstart
* call <b>/api/v2/{serviceProvider}/Decision/authorized/{mvpd}</b><br>
([Hämta auktoriseringsbeslut med specifikt MVPD](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * beslut = &#39;Tillåt&#39;, direktuppspelningsenheten börjar direktuppspelningen
   * beslut = &#39;Neka&#39;, strömningsenheten informerar användaren om att den inte har åtkomst till den videon

## E. Utloggningsfas {#logout-phase}

### Steg 7: Logga ut {#step-7-logout}

Direktuppspelningsenhet: Användaren vill logga ut från MVPD

* call <b>/api/v2/{serviceProvider}/logOut/{mvpd}</b><br>
([Initiera utloggning för specifikt MVPD ](.apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* om response actionType=&#39;interactive&#39; och url finns, öppnar du webbadressen i en webbläsare/sekundär skärm för att slutföra utloggningen med MVPD

