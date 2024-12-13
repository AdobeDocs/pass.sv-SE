---
title: Skicka klientinformation (enhet, anslutning och program)
description: Skicka klientinformation (enhet, anslutning och program)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1666'
ht-degree: 0%

---

# (Äldre) Skicka klientinformation (enhet, anslutning och program) {#pass-client-info}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

## Omfång {#pass-client-info-scope}

Det här dokumentet samlar information och cookbooks för att skicka klientinformation (enhet, anslutning och program) från ett programmeringsprogram till Adobe Pass Authentication REST API:er eller SDK:er.

Fördelarna med att tillhandahålla kundinformation är:

* Möjligheten att korrekt aktivera HBA (Home Base Authentication) för vissa enhetstyper och MVPD-program som stöder HBA.
* Möjligheten att korrekt tillämpa TTL:er för vissa enhetstyper (till exempel konfigurera längre TTL:er för autentiseringssessioner på tv-anslutna enheter).
* Möjlighet att på rätt sätt sammanställa affärsdata i uppdelade rapporter över olika enhetstyper med hjälp av ESM (Entitlement Service Monitoring).
* Hindrar möjligheten att tillämpa olika affärsregler korrekt (t.ex. nedbrytning) på specifika enhetstyper.

## Ökning {#pass-client-info-overview}

Klientinformationen består av:

* **Enhet** information om maskinvaru- och programvaruattributen för enheten som användaren försöker använda programmerarinnehållet från.
* **Anslutning** information om anslutningsattributen för den enhet från vilken användaren ansluter till Adobe Pass autentiseringstjänster och/eller programmeringstjänster (t.ex. server-till-server-implementeringar).
* **Program** information om det registrerade programmet som användaren försöker använda programmerarinnehållet från.

Klientinformationen är ett JSON-objekt som skapats med nycklar som presenteras i följande tabell.

>[!NOTE]
>
>Följande **nycklar** är **obligatoriska** som ska skickas i JSON-objektet för klientinformation: **model**, **osName**.
>
>Följande tangenter har **begränsade** värden: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Nyckel | Begränsad | Beskrivning | Möjliga värden |
|---|---|---|---|---|
|            | primärHardwareType | # Ja | Enhetens primära maskinvarutyp. | # Värdena är begränsade:                                                                     Kamera                                                      DataCollectionTerminal                                                      Skrivbord                                                      EmbeddedNetworkModule                                                      eReader                                                      GamesConsole                                                      GeolocationTracker                                                      Glasögon                                                      MediaPlayer                                                      MobilePhone                                                      PaymentTerminal                                                      PluginModem                                                      SetTopBox                                                      TV                                                      Tablet                                                      WirelessHotspot                                                      Armur                                                      Okänd |
| #mandatory | modell | Nej | Enhetens modellnamn. | t.ex. iPhone, SM-G930V, AppleTV osv. |
|            | version | Nej | Enhetens version. | t.ex. 2.0.1 osv. |
|            | tillverkare | Nej | Enhetens tillverkningsföretag/organisation. | t.ex. Samsung, LG, ZTE, Huawei, Motorola, Apple osv. |
|            | leverantör | Nej | Enhetens säljande företag/organisation. | t.ex. Apple, Samsung, LG, Google osv. |
| #mandatory | osName | # Ja | Enhetens operativsystemnamn. | # Värdena är begränsade:                                                   Android                   CHROME OS                   Linux                   MAC OS                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | Ja | Enhetens operativsystemsgruppnamn. | # Värdena är begränsade:                                                   Android                   BSD                   Linux                   PlayStation OS                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | Nej | Enhetens operativsystemsleverantör. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Tizen Project |
|            | osVersion | Nej | Enhetens operativsystemversion. | t.ex. 10.2, 9.0.1 osv. |
|            | browserName | # Ja | Webbläsarens namn. | # Värdena är begränsade:                                                   Android Browser                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   SeaMonkey                   Symbian Browser |
|            | browserVendor | # Ja | Webbläsarens byggföretag/organisation. | # Värdena är begränsade:                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | Nej | Enhetens webbläsarversion. | Exempel: 60.0.3112 |
|            | userAgent | Nej | Enhetens användaragent. | Exempel: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, t.ex. Gecko) Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | Nej | Enhetens fysiska skärmbredd. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | Nej | Enhetens fysiska skärmhöjd. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | Nej | Enhetens pixeldensitet för fysisk skärm. | Exempel: 294 |
|            | diagonalScreenSize | Nej | Enhetens diagonala mått för fysisk skärm i tum. | t.ex. 5.5, 10.1 |
|            | connectionIp | Nej | Enhetens IP som används för att skicka HTTP-begäranden. | t.ex. 8.8.4.4 |
|            | connectionPort | Nej | Enhetens port som används för att skicka HTTP-begäranden. | till exempel 53124 |
|            | connectionType | Nej | Nätverksanslutningstypen. | t.ex. WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Ja | Säkerhetsstatus för nätverksanslutning. | # Värdena är begränsade:                                                   true - om det är ett säkert nätverk                   false - om det är en offentlig aktiv punkt |
|            | applicationId | Nej | Programmets unika identifierare. | t.ex. CNN |

