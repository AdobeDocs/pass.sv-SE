---
title: Introduktion till övervakning av samtidig användning
description: Introduktion till övervakning av samtidig användning
exl-id: 725cc64b-6b03-46e3-a038-41e9b1341c6b
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Introduktion till övervakning av samtidig användning {#intro}

Övervakning av samtidig användning (Concurrency Monitoring) är en tjänst som gör det möjligt för innehållsleverantörer och identitetsleverantörer (MVPD och Programmers) att definiera och tillämpa begränsningar för samtidig videoströmning för flera olika applikationer, enheter och plattformar. Vare sig du är programmerare och vill styra hur många strömmar en prenumerant kan titta på samtidigt, eller en MVPD som vill genomdriva användarprofiler hos alla era innehållspartners, har Concurrency Monitoring de verktyg du behöver.

## Vad är övervakning av samtidig användning? {#what-is-cm}

Övervakning av samtidig användning är en centraliserad tjänst som spårar och hanterar aktiva videoströmningssessioner i realtid. Du kan:

- **Begränsa samtidiga strömmar per prenumerant** - Kontrollera hur många samtidiga videoströmmar en användare har åtkomst till
- **Använd enhetsbegränsningar** - Begränsa direktuppspelning till specifika enhetstyper eller kvantiteter
- **Implementera platsbaserade principer** - Begränsa direktuppspelning baserat på geografisk plats
- **Skapa innehållsspecifika regler** - Använd andra begränsningar för direktsänt och VOD-innehåll
- **Övervaka användningsmönster** - Få insikter om hur ditt innehåll konsumeras

## Så här fungerar det {#how-it-works}

Övervakning av samtidig användning sker via ett enkelt men kraftfullt API:

1. **Sessionsinitiering** - När en användare börjar titta på innehåll skapas en session i programmet
2. **Principutvärdering** - Tjänsten utvärderar dina definierade principer mot aktuell användning
3. **Övervakning i realtid** - pulsslagsanrop håller sessionerna aktiva och övervakar regelefterlevnaden

## Har du inte använt Concurrency Monitoring tidigare? {#new-to-cm}

Börja med vår [Komma igång-guide](getting-started/getting-started-overview.md) för att förstå grunderna och konfigurera din första integrering.

