---
title: Hämta lista över förauktoriserade resurser
description: Hämta lista över förauktoriserade resurser
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Hämta lista över förauktoriserade resurser {#retrieve-list-of-preauthorized-resources}

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

En begäran till Adobe Pass Authentication om att få en lista över förauktoriserade resurser.

Det finns två uppsättningar API:er: en uppsättning för Streaming App eller Programmer Service och en uppsättning för Second Screen Web App. Den här sidan beskriver API:t för Streaming App eller Programmer Service.


| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/förauktorisera | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. beställare (obligatoriskt)</br>2.  deviceId (obligatoriskt)</br>3.  resurslista (obligatoriskt)</br>4.  device_info/X-Device-Info (obligatoriskt)</br>5.  _deviceType_</br> 6.  _deviceUser_ (utgått)</br>7.  _appId_ (inaktuellt) | GET | XML eller JSON som innehåller individuella beslut före auktorisering eller felinformation. Se exemplen nedan. | 200 - Lyckades</br></br>400 - Felaktig begäran</br></br>401 - Obehörig</br></br>405 - Metoden tillåts inte </br></br> 412 - Förhandsvillkoret misslyckades</br></br>500 - Internt serverfel |


| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| resurslista | En sträng som innehåller en kommaavgränsad lista med resourceIds som identifierar det innehåll som kan vara tillgängligt för en användare och som känns igen av MVPD-auktoriseringsslutpunkter. |
| device_info/</br></br>X-Device-Info | Information om direktuppspelningsenhet.</br></br>**Obs!**: Detta kan skickas som device_info som URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (till exempel Roku, PC).</br></br>Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när klientlösa används, så att olika typer av analyser kan utföras, till exempel Roku, AppleTV och Xbox.</br></br>Se [fördelarna med att använda en enhetstypparameter utan klient i pass-mått ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Obs!** `device_info` ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. </br></br>**Obs!**: device_info ersätter den här parametern. |



### Exempelsvar {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
