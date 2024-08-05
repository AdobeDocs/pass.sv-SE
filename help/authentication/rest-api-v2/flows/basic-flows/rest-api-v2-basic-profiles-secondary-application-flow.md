---
title: Grundläggande profiler - sekundärt program - flöde
description: REST API V2 - Grundläggande profiler - Sekundärt program - flöde
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# Grundläggande profiler som körs i sekundärt program {#basic-profiles-flow-secondary-application}

**Profilerna flödar** inom Adobe Pass-autentiseringsberättigande vilket gör att det sekundära programmet kan komma åt information om aktiva användarinloggningar.

Med det grundläggande profilflödet kan du fråga efter följande scenarier:

* [Hämta profil för specifik kod](#retrieve-profile-for-specific-code)

## Hämta profil för specifik kod {#retrieve-profile-for-specific-code}

### Förutsättningar {#prerequisites-retrieve-profile-for-specific-code}

Innan du hämtar profilen för en viss autentiseringskod måste du se till att följande krav uppfylls:

* Det sekundära programmet, som har en `code` som används för att utföra den interaktiva autentiseringen med MVPD, vill hämta profilen för en viss autentiseringskod.

### Arbetsflöde {#workflow-retrieve-profile-for-specific-code}

Följ de angivna stegen för att implementera det grundläggande flödet för hämtning av profiler för en specifik autentiseringskod som har utförts i ett sekundärt program enligt bilden nedan.

![Hämta profil för specifik kod](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Hämta profil för specifik kod*

1. **Hämta profil för specifik kod:** Det sekundära programmet samlar in alla nödvändiga data för att hämta profilinformation för den specifika autentiseringskoden genom att skicka en begäran till profilslutpunkten.

   Mer information om följande finns i [Hämta profil för specifik API-dokumentation för koden](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md):
   * Alla _obligatoriska_-parametrar, som `serviceProvider` och `code`
   * Alla _obligatoriska_ rubriker, som `Authorization`
   * Alla _valfria_ parametrar och rubriker

1. **Hitta en vanlig profil:** Adobe Pass-servern identifierar en giltig profil baserat på mottagna parametrar och rubriker.

1. **Returinformation om vanlig profil:** Profilernas slutpunktssvar innehåller information om den hittade profil som är associerad med de mottagna parametrarna och rubrikerna.

   Mer information om vilken information som ges i ett profilsvar finns i [Hämta profil för specifik kod](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) API-dokumentation.

   >[!NOTE]
   >
   > Profilens slutpunkt validerar data i begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Ange att autentiseringsflödet har slutförts:** Om slutpunktssvaret för profiler innehåller en profil bearbetar det sekundära programmet svaret och kan använda det för att visa ett specifikt meddelande i användargränssnittet.

1. **Anger att autentiseringsflödet påträffade ett problem:** Om slutpunktssvaret för profiler inte innehåller någon profil bearbetar det sekundära programmet svaret och kan använda det för att visa ett specifikt meddelande i användargränssnittet.
