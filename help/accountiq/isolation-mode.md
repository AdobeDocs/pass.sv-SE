---
title: Visa rapporter i isoleringsläge
description: Visa rapporter i isoleringsläge för Xfinity.
exl-id: e7cf24c5-9bfa-48f6-b5c8-20443a976891
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Visa delningsrapporter i isoleringsläge {#report-isolation-mode}

I isoleringsläge identifierar programmeringsfönster (t.ex. Xfinity) konsekvent abonnenter på olika enheter, men identifierar deras abonnenter på olika sätt utifrån de programmerare de interagerar med. I standardläget identifierar programmeringsdokumentets prenumeranter på olika enheter, oberoende av programmerare.

I följande bild kopplas olika identifierare till de två olika åtkomstförsöken om en prenumerant B på ett MVPD-program för Isoleringsläge (till exempel Xfinity) öppnar det innehåll som erbjuds av två olika programmerare med samma enhet. För de programmerare (L och M i figuren) och Account IQ verkar det som om det finns två olika prenumeranter som får åtkomst till innehållet. Om prenumerant B däremot, för Standard MVPD, öppnar innehåll som erbjuds av två olika programmerare kommer MVPD att koppla en enda åtkomstidentifierare för båda åtkomstförsöken. MVPD-filer (t.ex. Xfinity) i isoleringsläge identifierar inte en prenumerant konsekvent även om prenumeranten använder samma enhet för olika programmerare.

![](assets/isolation-diff-new.png)

*Bild: Isoleringsläge MVPD identifierar fyra olika prenumeranter i stället för två*

För att hantera förvrängning av data (på grund av att samma abonnent identifieras som en annan baserat på åtkomst till olika programmerare) begränsar Isoleringsläget aktiviteten som rapporteras om en programmerare till aktiviteten endast i programmerarens program. För isoleringsläget i bilden ovan ser programmeraren L data som bara baseras på aktiviteten för Identiteter W och Y, och ignorerar Identiteter X och Z.

>[!IMPORTANT]
>
> Nackdelen är att programmeraren L inte längre kan dela information som samlats in om prenumeranter A och B på grund av aktivitet med någon annan programmerare än L.

I isoleringsläge görs alla beräkningar som gjorts för att hämta delningspoängen och alla tillhörande mått endast med hjälp av aktiviteten hos de enheter som direktuppspelas från program som tillhör den valda programmeraren och de valda kanalerna.
Delningspoängen och sannolikheterna beräknas endast med den ström som börjar från de valda kanalerna.

Så här visar du mått i isoleringsläge:

1. Välj **[!UICONTROL isolation mode]** från **[!UICONTROL MVPDs in segment]** nedrullningsbart alternativ och välj **[!UICONTROL Apply Selection]**.

   ![](assets/xfinity-in-segment.gif)

   *Bild: MVPD-val i isoleringsläge*

1. Välj önskade kanaler på menyn **[!UICONTROL Channels in segment]** nedrullningsbart alternativ och välj **[!UICONTROL Apply Selection]**.

   Välj även en [tidsram](/help/accountiq/product-concepts.md#granularity-def).

   >[!IMPORTANT]
   >
   >Eftersom kontodelning är mer relevant när den mäts för direktuppspelning i alla programmerarens program, kommer du att se lägre delningspoäng och vissa variationer i mätvärdena i isoleringsläge.

   ![](assets/aggregate-sharing-isolation.png)

   *Bild: Dela sannolikhetsmätare i isoleringsläge*

   Observera att ovanstående mätare visar att endast 6 % av alla konton delas och att endast 8 % av innehållet förbrukas av dessa 8 %. Kanalerna kan alltså jämföra sina bakgrundsmusik i isoleringsläge med andra videofilmsprogram. Därför bör den information som erhålls med Isoleringsläge tolkas annorlunda än de övriga uppgifterna.
