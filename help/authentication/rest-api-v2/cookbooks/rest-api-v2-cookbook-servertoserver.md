---
title: REST API V2 Cookbook (Server-to-Server)
description: REST API V2 Cookbook (Server-to-Server)
source-git-commit: 87d4d95a3bf4ace68bc71ca700b09da14ee316f4
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# REST API V2 Cookbook (Server-to-Server) {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. No unauthorized use is permitted.


## Ökning {#overview}

Syftet med det här cookbookdokumentet är att ge detaljerad information om bästa praxis för implementering av Adobe Pass Authentication med REST API V2 i en Server-till-Server-arkitektur.  Den innehåller grundläggande krav, stegvis implementering av flöde och allmänna överväganden för produktionsmiljöer och drift.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/throttling-mechanism.md).


## Komponenter {#components}

I en fungerande Server-till-Server-lösning ingår följande komponenter:


| Type | Component | Description |
| --- | --- | --- |
| Streaming Device | Streaming App | The Programmer application that resides on the user&#39;s streaming device and plays authenticated video. |
| | \[Optional\] AuthN Module | if Streaming Device has a User Agent (i.e. Web Browser), the AuthN Module is resposnible for authenticating the user on the MVPD IdP. |
| \[Optional\] AuthN Device | AuthN App | if the Streaming Device does not have a User Agent (i.e. Web Browser), the AuthN Application is a Programmer web application that is accessed from a separte user&#39;s device using a web browser. |
| Programmeringsinfrastruktur | Programmerartjänst | En tjänst som länkar direktuppspelningsenheten till Adobe Pass-tjänsten för att få autentiserings- och auktoriseringsbeslut. |
| Adobe infrastruktur | Adobe Pass Service | En tjänst som integreras med MVPD IdP- och AuthZ-tjänsten och som ger autentiserings- och auktoriseringsbeslut. |
| MVPD-infrastruktur | MVPD IdP | En MVPD-slutpunkt som tillhandahåller autentiseringsbaserad autentisering för att validera användarens identitet. |
| | MVPD AuthZ-tjänst | En MVPD-slutpunkt som ger auktoriseringsbeslut baserat på användarens prenumerationer, föräldrakontroll osv. |


Ytterligare termer som används i flödet definieras i
[Ordlista](/help/authentication/glossary.md).

I följande diagram visas hela flödet:

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### Steg för att implementera REST API V2 i server-till-server-arkitekturen {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

För att implementera Adobe Pass REST API V2 måste du följa stegen nedan grupperade i faser.

## 0. Förutsättningar {#prerequisites}

I server-till-server-implementeringar måste Streaming App och Programmer Service upprätta ett protokoll för att Programmer Service ska kunna:
* unikt identifiera den strömmande appen på enheten
* agera för Streaming App och kommunicera med Adobe Pass Service
* samla in och lagra information om strömningsappen och enheten som IP-adress, källport, enhetsinformation för att skicka den till Adobe Pass
* returnera beslut och instruktioner till Streaming App

Parametrar som enhets-ID, klient-ID, klienthemlighet (definieras nedan) kan lagras i antingen Streaming App eller Programmer Service.

## A. Registreringsfas {#registration-phase}

### Steg 1: Registrera ditt program {#step-1-register-your-application}

För att programmet ska kunna anropa Adobe Pass REST API V2 krävs en åtkomsttoken som krävs för API:ts säkerhetslager. För server-till-server-implementeringar kan programmeringstjänsten registrera för en programinstans.
Värdena för klient-ID och klienthemlighet måste hämtas för varje direktuppspelningsenhet.