## API-referenser {#api-ref}

I det här avsnittet presenteras det API som hanterar klientinformation när du använder Adobe Pass Authentication REST API:er eller SDK:er.

### REST API {#rest-api}

Adobe Pass autentiseringstjänster har stöd för att ta emot klientinformationen på följande sätt:

* Som ett **huvud: &quot;X-Device-Info&quot;**
* Som en **frågeparameter: &quot;device_info&quot;**
* Som en **post-parameter: &quot;device_info&quot;**

>[!IMPORTANT]
>
>I alla tre scenarierna måste nyttolasten för huvudet eller parametern vara **Base64-kodad och URL-kodad**.

**SDK**

#### JavaScript SDK {#js-sdk}

AccessEnabler JavaScript SDK skapar som standard ett JSON-objekt för klientinformation, som skickas till Adobe Pass autentiseringstjänster, om det inte åsidosätts.

AccessEnabler JavaScript SDK har stöd för **att endast** åsidosätta nyckeln &quot;applicationId&quot; från JSON-objektet för klientinformation via alternativparametern [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options)) för *applicationId* för .

>[!CAUTION]
>
>Parametervärdet `applicationId` måste vara ett vanligt textsträngsvärde.
>Om Programmer-programmet godkänner applicationId beräknas resten av klientinformationsnycklarna fortfarande av AccessEnabler JavaScript SDK.

#### iOS/tvOS SDK {#ios-tvos-sdk}

AccessEnabler iOS/tvOS SDK skapar som standard ett JSON-objekt för klientinformation, som skickas till Adobe Pass autentiseringstjänster, om det inte åsidosätts.

AccessEnabler iOS/tvOS SDK har stöd för **åsidosättning av JSON-objektet för hela**-klientinformationen via parametern device_info för [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions).

