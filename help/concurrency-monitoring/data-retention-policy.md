---
title: Datalagringspolicy
description: Datalagringspolicy
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Datalagringspolicy {#data-retention-policy}

>[!WARNING]
>
>**Obs!** Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.


## Introduktion {#introduction}

Adobe måste i sin roll som personuppgiftsbiträde vidta lämpliga åtgärder för att hjälpa sina kunder med att uppfylla åtkomst, borttagning och andra förfrågningar från enskilda personer. Att tillämpa lämpliga, säkra och vältajmade principer för borttagning är en viktig del av att uppfylla denna skyldighet.

## Definitioner {#definitions}

En datalagringspolicy avgör hur länge Adobe lagrar kunddata. Standarddatalagringsprincipen för övervakning av samtidig användning är **25 månader**.

| Datalagringsperiod | Datalagringsperioden är standardperioden för datalagring (25 månader). |
|---|---|
| **Fönstret Datalagring** | I fönstret för datalagring definieras parametrarna för vilka fullständiga data kan visas och rapporteras. Datalagringsfönstret bestäms enligt följande:<br/> *Startdatum* = aktuellt datum - datalagringsperiod <br/>*Slutdatum* = aktuellt datum |

## Datainsamling {#data-collection}

*Clickstream-data* representerar data som delas av kunder i sessionens pulsslag (till exempel subjectID, mvpdName och metadata). Alla anpassade metadatafält refereras i [standardmetadataattributen](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Kundtyper {#customer-types}

### Nuvarande kunder {#current-customers}

Såvida kunden inte köper tillägg för datalagring, uppfyller Concurrency Monitoring följande krav för datalagring:

* *Clickstream-data* som samlats in av Concurrency Monitoring måste tas bort senast **25 månader** från samlingsdatumet.

### Avslutade kunder {#terminated-customers}

En avbruten kund är en kund som har avslutat relationen med Adobe och inte längre använder övervakning av samtidig användning.

* *Clickstream-data* som samlats in av Concurrency Monitoring måste tas bort inom **6 månader** från kundens kontraktavslutningsdatum.

## Borttagning av data {#data-deletion}

Adobe behåller rätten att radera data för datum som ligger efter den period då data finns kvar utan möjlighet till återställning. Data för nuvarande kunder bör raderas rullande månadsvis.
