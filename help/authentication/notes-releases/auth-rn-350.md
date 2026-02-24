---
title: Versionsinformation om Adobe Pass Authentication 3.5.0
description: Läs om de nya funktionerna, ändringarna och de kända problemen i den här versionen.
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
source-git-commit: 1cbddf081fc7d57a187c9701e4ade8593baf8759
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.5.0

Senaste uppdatering: Tue Dec 09 2025 00:00:00 GMT+000 (universaltid)

* Ämnen:
* Autentisering

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements).

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

## Underhållsuppdatering - februari 2026 {#maintenance-update-february-2026}

Adobe Pass-autentisering: adobe-pass-**3.5.0.5**\
Releasedatum: **02/24/2026 - 02/26/2026**

Den här underhållsuppdateringen innehåller viktiga förbättringar som förbättrar systemets tillförlitlighet och säkerhet:

### Förbättringar

* Förbättrad hantering av försämring av autentiseringen för proxiderade MVPD-konfigurationer i REST API V2, vilket ger ett mer konsekvent beteende vid avbrott i MVPD-tjänster.
* Förbättrad validering av URL-parametrar och omdirigeringshantering för att stärka säkerhetskontrollerna och förbättra den övergripande systemintegriteten.
