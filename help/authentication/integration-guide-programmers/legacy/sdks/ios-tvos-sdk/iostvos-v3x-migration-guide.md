---
title: Migreringshandbok för iOS/tvOS v3.x
description: Migreringshandbok för iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# (Äldre) Migreringshandbok för iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

>[!TIP]
> 
> **Anteckningar:**
>
> - Från och med iOS SDK version 3.1 kan implementerare nu använda WKWebView eller UIWebView som båda är tillgängliga. Eftersom UIWebView är föråldrat bör program migreras till WKWebView för att undvika problem med framtida iOS-versioner.
> - Observera att migrering innebär att du helt enkelt byter UIWebView-klass till WKWebView. Det finns inget specifikt arbete att göra med Adobe AccessEnabler.

</br>

## Uppdatera bygginställningar {#update}

Den här versionen innehåller funktioner skrivna på SWIFT-språk. Om din app är helt objektiv-C måste du ange kryssrutan Inkludera alltid standardbibliotek för Swift i målets bygginställningar till Ja. När det här alternativet har angetts skannar Xcode in de paketerade ramverken i din app och, om någon av dem innehåller Swift-kod, kopierar den relevanta biblioteken till programpaketet. Om du inte uppdaterar build-inställningarna kan programmet krascha med fel som anger att det inte kan läsa in AccessEnabler.framework eller olika `ibswift*`-bibliotek.

</br>

## Lägga till programsatsen {#add}

> Mer information om hur du får programvarusatsen finns här
> sida:
> [Programregistrering](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

När du har programsatsen rekommenderar vi att du använder den på en fjärrserver så att du enkelt kan återkalla den eller ändra den utan att behöva installera en ny version av programmet i App Store. När programmet startar hämtar du programsatsen från fjärrplatsen och skickar den till konstruktorn AccessEnabler:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API-information här: [API-referens för iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Lägg till anpassat URL-schema {#add-custom}

> Mer information om hur du hämtar ett anpassat URL-schema finns på den här sidan: [Hämta ett kund-URL-schema](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

När du har fått det anpassade URL-schemat måste du lägga till det i programmets info.plist-fil. Det anpassade schemat har följande format: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. Du måste utesluta kolon och snedstreck när du lägger till det i filen. Ovanstående exempel blir `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## Avlyssnande anrop till det anpassade URL-schemat {#intercept}

Detta gäller endast om ditt program tidigare aktiverat manuell hantering av Safari View Controller (SVC) via anropet [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) och för specifika MVPD-anrop som kräver Safari View Controller (SVC), vilket innebär att URL:er för autentisering och utloggning av slutpunkter måste läsas in av en SFSafariViewController-styrenhet i stället för en i stället för en UIW-styrenhet WebView/WKWebView-kontrollant.

Under autentiserings- och utloggningsflöden måste ditt program övervaka aktiviteten för `SFSafariViewController `kontrollanten när den går igenom flera omdirigeringar. Programmet måste identifiera tidpunkten då det läser in en specifik anpassad URL som definieras av din `application's custom URL scheme` (t.ex.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`). När kontrollenheten läser in den här anpassade URL:en måste ditt program stänga `SFSafariViewController` och anropa AccessEnablers `handleExternalURL:url `API-metod.

Lägg till följande metod i `AppDelegate`:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API-information här: [Hantera extern URL](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Uppdatera signaturen för metoden setRequestor {#update-setreq}

Eftersom den nya SDK använder en ny autentiseringsmekanism behövs inte parametern signedRequestId eller den offentliga nyckeln och hemligheten (för tvOS). Metoden `setRequestor` är förenklad och behöver bara begärandeID.

### iOS

Den här koden:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

blir:

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

Den här koden:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

blir:

```swift
    accessEnabler.setRequestor(requestorId)
```

> API-information här: [Ange begärande](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Ersätt getAuthenticationToken-metod med handleExternalURL-metod {#replace}

Metoden `getAuthentication` användes tidigare för att slutföra autentiseringsflödet. Eftersom namnet var vilseledande har namnet ändrats till `handleExternalURL` och URL:en används som parameter.

Ändra alla förekomster av detta:

```swift
    accessEnabler.getAuthenticationToken()
```

till detta:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API-information här: [Hantera extern URL](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
