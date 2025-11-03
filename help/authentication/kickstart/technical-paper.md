---
title: Om Adobe Pass-autentisering
description: Om Adobe Pass-autentisering
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 7ca9d8996756086a6b963c0b6d5b0bb64608ecbc
workflow-type: tm+mt
source-wordcount: '1828'
ht-degree: 0%

---

# Om Adobe® Pass Authentication {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Om TV Everywhere {#about-tv-everywhere}

Dagens TV-tittare förväntar sig smidig åtkomst till Pay TV-innehåll när som helst, var som helst. I och med uppkopplingen till Internet konsumerar målgrupperna innehåll på ett växande antal plattformar, bland annat:

* Bärbara
* Surfplattor
* Smartphones
* Webbplatser
* Federerade appar
* Spelkonsoler
* Rutor
* Smarta TV-apparater

TV Everywhere är ett branschinitiativ som ser till att Pay TV-prenumeranter får tillgång till det innehåll de redan betalar för via flera enheter, både i och utanför sina hem.

Traditionell (linjär) TV är fortfarande stark, men de snabbast växande segmenten av videokonsumtion är tidförskjutet innehåll, onlineströmning och alternativa skärmar. Det här skiftet har stört videoutdelningsmarknaden och gjort TV Everywhere till en avgörande lösning som anpassar intressena hos **programmerare, betal-TV-leverantörer och prenumeranter.**

### Mål för TV Everywhere {#goals-tv-everywhere}

**Tekniskt mål**

* Gör det möjligt för Pay TV-kunder att få tillgång till sitt prenumererade innehåll sömlöst på alla enheter och plattformar.

**Affärsmål**

* Bevara och stärka befintliga kundrelationer samtidigt som ni skapar nya möjligheter.
* Gör det möjligt för programmerare och innehållsägare att nå en bredare publik och maximera värdet av premiuminnehåll.
* Bygg ut varumärken genom direktkontakt online med tittarna.

### Problem med TV överallt {#challenges-tv-everywhere}

Med möjligheterna med TV Everywhere kommer stora utmaningar och berättigandet är det mest kritiska. Innan ett visningsprogram kan komma åt prenumerationsinnehåll måste systemet verifiera deras behörighet.

Viktiga frågor:

* Har användaren en aktiv prenumeration hos en Pay TV-leverantör?
* Inkluderar deras prenumeration det begärda innehållet?

Det är en särskilt stor utmaning för programmerare och innehållsägare att avgöra rätten eftersom Pay TV-operatörer kontrollerar kunddata och behörigheter.

Förutom rätt till uppgradering finns det flera tekniska problem och integreringsproblem, bland annat:

* Utveckla en strategi för flera enheter som ger smidig åtkomst.
* Hantera komplexa relationer mellan programmerare och leverantörer av betal-TV.
* Förhindra bedräglig åtkomst och användning av användarvillkoren.
* Leverera en enhetlig, användarvänlig autentiseringsprocess för olika webbplatser och appar.
* Upprätthålla snabb time-to-market för att anpassa sig till filialavtal.
* Kontrollera kostnaderna för olika integreringar.

Dessa utmaningar gör att direkt integrering mellan programmerare och flera autentiseringssystem för Pay TV blir mycket resurskrävande, vilket kräver både tid och teknisk expertis.

**Adobe® Pass Authentication** förenklar och effektiviserar verifieringen av tillstånd och ger smidig och säker åtkomst till TV Everywhere-innehåll.

## Introduktion till Adobe Pass-autentisering {#introduction-adobe-pass-authentication}

Adobe Pass Authentication förmedlar på ett säkert sätt berättigandetransaktioner mellan programmerare och betal-TV-leverantörer, vilket säkerställer att rätt kunder enkelt kan komma åt rätt innehåll.

![](/help/authentication/assets/programmers-connect-authn.png)

*Några av programmerarna och betal-tv-leverantörerna som ansluter via Adobe Pass-autentisering*

**Vem har nytta av Adobe Pass-autentisering**

