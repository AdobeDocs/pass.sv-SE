---
title: Versionsinformation om Adobe Pass Authentication 2.65.1
description: Versionsinformation om Adobe Pass Authentication 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.65.1 {#authn-2651-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-2651}

* [Byggnummer](#build-number-2651)
* [Versionsöversikt](#release-overview-2651)

### Byggnummer {#build-number-2651}

Adobe Pass-autentisering: adobe-pass-**2.65.1**

Releasedatum: **06/20/2023 - 06/22/2023**

### Versionsöversikt {#release-overview-2651}

I den här versionen har följande lagts till:

Från och med den här versionen kommer vi att införa teknisk support för att lägga till hastighetsbegränsande regler i alla våra API:er, för att skydda hela ekosystemet från felaktig användning.

Det första målet är att övervaka hur ansökningarna beter sig för att fastställa korrekta gränsvärden och inga begränsningar kommer att tillämpas som en del av denna release. Om trafiken som genereras av en viss applikation överskrider vissa gränser kommer vi att samarbeta med varje kund för att validera implementeringen.

Regler för hastighetsbegränsning kommer att tillämpas senare i år, efter att vi har validerat trafik som genererats från alla kundens tillämpningar.
