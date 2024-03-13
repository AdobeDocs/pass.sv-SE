---
title: Initiera utloggning
description: Initiera utloggning
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Initiera utloggning {#initiate-logout}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!NOTE]
>
> REST API-implementering begränsas av [Begränsningsmekanism](/help/authentication/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Ta bort AuthN- och AuthZ-tokens från lagring.


| Slutpunkt | Anropat  </br>Av | Indata   </br>Parametrar | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/utloggning | Strömmande app</br></br>eller</br></br>Programmerartjänst | 1. begärande</br>2.  deviceId (obligatoriskt)</br>3.  device_info/X-Device-Info (obligatoriskt)</br>4.  _deviceType_</br> 5.  _deviceUser_ (Föråldrat)</br>6.  _appId_ (Föråldrat) | DELETE | Ingen | 204 |


| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Anteckning**: Detta kan skickas som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br>Om den här parametern är korrekt angiven erbjuder ESM värden som är [uppdelad per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när du använder Klientlös, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br>Se [Fördelar med att använda parametern för enhetstyp utan klient i passningsvärden ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Anteckning**: device_info ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare.</br></br>**Anteckning**: Om det används ska deviceUser ha samma värden som i [Skapa registreringskod](/help/authentication/registration-code-request.md) begäran. |
| _appId_ | Program-ID/namn. </br></br>**Anteckning**: device_info ersätter den här parametern. Om den används `appId` ska ha samma värden som i [Skapa registreringskod](/help/authentication/registration-code-request.md) begäran. |

>[!IMPORTANT]
> 
>Inloggningsanropet har för närvarande följande begränsning: det rensar AuthN- och AuthZ-tokens från lagringsutrymmet (dvs på programmeraren/Adobe Pass-autentiseringssidan), men **inte** anropa MVPD-slutpunkten för utloggning.
