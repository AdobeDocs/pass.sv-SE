---
title: Översikt över registrering av dynamisk klient
description: Översikt över registrering av dynamisk klient
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---

# Översikt över registrering av dynamisk klient {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Dynamisk klientregistrering representerar en auktoriseringsmekanism som definieras av [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) och baseras på OAuth 2.0-auktoriseringsramverket som beskrivs av [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Adobe Pass erbjuder en dynamisk klientregistreringstjänst som ger åtkomst till följande skyddade API:er:

* API:er för hantering av Adobe Pass-autentisering:
   * [Återställ API för tillfälligt pass](../../features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [Försämring av API](../../features-premium/degraded-access/degradation-feature.md#degradation-api-access)
   * [Proxy MVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [API för tillståndsövervaknings-API](../../features-premium/esm/entitlement-service-monitoring-api.md)
* Adobe Pass Authentication REST API:er:
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [(Äldre) REST API V1](../../legacy/rest-api-v1/rest-api-reference.md)
* SDK för Adobe Pass-autentisering:
   * [(Äldre) JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [(Äldre) iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [(Äldre) Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [(Äldre) FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Auktoriseringsmekanismen för dynamisk klientregistrering ersätter äldre Adobe Pass-autentiseringslösningar som kan upphöra:
>
> * ID-mekanismen för den signerade beställaren.
> * Domänlistningsmekanismen.
> * API-nyckelmekanismen.

I och med införandet av dynamisk kundregistrering är de största fördelarna:

* Förbättrad säkerhet.
* Enhetlig modell för olika plattformar.
* Detaljerad kontroll över programmets livscykel.

Mer information om hur du hanterar och använder dynamisk klientregistrering finns i följande avsnitt.

## Registreringshantering för dynamisk klient {#dynamic-client-registration-management}

Den dynamiska klientregistreringsprocessen gör att klientprogram som körs på specifika plattformar och som behöver åtkomst till specifika API:er för Adobe Pass-autentisering kan registreras via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

Adobe Pass TVE Dashboard är ett verktyg som används av Adobe Pass Authentication-kunder (programmerare) för att hantera konfiguration och data. Den här självbetjäningsinstrumentpanelen möjliggör en rad funktioner som beskrivs i [Adobe Pass TVE Dashboard User Guide](../../../user-guide-tve-dashboard/tve-dashboard-overview.md) -dokumentationen.

Om du har tillgång till [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) följer du stegen nedan för att skapa ett registrerat program och hämta programsatsen.

### Hantera registrerade program {#manage-registered-applications}

>[!IMPORTANT]
>
> Om du inte har tillgång till Adobe Pass TVE Dashboard skapar du en biljett via [Zendesk](https://adobeprimetime.zendesk.com) och ber din tekniska kontoansvarige (TAM) att skapa en registrerad applikation och dela programsatsen med dig.

Det finns två sätt att skapa ett registrerat program:

* **Programmernivå**

  Med registreringsprocessen på programmeringsnivå kan du skapa ett registrerat program som är länkat till alla tillgängliga kanaler eller en vald delmängd av kanaler. Mer information finns i dokumentationen för [TVE Dashboard User Guide for Programmers](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md).


* **Kanalnivå**

  Med registreringsprocessen på kanalnivå kan du skapa ett registrerat program som bara är länkat till den valda kanalen. Mer information finns i [användarhandboken för TVE Dashboard för kanaler](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) .

>[!IMPORTANT]
>
> Vi rekommenderar att du skapar registrerade program med mer specifika och begränsade behörigheter för att förbättra säkerheten och förhindra obehörig åtkomst. När du skapar registrerade program bör du därför överväga att använda smalare alternativ för de tilldelade `channels`, `platforms` och `scopes`.
>
> Vi rekommenderar att du skapar ett nytt registrerat program för varje större uppdatering av klientprogrammet för att hantera dess livscykel och användning. Om det behövs skapar du en biljett via vår [Zendesk](https://adobeprimetime.zendesk.com) och ber din tekniska kontohanterare (TAM) att återkalla ett registrerat program för att blockera funktionaliteten i en viss klientprogramversion.

### Hantera programsatser {#manage-software-statements}

>[!IMPORTANT]
>
> Om du inte har tillgång till Adobe Pass TVE Dashboard skapar du en biljett via [Zendesk](https://adobeprimetime.zendesk.com) och ber din tekniska kontoansvarige (TAM) att skapa en registrerad applikation och dela programsatsen med dig.

Innan du hämtar en programsats måste du se till att du har skapat ett registrerat program enligt beskrivningen i avsnittet [Hantera registrerade program](#manage-registered-applications) som uppfyller klientprogrammets krav.

Det finns två sätt att ladda ned en programsats baserat på nivån där det registrerade programmet skapades:

* **Programmernivå**

  Mer information finns i dokumentationen för [TVE Dashboard User Guide for Programmers](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md).

* **Kanalnivå**

  Mer information finns i [användarhandboken för TVE Dashboard för kanaler](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) .

Programsatsen är en JSON-webbtoken (`JWT`) som innehåller information om klientprogramvaran som ett paket. När programsatsen visas för [Hämta klientautentiseringsuppgifter](apis/dynamic-client-registration-apis-retrieve-client-credentials.md)-API:t signeras den digitalt med JSON-webbsignatur (`JWS`).

Mer detaljerad information om vilka programsatser som är och hur de fungerar finns i [RFC 7591](https://tools.ietf.org/html/rfc7591) -dokumentationen.

## Dynamiskt klientregistreringsflöde {#dynamic-client-registration-flow}

Sammanfattningsvis omfattar den dynamiska auktoriseringsmekanismen för klientregistrering flera steg:

**Hantering**

* En klientrepresentant måste skapa ett registrerat program enligt beskrivningen i avsnittet [Hantera registrerade program](#manage-registered-applications).
* En klientrepresentant måste hämta och bädda in en programsats enligt beskrivningen i avsnittet [Hantera programsatser](#manage-software-statements).

**Flöde**

* Klientprogrammet måste hämta klientautentiseringsuppgifterna enligt beskrivningen i API-dokumentationen för [Hämta klientautentiseringsuppgifter](apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
* Klientprogrammet måste hämta åtkomsttoken enligt beskrivningen i API-dokumentationen för [Hämta åtkomsttoken](apis/dynamic-client-registration-apis-retrieve-access-token.md).

Läs dokumentationen för [Dynamiskt klientregistreringsflöde](flows/dynamic-client-registration-flow.md) om du vill veta mer om hur du får åtkomst till Adobe Pass-skyddade API:er. Du kan även titta på den här [webbinariet](https://my.adobeconnect.com/pzkp8ujrigg1/)-inspelningen, som ger mer kontext och innehåller en demo.
