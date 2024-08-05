---
title: Header - X-Device-Info
description: REST API V2 - Header - X-Device-Info
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Header - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Huvudet <b>X-Device-Info</b> innehåller klientinformationen (enhet, anslutning och program) som hör till den faktiska direktuppspelningsenheten.

## Syntax {#syntax}

<table>
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

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Nyckel</th>
        <th style="background-color: #EFF2F7;">Beskrivning</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Närvaro</th>
        <th style="background-color: #EFF2F7;">Möjliga värden</th>
    </tr>
    <tr>
        <td>primärHardwareType</td>
        <td>Enhetens primära maskinvarutyp.</td>
        <td></td>
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
        <td>modell</td>
        <td>Enhetens modellnamn.</td>
        <td><i>obligatoriskt</i></td>
        <td>t.ex. iPhone, SM-G930V, AppleTV osv.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>Enhetens version.</td>
        <td></td>
        <td>t.ex. 2.0.1 osv.</td>
    </tr>
    <tr>
        <td>tillverkare</td>
        <td>Tillverkningsföretaget/tillverkningsorganisationen för enheten.</td>
        <td></td>
        <td>t.ex. Samsung, LG, ZTE, Huawei, Motorola, Apple osv.</td>
    </tr>
    <tr>
        <td>leverantör</td>
        <td>Enhetens säljande företag/organisation.</td>
        <td></td>
        <td>t.ex. Apple, Samsung, LG, Google osv.</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>Enhetens operativsystemnamn.</td>
        <td><i>obligatoriskt</i></td>
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
        <td>osFamily</td>
        <td>Enhetens operativsystemsgruppnamn.</td>
        <td></td>
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
        <td>osVendor</td>
        <td>Enhetens operativsystemsleverantör.</td>
        <td></td>
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
        <td>osVersion</td>
        <td>Enhetens operativsystemversion.</td>
        <td></td>
        <td>t.ex. 10.2, 9.0.1 osv.</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>Webbläsarens namn.</td>
        <td></td>
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
        <td>browserVendor</td>
        <td>Webbläsarens byggföretag/organisation.</td>
        <td></td>
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
        <td>browserVersion</td>
        <td>Enhetens webbläsarversion.</td>
        <td></td>
        <td>Exempel: 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>Enhetens användaragent.</td>
        <td></td>
        <td>Exempel: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, t.ex. Gecko) Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>Enhetens fysiska skärmbredd.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>Enhetens fysiska skärmhöjd.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>Enhetens pixeldensitet på den fysiska skärmen.</td>
        <td></td>
        <td>Exempel: 294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>Enhetens diagonala mått för fysisk skärm i tum.</td>
        <td></td>
        <td>t.ex. 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>Enhetens IP-adress som används för att skicka HTTP-begäranden.</td>
        <td></td>
        <td>t.ex. 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>Enhetens port som används för att skicka HTTP-begäranden.</td>
        <td></td>
        <td>till exempel 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>Nätverksanslutningstypen.</td>
        <td></td>
        <td>t.ex. WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>Säkerhetsstatus för nätverksanslutning.</td>
        <td></td>
        <td>
            Värdena är begränsade:
            <ul>
                <li>true - om det är ett säkert nätverk</li>
                <li>false - om det är en offentlig aktiv punkt</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>Programmets unika identifierare.</td>
        <td></td>
        <td>t.ex. CNN</td>
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
