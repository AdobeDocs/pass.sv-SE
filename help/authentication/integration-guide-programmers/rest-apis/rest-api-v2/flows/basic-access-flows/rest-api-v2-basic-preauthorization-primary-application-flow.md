---
title: Grundläggande förauktorisering - primärt program - flöde
description: REST API V2 - grundläggande förauktorisering - primärt program - flöde
exl-id: f557f6c3-d5b2-4ec8-be51-91a90fbd31c0
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# Grundläggande förauktoriseringsflöde som utförs i det primära programmet {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

**Förhandsauktoriseringsflödet** inom Adobe Pass-autentiseringsberättigandet gör att direktuppspelningsprogrammet kan avgöra om en MVPD kan tillåta eller neka användaren åtkomst till en lista med resurser. Verifieringen säkerställer att programmet kan presentera korrekt information för användaren om det innehåll som han/hon kan visa.

## Hämta förauktoriseringsbeslut med hjälp av en specifik mvpd {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Förutsättningar {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Innan du hämtar beslut om förauktorisering med en viss MVPD måste du kontrollera att följande krav är uppfyllda:

* Strömningsprogrammet måste ha en giltig vanlig profil som har skapats för MVPD med något av de grundläggande autentiseringsflödena:
   * [Utför autentisering i det primära programmet](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Utför autentisering i det sekundära programmet med förvald mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Utför autentisering i det sekundära programmet utan förvald mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
* Strömningsprogrammet vill hämta beslut om förauktorisering för att visa en lista över resurser tillsammans med deras associerade status.

### Arbetsflöde {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Följ de angivna stegen för att implementera det grundläggande förauktoriseringsflödet med en specifik MVPD som utförs i ett primärt program enligt bilden nedan.

![Hämta förauktoriseringsbeslut med hjälp av specifik mvpd](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*Hämta förauktoriseringsbeslut med hjälp av specifik mvpd*

1. **Hämta förauktoriseringsbeslut:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att erhålla förauktoriseringsbeslut för en lista med resurser genom att anropa slutpunkten för förauktorisering av beslut.

   >[!IMPORTANT]
   >
   > Se [Hämta beslut om förhandsauktorisering med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API-dokumentation för mer information om:
   >
   > * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `resources`
   > * Alla _obligatoriska_ rubriker, som `Authorization` och `AP-Device-Identifier`
   > * Alla _valfria_ parametrar och rubriker

1. **Hitta en vanlig profil:** Adobe Pass-servern identifierar en giltig profil baserat på mottagna parametrar och rubriker.

1. **Hämta MVPD-beslut för begärda resurser:** Adobe Pass-servern anropar MVPD-förauktoriseringsslutpunkten för att erhålla ett `Permit` - eller `Deny`-beslut för varje resurs som tagits emot från direktuppspelningsprogrammet.

1. **Returbeslut om förauktorisering:** Slutpunktssvaret för förauktorisering av beslut innehåller ett `Permit` - eller `Deny`-beslut för varje resurs:
   * Ett `Permit`-beslut betyder att resursen kan spelas upp. Svaret innehåller ingen medietoken eftersom förauktoriseringsflödet inte får användas för att spela upp resurser.
   * Ett `Deny`-beslut betyder att resursen inte kan spelas upp. Svaret innehåller en felnyttolast som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Mer information om vilken information som ges i ett beslutssvar finns i [Hämta förhandsauktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API-dokumentation.
   > 
   > <br/>
   > 
   > Slutpunkten för förauktorisering av beslut validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Hantera förauktoriseringsbeslut:** Direktuppspelningsprogrammet bearbetar svaret och kan använda det för att visa lämplig status för varje resurs i användargränssnittet.
