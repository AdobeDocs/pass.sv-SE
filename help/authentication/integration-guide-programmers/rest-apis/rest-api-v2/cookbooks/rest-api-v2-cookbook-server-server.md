---
title: REST API V2 Cookbook (Server-to-Server)
description: REST API V2 Cookbook (Server-to-Server)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# REST API V2 Cookbook (Server-to-Server) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

Syftet med det här cookbookdokumentet är att ge detaljerad information om bästa praxis för implementering av Adobe Pass Authentication med REST API V2 i en Server-till-Server-arkitektur. Den innehåller grundläggande krav, stegvis implementering av flöde och allmänna överväganden för produktionsmiljöer och drift.

## Komponenter {#components}

I en fungerande server-till-server-lösning ingår följande komponenter:

| Typ | Komponent | Beskrivning |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Strömmande enhet | Strömmande app | Programmeringsprogrammet som finns på användarens direktuppspelningsenhet och spelar upp autentiserad video. |
|                           | \[Valfritt\] AuthN-modul | Om direktuppspelningsenheten har en användaragent (t.ex. en webbläsare) ansvarar AuthN-modulen för att autentisera användaren på MVPD IdP. |
| \[Valfritt\] AuthN-enhet | AuthN-app | Om direktuppspelningsenheten inte har någon användaragent (t.ex. webbläsare) är AuthN-programmet ett webbprogram för programmerare som nås från en separat användares enhet via en webbläsare. |
| Programmeringsinfrastruktur | Programmerartjänst | En tjänst som länkar direktuppspelningsenheten till Adobe Pass-tjänsten för att få autentiserings- och auktoriseringsbeslut. |
| Adobe infrastruktur | Adobe Pass Service | En tjänst som integreras med MVPD IdP- och AuthZ-tjänsten och som ger autentiserings- och auktoriseringsbeslut. |
| MVPD Infrastructure | MVPD IdP | En MVPD-slutpunkt som tillhandahåller autentiseringsbaserad autentisering för att validera användarens identitet. |
|                           | MVPD AuthZ-tjänst | En MVPD-slutpunkt som ger auktoriseringsbeslut baserat på användarens prenumerationer, föräldrakontroll osv. |

