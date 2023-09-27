---
title: Användningsrapporter för övervakning av samtidig användning
description: Användningsrapporter för övervakning av samtidig användning
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '723'
ht-degree: 0%

---


# Användningsrapporter för övervakning av samtidig användning {#cm-usage-reports}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.



## Ökning {#usage-rep-overview}

The **Användningsrapporter för övervakning av samtidig användning** är tillgänglig via ett REST API som ger insikt i den samtidiga användningen enligt vad som rapporterats av kundens program.

## Förutsättningar {#usage-rep-prerequisites}

För att få åtkomst till produkten Concurrency Monitoring Usage Reports måste kunden först kontakta Concurrency Monitoring [Supportteam](mailto:tve-support@adobe.com) och de kommer att utföra de åtgärder som krävs för att ge dig tillgång till API-produkten.

## Allmänna rapportmått och uppdelningar {#general-rep-metrics-breakdown}

### Användningsrapporter kan övervaka följande mått:{#monitor-metrics}

| Namn | Beskrivning |
|:---|:---|
| active-users | antal distinkta användarkonton som har initierat minst en direktuppspelningssession under intervallet, uppdelat efter tidgranularitet |
| aktiva sessioner | antal direktuppspelningssessioner som har rapporterat aktivitet under intervallet (det inkluderar inte misslyckade försök, sessioner som har fått ett nekande svar för initieringsanropet) |
| startade sessioner | antal direktuppspelningssessioner som startades inom intervallet |
| slutförda sessioner | antal direktuppspelningssessioner som avslutats under intervallet (antingen explicit eller på grund av timeout) |
| misslyckade försök | antal sessionsinitieringsförsök som har fått svar på nekande |
| avvisade sessioner | antal direktuppspelningssessioner som har slutförts under uppspelning (i pulsslag) på grund av en principöverträdelse. Det här måttet är bara meningsfullt när en FIFO- eller last_one_wins-princip har antagits. |
| avbrutna sessioner | antal direktuppspelningssessioner som uttryckligen stoppades när en ny session startades (via X-Terminate-huvudet) |
| duration_0-15 | antal strömmar med en varaktighet mellan 0 och 15 minuter |
| duration_15-30 | antal strömmar med en varaktighet på mellan 15 och 30 minuter |
| duration_30-60 | antal strömmar med en varaktighet på mellan 30 och 60 minuter |
| duration_60-120 | antal strömmar med en varaktighet mellan 60 och 120 minuter |
| duration_2h-4h | antal strömmar med en varaktighet mellan 2 timmar (120 minuter) och 4 timmar |
| duration_4h-8h | antal strömmar med en varaktighet mellan 4 timmar och 8 timmar |
| duration_8h-16h | antal strömmar med en varaktighet mellan 8 timmar och 16 timmar |
| duration_16h-1d | antal strömmar med en varaktighet mellan 16 timmar och 1 dag (24 timmar) |
| duration_1d-3d | antal strömmar med en varaktighet mellan 1 dag och 3 dagar |
| duration_3d-7d | antal strömmar med en varaktighet mellan 3 dagar och 7 dagar |
| duration_1w-1m | antal strömmar med en varaktighet mellan 1 vecka (7 dagar) och 1 månad (30 dagar) |
| duration_over-1m | antal strömmar med en varaktighet över en månad (30 dagar) |

### Användningsrapporter kan filtrera mätvärdena ovan med följande mått: {#dimensions-2-filter-metrics}

| Dimensionens namn | Beskrivning |
|:---|:---|
| år | Det fyrsiffriga året |
| månad | Månad på året (1-12) |
| dag | Dag i månaden (1-31) |
| timme | Timme på dagen |
| minut | Minut av timmen |
| program | Programnamnet som är registrerat i Concurrency Monitoring och som används för att hantera sessioner |
| application-id | Program-ID som är registrerat i samtidighetsövervakning som används för att hantera sessioner |
| kanal | Kanalmetadata som skickas när sessionen initieras (markeras som Okänd om inga metadata skickas) |
| mvpd | MVPD-filen som tillhandahålls vid sessionshantering |
| plattform | Plattformsmetadata som tillhandahålls vid sessionsinitiering eller fördefinierade för ett program på konfigurationsnivå |

## Mätvärden och uppdelningar för samtidighetsrapport {#concurrency-reports-metrics-breakdown}

Från och med Concurrency Monitoring version 2.9.0 har vi infört en ny rapport för att förstå samtidig användning: ett histogram för **samtidighetsnivå** och **aktivitetsnivå**.

Huvudsyftet med den här rapporten är att hjälpa er att förstå effekten av att fastställa en policy med en viss samtidighetsgräns och att ge er tillräckligt med information för att avgöra om ni bör höja gränsen.

### Användningsrapporter kan övervaka följande mått: {#metrics-usage-rep-users}

| Dimensionens namn | Beskrivning |
|:---|:---|
| användare | antal användare som har nått varje samtidighets-/aktivitetsnivå |

### Användningsrapporter kan filtrera mätvärdena ovan med följande mått: {#dimensions-to-filter-metrics}

| Dimensionens namn | Beskrivning |
|:---|:---|
| år | Det fyrsiffriga året |
| månad | Månad på året (1-12) |
| dag | Dag i månaden (1-31) |
| samtidighetsnivå | Representerar distinkta **dataströmaktivitet som godkändes vid sessionsinitieringsfasen** för att en användare ska kunna se hur många samtidiga strömmar **öppnades** av en användare och förstå effekten av att tillämpa en viss samtidighetsgräns |
| aktivitetsnivå | Representerar distinkta **direktuppspelningsaktivitet (oavsett status: startad, aktiv, stoppad, avvisad)** för att en användare ska kunna se hur många samtidiga strömmar som öppnats av en användare och förstå effekten av att tillämpa en viss samtidighetsgräns |
| mvpd | MVPD-filen som tillhandahålls vid sessionshantering |