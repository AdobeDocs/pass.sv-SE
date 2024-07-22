---
title: Begränsningar
description: Lär dig mer om begränsningar och isoleringsläge för programmerare i Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Begränsningar {#limitations}

D2C- och TV Everywhere-versionerna av Account IQ ger analyser för användning och prenumeration till strömningsleverantörer. Den nuvarande versionen 1.3 har dock vissa begränsningar som kommer att åtgärdas i framtida versioner.

* Det [övergripande delningsresultatet](/help/accountiq/data-panels.md#overall-sharing-score) innehåller för närvarande [Delningsnivå](/help/accountiq/data-panels.md#sharing-level) och [Användning från delade konton](/help/accountiq/data-panels.md#usage-from-shared-accounts). Framtida releaser kommer att innehålla fler mätvärden.

* När du definierar segment på kontrollpanelen eller användningsmönster kan [videokategorierna i segmenten](/help/accountiq/data-panels.md#video-categories-segment), [Dela poäng per kanal och MVPD](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds) och [Fördelning av användningsmönster för videokategorier](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) endast visa data för upp till 20 [videokategorier](product-concepts.md#video-category-def). Segment som innehåller fler än 20 videokategorier visar inte data i dessa rapporter.

* För närvarande är alternativet att exportera kontostatistik begränsat till att exportera 1 000 konton.

* När du definierar åtgärder begränsas alternativet att välja [segmenttyp](/help/accountiq/operations.md#segment) till **Fast antal konton**. Alternativet **Variabelantal konton** är tillgängligt i framtida versioner.

* Avsnitten **Benchmarking**, **Detection Models**, **Actions** och **Settings** i den vänstra navigeringen är för närvarande inaktiverade och kommer att vara tillgängliga i en framtida version.

* När du skapar åtgärder kan du bara använda två typer av [åtgärder](/help/accountiq/operations.md#action) - regler för övervakning av samtidig användning och externa åtgärder.

* För närvarande kan du bara [skapa](/help/accountiq/operations.md#create-new-operation)- och [schemalägga](/help/accountiq/operations.md#schedule)-åtgärder. I framtida versioner kan du pausa, återuppta och hantera dem helt.

* Du kan bara analysera data för en enstaka vecka eller månad i taget när du väljer Kornighet och Tidsintervall.

* Det går inte att lägga till flerlägesobjekt för isoleringsläge i segmentdefinitionen med andra flerlägesobjektsvideofiler. Vissa programmerings-programmerings-programmerare identifierar inte abonnenter unikt över flera programmeringskanaler. Därför behandlas dessa programmerare separat för programmerare som arbetar med TV Everywhere.



