---
title: Enkel inloggning - partner - flöden
description: REST API V2 - enkel inloggning - partner - flöden
exl-id: 5735d67f-a311-4d03-ad48-93c0fcbcace5
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Samlad inloggning med partnerflöden {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Gå även till [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

Med partnermetoden kan flera program använda en statusnyttolast för partnerramverket för att uppnå enkel inloggning (SSO) på enhetsnivå när Adobe Pass-tjänster används.

Programmen ansvarar för att hämta partnerramverkets statusnyttolast med partnerspecifika ramverk eller bibliotek utanför Adobe Pass system.

Programmen ansvarar för att inkludera den här partnerramverkets statusnyttolast som en del av `AP-Partner-Framework-Status`-huvudet för alla begäranden som anger den.

Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

Adobe Pass Authentication REST API V2 har stöd för enkel inloggning (SSO) för slutanvändare av klientprogram som körs på iOS, iPadOS eller tvOS.

Mer information om enkel inloggning (SSO) för Apple-plattformen finns i [Apple SSO Cookbook (REST API V2)](/help/premium-workflow/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md) -dokumentationen.

## Hämta partnerautentiseringsbegäran {#retrieve-partner-authentication-request}

### Förutsättningar {#prerequisites-retrieve-partner-authentication-request}

Innan du hämtar partnerautentiseringsbegäran ska du kontrollera att följande krav är uppfyllda:

* Partnerramverket måste välja en MVPD.
* Direktuppspelningsprogrammet måste få statusinformation om partnerramverket från partnerramverket och skicka det till Adobe Pass-servern.
* Strömningsprogrammet måste hämta partnerautentiseringsbegäran från Adobe Pass-servern och skicka den till partnerramverket.

>[!IMPORTANT]
>
> Antaganden
> 
> <br/>
> 
> * Partnerramverket stöder användarinteraktion för att välja en MVPD.
> * Partnerramverket stöder användarinteraktion för autentisering med den valda MVPD.
> * Partnerramverket tillhandahåller användarbehörighet och providerinformation.

### Arbetsflöde {#workflow-retrieve-partner-authentication-request}

Utför de angivna stegen för att hämta partnerautentiseringsbegäran enligt bilden nedan.

![Hämta partnerautentiseringsförfrågan](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*Hämta partnerautentiseringsförfrågan*

1. **Hämta partnerramverkets status:** Direktuppspelningsprogrammet anropar partnerramverket utanför Adobe Pass system för att få användarbehörighet och providerinformation.

1. **Returnera statusinformation för partnerramverket:** Direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande villkor uppfylls:
   * Åtkomststatus för användarbehörighet beviljas.
   * Mappningsidentifieraren för användarprovidern finns och är giltig.
   * Användarleverantörsprofilens förfallodatum (om tillgängligt) är giltigt.

1. **Hämta partnerautentiseringsbegäran:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att initiera en autentiseringssession genom att anropa Sessions-partnerslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta API-dokumentationen för partnerautentiseringsbegäran](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider` och `partner`
   > * Alla _obligatoriska_ rubriker som `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` och `AP-Partner-Framework-Status`
   > * Alla _valfria_ rubriker och parametrar
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Ange nästa åtgärd:** Sessions-partnerns slutpunktssvar innehåller de data som krävs för att vägleda direktuppspelningsprogrammet angående nästa åtgärd.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som finns i ett sessionssvar finns i [Hämta API-dokumentationen för partnerautentiseringsbegäran](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md).
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
   > Om den grundläggande valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Slutpunkten för Sessions-partnern verifierar data för begäran för att säkerställa att villkoren för enkel inloggning för partner uppfylls:
   >
   >  * Partnerkonfigurationen för enkel inloggning på Adobe Pass-servern måste vara giltig och aktiverad.
   >  * Statusnyttolasten för partnerramverket som togs emot via rubriken [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) måste vara giltig.
   >
   > <br/>
   >
   > Om partnervalideringen för enkel inloggning misslyckas, används standardvärdet för det grundläggande autentiseringsflödet.

1. **Fortsätt med profilhämtningsflödet med partnerautentiseringssvar:** Sessionernas partnersvar innehåller följande data:
   * Attributet `actionName` är inställt på &quot;partner_profile&quot;.
   * Attributet `actionType` är inställt på&quot;direct&quot;.
   * Attributet `authenticationRequest - type` innehåller säkerhetsprotokollet som används av partnerramverket för MVPD-inloggning (som för närvarande endast är inställt på SAML).
   * Attributet `authenticationRequest - request` innehåller SAML-begäran som skickas till partnerramverket.
   * Attributet `authenticationRequest - attributesNames` innehåller de SAML-attribut som skickas till partnerramverket.

   Om Adobe Pass-serverdelen inte identifierar en giltig profil och partnervalideringen för enkel inloggning godkänns, får direktuppspelningsprogrammet ett svar med åtgärder och data som skickas till partnerramverket för att starta autentiseringsflödet med MVPD.

   Mer information om hur du hämtar profiler med hjälp av ett partnerautentiseringssvar finns i avsnittet [Skapa och hämta profil med partnerautentiseringssvar](#create-and-retrieve-profile-using-partner-authentication-response).

1. **Fortsätt med grundläggande autentiseringsflöde:** Sessions-partnerns slutpunktssvar innehåller följande data:
   * Attributet `actionName` är inställt på antingen &quot;authenticate&quot; eller &quot;resume&quot;.
   * Attributet `actionType` är inställt på antingen &quot;interaktiv&quot; eller &quot;direct&quot;.

   Om Adobe Pass serverdel inte kan identifiera en giltig profil och partnerverifieringen för enkel inloggning misslyckas, återgår Adobe Pass-servern till det grundläggande autentiseringsflödet.

   Mer information om det grundläggande autentiseringsflödet finns i följande dokument:
   * [Utför autentisering i det primära programmet](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Utför autentisering i det sekundära programmet med förvald mvpd](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Utför autentisering i det sekundära programmet utan förvald mvpd](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Fortsätt med beslutsflöden:** Sessionspartnerns slutpunktssvar innehåller följande data:
   * Attributet `actionName` är inställt på&quot;auktorisera&quot;.
   * Attributet `actionType` är inställt på&quot;direct&quot;.

   Om Adobe Pass serverdel identifierar en giltig profil behöver direktuppspelningsprogrammet inte autentisera igen med den valda MVPD eftersom det redan finns en profil som kan användas för efterföljande beslutsflöden.

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

## Skapa och hämta profil med partnerautentiseringssvar {#create-and-retrieve-profile-using-partner-authentication-response}

### Förutsättningar {#prerequisites-create-and-retrieve-profile-using-partner-authentication-response}

Innan du hämtar profilen med ett partnerautentiseringssvar måste du kontrollera att följande krav är uppfyllda:

* Partnerramverket måste utföra autentisering med den valda MVPD.
* Direktuppspelningsprogrammet måste få partnerautentiseringssvaret tillsammans med partnerramverkets statusinformation från partnerramverket och skicka det till Adobe Pass-servern.

>[!IMPORTANT]
>
> Förutsättning
>
> * Partnerramverket stöder användarinteraktion för att välja en MVPD.
> * Partnerramverket stöder användarinteraktion för autentisering med den valda MVPD.
> * Partnerramverket tillhandahåller användarbehörighet och providerinformation.

### Arbetsflöde {#workflow-create-and-retrieve-profile-using-partner-authentication-response}

Utför de angivna stegen för att implementera flödet för hämtning av profiler med hjälp av ett partnerautentiseringssvar, vilket visas i följande diagram.

![Skapa och hämta profil med partnerautentiseringssvaret](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*Skapa och hämta autentiserad profil med partnerautentiseringssvaret*

1. **Fullständig MVPD-autentisering med partnerramverket:** Om autentiseringsflödet lyckas skapar partnerramverkets interaktion med MVPD ett partnerautentiseringssvar (SAML-svar) som returneras tillsammans med partnerramverkets statusinformation.

1. **Retursvar på partnerautentisering:** Direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande villkor uppfylls:
   * Åtkomststatus för användarbehörighet beviljas.
   * Mappningsidentifieraren för användarprovidern finns och är giltig.
   * Användarleverantörsprofilens förfallodatum (om tillgängligt) är giltigt.

1. **Skapa och hämta profil med partnerautentiseringssvar:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att skapa och hämta en profil genom att anropa slutpunkten för Profiles Partner.

   >[!IMPORTANT]
   >
   > Mer information om hur du gör det finns i [Skapa och hämta profil med API-dokumentationen för partnerautentiseringssvar](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `partner` och `SAMLResponse`
   > * Alla _obligatoriska_-huvuden, som `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` och `AP-Partner-Framework-Status`
   > * Alla _valfria_ rubriker och parametrar
   >
   > <br/>
   > 
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Skapa och spara partnerprofil:** Adobe Pass-servern skapar och sparar en partnerprofil efter att alla villkor är uppfyllda.

1. **Returinformation om partnerprofil:** Profilernas slutpunktssvar innehåller information om partnerprofilen, inklusive attributet `type` som är inställt på &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett profilsvar finns i [Skapa och hämta profil med API-dokumentationen för partnerautentiseringssvar](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md).
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
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > Profiles Partner-slutpunkten verifierar data för begäran för att säkerställa att villkoren för enkel inloggning för partner uppfylls:
   >
   >  * Partnerkonfigurationen för enkel inloggning på Adobe Pass-servern måste vara giltig och aktiverad.
   >  * Statusnyttolasten för partnerramverket som togs emot via rubriken [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) måste vara giltig.
   >
   > <br/>
   >
   > Om valideringen av enkel inloggning från partner misslyckas, används standardvärdet för det grundläggande flödet för hämtning av profiler.

1. **Fortsätt med beslutsflöden:** Direktuppspelningsprogrammet kan fortsätta med efterföljande beslutsflöden.

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för partnerramverkets status innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AP-Partner-Framework-Status` finns i dokumentationen för [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).
