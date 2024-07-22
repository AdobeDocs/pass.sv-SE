---
title: Arbeta med segment
description: Förstå och använda segment. Lär dig hur du skapar och hanterar ett segment.
exl-id: 01431437-55f5-464d-8ee4-7a79ec553e4f
source-git-commit: 38402b411dd4fad3490115cff1854faa7f3cb293
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Arbeta med segment {#work-with-segments}

[Segment](product-concepts.md#segmet-def) är en samling prenumerantkonton som gör att du kan analysera delning av autentiseringsuppgifter under användardefinierade villkor. Du kan använda segment för att undersöka olika uppsättningar prenumerationskonton och generera motsvarande datarapporter i tabeller och diagram. Det finns två typer av segment i Account IQ:

1. **Standardsegment**: **Alla konton i dina egenskaper** är ett segment som inte är klart att användas i systemet och som innehåller alla aktiva prenumerantkonton utan särskilda villkor.

   >[!NOTE]
   >
   >Om du använder standardsegmentet kan det leda till att vissa tabeller inte visas, som [Videokategorier i segment ](data-panels.md#video-categories-segment), [Dela bakgrundsmusik efter kanaler och MVPD](data-panels.md#sharin-score-by-channels-and-mvpds) och [Använd mönsterdistribution för videokategorier](usage-patterns.md#usage-pattern-dis-video-categories). Dessa tabeller kan endast innehålla och visa data för upp till 20 rader åt gången. De återstående tabellerna, diagrammen och rapporterna är desamma för standardsegment och anpassade segment.

1. **Anpassade segment**: Dessa är skräddarsydda segment som gör att du kan gruppera prenumerantkonton från specifika kategorier, till exempel D2C-innehållstyper, programmerare, kanaler och distributörer för analys av delning av autentiseringsuppgifter under användardefinierade villkor. Läs mer om hur du [skapar ett anpassat segment](#create-new-segment).

   >[!IMPORTANT]
   >
   >Alla procedurer som beskrivs i den här handboken är baserade på anpassade segment. Begreppen är dock desamma för standardsegment och anpassade segment.

När du går till **Åtgärder** och väljer fliken **[!UICONTROL Segments]** i den vänstra panelen visas en lista med segment som är tillgängliga i systemet. På sidan Segment kan du snabbt utvärdera nyckeldetaljer för varje segment i ett tabellformat. Bland detaljerna finns segmentnamnet, antalet [videokategorier](product-concepts.md#video-category-def), mått, [åtgärder](product-concepts.md#operation-def) som använder det aktuella segmentet, datum och tid för senaste ändring samt namnet på segmentskaparen.

Du kan utföra följande funktioner med segment:

* [Skapa ett nytt segment](#create-new-segment)
* [Hantera segment](#manage-segments)


## Skapa ett nytt segment {#create-new-segment}

Processen att skapa ett nytt segment liknar den för D2C-tjänster och TV Everywhere. Videokategorierna är olika för respektive version av Account IQ.

+++D2C-tjänster

Om du vill skapa ett segment och analysera prenumerantens delningsbeteende väljer du **[!UICONTROL Create new segment]** längst upp till höger.

![Välj Skapa nytt segment](assets/create-new-segment-d2c.png)

*Välj Skapa nytt segment*

>[!NOTE]
>
>De videokategorier som visas i föregående bild, som **Regioner** och **Innehållstyper** är bara exempel. När du loggar in på Account IQ visar etiketterna företagets specifika videokategorier.

Sidan **Nytt segment** öppnas, som innehåller följande element:

![Ny segmentsida](assets/d2c-new-segment-dialog.png)

*Ny segmentsida*

**A.** Segmentkomponenter **B.** Segmentdefinition **C.** Segmentsammanfattning

* **Segmentkomponenter**: En inventering av [videokategorier](product-concepts.md##video-category-def) och beräknade värden som används för att definiera ett segment.

  >[!NOTE]
  >
  >Använd **[!UICONTROL Show all]** för att expandera listan med segmentkomponenter. Om du snabbt vill hitta en komponent söker du efter dess namn i **komponenter i söksegmentet** i stället för att bläddra igenom hela listan.

* **Segmentdefinition**: En arbetsyta där du kan dra och släppa olika segmentkomponenter för att skapa ett segment.

* **Segmentsammanfattning**: En sammanfattning som uppskattar kvalificerade konton baserat på komponenterna i segmentdefinitionen och ger en kort översikt över segmentet under utvärderingsperioden.

Så här skapar du ett segment:

1. Skriv namnet på ditt segment i **Segmentnamn** som ska visas i listan med segment och under segmentmarkering.
1. Skriv en detaljerad beskrivning av ditt segment i **Segmentbeskrivning**.
1. Dra till exempel **Områden och innehållstyper** från segmentkomponenterna på den vänstra panelen och släpp dem i avsnittet **Områden/innehållstyper** i **segmentdefinitionen**.

   >[!NOTE]
   >
   >Du kan skapa ett segment baserat på antingen regioner eller innehållstyper. Visa de associerade innehållstyperna för ett område i en listruta.

   Om du börjar med att lägga till en **innehållstyp** i avsnittet **Områden/innehållstyper** kan du bara lägga till innehållstyper som efterföljande komponenter.

   Om du börjar med att lägga till en **region** i avsnittet **Regioner/innehållstyper** visas en beslutsdialogruta.

   ![Lägg till segmentkomponent som en region eller dess innehållstyper ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Lägg till segmentkomponent som en region eller dess dialogruta för innehållstyper*

   Bestäm om du vill jämföra specifika regioner eller ett segment baserat på innehållstyperna som är kopplade till en region.

   Välj **[!UICONTROL As a region]** om du vill lägga till regioner i avsnittet **Områden/innehållstyper**.

   Välj **[!UICONTROL As its content types]** om du vill lägga till innehållstyper för ett område.

1. Dra **Metrisk** från segmentkomponenterna på den vänstra panelen och släpp dem i avsnittet **Metrisk** i **segmentdefinitionen**.

   ![Välj en operator och ange ett värde för det tillagda måttet](assets/component-metrics.png)

   *Välj en operator och tilldela ett värde för det tillagda måttet*

   När du har lagt till mätvärden i segmentdefinitionen väljer du en operator i listrutan **[!UICONTROL Select an operator]** och tilldelar ett värde med **[!UICONTROL Select an option]**.

   Justera värden för vissa mätvärden genom att använda uppåtpilen för att öka och nedåtpilen för att minska.

1. Dra **Beräknade mått** från segmentkomponenterna på den vänstra panelen och släpp dem i avsnittet **Beräknade mått** i **segmentdefinitionen**.

   ![Välj en operator och ange ett värde för det beräknade måttet som lagts till](assets/component-calculated-metrics.png)

   *Välj en operator och tilldela ett värde för det tillagda beräknade måttet*

   När du har lagt till beräknade värden i segmentdefinitionen, **[!UICONTROL Select an operator]** från listrutan och tilldelar ett värde med **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Alla mätvärden och beräknade mätvärden som du hamnar under segmentdefinitionen åtföljs av lämpliga operatorer för att tilldela värden till respektive mätvärden och beräknade värden.

1. Granska segmentinformationen i **segmentsammanfattningen** för att bestämma vilka ändringar du vill implementera i hela segmentet.
1. Välj **[!UICONTROL Last week]** eller **[!UICONTROL Last month]** i listrutan **Utvärderingsperiod** om du vill beräkna summeringsvärden för föregående vecka eller månad.
1. Välj **[!UICONTROL Update estimation]** om du vill beräkna antalet beräknade kvalificerade konton i det aktuella segmentet baserat på den valda utvärderingsperioden.
1. Välj **[!UICONTROL Save segment]**.

Segmentet som du har skapat finns nu i segmentlistan.

+++

+++TV Everywhere

Om du vill skapa ett segment och analysera prenumerantens delningsbeteende väljer du **[!UICONTROL Create new segment]** längst upp till höger.

![Välj Skapa nytt segment](assets/create-new-segment.png)

*Välj Skapa nytt segment*

Sidan **Nytt segment** öppnas, som innehåller följande element:

![Ny segmentsida](assets/new-segment-dialog.png)

*Ny segmentsida*

**A.** Segmentkomponenter **B.** Segmentdefinition **C.** Segmentsammanfattning

* **Segmentkomponenter**: En inventering av programmerare och kanaler, programmeringsprofiler, mätvärden och beräknade värden som används för att definiera ett segment.

  >[!NOTE]
  >
  >Använd **[!UICONTROL Show all]** för att expandera listan med segmentkomponenter. Om du snabbt vill hitta en komponent söker du efter dess namn i **komponenter i söksegmentet** i stället för att bläddra igenom hela listan.

* **Segmentdefinition**: En arbetsyta där du kan dra och släppa olika segmentkomponenter för att skapa ett segment.

* **Segmentsammanfattning**: En sammanfattning som uppskattar kvalificerade konton baserat på komponenterna i segmentdefinitionen och ger en kort översikt över segmentet under utvärderingsperioden.

Så här skapar du ett segment:

1. Skriv namnet på ditt segment i **Segmentnamn** som ska visas i listan med segment och under segmentmarkering.
1. Skriv en detaljerad beskrivning av ditt segment i **Segmentbeskrivning**.
1. Dra **Programmerare och kanaler** från segmentkomponenterna på den vänstra panelen och släpp dem i avsnittet **Programmerare/kanaler** i **segmentdefinitionen**.

   >[!NOTE]
   >
   >Du kan skapa ett segment baserat på antingen programmerare eller kanaler. Visa de associerade kanalerna med en programmerare i en listruta.

   Om du börjar med att lägga till en **kanal** i avsnittet **Programmerare/kanaler** kan du bara lägga till kanaler som efterföljande komponenter.

   Om du börjar med att lägga till en **programmerare** i avsnittet **Programmerare/kanaler** visas en beslutsdialogruta.

   ![Lägg till segmentkomponent som programmerare eller dess kanaler ](assets/segment-basis-selector.png){width="550" align="left"}


   *Lägg till segmentkomponent som programmerare eller dess kanaldialogruta*

   Bestäm om du vill jämföra specifika programmerare eller ett segment baserat på de kanaler som är kopplade till en programmerare.

   Välj **[!UICONTROL As a programmer]** om du vill lägga till programmerare i avsnittet **Programmerare/kanaler**.

   Välj **[!UICONTROL As its channels]** om du vill lägga till alla kanaler för en programmerare.

1. Dra **MVPD-filer** från segmentkomponenterna på den vänstra panelen och släpp dem i avsnittet **MVPD-filer** i **segmentdefinitionen**.

   >[!NOTE]
   >
   >När du loggar in som programmerare visas ett MVPD-dokument med namnet **xfinity** som ett fristående alternativ i avsnittet **MVPDs** . Du kan inte kombinera det med andra MVPD-program.

1. Dra **Metrisk** från segmentkomponenterna på den vänstra panelen och släpp dem i avsnittet **Metrisk** i **segmentdefinitionen**.

   ![Välj en operator och ange ett värde för det tillagda måttet](assets/component-metrics.png)

   *Välj en operator och tilldela ett värde för det tillagda måttet*

   När du har lagt till mätvärden i segmentdefinitionen väljer du en operator i listrutan **[!UICONTROL Select an operator]** och tilldelar ett värde med **[!UICONTROL Select an option]**.

   Justera värden för vissa mätvärden genom att använda uppåtpilen för att öka och nedåtpilen för att minska.

1. Dra **Beräknade mått** från segmentkomponenterna på den vänstra panelen och släpp dem i avsnittet **Beräknade mått** i **segmentdefinitionen**.

   ![Välj en operator och ange ett värde för det beräknade måttet som lagts till](assets/component-calculated-metrics.png)

   *Välj en operator och tilldela ett värde för det tillagda beräknade måttet*

   När du har lagt till beräknade värden i segmentdefinitionen, **[!UICONTROL Select an operator]** från listrutan och tilldelar ett värde med **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Alla mätvärden och beräknade mätvärden som du hamnar under segmentdefinitionen åtföljs av lämpliga operatorer för att tilldela värden till respektive mätvärden och beräknade värden.

1. Granska segmentinformationen i **segmentsammanfattningen** för att bestämma vilka ändringar du vill implementera i hela segmentet.
1. Välj **[!UICONTROL Last week]** eller **[!UICONTROL Last month]** i listrutan **Utvärderingsperiod** om du vill beräkna summeringsvärden för föregående vecka eller månad.
1. Välj **[!UICONTROL Update estimation]** om du vill beräkna antalet beräknade kvalificerade konton i det aktuella segmentet baserat på den valda utvärderingsperioden.
1. Välj **[!UICONTROL Save segment]**.

Segmentet som du har skapat finns nu i segmentlistan.
+++

## Hantera segment {#manage-segments}

Du kan markera ett segment i segmentlistan och sedan utföra följande åtgärder:

* [Redigera ett segment](#edit-segment)
* [Duplicera ett segment](#duplicate-segment)
* [Ta bort ett segment](#delete-segment)

![Redigera, duplicera eller ta bort ett segment](assets/manage-segments-list.png)

*Markera ett segment som du vill redigera, duplicera eller ta bort*

**A.** [Standardsegment](#work-with-segments) **B.** [Videokategorier](product-concepts.md#video-category-def)

>[!NOTE]
>
>De videokategorier som visas i det här avsnittet, till exempel **MVPDs**, **Programmers** och **Channels**, representerar etiketterna som används i TV Everywhere-versionen av Account IQ. Om du är inloggad som en D2C-tjänst visar etiketterna företagets specifika videokategorier.

Du kan inte redigera, duplicera eller ta bort standardsegmentet **Alla konton i egenskaperna**.

### Redigera ett segment {#edit-segment}

1. Navigera till fliken **[!UICONTROL Segments]** under **Åtgärder** i den vänstra panelen för att visa en lista med segment.
1. Markera det segment som du vill redigera.
1. Välj **[!UICONTROL Edit]**.
1. Ändra segmentinformation, t.ex. segmentnamnet, beskrivningen eller komponenterna i **segmentdefinitionen**.

   >[!TIP]
   >
   >Använd **[!UICONTROL Clear all]** om du vill ta bort alla segmentkomponenter i varje avsnitt under segmentdefinitionen samtidigt. Du kan också markera kryssknappen om du vill ta bort enskilda objekt.

   ![Rensa alla segmentkomponenter i varje avsnitt under segmentdefinitionen ](assets/clear-all-components.png)

   *Markera Rensa alla om du vill ta bort alla segmentkomponenter samtidigt*

1. Välj antingen **[!UICONTROL Update segment]** om du vill uppdatera det befintliga segmentet eller **[!UICONTROL Save as new segment]** om du vill skapa ett nytt segment med ändringarna.

   >[!NOTE]
   >
   >Det är inte tillåtet att uppdatera segment som är under pågående åtgärder. Det enda alternativet för segment med pågående åtgärder är att spara ändringar som ett nytt segment.

### Duplicera ett segment {#duplicate-segment}

1. Navigera till fliken **[!UICONTROL Segments]** under **Åtgärder** i den vänstra panelen för att visa en lista med segment.
1. Markera det segment som du vill duplicera.
1. Välj **[!UICONTROL Duplicate]**.

En kopia av det valda segmentet genereras och placeras i slutet av segmentlistan. Du kan redigera de nödvändiga detaljerna i det duplicerade segmentet och sedan antingen uppdatera det duplicerade segmentet eller spara det som ett nytt segment.

### Ta bort ett segment {#delete-segment}

1. Navigera till fliken **[!UICONTROL Segments]** under **Åtgärder** i den vänstra panelen för att visa en lista med segment.
1. Markera det segment som du vill ta bort.

   Markera flera segment om du vill ta bort dem i en enda åtgärd. Du kan också markera en kryssruta till vänster om **segmentnamnet** om du vill ta bort alla segment samtidigt.

   >[!NOTE]
   >
   > Du kan bara ta bort mer än ett segment eller alla segment om inget av segmenten används av åtgärderna. Det är inte heller tillåtet att ta bort standardsegmentet **Alla konton i dina egenskaper**. Den förblir omarkerad när du försöker ta bort alla segment samtidigt.

   ![Ta bort fler än ett segment](assets/delete-more-than-one-segment.png)

   *Markera flera segment om du vill ta bort fler än ett segment*

1. Välj **[!UICONTROL Delete]**.
1. Bekräfta på **[!UICONTROL Delete]** i dialogrutan om du vill ta bort segmentet permanent.

   >[!NOTE]
   >
   >Segmentet tas bort permanent från systemet och du kan inte ångra den här åtgärden.
