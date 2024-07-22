---
title: Segment och tidsintervall
description: Definiera kohorter eller välj abonnentsegment för att mäta möjligheterna och mönstren för kontodelning för era kanaltittare så att de kan använda grafiska verktyg och rapporter i Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segment och tidsintervall {#segment-timeinterval}

När du loggar in på Account IQ kan du definiera [segmentet](product-concepts.md#segmet-def) på segmentpanelen och panelen för tidsintervall ovanför instrumentpanelen. Den här panelen hjälper dig att filtrera resultat och visa rapporter om hur prenumeranter delar beteenden och mönster. Ett segment med namnet **ALLA KONTON I DINA EGENSKAPER** är markerat som standard där du kan visa följande alternativ:

![](assets/new-segment-selector-collapsed.png){align="left"}

*Segment- och tidsintervallpanel med komprimerad segmentsammanfattning*

**A.** Det valda segmentnamnet **B.** Öppna segmentlista **C.** Redigera segment **D.** Skapa nytt segment **E.** Kornighet och tidsintervallväljare **F.** Ikon för att expandera segmentsammanfattning **G.** Komprimerad segmentsammanfattning **}H.** Antal konton i segmentet för det valda intervallet

>[!NOTE]
>
> Den komprimerade segmentsammanfattningen visar de [videokategorier](product-concepts.md#video-category-def) som används i TV Everywhere-versionen av Account IQ. Om du är inloggad som en D2C-tjänst visar etiketterna företagets specifika videokategorier.

Läs mer om [hur du skapar](work-with-segments.md#create-new-segment) och [hanterar segment](work-with-segments.md#manage-segment) från fliken **Segment** i den vänstra panelen.

## Segmentmarkering {#segment-selection}

Så här markerar du ett specifikt segment:

1. Navigera till alternativet **[!UICONTROL Open segment]** i segmentpanelen och tidintervallpanelen.
1. Välj det **segmentnamn** som du vill visa rapporterna om kontodelning för.

   ![](assets/open-segment.png){align="left"}

   *Välj segmentnamn*

   >[!NOTE]
   >
   > De videokategorier som visas i den föregående bilden, till exempel **MVPDs**, **Programmers** och **Channels**, representerar etiketterna som används i TV Everywhere-versionen av Account IQ. Om du är inloggad som en D2C-tjänst visar etiketterna företagets specifika videokategorier.

1. Välj **[!UICONTROL Open segment]**.


## Val av kornighet och tidsintervall {#granularity-timeinterval}

Med väljaren **Kornighet och tidsintervall** kan du ange sammanställda datum och tidslängd varje vecka/månad för att observera prenumerantdelningsbeteende. Standardinställningen är den aktuella veckan.

![Kornighet och tidsintervall](assets/granularity-timeinterval-weekwise.png){align="left"}

*Dialogrutan Kornighet och tidsintervall*

**A.** Kornighet och tidsintervallväljare **B.** Högerpil som går till nästa månad/vecka **C.** Alternativ för att välja granularitet efter vecka/månad **D.** Aktuellt valt tidsintervall **E.** Vänsterpil som går till föregående månad/vecka

Du kan ändra längden på följande steg:

1. Välj **[!UICONTROL Granularity and Time Interval]** i datumväljaren.

1. Välj antingen **[!UICONTROL Week]** eller **[!UICONTROL Month]** från **[!UICONTROL Aggregate By]** om du vill ange granularitet för utvärderingen.

1. När du har valt granularitet kan du använda framåt- eller bakåtpilarna för att navigera i tidsintervallet.

1. Välj en specifik tidsperiod för utvärdering.

1. Välj **[!UICONTROL Apply]** för att kontrollera att markeringen börjar gälla.

På så sätt kan du definiera din problemlösning som&quot;prenumeranter på MVPD A som tittade på kanalerna X, Y och Z under valveckan i december&quot;.

## Segmentsammanfattning {#segment-summary}

Segmentsammanfattningen liknar den för D2C-tjänster och TV Everywhere. Videokategorierna är olika för respektive version av Account IQ.

Välj <img alt= "expandera segmentsammanfattning" src="./assets/expand-segment-summary.svg" width="25"> om du vill visa den detaljerade segmentsammanfattningen. Det innehåller också information om antalet abonnentkonton och deras uppspelningsbegäranden inom den valda tidsperioden.

+++ D2C-tjänster

![](assets/segment-panel-d2c.png){align="left"}

*Segmentsammanfattning för D2C-tjänster*

>[!NOTE]
>
>De [videokategorier](product-concepts.md#video-category-def) som visas i föregående bild, som **region** och **innehållstyper** i segment, är bara exempel. När du loggar in på Account IQ visar etiketterna företagets specifika videokategorier.

**Segmentsammanfattningen** innehåller följande villkor som definierar ett segment:

**[Regioner och innehållstyper](product-concepts.md#video-category-def) i segment** refererar till metadataetiketter som är associerade med videoströmmar som bevakas av delade konton som representeras i kontodelningsrapporter.

**[Mått](product-concepts.md#metric) i segment** refererar till attribut eller villkor som prenumeranter måste ha uppfyllt för att kunna identifieras i kontodelningsrapporter.

+++

+++ TV Everywhere

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Segmentsammanfattning för programmerare/MVPD*

**Segmentsammanfattningen** innehåller följande villkor som definierar ett segment:

**[Programmerare](product-concepts.md#programmer-def) i segment** refererar till innehållsleverantörer vars videoströmmar bevakades av delade konton i kontodelningsrapporter.

**[Kanaler](product-concepts.md#channel-def) i segment** refererar till kanaler vars videoströmmar bevakades av delade konton i kontodelningsrapporter.

**[MVPD:er](product-concepts.md#mvpd-def) i segment** refererar till distributörer av multivideoprogrammering som prenumeranterna är kopplade till för att identifieras i kontodelningsrapporter.

**[Mått](product-concepts.md#metric) i segment** refererar till attribut eller villkor som prenumeranter måste ha uppfyllt för att kunna identifieras i kontodelningsrapporter.

+++
