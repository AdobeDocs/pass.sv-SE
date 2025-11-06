---
title: Felsöka AccessEnabler iOS/tvOS SDK med hjälp av apploggarna i konsolen
description: Felsöka AccessEnabler iOS/tvOS SDK med hjälp av apploggarna i konsolen
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# (Äldre) Felsöka AccessEnabler iOS/tvOS SDK med hjälp av apploggar för konsolen {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

## Ökning

Omfånget för det här dokumentet är att fånga och presentera utvecklingen av loggningsmekanismen för SDK i AccessEnabler i iOS/tvOS tillsammans med användbar information för felsökning av AccessEnabler-ramverket med hjälp av apploggarna i Console.

## Loggningsmekanismens tillstånd

Syftet med loggningsmekanismen i AccessEnabler iOS/tvOS är att generera användbara meddelanden för felsökning av eventuella problem som ett program som använder AccessEnabler-ramverket kan stöta på på grund av detta.

### AccessEnabler iOS/tvOS 3.5.0 och senare

Från och med AccessEnabler-versionen av iOS/tvOS 3.5.0 introducerar loggningsfunktionen följande förbättringar som ändringar:

* I AccessEnabler-ramverket används den Apple rekommenderade [OSLog](https://developer.apple.com/documentation/os/oslog)-implementeringen.

* I AccessEnabler-ramverket introduceras möjligheten att filtrera konsolapploggar baserat på undersystemet: **com.adobe.pass.AccessEnabler**. Alla meddelanden som skickas av SDK ingår i com.adobe.pass.AccessEnabler.

* I AccessEnabler-ramverket introduceras möjligheten att filtrera konsolens apploggar baserat på Any (prefix): **[AccessEnabler]**. Alla meddelanden som skickas av SDK har prefixet [AccessEnabler].

* I AccessEnabler-ramverket introduceras möjligheten att filtrera konsolapploggar baserat på kategori: **debug**, **error** i kombination med något av de två ovanstående villkoren: Subsystem eller Any (prefix).

## Felsöka med hjälp av konsolapploggar

Beroende på vilka problem som utreds kanske du vill inkludera eller exkludera de loggningsmeddelanden som genereras av AccessEnabler-ramverket. Du kan därför nedan hitta användbar information som kan hjälpa dig vid utredningar och när du använder apploggar för Console.


### AccessEnabler iOS/tvOS 3.5.0 och senare

#### Inklusive {#including}

För att kunna se något av de loggningsmeddelanden som skickas av AccessEnabler-ramverket **måste** markera Inkludera informationsmeddelanden och Inkludera felsökningsmeddelanden i åtgärdsavsnittet i Console-appen, enligt bilden nedan.

![](../../../assets/include-info-debug-msg.png)


För att kunna felsöka funktionerna i AccessEnabler iOS/tvOS SDK och **se** loggarna i AccessEnabler-ramverket kan du:

* Sök i konsolappen med alternativet **Subsystem** som är lika med värdet com.adobe.pass.AccessEnabler som i bilden nedan.

![](../../../assets/subsys-console-app.png)

* Sök i konsolappen med alternativet **Valfri** som innehåller
  [AccessEnabler]-värde som i bilden nedan.

![](../../../assets/any-optn-console-app.png)

Tillsammans med ovanstående två villkor kan du även använda alternativet **Kategori** i kombination med **Delsystem** eller **Valfritt (prefix)** för att explicit söka efter **debug** - eller **error** -nivåmeddelanden som skickas av AccessEnabler iOS/tvOS SDK.

#### Exklusive

För att kunna felsöka funktionerna i andra komponenter bättre och **exkludera** loggarna i AccessEnabler-ramverket kan du:

* Sök i konsolappen med alternativet **Subsystem** som inte är lika med värdet com.adobe.pass.AccessEnabler.
* Sök i konsolappen med alternativet **Any** som inte innehåller värdet [AccessEnabler].

## Rapportera ett problem

När du rapporterar ett problem med Adobe Pass-autentisering bör du överväga följande förslag:

* Var god försök att tillhandahålla reproduktionsstegen.
* Försök att ange den eller de operativsystemsversioner och enhetsmodeller som problemet gäller.
* Ange vilken version av AccessEnabler iOS/tvOS SDK som har problem.
* försök att hämta och bifoga alla loggningsmeddelanden för AccessEnabler iOS/tvOS SDK med något av de två alternativen i avsnittet [Inkludera](#including).
