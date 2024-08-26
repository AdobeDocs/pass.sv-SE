---
title: Översikt över API-nedgradering
description: Översikt över API-nedgradering
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---


# Översikt över API-nedgradering {#degradation-api-overview}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Innan du använder API:t för degradering måste du kontrollera att följande krav är uppfyllda:
>
> * Hämta klientautentiseringsuppgifterna enligt beskrivningen i API-dokumentationen för [Hämta klientautentiseringsuppgifter](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Hämta åtkomsttoken enligt beskrivningen i API-dokumentationen för [Hämta åtkomsttoken](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Mer information om hur du skapar ett registrerat program och hämtar programsatsen finns i [Översikt över registrering av dynamiska klienter](./dcr-api/dynamic-client-registration-overview.md) .

## Allmän information {#general_info}

>[!NOTE]
>
>Detta API är inte allmänt tillgängligt. Kontakta din Adobe-representant för att få uppdateringar om tillgänglighet.

Den här funktionen ger alla tre parter i en integrering (programmerare, distributörer av videoprogrammeringstjänster och Adobe) möjlighet att tillfälligt kringgå specifika slutpunkter för MVPD-autentisering och -auktorisering. Vanligtvis är det Programmeraren som initierar en sådan åtgärd, men oavsett vem som utlöser en nedbrytningshändelse beror åtgärden på hur man tidigare kommit överens om arrangemang med de berörda PDF-filerna.

Det viktigaste användningsexemplet för den här funktionen är live-sporter eller stora event. I sådana höga trafikscenarier är det möjligt att belastningen på en specifik MVPD-slutpunkt blir för hög, vilket ger mycket långa svarstider för användarna. För att bevara en bra användarupplevelse under ett sådant scenario kan programmeraren besluta att utlösa en försämringsregel som tillfälligt kan autenticera/autoauktorisera användare, eller inaktivera ett MVPD genom att ta bort det från listan över tillgängliga MVPD.

En försämringsregel tillämpas bara under en fast tidsperiod. Även om de primära kunderna för den här funktionen är sportkanaler och dynamiska nyhetskanaler, kan alla programmerare vilja ha tillgång till den här funktionen, eftersom programmerartjänsterna då och då är desamma.

Försämringsinformation:

- Den här funktionen är avsedd att användas tillsammans med API:t för användningsövervakning, som ger realtidsinformation om antalet autentiseringar och auktoriseringar per MVPD, genomsnittlig auktoriseringsfördröjning och andra mått som behövs för en fullständig serviceöversikt.
- Den här funktionen tillåter inte att autentiseringstjänsten Adobe Primetime kringgås. Om Adobe Pass Authentication finns nere finns det ingen mekanism i tjänsten som kan användas för att tillåta användare att se innehåll. Webbplatserna eller apparna kan dock själva dirigera runt Adobe Pass Authentication.
- Adobe kommer för närvarande inte att utlösa någon direkt försämring - beslutet måste alltid fattas med en särskild programmerare som har samtyckt till sådana villkor med sidoskyddsprogram. I framtiden kan Adobe Pass Authentication vara proaktivt för att utlösa regler för försämring om avtal (SLA-skydd) kan nås med sidoskyddsprogram.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
