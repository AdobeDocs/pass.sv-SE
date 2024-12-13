---
title: Tillståndsövervakningens översikt
description: Tillståndsövervakningens översikt
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# Tillståndsövervakningens översikt {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#introduction}

TVE-sajter och -appar måste vara tillgängliga dygnet runt, så kunderna behöver realtidsinsikter i berättigandehändelser för att kunna upptäcka och korrigera problem så snabbt som möjligt. De måste också analysera månadsdata för att avgöra vilka plattformar som tillhandahåller större delen av trafiken, och vilka plattformar som kan ha dålig implementering och dålig konverteringsgrad.

ESM (Entitlement Service Monitoring) ger programmerare och distributörer av videoprogrammeringstjänster en datafeed som ger realtidsinsyn i deras autentiserings- och auktoriseringshändelser. Data samlas in från Adobe Pass autentiseringssystem och tillhandahålls via ett RESTful API.  Kunderna kan förbruka data direkt eller från sina egna skräddarsydda kontrollpaneler.

ESM-systemets kärnelement är dess mått och mått. ESM genererar rapporter som innehåller aggregerade mätvärden enligt dimensionsurvalet. När Adobe Pass-händelser är inloggade i PST-tidszonen är ESM-rapporterna också tillgängliga i PST-tidszonen.

ESM API är inte allmänt tillgängligt.  Kontakta din Adobe-representant om du har frågor om tillgänglighet.

## ESM för programmerare {#esm-for-programmers}

### Programmerare kan övervaka följande mätvärden: {#programmers-monitor-metrics}


| *Måttnamn* | *Beskrivning* |
|-------------------------|--------------------------|
| autentiseringsförsök | Antal initierade autentiseringsflöden |
| lyckad | Antal autentiseringstoken som har hämtats av klienter |
| författarväntande | Antal autentiseringstoken som har genererats (oavsett om klienten har fått den eller inte) |
| authn-failed | Antal misslyckade autentiseringar som utförts via ett externt system. |
| clientless-tokens | Antal klientlösa token som har skickats |
| klientlösa fel | Antal misslyckade försök att ta emot tokens från klientlöst API |
| authz-try | Antal försök till auktorisering |
| authz-success | Antal godkända auktoriseringar |
| authz-failed | Antal nekade auktoriseringar av sidoskyddsprogram på applikationsnivå |
| authz-jected | Antal försök till auktorisering som anses vara skadliga av Adobe Service Provider och som avvisats som ett resultat av DoS-attacker |
| authz-latency | Totalt antal millisekunder som har ägnats åt MVPD-slutpunkten |
| media-tokens | Antal genererade korta medietoken (som motsvarar antalet uppspelningsbegäranden) |
| unika konton | Antal unika användare som har utfört berättigandeåtgärder (AuthN/AuthZ) i det valda tidsintervallet. (Det här måttet visas bara om dagliga värden har begärts.) </br> Detta beräknas för varje enskilt datacenter. När dc-dimensionen inte begärs visas inte det här måttet. |
| unika sessioner | Antal unika sessioner som har utfört autentiseringsflödesanrop till tjänsten Adobe Pass Authentication inom det valda tidsintervallet. (Det här måttet visas bara om dagliga värden har begärts.) </br> Detta beräknas för varje enskilt datacenter. När dc-dimensionen inte begärs visas inte det här måttet. |
| antal | En enkel räknare som används i händelseorienterade rapporter |

</br>

### Programmerare kan filtrera mätvärdena som listas ovan efter följande mått: {#progr-filter-metrics}


