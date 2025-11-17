---
title: ESM Dashboard
description: Lär dig använda ESM Dashboard för att övervaka berättiganden och händelsedata mellan MVPD partners.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---


# ESM Dashboard {#esm-dashboard}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

ESM Dashboard ger en enhetlig vy över berättiganden och händelsedata som hjälper dig att övervaka prestanda, identifiera avvikelser och förstå användaråtkomstmönster i MVPD partners. Den här guiden förklarar hur du använder kontrollpanelens filter, tolkar rapporter och förstår viktiga mätvärden över konfigurerbara tidsintervall.

![ESM-instrumentpanelsvy](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## Användningsexempel {#use-cases}

- Visualisera trender per plattform eller MVPD
- Jämför MVPD prestanda
- Förstå kundanvändning per program

Mer information om ESM-data och händelser finns i [Entitlement Service Monitoring Overview](https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview).

## Rapporter {#reports}

Följande rapporter är tillgängliga:

### Spela upp begäranden {#play-requests}

Visar antalet uppspelningsbegäranden för det valda tidsintervallet.

**Diagramformat** - linje.

### Autentiseringskonvertering {#authentication-conversion}

Visar förhållandet mellan lyckade autentiseringshändelser och totalt antal autentiseringsförsök.

**Diagramformat** - vågrätt fält

### Auktoriseringskonvertering {#authorization-conversion}

Visar förhållandet mellan lyckade autentiseringshändelser och totalt antal autentiseringsförsök.

**Diagramformat** - vågrätt fält

### Auktoriseringsfördröjning {#authorization-latency}

Visar den genomsnittliga fördröjningen (i millisekunder) för MVPD-svar.

**Diagramformat** - linje.

### MVPD-användning {#mvpd-usage}

Visar en jämförelse av antalet unika användare per MVPD.

**Diagramformat** - liggande område.

### Slutförda autentiseringar {#successful-authentications}

Visar det totala antalet lyckade autentiseringar för det valda tidsintervallet.

**Diagramformat** - linje.

### Slutförda auktoriseringar {#successful-authorizations}

Visar det totala antalet lyckade auktoriseringar (MVPD &#39;Permit&#39;-svar) för det valda tidsintervallet.

**Diagramformat** - linje.

## Åtgärder {#actions}

### Visa efter {#view-by}

För varje diagram finns listrutan &quot;Visa efter&quot; där du kan välja exakt vilka data som ska visas.

- **Aggregate** - visar övergripande data
- **MVPD-filer/kanaler/plattformar** - visar de valda filtren

### Ladda ned {#download}

Du kan hämta rådata:

- **Hämta diagramdata som CSV** - hämtar data för ett specifikt diagram
- **Hämta alla data som CSV** - hämtar alla ESM-data i alla diagram

## Filter {#filters}

Använd filter för att begränsa datauppsättningen och fokusera analysen. Följande filter är tillgängliga:

- **Kanal**: innehåller alla tillgängliga kanaler (varumärken)
- **MVPD**: fokusera på en eller flera leverantörer
- **Plattform**: webb, mobil, ansluten TV eller enhetsfamilj

Om du vill lägga till ett nytt filter väljer du knappen Lägg till filter.

På sidan&quot;Datauppsättningsfilter&quot; kan du dra och släppa de filter som behövs.

![ESM Dashboard-filter](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

För varje avsnitt kan du ta bort filter individuellt eller ta bort hela markeringen.

### Filtertips {#filter-tips}

- Kombinera flera filter för att isolera en kohort (t.ex. en MVPD på en mobilplattform för en kanal).
- Lägg inte till filter för att zooma ut och skapa en baslinje innan du borrar in.

## Tidsintervall {#time-intervals}

Styr analysfönstret och granulariteten.

![Tidsintervall för ESM-instrumentpanelen](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### Datumintervall {#date-range}

**Förinställningar**: I dag, Aktuell vecka, Senaste 7 dagarna, Aktuell månad, Senaste 30 dagarna, Senaste 3 månaderna, Senaste 6 månaderna, Senaste 12 månaderna

**Egen**: Välj önskat tidsintervall

### Kornighet {#granularity}

Varje dag/månad
