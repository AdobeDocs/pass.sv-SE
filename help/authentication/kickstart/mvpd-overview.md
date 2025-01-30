---
title: Översikt över PDF-filer
description: Översikt över PDF-filer
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '2727'
ht-degree: 0%

---

# Översikt över flerkanalsprogrammeringsprogram {#mvpd-overview}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

Den här översikten är avsedd för distributörer av flerkanalsvideo (MVPD). Ytterligare dokumentation, inklusive Kickstart- och Integreringsguider, finns i avsnittet Relaterad information i slutet av det här dokumentet.



TV Everywhere (TVE) är den nu välkända branschrörelsen som gör det möjligt för Pay TV-prenumeranter att få tillgång till innehåll de redan betalar för, på flera enheter, både i och utanför sina hem.  För leverantörer av betal-TV skapar TVE nya möjligheter, både för att bevara befintliga kundrelationer och för att möjliggöra nya. Men tillsammans med dessa möjligheter finns det utmaningar. I TVE-landskapet tillhandahåller programmerare innehållet, men programmerarna innehåller kundinformation som verifierar att potentiella tittare är giltiga prenumeranter.



Det kan vara enkelt att samordna autentisering och behörighet av tittare med en programmerare, men det blir allt mer komplicerat att samordna med dussintals eller hundratals olika programmerare. Men med Adobe® Pass behöver programmeringsleverantörer bara implementera en enda, enkel integrering för att få tillgång till hela TVE-ekosystemet, inklusive programmerare som NBCUniversal Media, Turner Broadcasting (TBS, TNT, CNN), Fox Broadcast Networks, Hulu och så vidare.  Adobe Pass Authentication är ett integreringsramverk som gör det enkelt och säkert att avgöra användarnas behörighet.



Sammanfattningsvis: Adobe Pass Authentication medföljer på ett säkert sätt berättigandetransaktioner mellan programmerare och distributörer av videoprogrammeringstjänster, vilket underlättar läsarens åtkomst till prenumerationsmaterialet. Med andra ord gör Adobe Pass Authentication det enkelt och snabbt för rätt kunder att få tillgång till rätt innehåll.


Med Adobe Pass Authentication får MVPD:

Enkel integrering med programmerare.  Leverera direktanslutning från flera innehållsägare med en enda integrering.

Förbättrat kundengagemang.  Stöd för en smidig varumärkesanpassad upplevelse när kunderna ser innehåll på flera plattformar och enheter.

Säker autentisering.  Se till att endast behöriga användare och enheter får tillgång till premiuminnehåll och (valfritt) begränsa antalet enheter och samtidiga strömmar som kan anslutas per hushållskonto.

## Vanliga frågor {#faq}

Hur säker är Adobe Pass Authentication? Den främsta prioriteten i Adobe Pass autentiseringsarkitektur är att säkerställa att endast behöriga läsare autentiseras och beviljas åtkomst till premiuminnehåll. Adobe Pass Authentication binder åtkomsten till olika enheter och kan bidra till att begränsa strömmar, sessioner och/eller enheter för ett visst hushåll.


Krävs Flash Player? Adobe Pass Authentication for TV Everywhere är en spelare- och plattformsoberoende som integreras med alla uppspelningsapplikationer, inklusive Silverlight och HTML5. Dessutom har Adobe Pass Authentication inbyggt stöd för enheter som telefoner och surfplattor med iOS och Android.


Vilka enheter stöder Adobe Pass Authentication? Adobe Pass Authentication stöds av praktiskt taget alla enheter med webbpaketet HTML5 för visning i webbläsare. Dessutom fortsätter Adobe Pass Authentication att lansera SDK:er (Software Development Kits) för olika enhetsspecifika plattformar, bland annat iOS, Android™, Xbox360 (Borttagen) och Adobe Air® (borttagen). Nyligen har Adobe Pass Authentication tagit fram en klientlös lösning för enheter som inte kan återge webbläsarsidor (till exempel&quot;smarta&quot; TV-apparater, digitalboxar och spelkonsoler).  Möjligheten att återge webbläsarsidor är ett krav för att autentisera användare med PDF-filer.


