---
title: Versionsinformation om Adobe Pass-autentisering JavaScript 4.0.0
description: Versionsinformation om Adobe Pass-autentisering JavaScript 4.0.0
source-git-commit: 7057aeda34b4fe0d059912ab0a71ea856427654c
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass-autentisering JavaScript 4.0.0 {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Byggnummer {#build-no-javascript-sdk-400}

Adobe Pass-autentisering: JavaScript 4.0.0

Utgivningsdatum: **07/05/2018**


## Versionsöversikt {#overview-javascript-sdk-400}

* Implementerat stöd för dynamisk klientregistreringsmekanism, en enhetlig säkerhetsmekanism på olika plattformar. Det går att hantera säkerhetsåtkomsten på olika plattformar via TVE-kontrollpanelen, via själva programmeraren.
* Eliminerar användningen av alla cookies från tredje part och nästan alla cookies från första part för alla funktioner (förutom individualisering där en cookie från första part fortfarande används - kommer att tas bort i framtiden), vilket gör den mer kompatibel och framtidssäker med nya webbläsarprinciper kring cookies från tredje part, eller cookies i allmänhet (t.ex. Safaris ITP). Observera att den tidigare 3.x-versionen fungerar som väntat för det lyckliga flödet endast med cookies från första part, men detta kan ändras i och med att nya webbläsarversioner släpps.
* Stöd för att inaktivera cachelagring för preflight-auktoriseringskontroller.
* Den här versionen är INTE bakåtkompatibel och finns under en ny sökväg: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . För att dra nytta av denna nya JS SDK krävs en migreringsprocess från programmerare för att registrera sina webbprogram med den nya dynamiska klientregistreringsfunktionen och för att peka på den nya JS SDK-URL:en.


## Versionspaket {#rel-pkg-javascript-sdk-400}

Produktions-URL:en: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

Mellanlagrings-URL:en är: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
