---
title: Versionsinformation om Adobe Pass Authentication 3.5.0
description: Läs om de nya funktionerna, ändringarna och de kända problemen i den här versionen.
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---


# Versionsinformation om Adobe Pass Authentication 3.5.0

Senaste uppdatering: Tue Dec 09 2025 00:00:00 GMT+000 (universaltid)

* Ämnen:
* Autentisering

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](https://experienceleague.adobe.com/sv/docs/pass/authentication/product-announcements).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-350}

* [Byggnummer](#build-number-350)
* [Versionsöversikt](#release-overview-350)

### Byggnummer {#build-number-350}

Adobe Pass-autentisering: adobe-pass-**3.5.0**\
Releasedatum: **12/09/2025 - 12/11/2025**

### Versionsöversikt {#release-overview-350}

#### REST API v2

* En ny dimension har lagts till i ESM (&quot;api&quot;) för att hjälpa kunderna att skapa rapporter som skulle skilja på händelser baserat på implementeringstyp: SDK, REST V1 eller REST V2.

#### Felkorrigeringar

* Ett problem har korrigerats där anpassade nedbrytningsmeddelanden som angetts i TVE Dashboard inte visades i felinformationen för REST API V2.
* Korrigerade ett problem i REST API V2 där `authenticated_profile_expired` felkod inte returnerades när autentiserade profiler upphörde att gälla.
* Ett problem har korrigerats där beräkningar av auktoriseringsfördröjning och TTL-värden för preflight var felaktiga i REST API V2.
* Ett problem där inkonsekvent svarsformat returnerades när DCR-token upphörde att gälla har åtgärdats.
