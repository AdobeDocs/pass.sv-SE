---
title: Hämta auktoriseringstoken
description: Hämta auktoriseringstoken
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# Hämta auktoriseringstoken {#retrieve-authorization-token}

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

Hämtar AuthZ-token.


| Slutpunkt | Anropat  </br>Av | Indata   </br>Parametrar | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authz</br></br>Till exempel:</br></br>&lt;sp_fqdn>/api/v1/tokens/authz | Strömmande app</br></br>eller</br></br>Programmerartjänst | 1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  resurs (obligatoriskt)</br>4.  device_info/X-Device-Info (obligatoriskt)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Föråldrat)</br>7.  _appId_ (Föråldrat) | GET | 1. Lyckades</br>2.  Autentiseringstoken  </br>    hittades inte eller har gått ut:   </br>    Förklaring av XML  </br>    för författartoken hittades inte</br>3.  Auktoriseringstoken  </br>    hittades inte:  </br>    XML-förklaring</br>4.  Auktoriseringstoken  </br>    utgången:  </br>    XML-förklaring | 200 - lyckades  </br>412 - Ingen AuthN</br></br>404 - Ingen AuthZ</br></br>410 - AuthZ har upphört att gälla |

{style="table-layout:auto"}

</br>

| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| resurs | En sträng som innehåller ett resourceId (eller MRSS-fragment), identifierar innehållet som begärts av en användare och känns igen av MVPD-auktoriseringsslutpunkter. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Anteckning**: Detta kan skickas som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Enhetstypen (till exempel Roku, PC).</br></br>Om den här parametern är korrekt angiven erbjuder ESM värden som är [uppdelad per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när du använder Klientlös, så att olika typer av analyser kan utföras, till exempel Roku, AppleTV och Xbox.</br></br>Se, [Fördelar med att använda parametern för enhetstyp utan klient i passningsvärden ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Anteckning**: device_info ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. </br></br>**Anteckning**: device_info ersätter den här parametern. |

{style="table-layout:auto"}


### Exempelsvar {#response}



#### Lyckades

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### Autentiseringstoken saknas eller har upphört att gälla:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### Autentiseringstoken hittades inte:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found",
        "details": null
    }
```

</br>



#### Auktoriseringstoken har upphört att gälla:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
