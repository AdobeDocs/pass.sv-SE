---
title: Integrera data på serversidan för Adobe Pass Authentication i Adobe Analytics
description: Integrera data på serversidan för Adobe Pass Authentication i Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Integrera data på serversidan för Adobe Pass Authentication i Adobe Analytics

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Kunder som har Adobe Pass Authentication vill se serverdata på Adobe Pass Authentication (Adobe Pass) i Adobe Analytics Dashboard för enklare konsumtion.

Dessa data används för att spåra viktiga TVE-mått som konverteringsgrader per MVPD, unika användare baserat på MVPD-användar-ID med mera.

Den är inte avsedd att ersätta en implementering på klientsidan om det redan finns en implementering eftersom användaraktiviteten inte kan spåras utöver de specifika händelserna nedan om det inte finns något besökar-ID. Om kunderna tillhandahåller ett besökar-ID för Pass-samtal kan vi låsa upp en annan typ av Analytics-integrering - i realtid - som kan koppla alla Pass-händelser till befintliga kunddata, mer information om den nya typen av möjlig integrering här: [Använda Experience Cloud-ID i Adobe Pass-autentisering](/help/authentication/integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)

## Mätvärden ingår {#metrics-included-int-authn-analyt}

| Händelse | Beskrivning |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| AuthN begärdes | Antal initierade autentiseringsflöden |
| AuthN väntar | Antal autentiseringstoken som har genererats (oavsett om klienten har fått den eller inte) |
| AuthN OK | Antal autentiseringstoken som har hämtats av användare |
| AuthZ begärdes | Antal försök till auktorisering |
| AuthZ OK | Antal godkända auktoriseringar |
| AuthZ misslyckades | Antal nekade auktoriseringar av sidoskyddsprogram på applikationsnivå |
| Spela upp begäran | Antal genererade korta medietoken (som motsvarar antalet uppspelningsbegäranden) |
| Utloggning begärd | Antal initierade utloggningsflöden |
| Utloggningen är klar | Antal slutförda utloggningsflöden |
| Utloggningen misslyckades | Antal misslyckade utloggningsflöden |
| Förauktorisering begärdes | Antal initierade förauktoriseringsflöden |
| Förhandsauktorisering OK | Antal lyckade förauktoriseringshändelser med resurser som förauktoriserats |
| Förauktorisering nekad | Antal förauktoriseringshändelser med resurser som nekats förauktorisering |
| Förhandsauktoriseringen misslyckades | Antal misslyckade förauktoriseringshändelser |

| Adobe Analytics Name | Beskrivning |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kanal | ID för begärande som används för att utföra berättigandebegäran |
| MVPD | Det huvuddokument som ansvarar för att bevilja behörighet till användaren |
| Proxy | Proxyvariabeln MVPD (som kommer att vara &quot;Direkt&quot; för direkta integreringar) |
| SDK-typ | Klient-SDK används (Flash, HTML5, Android native, iOS, Clientless etc.) |
| SDK-version | Versionen av Adobe Pass Authentication Client SDK |
| Resurs-ID | Den faktiska resurstitel som ingår i auktoriseringsbegäran (extraherad från MRSS-nyttolasten som artikel/titel om sådan finns) |
| AuthZ-feltyp | Orsaken till fel, enligt Adobe Pass-autentisering <br/>, är de vanligaste värdena <br/> **noAuthZ** = MVPD svarade att användaren inte har kanalen i sitt paket<br/> **network** = det gick inte att nå MVPD (MVPD har ett problem vid tidpunkten för anropet och svarade inte)<br/> **norefreshtoken** = detta gäller endast för OAuth-implementeringar och det kan inträffa om användaren ändrar sitt lösenord eller MVPD av någon anledning nekade det. Det resulterar vanligtvis i en ny autentisering <br/> **matchar inte** = om begäran görs från en annan enhet än den som hade autentiseringstoken. Kan leda till att användare försöker lura systemet, men de flesta av dessa hände i samband med vårt gamla JavaScript SDK där enhets-ID använde IP-adressen som en del av beräkningen. Om en användare tittade på TVE hemma och sedan på jobbet utlöstes det här felet och måste autentisera igen<br/> **invalid** = ogiltig begäran, saknade eller ogiltiga parametrar<br/>  **authzNone** = Programmerare kan neka auktoriseringar för en viss channelMVPD-kombination. Detta utlöses av ett backend-API som programmerare har åtkomst till <br/> **Bedrägeri** = det är en skyddsmekanism på vår sida. Om användaren misslyckas med auktoriseringen och sedan begär det igen ett antal gånger i ett kort intervall (sekunder), nekar vi anropet direkt. Det händer vanligtvis när en programmerare har ett fel i implementeringen som frågar efter auktorisering hela tiden om det misslyckas. |
| Tokentyp | När tokens skapas på grund av AuthZ Alla och AuthN Alla, måste vi veta vad som orsakas av ett nedbrytningsmått.<br/> De är:<br/> &quot;normal&quot; = Normalt fall <br/> &quot;authall&quot; = När AuthN All är aktiverad<br/> &quot;authzall&quot; = När AuthZ Alla är aktiverat<br/> &quot;hba&quot; = När HBA är aktiverat |
| Typ av klientlös enhet | Enhetsplattformen (alternativ) som för närvarande används för klientlösa.<br/> Värdena kan vara:<br/> N/A - händelsen kom inte från en klientlös SDK<br/> Okänd - Eftersom parametern deviceType från en **klientlös API** är valfri finns det anrop som inte innehåller något värde.<br/> Alla andra värden som skickades via **klientlöst API**. Till exempel xbox, appletv och roku. |
| MVPD-användar-ID | Ersätter cookie-baserat besökar-ID |


