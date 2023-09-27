---
title: Registreringssida
description: Registreringssida
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 0%

---

# Registreringssida {#registration-page}

## REST API-slutpunkter {#clientless-endpoints}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#create-reg-code-svc}

Returnerar slumpmässigt genererad registreringskod och inloggningssidans URI.

| Slutpunkt | Anropat  </br>Av | Indata   </br>Parameter | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestor}/regcode</br>Till exempel:</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Strömmande app</br>eller</br>Programmerartjänst | 1. begärande  </br>    (Bankomponent)</br>2.  deviceId (Hashed)   </br>    (Obligatoriskt)</br>3.  device_info/X-Device-Info (obligatoriskt)</br>4.  mvpd (valfritt)</br>5.  ttl (valfritt)</br>6.  _deviceType_</br> 7.  _deviceUser_ (Föråldrat)</br>8.  _appId_ (Föråldrat) | POST | XML eller JSON som innehåller en registreringskod och information eller felinformation om felet misslyckas. Se scheman och exempel nedan. | 201 |

{style="table-layout:auto"}

| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| device_info/</br>X-Device-Info | Information om direktuppspelningsenhet.</br>**Anteckning**: Detta kan skickas som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br>Läs mer här: [Skicka information om enhet och anslutning](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | Det MVPD-ID som den här åtgärden gäller för. |
| ttl | Hur länge den här regkoden ska vara i sekunder.</br>**Anteckning**: Det högsta tillåtna värdet för ttl är 36 000 sekunder (10 timmar). Högre värden resulterar i ett 400 HTTP-svar (felaktig begäran). If `ttl` är tom, Adobe Pass Authentication anger standardvärdet 30 minuter. |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br>Om den här parametern är korrekt angiven erbjuder ESM värden som är [uppdelad per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när du använder Klientlös, så att olika typer av analyser kan utföras, till exempel Roku, AppleTV och Xbox.</br>Se, [Fördelar med att använda parametern för enhetstyp utan klient i passningsvärden ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Anteckning**: device_info ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. </br>**Anteckning**: device_info ersätter den här parametern. |

{style="table-layout:auto"}


>[!CAUTION]
>
>**IP-adress för direktuppspelningsenhet**
></br>
>För klient-till-server-implementeringar skickas IP-adressen för direktuppspelningsenheten implicit med det här anropet.  För implementeringar från server till server, där **regcode** anrop görs i programmeringstjänsten och inte i direktuppspelningsenheten. Följande huvud krävs för att skicka IP-adressen för direktuppspelningsenheten:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>där `<streaming\_device\_ip>` är den offentliga IP-adressen för direktuppspelningsenheten.
></br></br>
>Exempel:</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### XML-schema för svar {#xml-schema}


#### Registreringskod XSD {#registration-code-xsd}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

</br>

| Elementnamn | Beskrivning |
| --------------- | ------------------------------------------------------------------------------------ |
| id | UUID genererat av registreringskodstjänsten |
| kod | Registreringskod som genererats av registreringskodstjänsten |
| begärande | Begärande-ID |
| mvpd | Mvpd-ID |
| genererad | Tidsstämpel för att skapa registreringskod (i millisekunder sedan 1 januari 1970 GMT) |
| förfaller | Tidsstämpel när registreringskoden upphör (i millisekunder sedan 1 januari 1970 GMT) |
| deviceId | Unikt enhets-ID (eller XSTS-token) |
| deviceType | Enhetstyp |
| deviceUser | Användaren är inloggad på enheten |
| appId | Program-ID |
| appVersion | Programversion |
| registrationURL | URL till inloggningswebbappen som ska visas för slutanvändaren |

{style="table-layout:auto"}


</br>

### Felmeddelande-XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### Exempelsvar {#sample-response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
