---
title: Initiera auktorisering
description: Initiera auktorisering
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# (Äldre) Initiera auktorisering {#initiate-authorization}

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

Hämtar auktoriseringssvar.

| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorized | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | &#x200B;1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  resurs (obligatoriskt)</br>4.  device_info/X-Device-Info (obligatoriskt)</br>5.  _deviceType_</br> 6.  _deviceUser_ (utgått)</br>7.  _appId_ (utgått)</br>8.  extra parametrar (valfritt) | GET | XML eller JSON som innehåller auktoriseringsinformation eller felinformation om det misslyckas. Se exemplen nedan. | 200 - lyckades </br> 403 - misslyckades |

{style="table-layout:auto"}

</br>


| Indataparameter | Beskrivning |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| resurs | En sträng som innehåller ett resourceId (eller MRSS-fragment), identifierar det innehåll som begärts av en användare och känns igen av MVPD auktoriseringsslutpunkter. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Detta kan skickas device_info som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL bör det skickas som X-Device-Info i http-huvudet. </br></br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br>Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) när Clientless används, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br>Se [Fördelar med parameter för klientlös enhetstyp i passningsmått &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Obs!** Parametern device_info kommer att ersättas. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. </br></br>**Obs!**: device_info ersätter den här parametern. |
| extra parametrar | Anropet kan även innehålla valfria parametrar som aktiverar andra funktioner som:</br></br>* generic_data - aktiverar användningen av [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>Exempel: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-adress för direktuppspelningsenhet**</br>
>För klient-till-server-implementeringar skickas IP-adressen för direktuppspelningsenheten implicit med det här anropet.  För Server-till-Server-implementeringar, där anropet **regcode** görs av programmeringstjänsten och inte av direktuppspelningsenheten, krävs följande rubrik för att skicka IP-adressen för direktuppspelningsenheten:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>där `<streaming\_device\_ip>` är den offentliga IP-adressen för direktuppspelningsenheten.</br></br>
>Exempel:</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Exempelsvar {#sample-response}

* **Fall 1: Lyckades**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;auktorisering>
    &lt;expirres>1348148289000&lt;/expirres>
    &lt;mvpd>sampleMvpdId&lt;/mmbd&lt;/mres> vpd>
    &lt;beställare>sampleRequestorId&lt;/beställare>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authentication>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>När svaret kommer från en Proxy MVPD kan det innehålla ytterligare ett element med namnet `proxyMvpd`.



* **Fall 2: Autentisering nekas**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
