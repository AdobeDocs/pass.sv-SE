---
title: Integreringsguide för MVPD
description: Integreringsguide för MVPD
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 0%

---

# Integreringsguide för MVPD {#mvpd-integration-guide}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här integreringsguiden är avsedd för distributörer av flerkanalsvideo (MVPD) som planerar att integrera med Adobe® Pass Authentication.

TV Everywhere (TVE) är ett transformativt initiativ inom Pay TV-branschen, som gör det möjligt för abonnenterna att få tillgång till det innehåll de redan betalar för via flera enheter, både hemma och på resande fot. För leverantörer av betal-TV erbjuder TVE betydande möjligheter, bland annat att stärka befintliga kundrelationer och öppna dörrar för nya. Dessa möjligheter har dock utmaningar.

I TVE-ekosystemet tillhandahåller **Programmerare** innehållet, medan **MVPDs** (Multichannel Video Programming Distributors) hanterar kunddata som behövs för att verifiera om tittarna är berättigade prenumeranter. Samordning av autentisering och behörighet med en enda programmerare kan vara hanterbart, men det med dussintals eller till och med hundratals programmerare ger en avsevärd komplexitet.

Det är här som **Adobe® Pass Authentication** förenklar processen. MVPD behöver bara implementera en enda smidig integrering med Adobe Pass för att få tillgång till hela TVE-ekosystemet. Det tillhandahållna integreringsramverket snabbar upp time-to-market, ger en säker miljö för att minska bedrägerier och förbättrar kundupplevelsen genom att leverera mer TV-innehåll på flera plattformar.

## Adobe Pass Authentication for TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

Adobe Pass Authentication fungerar som en SaaS-lösning (Software as a Service) som är utformad för snabb serverintegration (server-till-server), enligt affärsreglerna för både Multichannel Video Programming Distributors (MVPD) och Programmers.

### Standarder och protokoll {#standards-protocols}

Adobe Pass Authentication är helt i linje med de nya standarderna och protokollen för TV Everywhere (TVE), som stöder smidig integrering och regelefterlevnad i hela ekosystemet.

* **CableLabs OLCA-specifikation (Online Content Access)**\
  Adobe Pass Authentication följer CableLabs OLCA-specifikationen, som definierar de tekniska kraven och arkitekturen för att leverera videoinnehåll från onlinekällor till Pay TV-kunder. Adobe deltog aktivt i CableLabs-projektet för interoperabilitetstestning i juni 2011 och lyckades framgångsrikt genomföra testningsprocessen för en tjänsteleverantörsimplementering.

Adobe Pass Authentication har utformats för att stödja flera protokoll (t.ex. SAML, OAuth 2.0 osv.), som möjliggör framtida utbyggnadsmöjligheter, inklusive anpassade protokoll, baserat på behoven under utvecklingen.

De flesta integreringar använder SAML-protokollet (Security Assertion Markup Language), en primär standard för autentisering. Adobe Pass Authentication fungerar som en proxytjänsteleverantör i SAML-ramverket och behåller SAML-autentiseringssvaret som en säker token i den gemensamma domänen Adobe.

Sammanfattningsvis är Adobe Pass Authentication protokollagnostiker, som är utformade för att vara nära anpassade till OLCA-standarder. Dessa standarder utgör en gemensam ram för programmerare, distributörer av videoprogrammeringstjänster och tjänsteleverantörer och säkerställer en sammanhängande strategi för att implementera TVE-funktioner.

### Integration och support {#integration-support}

Adobe Pass Authentication samarbetar med MVPD tekniska team för att konfigurera integreringar som är skräddarsydda efter deras specifika behov, enligt följande:

* **Standardintegreringar**\
  Erbjuds utan extra kostnad, inklusive dokumentation och grundläggande e-postsupport.

* **Förbättrat stöd**\
  För större anpassningar eller kortare tidslinjer kan en supportavgift tillkomma.

Adobe Pass Authentication stöder effektiv hantering av MVPD affärslogik enligt följande:

* **Självständig affärslogik**\
  För affärslogik som genomdrivs helt av MVPD vid auktoriseringsbegäranden tillhandahåller Adobe nödvändiga data. Dessa data kan omfatta, men är inte begränsade till, det unika enhets-ID:t och IP-adressen för den användarens enhet som begär detta.

* **Stöd för anpassade egenskaper**\
  För affärslogik som kräver användaringripande eller Adobe-specifik hantering kan Adobe upprätthålla anpassade konfigurationer för varje MVPD. Dessa profiler möjliggör fördefinierade arbetsflöden som kan aktiveras vid specifika punkter i berättigandearbetsflödet. Kontakta din Adobe-representant om du vill ha mer information.

## Tillståndsflöde {#entitlement-flow}

Berättigandeflödet är en serie steg som ett programmeringsprogram (TVE) måste slutföra för att strömma skyddat innehåll. Flödet innehåller flera faser som inbegriper interaktion med MVPD:

