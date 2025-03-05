---
title: Enkel inloggning - tjänsttoken - flöden
description: REST API V2 - enkel inloggning - tjänsttoken - flöden
exl-id: b0082d2a-e491-4cb5-bb40-35ba10db6b1a
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '1858'
ht-degree: 0%

---

# Enkel inloggning med tjänsttoken-flöden{#single-sign-on-service-token-full-flows}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Gå även till [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

Med Service Token-metoden kan flera program använda en unik användaridentifierare för att uppnå enkel inloggning (SSO) på flera enheter och plattformar när Adobe Pass-tjänster används.

Programmen hämtar nyttolasten för den unika användaridentifieraren med hjälp av externa identitetstjänster utanför Adobe Pass-system, till exempel:

* En direkt-till-kund-tjänst (DTC) där användarna loggar in på alla enheter med samma inloggningsuppgifter och är kopplade till samma användar-ID eller användarkontonamn.
* En autentiseringstjänst från tredje part, som Google eller Facebook, där användare loggar in på alla enheter med samma inloggningsuppgifter och är associerade med samma e-postadress.

Programmen ansvarar för att ta med den här unika användaridentifierarnyttolasten som en del av `AD-Service-Token`-huvudet för alla begäranden som anger den.

Mer information om rubriken `AD-Service-Token` finns i dokumentationen för [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

## Utför autentisering med enkel inloggning med tjänsttoken {#performing-authentication-flow-using-service-token-single-sign-on-method}

### Förutsättningar {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Innan du utför autentiseringsflödet genom enkel inloggning med en tjänsttoken måste du kontrollera att följande krav är uppfyllda:

* Den externa identitetstjänsten måste returnera konsekvent information som `JWS` nyttolast för alla program över flera enheter och plattformar.
* Det första direktuppspelningsprogrammet måste hämta den unika användaridentifieraren och inkludera `JWS`-nyttolasten som en del av [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)-huvudet för alla begäranden som anger det.
* Det första direktuppspelningsprogrammet måste välja en MVPD.
* Det första direktuppspelningsprogrammet måste initiera en autentiseringssession för att kunna logga in med det valda MVPD-programmet.
* Det första direktuppspelningsprogrammet måste autentiseras med den valda MVPD i en användaragent.
* Det andra direktuppspelningsprogrammet måste hämta den unika användaridentifieraren och inkludera `JWS`-nyttolasten som en del av [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)-huvudet för alla begäranden som anger det.

>[!IMPORTANT]
>
> Antaganden
> 
> <br/>
> 
> * Det första direktuppspelningsprogrammet stöder användarinteraktion för att välja en MVPD.
> * Det första direktuppspelningsprogrammet har stöd för användarinteraktion för autentisering med den valda MVPD i en användaragent.

### Arbetsflöde {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

Utför de angivna stegen för att implementera autentiseringsflödet med enkel inloggning med en tjänsttoken, vilket visas i följande diagram.

![Utför autentisering med enkel inloggning med tjänsttoken](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png)

*Utför autentisering med enkel inloggning med tjänsttoken*

1. **Autentisera med identitetstjänsten:** Det första direktuppspelande programmet anropar identitetstjänsten utanför Adobe Pass-system för att erhålla den `JWS`-nyttolast som är associerad med den unika användaridentifieraren.

1. **Returnera en unik användaridentifierare som JWS:** Det första direktuppspelningsprogrammet verifierar svarsdata för att säkerställa att grundläggande säkerhetsvillkor uppfylls:
   * Nyttolasten har inte gått ut.
   * Nyttolasten är signerad.

1. **Skapa autentiseringssession:** Det första direktuppspelningsprogrammet samlar in alla nödvändiga data för att initiera en autentiseringssession genom att anropa sessionens slutpunkt.

   >[!IMPORTANT]
   >
   > Mer information om hur du gör det finns i API-dokumentationen för [Skapa autentiseringssession](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd`, `domainName` och `redirectUrl`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker
   >
   > <br/>
   > 
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika användaridentifieraren innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AD-Service-Token` finns i dokumentationen för [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Ange nästa åtgärd:** Sessionernas slutpunktssvar innehåller de data som krävs för att vägleda det första direktuppspelningsprogrammet angående nästa åtgärd.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som finns i ett sessionssvar finns i API-dokumentationen för [Skapa autentiseringssession](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
   >
   > <br/>
   > 
   > Sessionernas slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Öppna URL i användaragent:** Sessionernas slutpunktssvar innehåller följande data:
   * `url` som kan användas för att initiera den interaktiva autentiseringen på inloggningssidan för MVPD.
   * Attributet `actionName` är inställt på &quot;authenticate&quot;.
   * Attributet `actionType` är inställt på &quot;interactive&quot;.

   Om Adobe Pass serverdel inte identifierar en giltig profil, öppnar det första direktuppspelningsprogrammet en användaragent för att läsa in `url`, vilket gör en begäran till slutpunkten för autentisering. Det här flödet kan innehålla flera omdirigeringar, vilket i slutänden leder till inloggningssidan för MVPD och anger giltiga inloggningsuppgifter.

1. **Fullständig MVPD-autentisering:** Om autentiseringsflödet lyckas sparar användaragentinteraktionen en vanlig profil i Adobe Pass serverdel och når den angivna `redirectUrl`.

1. **Hämta profil för specifik kod:** Det första direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta profilinformation genom att skicka en begäran till profilslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta profil för specifik API-dokumentation för koden](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md):
   > 
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `code`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker

   >[!TIP]
   >
   > Förslag: Direktuppspelningsprogrammet kan vänta på att användaragenten ska nå den angivna `redirectUrl` för att kontrollera om den vanliga profilen har genererats och sparats.

1. **Hitta en vanlig profil:** Adobe Pass-servern identifierar en giltig profil baserat på mottagna parametrar och rubriker.

1. **Returinformation om vanlig profil:** Profilernas slutpunktssvar innehåller information om den hittade profil som är associerad med de mottagna parametrarna och rubrikerna.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett profilsvar finns i [Hämta profil för specifik kod](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API-dokumentation.
   >
   > <br/>
   > 
   > Profilens slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Fortsätt med beslutsflöden:** Det första direktuppspelningsprogrammet kan fortsätta med efterföljande beslutsflöden.

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika användaridentifieraren innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AD-Service-Token` finns i dokumentationen för [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Autentisera med identitetstjänsten:** Det andra direktuppspelningsprogrammet anropar identitetstjänsten, utanför Adobe Pass-system, för att erhålla den `JWS`-nyttolast som är associerad med den unika användaridentifieraren.

1. **Returnera en unik användaridentifierare som JWS:** Det andra direktuppspelningsprogrammet verifierar svarsdata för att säkerställa att grundläggande säkerhetsvillkor uppfylls:
   * Nyttolasten har inte gått ut.
   * Nyttolasten är signerad.

1. **Hämta profiler:** Det andra direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta all profilinformation genom att skicka en begäran till profilslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta profiler](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API-dokumentationen:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker
   >
   > <br/>
   > 
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika användaridentifieraren innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AD-Service-Token` finns i dokumentationen för [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Sök efter en enkel inloggningsprofil:** Adobe Pass-servern identifierar en giltig enkel inloggningsprofil baserat på mottagna parametrar och rubriker.

1. **Returinformation om enkel inloggningsprofil:** Profilens slutpunktssvar innehåller information om den hittade profil som är associerad med de mottagna parametrarna och rubrikerna.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett profilsvar finns i [Hämta profiler](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API-dokumentationen.
   > 
   > <br/>
   > 
   > Profilens slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Fortsätt med beslutsflöden:** Det andra direktuppspelningsprogrammet kan fortsätta med efterföljande beslutsflöden.

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika användaridentifieraren innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AD-Service-Token` finns i dokumentationen för [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

## Hämta auktoriseringsbeslut via enkel inloggning med tjänsttoken {#performing-authorization-flow-using-service-token-single-sign-on-method}

### Förutsättningar {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Innan du utför auktoriseringsflödet via enkel inloggning med en tjänsttoken måste du kontrollera att följande krav är uppfyllda:

* Den externa identitetstjänsten måste returnera konsekvent information som `JWS` nyttolast för alla program över flera enheter och plattformar.
* Det första direktuppspelningsprogrammet måste hämta den unika användaridentifieraren och inkludera `JWS`-nyttolasten som en del av [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)-huvudet för alla begäranden som anger det.
* Det andra direktuppspelningsprogrammet måste hämta ett auktoriseringsbeslut innan en användarvald resurs spelas upp.

>[!IMPORTANT]
>
> Antaganden
>
> <br/>
> 
> * Det första direktuppspelningsprogrammet har utfört autentisering och har inkluderat ett giltigt värde för begärandehuvudet [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

### Arbetsflöde {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

Utför de angivna stegen för att implementera auktoriseringsflödet genom enkel inloggning med en servicetoken, vilket visas i följande diagram.

![Hämta auktoriseringsbeslut via enkel inloggning med tjänsttoken](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png)

*Hämta auktoriseringsbeslut via enkel inloggning med tjänsttoken*

1. **Autentisera med identitetstjänsten:** Det andra direktuppspelningsprogrammet anropar identitetstjänsten, utanför Adobe Pass-system, för att erhålla den `JWS`-nyttolast som är associerad med den unika användaridentifieraren.

1. **Returnera en unik användaridentifierare som JWS:** Det andra direktuppspelningsprogrammet verifierar svarsdata för att säkerställa att grundläggande säkerhetsvillkor uppfylls:
   * Nyttolasten har inte gått ut.
   * Nyttolasten är signerad.

1. **Hämta auktoriseringsbeslut:** Det andra direktuppspelningsprogrammet samlar in alla nödvändiga data för att erhålla ett auktoriseringsbeslut för en specifik resurs genom att anropa auktoriseringsslutpunkten för beslut.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta auktoriseringsbeslut med hjälp av specifik API-dokumentation för mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `resources`
   > * Alla _obligatoriska_ rubriker, som `Authorization` och `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker
   >
   > <br/>
   > 
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika användaridentifieraren innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `AD-Service-Token` finns i dokumentationen för [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Sök efter en enkel inloggningsprofil:** Adobe Pass-servern identifierar en giltig enkel inloggningsprofil baserat på mottagna parametrar och rubriker.

1. **Hämta MVPD-beslut för begärd resurs:** Adobe Pass-servern anropar MVPD-auktoriseringsslutpunkten för att erhålla ett `Permit` - eller `Deny`-beslut för den specifika resurs som tagits emot från direktuppspelningsprogrammet.

1. **Returbeslut `Permit` med medietoken:** Slutpunktssvaret för beslutsauktorisering innehåller ett `Permit`-beslut och en medietoken.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett beslutssvar finns i [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API-dokumentation.
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
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Starta dataström med medietoken:** Det andra direktuppspelningsprogrammet använder medietoken för att spela upp innehållet.

1. **Returbeslut `Deny` med information:** Slutpunktssvaret för beslut auktoriserar innehåller ett `Deny`-beslut och en felnyttolast som följer [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md) -dokumentationen.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett beslutssvar finns i [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API-dokumentation.
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
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Hantera `Deny` beslutsinformation:** Det andra direktuppspelningsprogrammet behandlar felinformationen från svaret och kan använda den för att visa ett specifikt meddelande i användargränssnittet.

>[!NOTE]
>
> Stegen för förauktoriseringsflödet är desamma som för auktoriseringsflödet, förutom att slutpunkten som används är den som beskrivs i [Hämta förauktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)-dokumentation.
