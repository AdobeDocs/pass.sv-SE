---
title: Användarmetadata
description: Användarmetadata
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# Användarmetadata {#user-metadata}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!NOTE]
>
> REST API-implementeringen begränsas av [Begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Hämta metadata som MVPD delade om den autentiserade användaren.


| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. beställare</br>2.  deviceId (obligatoriskt)</br>3.  device_info/X-Device-Info (obligatoriskt)</br>4.  deviceType</br>5.  deviceUser (deprecated)</br>6.  appId (utgått) | GET | XML eller JSON som innehåller användarmetadata eller felinformation om det misslyckas. | 200 - lyckades<p>404 - Inga metadata hittades<p>412 - Ogiltig AuthN-token (t.ex. utgången token) |


| Indataparameter | Beskrivning |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| device_info/<p>X-Device-Info | Information om direktuppspelningsenhet.</br></br> **Obs!** Det här kan skickas device_info som en URL-parameter, men på grund av parameterns potentiella storlek och begränsningar i längden på en GET-URL, bör det skickas som X-Device-Info i http-huvudet. </br></br> Se de fullständiga detaljerna i [Skicka information om enheter och anslutningar](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | Enhetstypen (t.ex. Roku, PC).</br></br> Om den här parametern är korrekt har ESM värden som är [nedbrutna per enhetstyp](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics) när Clientless används, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.</br></br> Se [Fördelar med att använda parameter för enhetstyp utan klient i mätvärden för pass ](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Obs!** `device_info` ersätter den här parametern. |
| _deviceUser_ | Enhetens användaridentifierare.</br></br> **Obs!** Om det används bör `deviceUser` ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | Program-ID/namn. </br></br> **Obs!** `device_info` ersätter den här parametern. Om det används ska `appId` ha samma värden som i begäran [Skapa registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

>[!NOTE]
> 
>Användarmetadatainformation ska vara tillgänglig när autentiseringsflödet har slutförts, men kan uppdateras i auktoriseringsflödet beroende på MVPD och på metadatatypen.




## Exempelsvar {#sample-response}

Efter ett lyckat anrop kommer servern att svara med ett XML- (standard) eller JSON-objekt med en struktur som liknar den som visas nedan:


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

I objektets rot finns det tre noder:

* *uppdaterad*: anger en UNIX-tidsstämpel som representerar den senaste gången metadata uppdaterades. Den här egenskapen ställs in från början av servern när metadata genereras under autentiseringsfasen. Efterföljande anrop (efter att metadata har uppdaterats) resulterar i en inkrementell tidsstämpel.
* *data*: innehåller de faktiska metadatavärdena.
* *krypterad*: en array med de krypterade egenskaperna. Om du vill dekryptera ett specifikt metadatavärde måste programmeraren utföra en Base64-avkodning på metadata och sedan tillämpa en RSA-dekryptering på resultatvärdet med hjälp av dess egen privata nyckel (Adobe krypterar metadata på servern med programmerarens offentliga certifikat).

Om ett fel inträffar returnerar servern ett XML- eller JSON-objekt som anger ett detaljerat felmeddelande.

Mer information finns i [Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md).

[Tillbaka till REST API-referens](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
