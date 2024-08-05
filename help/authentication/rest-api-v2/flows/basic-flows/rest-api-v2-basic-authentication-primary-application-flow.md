---
title: Grundläggande autentisering - primärt program - flöde
description: REST API V2 - grundläggande autentisering - primärt program - flöde
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '883'
ht-degree: 0%

---


# Grundläggande autentiseringsflöde som utförs i det primära programmet {#basic-authentication-flow-performed-within-primary-application}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

**Autentiseringsflödet** i Adobe Pass-autentiseringsberättigandet gör att direktuppspelningsprogrammet kan verifiera att en användare har ett giltigt MVPD-konto. Denna process kräver att användaren har ett aktivt MVPD-konto och anger giltiga inloggningsuppgifter på MVPD-inloggningssidan.

Autentiseringsflöde krävs i följande fall:

* När användaren öppnar ett program för första gången.
* När användarens tidigare autentisering har upphört att gälla.
* När användaren loggar ut från MVPD-kontot.
* När användaren vill autentisera med ett annat MVPD.

I alla dessa fall får programmet som anropar någon av profilslutpunkterna ett tomt svar eller en eller flera profiler, men för olika programmeringsvideofilmsprogram.

**Autentiseringsflödet** kräver att en användaragent (webbläsare) slutför en serie samtal från programmet till Adobe Pass-servern, sedan till inloggningssidan för MVPD och slutligen tillbaka till programmet. Detta flöde kan omfatta flera omdirigeringar till MVPD-system och hantering av cookies eller sessioner som lagras för varje domän, vilket kan vara en utmaning att uppnå och skydda utan användaragent.

Autentiseringsscenarierna är följande:

* [Utför autentisering i det primära programmet](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Utför autentisering i det sekundära programmet med förvald mvpd](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Utför autentisering i det sekundära programmet utan förvald mvpd](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

## Utför autentisering i det primära programmet {#perform-authentication-within-primary-application}

### Förutsättningar {#prerequisites-perform-authentication-within-primary-application}

Innan du utför autentisering genom användarinteraktion i ett primärt program måste du kontrollera att följande krav är uppfyllda:

* Strömningsprogrammet måste välja ett MVPD.
* Strömningsprogrammet måste initiera en autentiseringssession för att kunna logga in med det valda MVPD-programmet.
* Strömningsprogrammet måste autentisera med det valda MVPD-programmet i en användaragent.

>[!IMPORTANT]
>
> Antaganden
>
> <br/>
> 
> * Strömningsprogrammet stöder användarinteraktion för att välja ett MVPD.
> * Strömningsprogrammet stöder användarinteraktion för att autentisera med det valda MVPD i en användaragent.

### Arbetsflöde {#workflow-perform-authentication-completed-on-primary-application}

Följ de angivna stegen för att implementera det grundläggande autentiseringsflödet som utförs i ett primärt program enligt bilden nedan.

![Utför autentisering i det primära programmet](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-perform-authentication-within-primary-application.png)

*Utför autentisering i det primära programmet*

1. **Skapa autentiseringssession:** Direktuppspelningsprogrammet samlar in alla data som behövs för att initiera en autentiseringssession genom att anropa sessionens slutpunkt.

   Mer information om hur du gör det finns i API-dokumentationen för [Skapa autentiseringssession](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md):
   * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd`, `domainName` och `redirectUrl`
   * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   * Alla _valfria_ parametrar och rubriker

   >[!IMPORTANT]
   > 
   > Direktuppspelningsprogrammet måste tillhandahålla alla nödvändiga parametrar i ett enda anrop när autentiseringssessionen skapas.

1. **Ange nästa åtgärd:** Sessionernas slutpunktssvar innehåller de data som behövs för att vägleda direktuppspelningsprogrammet när det gäller nästa åtgärd.

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

1. **Fortsätt med beslutsflöden:** Sessionernas slutpunktssvar innehåller följande data:
   * Attributet `actionName` är inställt på&quot;auktorisera&quot;.
   * Attributet `actionType` är inställt på&quot;direct&quot;.

   Om Adobe Pass serverdel identifierar en giltig profil behöver direktuppspelningsprogrammet inte autentisera igen med det valda MVPD-programmet, eftersom det redan finns en profil som kan användas för efterföljande beslutsflöden.

1. **Öppna URL i användaragent:** Sessionernas slutpunktssvar innehåller följande data:
   * `url` som kan användas för att initiera den interaktiva autentiseringen på MVPD-inloggningssidan.
   * Attributet `actionName` är inställt på &quot;authenticate&quot;.
   * Attributet `actionType` är inställt på &quot;interactive&quot;.

   Om Adobe Pass serverdel inte identifierar en giltig profil, öppnar direktuppspelningsprogrammet en användaragent för att läsa in `url`, vilket gör en begäran till slutpunkten för autentisering. Det här flödet kan innehålla flera omdirigeringar, vilket i slutänden leder användaren till MVPD-inloggningssidan och anger giltiga inloggningsuppgifter.

1. **Fullständig MVPD-autentisering:** Om autentiseringsflödet lyckas sparar användaragentinteraktionen en vanlig profil i Adobe Pass-serverdelen och når den angivna `redirectUrl`.

1. **Hämta profil för specifik kod:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta profilinformation genom att skicka en begäran till profilslutpunkten.

   Mer information om följande finns i [Hämta profil för specifik API-dokumentation för koden](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md):
   * Alla _obligatoriska_-parametrar, som `serviceProvider`, `code`
   * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   * Alla _valfria_ parametrar och rubriker

   >[!NOTE]
   >
   > Förslag: Direktuppspelningsprogrammet kan vänta på att användaragenten ska nå den angivna `redirectUrl` för att kontrollera om den vanliga profilen har genererats och sparats.

1. **Returinformation om vanlig profil:** Profilernas slutpunktssvar innehåller information om den vanliga profil som är associerad med de mottagna parametrarna och rubrikerna.

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