Har Adobe Pass Authentication stöd för de nya standarderna för TV Everywhere? Adobe Pass Authentication är kompatibelt med specifikationen CableLabs OLCA (Online Content Access), som innehåller tekniska krav och arkitektur för leverans av video till en Pay TV-kund från onlinekällor. Adobe deltog i det gemensamma CableLabs-projektet för interopt-testning i juni 2011 och klarade testprocessen för en implementering av en tjänsteleverantör. Adobe Pass-autentisering verifieras (slutförd och testad) mot OLCA-specifikationerna för autentisering. Auktoriseringskomponenten är slutförd, men testverifieringen väntar på att CableLabs-testmiljön ska släppas. Adobe är också en aktiv medlem av OATC (Open Authentication Technical Consortium) och deltar i flera av underkommittéernas projekt för att utarbeta specifikationer som en del av detta organ.



Vad är autentisering? Autentisering är den process där en MVPD bekräftar att en viss användare är en känd kund.



Vad är auktorisering? Behörighet är den process där en MVPD bekräftar att en autentiserad användare har en giltig prenumeration på en viss resurs.



## Arkitektur {#architecture}

Adobe Pass Authentication är en värdtjänst som möjliggör snabb serverintegration (server till server) baserat på de affärsregler som krävs av både programmerare och distributörer. Detta innebär en snabb time to market för alla parter, en säkrare miljö för att förhindra bedrägerier och en överlägsen kundupplevelse, med mer TV-innehåll tillgängligt för fler personer över flera plattformar.


Adobe Pass-autentisering erbjuds via SaaS-modellen (Software as a Service) och möjliggör säkrare kommunikation mellan slutanvändare, distributörer och programmerare för att validera rätten till innehåll. Huvudkomponenterna i tjänsten omfattar följande:

Serversidan - Den värdbaserade Adobe Pass-autentiseringsservern. Det här är en programserver som engagerar i kommunikation bakkanal (server-till-server) med autentiseringssystemen i MVPD-program.
Klientsida:
Åtkomstaktivering på klientsidan - Åtkomstaktiveraren är en liten fil som läses in i programmerarens webbsida eller spelarprogram. Den ger berättigande API:er till programmerarens visningsprogram och kommunicerar med Adobe Pass Authentication Server.
Klientlösa webbtjänster (för icke-webbkompatibla enheter) - RESTful-webbtjänster som tillhandahåller berättigande API:er för enheter som Smart TV, spelkonsoler och digitalboxar.

>[!NOTE]
>
>Som MVPD måste dina webbtjänster kunna känna igen begäranden om autentisering och auktorisering från Adobe Pass-autentisering och svara med nödvändiga data i det förväntade formatet.
>

Med Adobe Pass Authentication kan ni förse kunderna med federerad identitetshantering, som också kallas autentisering och auktorisering för enkel inloggning (SSO). Med Adobe Pass Authentication behöver abonnenterna inte logga in igen efter den första autentiseringen, så länge som autentiseringen tillåts av MVPD. (Vanligtvis 30 dagar.) För att uppnå detta tillhandahåller Adobe Pass Authentication en gemensam domän för autentiseringstoken för våra kunder. Den här informationen om autentiseringstillstånd är tillgänglig för alla deltagande webbplatser som är integrerade med en viss MVPD.


För närvarande använder de flesta Adobe Pass Authentication-integreringar med MVPD-program SAML-protokollet, en av de primära autentiseringsstandarderna. Adobe Pass Authentication fungerar som en proxytjänsteleverantör i SAML-arkitekturen och bibehåller SAML-autentiseringssvaret som en säker token i den gemensamma domänen Adobe. Adobe Pass Authentication är SAML 2.0-kompatibel. Adobe Pass Authentication används för närvarande vanligtvis med SAML SSO-lösningar, men Adobe Pass Authentication-arkitekturen är inte bunden till något specifikt protokoll. Därför kan stöd för nya protokoll, t.ex. ett som är baserat på OAuth 2.0 eller anpassade protokoll, läggas till över tid.