* **Programmerare**

  som enkelt vill kunna integreras med leverantörer av betal-TV, även kallade distributörer av flerkanalsvideo, för att nå största möjliga publik och optimera intäkterna.

* **Betala TV-leverantörer (MVPD)**

  som vill ha kontakt med flera olika innehållsägare (programmerare) genom en enda integrering, vilket ger nöjdare kunder genom att göra det enklare att få tillgång till prenumerationsinnehåll online.

* **Betala TV-kunder**

  Vem som vill se det innehåll de redan betalar för när som helst, var som helst, på vilken enhet som helst.

**Programmerare**

Programmerare som vill maximera målgruppens räckvidd och intäkterna kan

* Integrera enkelt med de främsta leverantörerna av betal-TV utan att behöva hantera flera direktanslutningar.
* Utvidga målgruppen genom att säkerställa autentisering för alla större leverantörer och plattformar.
* Skydda premiuminnehåll med säker autentisering, begränsa åtkomsten till behöriga användare och enheter.
* Aktivera enkel inloggning (SSO) för att förbättra användarupplevelsen mellan appar och webbplatser.

**Betala TV-leverantörer (MVPD)**

Betala TV-leverantörer kan förbättra kundupplevelsen och effektivisera verksamheten genom att:

* Skapa kontakter med flera innehållsägare genom en enda integrering.
* En varumärkesanpassad, smidig upplevelse på olika enheter och plattformar.
* Säker autentisering för att förhindra obehörig åtkomst och hantera samtidiga strömmar per hushåll.

**Betala TV-kunder**

Prenumeranter har nytta av TV Everywhere, så att de kan se det innehåll de redan betalar för när som helst, var som helst, på vilken enhet som helst.

### Integrera med Adobe Pass-autentisering {#integrating-adobe-pass-authentication}

Vare sig du är leverantör av betal-TV eller programmerare kräver integrering med Adobe Pass Authentication ett aktivt deltagande. Nedan visas en översikt över processen för båda rollerna.

#### Integreringsprocess för programmerare {#programmer-integration-process}

Ytterligare vägledning finns när integreringen har initierats formellt, men processen omfattar vanligtvis följande steg:

**Förutsättningar**

* Ett innehållshanteringssystem (CMS).
* En innehållsleveransmekanism, som kan omfatta ett tredjepartsnätverk för innehållsleverans (CDN).
* En befintlig videoplattform online med en mediespelare som antingen är inbäddad på en webbplats eller i ett fristående program.

**Integreringsaktiviteter**

* Integrera Adobe Pass-autentiseringen [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).
* Integrera Adobe Pass-autentiseringen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md).
* Integrera Adobe Pass-autentiseringen [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).
* Utveckla ett användargränssnitt för autentisering, auktorisering och utloggning.

Mer information om programvaruintegreringsprocessen finns i [Programmerarens kickstart](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md) -dokument.

#### Integreringsprocess för betal-TV-leverantör {#pay-tv-provider-integration-process}

Ytterligare vägledning finns när integreringen har initierats formellt, men processen omfattar vanligtvis följande steg:

**Förutsättningar**

* Underteckna Adobe Pass NDA (Authentication Non-Disclosure Agreement).
* Ge Adobe Pass Authentication Engineering Team specifikationer för autentiserings- och auktoriseringssystemet. En SAML-baserad identitetsleverantör (IdP) för autentisering och ett SOAP-baserat auktoriseringssystem rekommenderas för den enklaste integrationen.

**Integreringsaktiviteter**

* Upprätta anslutningsmöjligheter med Adobe Pass autentiseringsservrar.
* Fullständig mellanlagringsrelease och säkerställ kvalitetssäkring.
* Fullständig produktionsrelease och kvalitetssäkring.

Standardintegrering är kostnadsfri för leverantörer av betal-TV, inklusive dokumentation och grundläggande e-postsupport. Leverantörer som behöver omfattande assistans eller kortare tidslinjer kan dock ådra sig en supportavgift eller välja att arbeta med en erfaren tredjepartspartner, som Synacor.

