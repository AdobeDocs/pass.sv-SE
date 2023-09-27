---
title: Definiera ett segment och en tidsram
description: Definiera ett segment och en tidsram
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# Definiera ett segment och en tidsram {#define-segment}

Alla analyser eller rapporter i [!UICONTROL Account IQ] börja med att definiera segment och välja tidsram för utvärdering. [Segment](/help/accountiq/product-concepts.md#segmet-def) avser alla prenumeranter eller tittare som uppfyller dina kriterier (som prenumererar på ett separat dokumentationsdokument och visar specifika kanaler) för utvärderingen.

![](assets/segment-panel.png)

*Bild: Markering av segment och tidsramar*

Överst på alla rapportsidor i [!UICONTROL Account IQ]finns det en panel för att definiera segment genom att välja programmeringsprogram för videoprogrammering, samt granularitet och tidsram.

## Segmentmarkering {#select-segment}

### Välj flerkanalsdokumentskydd i segment {#select-segment-mvpds}

Välj MVPD från **[!UICONTROL MVPDs in segment]** alternativ:

1. Klicka eller tryck på **[!UICONTROL MVPDs in segment]** rullgardinsalternativ.

   >[!NOTE]
   >
   >**Alla** branschens videofilmsprogram väljs som standard. Här kan du välja något av **Top 10 MVPDs by sharing score**, **De 10 viktigaste videobandspelare per användning**, **De 10 viktigaste versionerna per konto** eller enskilda videofilmsprogram. Om du vill markera enskilda PDF-filer måste du avmarkera **Alla**.

1. Klicka på eller tryck på de MVPD-filer du vill använda.

   Du kan ta bort ett PDF-dokument från markeringen genom att avmarkera det.

1. Klicka eller tryck **[!UICONTROL Apply selection]** för att det valda innehållet ska börja gälla. Annars kommer du att förlora det du har gjort.

   >[!NOTE]
   >
   >Om du väljer Isoleringsläge går det inte att välja någon av de andra PDF-filerna.

### Markera kanaler i segment {#select-segment-channels}

Välj önskade programmeringskanaler på menyn **[!UICONTROL Channels in segment]** alternativ:

1. Klicka eller tryck på **[!UICONTROL Channels in segment]** rullgardinsalternativ.

   >[!NOTE]
   >
   >**Alla** programkanaler för ditt företag väljs som standard. Om du vill markera enskilda kanaler eller programmerare måste du först avmarkera **Alla**.

1. Klicka eller tryck på de kanaler eller programmerare du vill använda.

   Listobjekt på den översta nivån i **[!UICONTROL Channels in segment]** är [programmerare](/help/accountiq/product-concepts.md#programmer-def) företag och listobjekten under programmerarnamn är deras [kanaler](/help/accountiq/product-concepts.md#channel-def). Du kan antingen välja enskilda kanaler under programmerare eller välja programmerare och alla aktiviteter för kanalerna under den programmeraren inkluderas i rapport- och diagramresultaten.

   ![](assets/programmer-channels.png)


   *Bild: Programmerare och kanaler listade i kanalväljaren*

   >[!IMPORTANT]
   >
   >Resultatet av att välja enskilda kanaler under en programmerare är inte detsamma som att välja programmerare.
   >
   >
   >När du väljer enskilda kanaler delas aktiviteterna i dessa kanaler upp individuellt i vissa rapporter. När du väljer överordnad programmerare för alla dessa kanaler inkluderas alla dessa kanalers aktivitet, men de delas inte upp individuellt i rapporter.

1. Klicka eller tryck **[!UICONTROL Apply selection]** för att det valda innehållet ska börja gälla.

>[!NOTE]
>
>Du kan inte markera mer än 10 objekt i menyerna MVPD eller Programmer pulldown.

### Avmarkera videofilmsprogram och kanaler {#deselect-segment-mvpds-channels}

Förutom att ändra markeringen i dialogrutan **[!UICONTROL MVPDs in segment]** och **[!UICONTROL Channels in segment]** segmentväljare kan du avmarkera de tidigare markerade videofilmsprogrammen och kanalerna genom att:

* Markera **[!UICONTROL Remove]** ikon (![ta bort ikon](assets/remove-icon.png)) på namnen på de markerade videofilmarna och kanalerna som visas under segmentväljaren.

* Du kan också använda **[!UICONTROL Clear Selection]** om du vill ta bort alla tidigare markerade programmeringsskyltar eller kanaler.

![](assets/segment-panel-selection.png)

*Bild: Markerade videofilmsprogram och kanaler på segmentpanelen och tidslinjepanelen*

## Kornighet och val av tidsram {#granularity-timeframe}

Så här väljer du en utvärderingsperiod:

1. Välj **[!UICONTROL Granularity and time frame]** datumväljare.

1. Välj antingen **[!UICONTROL Week]** eller **[!UICONTROL Month]** från **[!UICONTROL Aggregate By]** om du vill ange granularitet för utvärderingen.

   ![](assets/granularity-timeframe-weekwise.png)


   *Bild: Datumväljaren för att välja Kornighet och tidsram*

1. När du har valt granularitet kan du använda framåt- eller bakåtpilarna för att flytta framåt eller bakåt i tiden.

1. Ange en tidsperiod i bakgrunden (i månad eller vecka baserat på vald granularitet) för utvärdering.

1. Välj **[!UICONTROL Apply Selection]** för att säkerställa att markeringen genomförs.