Adobe samarbetar med ett MVPD tekniska team för att konfigurera Adobe Pass Authentication så att det uppfyller alla befintliga integrationers behov. Integreringen är kostnadsfri för distributörer av videoprogrammeringstjänster och förutsätter en&quot;standard&quot;-integrering och minimala supportkrav (dokumentation och grundläggande support via e-post). Om en MVPD kräver omfattande support eller en upptrappad tidslinje kan en supportavgift debiteras, eller så kanske leverantören vill arbeta med en tredje part som är bekant med vår lösning, som Synacor.


Adobe Pass Authentication stöder även effektiv hantering av MVPD affärslogik enligt följande:

För affärslogik som är fristående och som kan tillämpas av MVPD när en begäran om tillstånd tas emot, tillhandahåller Adobe de data som behövs för att stödja logiska funktioner när MVPD tar emot en begäran om tillstånd. Dessa data kan omfatta, men är inte begränsade till, det unika enhets-ID som användaren som gör begäran och enhetens IP-adress har.

För affärslogik som kräver användaråtgärder och/eller specifik hantering av Adobe kan Adobe behålla vissa anpassade egenskaper för varje MVPD. Dessa MVPD-specifika konfigurationer/principer omfattar aktivering av fördefinierade arbetsflöden som kan startas vid specifika punkter i arbetsflödet på den översta nivån. Kontakta din Adobe-representant om du vill ha mer information om stöd för anpassade egenskaper.

I följande diagram visas relationen mellan MVPD och Programmer och dessa Adobe Pass-autentiseringskomponenter:

![](../assets/high-level-architecture-nflows.png)

*Figur: Arkitektur och flöden på hög nivå*

## Adobe Pass-autentiseringskomponenter {#components}

Nedan finns en översikt över några av huvudkomponenterna i Adobe Pass Authentication-ekosystemet. Bland dessa finns:

