---
title: Kontrollera autentiseringstoken
description: Kontrollera autentiseringstoken
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Kontrollera autentiseringstoken {#check-authentication-token}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!NOTE]
>
> REST API-implementeringen begränsas av [Begränsningsmekanismen](/help/authentication/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Anger om enheten har en autentiseringstoken som inte har gått ut.

| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  device_info/X-Device-Info (obligatoriskt)</br>4.  _deviceType_ </br>5.  _deviceUser_ (utgått)</br>6.  _appId_ (inaktuellt) | GET | XML eller JSON som innehåller felinformation om det misslyckas. | 200 - lyckades   </br>403 - Inga lyckade |

{style="table-layout:auto"}


| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Det här kan skickas device_info som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br>Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när Clientless används, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br>Mer information finns i [Fördelar med att använda parametern deviceType utan klient i Adobe Pass-autentiseringsmått ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Obs!** Parametern kommer att ersättas av device_info. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn.</br>**Obs!**: device_info ersätter den här parametern. |

{style="table-layout:auto"}


## Svar (om misslyckades) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [Tillbaka till REST API-referens](/help/authentication/rest-api-reference.md)
