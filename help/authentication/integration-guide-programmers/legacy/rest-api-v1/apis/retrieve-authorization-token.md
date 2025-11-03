---
title: Hämta auktoriseringstoken
description: Hämta auktoriseringstoken
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---

# (Äldre) Hämta auktoriseringstoken {#retrieve-authorization-token}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

>[!NOTE]
>
> REST API-implementeringen begränsas av [Begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Hämtar AuthZ-token.


| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>Till exempel:</br></br>&lt;SP_FQDN>/api/v1/tokens/authz | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | &#x200B;1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  resurs (obligatoriskt)</br>4.  device_info/X-Device-Info (obligatoriskt)</br>5.  _deviceType_</br> 6.  _deviceUser_ (utgått)</br>7.  _appId_ (inaktuellt) | GET | &#x200B;1. Lyckades</br>2.  Autentiseringstoken </br>    hittades inte eller har gått ut:   </br>    Förklaring av XML </br>    för författartoken hittades inte</br> .  Auktoriseringstoken </br>    hittades inte: </br>    XML-förklaring </br>4.  Auktoriseringstoken </br>    utgången: </br>    XML-förklaring | 200 - Lyckades </br>412 - Inget AuthN</br></br>404 - Ingen AuthZ</br></br>410 - AuthZ har gått ut |

{style="table-layout:auto"}

</br>

| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| resurs | En sträng som innehåller ett resourceId (eller MRSS-fragment), identifierar det innehåll som begärts av en användare och känns igen av MVPD auktoriseringsslutpunkter. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Detta kan skickas device_info som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL bör det skickas som X-Device-Info i http-huvudet. </br></br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (till exempel Roku, PC).</br></br>Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) när klientlösa används, så att olika typer av analyser kan utföras, till exempel Roku, AppleTV och Xbox.</br></br>Se, [Fördelar med att använda parametern för klientlös enhetstyp i pass-mått ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Obs!** Parametern device_info kommer att ersättas. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. </br></br>**Obs!**: device_info ersätter den här parametern. |

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