[](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B. Autentiseringsfas {#authentication-phase}

### Steg 2: Kontrollera om det finns befintliga autentiserade profiler {#step-2-check-for-existing-authenticated-profiles}

Programmerartjänsten söker efter befintliga autentiserade profiler för direktuppspelningsappens räkning: `/api/v2/{serviceProvider}/profiles` ([Hämta autentiserade profiler](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Om ingen profil hittas och Streaming-programmet implementerar ett TempPass-flöde
   * Följ dokumentationen om hur du implementerar [Tillfälliga åtkomstflöden](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Om ingen profil hittas implementerar direktuppspelningsprogrammet ett autentiseringsflöde
   * <b>Steg 2.a:</b> Programmeringstjänsten hämtar listan över tillgängliga MVPD-filer för serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Hämta lista över tillgängliga MVPD:er ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * Programmeringstjänsten får använda filtrering i listan över videofilmsprogram och endast visa videofilmsprogram som är avsedda att dölja andra (TempPass, test-videofilmsprogram, videofilmsprogram under utveckling osv.)
   * Om programmeringstjänsten ska returnera en filtrerad MVPD-lista för direktuppspelningsappen för att visa väljaren, väljer användaren MVPD
   * med MVPD valt från direktuppspelningsappen skapar programmeringstjänsten en session: <b>/api/v2/{serviceProvider}/sessions </b><br>
([Skapa autentiseringssession ](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * en CODE och URL som ska användas för autentisering returneras
      * Om en profil hittas kan programmeringstjänsten fortsätta till <a href="#preauthorization-phase">C. Förhandsauktoriseringsfas </a>
   * Programmerartjänsten ska returnera CODE och URL till direktuppspelningsappen

### Steg 3: Autentisera användaren {#step-3-authenticate-the-user}

Använda en webbläsare eller ett webbaserat program för en andra skärm:

* Alternativ 1. Streaming App can open a browser or webview, load the URL to authenticate and the user lands on MVPD login page where credentials needs to be submitted
   * användarens inloggning/lösenord, den slutliga omdirigeringen visar en sida som slutfördes
* Alternativ 2. Direktuppspelningsappen kan inte öppna en webbläsare och bara visa CODE. <b>Ett separat webbprogram, AuthN_APP, måste utvecklas</b> för att användaren ska kunna ange CODE, skapa och öppna URL:en: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * användarens inloggning/lösenord, den slutliga omdirigeringen visar en sida som slutfördes

### Steg 4: Kontrollera om det finns autentiserade profiler {#step-4-check-for-authenticated-profiles}

Programmerartjänsten söker efter autentisering med MVPD som ska slutföras i webbläsaren eller på andra skärmen

* <b></b><br>[](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
   * <b></b><br>[](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* Polling should not exceed 30 minutes, in case 30 minutes are reached and the Streaming Application is still active, a new session needs to be initiated and a new CODE and URL will be returned
* When authentication is complete the return is 200 with authenticated profile
* <a href="#preauthorization-phase"></a>

## C. Preauthorization phase {#preauthorization-phase}

### Step 5: Check for preauthorized resources {#step-5-check-for-preauthorized-resources}

With a valid authentication profile for a user, the Programmer Service has the possibility to check the
access to the videos available and pass the list to the Streaming Application to display.

* Step is optional and executed if the application wants to filter our the resources not available in the authenticated user package
* <b></b><br>[](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## D. Authorization phase {#authorization-phase}

### Steg 6: Kontrollera om det finns auktoriserade resurser {#step-6-check-for-authorized-resources}

Direktuppspelningsappen förbereds för uppspelning av en video/resurs/resurs som valts av användaren.

* Steg krävs för varje uppspelningsstart
* Direktuppspelande app skickar informationen till programmerartjänsten
* Programmeringtjänsten för direktuppspelningsappen, anropa <b>/api/v2/{serviceProvider}/Decision/authorized/{mvpd}</b><br>
([Hämta auktoriseringsbeslut med specifikt MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * beslut = &#39;Permit&#39;, programmerartjänsten instruerar direktuppspelningsappen att starta direktuppspelning
   * beslut = &#39;Neka&#39;, Programmeringstjänsten instruerar strömningsappen att informera användaren om att den inte har åtkomst till den videon
   * under processen kan Programmeringstjänsten utvärdera andra affärsregler och returnera lämpliga beslut till Streaming App

## E. Utloggningsfas {#logout-phase}

### Steg 7: Logga ut {#step-7-logout}

Strömmande app: Användaren vill logga ut från MVPD

* Direktuppspelningsappen informerar programmeringstjänsten om att den behöver logga ut från MVPD för den här specifika appen.
* Programmeringstjänsten kan rensa upp informationen om den autentiserade användaren
* programmeringstjänstens anrop <b>/api/v2/{serviceProvider}/logOut/{mvpd}</b><br>
([Initiera utloggning för specifikt MVPD ](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Om response actionType=&#39;interactive&#39; och url finns med, kommer programmerartjänsten att returnera till Streaming App på webbadressen
* Baserat på befintliga funktioner kan Streaming App öppna URL:en i webbläsaren (vanligtvis samma som används för autentisering)
* Om direktuppspelningsappen inte har någon webbläsare eller är en annan instans än den vid autentiseringen, kan flödet stoppas eftersom MVPD-sessionen inte sparades i webbläsarens cache.

## Miljö och funktionskrav{#environments}

En programmerare bör skapa minst två miljöer: en för produktion och en eller flera för mellanlagring.


### Produktion

Produktionsmiljön bör vara mycket tillgänglig och skalad på lämpligt sätt för stora eller oväntade toppar (t.ex. live-sporter, krossa
nyheter).



Adobe Pass-tjänsten körs på flera datacenter som är geografiskt spridda i hela USA.  För att få bästa svarstid (dvs. lägsta svarstid) från Adobe Pass-tjänsten bör Programmeraren även skapa en liknande geografiskt spridd tjänst
infrastruktur.


Programmerartjänsten bör begränsa DNS-cachen till högst 30 sekunder om Adobe behöver dirigera om trafiken. Detta kan inträffa om ett datacenter blir otillgängligt.


Programmeraren ska tillhandahålla produktionsmiljöns offentliga IP-intervall. Dessa kommer att ingå i en tillåten lista över IP-adresser i Adobe Pass-infrastruktur för åtkomst och hanteras av Adobe användningspolicyer för rättvisa API.

### Mellanlagring

Mellanlagringsmiljön kan vara minimal, men bör innehålla alla systemkomponenter och affärslogik. Den bör fungera på liknande sätt som produktionen och göra det möjligt att testa releaser utanför produktionen. I idealfallet kan mellanlagringsmiljön anslutas till Adobe Pass testmiljöer som kan användas av Programmer och av Adobe vid behov så att vi kan hjälpa till med testning och felsökning.

### Funktionskrav

Programmerartjänsten måste skicka korrekt enhetsidentifieringsinformation för den enhet som de kör flödena för. Programmerartjänsten måste dessutom skicka IP-adressen för den enhet som de kör flödena för (i ett x-vidarebefordrat-for-huvud) tillsammans med anslutningskällporten (i enhetsinformationsfältet):

Programmerartjänsten ska skicka data och formatering som krävs av enskilda MVPD-program eller integrerade program (t.ex. enhets-IP, källport, enhetsinformation, MRSS, valfria data som ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

Programmerartjänsten måste respektera autentiseringsprofilen och beslutets giltighet vid cachelagring och ogiltigförklara autentiseringarna eller besluten när de meddelas.

Programmeringstjänsten måste underhålla certifikat som delas med Adobe (för krypterade användarmetadata).

## Relaterad information {#related}

* [REST API V2-referens](/help/authentication/rest-api-v2/rest-api-v2-flows-overview.md)
