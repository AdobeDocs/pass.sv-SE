---
title: Återställ tillfälligt pass
description: Återställ tillfälligt pass
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '478'
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
> * Hämta klientautentiseringsuppgifterna enligt beskrivningen i API-dokumentationen för [Hämta klientautentiseringsuppgifter](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Hämta åtkomsttoken enligt beskrivningen i API-dokumentationen för [Hämta åtkomsttoken](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Mer information om hur du skapar ett registrerat program och hämtar programsatsen finns i [Översikt över registrering av dynamiska klienter](./dcr-api/dynamic-client-registration-overview.md) .

För att **återställa ett specifikt tillfälligt pass** tillhandahåller Adobe Pass Authentication programmerare ett *offentligt* webb-API:

* **Miljö:** anger serverslutpunkten Adobe Pay-TV som tar emot återställningen av tillfälligt pass-nätverksanropet. Möjliga värden: **Prequal** (*mgmt* prequal.auth.adobe.com*), **Release** (*mgmt.auth.adobe.com*) eller **Custom** (reserverat för intern Adobe-testning).
* **OAuth2-åtkomsttoken:** OAuth2-token krävs för att auktorisera programmeraren för Adobe Pay-TV-autentisering. En sådan token kan hämtas enligt beskrivningen i API-dokumentationen för [Hämta åtkomsttoken](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
* **Temporärt ID:** är det unika ID som används för att återställa det temporära MVPD-dokumentet.(en programmerare kan använda flera Temp Pass MVPD och vill återställa en specifik)
* **Allmän nyckel:** vissa temporära pass-MVPD (dvs. [Kampanjtemporärt pass](promotional-temp-pass.md)).

Alla ovanstående parametrar (förutom *Allmän nyckel*) är obligatoriska. Här är ett exempel på parametrar och det associerade nätverksanropet (exemplet är i form av ett *curl *command):

* **Miljö:** Utgåva (*mgmt.auth.adobe.com*)
* **OAuth2-åtkomsttoken:** &lt;åtkomsttoken> enligt beskrivningen i [Hämta åtkomsttoken](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API-dokumentationen
* **Program-ID:** REF
* **Tillfälligt pass-ID:** TempPassREF
* **Allmän nyckel:** null (inget värde har angetts)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

en DELETE HTTP-begäran kommer att göras till **/reset**-slutpunkten, och *OAuth2-åtkomsttoken* skickas i auktoriseringshuvudet och *enhets-ID*, *begärande-ID* och *Temp Pass-ID (MVPD ID)* skickas som parametrar.

Om programmeraren anger ett värde för den *allmänna nyckeln* kommer ett annat HTTP-anrop att utföras (den här gången till **/reset/generic** -slutpunkten) och *Generic Key* skickas i *key* -begärandeparametern.

Om du till exempel anger *Allmän nyckel* som en hash för e-postadressen (till exempel
Temp-pass-MVPD-program som stöder den här typen av funktioner) ger
efter HTTP-anrop (e-postmeddelandet är `user@domain.com` dess SHA-256
hash är `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Adobe Pass Authentication förser programmerare med ett *public* webb-API för att **återställa ett specifikt tillfälligt pass för alla enheter**:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>URL:en ovan ersätter föregående återställnings-API. Det gamla återställnings-API:t (v1) stöds inte längre.

* **Protokoll:** HTTPS
* **Värd:**
   * Version - mgmt.auth.adobe.com
   * Föregående - mgmt-prequal.auth.adobe.com
* **Sökväg:** /reset-tempass/v3/reset
* **Frågeparametrar:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Rubriker:** Behörighet: Bearer &lt;access_token_here>
* **Svar:**
   * Lyckades - HTTP 204
   * Fel:
      * HTTP 400 för en felaktig begäran
      * HTTP 401 om åtkomst nekas. Klienten MÅSTE begära en ny access_token.
      * HTTP 403 om klient-ID inte längre tillåts utföra begäranden. Nya klientautentiseringsuppgifter ska genereras.


Exempel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
