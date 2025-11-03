---
title: Enkel utloggning - flöde
description: REST API V2 - enkel utloggning - flöde
exl-id: d7092ca7-ea7b-4e92-b45f-e373a6d673d6
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Enkelt utloggningsflöde {#single-logout-flow}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Gå även till [REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

## Initiera enkel utloggning för specifik mvpd {#initiate-single-logout-for-specific-mvpd}

### Förutsättningar {#prerequisites-initiate-single-logout-for-specific-mvpd}

Innan du startar en enda utloggning för en viss MVPD måste du kontrollera att följande krav är uppfyllda:

* Det andra direktuppspelningsprogrammet måste ha en giltig enkel inloggningsprofil som har skapats för MVPD med något av autentiseringsflödena för enkel inloggning:
   * [Utför autentisering genom enkel inloggning med plattformsidentitet](rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Utför autentisering med enkel inloggning med tjänsttoken](rest-api-v2-single-sign-on-service-token-flows.md)
* Det andra direktuppspelningsprogrammet måste initiera ett enda utloggningsflöde när det behöver logga ut från MVPD.

>[!IMPORTANT]
> 
> Antaganden
>
> <br/>
> 
> * Det första och andra direktuppspelningsprogrammet får samma unika plattformsidentifierarnyttolast som `JWS` eller `JWE` eller samma unika användaridentifierarnyttolast som `JWS`.

### Arbetsflöde {#workflow-initiate-single-logout-for-specific-mvpd}

Utför de angivna stegen för att implementera ett enda utloggningsflöde för en specifik MVPD enligt bilden nedan.

![Initiera enkel utloggning för specifik mvpd](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Initiera enkel utloggning för specifik mvpd*

1. **Initiera Adobe Pass-utloggning:** Direktuppspelningsprogrammet samlar in alla data som behövs för att initiera utloggningsflödet genom att anropa Adobe Pass utloggningsslutpunkt.

   >[!IMPORTANT]
   >
   > Mer information om hur du använder API-dokumentationen för mvpd[&#x200B; finns i &#x200B;](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)Initiera utloggning:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `redirectUrl`
   > * Alla _obligatoriska_ rubriker, som `Authorization`, `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker
   >
   > <br/>
   >
   > Strömningsprogrammet måste se till att det innehåller ett giltigt värde för den unika plattforms-ID:t eller den unika användaridentifieraren innan en begäran görs.
   >
   > <br/>
   > 
   > Mer information om rubriken `Adobe-Subject-Token` finns i dokumentationen för [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).
   > 
   > <br/>
   > 
   > Mer information om rubriken `AD-Service-Token` finns i dokumentationen för [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Hitta vanliga profiler och profiler för enkel inloggning:** Adobe Pass-servern identifierar giltiga profiler för både vanlig och enkel inloggning baserat på mottagna parametrar och rubriker.

1. **Ta bort vanliga profiler och profiler för enkel inloggning:** Adobe Pass-servern tar bort identifierade vanliga profiler och profiler för enkel inloggning från Adobe Pass serverdel.

1. **Ange nästa åtgärd:** Slutpunktssvaret för Adobe Pass-utloggningen innehåller de data som krävs för att vägleda direktuppspelningsprogrammet angående nästa åtgärd.

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett utloggningssvar finns i [Initiera utloggning för specifik mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API-dokumentation.
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
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Ange att utloggningen är slutförd:** Om MVPD inte stöder utloggningsflödet bearbetar direktuppspelningsprogrammet svaret och kan använda det för att visa ett specifikt meddelande i användargränssnittet.

1. **Initiera MVPD-utloggning:** Om MVPD stöder utloggningsflödet bearbetar direktuppspelningsprogrammet svaret och använder en användaragent för att initiera utloggningsflödet med MVPD. Flödet kan omfatta flera omdirigeringar till MVPD-system. Resultatet är dock att MVPD utför sin interna rensning och skickar den slutliga utloggningsbekräftelsen tillbaka till Adobe Pass backend.

1. **Ange att utloggningen är slutförd:** Strömningsprogrammet kan vänta på att användaragenten ska nå den angivna `redirectUrl` och kan använda den som en signal för att visa ett specifikt meddelande i användargränssnittet.

>[!NOTE]
>
> Stegen för det enskilda utloggningsflödet är desamma som ovan, om det initieras från det första direktuppspelningsprogrammet.
