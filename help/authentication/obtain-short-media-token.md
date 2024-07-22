---
title: Hämta kort medietoken
description: hämta kort medietoken
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Hämta kort medietoken {#obtain-short-media-token}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!NOTE]
>
> REST API-implementeringen begränsas av [Begränsningsmekanismen](/help/authentication/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Mellanlagring - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Mellanlagring - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Hämtar kort medietoken.

| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br> eller</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>Till exempel:</br></br>&lt;SP_FQDN>/api/v1/tokens/media | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  resurs (obligatoriskt)</br>4.  device_info/X-Device-Info (obligatoriskt)</br>5.  _deviceType_</br> 6.  _deviceUser_ (utgått)</br>7.  _appId_ (inaktuellt) | GET | XML eller JSON som innehåller en Base64-kodad medietoken eller felinformation om felet misslyckas. | 200 - lyckades </br> 403 - misslyckades |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| resurs | En sträng som innehåller ett resourceId (eller MRSS-fragment), identifierar innehållet som begärts av en användare och känns igen av MVPD-auktoriseringsslutpunkter. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Detta kan skickas som device_info som URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (till exempel Roku, PC).</br></br>Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när du använder Klientlös, så att olika typer av analyser kan utföras för. Till exempel Roku, AppleTV och Xbox.</br></br>Se [Fördelar med att använda parametern client_type ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Obs!** Parametern device_info kommer att ersättas. |
| _deviceUser_ | Enhetens användaridentifierare.</br></br>**Obs!** Om det används ska deviceUser ha samma värden som i begäran [Skapa registreringskod](/help/authentication/registration-code-request.md). |
| _appId_ | Program-ID/namn. </br></br>**Obs!**: device_info ersätter den här parametern. Om det används ska `appId` ha samma värden som i begäran [Skapa registreringskod](/help/authentication/registration-code-request.md). |

{style="table-layout:auto"}

### Exempelsvar {#response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Kompatibilitet med medieverifieringsbibliotek

Fältet `serializedToken` från anropet &quot;Hämta kort medietoken&quot; är en Base64-kodad token som kan verifieras mot Adobe Media Verification Library.
