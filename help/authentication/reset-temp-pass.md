---
title: Återställ tillfälligt pass
description: Återställ tillfälligt pass
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Återställ tillfälligt pass {#reset-temp-pass}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.
>
>Om du vill använda API:t Återställ tillfälligt pass måste du:
>- be supportteamet om en programsats för ditt registrerade program
>- hämta en åtkomsttoken baserad på [Dynamisk klientregistrering](dynamic-client-registration.md)
> 

För att **återställa ett specifikt tillfälligt pass**, Adobe Pass Authentication ger programmerare en *public* webb-API:

- **Environment:** Anger serverslutpunkten Adobe Pay-TV-pass som tar emot återställningen av tillfälligt Pass-nätverksanropet. Möjliga värden: **Föregående** (*mgmt-prequal.auth.adobe.com*), **Frigör** (*mgmt.auth.adobe.com*) eller **Egen** (reserverat för intern testning i Adobe).
- **OAuth2-åtkomsttoken:** OAuth2-token krävs för att godkänna programmeraren för autentisering med Adobe Pay-TV. En sådan token kan hämtas från [Dynamisk klientregistrering](dynamic-client-registration.md).
- **ID för tillfälligt pass:** Det unika ID:t för det temporära pass-MVPD som ska återställas.(en programmerare kan använda flera Temp Pass MVPD och vill återställa en specifik)
- **Allmän nyckel:** vissa Temp Pass MVPD (dvs. [Kampanjtillfälligt pass](promotional-temp-pass.md)).

Alla ovanstående parametrar (utom *Allmän nyckel*) är obligatoriska. Här är ett exempel på parametrar och det associerade nätverksanropet (exemplet är i form av ett *curl *command):

- **Environment:** Frigör (*mgmt.auth.adobe.com*)
- **OAuth2-åtkomsttoken:** &lt;access_token> från [Dynamisk klientregistrering](dynamic-client-registration.md)
- **Program-ID:** REF
- **ID för tillfälligt pass:** TempPassREF
- **Allmän nyckel:** null (inget värde har angetts)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

en DELETE HTTP-begäran kommer att göras till **/reset** slutpunkt, skicka *OAuth2-åtkomsttoken* i auktoriseringsrubriken och *Enhets-ID*, *Begärande-ID* och *Tillfälligt pass-ID (MVPD ID)* som parametrar.

Om Programmeraren anger ett värde för *Allmän nyckel*, kommer ytterligare ett HTTP-anrop att utföras (den här gången till **/reset/generic** slutpunkt), skicka *Allmän nyckel* innanför *key* request-parameter.

Du kan till exempel ange *Allmän nyckel* till en hash-kodad e-postadress (för Temp Pass MVPD-program som stöder den här typen av funktioner) ger följande HTTP-anrop (e-postmeddelandet är `user@domain.com` dess SHA-256-hash är `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


För att **återställa ett specifikt tillfälligt pass för alla enheter**, Adobe Pass Authentication ger programmerare en *public* webb-API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>URL:en ovan ersätter föregående återställnings-API. Det gamla återställnings-API:t (v1) stöds inte längre.

- **Protokoll:** HTTPS
- **Värd:**
   - Version - mgmt.auth.adobe.com
   - Föregående - mgmt-prequal.auth.adobe.com
- **Sökväg:** /reset-tempass/v3/reset
- **Frågeparametrar:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **Sidhuvuden:** Behörighet: Bearer &lt;access_token_here>
- **Svar:**
   - Lyckades - HTTP 204
   - Fel:
      - HTTP 400 för en felaktig begäran
      - HTTP 401 om åtkomst nekas. Klienten MÅSTE begära en ny access_token.
      - HTTP 403 om klient-ID inte längre tillåts utföra begäranden. Nya klientautentiseringsuppgifter ska genereras.


Exempel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
