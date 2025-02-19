---
title: Adobe Pass Authentication JavaScript 4.4.0 Release Notes
description: Adobe Pass Authentication JavaScript 4.4.0 Release Notes
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.4.0 Release Notes {#javascript-sdk-440-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Byggnummer {#build-number-440}

Adobe Pass-autentisering: JavaScript 4.4.0

Releasedatum: **06/22/2021**

## Versionsöversikt {#release-overview-440}

### Nya funktioner

Preflight-behörighet

* Nytt förauktoriserat API - det här är det nya API-anropet som ska användas för funktionen Preflight-auktorisering, som även introducerar förbättrad felrapportering för preflight-flöde.
* Den här funktionen är tillgänglig på begäran eftersom den måste aktiveras i konfigurationen för Primetime-autentisering. Kontakta din Adobe TAM för mer information om hur du aktiverar den här funktionen.
* Kontrollerar API:t CheckPreauthorizedResources.

Plattformsidentifiering

* Lägger till rubriker för AP-SDK-ID i alla SDK-anrop för att bättre identifiera SDK-typ och -version.

Övriga

* Interna förbättringar av arkitekturen.

### Felkorrigeringar

* Åtgärda konkurrensvillkor som uppstår när setRequestor och getAuthentication anropas samtidigt.
* Korrigerade ett problem som förhindrade att berättiganden lästes in korrekt i mellanlagringsmiljöer.
* Ett problem som hindrade inloggningsflödet i bakgrunden att slutföras i Safari-webbläsare har åtgärdats, vilket innebär att användaren fortfarande verkade autentiserad tills en uppdatering av sidan gjordes. En timeout uppstod, som för närvarande är inställd på 30 sekunder, så om det inte kommer något svar från Primetime Authentication-servern under den här perioden, kommer SDK att anropa setAuthenticationStatus-återanropet.

## Versionspaket {#release-package-440}

Produktions-URL:en: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Mellanlagrings-URL:en är: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