Adobe Pass Authentication kan effektivt stödja affärslogik som är specifik för betal-tv-leverantörer på två viktiga sätt:

* För affärslogik som är självständig och som tillämpas av Pay TV-leverantören när en auktoriseringsbegäran tas emot, tillhandahåller Adobe nödvändiga data (t.ex. unikt enhets-ID, IP-adress) för att stödja användningen.
* För affärslogik som kräver användaringripande eller specifik hantering av Adobe kan anpassade egenskaper bevaras för varje Pay TV-leverantör. Dessa konfigurationer kan innehålla fördefinierade arbetsflöden som aktiveras vid specifika punkter i autentiseringsprocessen.

Mer information om hur du integrerar betal-tv-leverantörer finns i [MVPD kickstart-guiden](/help/authentication/kickstart/mvpd-kickstart-guide.md) och [MVPD integreringsguide](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md) -dokumenten.

### Tillståndsflöde {#entitlement-flow}

Adobe Pass Authentication fungerar som en proxy och underlättar tillståndsflödet mellan programmerare och distributörer genom att erbjuda säkra och enhetliga gränssnitt för båda parter.

För programmerare tillhandahåller Adobe Pass Authentication API:er som en del av en **Standard**- eller en **Premium**-nivå:

* Adobe Pass standard-API för autentisering:
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium-API:er för Adobe Pass-autentisering:
   * [Återställ API för tillfälligt pass](/help/premium-workflow/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass-funktion](/help/premium-workflow/temporary-access/temp-pass-feature.md)
   * [Försämring av API](/help/premium-workflow/degraded-access/degradation-feature.md#degradation-api-access)
      * [Försämringsfunktion](/help/premium-workflow/degraded-access/degradation-feature.md)
   * [API för tillståndsövervaknings-API](/help/premium-workflow/esm/entitlement-service-monitoring-api.md)

Mer information om berättigandeflödet finns i dokumentationen till [Programmeringsintegreringshandboken](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow).

#### Förstå berättiganden {#understanding-entitlements}

Adobe Pass lösning för autentisering handlar om att skapa berättiganden - specifika datadelar som genereras när autentiserings- och auktoriseringsarbetsflödena har slutförts. Dessa rättigheter ger åtkomst till skyddat innehåll men har en begränsad livslängd. När ett berättigande upphör att gälla måste det förnyas genom att autentiserings- eller auktoriseringsprocesserna återupptas.

Mer information om berättiganden finns i följande dokument:

* **Profiler**

  När autentiseringen är klar skapar Adobe Pass Authentication en autentiserad profil (&quot;långvarig&quot;) som är kopplad till det program, den enhet och den tjänsteleverantörsidentifierare som begär autentisering (begärande-ID).

* **[Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  När autentiseringen är klar (och i vissa fall även efter behörigheten) får Adobe Pass Authentication in användarmetadata från MVPD som kan visa dem för det begärande programmet.

* **[Beslut](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  När auktoriseringen är klar skapar Adobe Pass Authentication ett auktoriseringsbeslut (&quot;långlivad&quot;) som är kopplat till det begärande programmet, enheten, tjänstleverantörens identifierare (beställarens identifierare) och en specifik skyddad resurs (resursidentifierare).

* **[Medietoken](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  När auktoriseringen är klar skapar Adobe Pass Authentication en medietoken (&quot;kortlivad&quot;) som är associerad med en lyckad spelbegäran och ger stöd för branschens bästa metoder för att minska bedrägerier (t.ex. direktuppspelning).

TTL-värdena (time-to-live) för profiler och beslut baseras på avtal mellan programmerare och leverantörer av betal-TV, som är överens om ett värde som passar alla bäst.

#### Skapa användargränssnittet {#building-user-interface}

Programmerarna ansvarar för att utforma och implementera användargränssnittet för berättigandearbetsflödet på sin webbplats eller i sin app, medan vissa element, som inloggningsprocessen, hanteras av Pay TV-leverantören.

Programmerarna måste åtminstone

* **Implementera ett gränssnitt för val av leverantör**
   * Tillåt nya användare att identifiera sin Pay TV-leverantör och logga in för första gången.
   * En del leverantörer av betal-TV dirigerar om användare till en extern inloggningssida, medan andra kräver inloggning inom en iframe. Programmerare måste implementera en callback-funktion för att skapa iframe-elementet vid behov.

* **Hantera en lista över Betal-TV-leverantörer som stöds**
   * Se till att användarna bara har åtkomst till innehåll via godkända leverantörer.

* **Ange autentiseringsstatus**
   * Visa när en användare autentiseras i appen eller på webbplatsen.

* **Identifiera skyddade resurser**
   * Ange tydligt vilket innehåll som kräver behörighet innan det visas.
   * Uppdatera användargränssnittet så att det återspeglar auktoriseringen när åtkomst beviljas.

## Vanliga frågor {#faqs}

**Vad är TV Everywhere?**

TV Everywhere är ett branschinitiativ som ger Pay TV-kunder tillgång till det premiuminnehåll de redan prenumererar på på olika internetanslutna enheter. Detta omfattar persondatorer, surfplattor, smarttelefoner, spelkonsoler, digitalboxar och smarta tv-apparater. Den största utmaningen är att säkerställa en smidig och användarvänlig autentiseringsprocess, så att kunderna kan få tillgång till sitt prenumerationsmaterial utan flera inloggningar eller tekniska hinder.

**Vad är Adobe Pass-autentisering och hur stöder den TV Everywhere?**

Adobe Pass Authentication ger TV Everywhere liv genom att på ett säkert sätt bekräfta användarens rätt till innehåll på ett enkelt och effektivt sätt. Det är en värdtjänst som underlättar snabb backend-integrering baserat på affärsregler som fastställts av både programmerare och betal-TV-leverantörer. Detta ger snabbare time-to-market för alla intressenter, en säkrare miljö som minimerar bedrägerier och en bättre användarupplevelse med mer TV-innehåll tillgängligt över flera plattformar.

**Hur levereras Adobe Pass-autentisering?**

Adobe Pass Authentication tillhandahålls som en SaaS-lösning (Software as a Service). Detta tillvägagångssätt garanterar säker kommunikation mellan slutanvändare, programmerare och leverantörer av betal-TV för validering av innehållsberättigande.

**Vad skiljer Adobe Pass-autentisering från andra TV Everywhere-lösningar?**

Adobe Pass Authentication ger flera fördelar jämfört med alternativa lösningar:

* **Sömlös enkel inloggning (SSO)** - Till skillnad från direktintegrering med enskilda leverantörer möjliggör Adobe Pass-autentisering en beständig inloggning när användare förflyttar sig mellan olika webbplatser och appar.
* **Broad Market Penetration** - När en programmerare har integrerats med Adobe Pass Authentication får de omedelbart tillgång till Pay TV-operatörer som täcker över 90 % av hushållen i USA.
* **Integrering med Adobe ekosystem** - Fungerar sömlöst med andra Adobe-lösningar för innehållsleverans, skydd och intäktsgenerering, inklusive Adobe Analytics.

**Hur säker är Adobe Pass-autentiseringen?**

Säkerhet är högsta prioritet. Adobe Pass Authentication säkerställer att endast behöriga användare kan komma åt premiuminnehåll genom att binda åtkomsten till användarens enhet. Det innehåller även alternativ för att begränsa antalet samtidiga strömmar, sessioner eller enheter per hushåll.

**Vilka enheter stöder Adobe Pass-autentisering?**

Adobe Pass Authentication är utformat för att fungera på en rad olika enheter, t.ex. webbaserade enheter, mobilenheter eller tv-anslutna enheter.

**Kostar Adobe Pass-autentisering något för slutanvändare?**

Nej. Adobe Pass Authentication är kostnadsfritt för slutanvändare.
