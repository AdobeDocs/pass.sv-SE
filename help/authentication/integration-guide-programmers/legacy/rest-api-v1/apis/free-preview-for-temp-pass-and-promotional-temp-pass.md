---
title: Kostnadsfri förhandsgranskning för tillfälligt pass och tillfälligt kampanjpass
description: Kostnadsfri förhandsgranskning för tillfälligt pass och tillfälligt kampanjpass
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Äldre) Kostnadsfri förhandsvisning för tillfälligt pass och kampanjtillfälligt pass {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Tillåter att en autentiseringstoken skapas för tillfälligt pass- och kampanjtillfälligt pass utan behov av en andra skärm.


| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | &#x200B;1. request_id (obligatoriskt)</br>    </br>2.  deviceId (obligatoriskt)</br>    </br>3.  mso_id (obligatoriskt)</br>    </br>4.  domain_name (obligatoriskt)</br>    </br>5.  device_info/X-Device-Info (obligatoriskt)</br>6.  deviceType</br>    </br>7.  deviceUser (utgått)</br>    </br>8.  appId (utgått)</br>    </br>9.  generisk_data (valfritt) | POST | Svaret blir 2004 No Content, vilket anger att token har skapats och är klar att användas för redigeringsflödena. | 204 - Inget innehåll   </br>400 - Ogiltig begäran |

<div>


| Indataparameter | Beskrivning |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| beställare_id | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| mso_id | Det MVPD-ID som den här åtgärden gäller för. |
| domain_name | Domännamnet som en token ska beviljas för. Detta jämförs med domänerna för tjänsteleverantören när en auktoriseringstoken beviljas. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Detta kan skickas device_info som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL bör det skickas som X-Device-Info i http-huvudet. </br></br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br>Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) när Clientless används, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br>Se [Fördelar med att använda parametrar för klientlös enhetstyp ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Obs!** Den här parametern ersätts av device_info. |
| _deviceUser_ | Enhetens användaridentifierare.</br></br>**Obs!** Om det används ska deviceUser ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | Program-ID/namn. </br></br>**Obs!**: device_info ersätter den här parametern. Om det används ska `appId` ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| generisk_data | Används för att begränsa omfattningen av token för tillfälligt kampanjpass. |


### [Tillbaka till REST API-referens](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
