---
title: Bilaga B "Felsökningstips"
description: Bilaga B "Felsökningstips"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Bilaga B: Felsökningstips {#appendix-b-debugging-tips}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.


## Rensar temporära data {#clearing-temporary-data}

Adobe Pass Authentication lagrar temporära data som webbläsarcache, LSO-cache och cookies. Det är viktigt att du rensar tillfälliga data för att vara säker på att du får en ren status när du testar.

- [Rensa webbläsarens cache och cookies](#clearing-the-browser-cache-and-cookies)
- [Rensar LSO-cache](#clearing-lsos-cache)


## Rensa webbläsarens cache och cookies {#clearing-the-browser-cache-and-cookies}

Den är webbläsartillförlitlig, men i Firefox: &quot;Verktyg&quot; -\> &quot;Rensa senaste historik...&quot; -\> På &quot;Tidsintervall för att rensa:&quot; väljer du &quot;Allt&quot; och på &quot;Detaljer&quot;: markera &quot;Cookies&quot; och &quot;Cache&quot; -\> Klicka på &quot;Rensa nu&quot;.


## Rensar LSO-cache {#clearing-lsos-cache}

Öppna [Hjälp om Flash Player](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Välj ```entitlement.\*``` (beroende på vad som har testats) och klicka på Ta bort webbplats.


## Felsökningsverktyg {#tools}

Ingenjörer av Adobe Pass-autentisering använder följande felsökningsverktyg:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (fungerar med felsökningsversionen av Flash Player)
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Tråka - <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
