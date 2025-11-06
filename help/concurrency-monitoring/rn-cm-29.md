---
title: Versionsinformation om Adobe Concurrency Monitoring 2.9
description: Versionsinformation om Adobe Concurrency Monitoring 2.9
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---

# Samtidighetsövervakning 2.9 versionsinformation {#rn-cm29}

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen.

## Releasedatum {#release-date}

03/14/2019


## Versionsöversikt {#release-overview}

* Från och med den här versionen har vi tagit fram en ny rapport för förståelse av samtidig användning: ett histogram för samtidighetsnivå, som visar:

* antalet användare som har nått varje samtidighetsnivå (dvs. hur många användare som någonsin haft 2 samtidiga strömmar, 3 samtidiga strömmar osv.) under varje granularitetsintervall
* den totala varaktigheten för varje samtidighetsnivå, i minuter (det genomsnittliga värdet kan beräknas genom att helt enkelt dividera värdet med ovanstående antal)
* det totala antalet gånger som användare har påträffat varje samtidighetsnivå, för att uppskatta effekten av en viss regel både vad gäller påverkade användare och sammanställd användarupplevelse
Mer information finns på sidan [Användningsrapporter](/help/concurrency-monitoring/cm-usage-reports.md).

Vi har även förbättrat SQL-injektionsskyddet och lagt till flera felkorrigeringar.

## Kända fel {#known-issues}

Ingen.
