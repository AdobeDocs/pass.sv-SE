---
title: Komma igång med övervakning av samtidig användning
description: Lär dig grunderna i övervakning av samtidig användning och hur du kommer igång med din integrering
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Komma igång med övervakning av samtidig användning {#getting-started-overview}

Välkommen till övervakning av samtidighet! Den här guiden hjälper dig att förstå grunderna och snabbt komma igång med integrationen.

## Problemövervakning löser problemet {#problem-solved}

I dagens strömmande landskap förväntar sig abonnenterna att få tillgång till innehåll på flera enheter och i flera program. Detta skapar dock problem för innehållsleverantörer:

- **Innehållslicensavtal** begränsar ofta antalet samtidiga strömmar
- **Bandbreddskostnaderna** ökar med för stor samtidig användning
- **Användarupplevelsen** påverkas när för många strömmar försämrar prestanda
- **Intäktsskydd** förhindrar missbruk av kontodelning

Övervakning av samtidig användning (Concurrency Monitoring) är en centraliserad lösning på dessa problem.


## Hur övervakning av samtidig användning fungerar

### Kärnfunktioner

Övervakning av samtidig användning fungerar som en **sessionshanteringstjänst** som:

1. **Spårar aktiva direktuppspelningssessioner** i realtid i alla program
2. **Utvärderar affärsprofiler** mot aktuella användningsmönster
3. **Tvingar fram gränser** när principer överträds
4. **Tillhandahåller lösningsalternativ** när konflikter uppstår

## Fördelar för olika intressenter

### Innehållsleverantörer (programmerare)

- **Skydda innehållsrättigheter** och följ licensavtal
- **Optimera bandbreddskostnaderna** genom att förhindra för stor användning
- **Förbättra användarupplevelsen** med tydlig feedback om begränsningar
- **Få användningsinsikter** genom detaljerad rapportering

### Identitetsleverantörer (MVPD)

- **Tvinga prenumerationsavtal** för alla innehållspartners
- **Centraliserad principhantering** för flera program
- **Realtidsövervakning** av användningsmönster
- **Skalbar arkitektur** som hanterar miljontals sessioner

### Slutanvändare

- **Tydlig förståelse** av användningsgränserna
- **Enhetlig upplevelse** i alla program
- **Alternativ för att lösa konflikter** när gränser nås
- **Genomskinlig feedback** om varför åtkomsten är begränsad


## Snabbstartbana {#quick-start-path}

1. **Granska viktiga begrepp** - Lär dig mer om [sessioner, principer och metadata](key-concepts.md)
2. **Välj strategi** - Granska [LIFO jämfört med FIFO-strategier](../use-cases/lifo-fifo-strategies.md)
3. **Börja implementera** - Följ [API-referensen](../api/api-reference-overview.md)

## Är du redo att integrera? {#ready-to-integrate}

- [API-referens](../api/api-reference-overview.md)
- [Användningsexempel](../use-cases/cm-use-cases.md)


## Tjänstregistrering {#service-registration}

Kontakta vårt [supportteam](mailto:tve-support@adobe.com) med följande information för att komma igång med övervakning av samtidig användning:

1. **Företagsnamn** och kontaktinformation
2. **Program** som du vill integrera med övervakning av samtidig användning. För varje ansökan, ange:
   1. Programnamn
   2. Programplattform(er)
3. **Integrationspartner** (om du prenumererar på Concurrency Monitoring på begäran av en annan part, programmerare eller MVPD)


## Behöver du hjälp? {#need-help}

- **API-utforskaren** - Testa API:er interaktivt i [Växlargränssnittet](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
- **Nyckeltermer och definitioner** - [Ordlista](../cm-glossary.md)
- **Hur får jag hjälp?** - [Supportprocedurer](../support/cm-escalation-procedures.md)
- **Support** - kontakta [tve-support@adobe.com](mailto:tve-support@adobe.com)
