---
title: Versionsinformation om Adobe Pass Authentication 2.63
description: Versionsinformation om Adobe Pass Authentication 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.63 {#authn-263-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-263}

* [Byggnummer](#build-number-263)
* [Versionsöversikt](#release-overview-263)

### Byggnummer {#build-number-263}

Adobe Pass-autentisering: adobe-pass-**2.63**

Releasedatum: **09/20/2022 - 09/22/2022**

### Versionsöversikt {#release-overview-263}

#### Förbättra plattformsidentifieringsmekanismen

* Från och med den här versionen har vi förbättrat den mekanism som används för att identifiera en enhet och den kommer inte längre att vara beroende av implementeringen på klientsidan. Detta ger en exaktare detaljrikedom vid tillämpning av affärsregler på plattformsnivå och en bättre förståelse för trafikvärden i ESM-rapporter.

* En ny ESM-release kommer snart, med nya och förbättrade rapporter som visar de plattformsrelaterade fälten.

* Mer information om de planerade ändringarna får du av din Adobe TAM.

#### MVPD självförsämring

Den här funktionen ger distributörer av videoprogrammeringstjänster möjlighet att tillfälligt kringgå sina egna autentiserings- och auktoriseringsslutpunkter för högtrafikscenarier när belastningen på dessa respektive slutpunkter blir för hög.

#### Lägg till proxyID i huvudet för auktoriseringsanrop

Den här funktionen lägger till ID:t för en proxyserver-MVPD i auktoriseringssamtalets huvud. Det gör att Synacor kan ställa in affärsregler för varje enskild proxiderad (t.ex. dirigering till olika domäner per proxiderad MVPD).

#### TVE Dashboard

I den här versionen har ett problem korrigerats där authN eller authZ TTL som angetts på MVPD-nivå inte beräknades korrekt i konfigurationsrapporterna.

#### JavaScript SDK 4.6.0

* Användning av funktionen `eval` har tagits bort, vilket gör att SDK följer Content Security Policy.
* Korrigerade ett problem som förhindrade autentiseringsflödet från att slutföras när webbläsarens lokala lagring rensades explicit av ett partnerprogram.