| *Dimensionens namn* | *Beskrivning* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| år | Det fyrsiffriga året |
| månad | Månad på året (1-12) |
| dag | Dag i månaden (1-31) |
| timme | Timme på dagen |
| minut | Minut av timmen |
| medieföretag | Medieföretaget som äger webbplatsen som initierade berättigandeprocessen för användaren |
| dc | (Datacenter) Hemregionen där begäran togs emot. |
| proxy | Proxyservern MVPD (som kommer att vara&quot;Direct&quot; för direkta integreringar) |
| mvpd | MVPD som ansvarar för att bevilja behörighet till användaren |
| beställar-id | ID för begärande som används för att utföra berättigandebegäran |
| kanal | Kanalwebbplatsen, som har extraherats från resursfältet (extraherats från MRSS-nyttolasten som kanal/titel om sådan finns, eller mappats till resursvärdet om det inte är i RSS-format). |
| resource-id | Den faktiska resurstitel som ingår i auktoriseringsbegäran (extraherad från MRSS-nyttolasten som artikel/titel om sådan finns) |
| enhet | Enhetsplattformen (dator, mobil, konsol osv.) |
| EAP | Den externa autentiseringsprovidern när autentiseringsflödet utförs via ett externt system. </br> Värdena kan vara: </br> - Ej tillämpligt - Autentiseringen tillhandahölls av Adobe Pass Authentication </br> - Apple - det externa system som tillhandahöll autentiseringen är Apple |
| os-family | Operativsystem som körs på enheten |
| webbläsarfamiljen | Användaragent som används för åtkomst till Adobe Pass-autentisering |
| cdt | Enhetsplattformen (alternativ) som för närvarande används för klientlösa. </br> Värdena kan vara: </br> - Ej tillämpbart - händelsen kom inte från en klientlös SDK </br> - Okänd - Eftersom parametern deviceType från en klientlös API är valfri finns det anrop som inte innehåller något värde. </br> - alla andra värden som skickades via det klientlösa API:t, t.ex. xbox, appletv, roku osv. </br> |
| platform-version | Den version av SDK utan klient som |
| os-type | Operativsystem som körs på enheten, alternativ (används inte för närvarande) |
| webbläsarversion | Version av användaragent |
| nsdk | Klienten SDK använde (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-version | Versionen av Adobe Pass Authentication-klienten SDK |
| event | Händelsenamnet för Adobe Pass-autentisering |
| orsak | Orsaken till fel enligt Adobe Pass-autentisering |
| sso-type | Den underliggande SSO-mekanismen: platform/passive/adobe. Anger att auktoriseringstoken utfärdades genom att AuthN återanvändes i ett annat program |
| plattform | Enheten identifierade plattformen. Möjliga värden: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - osv. |
| application-name | Programnamnet som konfigurerats i TVE Dashboard för det DCR-registrerade programmet som ska användas. |
| application-version | Programversionen som konfigurerats i TVE Dashboard för det DCR-registrerade programmet som är konfigurerat att användas. |
| kundapp | Det anpassade program-ID som skickades via [enhetsinformation](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| innehållskategori | Kategorin för det innehåll som begärts av ditt program. |

## ESM för MVPD {#esm-for-mvpds}

### MVPD-program kan övervaka följande mått:

| *Måttnamn* | *Beskrivning* |
|---|---|
| autentiseringsförsök | Antal initierade autentiseringsflöden |
| lyckad | Antal autentiseringstoken som har hämtats av klienter |
| författarväntande | Antal autentiseringstoken som har genererats (oavsett om klienten har fått den eller inte) |
| authn-failed | Antal misslyckade autentiseringar som utförts via ett externt system. |
| authz-try | Antal försök till auktorisering |
| authz-success | Antal godkända auktoriseringar |
| authz-failed | Antal nekade auktoriseringar av sidoskyddsprogram på applikationsnivå |
| authz-jected | Antal försök till auktorisering som anses vara skadliga av Adobe Service Provider och som avvisats som ett resultat av DoS-attacker |
| authz-latency | Totalt antal millisekunder som har ägnats åt MVPD-slutpunkten |

### MVPD-program kan filtrera mätvärdena som anges ovan med följande mått:

| *Dimensionens namn* | *Beskrivning* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| år | Det fyrsiffriga året |
| månad | Månad på året (1-12) |
| dag | Dag i månaden (1-31) |
| timme | Timme på dagen |
| minut | Minut av timmen |
| mvpd | Det mvpd-ID som används för att utföra berättigandebegäran |
| beställar-id | ID för begärande som används för att utföra berättigandebegäran |
| EAP | Den externa autentiseringsprovidern när autentiseringsflödet utförs via ett externt system. </br> Värdena kan vara: </br> - Ej tillämpligt - Autentiseringen tillhandahölls av Adobe Pass Authentication </br> - Apple - det externa system som tillhandahöll autentiseringen är Apple |
| cdt | Enhetsplattformen (alternativ) som för närvarande används för klientlösa. </br> Värdena kan vara: </br> - Ej tillämpbart - händelsen kom inte från en klientlös SDK </br> - Okänd - Eftersom parametern deviceType från en klientlös API är valfri finns det anrop som inte innehåller något värde. </br> - alla andra värden som skickades via det klientlösa API:t, t.ex. xbox, appletv, roku osv. </br> |
| sdk-typ | Klienten SDK använde (Flash, HTML 5, Android native, iOS, Clientless etc.) |
| plattform | Enheten identifierade plattformen. Möjliga värden: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - osv. |
| nsdk | Klienten SDK använde (android, fireTV, js, iOS, tvOS, non-sdk) |
| nsdk-version | Versionen av Adobe Pass Authentication-klienten SDK |

## Användningsexempel {#use-cases}

Du kan använda ESM-data för följande användningsområden:

- **Övervakning** - Ops- eller övervakningsteam kan skapa en instrumentpanel eller ett diagram som anropar API:t varje minut. Med hjälp av den information som visas kan de upptäcka ett problem (med Adobe Pass-autentisering eller med en MVPD) så fort det visas.

- **Felsökning/kvalitetstestning** - Eftersom data också delas upp efter plattform, enhet, webbläsare och operativsystem kan analysmönster identifiera problem med specifika kombinationer (t.ex. Safari i OSX).

- **Analytics** - De data som tillhandahålls kan användas för att komplettera/granska data på klientsidan som samlas in via Adobe Analytics eller något annat analysverktyg.

<!--
## Related Information {#related-information}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
