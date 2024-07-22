---
title: Dynamisk klientregistrering
description: Dynamisk klientregistrering
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Dynamisk klientregistrering {#dynamic-client-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Kontext {#context}

Förbättrade användarupplevelser och plattformsägare som passar ihop med modern säkerhetspraxis
Adobe Pass Authentication Android SDK och iOS SDK går nu i riktning mot att använda [anpassade Android Chrome-flikar](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} och [Apple Safari-vykontroller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

Den aktuella AdobePass-implementeringen använder plattformsspecifika webbvyer för att tillhandahålla webbmiljön för att visa MVPD-inloggningssidan. De här webbvyerna delar inte autentiseringsuppgiftshantering med plattformswebbläsare och användaren kan därför inte använda ett lösenord som sparats i webbläsaren när ett Adobe Pass-autentiseringsprogram används. Av säkerhetsskäl håller vissa plattformar dessutom på att bli inaktuella i WebView-styrenheterna för autentiseringsuppgifter. Både Google och Apple har alternativa alternativ som&quot;Chrome anpassade flikar&quot; och&quot;Safari View Controller&quot;. De här flikarna är i stort sett enkla att använda i sina respektive webbläsare. Adobe Pass Authentication antar dessa nya komponenter under 2018.

## Information {#details}

För närvarande finns det två sätt på vilka Adobe Pass Authentication identifierar och registrerar program:

* webbläsarbaserade klienter registreras via tillåten domänlista
* Inbyggda programklienter, som iOS och Android, registreras via den signerade beställarmekanismen

För att hantera de nya flödena för Chrome anpassade flikar och Safari-vystyrenhet föreslår Adobe Pass en ny klientregistreringsmekanism för registrering av nya program. Den här mekanismen ger en säkrare och mer detaljerad kontroll över dina program och kan användas för att registrera program på alla plattformar.

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
