---
title: Rapporter om delade konton
description: Rapporter om delade konton
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Rapporter om delade konton {#shared-accounts-reports}

Rapporterna om delade konton delar upp mätvärden som antal enheter och enhetstyper efter det valda intervallet med sannolikhet för delning, till exempel **[!UICONTROL Over Moderate Probability]** och **[!UICONTROL Over Low Probability]** för aktuellt segment.

Intervallen kan sedan fungera som användardefinierade tröskelvärden och diagrammen uppdateras utifrån de valda tröskelvärdena.

Med konto-IQ klassificeras alla abonnentkonton för det definierade segmentet i kontona med följande fem kategorier baserat på deras delningssannolikhet:

* Mycket hög (80-100 %)
* Hög (60-80 %)
* Måttligt (40-60 %)
* Låg (20-40 %)
* Mycket låg (0 %-20 %)

## Sannolikhet för kontodelning {#accounts-sharing-probability}

I donutdiagrammet här kategoriseras och visas procentsatserna (och absoluta tal) för abonnentkonton från olika sannolikhetskategorier.

Den röda raden markerar det tröskelvärde som valts av användare i [Konton över tröskelvärdet i aktuellt segment](#threshold-selector) -panelen.

![](assets/accounts-sharing-probability-pie.png)

I stapeldiagrammet visas antalet konton på y-axeln för olika kategorier av delningssannolikheter (ritade på x-axeln).

![](assets/accounts-sharing-probability-bar.png)

Den röda linjen anger tröskelvärdet och kan justeras i stapeldiagrammet. Tröskelvärdet som justeras i stapeldiagrammet återspeglar tröskelvärdet i donutdiagrammet.

<!--![](assets/shared-accounts-rep.gif)-->

### Konton över tröskelvärdet i aktuellt segment{#threshold-selector}

På den här panelen kan du välja ett intervall från följande som tröskelvärde för prenumerantkonton (baserat på deras delningssannolikhet):

* Konton **över mycket låg** delning **sannolikhet**

* Konton **över lågt** delning **sannolikhet**

* Konton **övermåttlig** delning **sannolikhet**

* Konton **över högt** delning **sannolikhet**

![](assets/threshold-selector-shared-accounts.png)

När du har valt tröskelvärdet visas procentandelen (och antalet) konton av alla prenumerantkonton i det valda segmentet.

## Segment - Spela upp begäranden av totalt {#play-request-out-total}

Miniatyrdiagrammet visar hur många (och hur många) uppspelningsbegäranden som gjorts av prenumeranter i segmentet och du kan jämföra uppspelningsbegäranden som gjorts av prenumeranter som inte finns i det definierade segmentet.

![](assets/play-req-outof-total.png)

När du flyttar markören i donatdiagrammet visas även procenttal och siffror för prenumeranter från olika sannolikhetsintervall.

<!--![](assets/play-request-total.gif)-->

## Segment - genomsnittligt antal enheter per konto{#avg-devices-account}

I stapeldiagrammet visas det genomsnittliga antalet enheter av varje enhetstyp som används av prenumeranter i det aktuella segmentet och prenumeranter som inte finns i det aktuella segmentet.

![](assets/avg-devices-per-acc.png)

## Segment - postnummer per period per konto {#zip-codes-period-account}

I det här diagrammet visas hur många abonnenter som konsumerar innehåll från olika platser i en tidsram.

![](assets/zip-period-account.png)

Du kan zooma in om du vill begränsa och visa detaljer för ett fält i diagrammet som ritar ett intervall med platser.

<!--![](assets/zip-code-period.gif)-->

## Segment - geografiskt område/period/konto {#geo-span-period-account}

I detta stapeldiagram visas antalet abonnentkonton i förhållande till olika geografiska intervall i engelska mil. Intervallet baseras på det maximala avståndet mellan de platser där en prenumerant har direktuppspelat under tidsramen.

<!--Total number of users ...

How many accounts are within 99 miles of each other.....and how many are apart. 

Based on points on the map.-->

![](assets/geogr-span-account.png)

När du markerar en stapel som representerar ett intervall av geografiskt avstånd, utökas intervallet så att du får mer information.

<!--![](assets/geo-span-period-acc.gif)-->
