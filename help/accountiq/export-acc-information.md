---
title: Exportera information för konton med hög delning
description: Exportera information för konton med hög delningspoäng.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Exportera information för konton med hög delning {#export-account-info-high-score}

[!UICONTROL Account IQ] kan du exportera kontodelningsinformation för de 1 000 vanligaste prenumerantkontona baserat på deras [delningssannolikhet](/help/accountiq/product-concepts.md#account-sharing-probability-def). Du kan exportera kontodelningsinformation för den aktuella [segment](/help/accountiq/product-concepts.md#segment-def) och [angivet tidsintervall](/help/accountiq/product-concepts.md#time-interval-def) på [Rapporter om delade konton](/help/accountiq/shared-acc-reports.md) sida.

Följ de här stegen för att exportera kontodelningsinformation för prenumerationskonton för ett visst segment.

1. Logga in med dina inloggningsuppgifter.
1. Navigera till **Delade konton** flik under **Rapporter** -avsnitt.
1. Välj önskat segment och tidsintervall från segmentpanelen och tidintervallpanelen. Läs [välja ett segment och tidsintervall](segments-timeinterval.md).

   Om det behövs, se instruktionerna för [skapa ett segment](work-with-segments.md#create-new-segment) eller [redigera ett segment](work-with-segments.md#edit-segment).

1. Välj **[!UICONTROL Export top 1000 accounts]** i segmentets övre högra hörn och panelen för tidsintervall.

   ![Exportera de 1 000 populäraste kontona](assets/export-top-1000-accounts.png)

   *Välj alternativet Exportera de 1 000 populäraste kontona*

Filen hämtas automatiskt till din lokala dator som en CSV-fil.

Den här filen innehåller data för de 1000 främsta kontona baserat på delningssannolikheten för prenumerantkontona i det aktuella segmentet i minskande ordning.

Följande är ett exempel på den exporterade CSV-filen.

![exporterade data i en CSV-fil](assets/exported-csv.png)

*Exporterade data i en CSV-fil*

## Kolumner i den exporterade rapporten {#columns-in-export}

**Vecka/månad**

Den valda veckan eller månaden i **[!UICONTROL Granularity and Time Interval]** i segmentväljaren.

**MVPD**

Om du är programmerare visar kolumnen den distributör som kontot prenumererar på.

>[!NOTE]
>
> The **MVPD** -kolumnen är bara tillgänglig för TV Everywhere-versioner.

**Prenumerant-ID**

Unik identifierare för det specifika kontot.

**Minsta antal enheter**

Det minsta antal enheter från vilka användare direktuppspelar innehåll.

>[!NOTE]
>
>Det faktiska antalet enheter som direktuppspelar innehåll är större än det minsta antalet enheter som anges för ett visst konto.

**Minsta antal personer**

Det minsta antalet personer som aktivt strömmat innehåll med dessa enheter.

>[!NOTE]
>
>Det faktiska antalet personer som direktuppspelar innehåll är större än det minsta antalet personer som tilldelats ett visst konto.

**[!UICONTROL # IPs]**

Antalet IP-adresser som innehållet direktuppspelas från.

**[!UICONTROL # Locations]**

Antalet platser (baserat på postnummer) från vilka innehållet direktuppspelas.

**[!UICONTROL # Cities]**

Antalet städer där direktuppspelningsaktiviteten har ägt rum.

**[!UICONTROL # States]**

Antalet lägen där direktuppspelningsaktiviteten har ägt rum.

**[!UICONTROL # Clusters]**

Antalet distinkta [kluster](/help/accountiq/product-concepts.md#cluster-def) där strömningen har ägt rum.

**[!UICONTROL Geographic span (miles)]**

Det maximala avståndet mellan de direktuppspelningsplatser som är associerade med kontot.

**[!UICONTROL # AuthN OK]**

Antalet inloggningar som användare gör under den angivna perioden med det kontot.

>[!NOTE]
>
> Vissa D2C-tjänster kanske inte visas **[!UICONTROL # AuthN OK]** data eftersom de kanske inte ingår i företagets data.

**[!UICONTROL # AuthZ OK]**

Antalet gånger som en MVPD har auktoriserat en ström eller beviljat åtkomst till innehåll för det kontot.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** är inte tillgängligt för D2C-tjänster.

>[!NOTE]
>
>För TV Everywhere, **[!UICONTROL # AuthZ OK]** är korrelerat med antalet **[# uppspelningsbegäranden](/help/accountiq/product-concepts.md##play-requests-def)**. Den kommer alltid att vara mindre än **[!UICONTROL # Play Requests]** eftersom Adobe vanligtvis cachar tillstånden från sidoskyddsorganisationen i cirka 24 timmar.


**[!UICONTROL # Play Requests]**

Det faktiska antalet strömmar inträffade under en angiven tidsperiod.

>[!NOTE]
>
>The [# uppspelningsbegäranden](/help/accountiq/product-concepts.md##play-requests-def) -kolumnen är inte tillgänglig i TV Everywhere MVPD-versionen.

**[!UICONTROL # Channels]**

Det totala antalet kanaler som kontot har bevakat under en angiven period.

>[!NOTE]
>
> För D2C-tjänster **[!UICONTROL # Channels]** motsvarar antalet **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>För TV Everywhere innehåller de kanaler som kanske inte tillhör den inloggade programmeraren. Det här numret för kontot omfattar din kanal och andra kanaler som du har åtkomst till under den angivna perioden.


**Användningsmönster**

Värdena i de här kolumnerna fungerar som identifierare som motsvarar ett av de 14 mönster som vi använder för att kategorisera alla användarkonton.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Användningsmönster</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Vanlig användare</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Resa eller dator</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Stor familj</td>
      </tr>
      <tr>
        <td>4</td>
        <td>Nära släkt och vänner</td>
      </tr>
      </tr>
         <td>5 och 8</td>
         <td>Delning i sociala grupper</td>
      </tr>
      </tr>
         <td>6</td>
         <td>Stor grupp av vänner</td>
      </tr>
      </tr>
         <td>7</td>
         <td>Samtidig strömning</td>
      </tr>
      </tr>
         <td>9</td>
         <td>Community-delning</td>
      </tr>
      </tr>
         <td>10 och 11</td>
         <td>Osäkert beteende</td>
      </tr>
      </tr>
         <td>12</td>
         <td>Liten familj</td>
      </tr>
      </tr>
         <td>13</td>
         <td>Andra hemmet </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Onormal användning</td>
      </tr>
    </tbody>
  </table>

*Användningsmönsteridentifierare i exporterad CSV-mappning med användningsmönster*

**Sannolikhet för delning**

Sannolikheten för att ett visst konto delar sina autentiseringsuppgifter.

>[!NOTE]
>
> Medelvärdet för delningssannolikheten för alla konton i det valda segmentet används för att beräkna [delningsnivå](/help/accountiq/data-panels.md#sharing-level) i [medelvärde för delningspoäng](/help/accountiq/data-panels.md#aggregated-sharing).
