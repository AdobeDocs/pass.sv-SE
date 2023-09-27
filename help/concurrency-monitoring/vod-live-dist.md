---
title: Hur man skiljer mellan VOD och Live Content i Concurrency Monitoring
description: Hur man skiljer mellan VOD och Live Content i Concurrency Monitoring
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# Så här gör du: Skilja mellan VOD och Live Content i Concurrency Monitoring {#dist-vod-live}

**F:** Kan tjänsten för övervakning av samtidig användning skilja mellan vilken typ av innehåll som spelas upp (Live-innehåll kontra Video on demand)?



**S:** Övervakning av samtidig användning kan inte direkt skilja mellan live-innehåll och on demand-video (VOD). Videospelaren måste känna till vilken typ av innehåll som spelas upp och skicka informationen under [sessionsinitieringsanrop](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (krävs för övervakning av samtidig användning). Det reguljära arbetsflödet ser ut så här:

1. Kunder med Concurrency Monitoring definierar en uppsättning metadata som de vill ha regler implementerade på (t.ex. content-type=live|vod, device-type=mobile|console|desktop).
1. Concurrency Monitoring-teamet implementerar den önskade principen. Exempel:
   1. if content-type=live, max streams=3, latest wins
   1. if content-type=vod, max streams=1, latest wins

1. När slutanvändaren spelar upp innehållet måste videospelaren skicka värden för de metadatafält som har etablerats som en del av en princip.

1. Tjänsten för övervakning av samtidig användning (Concurrency Monitoring), som baseras på den definierade principen och mottagna värden, kommer att fatta ett beslut (play/stop).

1. Videospelaren måste följa beslutet för att systemet ska fungera.



## Relaterad information {#related-info-vod-live-dist}

* [Standardmetadataattribut för övervakning av samtidig användning](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [API-översikt för övervakning av samtidig användning](/help/concurrency-monitoring/cm-api-overview.md)
