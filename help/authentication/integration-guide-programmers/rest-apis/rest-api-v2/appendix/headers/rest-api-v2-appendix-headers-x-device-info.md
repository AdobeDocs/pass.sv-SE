---
title: Header - X-Device-Info
description: REST API V2 - Header - X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 42df16e34783807e1b5eb1a12ca9db92f4e4c161
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 1%

---

# Header - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Huvudet <b>X-Device-Info</b> innehåller klientinformationen (enhet, anslutning och program) som är relaterad till den faktiska direktuppspelningsenheten och används för att fastställa plattformsspecifika regler som MVPD-program kan tillämpa.

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;enhetsinformation&gt;</td>
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

<b>&lt;device_information></b>

Värdet `Base64-encoded` för JSON-elementet som innehåller minst de attribut som markerats enligt följande tabell.

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Närvaro</th>
        <th style="background-color: #EFF2F7; width: 15%;">Nyckel</th>
        <th style="background-color: #EFF2F7;">Beskrivning</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Begränsad</th>
        <th style="background-color: #EFF2F7;">Möjliga värden</th>
    </tr>
    <tr>
        <td></td>
        <td>primärHardwareType</td>
        <td>Enhetens primära maskinvarutyp.</td>
        <td>&check;</td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>Kamera</li>
                <li>DataCollectionTerminal</li>
                <li>Skrivbord</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Glasögon</li>
                <li>MediaPlayer</li>
                <li>MobilePhone</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tablet</li>
                <li>WirelessHotspot</li>
                <li>Armur</li>
                <li>Okänd</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obligatoriskt</i></td>
        <td>modell</td>
        <td>Enhetens modellnamn.</td>
        <td></td>
        <td>t.ex. iPhone, SM-G930V, AppleTV osv.</td>
    </tr>
    <tr>
        <td><i>obligatoriskt</i></td>
        <td>version</td>
        <td>Enhetens version.</td>
        <td></td>
        <td>t.ex. 2.0.1 osv.</td>
    </tr>
    <tr>
        <td></td>
        <td>tillverkare</td>
        <td>Tillverkningsföretaget/tillverkningsorganisationen för enheten.</td>
        <td></td>
        <td>t.ex. Samsung, LG, ZTE, Huawei, Motorola, Apple osv.</td>
    </tr>
    <tr>
        <td></td>
        <td>leverantör</td>
        <td>Enhetens säljande företag/organisation.</td>
        <td></td>
        <td>t.ex. Apple, Samsung, LG, Google osv.</td>
    </tr>
    <tr>
        <td><i>obligatoriskt</i></td>
        <td>osName</td>
        <td>Enhetens operativsystemnamn.</td>
        <td>&check;</td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>Android</li>
                <li>CHROME OS</li>
                <li>Linux</li>
                <li>MAC OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>Enhetens operativsystemsgruppnamn.</td>
        <td>&check;</td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>Enhetens operativsystemsleverantör.</td>
        <td>&check;</td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Tizen Project</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obligatoriskt</i></td>
        <td>osVersion</td>
        <td>Enhetens operativsystemversion.</td>
        <td></td>
        <td>t.ex. 10.2, 9.0.1 osv.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>Webbläsarens namn.</td>
        <td>&check;</td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>Android Browser</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Symbian Browser</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>Webbläsarens byggföretag/organisation.</td>
        <td>&check;</td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>Enhetens webbläsarversion.</td>
        <td></td>
        <td>Exempel: 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>Enhetens användaragent.</td>
        <td></td>
        <td>Exempel: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, t.ex. Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>Enhetens fysiska skärmbredd.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>Enhetens fysiska skärmhöjd.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>Enhetens pixeldensitet på den fysiska skärmen.</td>
        <td></td>
        <td>Exempel: 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>Enhetens diagonala mått för fysisk skärm i tum.</td>
        <td></td>
        <td>t.ex. 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>Enhetens IP-adress som används för att skicka HTTP-begäranden.</td>
        <td></td>
        <td>t.ex. 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>Enhetens port som används för att skicka HTTP-begäranden.</td>
        <td></td>
        <td>till exempel 53124</td>
    </tr>
    <tr>
        <td><i>obligatoriskt</i></td>
        <td>connectionType</td>
        <td>Nätverksanslutningstypen.</td>
        <td></td>
        <td>t.ex. WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>Säkerhetsstatus för nätverksanslutning.</td>
        <td>&check;</td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>true - om det är ett säkert nätverk</li>
                <li>false - om det är en offentlig aktiv punkt</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>Programmets unika identifierare.</td>
        <td></td>
        <td>t.ex. REF30</td>
    </tr>
</table>


