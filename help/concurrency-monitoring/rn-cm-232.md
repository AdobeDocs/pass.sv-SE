---
title: Versionsinformation om Adobe Pass Concurrency Monitoring 2.3.2
description: Versionsinformation om Adobe Pass Concurrency Monitoring 2.3.2
exl-id: 3996da45-498c-482a-b374-3cda1c5df2f7
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 1%

---

# Versionsinformation om Adobe Pass Concurrency Monitoring 2.3.2 {#cm-232}

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

Releasedatum: 2015-12-11

## Nya funktioner och förbättringar {#new-features}

* Nya uppdelningar finns i användningsrapporter. De nya uppdelningarna är tillgängliga om programmet som är integrerat med Concurrency Monitoring skickar anpassade metadata.
   * program - det program-ID som rapporteras i anrops-URL
   * mvpd - den MVPD som rapporteras i call URL
   * kanal - den anpassade metadatakanalen
   * platform - det anpassade metadataprogrammetPlatform
* Nya mått relaterade till **dataströmmens varaktighet** finns tillgängliga i användningsrapporterna. De nya mätvärdena kan användas för att skapa ett histogram med flödeslängd. Följande intervaller i minuter är tillgängliga:
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## Felkorrigeringar {#bug-fixes}

Ej tillämpligt

## Kända fel {#known-issues}

Ej tillämpligt
