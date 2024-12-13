---
title: Hämta autentiseringstoken
description: Hämta autentiseringstoken
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# (Äldre) Hämta autentiseringstoken {#retrieve-authentication-token}

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

Hämtar autentiseringstoken (AuthN).

| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br>Till exempel:</br></br>&lt;SP_FQDN>/api/v1/tokens/authn | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  device_info/X-Device-Info (obligatoriskt)</br>4.  _deviceType_ (utgått)</br>5.  _deviceUser_ (utgått)</br>6.  _appId_ (inaktuellt) | GET | XML eller JSON som innehåller autentiseringsinformation eller felinformation om det misslyckas. | 200 - Klart.  </br>404 - Det gick inte att hitta token </br>410 - token har upphört att gälla |

{style="table-layout:auto"}


| Indataparameter | Beskrivning |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Detta kan skickas som device_info som URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (till exempel Roku, PC).</br></br>**Obs!**: device_info ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare.</br></br>**Obs!** Om det används ska deviceUser ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | Program-ID/namn. </br></br>**Obs!**: device_info ersätter den här parametern. Om det används ska `appId` ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

{style="table-layout:auto"}

</br>

### Exempelsvar {#response}



#### Lyckades

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





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
        "message": "Not Found"
    }
```
