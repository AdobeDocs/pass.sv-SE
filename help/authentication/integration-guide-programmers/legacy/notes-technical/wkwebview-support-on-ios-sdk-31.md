---
title: Stöd för WKWebView i iOS SDK 3.1+
description: Stöd för WKWebView i iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# (Äldre) Stöd för WKWebView i iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

</br>

**På grund av att Apple har ersatt UIWebView i iOS har vi uppdaterat iOS SDK 3.1 med stöd för WKWebView.**

## Kompatibilitet {#compatibility}

Från och med iOS SDK version 3.1 kan användare nu använda WKWebView eller UIWebView som båda två. Eftersom UIWebView är borttaget av Apple bör appar migreras till WKWebView för att undvika problem med framtida iOS-versioner.

Observera att migrering innebär att du helt enkelt byter UIWebView-klass till WKWebView. Det finns inget specifikt arbete att göra med Adobe AccessEnabler.

## Kända fel {#known-issues}

Adobe AccessEnabler använde en dold intern UIWebView-instans för att utföra [passiv autentisering](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md) för vissa MVPD-program. Det&quot;passiva&quot; flödet var användbart för MVPD-program som kräver autentisering för varje begärande-ID, och från det här flödet användes programmerare som använde samma team-ID i flera iOS-program för att simulera en SSO-upplevelse (Adobe SSO). Den här funktionen används för närvarande av ett begränsat antal MVPD-program.

Funktionen använde ett beteende hos UIWebView som gjorde att Adobe kunde hämta autentiseringscookies och spela upp dem igen under det passiva flödet. WKWebView har nu en starkare säkerhet som förhindrar Adobe att hämta cookies som angetts vid inloggning och spela upp dem igen med en dold instans av WKWebView. På grund av den här säkerhetsförbättringen och med tanke på att det passiva flödet endast omfattade en mycket begränsad uppsättning programmeringsdokument i ett mycket specifikt implementeringsscenario (flera program som använder samma team-ID) tog Adobe bort funktionen&quot;passiv autentisering&quot; för programmeringsvideoprogrammeringsprogram som använder webbvyer för autentisering.

Funktionen finns fortfarande för MVPD-program som är konfigurerade att använda SFSafariViewController, men observera att i det här fallet är den &quot;passiva&quot; autentiseringen synlig för användaren eftersom SFSafariViewController inte kan användas på ett &quot;dolt&quot; sätt.