>[!CAUTION]
>
>Parametervärdet *device_info* måste vara ett **Base64-kodat** *NSString* -värde.
>
>Om programmerarprogrammet beslutar att skicka *device_info* åsidosätts alla klientinformationsnycklar som beräknas av AccessEnabler iOS/tvOS SDK. Därför är det mycket viktigt att beräkna och skicka värdena för så många tangenter som möjligt. Mer information om implementeringen finns i tabellen [Översikt](#pass-client-info-overview) och i [iOS/tvOS-cookbook](#ios-tvos).

#### Android/FireOS SDK {#and-fire-os-sdk}

`AccessEnabler` Android/FireOS SDK skapar som standard ett JSON-objekt för klientinformation, som skickas till Adobe Pass autentiseringstjänster, om det inte åsidosätts.

`AccessEnabler` Android/FireOS SDK har stöd för **åsidosättning av JSON-objektet för hela**-klientinformationen via [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)s/[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption)s `device_info` -parameter.

>[!NOTE]
>
>Parametervärdet `device_info` måste vara ett **Base64-kodat**-strängvärde.

>[!IMPORTANT]
>
>Om programmerarprogrammet beslutar att skicka `device_info` åsidosätts alla klientinformationsnycklar som beräknas av `AccessEnabler` Android/FireOS SDK. Därför är det mycket viktigt att beräkna och skicka värdena för så många tangenter som möjligt. Mer information om implementeringen finns i tabellen [Översikt](#pass-client-info-overview) och i cookbook-programmen [Android](#android) och [FireOS](#fire-tv) .

## Cookbooks {#cookbooks}

I det här avsnittet presenteras en cookbook för att skapa JSON-objekt för klientinformation för olika enhetstyper.

>[!IMPORTANT]
>
>Nycklarna som är markerade med **!** måste skickas.

### Android {#android}

Enhetsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---------------|-----------------------------|---------------|
| ! | modell | Build.MODEL | GT-I9505 |
|   | leverantör | Build.BRAND | samsung |
|   | tillverkare | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | hårdkodad | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

Anslutningsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

Programinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---------------|-----------|--------------|
|   | applicationId | hårdkodad | CNN |

>[!IMPORTANT]
>
Information om enheten, anslutningen och programmet måste läggas till i samma JSON-objekt. Efteråt måste det resulterande objektet vara **Base64-kodat**. För Adobe Pass Authentication REST API:er måste värdet dessutom vara **URL-kodat**.

**Exempelkod**

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
**Resurser:**
* den offentliga klassen [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} i Java-utvecklarens dokumentation.

### FireTV {#fire-tv}

Enhetsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (till exempel) |
|---|---------------|-----------------------------|--------------|
| ! | modell | Build.MODEL | AFTM |
|   | leverantör | Build.BRAND | Amazon |
|   | tillverkare | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | hårdkodad | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

Anslutningsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

Programinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---------------|-----------|--------------|
|   | applicationId | hårdkodad | CNN |

>[!IMPORTANT]
>
Information om enheten, anslutningen och programmet måste läggas till i samma JSON-objekt. Efteråt måste det resulterande objektet vara **Base64-kodat**. För Adobe Pass Authentication REST API:er måste värdet dessutom vara **URL-kodat**.

>[!NOTE]
>
**Resurser:**
* den offentliga klassen [Build](https://developer.android.com/reference/android/os/Build.html){target=_blank} i dokumentationen för Android-utvecklare.
* [Identifiera FireTV-enheter](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

Enhetsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---------------|------------------------|--------------|
| ! | modell | uname.machine | iPhone |
|   | leverantör | hårdkodad | Apple |
|   | tillverkare | hårdkodad | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

Anslutningsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Nyttjbarhet currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


Programinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---------------|-----------|--------------|
|   | applicationId | hårdkodad | CNN |

>[!IMPORTANT]
>
Information om enheten, anslutningen och programmet måste läggas till i samma JSON-objekt. Efteråt måste det resulterande objektet vara Base64-kodat. För Adobe Pass Authentication REST API:er måste värdet också vara URL-kodat.

**Exempelkod**

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
**Resurser:**
* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
* [Om nåbarhet](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

Enhetsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | modell | hårdkodad | &quot;Roku&quot; |
|     | leverantör | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | tillverkare | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | hårdkodad | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

Anslutningsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | hårdkodad | true om anslutningen är kopplad |

Programinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---------------|-----------|--------------|
|   | applicationId | hårdkodad | CNN |

>[!IMPORTANT]
>
Information om enheten, anslutningen och programmet måste läggas till i samma JSON-objekt. Efteråt måste det resulterande objektet vara **Base64-kodat**. För Adobe Pass Authentication REST API:er måste värdet också vara URL-kodat.

>[!NOTE]
>
Mer information finns i [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

Enhetsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Värde (exempel) |
|---|---|---|---|
| ! | modell | EasClientDeviceInformation.SystemProductName |                 |
|   | leverantör | hårdkodad | Microsoft |
|   | tillverkare | hårdkodad | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

Anslutningsinformationen kan utformas på följande sätt:

|   | Nyckel | Source | Exempel |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;Ingen&quot;, &quot;Wpa&quot; etc. |

Programinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---|---|---|
| applicationId | hårdkodad | CNN |

>[!IMPORTANT]
>
Information om enheten, anslutningen och programmet måste läggas till i samma JSON-objekt. Efteråt måste det resulterande objektet vara **Base64-kodat**. För Adobe Pass Authentication REST API:er måste värdet dessutom vara **URL-kodat**.

**Resurser**

* [Klassen EasClientDeviceInformation](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [Klassen DisplayInformation](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
