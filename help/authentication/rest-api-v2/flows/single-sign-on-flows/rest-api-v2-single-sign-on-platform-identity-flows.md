---
title: Enkel inloggning - plattformsidentitet - flöden
description: REST API V2 - enkel inloggning - plattformsidentitet - flöden
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '1819'
ht-degree: 0%

---


# Samlad inloggning med plattformsidentitetsflöden {#single-sign-on-platform-identity-full-flows}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Metoden Platform Identity gör det möjligt för flera program att använda en unik plattformsidentifierare för att uppnå enkel inloggning (SSO) på enhets- eller plattformsnivå när Adobe Pass-tjänster används.

Programmen hämtar den unika plattformsidentifierarnyttolasten med enhetsspecifika identitetstjänster eller bibliotek utanför Adobe Pass-system.

Programmen ansvarar för att inkludera den här unika plattforms-ID-nyttolasten som en del av `Adobe-Subject-Token`-huvudet för alla begäranden som anger den.

Mer information om rubriken `Adobe-Subject-Token` finns i [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) -dokumentationen.

## Utför autentisering genom enkel inloggning med plattformsidentitet {#perform-authentication-through-single-sign-on-using-platform-identity}

### Förutsättningar {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

Innan du utför autentiseringsflödet genom enkel inloggning med en plattformsidentitet måste du kontrollera att följande krav är uppfyllda:

* Plattformen måste tillhandahålla en identitetstjänst eller ett bibliotek som returnerar konsekvent information som `JWS` eller `JWE` nyttolast för alla program på samma enhet eller plattform.
* Det första direktuppspelningsprogrammet måste hämta den unika plattforms-ID:t och inkludera `JWS`- eller `JWE`-nyttolasten som en del av [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)-huvudet för alla begäranden som anger det.
* Det första direktuppspelningsprogrammet måste välja en MVPD.
* Det första direktuppspelningsprogrammet måste initiera en autentiseringssession för att kunna logga in med det valda MVPD-programmet.
* Det första direktuppspelningsprogrammet måste autentisera med det valda MVPD-programmet i en användaragent.
* Det andra direktuppspelningsprogrammet måste hämta den unika plattforms-ID:t och inkludera `JWS`- eller `JWE`-nyttolasten som en del av [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)-huvudet för alla begäranden som anger det.

>[!IMPORTANT]
>
> Antaganden
>
> <br/>
> 
> * Det första direktuppspelningsprogrammet stöder användarinteraktion för att välja ett MVPD.
> * Det första direktuppspelningsprogrammet stöder användarinteraktion för att autentisera med det valda MVPD-programmet i en användaragent.

### Arbetsflöde {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

Utför de angivna stegen för att implementera autentiseringsflödet genom enkel inloggning med en plattformsidentitet, vilket visas i följande diagram.

