---
title: Datalagringspolicy
description: Datalagringspolicy
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Datalagringspolicy {#data-retention-policy}

>[!WARNING]
>
>**Obs!** Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.


## Introduktion {#introduction}

Adobe måste i sin roll som personuppgiftsbiträde vidta lämpliga åtgärder för att hjälpa sina kunder med att uppfylla kraven på åtkomst, borttagning och andra förfrågningar från enskilda personer. Att tillämpa lämpliga, säkra och vältajmade principer för borttagning är en viktig del av att uppfylla denna skyldighet.

## Definitioner {#definitions}

En datalagringspolicy avgör hur länge Adobe lagrar kundens data. Standarddatalagringsprincipen för övervakning av samtidig användning är **25 månader**.

| Datalagringsperiod | Datalagringsperioden är standardperioden för datalagring (25 månader). |
|---|---|
| **Fönstret Datalagring** | I fönstret för datalagring definieras parametrarna för vilka fullständiga data kan visas och rapporteras. Datalagringsfönstret bestäms enligt följande:<br/> *Startdatum* = aktuellt datum - datalagringsperiod <br/>*Slutdatum* = aktuellt datum |

## Datainsamling {#data-collection}

*Clickstream-data* representerar data som delas av kunder i sessionens pulsslag (till exempel subjectID, mvpdName och metadata). Alla anpassade metadatafält refereras i [Standardmetadataattribut](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Kundtyper {#customer-types}

### Nuvarande kunder {#current-customers}

Såvida kunden inte köper tillägg för datalagring, uppfyller Concurrency Monitoring följande krav för datalagring:

* *Clickstream-data* som samlats in av Concurrency Monitoring måste tas bort senast **25 månader** från och med datumet för insamlingen.

### Avslutade kunder {#terminated-customers}

En avbruten kund är en kund som har avslutat relationen med Adobe och inte längre använder övervakning av samtidig användning.

* *Clickstream-data* samlas in av Concurrency Monitoring måste tas bort inom **6 månader** från kundens uppsägningsdatum.

## Borttagning av data {#data-deletion}

Adobe behåller rätten att ta bort data för datum som ligger efter den period då data bevaras utan möjlighet till återställning. Data för nuvarande kunder bör raderas rullande månadsvis.

