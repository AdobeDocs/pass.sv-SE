---
title: Header - AP-Device-Identifier
description: REST API V2 - huvud - AP-enhets-ID
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Header - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Huvudet för begäran <b>AP-Device-Identifier</b> innehåller identifieraren för direktuppspelningsenheten som den skapades av klientprogrammet.

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;typ&gt; &lt;identifierare&gt;</td>
   </tr>
   <tr>
      <td>Huvudtyp</td>
      <td>Huvud för begäran</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nej</td>
   </tr>
</table>

## Direktiv {#directives}

<b>&lt;typ></b>

Enhetsidentifierartypen.

Det finns bara en typ som stöds enligt nedan.

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Typ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>fingeravtryck</td>
      <td>
            Enhetsidentifieraren består av en stabil och unik identifierare som skapas och hanteras av klientprogrammet för varje enhet.
            <br/>
            Klientprogrammet bör cachelagra enhetsidentifieraren i beständigt lagringsutrymme, eftersom en förlust eller ändring gör autentiseringen ogiltig. Klientprogrammet bör förhindra värdeförändringar som orsakas av användaråtgärder som programavinstallation, ominstallation eller uppgraderingar.
      </td>
   </tr>
</table>


<b>&lt;identifierare></b>

Värdet `Base64-encoded` för enhets-ID.

## Exempel {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
>
> Dokumentationsresurserna tillhandahålls i referenssyfte.
>
> Dokumentationsresurserna är inte fullständiga och kan kräva ytterligare ändringar för att arbeta i ditt projekt.
> 
> Oavsett vilken implementering du har måste rubriken `AP-Device-Identifier` innehålla ett värde som är formaterat enligt beskrivningen i avsnittet [ Direktiv ](#directives) .

### Webbläsare {#browsers}

Om du vill skapa `AP-Device-Identifier`-rubriken för enheter som körs i en webbläsare, måste klientprogrammet beräkna en stabil och unik identifierare baserat på tillgängliga data som webbläsare, enhet eller användarspecifika data.

_(*) Vi rekommenderar att du integrerar ett bibliotek eller en tjänst som tillhandahåller en fingeravtrycksmekanism för webbläsare eller enheter._

### Mobila enheter {#mobile-devices}

#### iOS &amp; iPadOS {#ios-ipados}

Om du vill skapa `AP-Device-Identifier`-rubriken för enheter som kör [iOS eller iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) kan du läsa följande dokument:

* Apple utvecklardokumentation för [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Vi rekommenderar att du använder en SHA-256-hash-funktion över det angivna värdet för operativsystemet._

#### Android {#android}

Om du vill skapa `AP-Device-Identifier`-rubriken för enheter som kör [Android](https://developer.android.com/about/versions) kan du läsa följande dokument:

* Android utvecklardokumentation för [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Vi rekommenderar att du använder en SHA-256-hash-funktion över det angivna värdet för operativsystemet._

### TV-anslutna enheter {#tv-connected-devices}

#### tvOS {#tvos}

Om du vill skapa `AP-Device-Identifier`-rubriken för enheter som kör [tvOS](https://developer.apple.com/documentation/tvos-release-notes) kan du läsa följande dokument:

* Apple utvecklardokumentation för [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Vi rekommenderar att du använder en SHA-256-hash-funktion över det angivna värdet för operativsystemet._

#### Fire OS {#fireos}

Om du vill skapa `AP-Device-Identifier`-rubriken för enheter som kör [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) kan du läsa följande dokument:

* Android utvecklardokumentation för [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Vi rekommenderar att du använder en SHA-256-hash-funktion över det angivna värdet för operativsystemet._

#### Roku OS {#rokuos}

Om du vill skapa `AP-Device-Identifier`-rubriken för enheter som kör [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) kan du läsa följande dokument:

* Dokumentation för Roku-utvecklare för [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) Vi rekommenderar att du använder en SHA-256-hash-funktion över det angivna värdet för operativsystemet._

### Övriga {#others}

För enhetsplattformar som inte omfattas av dokumentationen bör enhetsidentifieraren länkas till en tillgänglig maskinvaruidentifiering, som vanligtvis anges i enhetens maskinvaruhandbok.

Om det inte finns några maskinvaruidentifierare tillgängliga bör en unikt genererad identifierare som baseras på klientprogramattribut användas och cachelagras i beständig lagring.
