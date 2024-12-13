---
title: REST API-referens
description: Rest api reference
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 2%

---

# (Äldre) REST API-referens {#rest-api-reference}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Begränsningsmekanism

REST-API:t för Adobe Pass-autentisering styrs av en [begränsningsmekanism](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Svarsformat {#response-formats}


>[!NOTE]
>
> De API:er som tillhandahålls i dessa tjänster kan returnera svar i antingen XML eller JSON (för API:er som returnerar ett svar). Det finns tre olika sätt att ange svarsformatet i begäran:
>
>* Ange HTTP Accept Header till `application/xml` eller `application/json`.
>* Ange parametern `format=xml` eller `format=json` i nyttolasten för begäran.
>* Anropa webbtjänstens slutpunkt med tillägget `.xml` eller `.json`. Till exempel `/regcode.xml` eller `/regcode.json`
>
>Du kan ange en ENDA av ovanstående metoder. Om du anger flera metoder med olika format kan det leda till fel eller oönskade utdata.

## REST API-slutpunkter {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Sammanfattning av webbtjänster {#web_srvs_summary}

Tabellen nedan visar de tillgängliga webbtjänsterna för kundlösa metoder. Klicka på webbtjänstens slutpunkter om du vill ha mer information (exempelbegäran och svar, indataparametrar, HTTP-metoder osv.)


| Sr | Slutpunkt för webbtjänst | Beskrivning | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Hosted At | Anropat av |
|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | Returnerar slumpmässigt genererad registreringskod och inloggningssidans URI | 2 | Adobe </br>Reg Code Service | Smart enhet |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | Returnerar registreringskodposten som innehåller registreringskoden UUID, registreringskoden och hash-enhets-ID | 8 | Adobe </br>Reg Code Service | Adobe Pass-autentisering |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | Returnerar en lista över konfigurerade MVPD-filer för den som gjorde begäran | 5 | Adobe </br>Adobe Pass </br>authentication </br>Service | Logga in </br>på webben </br> |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | Initierar AuthN-processen genom att informera MVPD-markeringshändelsen. Skapar en post i autentiseringsdatabasen, som avstäms när ett svar tas emot från MVPD (steg 13) | 7 | Adobe </br>Adobe Pass </br>authentication </br>Service | Logga in </br>på webben </br> |
| 5. | SAML Assertion Consumer | Befintligt SAML-arbetsflöde mellan Adobe Pass Authentication och MVPD | 13 | Tjänsten Adobe Pass </br>authentication </br>Service | Adobe Pass-autentisering |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Inloggningswebbappen kan kontrollera om inloggningsflödet lyckades |                                                                                             | Adobe Pass </br>autentisering   </br> Service | Inloggning   </br>Webben   </br> App |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Hämtar AuthN-tokenrelaterade metadata | 15 | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | Tar bort reg.kodposten och släpper reg.koden för återanvändning | 16 | Adobe </br>Reg Code Service | Adobe Pass-autentisering |
| 9. | [&lt;SP_FQDN>/api/v1/authorized](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | Hämtar auktoriseringssvar. | 17 | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | Anger om enheten har en AuthN-token som inte har gått ut. |                                                                                             | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Returnerar AuthN-token om den hittas. |                                                                                             | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | Returnerar AuthZ-token om den hittas. |                                                                                             | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/integration-guide-programmer/rest-apis/rest-api-v1/apis/obtain-short-media-token.md) | Returnerar Short Media Token om den hittas - samma som /api/v1/mediatoken |                                                                                             | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | Hämtar kort mediatoken |                                                                                             | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorized](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | Hämtar listan över förauktoriserade resurser |                                                                                             | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorized/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Hämtar listan över förauktoriserade resurser |                                                                                             | Tjänsten Adobe Pass </br>authentication </br>Service | Inloggningswebbapp |
| 17. | [&lt;SP_FQDN>/api/v1/log](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | Ta bort AuthN- och AuthZ-tokens från lagring |                                                                                             | Adobe Pass </br>autentisering   </br> Service | Smart enhet |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Hämtar användarmetadata när autentiseringsflödet har slutförts | Ej tillämpligt | Ej tillämpligt | Smart enhet |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Skapa en autentiseringstoken för tillfälligt pass eller tillfälligt kampanjpass | Ej tillämpligt | Tjänsten Adobe Pass </br>authentication </br>Service | Smart enhet |


## REST API-säkerhet {#security}

Alla Adobe Pass Authentication REST API:er måste anropas med HTTPS-protokollet för säker kommunikation. Dessutom bör de flesta API:er som anropas innehålla en åtkomsttoken som hämtas enligt beskrivningen i API-dokumentationen för [Hämta åtkomsttoken](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
