---
title: Initiera utloggning
description: Initiera utloggning
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# (Äldre) Starta utloggning {#initiate-logout}

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

Ta bort AuthN- och AuthZ-tokens från lagring.


| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logOut | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | &#x200B;1. beställare</br>2.  deviceId (obligatoriskt)</br>3.  device_info/X-Device-Info (obligatoriskt)</br>4.  _deviceType_</br> 5.  _deviceUser_ (utgått)</br>6.  _appId_ (inaktuellt) | DELETE | Ingen | 204 |


| Indataparameter | Beskrivning |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Detta kan skickas device_info som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL bör det skickas som X-Device-Info i http-huvudet. </br></br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br>Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) när Clientless används, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br>Se [Fördelar med att använda parametern för enhetstyp utan klient i passningsmått ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Obs!** Parametern device_info kommer att ersättas. |
| _deviceUser_ | Enhetens användaridentifierare.</br></br>**Obs!** Om det används ska deviceUser ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | Program-ID/namn. </br></br>**Obs!**: device_info ersätter den här parametern. Om det används ska `appId` ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

>[!IMPORTANT]
> 
>Logout-anropet har för närvarande följande begränsning: det rensar AuthN- och AuthZ-tokens från lagringsutrymmet (dvs på programmeraren/Adobe Pass-autentiseringssidan), men **anropar inte** MVPD-utloggningsslutpunkten.
