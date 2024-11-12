---
title: Apple SSO Cookbook (REST API V2)
description: Apple SSO Cookbook (REST API V2)
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '3402'
ht-degree: 0%

---

# Apple SSO Cookbook (REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Adobe Pass Authentication REST API V2 har stöd för enkel inloggning (SSO) för slutanvändare av klientprogram som körs på iOS, iPadOS eller tvOS.

Det här dokumentet fungerar som ett tillägg till den befintliga REST API V2-dokumentationen som beskriver hur du implementerar [enkel inloggning med partnerflöden](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## Apple enkel inloggning med partnerflöden {#apple-sso-cookbook-rest-api-v2-cookbook}

### Förutsättningar {#apple-sso-cookbook-rest-api-v2-prerequisites}

Innan du fortsätter med Apple enkel inloggning med partnerflöden måste du kontrollera att följande krav är uppfyllda:

* Strömningsprogrammet måste samla in alla nödvändiga data som krävs av rubrikerna `X-Device-Info` och/eller `User-Agent` så att Adobe Pass Authentication-backend kan identifiera enhetsplattformen och dess funktioner. Mer information om rubriken `X-Device-Info` finns i [X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) -dokumentationen.

* Strömningsprogrammet måste begära åtkomst till användarens prenumerationsinformation som sparats på enhetsnivå, och användaren måste ge programmet behörighet att fortsätta, på samma sätt som att ge åtkomst till enhetens kamera eller mikrofon. Den här behörigheten måste begäras per program med Apple [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) och enheten sparar användarens val.

  Vi rekommenderar att du uppmuntrar användare som vägrar ge åtkomst till prenumerationsinformation genom att förklara fördelarna med Apple användarupplevelse med enkel inloggning, men tänk på att användaren kan ändra sitt beslut genom att gå till programinställningarna (behörighet för TV-leverantör) eller till *`Settings -> TV Provider`* på iOS och iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS.

  Strömningsprogrammet kan begära användarens tillstånd när programmet försätts i förgrundstillstånd, eftersom programmet när som helst kan kontrollera [behörighet att komma åt ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation innan användarautentisering krävs.

>[!IMPORTANT]
>
> Antaganden
>
> <br/>
>
> * Strömningsprogrammet har slutfört de [startkrav](/help/authentication/single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-overview.md#apple-sso-prerequisites-programmer) som gäller för en programmerare och som krävs för att aktivera Apple Single Sign-On-användarupplevelsen.

### Arbetsflöde {#apple-sso-cookbook-rest-api-v2-workflow}

Utför de angivna stegen för att implementera enkel inloggning från Apple med partnerflöden enligt bilden nedan.

![Apple enkel inloggning med partnerflöden](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Apple enkel inloggning med partnerflöden*

+++A. Registreringsfas

1. **Hämta klientautentiseringsuppgifter:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta klientautentiseringsuppgifter genom att anropa slutpunkten för klientregistret.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta klientautentiseringsuppgifter](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API-dokumentationen:
   >
   > * Alla _obligatoriska_-parametrar, som `software_statement`
   > * Alla _obligatoriska_ rubriker, som `Content-Type`, `X-Device-Info`
   > * Alla _valfria_ parametrar och rubriker

1. **Returnera klientautentiseringsuppgifter:** Svaret på klientregisterslutpunkten innehåller information om klientautentiseringsuppgifterna som är associerade med de mottagna parametrarna och rubrikerna.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett svar på klientautentiseringsuppgifter finns i [Hämta klientautentiseringsuppgifter](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API-dokumentationen.
   >
   > <br/>
   >
   > Klientregistret validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer [Hämta klientautentiseringsuppgifter](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API-dokumentationen.

   >[!TIP]
   >
   > Förslag: Klientens autentiseringsuppgifter måste cachelagras och kan användas på obestämd tid.

1. **Hämta åtkomsttoken:** Direktuppspelningsprogrammet samlar in alla data som behövs för att hämta åtkomsttoken genom att anropa klienttokenslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta åtkomsttoken](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API-dokumentationen:
   >
   > * Alla _obligatoriska_-parametrar, som `client_id`, `client_secret` och `grant_type`
   > * Alla _obligatoriska_ rubriker, som `Content-Type`, `X-Device-Info`
   > * Alla _valfria_ parametrar och rubriker

1. **Returåtkomsttoken:** Slutpunktssvaret för klienttoken innehåller information om åtkomsttoken som är associerad med de mottagna parametrarna och rubrikerna.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett åtkomsttoken-svar finns i [Hämta åtkomsttoken](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API-dokumentationen.
   >
   > <br/>
   >
   > Klienttoken validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer [Hämta åtkomsttoken](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API-dokumentationen.

   >[!TIP]
   >
   > Förslag: Åtkomsttoken måste cachelagras och användas endast inom den angivna varaktigheten (t.ex. 24 timmars time-to-live). När direktuppspelningsprogrammet har upphört att gälla måste det begära en ny åtkomsttoken.

+++

+++B. Kontrollera autentiseringsfas

1. **Hämta partnerramverkets status:** Direktuppspelningsprogrammet anropar [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) som utvecklats av Apple för att få användarbehörighet och providerinformation.

   >[!IMPORTANT]
   >
   > Mer information finns i dokumentationen för [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount):
   >
   > <br/>
   >
   > * Direktuppspelningsprogrammet måste kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
   > * Strömningsprogrammet måste tillhandahålla en [delegat](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) för `VSAccountManager`.
   > * Strömningsprogrammet måste skicka en [begäran](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation.
   > * Direktuppspelningsprogrammet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det anger ett booleskt värde som är lika med `false` för egenskapen [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) i objektet `VSAccountMetadataRequest`, vilket anger att användaren inte kan avbrytas i den här fasen.

1. **Returnera statusinformation för partnerramverket:** Direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande villkor uppfylls:
   * Åtkomststatus för användarbehörighet beviljas.
   * Mappningsidentifieraren för användarprovidern finns och är giltig.
   * Användarleverantörsprofilens förfallodatum (om tillgängligt) är giltigt.

1. **Hämta profiler:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta all profilinformation genom att skicka en begäran till profilslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta profiler](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API-dokumentationen:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier` och `AP-Partner-Framework-Status`
   > * Alla _valfria_ parametrar och rubriker
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status så att det hämtade svaret kan innehålla en AppleSSO-typprofil.
   >
   > <br/>
   >
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Returnera information om hittade profiler:** Profilernas slutpunktssvar innehåller information om de hittade profiler som är associerade med de mottagna parametrarna och rubrikerna.

1. **Välj en profil och fortsätt med beslutsflöden:** Om slutpunktssvaret för profiler innehåller profiler använder direktuppspelningsprogrammet sin interna logik (genom att till slut interagera med slutanvändaren) för att välja en av de tillgängliga profilerna för att fortsätta med efterföljande beslutsflöden.

1. **Fortsätt med partnerautentiseringsflödet:** Om slutpunktssvaret för profiler inte innehåller någon profil fortsätter direktuppspelningsprogrammet med partnerautentiseringsflödet.

+++

+++C. Autentiseringsfas för partner

1. **Hämta konfiguration:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta listan över MVPD-filer som har en aktiv integrering genom att skicka en begäran till Configuration-slutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta konfiguration för specifik API-dokumentation för tjänstprovidern](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier` och `X-Device-Info`
   > * Alla _valfria_ parametrar och rubriker

1. **Returkonfiguration:** Konfigurationsslutpunktssvaret innehåller information om de MVPD-filer som har en aktiv integrering med tjänstprovidern.

   >[!IMPORTANT]
   >
   > Mer information om informationen som tillhandahålls i ett konfigurationssvar finns i [Hämta konfiguration för specifik API-dokumentation för tjänstleverantör](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response).
   >
   > <br/>
   >
   > Konfigurationsslutpunkten validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer de [förbättrade felkoderna](/help/authentication/enhanced-error-codes.md)

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att följande information för varje MVPD-fil bearbetas när du fortsätter:
   >
   > * `enablePlatformServices`: Anger om MVPD för närvarande stöder enkel inloggning från Apple.
   > * `displayInPlatformPicker`: Anger om MVPD kan visas i Apple-väljaren.
   > * `boardingStatus`: Anger om MVPD har registrerats i Apple Single Sign-on.

1. **Hämta partnerramverkets status:** Direktuppspelningsprogrammet anropar [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) som utvecklats av Apple för att få användarbehörighet och providerinformation.

   >[!IMPORTANT]
   >
   > Mer information finns i dokumentationen för [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount):
   >
   > <br/>
   >
   > * Direktuppspelningsprogrammet måste kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
   > * Strömningsprogrammet måste tillhandahålla en [delegat](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) för `VSAccountManager`.
   > * Strömningsprogrammet måste skicka en [begäran](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation.
   > * Direktuppspelningsprogrammet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det anger ett booleskt värde som är lika med `true` för egenskapen [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) i objektet `VSAccountMetadataRequest`, vilket anger att användaren kan avbrytas för att välja TV-leverantör i den här fasen.

1. **Returnera statusinformation för partnerramverket:** Direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande villkor uppfylls:
   * Åtkomststatus för användarbehörighet beviljas.
   * Mappningsidentifieraren för användarprovidern finns och är giltig.
   * Användarleverantörsprofilens förfallodatum (om tillgängligt) är giltigt.

1. **Hämta partnerautentiseringsbegäran:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att initiera en autentiseringssession genom att anropa Sessions-partnerslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta API-dokumentationen för partnerautentiseringsbegäran](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider` och `partner`
   > * Alla _obligatoriska_ rubriker som `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` och `AP-Partner-Framework-Status`
   > * Alla _valfria_ rubriker och parametrar
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status så att det hämtade svaret kan innehålla en begäran om partnerautentisering (SAML-begäran).
   >
   > <br/>
   >
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Ange nästa åtgärd:** Sessions-partnerns slutpunktssvar innehåller de data som krävs för att vägleda direktuppspelningsprogrammet angående nästa åtgärd.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som finns i ett sessionssvar finns i [Hämta API-dokumentationen för partnerautentiseringsbegäran](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response).
   >
   > <br/>
   >
   > Slutpunkten för Sessions-partnern validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   >
   > Om den grundläggande valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Slutpunkten för Sessions-partnern verifierar data för begäran för att säkerställa att villkoren för enkel inloggning för partner uppfylls:
   >
   >  * Partnerkonfigurationen för enkel inloggning på Adobe Pass-servern måste vara giltig och aktiverad.
   >  * Statusnyttolasten för partnerramverket som togs emot via rubriken [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) måste vara giltig.
   >
   > <br/>
   >
   > Om partnervalideringen för enkel inloggning misslyckas, används standardvärdet för det grundläggande autentiseringsflödet.

1. **Fortsätt med beslutsflöden:** Sessionspartnerns slutpunktssvar innehåller följande data:
   * Attributet `actionName` är inställt på&quot;auktorisera&quot;.
   * Attributet `actionType` är inställt på&quot;direct&quot;.

   Om Adobe Pass serverdel identifierar en giltig profil behöver direktuppspelningsprogrammet inte autentisera igen med det valda MVPD-programmet, eftersom det redan finns en profil som kan användas för efterföljande beslutsflöden.

1. **Fortsätt med grundläggande autentiseringsflöde:** Sessions-partnerns slutpunktssvar innehåller följande data:
   * Attributet `actionName` är inställt på antingen &quot;authenticate&quot; eller &quot;resume&quot;.
   * Attributet `actionType` är inställt på antingen &quot;interaktiv&quot; eller &quot;direct&quot;.

   Om Adobe Pass serverdel inte kan identifiera en giltig profil och partnerverifieringen för enkel inloggning misslyckas, återgår Adobe Pass-servern till det grundläggande autentiseringsflödet.

   Mer information om det grundläggande autentiseringsflödet finns i följande dokument:
   * [Utför autentisering i det primära programmet](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Utför autentisering i det sekundära programmet med förvald mvpd](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Utför autentisering i det sekundära programmet utan förvald mvpd](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Fortsätt med att hämta profil med hjälp av svarsflöde för partnerautentisering:** Sessionernas partnersvar innehåller följande data:
   * Attributet `actionName` är inställt på &quot;partner_profile&quot;.
   * Attributet `actionType` är inställt på&quot;direct&quot;.
   * Attributet `authenticationRequest - type` innehåller säkerhetsprotokollet som används av partnerramverket för MVPD-inloggning (som för närvarande endast är inställt på SAML).
   * Attributet `authenticationRequest - request` innehåller SAML-begäran som skickas till partnerramverket.
   * Attributet `authenticationRequest - attributesNames` innehåller de SAML-attribut som skickas till partnerramverket.

   Om Adobe Pass-serverdelen inte identifierar en giltig profil och partnervalideringen för enkel inloggning godkänns, får direktuppspelningsprogrammet ett svar med åtgärder och data som skickas till partnerramverket för att starta autentiseringsflödet med MVPD.

1. **Fullständig MVPD-autentisering med partnerramverket:** Vidarebefordra partnerautentiseringsbegäran (SAML-begäran) som hämtades i föregående steg till [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount). Om autentiseringsflödet lyckas skapar [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)-interaktionen med MVPD ett partnerautentiseringssvar (SAML-svar) som returneras tillsammans med partnerramverkets statusinformation.

   >[!IMPORTANT]
   >
   > Mer information finns i dokumentationen för [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount):
   >
   > <br/>
   >
   > * Direktuppspelningsprogrammet måste kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
   > * Strömningsprogrammet måste tillhandahålla en [delegat](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) för `VSAccountManager`.
   > * Strömningsprogrammet måste skicka en [begäran](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation och måste innehålla partnerautentiseringsbegäran (SAML-begäran) som hämtades i föregående steg.
   > * Direktuppspelningsprogrammet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det anger ett booleskt värde som är lika med `true` för egenskapen [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) i objektet `VSAccountMetadataRequest`, vilket anger att användaren kan avbrytas för att autentisera med den valda TV-leverantören i den här fasen.

1. **Retursvar på partnerautentisering:** Direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande villkor uppfylls:
   * Åtkomststatus för användarbehörighet beviljas.
   * Mappningsidentifieraren för användarprovidern finns och är giltig.
   * Användarleverantörsprofilens förfallodatum (om tillgängligt) är giltigt.
   * SAML-svar (Partner Authentication Response) finns och är giltigt.

1. **Hämta profil med partnerautentiseringssvar:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att skapa och hämta en profil genom att anropa slutpunkten för profilpartnern.

   >[!IMPORTANT]
   >
   > Mer information om hur du gör det finns i [Hämta profil med API-dokumentationen för partnerautentiseringssvar](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `partner` och `SAMLResponse`
   > * Alla _obligatoriska_-huvuden, som `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` och `AP-Partner-Framework-Status`
   > * Alla _valfria_ rubriker och parametrar
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det innehåller giltiga värden för partnerramverkets status och partnerautentiseringssvaret (SAML-svar) så att det hämtade svaret kan innehålla en AppleSSO-typprofil.
   >
   > <br/>
   >
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Returinformation om partnerprofil:** Profilernas slutpunktssvar innehåller information om partnerprofilen, inklusive attributet `type` som är inställt på &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett profilsvar finns i [Hämta profil med API-dokumentationen för partnerautentiseringssvar](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response).
   >
   > <br/>
   >
   > Profiles Partner-slutpunkten verifierar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Profiles Partner-slutpunkten verifierar data för begäran för att säkerställa att villkoren för enkel inloggning för partner uppfylls:
   >
   >  * Partnerkonfigurationen för enkel inloggning på Adobe Pass-servern måste vara giltig och aktiverad.
   >  * Statusnyttolasten för partnerramverket som togs emot via rubriken [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) måste vara giltig.
   >
   > <br/>
   >
   > Om valideringen av enkel inloggning från partner misslyckas, används standardvärdet för det grundläggande flödet för hämtning av profiler.

1. **Fortsätt med beslutsflöden:** Direktuppspelningsprogrammet kan fortsätta med efterföljande beslutsflöden.

+++

+++ D. Beslutsfasen

1. **Hämta partnerramverkets status:** Direktuppspelningsprogrammet anropar [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) som utvecklats av Apple för att få användarbehörighet och providerinformation.

   >[!IMPORTANT]
   >
   > Mer information finns i dokumentationen för [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount):
   >
   > <br/>
   >
   > * Direktuppspelningsprogrammet måste kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
   > * Strömningsprogrammet måste tillhandahålla en [delegat](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) för `VSAccountManager`.
   > * Strömningsprogrammet måste skicka en [begäran](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation.
   > * Direktuppspelningsprogrammet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det anger ett booleskt värde som är lika med `false` för egenskapen [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) i objektet `VSAccountMetadataRequest`, vilket anger att användaren inte kan avbrytas i den här fasen.

   >[!TIP]
   > 
   > Förslag: Direktuppspelningsprogrammet kan använda ett cachelagrat värde för statusinformationen för partnerramverket, som vi rekommenderar att du uppdaterar när programmet övergår från bakgrund till förgrundstillstånd.

1. **Returnera statusinformation för partnerramverket:** Direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande villkor uppfylls:
   * Åtkomststatus för användarbehörighet beviljas.
   * Mappningsidentifieraren för användarprovidern finns och är giltig.
   * Användarleverantörsprofilens förfallodatum (om tillgängligt) är giltigt.

1. **Hämta förauktoriseringsbeslut:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att erhålla förauktoriseringsbeslut för en lista med resurser genom att anropa slutpunkten för förauktorisering av beslut.

   >[!IMPORTANT]
   >
   > Se [Hämta beslut om förhandsauktorisering med hjälp av specifik mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) API-dokumentation för mer information om:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `resources`
   > * Alla _obligatoriska_ rubriker, som `Authorization` och `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker
   >
   > <br/>
   >
   > Direktuppspelningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status innan en begäran görs vidare, när den valda profilen är en AppleSSO-typprofil.
   >
   > <br/>
   >
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Returbeslut om förauktorisering:** Slutpunktssvaret för förauktorisering av beslut innehåller ett `Permit` - eller `Deny`-beslut för varje resurs:
   * Ett `Permit`-beslut betyder att resursen kan spelas upp. Svaret innehåller ingen medietoken eftersom förauktoriseringsflödet inte får användas för att spela upp resurser.
   * Ett `Deny`-beslut betyder att resursen inte kan spelas upp. Svaret innehåller en felnyttolast som följer dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett beslutssvar finns i [Hämta förhandsauktoriseringsbeslut med hjälp av specifik mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) API-dokumentation.
   >
   > <br/>
   >
   > Slutpunkten för förauktorisering av beslut validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md).

1. **Hämta partnerramverkets status:** Direktuppspelningsprogrammet anropar [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) som utvecklats av Apple för att få användarbehörighet och providerinformation.

   >[!IMPORTANT]
   >
   > Mer information finns i dokumentationen för [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount):
   >
   > <br/>
   >
   > * Direktuppspelningsprogrammet måste kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
   > * Strömningsprogrammet måste tillhandahålla en [delegat](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) för `VSAccountManager`.
   > * Strömningsprogrammet måste skicka en [begäran](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation.
   > * Direktuppspelningsprogrammet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det anger ett booleskt värde som är lika med `false` för egenskapen [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) i objektet `VSAccountMetadataRequest`, vilket anger att användaren inte kan avbrytas i den här fasen.

   >[!TIP]
   >
   > Förslag: Direktuppspelningsprogrammet kan använda ett cachelagrat värde för statusinformationen för partnerramverket, som vi rekommenderar att du uppdaterar när programmet övergår från bakgrund till förgrundstillstånd.

1. **Returnera statusinformation för partnerramverket:** Direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande villkor uppfylls:
   * Åtkomststatus för användarbehörighet beviljas.
   * Mappningsidentifieraren för användarprovidern finns och är giltig.
   * Användarleverantörsprofilens förfallodatum (om tillgängligt) är giltigt.

1. **Hämta auktoriseringsbeslut:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att erhålla ett auktoriseringsbeslut för en specifik resurs genom att anropa auktoriseringsslutpunkten för beslut.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta auktoriseringsbeslut med hjälp av specifik API-dokumentation för mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `resources`
   > * Alla _obligatoriska_ rubriker, som `Authorization` och `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker
   >
   > <br/>
   >
   > Direktuppspelningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status innan en begäran görs vidare, när den valda profilen är en AppleSSO-typprofil.
   >
   > <br/>
   >
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Beslut om returauktorisering:** Slutpunktssvaret för auktorisering av beslut innehåller ett `Permit` - eller `Deny`-beslut för den specifika resursen:
   * Ett `Permit`-beslut betyder att resursen kan spelas upp. Svaret innehåller en medietoken.
   * Ett `Deny`-beslut betyder att resursen inte kan spelas upp. Svaret innehåller en felnyttolast som följer dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett beslutssvar finns i [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) API-dokumentation.
   >
   > <br/>
   >
   > Slutpunkten för beslutsauktorisering validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md).

+++

+++ D. Utloggningsfas

1. **Initiera Adobe Pass-utloggning:** Direktuppspelningsprogrammet samlar in alla data som behövs för att initiera utloggningsflödet genom att anropa Adobe Pass utloggningsslutpunkt.

   >[!IMPORTANT]
   >
   > Mer information om hur du använder API-dokumentationen för mvpd](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) finns i [Initiera utloggning:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `redirectUrl`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker

1. **Ange nästa åtgärd:** Slutpunktssvaret för Adobe Pass-utloggningen innehåller de data som krävs för att vägleda direktuppspelningsprogrammet angående nästa åtgärd.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett utloggningssvar finns i [Initiera utloggning för specifik mvpd](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) API-dokumentation.
   >
   > <br/>
   >
   > Adobe Pass utloggningsslutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   >
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](/help/authentication/enhanced-error-codes.md).

   >[!IMPORTANT]
   > 
   > Strömningsprogrammet måste se till att det anger för användaren att fortsätta logga ut från partnernivå.

+++