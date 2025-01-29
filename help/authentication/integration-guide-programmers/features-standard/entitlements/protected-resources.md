---
title: Skyddade resurser
description: Skyddade resurser
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Skyddade resurser {#protected-resources}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Skyddade resurser representerar innehåll som användare kan strömma och identifieras av unika värden som upprättats genom avtal mellan distributörer av videoprogrammeringstjänster och deltagande programmerare.

Skyddade resurser följer en hierarkisk trädstruktur, där varje nivå ger större detaljrikedom vid innehållsauktorisering:

* Nätverk
   * Kanal
      * Visa
         * Episod
            * Tillgång

## Identifierare för skyddade resurser {#identifiers}

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

## REST API V2 {#rest-api-v2}

Adobe Pass autentiserings-API:er som hämtar förauktoriserings- och auktoriseringsbeslut kräver att klientprogrammet inkluderar identifierarna för skyddade resurser som parametrar.

Du kan hämta beslut om förauktorisering och auktorisering med följande API:er:

* [Hämta förauktoriseringsbeslut med hjälp av en specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Se avsnitten **Request** och **Samples** i API:erna ovan för att förstå vilket format som krävs för att tillhandahålla de unika identifierarna för skyddade resurser.

De unika identifierarna är ogenomskinliga för Adobe Pass-autentisering eftersom de skickas direkt till MVPD. Om MVPD inte känner igen eller tolkar en resursidentifierare returneras ett fel till Adobe Pass Authentication, som sedan skickar tillbaka felet till klientprogrammet med hjälp av en [Förbättrad felkod](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

Mer information om hur och när ovanstående API:er ska integreras finns i följande dokument:

* [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