![Utför autentisering med enkel inloggning med plattformsidentitet](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Utför autentisering med enkel inloggning med plattformsidentitet*

1. **Hämta plattformsidentifierare:** Det första direktuppspelningsprogrammet anropar identitetstjänsten eller -biblioteket utanför Adobe Pass-systemen för att hämta `JWS`- eller `JWE`-nyttolasten som är associerad med den unika plattforms-ID:t.

1. **Returnera en unik plattformsidentifierare som JWS eller JWE:** Det första direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande säkerhetsvillkor uppfylls:
   * Nyttolasten har inte gått ut.
   * Nyttolasten är signerad eller krypterad.

1. **Skapa autentiseringssession:** Det första direktuppspelningsprogrammet samlar in alla nödvändiga data för att initiera en autentiseringssession genom att anropa sessionens slutpunkt.

   Mer information om hur du gör det finns i API-dokumentationen för [Skapa autentiseringssession](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md):
   * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd`, `domainName` och `redirectUrl`
   * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   * Alla _valfria_ parametrar och rubriker

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika plattforms-ID:t innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `Adobe-Subject-Token` finns i [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) -dokumentationen.

1. **Ange nästa åtgärd:** Sessionernas slutpunktssvar innehåller de data som krävs för att vägleda det första direktuppspelningsprogrammet angående nästa åtgärd.

   Mer information om vilken information som finns i ett sessionssvar finns i API-dokumentationen för [Skapa autentiseringssession](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

   >[!IMPORTANT]
   >
   > Sessionernas slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Öppna URL i användaragent:** Sessionernas slutpunktssvar innehåller följande data:
   * `url` som kan användas för att initiera den interaktiva autentiseringen på MVPD-inloggningssidan.
   * Attributet `actionName` är inställt på &quot;authenticate&quot;.
   * Attributet `actionType` är inställt på &quot;interactive&quot;.

   Om Adobe Pass serverdel inte identifierar en giltig profil, öppnar det första direktuppspelningsprogrammet en användaragent för att läsa in `url`, vilket gör en begäran till slutpunkten för autentisering. Det här flödet kan innehålla flera omdirigeringar, vilket i slutänden leder användaren till MVPD-inloggningssidan och anger giltiga inloggningsuppgifter.

1. **Fullständig MVPD-autentisering:** Om autentiseringsflödet lyckas sparar användaragentinteraktionen en vanlig profil i Adobe Pass-serverdelen och når den angivna `redirectUrl`.

1. **Hämta profil för specifik kod:** Det första direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta profilinformation genom att skicka en begäran till profilslutpunkten.

   Mer information om följande finns i [Hämta profil för specifik API-dokumentation för koden](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md):
   * Alla _obligatoriska_-parametrar, som `serviceProvider`, `code`
   * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   * Alla _valfria_ parametrar och rubriker

   >[!NOTE]
   >
   > Förslag: Direktuppspelningsprogrammet kan vänta på att användaragenten ska nå den angivna `redirectUrl` för att kontrollera om den vanliga profilen har genererats och sparats.

1. **Hitta en vanlig profil:** Adobe Pass-servern identifierar en giltig profil baserat på mottagna parametrar och rubriker.

1. **Returinformation om vanlig profil:** Profilernas slutpunktssvar innehåller information om den hittade profil som är associerad med de mottagna parametrarna och rubrikerna.

   Mer information om vilken information som ges i ett profilsvar finns i [Hämta profil för specifik kod](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) API-dokumentation.

   >[!IMPORTANT]
   >
   > Profilens slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Fortsätt med beslutsflöden:** Det första direktuppspelningsprogrammet kan fortsätta med efterföljande beslutsflöden.

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika plattforms-ID:t innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `Adobe-Subject-Token` finns i [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) -dokumentationen.

1. **Hämta plattformsidentifierare:** Det andra direktuppspelningsprogrammet anropar identitetstjänsten eller -biblioteket utanför Adobe Pass-systemen för att hämta `JWS`- eller `JWE`-nyttolasten som är associerad med den unika plattforms-ID:t.

1. **Returnera en unik plattformsidentifierare som JWS eller JWE:** Det andra direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande säkerhetsvillkor uppfylls:
   * Nyttolasten har inte gått ut.
   * Nyttolasten är signerad eller krypterad.

1. **Hämta profiler:** Det andra direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta all profilinformation genom att skicka en begäran till profilslutpunkten.

   Mer information om följande finns i [Hämta profiler](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API-dokumentationen:
   * Alla _obligatoriska_-parametrar, som `serviceProvider`
   * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   * Alla _valfria_ parametrar och rubriker

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika plattforms-ID:t innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `Adobe-Subject-Token` finns i [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) -dokumentationen.

1. **Sök efter en enkel inloggningsprofil:** Adobe Pass-servern identifierar en giltig enkel inloggningsprofil baserat på mottagna parametrar och rubriker.

1. **Returinformation om enkel inloggningsprofil:** Profilens slutpunktssvar innehåller information om den hittade profil som är associerad med de mottagna parametrarna och rubrikerna.

   Mer information om vilken information som ges i ett profilsvar finns i [Hämta profiler](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API-dokumentationen.

   >[!IMPORTANT]
   >
   > Profilens slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Fortsätt med beslutsflöden:** Det andra direktuppspelningsprogrammet kan fortsätta med efterföljande beslutsflöden.

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika plattforms-ID:t innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `Adobe-Subject-Token` finns i [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) -dokumentationen.

## Hämta auktoriseringsbeslut via enkel inloggning med plattformsidentitet{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### Förutsättningar {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Innan du utför auktoriseringsflödet genom enkel inloggning med en plattformsidentitet måste du kontrollera att följande krav är uppfyllda:

* Plattformen måste tillhandahålla en identitetstjänst eller ett bibliotek som returnerar konsekvent information som `JWS` eller `JWE` nyttolast för alla program på samma enhet eller plattform.
* Det andra direktuppspelningsprogrammet måste hämta den unika plattforms-ID:t och inkludera `JWS`- eller `JWE`-nyttolasten som en del av [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)-huvudet för alla begäranden som anger det.
* Det andra direktuppspelningsprogrammet måste hämta ett auktoriseringsbeslut innan en användarvald resurs spelas upp.

>[!IMPORTANT]
>
> Antaganden
> 
> <br/>
> 
> * Det första direktuppspelningsprogrammet har utfört autentisering och har inkluderat ett giltigt värde för begärandehuvudet [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

### Arbetsflöde {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Utför de angivna stegen för att implementera auktoriseringsflödet genom enkel inloggning med en plattformsidentitet, vilket visas i följande diagram.

![Hämta auktoriseringsbeslut via enkel inloggning med plattformsidentitet](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*Hämta auktoriseringsbeslut via enkel inloggning med plattformsidentitet*

1. **Hämta plattformsidentifierare:** Det andra direktuppspelningsprogrammet anropar identitetstjänsten eller -biblioteket utanför Adobe Pass-systemen för att hämta `JWS`- eller `JWE`-nyttolasten som är associerad med den unika plattforms-ID:t.

1. **Returnera en unik plattformsidentifierare som JWS eller JWE:** Det andra direktuppspelningsprogrammet validerar svarsdata för att säkerställa att grundläggande säkerhetsvillkor uppfylls:
   * Nyttolasten har inte gått ut.
   * Nyttolasten är signerad eller krypterad.

1. **Hämta auktoriseringsbeslut:** Det andra direktuppspelningsprogrammet samlar in alla nödvändiga data för att erhålla ett auktoriseringsbeslut för en specifik resurs genom att anropa auktoriseringsslutpunkten för beslut.

   Mer information om följande finns i [Hämta auktoriseringsbeslut med hjälp av specifik API-dokumentation för mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md):
   * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `resources`
   * Alla _obligatoriska_ rubriker, som `Authorization` och `AP-Device-Identifier`
   * Alla _valfria_ parametrar och rubriker

   >[!IMPORTANT]
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika plattforms-ID:t innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `Adobe-Subject-Token` finns i [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) -dokumentationen.

1. **Sök efter en enkel inloggningsprofil:** Adobe Pass-servern identifierar en giltig enkel inloggningsprofil baserat på mottagna parametrar och rubriker.

1. **Hämta MVPD-beslut för begärd resurs:** Adobe Pass-servern anropar MVPD-auktoriseringsslutpunkten för att erhålla ett `Permit` - eller `Deny`-beslut för den specifika resurs som tagits emot från direktuppspelningsprogrammet.

1. **Returbeslut `Permit` med medietoken:** Slutpunktssvaret för beslutsauktorisering innehåller ett `Permit`-beslut och en medietoken.

   Mer information om vilken information som ges i ett beslutssvar finns i [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API-dokumentation.

   >[!IMPORTANT]
   >
   > Slutpunkten för beslutsauktorisering validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Starta dataström med medietoken:** Det andra direktuppspelningsprogrammet använder medietoken för att spela upp innehållet.

1. **Returbeslut `Deny` med information:** Slutpunktssvaret för beslut auktoriserar innehåller ett `Deny`-beslut och en felnyttolast som följer [Förbättrade felkoder](../../../enhanced-error-codes.md) -dokumentationen.

   Mer information om vilken information som ges i ett beslutssvar finns i [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API-dokumentation.

   >[!IMPORTANT]
   >
   > Slutpunkten för beslutsauktorisering validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Hantera `Deny` beslutsinformation:** Det andra direktuppspelningsprogrammet behandlar felinformationen från svaret och kan använda den för att visa ett specifikt meddelande i användargränssnittet.

>[!NOTE]
>
> Stegen för förauktoriseringsflödet är desamma som för auktoriseringsflödet, förutom att slutpunkten som används är den som beskrivs i [Hämta förauktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)-dokumentation.
