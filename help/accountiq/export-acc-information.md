---
title: Exportera information för konton med hög delning
description: Exportera information för konton med hög delningspoäng.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# Exportera information för konton med hög delning {#export-account-info-high-score}

[!UICONTROL Account IQ] ger dig möjlighet att exportera kontodelningsinformation för de 1 000 vanligaste prenumerantkontona baserat på deras [delningssannolikhet](/help/accountiq/product-concepts.md#account-sharing-probability-def). Data i den exporterade CSV-filen sorteras i minskande ordning efter delningssannolikhet för prenumerantkonton - för de valda MVPD-programmen i [segment](/help/accountiq/product-concepts.md#segment-def), för [angiven tidsram](/help/accountiq/product-concepts.md#time-frame-def).

Alternativet att exportera kontodelningsinformation är tillgängligt på [Allmänna användningsrapporter](/help/accountiq/general-usage-reports.md) och [Rapporter om delade konton](/help/accountiq/shared-acc-reports.md) sidor.

>[!NOTE]
>
>Siffrorna i den hämtade CSV-filen är olika för rapportsidorna General Usage och Shared Accounts. Detta beror på att sidan General Usage Reports (Rapporter om allmän användning) har ytterligare filter som programmerarna kan använda för att välja Tröskelvärde för antal enheter, IP-adresser och postkoder. De data som exporteras från rapporter om allmän användning baseras på det extra tröskelfilter som tillämpas.

![Alternativet Exportera i allmän användning](assets/export.png)

Så här exporterar du kontodelningsinformation till prenumeranter:

1. Definiera ett önskat segment enligt stegen i [Definiera segment och markera tidsram](/help/accountiq/howto-select-segment-timeframe.md) för utvärdering från [segment och tidsram](/help/accountiq/segments-timeframe.md) -panelen.

1. Välj **[!UICONTROL Export top 1000 accounts]** möjlighet att exportera kontoinformation för 1 000 prenumeranter med störst sannolikhet för delning.

När du använder exportalternativet hämtas statistiken för 1 000-konton med de största delningssannolikheterna (för en definierad tidsram) till mappen Hämtningar på den lokala datorn.

>[!NOTE]
>
>Den hämtade CSV-filen kan öppnas i alla program som läser CSV-filer, till exempel Microsoft Excel.

![exporterade data i CSV-format](assets/exported-csv.png)

*Bild: Exporterade delade kontodata i CSV-format*

## Kolumner i den exporterade rapporten {#columns-in-export}

**Vecka/månad**

Veckan eller månaden som du valde den **[!UICONTROL Granularity and Time Frame]** i segmentväljaren, för vilken delningsstatistik söks.

**MVPD**

Om du är programmerare visar kolumnen vilken MVPD som prenumerantkontot tillhör.

**Prenumerant-ID**

Särskilt konto som vi talar om på rad.

**Minsta antal enheter**

Det faktiska antalet enheter (det ströminnehållet) är nästan säkert större än det minsta antalet enheter som anges för ett visst konto.

>[!NOTE]
>
>Det faktiska antalet enheter (det ströminnehållet) är säkert större än det minsta antalet enheter som anges för ett visst konto.

**Minsta antal personer**

Det absoluta minsta antalet personer som var aktiva direktuppspelningsinnehåll som använde dessa enheter.

>[!NOTE]
>
>Det faktiska antalet personer (som strömmar innehåll) är nästan säkert mycket större än det minsta antalet personer som anges för ett visst konto.

**[!UICONTROL # IPs]**

Antal IP-adresser som innehållet direktuppspelas från.

**[!UICONTROL # Locations]**

Antal platser (baserat på postnummer) från vilka innehållet direktuppspelas.

**[!UICONTROL # Cities]**

Antal städer där strömningen har ägt rum.

**[!UICONTROL # States]**

Antal lägen där strömningen har ägt rum.

**[!UICONTROL # Clusters]**

Antalet distinkta [kluster](/help/accountiq/product-concepts.md#cluster-def) där strömningen har ägt rum.

**[!UICONTROL Geographic span (miles)]**

Det maximala avståndet mellan de direktuppspelningsplatser som är associerade med kontot.

**[!UICONTROL # AuthN OK]**

Antalet gånger som användarna har loggat in under perioden med det kontot.

**[!UICONTROL # AuthZ OK]**

Antal gånger ett MVPD har auktoriserat en ström eller beviljat åtkomst (till innehåll) till det kontot.

>[!NOTE]
>
>The **[!UICONTROL # AuthZ OK]** är relaterad till **[!UICONTROL # Play Requests]**; den är mindre än **[!UICONTROL # Play Requests]** eftersom Adobe cachar de tillstånd som normalt gäller för luftvärdighetsbevis (MVPD) i 24 timmar.

**[!UICONTROL # Play Requests]**

Det faktiska antalet strömmar under tidsperioden.

**[!UICONTROL # Channels]**

Totalt antal olika kanaler som kontot har bevakat under tidsperioden.

>[!NOTE]
>
>**[!UICONTROL # Channels]** innehåller de kanaler som inte nödvändigtvis tillhör den inloggade programmeraren.
>
>Det här numret för kontot visas eftersom kontot tittade på din kanal, men även på andra kanaler under den perioden.

**Användningsmönster**

Numren i den här kolumnen är identifierare som mappar till ett av de 14 mönstren som vi identifierar alla användarkonton som.

*Tabell: Användningsmönsteridentifierare i exporterad CSV-mappning med användningsmönster*

| ID | 1 | 2 | 3 | 4 | 5 och 8 | 6 | 7 | 9 | 10 och 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Användningsmönster | Vanlig användare | Resa eller dator | Stor familj | Nära släkt och vänner | Delning i sociala grupper | Stor grupp av vänner | Samtidig strömning | Community-delning | Osäkert beteende | Liten familj | Andra hemmet | Onormal användning |

{style="table-layout:auto"}

**Sannolikhet för delning**

Sannolikheten för delning är sannolikheten att det specifika kontot delar sina autentiseringsuppgifter.

>[!NOTE]
>
> Medelvärdet för delningssannolikheten för alla konton (i det valda segmentet) används för att beräkna [delningsnivå](/help/accountiq/dashboard.md#sharing-level) i [Aggregerad delningspoäng](/help/accountiq/dashboard.md#aggregated-sharing).
