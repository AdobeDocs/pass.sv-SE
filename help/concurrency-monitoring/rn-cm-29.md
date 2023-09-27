---
title: Adobe Concurrency Monitoring 2.9 Release Notes
description: Adobe Concurrency Monitoring 2.9 Release Notes
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Samtidighetsövervakning 2.9 versionsinformation {#rn-cm29}

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen.

## Releasedatum {#release-date}

03/14/2019


## Versionsöversikt {#release-overview}

* Från och med den här versionen har vi tagit fram en ny rapport för förståelse av samtidig användning: ett histogram för samtidighetsnivå, som visar:

* antalet användare som har nått varje samtidighetsnivå (dvs. hur många användare som någonsin haft 2 samtidiga strömmar, 3 samtidiga strömmar osv.) under varje granularitetsintervall
* den totala varaktigheten för varje samtidighetsnivå, i minuter (det genomsnittliga värdet kan beräknas genom att helt enkelt dividera värdet med ovanstående antal)
* det totala antalet gånger som användare har påträffat varje samtidighetsnivå, för att uppskatta effekten av en viss regel både vad gäller påverkade användare och sammanställd användarupplevelse Mer information finns på [Användningsrapporter](/help/concurrency-monitoring/cm-usage-reports.md) sida.

Vi har även förbättrat SQL-injektionsskyddet och lagt till flera felkorrigeringar.

## Kända fel {#known-issues}

Ingen.