## Exempel {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
> 
> Kodfragmenten och dokumentationsresurserna tillhandahålls i referenssyfte.
> 
> Kodfragmenten är inte fullständiga och kan kräva ytterligare ändringar för att fungera i ditt projekt.
>
> Oavsett vilken implementering du har måste rubriken `X-Device-Info` innehålla ett värde som är formaterat enligt beskrivningen i avsnittet [&#x200B; Direktiv &#x200B;](#directives) .

### Webbläsare {#browsers}

För klientprogram som körs i en webbläsare kan rubriken `X-Device-Info` utelämnas eftersom webbläsaren automatiskt skickar en minimiuppsättning obligatorisk information i rubriken `User-Agent`.

Du kan fortfarande använda rubriken `X-Device-Info` för att ge ytterligare information om enheten, anslutningen och programmet om klientprogrammet integrerar ett bibliotek eller en tjänst som tillhandahåller en enhetsidentifieringsmekanism.

### Mobila enheter {#mobile-devices}

#### iOS &amp; iPadOS {#ios-ipados}

Om du vill skapa `X-Device-Info`-rubriken för enheter som kör [iOS eller iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) kan du läsa följande dokument och under kodfragmentet:

* Apple utvecklardokumentation för [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple utvecklardokumentation för [nåbarhet](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Handbok för Linux för [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Enhetsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|------------------------|-----------------|
| modell | uname.machine | iPhone |
| leverantör | hårdkodad | Apple |
| tillverkare | hårdkodad | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Anslutningsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Nyttjbarhet currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


Programinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|-----------|-----------------|
| applicationId | hårdkodad | REF30 |

#### Android {#android}

Om du vill skapa `X-Device-Info`-rubriken för enheter som kör [Android](https://developer.android.com/about/versions) kan du läsa följande dokument och under kodfragment:

* Android utvecklardokumentation för klassen [Build](https://developer.android.com/reference/android/os/Build.html).

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
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

Enhetsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|-----------------------------|-----------------|
| modell | Build.MODEL | GT-I9505 |
| leverantör | Build.BRAND | samsung |
| tillverkare | Build.MANUFACTURER | samsung |
| version | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | hårdkodad | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

Anslutningsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

Programinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|-----------|-----------------|
| applicationId | hårdkodad | REF30 |

### TV-anslutna enheter {#tv-connected-devices}

#### tvOS {#tvos}

Om du vill skapa `X-Device-Info`-rubriken för enheter som kör [tvOS](https://developer.apple.com/documentation/tvos-release-notes) kan du läsa följande dokument och kodfragment nedan:

* Apple utvecklardokumentation för [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Apple utvecklardokumentation för [nåbarhet](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Handbok för Linux för [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

Enhetsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|------------------------|-----------------|
| modell | uname.machine | AppleTV |
| leverantör | hårdkodad | Apple |
| tillverkare | hårdkodad | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

Anslutningsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Nyttjbarhet currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

Programinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|-----------|-----------------|
| applicationId | hårdkodad | REF30 |

#### Fire OS {#fireos}

Om du vill skapa `X-Device-Info`-rubriken för enheter som kör [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) kan du läsa följande dokument:

* Android utvecklardokumentation för klassen [Build](https://developer.android.com/reference/android/os/Build.html).
* Amazon utvecklardokumentation för [Identifiering av Fire TV-enheter](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html).

Enhetsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|-----------------------------|-----------------|
| modell | Build.MODEL | AFTM |
| leverantör | Build.BRAND | Amazon |
| tillverkare | Build.MANUFACTURER | Amazon |
| version | Build.DEVICE | montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | hårdkodad | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

Anslutningsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

Programinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|-----------|-----------------|
| applicationId | hårdkodad | REF30 |

#### Roku OS {#rokuos}

Om du vill skapa `X-Device-Info`-rubriken för enheter som kör [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) kan du läsa följande dokument:

* Kör utvecklardokumentation för [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

Enhetsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|--------------------------------------------|-----------------|
| modell | hårdkodad | &quot;Roku&quot; |
| leverantör | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| tillverkare | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | hårdkodad | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

Anslutningsinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | hårdkodad | true om anslutningen är kopplad |

Programinformationen kan utformas på följande sätt:

| Nyckel | Source | Värde (exempel) |
|---------------|-----------|-----------------|
| applicationId | hårdkodad | REF30 |

### Övriga {#others}

För enhetsplattformar som inte omfattas av dokumentationen bör klientinformationen (enhet, anslutning och program) länkas till alla tillgängliga maskinvaru- och operativsystemattribut (OS), som vanligtvis anges i enhetens maskinvaru- och OS-handböcker.
