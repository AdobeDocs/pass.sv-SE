---
title: Beslut
description: Beslut
exl-id: 1efd70af-8c1d-43c4-87fc-14488d42b23d
source-git-commit: a19f4fd40c9cd851a00f05f82adbabb85edd8422
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---

# Beslut {#decisions}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Beslut genereras av Adobe Pass-autentiseringen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) baserat på användarens MVPD-auktoriserings- eller förauktoriseringsfrågor och avgör om åtkomst till [skyddat innehåll](#protected-resources) beviljas eller nekas.

Det finns två typer av beslut, beroende på vilket API som anropas:

* [Förhandsauktoriseringsbeslut](#preauthorization-decisions) som är informativa beslut.
* [Auktoriseringsbeslut](#authorization-decisions) som är auktoritativa beslut.

## Förauktoriseringsbeslut {#preauthorization-decisions}

Förauktoriseringsbeslutet är ett informativt beslut som gör att klientprogrammet kan informeras om huruvida MVPD kan tillåta eller neka användarens åtkomst till en [skyddad resurs](#protected-resources).

Syftet med förhandsauktoriseringen (preflight-auktorisering) är att programmet ska kunna visa korrekt information om det innehåll som användaren kan ha rätt att visa. Detta uppnås genom att förbättra användargränssnittet med indikatorer, t.ex. låsta eller olåsta ikoner, för att återspegla åtkomststatusen.

>[!IMPORTANT]
>
> Förhandsauktoriseringsbeslutet får inte användas på ett auktoritativt sätt för att spela upp resurser, eftersom det är syftet med ett [auktoriseringsbeslut](#authorization-decisions).

Det är inte obligatoriskt att använda API:t för förhandsauktorisering. Klientprogrammet kan hoppa över detta om det vill visa en katalog med resurser utan någon filtrering.

Om klientprogrammet har för avsikt att använda den här funktionen är det viktigt att observera att beslut om förauktorisering bara kan erhållas för ett begränsat antal resurser per API-begäran, vanligtvis upp till 5.

>[!IMPORTANT]
> 
> Det maximala antalet resurser kan bara ökas efter att ett avtal har träffats med representanterna för distributörer av videoprogrammeringstjänster och Adobe Pass-autentisering. När vi kommit överens om det kan ändringarna implementeras via Adobe Pass TVE Dashboard av en administratör i din organisation eller en representant för Adobe Pass Authentication som agerar för din räkning.
> 
> Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) .

MVPD-program kan ha stöd för förauktorisering via olika mekanismer, var och en med tydliga följder för prestanda och det maximala antalet resurser som kan hanteras i en enda API-begäran.

Mer information om de mekanismer som stöder förauktorisering finns i dokumentationen för [MVPD Preflight Authorization](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md).

>[!IMPORTANT]
>
> För distributörer av videoprogrammeringstjänster utan fullständigt preflight-auktoriseringsstöd måste förhandsauktoriseringen godkännas i förväg av representanter för distributörer av videoprogrammeringstjänster och Adobe Pass-autentisering, eftersom detta kan leda till prestandaproblem och långsammare svarstider.

## Beslut om tillstånd {#authorization-decisions}

Auktoriseringsbeslutet är ett auktoritativt beslut som gör att klientprogrammet följer MVPD beslut att tillåta eller neka användarens åtkomst till en [skyddad resurs](#protected-resources).

Syftet med auktoriseringen är att göra det möjligt för programmet att spela upp de resurser som begärts av användaren, efter rättighetsvalidering med MVPD och mottagande av en medietoken från Adobe Pass Authentication.

>[!IMPORTANT]
> 
> Adobe Pass Authentication rekommenderar att programmerare använder Media Token Verifier-biblioteket för att validera den medietoken som ingår i ett auktoriseringsbeslut och säkerställa säker åtkomst innan videoströmmen startas.
> 
> Mer information finns i dokumentationen för [Medietoken](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md).

Användningen av auktoriserings-API är obligatorisk, klientprogrammet kan inte hoppa över den här fasen om det vill spela upp resurser som användaren begär, eftersom det kräver verifiering med MVPD att användaren har rätt innan strömmen släpps.

Det är viktigt att komma ihåg att auktoriseringsbeslut bara kan erhållas för ett begränsat antal resurser per API-begäran, vanligtvis 1.

>[!IMPORTANT]
>
> Det maximala antalet resurser kan bara ökas efter att ett avtal har träffats med representanterna för distributörer av videoprogrammeringstjänster och Adobe Pass-autentisering.

## TTL-hantering (Time-to-Live) för auktorisering {#authorization-ttl-management}

TTL (Time-to-Live) för auktorisering anger hur länge en resurs förblir auktoriserad innan den behöver auktoriseras igen. Denna tidsram är begränsad och måste avtalas med MVPD representanter. TTL-värdena kan variera beroende på:

* Plattformskategori (t.ex. dator, mobil, tv-anslutna enheter)
* Specifik plattform (t.ex. iOS, Android, tvOS, Roku, FireTV)

TTL för auktorisering (authZ) kan visas och ändras via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

## Skyddade resurser {#protected-resources}

Skyddade resurser avser strömlinjeformat innehåll, som identifieras av unika värden som definieras genom avtal mellan distributörer av videoprogrammeringstjänster och deltagande programmerare.

Skyddade resurser följer en hierarkisk trädstruktur, där varje nivå ger större detaljrikedom vid innehållsauktorisering:

* Nätverk
   * Kanal
      * Visa
         * Episod
            * Tillgång

>[!IMPORTANT]
>
> Preauktorisering (preflight-auktorisering) fokuseras på resurser på kanalnivå med identifierare som har antingen en enkel sträng eller MRSS-format.
> 
> Vi rekommenderar inte att du använder resurser med identifierare som innehåller `CDATA` avsnitt vid förauktorisering, eftersom de i första hand används för resurser på tillgångsnivå som definieras av en MRSS.

### Resurs-ID {#resource-identifier}

Den unika identifieraren för resursen kan ha två format:

* Ett enkelt strängformat, till exempel en unik identifierare för en kanal (varumärke).
* Ett medie-RSS-format (MRSS) som innehåller ytterligare information som titel, gradering och metadata för föräldrakontroll.

När det gäller en enkel resursidentifierare, t.ex. &quot;REF30&quot; (antas representera en kanal), kan den översättas till en RSS-resursidentifierare enligt följande:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

Om resursidentifieraren är mer komplex kan den innehålla ytterligare klassificeringsinformation enligt följande:

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

De unika identifierarna är i första hand ogenomskinliga för Adobe Pass Authentication, men transformerare kan tillämpas baserat på MVPD funktioner och krav. Om MVPD inte känner igen eller tolkar en resursidentifierare returneras ett fel till Adobe Pass Authentication, som därefter skickar felet till klientprogrammet med hjälp av en [Förbättrad felkod](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

## REST API V2 {#rest-api-v2}

Förauktoriseringsbesluten kan hämtas med följande API:

* [Hämta förauktoriseringsbeslut med hjälp av en specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Behörighetsbesluten kan hämtas med följande API:

* [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Se avsnitten **Svar** och **Exempel** i ovanstående API:er för att förstå strukturen för beslut om förauktorisering och auktorisering.

Mer information om hur och när ovanstående API:er ska integreras finns i följande dokument:

* [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [Vanliga frågor om förauktoriseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)
> [Vanliga frågor om auktoriseringsfasen ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
