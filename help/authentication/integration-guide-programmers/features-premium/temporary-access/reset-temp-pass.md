---
title: Återställ tillfälligt pass
description: Återställ tillfälligt pass
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# Återställ tillfälligt pass {#reset-temp-pass}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Innan du använder API:t Återställ tillfälligt pass måste du kontrollera att följande krav är uppfyllda:
>
> * Hämta klientautentiseringsuppgifterna enligt beskrivningen i API-dokumentationen för [Hämta klientautentiseringsuppgifter](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Hämta åtkomsttoken enligt beskrivningen i API-dokumentationen för [Hämta åtkomsttoken](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Mer information om hur du skapar ett registrerat program och hämtar programsatsen finns i [Översikt över registrering av dynamiska klienter](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) .

För att **återställa ett specifikt tillfälligt pass för en enhet eller alla enheter** tillhandahåller Adobe Pass Authentication programmerare ett *offentligt* webb-API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **Protokoll:** HTTPS
* **Värd:**
   * Version - mgmt.auth.adobe.com
   * Föregående - mgmt-prequal.auth.adobe.com
* **Sökväg:** /reset-tempass/v3/reset
* **Frågeparametrar:** device_id(när inget värde anges används alla), request_id (obligatoriskt), mvpd_id (obligatoriskt), appId, deviceUser, environment
* **Rubriker:** Behörighet: Bearer &lt;access_token_here>
* **Svar:**
   * Lyckades - HTTP 204
   * Fel:xw
      * HTTP 400 för en felaktig begäran
      * HTTP 401 om åtkomst nekas. Klienten MÅSTE begära en ny access_token.
      * HTTP 403 om klient-ID inte längre tillåts utföra begäranden. Nya klientautentiseringsuppgifter ska genereras.


Exempel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

För att **återställa det flexibla tillfälliga lösenordet för en generisk nyckel eller alla nycklar** tillhandahåller Adobe Pass Authentication programmerare ett *offentligt* webb-API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **Protokoll:** HTTPS
* **Värd:**
   * Version - mgmt.auth.adobe.com
   * Föregående - mgmt-prequal.auth.adobe.com
* **Sökväg:** /reset-tempass/v3/reset/generic
* **Frågeparametrar:** key(when no value is provided, defaults to all), request_id (required), mvpd_id (required), environment
* **Rubriker:** Behörighet: Bearer &lt;access_token_here>
* **Svar:**
   * Lyckades - HTTP 204
   * Fel:
      * HTTP 400 för en felaktig begäran
      * HTTP 401 om åtkomst nekas. Klienten MÅSTE begära en ny access_token.
      * HTTP 403 om klient-ID inte längre tillåts utföra begäranden. Nya klientautentiseringsuppgifter ska genereras.


Om du till exempel anger *Allmän nyckel* som en hash för e-postadressen (till exempel
Temp-pass-MVPD-program som stöder den här typen av funktioner) ger
efter HTTP-anrop (e-postmeddelandet är `user@domain.com` dess SHA-256
hash är `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
