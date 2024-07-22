---
title: Allmänna användningsrapporter
description: Läs mer om allmänna användningsrapporter
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: 4a8a73d6c67508e88ba3ffbb9033b7e339f4fe8f
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 0%

---

# [!UICONTROL General usage] rapporter {#general-usage-reports}

[!UICONTROL Account IQ]-rapporter är grundläggande analysverktyg som du kan använda för att fördjupa dig i dina data och isolera [kohorter](/help/accountiq/product-concepts.md#segmet-def), identifiera avvikelser och skapa en förståelse för dina kontoegenskaper.

[!UICONTROL General usage]-rapportsidan innehåller verktyg för att ta bort undergruppsmått baserat på antalet kontoenheter som används, IP-adresser som har identifierats och deras respektive postnummer.

Rapporterna är alla baserade på det aktuella segment som valts på panelen [Segment och tidsintervall](/help/accountiq/segments-timeinterval.md). Du kan finjustera markeringen och ytterligare begränsa den genom att ange tröskelvärden (antal enheter, antal IP-adresser och antal postkoder) på panelen [Översikt över ögonblicksbild - konton över tröskelvärdena](#snapshot-overview).

## Spela upp förfrågningar och unika prenumeranter {#playreq-uniquesubs}

I linjediagrammen här visas hur värdena ändras över tid, t.ex. uppspelningsbegäranden och unika prenumeranter i ett valt tidsintervall för det definierade segmentet.

+++ D2C-tjänster: uppspelningsförfrågningar/unika prenumeranter

![](assets/d2c-line-graph-gu.png)


*Spela upp begäranden/unika prenumeranter för D2C-tjänster*

+++

+++Programmerare: Spela upp begäranden/unika prenumeranter

![](assets/progr-line-graph-gu.png)


*Spela upp begäranden/unika prenumeranter för programmerare*

+++

+++MVPD: Unika prenumeranter

![](assets/mvpd-line-graph-gu.png)

*Unika prenumeranter för MVPD*

+++

<br/>

X-axeln representerar tiden baserat på det aktuella intervallet och y-axeln representerar grundläggande aktivitetsmått för prenumeranten under den perioden. Med linjediagrammen kan du visualisera och jämföra aktiviteten för prenumeranterna i det aktuella segmentet. Beroende på vilken version av Account IQ det är kan måtten vara:

* **AuthN OK**: Antal lyckade autentisering. Läs mer om [AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**: Antal lyckade auktoriseringar. Läs mer om [AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Uppspelningsbegäranden**: Antal uppspelningsbegäranden. Läs mer om [Spela upp begäranden](/help/accountiq/product-concepts.md#play-requests-def).

* **Unika prenumeranter**: Antal slutförda unika prenumeranter. Läs mer om [Unika prenumeranter](/help/accountiq/product-concepts.md#unique-subscriber-def).

>[!NOTE]
>
>Vilka mätvärden som är tillgängliga varierar beroende på vilken version av Account IQ du har.

## Översikt över ögonblicksbilder - konton över tröskelvärden {#snapshot-overview}

Finjustera analyser och rapporter med hjälp av det här extra filtret för att ange olika användningströsklar. När du har valt ett segment kan du även använda följande filter för att analysera prenumerantbeteendet ytterligare:

* Tröskelvärde för antal enheter

* Antal IP-tröskelvärden

* Antal postkoder, tröskel

När du uppdaterar tröskelvärden i [Kontoningssegment baserat på valda tröskelvärden](#account-segments-basedon-segments) visas effekten i:

* [Enheter per vecka (eller månad) per konto](#devices-week-account)

* [Platser per vecka (eller månad) per konto](#locations-week-account)

* [IP-adresser per vecka (eller månad) per konto](#ip-week-account)

* [Historisk vy över kontosegment](#account-segment-historical-view)

>[!NOTE]
>
>Varje tröskelvärde sätts till standardvärdet 4. På sidan General Usage (Allmän användning) visas en analys för prenumeranter som använder fler än fyra enheter, som förbrukar innehåll från mer än fyra olika IP-adresser, *och*, mer än fyra olika postnummer.

### Kontonegment baserat på valda tröskelvärden {#account-segments-basedon-segments}

På panelen **Kontonegsnitt som baseras på valda tröskelvärden** finns alternativ för att ange tröskelvärden (mellan 1 och 10) för antal enheter, antal IP-adresser och antal postkoder.

I diagrammet visas följande:

* Absolut antal abonnentkonton.

* Procentandel av det totala antalet abonnentkonton i segmentet som använder antalet enheter, från antalet IP-adresser, i antalet postkoder som anges av tröskelvärdena.

![](assets/select-thresholds.png)

## Enheter per vecka (eller månad) per konto {#devices-week-account}

I det här stapeldiagrammet visas hur abonnenterna använder sina enheter för att komma åt innehåll.

X-axeln visar Antal konton och y-axeln visar Antal enheter. Baserat på det tröskelvärde som du anger för antal enheter per konto anger det absoluta antalet prenumerantkonton som förbrukar innehåll från ett visst antal enheter under en veckas tid.

![](assets/bar-gr-devices-w-acc.png)

När du hovrar över ett fält (specifikt för antalet enheter) visas en etikett som ger information om antalet prenumerantkonton (och procentandelen av det totala antalet prenumerantkonton i segmentet) som direktuppspelar kanalinnehåll med dessa många enheter under en vecka.

I diagrammet markeras även följande:

* En röd linje som markerar tröskelvärdet som du anger.

* En grön linje som anger medelvärdet för antalet olika enheter som används av ett prenumerantkonto per vecka (eller månad).

Nöden ger en alternativ vy över de enheter som används av konton i det aktuella segmentet över det angivna tröskelvärdet.

![](assets/donut-devices-w-acc.png)

## Platser per vecka (eller månad) per konto {#locations-week-account}

På samma sätt som måttet för [enheter per vecka (eller månad) per konto](#devices-week-account) kan du analysera användningen av prenumerantkontot från olika platser med hjälp av värdena Platser per vecka (eller månad) per konto. X-axeln visar Antal konton och y-axeln visar Antal platser.

![](assets/graph-loc-week-acc.png)

När du har angett ett tröskelvärde för antalet platser kan du använda diagrammet för att identifiera följande:

* Antal (och procentandel) prenumeranter som konsumerar innehåll från (ett visst) x antal platser under en vecka.

* Procentandel av det totala antalet prenumerantkonton som visar innehåll från fler platser än tröskelvärdet.

* Jämför veckomedelvärdet (antal olika platser för ett konto) med tröskelvärdet.

## Ips per vecka (eller månad) per konto {#ip-week-account}

Ungefär som måttet för **Antal platser per vecka per konto** kan du med hjälp av **antalet IP-adresser per vecka per konto** utvärdera hur mycket som har ändrats i direktuppspelningskällan för det aktuella segmentet.

X-axeln visar Antal konton och y-axeln visar Antal IP.

![](assets/graph-ip-week-acc.png)

När du har definierat ett segment och angett ett tröskelvärde för antalet IP-adresser kan du använda diagrammet för att identifiera följande:

* Antal (och procentandel) prenumeranter som konsumerar innehåll från ett visst antal IP-adresser per vecka.

* Procentandel av det totala antalet prenumerantkonton som visar innehåll från fler IP-adresser än tröskelvärdet.

* Jämför veckomedelvärdet (antal olika IP-adresser för ett konto) med tröskelvärdet.

## Konton, segmenthistorikvy {#account-segment-historical-view}

I diagrammet med historikvyfältet kan du jämföra användningsstatistik över olika tidsintervall. Dessutom visas de olika användningsmåtten tillsammans, till exempel [Enheter per vecka (eller månad) per konto](#devices-week-account), [Platser per vecka (eller månad) per konto](#locations-week-account) och [IP-adresser per vecka (eller månad) per konto](#ip-week-account).

* X-axeln ritar upp tidsintervallet och y-axeln visar antalet abonnentkonton, enheter, platser och IP-adresser.

* De orangefärgade strecken betecknar segment i olika tidsintervall.

* I linjediagrammet visas ändringarna i [Enheter per vecka (eller månad) per konto](#devices-week-account), [Platser per vecka (eller månad) per konto](#locations-week-account) och [IP-adresser per vecka (eller månad) per konto](#ip-week-account) över tidsintervallet baserat på tröskelvärdet.

![](assets/historical-view.png)

* De blå staplarna anger det totala antalet aktiva prenumeranter i branschen under ett tidsintervall.

* Du kan markera specifika teckenförklaringar och de hjälper dig att skala diagrammet.

![](assets/historical-view-total.png)