* [Åtkomstaktivering/klientlösa webbtjänster](#ae)
* [Backend-servern som är värd för Adobe](#backend)
* [Tokens](#tokens)

### Åtkomstaktivering/klientlösa webbtjänster {#ae}

Åtkomstaktiveraren underlättar all autentisering och auktoriseringsinteraktion med användaren och körs lokalt på användarens system. Det är Access Enabler som hanterar de faktiska tillståndsarbetsflödena med MVPD, medan Programmeraren behåller ansvaret för webbsidan eller spelarprogrammet på den högre nivån.

Klientlösa webbtjänster tillhandahålls av Adobe Pass Authentication för enheter som inte kan återge webbsidor.  För dessa enheter initieras tillståndsprocessen och innehåll visas på den smarta enheten medan autentiseringen med en MVPD sker på en webbkompatibel enhet (dator, smartphone och surfplatta).

Åtkomstfunktionen:

* Initierar MVPD-specifika autentiserings- och auktoriseringsarbetsflöden.
* Cachelagrar de lyckade auktoriseringssvaren per programmerarresurs/kanal för att minimera onödig trafik på begäran.
* Kan konfigureras för fördefinierade arbetsflöden som är specifika för varje MVPD, till exempel explicit enhetsregistrering.
* Det finns i följande former:
   * En SWF-fil som Flashen Player kan köra
   * En JS-fil som körs direkt av webbläsaren
   * En inbyggd åtkomstfunktion för olika plattformar, inklusive iOS, Android och Xbox.

### Backend-server som är värd för Adobe {#backend}

Adobe Pass Authentication-serverdelen, som hanteras av Adobe:

* Tillhandahåller autentiserings- och auktoriseringsarbetsflöden med de MVPD-program som kräver server-till-server-kommunikation mellan Adobe Pass Authentication och operatorn.
* Upprätthåller konfigurationen för programmerarplatser och program.
* Värdar för komponentfilerna för åtkomstaktivering.
* Skapar autentiserings- och auktoriseringstoken.

### Tokens {#tokens}

Tillståndslösningen Adobe Pass Authentication är inriktad på att skapa specifika datadelar som hämtas när autentiserings-/auktoriseringsarbetsflödena har slutförts. Dessa datadelar kallas för tokens. De har begränsad livslängd och lagras säkert på plattformsberoende platser. När tokens förfaller måste de utfärdas på nytt genom att autentiserings- och/eller auktoriseringsarbetsflödena återupptas.

Det finns tre typer av tokens som utfärdas under arbetsflödena för autentisering/auktorisering. Två&quot;långlivade&quot; ger kontinuitet i användarens tittarupplevelse. Den tredje, som är en kortlivad variabel, ger stöd för branschens bästa metoder för att minska bedrägerier genom strömrippning. TTL-värdena (time-to-live) för tokens ställs in baserat på avtal mellan MVPD och programmerare. Ni bestämmer er för ett TTL-värde som passar er verksamhet och era kunder bäst.

**Den långvariga autentiseringstoken**. Autentiseringen är klar när en kund har loggat in på sitt MVPD-konto med Adobe Pass-autentisering. Adobe Pass Authentication skapar sedan en token för långvarig autentisering (&quot;authN&quot;) som är kopplad till den begärande enheten och (beroende på MVPD) en globalt unik identifierare (&quot;GUID&quot;) som identifierar användaren anonymt.

**Den långvariga auktoriseringstoken**. När auktoriseringen är klar skapar Adobe Pass Authentication en token för långvarig auktorisering (&quot;authZ&quot;). Denna token är inte portabel eftersom den är kopplad till den begärande enheten och en specifik skyddad resurs (t.ex. en kanal, serie eller avsnitt). Access Enabler använder den långvariga authZ-token för att skapa de kortlivade medietoken som används för visningsåtkomst.

**Den kortlivade medietoken**. När användaren har auktoriserats genererar Adobe Pass Authentication en authZ-token och använder denna token för att generera en kort medietoken som signeras av Adobe och krypteras för att undvika manipulering under utbyte. Eftersom den kortlivade variabeln exponeras för inbäddningsplatsen via antingen API:t för åtkomstaktivering eller klientlösa webbtjänster måste programmerarens medieserver använda en Adobe Pass Authentication-komponent, Media Token Verifier, för att validera variabeln innan de kan komma åt den skyddade resursen.

## MVPD Integration Lifecycle {#lifecycle}

Följande bild visar livscykeln för integreringen mellan Adobe Pass Authentication och en MVPD.

![](../assets/mvpd-int-lifecycle.png)

*Figur: Integreringslivscykel för MVPD*

## Tillståndsflödesschema {#chart}

I följande flödesdiagram visas den övergripande processen för att bekräfta tillstånd med hjälp av Adobe Pass-autentisering:

![](../assets/authn-authz-entitlmnt-flow.png)

*Figur: Processen för att bekräfta tillstånd med Adobe Pass-autentisering*

## Autentiseringssteg {#authn-steps}

I följande steg visas ett exempel på autentiseringsflödet för Adobe Pass-autentisering.  Detta är den del av tillståndsprocessen där en programmerare avgör om användaren är en giltig kund till en MVPD.  I det här scenariot är användaren en giltig prenumerant på en MVPD.  Användaren försöker visa skyddat innehåll med en programmerares program för Flash:

1. Användaren går till programmerarens webbsida, som läser in programmerarens program för Flash och Adobe Pass Authentication Access Enabler-komponenterna på användarens dator. Flashen använder Access Enabler för att ange programmerarens ID med Adobe Pass Authentication, och Adobe Pass Authentication-kontot använder Access Enabler med konfigurations- och tillståndsdata för den programmeraren (&quot;begäraren&quot;). Åtkomstaktiveraren måste ta emot dessa data från servern innan andra API-anrop kan utföras.  Teknisk kommentar: Programmeraren anger sin identitet med åtkomstaktiveringens `setRequestor()`-metod.
1. När användaren försöker visa programmerarens skyddade innehåll, visar programmerarens program användaren en lista med distributörer (MVPD) som användaren väljer en leverantör från.
1. Användaren omdirigeras till en Adobe Pass-autentiseringsserver där en krypterad SAML-begäran för den MVPD som användaren har valt skapas. Denna begäran skickas som en autentiseringsförfrågan från Programmer till MVPD. Beroende på vilket MVPD-system du använder dirigeras användarens webbläsare sedan antingen till MVPD webbplats för inloggning, eller så skapas en iFrame-inloggning i programmerarens app.
1. I båda fallen (omdirigering eller iFrame) godkänner MVPD begäran och visar sin inloggningssida.
1. Användaren loggar in med MVPD, MVPD validerar användarens status som betalande kund och sedan skapar MVPD en egen HTTP-session.
1. När användaren valideras skapar MVPD ett svar (SAML &amp; krypterat) som MVPD skickar tillbaka till Adobe Pass Authentication.
1. Adobe Pass Authentication tar emot MVPD-svar, ser att en Adobe Pass Authentication HTTP-session är öppen, validerar [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)-svaret från MVPD och dirigerar om tillbaka till programmerarens webbplats.
1. Programmerarens webbplats läses in igen, Access Enabler läses in igen och programmeraren anropar setRequestor() igen.  Det andra anropet till setRequestor() är nödvändigt eftersom den aktuella konfigurationen har ändrats - det finns nu en flagga som informerar Access Enabler om att en AuthN-token väntar på att genereras på servern.
1. Åtkomstaktiveraren ser att det finns en väntande autentisering och begär token från Adobe Pass-autentiseringsservern. Token hämtas från servern genom att Flashens Player DRM-funktioner anropas.
1. AuthN-token lagras i programmerarens LSO-cache för Flash Player. Autentiseringen är nu slutförd och sessionen förstörs på Adobe Pass autentiseringsserver.

## Auktoriseringssteg {#authz-steps}

Följande steg fortsätter från föregående avsnitt ([Autentiseringssteg](#authn-steps)):

1. När användaren försöker få åtkomst till programmerarens skyddade innehåll söker programmerarens program först efter en AuthN-token på användarens lokala dator eller enhet.  Om denna token inte finns där följs [autentiseringsstegen](#authn-steps) ovan.  Om AuthN-token finns där, fortsätter auktoriseringsflödet med programmerarens program som startar ett anrop till Access Enabler med en begäran om att få användarens visningsrättigheter för ett visst objekt med skyddat innehåll.
1. Den specifika posten med skyddat innehåll representeras av en &quot;resursidentifierare&quot;.  Detta kan vara en enkel sträng eller en mer komplex struktur, men i vilket fall som helst avtalas om resursidentifierarens typ i förväg mellan Programmeraren och MVPD.  Programmerarens program skickar resursidentifieraren till Access Enabler.  Åtkomstaktiveraren söker efter en AuthZ-token på användarens lokala dator eller enhet.  Om AuthZ-token inte finns där, skickar Access Enabler begäran till Adobe Pass-autentiseringsservern i serverdelen.
1. Adobe Pass-autentiseringsservern kommunicerar med MVPD-auktoriseringsslutpunkten med hjälp av standardiserade protokoll.  Om MVPD svar indikerar att användaren har rätt att visa det skyddade innehållet, skapar Adobe Pass Authentication-servern en AuthZ-token och skickar tillbaka den till Access Enabler, som lagrar AuthZ-token på användarens dator.
1. Med en AuthZ-token lagrad på användarens dator eller enhet anropar programmerarens program Access Enabler för att erhålla en medietoken från Adobe Pass Authentication-servern och tillhandahåller denna token till programmerarens program.
1. Slutligen använder programmerarens program Media Token Verifier-komponenten för att bekräfta att rätt användare visar rätt innehåll, och med Media Token på plats tillåts användaren att visa det skyddade innehållet.

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
