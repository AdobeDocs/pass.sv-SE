---
title: Använda Charles Proxy
description: Använda Charles Proxy
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 175755aa7463257487b29c5f4da989cf34e91bfd
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# (Äldre) Använda Charles Proxy {#using-charles-proxy}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

**Charles:** <http://charlesproxy.com>


## Hämta, installera och komma igång med Charles Proxy {#download-install-and-get-stared-with-charles-proxy}

- **Hämta** - <http://www.charlesproxy.com/download/>
- **Installera** - <http://www.charlesproxy.com/documentation/installation/>
- **Komma igång** - <http://www.charlesproxy.com/documentation/getting-started/>


## Struktur kontra sekvensflikar {#structure-vs-sequence-tabs}

Det finns två olika sätt att visa trafiken:

1. **Struktur** - Begäranden grupperas efter värd
1. **Sekvens** - Begäranden visas i den ordning som de anropas


## SSL och certifikat {#ssl-and-certificates}

Aktivera SSL-proxy `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

Markera kryssrutan Aktivera SSL-proxy och lägg till alla HTTPS-platser.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- SSL-proxy - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL-certifikat - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- SSL Proxying från mobila enheter - se nedan.


## Ignorera/exkludera värdar {#ignore-/-exclude-hosts}

Om dina utdata blir för röriga kan du välja att ignorera eller exkludera platser. Du kan ignorera eller exkludera platser genom att göra något av följande:

- Högerklicka på de förfrågningar du vill ignorera och välj sedan &quot;Ignorera&quot;
- Lägg till platser som ska uteslutas från `\[ *Proxy -\> Recording Settings... -\> Exclude* \]` manuellt


## DNS-förfalskning {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS-förfalskning är mycket användbart när du försöker dirigera om en begäran till en annan IP-adress, särskilt när du arbetar med mobila enheter:

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Kartfjärrmapp {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Med kartfjärrkontrollen kan du dirigera om en inkommande begäran till en annan slutpunkt. Det vanligaste användningsexemplet för den här funktionen är &quot;Mappa&quot; `AccessEnabler.swf` till `AccessEnablerDebug.swf:`

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Invertera proxy {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Mobil {#mobile}

### Använd Charles på en iOS-enhet (iPhone/iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### SSL-anslutning från iPhone {#ssl-connection-from-iphone}

Bläddra till <http://charlesproxy.com/charles.crt> från din iOS-enhet.  Dialogrutan för certifikatinstallation startas:

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

Klicka på `\[ *Install*... *Install*... *Done* \]` för att slutföra installationen av certifikatet.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Using Charles from an iOS device {#using-charles-from-an-ios-device}

På din iOS-enhet väljer du `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`. Klicka på den lilla blå pilen bredvid nätverket och gå sedan till HTTP-proxy och välj Manuell:


</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
Här måste du ange IP-adress och port för datorn där du kör Charles. <span style="line-height: 1.6em;">Om du nu öppnar Safari på din iOS-enhet och försöker öppna en webbsida bör du få följande popup-fönster på datorn som kör Charles:

</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
Klicka på"Tillåt" för att tillåta att enheten använder Charles för att proxyvisa alla dess
förfrågningar.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Lita på alla certifikat {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS Authentication error - adobepass.ios.app cannot found

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Använd Charles för Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Bläddra till [Charles proxy](http://charlesproxy.com/charles.crt) från din Android-enhet.
