---
title: Prenumerationssegment och tidsintervall
description: Definiera kohorter eller välj abonnentsegment för att mäta möjligheterna och mönstren för kontodelning för era kanaltittare så att de kan använda grafiska verktyg och rapporter i konto-IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: 6b790728f3d6a8eed5dfc0f8b3d0dad283af6418
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---


# Prenumerationssegment och tidsintervall {#cohorts-segments}


När du loggar in på konto-IQ kan du med segmentstartspanelen längst upp ange prenumeranten [segment](/help/accountiq/product-concepts.md#segment-segmet-def). Detta hjälper till att filtrera resultat när du visar rapporter om delningsbeteende och -mönster för prenumeranter. Ett standardsegment med namnet Alla konton i dina egenskaper är redan markerat och följande alternativ visas i segmentstartaren:

![](assets/new-segment-selector-collapsed.png){width="800" align="left"}

*Bild: Segmentstart med komprimerad segmentsammanfattning*

**A** Markerat segmentnamn<br/>
**B** Tidsintervall och granularitetsväljare<br/>
**C** Segmentsammanfattning komprimerad<br/>
**D** Alternativ för att expandera segmentsammanfattning<br/>
**E** Segmentdata (i antal abonnentkonton i segmentet under en tidsperiod)<br/>
**F** Öppna segmentlistalternativ<br/>
**G** Redigera segment, alternativ<br/>
**H** Skapa nytt segmentalternativ<br/>

## Segmentmarkering {#segment-selection}

För programmerare- eller MVPD-användare går du till **Öppna segment** alternativ. Välj ett segment i listan och markera **Öppna segment** om du vill visa rapporter om kontodelning.

Använd **Ögon** om du vill visa en detaljerad segmentsammanfattning, med information om antalet prenumerantkonton och uppspelningsförfrågningar från dem inom det valda tidsintervallet.

+++Segmentmarkeringspanel för programmerare/videoprogrammerare

![](assets/segment-panel-programmers-mvpds.png) {width="800" align="left"}

*Bild: Segmentpanel för programmerare/videoprogrammerare*

+++

Segmentsammanfattningen används för att definiera följande parametrar:

**[!UICONTROL Programmers in segment]**

**[!UICONTROL Channels in segment]**

**[!UICONTROL MVPD in segment]**

**[!UICONTROL Metrics in segment]**

<!-- The definitions of these parameters will be defined in the glossary article-->

## [!UICONTROL Granularity and time interval] {#granularity-timeinterval}

The **[!UICONTROL Granularity and time interval]** Med väljaren kan du ange sammanställda datum och tidslängd varje vecka/månad för att observera hur abonnentkonton delas. Standardinställningen för tidsintervallet är den aktuella veckan, men du kan ändra längden med alternativen som visas i bilden.

![[!UICONTROL Granularity and timeinterval]](assets/granularity-timeinterval-weekwise.png){width="350" align="left"}

*Bild: Dialogrutan Kornighet och tidsintervall*

**A** Välj ett datum från datumväljaren<br/>
**B** Markera vänsterpilen om du vill gå bakåt<br/>
**C** Markera högerpilen för att gå framåt<br/>
**D** Välj granularitet per vecka/månad<br/>
**E** Markerat tidsintervall<br/>

Genom att använda dessa kontroller kan du definiera din problemprogramsats som&quot;prenumeranter på MVPD A som tittade på kanalerna X, Y och Z i oktober&quot;.

