---
title: Grundläggande profiler - primärt program - flöde
description: REST API V2 - Grundläggande profiler - Primärt program - Flöde
exl-id: 19ddf382-9a32-4b94-aa84-7611c0e1780e
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '974'
ht-degree: 0%

---

# Grundläggande profilflöden som utförs i det primära programmet {#basic-profiles-flow-primary-application}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Gå även till [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

**Profilerna flödar** inom Adobe Pass-autentiseringsberättigande vilket gör att direktuppspelningsprogrammet kan komma åt information om aktiva användarinloggningar.

Med det grundläggande profilflödet kan du fråga efter följande scenarier:

* [Hämta profiler](#retrieve-profiles)
* [Hämta profil för specifik mvpd](#retrieve-profile-for-specific-mvpd)
* [Hämta profil för specifik kod](#retrieve-profile-for-specific-code)

## Hämta profiler {#retrieve-profiles}

### Förutsättningar {#prerequisites-retrieve-profiles}

Innan du hämtar profiler kontrollerar du att följande krav är uppfyllda:

* Strömningsprogrammet vill hämta alla vanliga profiler.

### Arbetsflöde {#workflow-retrieve-profiles}

Följ de angivna stegen för att implementera det grundläggande återgivningsflödet för profiler som utförs i ett primärt program enligt bilden nedan.

![Hämta profiler](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profiles-within-primary-application.png)

*Hämta profiler*

1. **Hämta profiler:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta all profilinformation genom att skicka en begäran till profilslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta profiler](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API-dokumentationen:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker

1. **Hitta vanliga profiler:** Adobe Pass-servern identifierar alla giltiga profiler baserat på mottagna parametrar och rubriker.

1. **Returnera information om vanliga profiler:** Profilernas slutpunktssvar innehåller information om de hittade profiler som är associerade med de mottagna parametrarna och rubrikerna.

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

1. **Välj en profil och fortsätt med beslutsflöden:** Om slutpunktssvaret för profiler innehåller profiler använder direktuppspelningsprogrammet sin interna logik (genom att till slut interagera med slutanvändaren) för att välja en av de tillgängliga profilerna för att fortsätta med efterföljande beslutsflöden.

1. **Ange nytt grundläggande autentiseringsflöde:** Om slutpunktssvaret för profiler inte innehåller någon profil anger direktuppspelningsprogrammet att användaren ska initiera ett nytt grundläggande autentiseringsflöde.

## Hämta profil för specifik mvpd {#retrieve-profile-for-specific-mvpd}

### Förutsättningar {#prerequisites-retrieve-profile-for-specific-mvpd}

Innan du hämtar profilen för en viss MVPD måste följande krav vara uppfyllda:

* Strömningsprogrammet, som har en markerad eller cachelagrad `mvpd`-identifierare, vill hämta den vanliga profilen för en specifik MVPD.

### Arbetsflöde {#workflow-retrieve-profile-for-specific-mvpd}

Följ de angivna stegen för att implementera det grundläggande flödet för hämtning av profiler för en viss MVPD som utförs i ett primärt program enligt bilden nedan.

![Hämta profil för specifik mvpd](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png)

*Hämta profil för specifik mvpd*

1. **Hämta profil för specifik mvpd:** Direktuppspelningsprogrammet samlar in alla data som behövs för att hämta profilinformation för den specifika MVPD-filen genom att skicka en begäran till profilslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta profil för specifik mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API-dokumentation:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider` och `mvpd`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker

1. **Hitta en vanlig profil:** Adobe Pass-servern identifierar en giltig profil baserat på mottagna parametrar och rubriker.

1. **Returinformation om vanlig profil:** Profilernas slutpunktssvar innehåller information om den hittade profil som är associerad med de mottagna parametrarna och rubrikerna.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett profilsvar finns i [Hämta profil för specifik dokumentation för mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) API.
   > 
   > <br/>
   > 
   > Profilens slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Fortsätt med beslutsflöden:** Om slutpunktssvaret för profiler innehåller en profil använder direktuppspelningsprogrammet profilinformationen för att fortsätta med efterföljande beslutsflöden.

1. **Ange nytt grundläggande autentiseringsflöde:** Om slutpunktssvaret för profiler inte innehåller någon profil anger direktuppspelningsprogrammet att användaren ska initiera ett nytt grundläggande autentiseringsflöde.

## Hämta profil för specifik kod {#retrieve-profile-for-specific-code}

### Förutsättningar {#prerequisites-retrieve-profile-for-specific-code}

Innan du hämtar profilen för en viss autentiseringskod måste du se till att följande krav uppfylls:

* Strömningsprogrammet, som har en `code` som används för att utföra den interaktiva autentiseringen med MVPD, vill hämta profilen för en viss autentiseringskod.

### Arbetsflöde {#workflow-retrieve-profile-for-specific-code}

Följ de angivna stegen för att implementera det grundläggande flödet för hämtning av profiler för en specifik autentiseringskod som har utförts i ett primärt program enligt bilden nedan.

![Hämta profil för specifik kod](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png)

*Hämta profil för specifik kod*

1. **Hämta profil för specifik kod:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att hämta profilinformation för den specifika autentiseringskoden genom att skicka en begäran till profilslutpunkten.

   >[!IMPORTANT]
   >
   > Mer information om följande finns i [Hämta profil för specifik API-dokumentation för koden](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md):
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider` och `code`
   > * Alla _obligatoriska_ rubriker, som `Authorization`
   > * Alla _valfria_ parametrar och rubriker

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

1. **Fortsätt med beslutsflöden:** Om slutpunktssvaret för profiler innehåller en profil använder direktuppspelningsprogrammet profilinformationen för att fortsätta med efterföljande beslutsflöden.

1. **Ange nytt grundläggande autentiseringsflöde:** Om slutpunktssvaret för profiler inte innehåller någon profil anger det primära programmet att användaren ska initiera ett nytt grundläggande autentiseringsflöde.
