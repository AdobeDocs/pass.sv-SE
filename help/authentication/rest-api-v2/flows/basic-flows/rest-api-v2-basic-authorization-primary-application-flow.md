---
title: Grundläggande auktorisering - primärt program - flöde
description: REST API V2 - grundläggande auktorisering - primärt program - flöde
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 0%

---


# Grundläggande auktoriseringsflöde som utförs i primärt program {#basic-authorization-flow-performed-within-primary-application}

**Auktoriseringsflödet** inom Adobe Pass-autentiseringsberättigandet gör att direktuppspelningsprogrammet kan avgöra om ett MVPD tillåter eller nekar användarens begäran att direktuppspela innehåll. Om beslutet är `Permit` innehåller svaret en medietoken. Adobe Pass-servern signerar medietoken och tillåter att direktuppspelningsprogrammet använder medietokenkontrollerarbiblioteket för att kontrollera dess autenticitet innan strömmen släpps.

Verifieringen med kontrollerarbiblioteket för medietoken bör ske på den serverdelstjänst för direktuppspelade program som är länkad i behörighetskedjan för att frigöra en ström från CDN.

## Hämta auktoriseringsbeslut med hjälp av specifik mvpd {#retrieve-authorization-decisions-using-specific-mvpd}

### Förutsättningar {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Innan du hämtar auktoriseringsbeslut med hjälp av ett specifikt MVPD måste du kontrollera att följande krav är uppfyllda:

* Strömningsprogrammet måste ha en giltig vanlig profil som har skapats för MVPD med något av de grundläggande autentiseringsflödena:
   * [Utför autentisering i det primära programmet](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Utför autentisering i det sekundära programmet med förvald mvpd](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Utför autentisering i det sekundära programmet utan förvald mvpd](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* Strömningsprogrammet måste hämta ett auktoriseringsbeslut innan en användarvald resurs spelas upp.

### Arbetsflöde {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Följ de angivna stegen för att implementera det grundläggande auktoriseringsflödet med hjälp av ett specifikt MVPD som utförs i ett primärt program enligt bilden nedan.

![Hämta auktoriseringsbeslut med en specifik mvpd](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*Hämta auktoriseringsbeslut med en specifik mvpd*

1. **Hämta auktoriseringsbeslut:** Direktuppspelningsprogrammet samlar in alla nödvändiga data för att erhålla ett auktoriseringsbeslut för en specifik resurs genom att anropa auktoriseringsslutpunkten för beslut.

   Mer information om följande finns i [Hämta auktoriseringsbeslut med hjälp av specifik API-dokumentation för mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md):
   * Alla _obligatoriska_-parametrar, som `serviceProvider`, `mvpd` och `resources`
   * Alla _obligatoriska_ rubriker, som `Authorization` och `AP-Device-Identifier`
   * Alla _valfria_ parametrar och rubriker

1. **Hitta en vanlig profil:** Adobe Pass-servern identifierar en giltig profil baserat på mottagna parametrar och rubriker.

1. **Hämta MVPD-beslut för begärd resurs:** Adobe Pass-servern anropar MVPD-auktoriseringsslutpunkten för att erhålla ett `Permit` - eller `Deny`-beslut för den specifika resurs som tagits emot från direktuppspelningsprogrammet.

1. **Returbeslut `Permit` med medietoken:** Slutpunktssvaret för beslutsauktorisering innehåller ett `Permit`-beslut och en medietoken.

   Mer information om vilken information som ges i ett beslutssvar finns i [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API-dokumentation.

   >[!IMPORTANT]
   >
   > Slutpunkten för beslutsauktorisering validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Starta dataström med medietoken:** Direktuppspelningsprogrammet använder medietoken för att spela upp innehållet.

1. **Returbeslut `Deny` med information:** Slutpunktssvaret för beslut auktoriserar innehåller ett `Deny`-beslut och en felnyttolast som följer [Förbättrade felkoder](../../../enhanced-error-codes.md) -dokumentationen.

   Mer information om vilken information som ges i ett beslutssvar finns i [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API-dokumentation.

   >[!IMPORTANT]
   >
   > Slutpunkten för beslutsauktorisering validerar data för begäran för att säkerställa att de grundläggande villkoren uppfylls:
   >
   > * Parametrarna och rubrikerna _required_ måste vara giltiga.
   > * Integrationen mellan angiven `serviceProvider` och `mvpd` måste vara aktiv.
   >
   > <br/>
   > 
   > Om valideringen misslyckas genereras ett felsvar som ger ytterligare information som följer dokumentationen för [Förbättrade felkoder](../../../enhanced-error-codes.md).

1. **Hantera `Deny` beslutsinformation:** Direktuppspelningsprogrammet bearbetar felinformationen från svaret och kan använda den för att visa ett specifikt meddelande i användargränssnittet.
