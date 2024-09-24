---
title: Exempel på användningsrapporter för övervakning av samtidig användning
description: Exempel på användningsrapporter för övervakning av samtidig användning
source-git-commit: ca9bfb964ad7e7437bbea4704bca4ac5105874f1
workflow-type: tm+mt
source-wordcount: '2374'
ht-degree: 0%

---

# Exempel på användningsrapporter för övervakning av samtidig användning{#cm-usage-reports-examples}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Exempel på månatliga rapporter {#monthly-reports-examples}

| Namn | Dimensioner | URL | Mått |
|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Månadsstatistik för olika MVPD-program | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot; | cmu/v2/year/month/mvpd | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Månadsstatistik för olika MVPD-klientorganisationer | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;tenant&quot; | cmu/v2/year/month/mvpd/tenant | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Månadsstatistik för cross-MVPD-klient/tillämpning | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;tenant&quot;, &quot;application-id&quot; | cmu/v2/year/month/mvpd/tenant/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Månadsstatistik för MVPD-klient/tillämpning med programnamn och plattform | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;tenant&quot;, &quot;application-id&quot;, &quot;application&quot;, &quot;platform&quot; | cmu/v2/year/month/mvpd/tenant/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Månadsrapport per klient | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot; | cmu/v2/tenant/year/month | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik för klientprogram | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik för klientprogram med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik för mvpd-plattformskanal per klient | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik för mvpd-plattformskanal per klient med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik för kanal/plattform per klient | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/channel/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik för kanal/plattform per klient med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik per mvpd | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot; | cmu/v2/mvpd/year/month | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsstatistik per mvpd-klient | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;tenant&quot; | cmu/v2/mvpd/year/month/tenant | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Månadsrapport för samtidighetsnivå | &quot;year&quot;, &quot;month&quot;, &quot;concurrency-level&quot; | cmu/v2/year/month/concurrency-level | &quot;concurrency-level&quot;, &quot;users&quot; |
| Månadsrapport för samtidighetsnivå per innehavare | &quot;year&quot;, &quot;month&quot;, &quot;concurrency-level&quot;, &quot;tenant&quot; | cmu/v2/year/month/concurrency-level/tenant | &quot;concurrency-level&quot;, &quot;tenant&quot;, &quot;users&quot; |
| Månadsrapport för samtidighetsnivå per innehavare mvpd | &quot;year&quot;, &quot;month&quot;, &quot;concurrency-level&quot;, &quot;tenant&quot;, &quot;mvpd&quot; | cmu/v2/year/month/concurrency-level/tenant/mvpd | &quot;concurrency-level&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;users&quot; |
| Månadsrapport för aktivitetsnivå | &quot;year&quot;, &quot;month&quot;, &quot;activity-level&quot; | cmu/v2/year/month/activity-level | &quot;activity-level&quot;, &quot;users&quot; |
| Månadsrapport för aktivitetsnivå per innehavare | &quot;year&quot;, &quot;month&quot;, &quot;activity-level&quot;, &quot;tenant&quot; | cmu/v2/year/month/activity-level/tenant | &quot;activity-level&quot;, &quot;tenant&quot;, &quot;users&quot; |
| Månadsrapport för aktivitetsnivå per innehavare mvpd | &quot;year&quot;, &quot;month&quot;, &quot;activity-level&quot;, &quot;tenant&quot;, &quot;mvpd&quot; | cmu/v2/year/month/activity-level/tenant/mvpd | &quot;activity-level&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;users&quot; |

## Exempel på dagliga rapporter {#daily-reports-examples}

