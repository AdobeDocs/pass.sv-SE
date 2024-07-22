---
title: Utvärdering av förebyggande av spårning Google Chrome
description: Utvärdering av förebyggande av spårning Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# Utvärdering av förebyggande av spårning - Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning

I det här dokumentet sammanställs användbara resurser och en utvärdering görs av de kommande förändringar som planeras av Google Chrome som en del av deras initiativ att fasa ut cookies från tredje part.

Utvärderingen görs för TV Everywhere-program (TVE) som körs i webbläsaren Google Chrome och som använder Adobe Pass Access Enabler JavaScript SDK v4 för att integreras med Adobe Pass Authentication-backend-tjänsterna.

## Offentliga resurser

Nedan finns en lista över resurser som samlats in från Google utvecklingswebbplats och även från deras officiella blogg som vi rekommenderar våra kunder att läsa:

* [Nästa steg mot att fasa ut cookies från tredje part i Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Utvecklardokumentation för sekretesssandlådan](https://developers.google.com/privacy-sandbox)
* [Förbered dig för begränsningar av cookies från tredje part](https://developers.google.com/privacy-sandbox/3pcd)
* [Förbered dig för tredjeparts-cookie-avfasning](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Förbereder för slut på cookies från tredje part](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Tredjepartscookies är begränsade som standard för 1 % av Chrome-användarna](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Tidslinje

Som en kort sammanfattning började Google Chrome testa [spårningsskydd](https://privacysandbox.com/), en ny funktion som begränsar spårning mellan webbplatser som påverkar alla cookies från tredje part.

Inledningsvis började detta i början av 2024 och påverkade ungefär 1 % av användarna, och deras (preliminära) plan var att utöka detta för upp till 100 % av användarna från och med tredje kvartalet 2024.

## Utvärdering

Google har publicerat ett dokument som sammanställer den rekommenderade spelboken för att förbereda för att fasa ut cookies från tredje part på följande länk: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Vi följde den här spelboken för bedömning av TV Everywhere-program (TVE) som körs i webbläsaren Google Chrome och som använder Adobe Pass Access Enabler JavaScript SDK v4 för integrering med Adobe Pass Authentication-backend-tjänsterna.

### Slutsatser

Baserat på våra tester, som simulerar kommande uppdateringar av Google Chrome, fortsätter de primära TVE-affärsflödena **att fungera som förväntat**.

Det är dock viktigt att erkänna Google bredare strategi, som inte bara innebär att cookies från tredje part upphör, utan även att lagring från tredje part partitioneras.

Därför kommer Chrome-användare att drabbas av störningar med SSO (Single Sign-On), SLO (Single Logout) och passiva autentiseringsfunktioner, vilket kräver separata inloggnings-/utloggningsåtgärder för varje TVE-program de använder (i linje med den nuvarande Safari-upplevelsen).

## Ansökan om självbedömning

Vi uppmanar våra kunder att aktivt genomföra liknande utvärderingar för att i god tid kunna identifiera potentiella problem och bekanta sig med den reviderade användarupplevelsen för Google Chrome.

Utvärderingen bör omfatta både förstahandstjänster och tredjepartstjänster, särskilt när det gäller integreringen av JavaScript SDK v4 i Adobe Pass Access Enabler.

Om du får problem med TVE-affärsflöden, inklusive autentisering, förauktorisering, behörighet, användarens metadata eller utloggning, kan du skicka in en rapport via en Zendesk-biljett till vårt kundtjänstteam.

Hjälp med att utveckla din självutvärderingsplan finns i avsnitten nedan.

### Granska användningen av cookies

Från och med Chrome 118 markeras cookies som kan påverkas på fliken [DevTools Issues](https://developer.chrome.com/docs/devtools/issues/) med följande meddelande: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Cookies som har markerats för tredjepartsanvändning kan identifieras med deras `SameSite=None`-attributvärde.

Följ den här länken om du vill läsa mer: https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Test för brott

Om du vill testa om ett fel har uppstått startar du Chrome med kommandoradsflaggan `--test-third-party-cookie-phaseout` eller Chrome 118 enable `#test-third-party-cookie-phaseout` in `chrome://flags/`.

Detta konfigurerar Google Chrome att blockera cookies från tredje part och se till att framtida funktioner är aktiva för att bäst simulera läget efter utfasningen.

Det är värt en djupdykning i de tekniska specifikationerna för följande Chrome-flaggor:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Följ den här länken om du vill läsa mer: https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Andra webbläsare

### Firefox

Firefox hade en utrullning av sin mekanism som hette: `Enhanced Tracking Protection` för några år sedan.

Nedan finns några användbara resurser från Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari hade en utrullning av sin mekanism som hette: `Intelligent Tracking Prevention` för några år sedan.

Nedan finns användbara resurser från Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Nedan finns några användbara resurser från Adobe Pass:

* [Utvärdering av förebyggande av spårning - Apple Safari](tracking-prevention-assessment-apple-safari.md)