Ytterligare termer som används i flödet definieras i [ordlistan](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

I följande diagram visas hela flödet:

![REST API V2 Cookbook (Server-to-Server)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### Steg för att implementera REST API V2 i server-till-server-arkitekturen {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

För att implementera Adobe Pass REST API V2 måste du följa stegen nedan grupperade i faser.

## 0. Förutsättningar {#prerequisites}

I Server-to-Server-implementeringar måste Streaming App och Programmer Service upprätta ett protokoll för programmerartjänsten för att kunna:

* Identifiera strömningsappen på enheten unikt.
* Agera för Streaming App och kommunicera med Adobe Pass Service.
* Samla in och lagra information om strömningsappen och enheten som IP-adress, källport, enhetsinformation för att skicka den till Adobe Pass.
* Skicka tillbaka beslut och instruktioner till Streaming App.

Parametrar som enhets-ID, klient-ID, klienthemlighet (definieras nedan) kan lagras i antingen Streaming App eller Programmer Service.

## A. Registreringsfas {#registration-phase}

### Steg 1: Registrera ditt program {#step-1-register-your-application}

För att programmet ska kunna anropa Adobe Pass REST API V2 krävs en åtkomsttoken som krävs för API:ts säkerhetslager.

För Server-till-server-implementeringar kan programmeringstjänsten registrera för en programinstans, men klientens autentiseringsuppgifter (klient-ID och klienthemlighet) måste hämtas för varje direktuppspelningsenhet.

För att få åtkomst-token kan programmeringstjänsten agera för en direktuppspelningsapp och måste följa de steg som beskrivs i dokumentationen för [registrering av dynamisk klient](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## B. Autentiseringsfas {#authentication-phase}

### Steg 2: Kontrollera om det finns befintliga autentiserade profiler {#step-2-check-for-existing-authenticated-profiles}

Programmerartjänsten söker efter befintliga autentiserade profiler för direktuppspelningsappens räkning: `/api/v2/{serviceProvider}/profiles` ([Hämta autentiserade profiler](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)).

* Om ingen profil hittas och Streaming-programmet implementerar ett TempPass-flöde
   * Följ dokumentationen om hur du implementerar [Tillfälliga åtkomstflöden](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Om ingen profil hittas implementerar direktuppspelningsprogrammet ett autentiseringsflöde
   * <b>Steg 2.a:</b> Programmeringstjänsten hämtar listan över tillgängliga MVPD-filer för serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Hämta lista över tillgängliga MVPD:er ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Programmeringstjänsten kan använda filtrering i listan över MVPD och endast visa MVPD som är avsedda att dölja andra (TempPass, test MVPD, MVPD under utveckling osv.)
   * Programmeringstjänsten ska returnera en filtrerad MVPD-lista för direktuppspelningsappen som ska visa väljaren. Användaren väljer MVPD
   * Med MVPD valt från direktuppspelningsappen skapar programmeringstjänsten en session: <b>/api/v2/{serviceProvider}/sessions </b><br>
([Skapa autentiseringssession ](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * En CODE och URL som ska användas för autentisering returneras
      * Om en profil hittas kan programmeringstjänsten fortsätta till <a href="#preauthorization-phase">C. Förhandsauktoriseringsfas </a>
   * Programmerartjänsten ska returnera KOD och URL till strömningsappen

### Steg 3: Autentisera användaren {#step-3-authenticate-the-user}

Använda en webbläsare eller ett webbaserat program för sekundär skärm:

* Alternativ 1. Strömmande app kan öppna en webbläsare eller webbvy, läsa in den URL som ska autentiseras och användaren loggar in på MVPD inloggningssida där inloggningsuppgifter måste skickas
   * Användaren anger inloggning/lösenord, den slutliga omdirigeringen visar en sida om slutförd åtgärd
* Alternativ 2. Direktuppspelningsappen kan inte öppna en webbläsare och bara visa CODE. <b>Ett separat webbprogram, AuthN_APP, måste utvecklas</b> för att användaren ska kunna ange CODE, skapa och öppna URL: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * Användaren anger inloggning/lösenord, den slutliga omdirigeringen visar en sida om slutförd åtgärd

### Steg 4: Kontrollera om det finns autentiserade profiler {#step-4-check-for-authenticated-profiles}

Programmerartjänsten söker efter autentisering med MVPD som ska slutföras i webbläsaren eller på andra skärmen

* Avsökning var femtonde sekund rekommenderas för <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Hämta autentiserade profiler för specifika MVPD](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Om MVPD-val inte görs i direktuppspelningsprogrammet eftersom MVPD-väljaren presenteras i programmet för sekundär skärm, ska avsökningen göras med CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Hämta autentiserade profiler för specifik KOD ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* Avsökningen bör inte överstiga 30 minuter, om 30 minuter har uppnåtts och direktuppspelningsprogrammet fortfarande är aktivt, en ny session måste initieras och en ny CODE och URL returneras
* När autentiseringen är klar är returen 200 med autentiserad profil
* Programmerartjänsten kan fortsätta till <a href="#preauthorization-phase">C. Förhandsauktoriseringsfas </a>

## C. Förhandsauktoriseringsfas {#preauthorization-phase}

### Steg 5: Sök efter förauktoriserade resurser {#step-5-check-for-preauthorized-resources}

Med en giltig autentiseringsprofil för en användare kan programmeringstjänsten kontrollera åtkomsten till de videor som är tillgängliga och skicka listan till det direktuppspelningsprogram som ska visas.

* Steget är valfritt och körs om programmet vill filtrera bort resurser som inte är tillgängliga i det autentiserade användarpaketet
* Anrop till <b>/api/v2/{serviceProvider}/Decision/preauthorized/{mvpd}</b><br>
([Hämta förauktoriseringsbeslut med specifika MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Auktoriseringsfas {#authorization-phase}

### Steg 6: Kontrollera om det finns auktoriserade resurser {#step-6-check-for-authorized-resources}

Direktuppspelningsappen förbereds för uppspelning av en video/resurs/resurs som valts av användaren.

* Steg krävs för varje uppspelningsstart
* Direktuppspelningsappen skickar informationen till programmeringstjänsten
* Programmeringstjänsten för direktuppspelningsappen, anropa <b>/api/v2/{serviceProvider}/Decision/authorized/{mvpd}</b><br>
([Hämta auktoriseringsbeslut med specifika MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * beslut = &#39;Permit&#39;, Programmer Service instruerar Streaming App att starta direktuppspelning
   * beslut = &#39;Neka&#39;, Programmeringstjänsten instruerar direktuppspelningsappen att informera användaren om att den inte har åtkomst till den videon
   * under processen kan Programmeringstjänsten utvärdera andra affärsregler och returnera lämpliga beslut till Streaming App

## E. Utloggningsfas {#logout-phase}

### Steg 7: Logga ut {#step-7-logout}

Direktuppspelningsapp: Användaren vill logga ut från MVPD

* Direktuppspelningsappen informerar programmeringstjänsten om att den behöver logga ut från MVPD för den här specifika appen.
* Programmeringstjänsten kan rensa upp den information som lagras om den autentiserade användaren
* Programmerartjänsten anropar <b>/api/v2/{serviceProvider}/logOut/{mvpd}</b><br>
([Initiera utloggning för specifik MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Om response actionType=&#39;interactive&#39; och url finns med, kommer programmerartjänsten att returnera till Streaming App på webbadressen
* Baserat på befintliga funktioner kan Streaming App öppna URL:en i webbläsaren (vanligtvis samma som används för autentisering)
* Om direktuppspelningsappen inte har någon webbläsare, eller om den är en annan instans än den vid autentiseringen, kan flödet stoppas eftersom MVPD-sessionen inte sparades i webbläsarcachen.

## Miljö och funktionskrav{#environments}

En programmerare bör skapa minst två miljöer: en för produktion och en eller flera för mellanlagring.

### Produktion

Produktionsmiljön bör vara mycket tillgänglig och skalas på lämpligt sätt för stora eller oväntade toppar (t.ex. live-sporter, nyheter).

Adobe Pass-tjänsten körs på flera datacenter som är geografiskt spridda i hela USA. För att få bästa svarstid (dvs. lägsta svarstid) från Adobe Pass-tjänsten bör Programmeraren även skapa en liknande geografiskt spridd infrastruktur.

Programmerartjänsten bör begränsa DNS-cachen till högst 30 sekunder om Adobe behöver dirigera om trafiken. Detta kan inträffa om ett datacenter blir otillgängligt.

Programmeraren ska tillhandahålla produktionsmiljöns offentliga IP-intervall. Dessa kommer att ingå i en tillåten lista över IP-adresser i Adobe Pass-infrastruktur för åtkomst och hanteras av Adobe användningspolicyer för rättvisa API.

### Mellanlagring

Mellanlagringsmiljön kan vara minimal, men bör innehålla alla systemkomponenter och affärslogik. Den bör fungera på liknande sätt som produktionen och göra det möjligt att testa releaser utanför produktionen. I idealfallet kan mellanlagringsmiljön anslutas till Adobe Pass testmiljöer som kan användas av Programmer och av Adobe vid behov, så att vi kan hjälpa till med testning och felsökning.

### Funktionskrav

Programmerartjänsten måste skicka korrekt enhetsidentifieringsinformation för den enhet som de kör flödena för. Programmerartjänsten måste dessutom skicka IP-adressen för den enhet som de kör flödena för (i ett x-vidarebefordrat-for-huvud) tillsammans med anslutningskällporten (i enhetsinformationsfältet):

Programmerartjänsten ska skicka data och formatering som krävs av enskilda MVPD-program eller integrerade program (t.ex. enhets-IP, källport, enhetsinformation, MRSS, valfria data som ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

Programmerartjänsten måste respektera autentiseringsprofilen och beslutets giltighet vid cachelagring och ogiltigförklara autentiseringarna eller besluten när de meddelas.

Programmeringstjänsten måste underhålla certifikat som delas med Adobe (för krypterade användarmetadata).

## Relaterad information {#related}

* [REST API V2-referens](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
