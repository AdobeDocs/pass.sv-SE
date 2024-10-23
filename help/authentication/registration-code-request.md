---
title: Registreringssida
description: Registreringssida
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3c44f1dfbbb5b9ec31f13e267dc691e14dd2ddfa
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# Registreringssida {#registration-page}

## REST API-slutpunkter {#clientless-endpoints}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!NOTE]
>
> REST API-implementeringen begränsas av [Begränsningsmekanismen](/help/authentication/throttling-mechanism.md)

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## Beskrivning {#create-reg-code-svc}

Returnerar slumpmässigt genererad registreringskod och inloggningssidans URI.

| Slutpunkt | Anropat <br>av | Indata   <br>Parameter | HTTP <br>Metod | Svar | HTTP <br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>Till exempel:<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Direktuppspelande app<br>eller<br>Programmeringtjänst | 1. beställare <br>    (Bankomponent)<br>2.  deviceId (Hashed)   <br>    (Obligatoriskt)<br>3.  device_info/X-Device-Info (obligatoriskt)<br>4.  mvpd (valfritt)<br>5.  ttl (valfritt)<br> | POST | XML eller JSON som innehåller en registreringskod och information eller felinformation om felet misslyckas. Se exemplen nedan. | 201 |

{style="table-layout:auto"}

| Indataparameter | Typ | Beskrivning |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Behörighet | Header <br> Värde: Bearer &lt;access_token> | DCR-åtkomsttoken |
| Acceptera | Header <br> Värde: application/json | ange vilken innehållstyp klienten ska förstå |
| begärande | Frågeparameter | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Frågeparameter | Byte för enhets-ID. |
| device_info/<br>X-Device-Info | device_info: Body <br> X-Device-Info: Header | Information om direktuppspelningsenhet.<br>**Obs!**: Detta kan skickas som device_info som URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. <br>Mer information finns i [Skicka information om enheter och anslutningar](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | Frågeparameter | Det MVPD-ID som den här åtgärden gäller för. |
| ttl | Frågeparameter | Hur länge den här regkoden ska vara i sekunder.<br>**Obs!**: Det högsta tillåtna värdet för ttl är 36000 sekunder (10 timmar). Högre värden resulterar i ett 400 HTTP-svar (felaktig begäran). Om `ttl` lämnas tomt anges standardvärdet 30 minuter av Adobe Pass Authentication. |
| _deviceType_ | Frågeparameter | Föråldrat, ska inte användas. |
| _deviceUser_ | Frågeparameter | Föråldrat, ska inte användas. |
| _appId_ | Frågeparameter | Föråldrat, ska inte användas. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**IP-adress för direktuppspelningsenhet**
><br>
>För klient-till-server-implementeringar skickas IP-adressen för direktuppspelningsenheten implicit med det här anropet.  För Server-till-Server-implementeringar, där anropet **regcode** görs till Programmer-tjänsten och inte till direktuppspelningsenheten, krävs följande rubrik för att skicka IP-adressen för direktuppspelningsenheten:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>där `<streaming\_device\_ip>` är den offentliga IP-adressen för direktuppspelningsenheten.
><br><br>
>Exempel:<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### SVAR-JSON


#### JSON-PROV för registreringskod

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| Elementnamn | Beskrivning |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | UUID genererat av registreringskodstjänsten |
| kod | Registreringskod som genererats av registreringskodstjänsten |
| begärande | Begärande-ID |
| mvpd | Mvpd-ID |
| genererad | Tidsstämpel för att skapa registreringskod (i millisekunder sedan 1 januari 1970 GMT) |
| förfaller | Tidsstämpel när registreringskoden upphör (i millisekunder sedan 1 januari 1970 GMT) |
| deviceId | Base64 Unikt enhets-ID |
| info:deviceId | Base64-enhetstyp |
| info:deviceInfo | Base64 Normaliserad enhetsinformation bygger på information som mottagits från User-Agent, X-Device-Info eller device_info |
| info:userAgent | Användaragent som skickas av programmet |
| info:originalUserAgent | Användaragent som skickas av programmet |
| info:authenticationType | OAUTH2 för anrop med DCR |
| info:sourceApplicationInformation | Programinformation som konfigurerats i DCR |

{style="table-layout:auto"}


<br>

### JSON-svarsexempel för felmeddelande (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

