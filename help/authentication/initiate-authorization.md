---
title: Initiera auktorisering
description: Initiera auktorisering
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Initiera auktorisering {#initiate-authorization}

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

Hämtar auktoriseringssvar.

| Slutpunkt | Anropat  </br>Av | Indata   </br>Parametrar | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorized | Strömmande app</br></br>eller</br></br>Programmerartjänst | 1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  resurs (obligatoriskt)</br>4.  device_info/X-Device-Info (obligatoriskt)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Föråldrat)</br>7.  _appId_ (Föråldrat)</br>8.  extra parametrar (valfritt) | GET | XML eller JSON som innehåller auktoriseringsinformation eller felinformation om det misslyckas. Se exemplen nedan. | 200 - lyckades  </br>403 - Ingen framgång |

{style="table-layout:auto"}

</br>


| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| resurs | En sträng som innehåller ett resourceId (eller MRSS-fragment), identifierar innehållet som begärts av en användare och känns igen av MVPD-auktoriseringsslutpunkter. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Anteckning**: Detta kan skickas som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br>Om den här parametern är korrekt angiven erbjuder ESM värden som är [uppdelad per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när du använder Klientlös, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br>Se [Fördelar med klientlösa enhetstypparametrar i passningsvärden ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Anteckning**: device_info ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. </br></br>**Anteckning**: device_info ersätter den här parametern. |
| extra parametrar | Anropet kan även innehålla valfria parametrar som möjliggör andra funktioner som:</br></br>* generic_data - aktiverar användningen av [TempPass för kampanjerbjudande](/help/authentication/promotional-temp-pass.md)</br></br>Exempel: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-adress för direktuppspelningsenhet**</br>
>För klient-till-server-implementeringar skickas IP-adressen för direktuppspelningsenheten implicit med det här anropet.  För implementeringar från server till server, där **regcode** anrop görs av programmeringstjänsten och inte av direktuppspelningsenheten. Följande huvud krävs för att skicka IP-adressen för direktuppspelningsenheten:</br></br>
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
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
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
>När svaret kommer från ett MVPD-dokument kan det innehålla ytterligare ett element med namnet `proxyMvpd`.



* **Fall 2: Behörighet nekas**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