## Information {#details-int-authn-analyt}

* Måtten infogas, händelse för händelse i den specifika rapportsviten via Adobe Analytics Data Insertion API
* Infogningen grupperas och skickas var 30:e minut. På grund av detta måste rapporten tidsstämplas
* Varje kund har en eller flera rapportsviter. Ett begärande-ID (kanal) mappas endast till en rapportserie. Flera begärande-ID:n kan bara mappas till en rapportserie.
* Historiska data kan tillhandahållas, men särskild försiktighet måste vidtas på grund av trafik-/prestandaproblem.
* Den unika besökarvariabeln ställs in på användar-ID:t för MVPD
* Mappningen av händelser och eVars kan inte konfigureras.


## SLA {#sla-int-authn-serv-anal}

Eftersom det inte är en viktig komponent finns det ingen SLA-garanti för integreringen.

## Rapportsvitens konfiguration {#report-suite-config}

Rapporten måste tidsstämplas eftersom händelserna skickas gruppvis.

### Händelser {#report-suite-config-events}


>[!NOTE]
>Alla ska anges med:
>
>* Räknare (inga underrelationer)

| Händelse | Adobe Analytics event |
|---------------------------------------|-----------------------|
| AuthN begärdes | event1 |
| AuthN väntar | event2 |
| AuthN OK | event3 |
| AuthZ begärdes | event4 |
| AuthZ OK | event5 |
| AuthZ misslyckades | event6 |
| Spela upp begäran | event7 |
| AuthN misslyckades | event8 |
| AuthN-tokenbegäran utan klient OK | event9 |
| Begäran om AuthN-token utan klient misslyckades | event10 |
| Uppspelningsbegäran misslyckades | event11 |
| Utloggningsbegäran | event12 |
| Utloggningen är klar | event13 |
| Utloggningen misslyckades | event14 |
| Preflight-förfrågan | event15 |
| Preflight misslyckades | event16 |
| Preflight beviljad | event17 |
| Preflight nekad | event18 |


### eVars {#evars}


>[!NOTE]
>Alla ska anges med:
>
>* Allokering: Senaste (senaste)
>* Förfaller efter: träff
>* Typ: Textsträng

| Egenskap | eVar |
|-----------------------------------|--------------------------------|
| Kanal | eVar1 |
| MVPD | eVar2 |
| Proxy | eVar3 |
| SDK-typ | eVar4 |
| SDK-version | eVar5 |
| Resurs-ID | eVar6 |
| AuthZ-feltyp | eVar7 |
| Tokentyp | eVar8 |
| Typ av klientlös enhet | eVar9 |
| MVPD-användar-ID | visitorID (klart automatiskt) |
| MVPD-användar-ID | eVar10 |
| Enhetstyp | eVar11 |
| Operativsystem | eVar12 |
| Primär maskinvarutyp | eVar13 |
| TTL | eVar14 |
| Autentiseringstyp | eVar15 |
| Version av serverarkitektur | eVar16 |
| Extern autentiseringsprovider | eVar17 |
| Latens | eVar18 |
| Besökar-ID | eVar19 |
| SSO-mekanism | eVar20 |
| Enhetsmodell | eVar21 |
| Enhetsversion | eVar22 |
| Enhetens maskinvarumodell | eVar23 |
| Leverantör av maskinvara | eVar24 |
| Maskinvarutillverkare | eVar25 |
| Enhetens maskinvaruversion | eVar26 |
| Enhetens operativsystemnamn | eVar27 |
| Enhetens OS-familj | eVar28 |
| Operativsystemsleverantör för enhet | eVar29 |
| OS-version för enhet | eVar30 |
| Enhetsläsarens namn | eVar31 |
| Leverantör av enhetsläsare | eVar32 |
| Enhetsläsarversion | eVar33 |
| Normaliserad, klientlös enhetstyp | eVar34 |

## Priser {#pricing}

Kontakta din TAM om du vill ha mer information.