* [Autentiseringsfas](#authentication-phase)
* (Valfritt) Förhandsauktoriseringsfas
* [Auktoriseringsfas](#authorization-phase)
* Utloggningsfas

Vid en användares första besök i ett program (TVE) följer tillståndsflödet den angivna ordningen. Vid efterföljande besök kan programmet dock kringgå vissa steg baserat på autentiseringsstatus och tillämpliga visningsregler.

>[!NOTE]
>
> Programmeringsprogram (TVE) används i det här dokumentet för att tillsammans hänvisa till de typer av program som körs på olika plattformar (webbläsare, mobila enheter, tv-anslutna enheter osv.) som stöds av Adobe Pass Authentication.

### Autentiseringsfas {#authentication-phase}

Följande steg beskriver de övergripande stegen i händelse av en SAML-integrering:

1. **Programmerarens programinläsning (webbplats)**\
   Användaren navigerar till programmerarens program (webbplats), som integrerar Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Begäran om skyddat innehåll**\
   När användaren försöker få åtkomst till skyddat innehåll visas en lista med MVPD:er som användaren kan välja mellan.

1. **Initiering av autentiseringsbegäran**\
   När MVPD har valts omdirigeras användaren till en Adobe Pass-autentiseringsserver. Här genereras en krypterad SAML-autentiseringsbegäran för den valda MVPD om en SAML-integrering används. Denna begäran skickas för programmerarens räkning till MVPD. Beroende på vilket MVPD-system som används, dirigeras användarens webbläsare antingen till inloggningssidan för MVPD eller så bäddas en iFrame-inloggningssida in i programmerarens program.

1. **MVPD-inloggning**\
   MVPD godkänner begäran och presenterar inloggningsgränssnittet, antingen via redirect eller iFrame.

1. **Användarinloggning och validering**\
   Användaren loggar in med sina MVPD-uppgifter. MVPD validerar användarens prenumerationsstatus och skapar en egen HTTP-session.

1. **MVPD-svar på Adobe Pass-autentisering**\
   När valideringen är klar genererar MVPD ett SAML-svar (krypterat) och skickar tillbaka det till Adobe Pass Authentication.

1. **Profilgenerering**\
   Adobe Pass Authentication verifierar SAML-svaret, genererar en användarprofil som cachelagras och dirigerar om användaren tillbaka till programmerarens program (webbplats).

### Auktoriseringsfas {#authorization-phase}

**Steg på hög nivå**

I följande steg beskrivs de högnivåsteg:

1. **Hantering av resursidentifierare**\
   Det skyddade innehållet identifieras av en [resursidentifierare](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) som kan vara en enkel sträng eller en mer komplex struktur. Identifieraren är fördefinierad och överenskommen av Programmer och MVPD. Programmerarens program skickar resursidentifieraren till Adobe Pass-autentiseringen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **MVPD Authorization Check**\
   Adobe Pass autentiseringsserver kommunicerar med MVPD autentiseringsslutpunkt med hjälp av standardiserade protokoll.

1. **MVPD-svar på Adobe Pass-autentisering**\
   När valideringen är klar bekräftar MVPD att användaren har rätt (eller inte) att få tillgång till innehållet och skickar ett svar tillbaka till Adobe Pass Authentication.

1. **Generering av beslut och medietoken**\
   Adobe Pass Authentication verifierar svaret, genererar ett [beslut](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) som cachelagras och returnerar beslutet som innehåller en medietoken till programmerarens program (webbplats).

1. **Verifiering av innehållsåtkomst**\
   Programmerarens program använder [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) för att bekräfta att rätt användare använder rätt innehåll. När den har validerats får användaren åtkomst att visa det skyddade innehållet.

## Förstå berättiganden {#understanding-entitlements}

Adobe Pass lösning för autentisering handlar om att skapa berättiganden - specifika datadelar som genereras när autentiserings- och auktoriseringsarbetsflödena har slutförts. Dessa rättigheter ger åtkomst till skyddat innehåll men har en begränsad livslängd. När ett berättigande upphör att gälla måste det förnyas genom att autentiserings- eller auktoriseringsprocesserna återupptas.

Mer information om berättiganden finns i följande dokument:

* **Profiler**

  När autentiseringen är klar skapar Adobe Pass Authentication en autentiserad profil (&quot;långvarig&quot;) som är kopplad till det program, den enhet och den tjänsteleverantörsidentifierare som begär autentisering (begärande-ID).

* **[Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  När autentiseringen är klar (och i vissa fall även efter behörigheten) får Adobe Pass Authentication in användarmetadata från MVPD som kan visa dem för det begärande programmet.

* **[Beslut](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  När auktoriseringen är klar skapar Adobe Pass Authentication ett auktoriseringsbeslut (&quot;långlivad&quot;) som är kopplat till det begärande programmet, enheten, tjänstleverantörens identifierare (beställarens identifierare) och en specifik skyddad resurs (resursidentifierare).

* **[Medietoken](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  När auktoriseringen är klar skapar Adobe Pass Authentication en medietoken (&quot;kortlivad&quot;) som är associerad med en begäran om att spela upp.
