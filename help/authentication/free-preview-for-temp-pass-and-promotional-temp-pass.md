---
title: Kostnadsfri förhandsgranskning för tillfälligt pass och tillfälligt kampanjpass
description: Kostnadsfri förhandsgranskning för tillfälligt pass och tillfälligt kampanjpass
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Kostnadsfri förhandsgranskning för tillfälligt pass och tillfälligt kampanjpass {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Tillåter att en autentiseringstoken skapas för tillfälligt pass- och kampanjtillfälligt pass utan behov av en andra skärm.


| Slutpunkt | Anropat  </br>Av | Indata   </br>Parametrar | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | Strömmande app</br></br>eller</br></br>Programmerartjänst | 1. beställare_id (obligatoriskt)</br>    </br>2.  deviceId (obligatoriskt)</br>    </br>3.  mso_id (obligatoriskt)</br>    </br>4.  domain_name (obligatoriskt)</br>    </br>5.  device_info/X-Device-Info (obligatoriskt)</br>6.  deviceType</br>    </br>7.  deviceUser (utgått)</br>    </br>8.  appId (utgått)</br>    </br>9.  generisk_data (valfritt) | POST | Svaret blir 2004 No Content, vilket anger att token har skapats och är klar att användas för redigeringsflödena. | 204 - Inget innehåll   </br>400 - Ogiltig begäran |

<div>


| Indataparameter | Beskrivning |
| --- | --- |
| beställare_id | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| mso_id | Det MVPD-ID som den här åtgärden är giltig för. |
| domain_name | Domännamnet som en token ska beviljas för. Detta jämförs med domänerna för tjänsteleverantören när en auktoriseringstoken beviljas. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Anteckning**: Detta kan skickas som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br>Om den här parametern är korrekt angiven erbjuder ESM värden som är [uppdelad per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när du använder Klientlös, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br>Se [Fördelar med att använda parametrar för enhetstyp utan klient ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Anteckning**: device_info ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare.</br></br>**Anteckning**: Om det används ska deviceUser ha samma värden som i [Skapa registreringskod](/help/authentication/registration-code-request.md) begäran. |
| _appId_ | Program-ID/namn. </br></br>**Anteckning**: device_info ersätter den här parametern. Om den används `appId` ska ha samma värden som i [Skapa registreringskod](/help/authentication/registration-code-request.md) begäran. |
| generisk_data | Används för att begränsa omfattningen av token för tillfälligt kampanjpass. |


### [Tillbaka till REST API-referens](/help/authentication/rest-api-reference.md)
