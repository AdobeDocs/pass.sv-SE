---
title: Förbättrade felkoder
description: Förbättrade felkoder
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2606'
ht-degree: 2%

---

# Förbättrade felkoder {#enhanced-error-codes}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Förbättrade felkoder är en Adobe Pass-autentiseringsfunktion som ger ytterligare felinformation till klientprogram som är integrerade med:

* Adobe Pass Authentication REST API:er:
   * [REST API v1](../../legacy/rest-api-v1/apis/rest-api-overview.md)
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
* Adobe Pass Authentication SDKs Förauktorisera API:
   * [JavaScript SDK (förauktorisera API)](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [iOS/tvOS SDK (förauktorisera API)](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [Android SDK (förauktorisera API)](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) API för förauktorisering är det enda Adobe Pass Authentication SDK API som har stöd för förbättrade felkoder._

>[!IMPORTANT]
>
> Program som integrerar Adobe Pass Authentication REST API v2 kan som standard dra nytta av förbättrade felkoder utan att ytterligare konfiguration krävs.
>
> <br/>
>
> Program som integrerar Adobe Pass Authentication REST API v1 eller SDKs Preauthorized API kan endast utnyttja Enhanced Error Codes om funktionen uttryckligen är aktiverad.
>
> <br/>
>
> Om du vill aktivera den här funktionen explicit skapar du en biljett via [Zendesk](https://adobeprimetime.zendesk.com) och ber din tekniska kontohanterare (TAM) om hjälp.

## Representation {#enhanced-error-codes-representation}

Förbättrade felkoder kan representeras i `JSON`- eller `XML`-format beroende på det inbyggda Adobe Pass Authentication-API:t och det använda Accept-rubrikvärdet (dvs. `application/json` eller `application/xml`):

| Adobe Pass-autentiserings-API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v1 | &amp;check; | &amp;check; |
| REST API v2 | &amp;check; |         |
| SDK Förauktorisera API | &amp;check; |         |

>[!IMPORTANT]
>
> Adobe Pass Authentication kan kommunicera Enhanced Error Codes till klientapplikationer i två former:
>
> <br/>
>
> * **Felinformation på den översta nivån**: I det här fallet finns ***&quot;error&quot;*** -objektet på den översta nivån, och därför kan svarsbrödtexten bara innehålla ***&quot;error&quot;*** -objektet.
> * **Felinformation på objektnivå**: I det här fallet finns ***&quot;error&quot;*** -objektet på objektnivå, och därför kan svarstexten innehålla ett ***&quot;error&quot;*** -objekt för alla objekt som har råkat ut för ett fel under bearbetningen.
>
> <br/>
>
> Läs den offentliga dokumentationen för varje inbyggt Adobe Pass Authentication API för att fastställa informationen för den utökade felkodsrepresentationen.

Se följande HTTP-svar som innehåller exempel på förbättrade felkoder som representeras av `JSON` eller `XML`.

>[!BEGINTABS]

>[!TAB REST API v1 - Felinformation på högsta nivån (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB REST API v1 - Felinformation på högsta nivån (XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!TAB REST API v1 - Felinformation på artikelnivå (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 - Felinformation på högsta nivån (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!TAB REST API v2 - Felinformation på artikelnivå (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!ENDTABS]

Förbättrade felkoder innehåller följande `JSON`-fält eller `XML`-attribut:

| Namn | Typ | Exempel | Begränsad | Beskrivning |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *åtgärd* | *sträng* | *försök igen* | &amp;check; | Adobe Pass-autentiseringen rekommenderade en åtgärd som kan åtgärda situationen enligt definitionen i det här dokumentet. <br/><br/> Mer information finns i avsnittet [Åtgärd](#enhanced-error-codes-action). |
| *status* | *heltal* | *403* | &amp;check; | Statuskoden för HTTP-svar enligt definitionen i [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6) -dokumentet. <br/><br/> Mer information finns i avsnittet [Status](#enhanced-error-codes-status). |
| *kod* | *sträng* | *network_connection_error* | &amp;check; | Den unika identifierarkod för Adobe Pass-autentisering som är associerad med felet enligt definitionen i det här dokumentet. <br/><br/> Mer information finns i avsnittet [Kod](#enhanced-error-codes-code). |
| *meddelande* | *sträng* | *Det går inte att kontakta din TV-leverantör* |            | Det läsbara meddelandet som kan visas för slutanvändaren i vissa fall. <br/><br/> Mer information finns i avsnittet [Svarshantering](#enhanced-error-codes-response-handling). |
| *information* | *sträng* | *Prenumerationspaketet innehåller inte &quot;Live&quot;-kanalen* |            | Det detaljerade meddelandet som kan tillhandahållas av en tjänstpartner i vissa fall, <br/><br/> Det här fältet kanske inte finns om tjänstpartnern inte tillhandahåller något anpassat meddelande. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | Den offentliga dokumentations-URL:en för Adobe Pass-autentisering som länkar till mer information om varför felet uppstod och möjliga lösningar. <br/><br/> Det här fältet innehåller en absolut URL-adress och bör inte härledas från felkoden, beroende på felkontexten kan en annan URL anges. |
| *trace* | *sträng* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | Den unika identifieraren för det svar som kan användas när du kontaktar Adobe Pass Authentication support för att felsöka specifika problem. |

>[!IMPORTANT]
>
> Kolumnen **Begränsad** anger om respektive fält innehåller ett värde från en begränsad uppsättning, medan obegränsade fält kan innehålla alla data.
>
> <br/>
>
> Framtida uppdateringar av det här dokumentet kan lägga till värden i de finite-uppsättningarna, men befintliga värden tas inte bort eller ändras inte.

### Åtgärd {#enhanced-error-codes-representation-action}

Förbättrade felkoder innehåller ett åtgärdsfält som innehåller en rekommenderad åtgärd som kan åtgärda problemet.

Möjliga värden för åtgärdsfältet är:

| Åtgärd | Beskrivning | Kategori |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| ingen | Det finns ingen fördefinierad åtgärd för att åtgärda det här problemet, men i vissa fall kan det tyda på att API:t anropas felaktigt. | Åtgärda begärandekontexten. |
| konfiguration | Klientprogrammet kräver en konfigurationsändring, vilket oftast sker via Adobe Pass TVE Dashboard. | Korrigera integreringskonfigurationskontexten. |
| programregistrering | Klientprogrammet måste registrera sig igen. | Åtgärda klientprogramskontexten. |
| autentisering | Klientprogrammet kräver att användaren autentiseras eller autentiseras på nytt. | Åtgärda klientprogramskontexten. |
| auktorisation | Klientprogrammet kräver att få behörighet för den angivna resursen. | Åtgärda klientprogramskontexten. |
| försök igen | Klientprogrammet måste försöka utföra begäran igen. | Åtgärda begärandekontexten. |

_(*) För vissa fel kan flera åtgärder vara möjliga lösningar, men åtgärdsfältet anger den som har den största sannolikheten att åtgärda felet._

### Status {#enhanced-error-codes-representation-status}

Förbättrade felkoder innehåller ett statusfält som anger HTTP-statuskoden som är associerad med felet.

Möjliga värden för statusfältet är:

| Code | Orsaksfras |
|------|-----------------------|
| 400 | Felaktig begäran |
| 401 | Obehörig |
| 403 | Förbjuden |
| 404 | Hittades inte |
| 405 | Metoden tillåts inte |
| 410 | Borta |
| 412 | Förhandsvillkoret misslyckades |
| 500 | Internt serverfel |

Förbättrade felkoder med 4xx &quot;status&quot; visas vanligtvis när felet genereras av klienten och större delen av tiden det innebär att klienten behöver ytterligare arbete för att kunna åtgärda det.

Förbättrade felkoder med 5xx &quot;status&quot; visas vanligtvis när felet genereras av servern och oftast kräver servern mer arbete för att åtgärda felet.

>[!IMPORTANT]
>
> Det finns fall där HTTP-svarsstatuskoden skiljer sig från statusfältet för den utökade felkoden, särskilt när du interagerar med ett Adobe Pass Authentication API som kommunicerar utökade felkoder som felinformation på objektnivå.

### Code {#enhanced-error-codes-representation-code}

Förbättrade felkoder innehåller ett kodfält med en unik identifierare för Adobe Pass-autentiseringen som är associerad med felet.

Möjliga värden för kodfältet slås samman [nedan](#enhanced-error-codes-list) i två listor som baseras på det inbyggda Adobe Pass Authentication API:t.

## Listor {#enhanced-error-codes-lists}

### REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

Tabellen nedan visar möjliga felkoder som ett klientprogram kan stöta på när det är integrerat med Adobe Pass Authentication REST API v1.

| Åtgärd | Code | Status | Meddelande |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ingen** | *invalid_requested* | 400 | Parametern för begärande saknas eller är ogiltig. |
|                    | *invalid_device_info* | 400 | Enhetsinformationen saknas eller är ogiltig. |
|                    | *invalid_device_id* | 400 | Enhetsidentifieraren saknas eller är ogiltig. |
|                    | *missing_resource* | 400, 412 | Resursparametern saknas. |
|                    | *malformed_authz_request* | 400, 412 | Auktoriseringsbegäran är null eller ogiltig. |
|                    | *preauthentication_deny_by_mvpd* | 403 | MVPD har returnerat ett beslut om att neka vid begäran om förauktorisering för den angivna resursen. |
|                    | *authentication_deny_by_mvpd* | 403 | MVPD har returnerat ett beslut om att neka när den angivna resursen begärdes. |
|                    | *authentication_deny_by_parental_controls* | 403 | MVPD har returnerat ett beslut om att neka, på grund av inställningar för föräldrakontroll för den angivna resursen. |
|                    | *internal_error* | 400, 405, 500 | Begäran misslyckades på grund av ett internt serverfel. |
| **konfiguration** | *unknown_integration* | 400, 412 | Integrationen mellan den angivna programmeraren och identitetsleverantören finns inte. Använd TVE Dashboard för att skapa den nödvändiga integreringen. |
|                    | *för_många_resurser* | 403 | Begäran om auktorisering eller förauktorisering misslyckades eftersom för många resurser efterfrågades. Kontakta supportteamet för att konfigurera begränsningarna för auktorisering och förhandsauktorisering. |
| **autentisering** | *authentication_session_utfärdare_mismatch* | 400 | Auktoriseringsbegäran misslyckades på grund av att det angivna MVPD-värdet för auktoriseringsflödet skiljer sig från det som utfärdade autentiseringssessionen. Användaren måste autentisera på nytt med önskat MVPD för att kunna fortsätta. |
|                    | *authentication_deny_by_hba_policies* | 403 | MVPD har returnerat ett beslut om att neka, på grund av hembaserade autentiseringsprinciper. Den aktuella autentiseringen erhölls med ett hembaserat autentiseringsflöde (HBA), men enheten är inte längre hemma när den begär auktorisering för den angivna resursen. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                    | *authentication_deny_by_session_invalidated* | 403 | Autentiseringssessionen ogiltigförklarades av identitetsleverantören. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                    | *identity_not_recognized_by_mvpd* | 403 | Auktoriseringsbegäran misslyckades på grund av att användaridentiteten inte kändes igen av MVPD. |
|                    | *authentication_session_invalidated* | 403 | Autentiseringssessionen ogiltigförklarades av identitetsleverantören. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                    | *authentication_session_missing* | 403, 412 | Autentiseringssessionen som är associerad med denna begäran kunde inte hämtas. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                    | *authentication_session_utgången* | 403, 412 | Den aktuella autentiseringssessionen har gått ut. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                    | *preauthentication_authentication_session_missing* | 412 | Autentiseringssessionen som är associerad med denna begäran kunde inte hämtas. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                    | *preauthentication_authentication_session_utgången* | 412 | Den aktuella autentiseringssessionen har gått ut. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
| **auktorisering** | *authentication_not_found* | 403, 404 | Det gick inte att hitta någon auktorisering för den angivna resursen. Användaren måste skaffa en ny auktorisering för att kunna fortsätta. |
|                    | *authentication_utgången* | 410 | Den tidigare auktoriseringen för den angivna resursen har gått ut. Användaren måste skaffa en ny auktorisering för att kunna fortsätta. |
| **försök igen** | *network_receive_error* | 403 | Det uppstod ett läsfel när svaret skulle hämtas från den associerade partnertjänsten. Ett nytt försök att utföra begäran kan lösa problemet. |
|                    | *network_connection_timeout* | 403 | En timeout uppstod för anslutningen till den associerade partnertjänsten. Ett nytt försök att utföra begäran kan lösa problemet. |
|                    | *maximum_execution_time_pped* | 403 | Begäran slutfördes inte inom den tillåtna maxtiden. Ett nytt försök att utföra begäran kan lösa problemet. |

### SDK Förauktorisera API {#enhanced-error-codes-lists-sdks-preauthorize-api}

I föregående [avsnitt](#enhanced-error-codes-list-rest-api-v1) finns information om möjliga felkoder som ett klientprogram kan stöta på när det är integrerat med Adobe Pass Authentication SDKs PreAuthze API.

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

Tabellen nedan visar möjliga felkoder som ett klientprogram kan stöta på när det är integrerat med Adobe Pass Authentication REST API v2.

| Åtgärd | Code | Status | Meddelande |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ingen** | *invalid_parameter_service_provider* | 400 | Tjänstleverantörens parametervärde saknas eller är ogiltigt. |
|                              | *invalid_parameter_mvpd* | 400 | Parametervärdet för mvpd saknas eller är ogiltigt. |
|                              | *invalid_parameter_code* | 400 | Kodparametervärdet saknas eller är ogiltigt. |
|                              | *invalid_parameter_resources* | 400 | Resursparametervärdet saknas eller är ogiltigt. |
|                              | *invalid_parameter_redirect_url* | 400 | Parametervärdet för omdirigerings-URL saknas eller är ogiltigt. |
|                              | *invalid_parameter_partner* | 400 | Partnerparametervärdet saknas eller är ogiltigt. |
|                              | *invalid_parameter_saml_response* | 400 | SAML-svarsparametervärdet saknas eller är ogiltigt. |
|                              | *invalid_header_device_info* | 400 | Rubrikvärdet för enhetsinformation saknas eller är ogiltigt. |
|                              | *invalid_header_device_identifier* | 400 | Enhetsidentifierarens rubrikvärde saknas eller är ogiltigt. |
|                              | *invalid_header_identity_for_temporary_access* | 400 | Identiteten för rubrikvärdet för temporär åtkomst saknas eller är ogiltig. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | Statusvärdet för åtkomstbehörighet från statusrubriken för partnerramverket finns inte. |
|                              | *invalid_header_pfs_permission_access_not_defined* | 400 | Statusvärdet för åtkomstbehörighet från partnerramverkets statusrubrik är inte definierat. |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | Statusvärdet för åtkomstbehörighet från statusrubriken för partnerramverket har inte beviljats. |
|                              | *invalid_header_pfs_provider_id_not_defined* | 400 | Värdet för provider-ID från statusrubriken för partnerramverket är inte associerat med en känd mvpd. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | Värdet för provider-ID från statusrubriken för partnerramverket matchar inte det mvpd som skickades som parameter. |
|                              | *invalid_header_pfs_provider_info_utgången* | 400 | Providerinformationen från statusrubriken för partnerramverket har upphört att gälla. |
|                              | *invalid_integration* | 400 | Integrationen mellan den angivna tjänstprovidern och mvpd finns inte eller är inaktiverad. |
|                              | *invalid_authentication_session* | 400 | Autentiseringssessionen som är associerad med denna begäran saknas eller är ogiltig. |
|                              | *preauthentication_deny_by_mvpd* | 403 | MVPD har returnerat ett beslut om att neka vid begäran om förauktorisering för den angivna resursen. |
|                              | *authentication_deny_by_mvpd* | 403 | MVPD har returnerat ett beslut om att neka när den angivna resursen begärdes. |
|                              | *authentication_deny_by_parental_controls* | 403 | MVPD har returnerat ett beslut om att neka, på grund av inställningar för föräldrakontroll för den angivna resursen. |
|                              | *permission_deny_by_Nedbrytningsregel* | 403 | Integrationen mellan den angivna tjänstleverantören och mvpd har en distributionsregel som inte tillåter auktorisering för de begärda resurserna. |
|                              | *internal_server_error* | 500 | Begäran misslyckades på grund av ett internt serverfel. |
| **konfiguration** | *för_många_resurser* | 403 | Begäran om auktorisering eller förauktorisering misslyckades eftersom för många resurser efterfrågades. Kontakta supportteamet för att konfigurera begränsningarna för auktorisering och förhandsauktorisering. |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | Certifikatkonfigurationen för användarens metadata saknas eller är ogiltig. |
|                              | *invalid_configuration_temporary_access* | 500 | Konfigurationen för temporär åtkomst är ogiltig. |
|                              | *invalid_configuration_platform* | 500 | Plattformskonfigurationen saknas eller är ogiltig för integrering. |
|                              | *invalid_configuration_platform_id* | 500 | Konfigurationen av plattforms-ID saknas eller är ogiltig. |
|                              | *invalid_configuration_platform_trait* | 500 | Konfigurationen av plattformens egenskaper saknas eller är ogiltig. |
|                              | *invalid_configuration_platform_category_trait* | 500 | Konfigurationen av plattformskategorins egenskaper saknas eller är ogiltig. |
|                              | *invalid_configuration_platform_services* | 500 | Konfigurationen för plattformstjänster saknas eller är ogiltig för integrering. |
|                              | *invalid_configuration_mvpd_platform* | 500 | Konfigurationen av mvpd-plattformen saknas eller är ogiltig för mvpd och plattform. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | Statuskonfigurationen för mvpd-plattformens boarding saknas eller är ogiltig för mvpd och platform. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | Konfigurationen för profilutbyte för mvpd-plattformen saknas eller är ogiltig för mvpd och plattform. |
| **programregistrering** | *invalid_access_token_service_provider* | 401 | Åtkomsttoken är ogiltig på grund av ogiltig tjänstleverantör. |
|                              | *invalid_access_token_client_application* | 401 | Åtkomsttoken är ogiltig på grund av ett ogiltigt klientprogram. |
| **autentisering** | *authenticated_profile_missing* | 403 | Den autentiserade profil som är associerad med denna begäran saknas. |
|                              | *authenticated_profile_utgången* | 403 | Den autentiserade profil som är associerad med denna begäran har gått ut. |
|                              | *authenticated_profile_invalidated* | 403 | Den autentiserade profil som är associerad med denna begäran har ogiltigförklarats. |
|                              | *temporary_access_duration_limit_Over* | 403 | Tidsgränsen för temporär åtkomst har överskridits. |
|                              | *temporary_access_resources_limit_Over* | 403 | Resursgränsen för temporär åtkomst har överskridits. |
|                              | *authentication_deny_by_hba_policies* | 403 | MVPD har returnerat ett beslut om att neka, på grund av hembaserade autentiseringsprinciper. Den aktuella autentiseringen erhölls via ett hembaserat autentiseringsflöde, men enheten är inte längre hemma när den begär auktorisering för den angivna resursen. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                              | *authentication_deny_by_session_invalidated* | 403 | Autentiseringssessionen ogiltigförklarades av identitetsleverantören. Användaren måste autentisera på nytt med en MVPD som stöds för att kunna fortsätta. |
|                              | *identity_not_recognized_by_mvpd* | 403 | Auktoriseringsbegäran misslyckades på grund av att användaridentiteten inte kändes igen av MVPD. |
| **försök igen** | *network_receive_error* | 403 | Det uppstod ett läsfel när svaret skulle hämtas från den associerade partnertjänsten. Ett nytt försök att utföra begäran kan lösa problemet. |
|                              | *network_connection_timeout* | 403 | En timeout uppstod för anslutningen till den associerade partnertjänsten. Ett nytt försök att utföra begäran kan lösa problemet. |
|                              | *maximum_execution_time_pped* | 403 | Begäran slutfördes inte inom den tillåtna maxtiden. Ett nytt försök att utföra begäran kan lösa problemet. |

## Svarshantering {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Det finns utökade felkoder som kan hanteras automatiskt i klientprogramkoden, till exempel att försöka göra om en auktoriseringsbegäran om nätverkstimeout skulle inträffa, eller kräva att användaren autentiserar sig när sessionen har gått ut, men andra typer kan kräva konfigurationsändringar eller att kundtjänstteamet interagerar med Adobe Pass Authentication.
>
> <br/>
>
> Därför är det viktigt att samla in och tillhandahålla fullständig felinformation när du skapar en biljett via [Zendesk](https://adobeprimetime.zendesk.com) för att se till att nödvändiga ändringar görs innan du startar det nya programmet eller den nya funktionen.

Sammanfattningsvis bör du tänka på följande när du hanterar svar som innehåller förbättrade felkoder:

1. **Kontrollera båda statusvärdena**: Kontrollera alltid både HTTP-svarets statuskod och det utökade felkodens statusfält. De kan skilja sig åt och båda ge värdefull information.

1. **Felinformation på högsta nivån kontra objektnivå**: Hantera felinformation på högsta nivån och på objektnivå oavsett hur den kommuniceras, se till att du kan hantera båda typerna av överföring av utökade felkoder.

1. **Återförsökslogik**: För fel som kräver ett nytt försök måste du se till att försök görs med exponentiell säkerhetskopiering för att undvika att servern överbelastas. Om det gäller API:er för Adobe Pass-autentisering som hanterar flera objekt samtidigt (t.ex. API för förauktorisering), bör du i den upprepade begäran endast inkludera de objekt som är markerade med&quot;återtry&quot; och inte hela listan.

1. **Konfigurationsändringar**: För fel som kräver konfigurationsändringar måste du se till att nödvändiga ändringar görs innan du startar det nya programmet eller den nya funktionen.

1. **Autentisering och auktorisering**: För fel som rör autentisering och auktorisering måste du uppmana användaren att autentisera igen eller skaffa ny auktorisering efter behov.

1. **Användarfeedback**: Om du vill kan du använda de läsbara fälten &quot;message&quot; och (potentiella) &quot;details&quot; för att informera användaren om problemet. Textmeddelandet &quot;details&quot; kan skickas från MVPD-slutpunkterna för förauktorisering eller auktorisering eller från Programmeraren när reglerna för nedbrytning tillämpas.
