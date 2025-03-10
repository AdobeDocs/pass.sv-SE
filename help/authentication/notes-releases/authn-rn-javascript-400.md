---
title: Adobe Pass Authentication JavaScript 4.0.0 Release Notes
description: Adobe Pass Authentication JavaScript 4.0.0 Release Notes
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.0.0 Release Notes {#javascript-sdk-400-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Byggnummer {#build-number-400}

Adobe Pass-autentisering: JavaScript 4.0.0

Releasedatum: **07/05/2018**

## Versionsöversikt {#release-overview-400}

* Implementerat stöd för dynamisk klientregistreringsmekanism, en enhetlig säkerhetsmekanism på olika plattformar. Det går att hantera säkerhetsåtkomsten på olika plattformar via TVE-kontrollpanelen, via själva programmeraren.
* Eliminerar användningen av alla cookies från tredje part och nästan alla cookies från första part för alla funktioner (förutom individualisering där en cookie från första part fortfarande används - kommer att tas bort i framtiden), vilket gör den mer kompatibel och framtidssäker med nya webbläsarprinciper kring cookies från tredje part, eller cookies i allmänhet (t.ex. Safaris ITP). Observera att den tidigare 3.x-versionen fungerar som väntat för det lyckliga flödet endast med cookies från första part, men detta kan ändras i och med att nya webbläsarversioner släpps.
* Stöd för att inaktivera cachelagring för preflight-auktoriseringskontroller.
* Den här versionen är INTE bakåtkompatibel och finns under en ny sökväg: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . För att kunna dra nytta av denna nya JS SDK behöver programmerare en migreringsprocess för att registrera sina webbapplikationer med den nya dynamiska klientregistreringsfunktionen och hänvisa till den nya JS SDK-adressen.

## Versionspaket {#release-package-400}

Produktions-URL:en: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Mellanlagrings-URL:en är: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