| Namn | Dimensioner | URL | Mått |
|------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Daglig statistik för multi-tenant mvpd/platform | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/tenant/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Flerspråksstatistik för mvpd/plattform med programnamn | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/tenant/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Daglig statistik för olika typer av plattformar | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;tenant&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/tenant/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Daglig statistik för olika plattformar med programnamn | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;tenant&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/tenant/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Daglig statistik för olika kanaler/plattformar | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;tenant&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/tenant/channel/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Daglig status för olika kanaler/plattformar med programnamn | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;tenant&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/tenant/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Daglig statistik för olika valutor | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, mvpd&quot; | cmu/v2/year/month/day/mvpd | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| MVPD-tenantens dagliga statistik | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;tenant&quot; | cmu/v2/year/month/day/mvpd/tenant | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| MVPD-klient-/programdagsstatistik | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;tenant&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/mvpd/tenant/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| MVPD-klient-/programdagsstatistik för olika program med programnamn och plattform | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, mvpd&quot;, &quot;tenant&quot;, &quot;application-id&quot;, &quot;application&quot;, &quot;platform&quot; | cmu/v2/year/month/day/mvpd/tenant/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Daglig rapport per klient | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot; | cmu/v2/tenant/year/month/day | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig statistik för tillämpning per klient | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/day/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig statistik för klientprogram med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/day/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Per tenant mvpd daglig statistik | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/day/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig mvpd-statistik per innehavare med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/day/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig statistik för kanal/plattform per innehavare | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/day/channel/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig status per klient-kanal/plattform med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/day/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig statistik per MVPD | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot; | cmu/v2/mvpd/year/month/day | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig statistik per mvpd-klient | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;tenant&quot; | cmu/v2/mvpd/year/month/day/tenant | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Daglig rapport för samtidighetsnivå | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;concurrency level&quot; | cmu/v2/year/month/day/concurrency-level | &quot;concurrency-level&quot;, &quot;users&quot; |
| Daglig rapport för samtidighetsnivå per innehavare | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;concurrency level&quot;, &quot;tenant&quot; | cmu/v2/year/month/day/concurrency-level/tenant | &quot;concurrency-level&quot;, &quot;tenant&quot;, &quot;users&quot; |
| Daglig rapport för samtidighetsnivå per innehavare mvpd | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;concurrency level&quot;, &quot;tenant&quot;, &quot;mvpd&quot; | cmu/v2/year/month/day/concurrency-level/tenant/mvpd | &quot;concurrency-level&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;users&quot; |
| Daglig rapport om aktivitetsnivå | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;activity-level&quot; | cmu/v2/year/month/day/activity-level | &quot;activity-level&quot;, &quot;users&quot; |
| Daglig rapport för aktivitetsnivå per klient | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;activity-level&quot;, &quot;tenant&quot; | cmu/v2/year/month/day/activity-level/tenant | &quot;activity-level&quot;, &quot;tenant&quot;, &quot;users&quot; |
| Daglig rapport på aktivitetsnivå per innehavare mvpd | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;activity-level&quot;, &quot;tenant&quot;, &quot;mvpd&quot; | cmu/v2/year/month/day/activity-level/tenant/mvpd | &quot;activity-level&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;users&quot; |

## Exempel på timrapporter {#hourly-reports-examples}

| Namn | Dimensioner | URL | Mått |
|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Timstatistik för olika innehavare | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/tenant/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för flerklientprogram med programnamn och plattform | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;application-id&quot;, &quot;application&quot;, &quot;platform&quot; | cmu/v2/year/month/day/hour/tenant/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för mvpd/plattform för olika innehavare | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/tenant/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för mvpd/plattform för olika innehavare med programnamn | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/hour/tenant/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för olika innehavare | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/tenant/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för olika innehavarplattformar med programnamn | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/hour/tenant/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för olika kanaler/plattformar | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/tenant/channel/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för olika kanaler/plattformar med programnamn | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/hour/tenant/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för korsprogrammering | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot; | cmu/v2/year/month/day/hour/mvpd/ | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för olika MVPD-hyresgäster | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;tenant&quot; | cmu/v2/year/month/day/hour/mvpd/tenant | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för olika MVPD-klient/tillämpning | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;tenant&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/mvpd/tenant/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik för olika MVPD-klient/program med programnamn och plattform | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;tenant&quot;, &quot;application-id&quot;, &quot;application&quot;, &quot;platform&quot; | cmu/v2/year/month/day/hour/mvpd/tenant/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;completed-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot; |
| Timstatistik per klient | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot; | cmu/v2/tenant/year/month/day/hour | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Timstatistik för per tenant-program | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/day/hour/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Timstatistik för enskilda program med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/day/hour/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Per tenant mvpd timstatistik | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/day/hour/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Per-tenant mvpd timstatistik med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/day/hour/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Timstatistik för kanal/plattform per innehavare | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/tenant/year/month/day/hour/channel/platform/application-id | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Timstatistik för kanal/plattform per innehavare med programnamn | &quot;tenant&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/tenant/year/month/day/hour/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Timstatistik per MVPD | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot; | cmu/v2/mvpd/year/month/day/hour | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Timstatistik per MVPD-klient | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;tenant&quot; | cmu/v2/mvpd/year/month/day/hour/tenant | &quot;active-users&quot;, &quot;active-sessions&quot;, &quot;started-sessions&quot;, &quot;complete-sessions&quot;, &quot;failed-try&quot;, &quot;dismissing-sessions&quot;, &quot;Kill-sessions&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot; , &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
