---
title: Kampanjtillfälligt pass
description: Kampanjtillfälligt pass
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 0%

---

# Kampanjtillfälligt pass {#promotional-temp-pass}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Sammanfattning av funktioner {#feature-summary}

Med ett tillfälligt kampanjpass kan programmerare ge tillfälligt åtkomst till skyddat innehåll för användare som inte har kontoinloggningsuppgifter med ett separat programmeringsfönster.

Temporärt kampanjpass är avsett att användas för att köra kampanjkampanjer där en användare, efter att ha angett giltig identifieringsinformation (till exempel e-postadress) till programmeraren, kan förbruka ett **fördefinierat antal olika VOD-titlar under en fördefinierad tidsperiod**.

>[!IMPORTANT]
>
>Adobe lagrar ingen personligt identifierbar information (PII). Därför måste programmeraren ställa in en hash över den unika användarinformationen i Adobe Pass Authentication API:er.

Kampanjtillfälligt pass är byggt ovanpå funktionen [Tillfälligt pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md), vilket innebär att det innehåller alla funktioner för tillfälligt pass.

När det maximala antalet fördefinierade VOD-titlar eller den fördefinierade tidsperioden har överskridits kommer användaren inte att kunna komma åt innehåll på samma enhet eller genom att använda samma användaridentifierarinformation (till exempel e-postadress) förrän auktoriseringstoken har rensats från Adobe Pass autentiseringsserver.

>[!NOTE]
>
>Temporärt pass är en del av Premium Workflow-paketet. Kontakta din Adobe Pass-säljare om du är intresserad av att använda den här funktionen.

## Jämförelse av tillfälligt pass och kampanjtillfälligt pass {#tp-ptp-comparison}

| Temporärt pass | Kampanjtillfälligt pass |
|----------------------------------|----------------------------------------------------------------------------------------|
| Åtkomst till innehåll <ul><li>tidsbaserad</li></ul> | Åtkomst till innehåll <ul><li>tidsbaserad</li><li>baserat på antalet resurser</li></ul> |
| Åtkomstsäkerhet baserad på <ul><li>enhets-ID</li></ul> | Säkerhet baserad på <ul><li>enhets-ID</li><li>hash over-given användaridentifierarinformation (t.ex. e-post)</li></ul> |
| Klientfel-API finns | Klientfel-API finns |
| Återställning/rensning är tillgängligt | Återställning/rensning är tillgängligt |

## Funktionsinformation {#feature-details}

Med den här funktionen kan användare komma åt reklaminnehåll från en viss enhet (telefon och surfplatta) efter att ha angett unik information, till exempel e-postadressen, i Programmerarens program.

Programmeraren kommer att ge en hash över användarens PII-kod för autentiserings- och auktoriserings-API:er. Den här hashen kommer att användas tillsammans med enhets-ID när en unik nyckel skapas som identifierar användaren och enheten.

Baserat på enhets-ID och den information som användaren angett och enligt logiken nedan avgör Adobe Pass Authentication om användaren befinner sig i en ny testversion eller i en befintlig:

* ny hash over användartillhandahållen information (t.ex. e-post), nytt enhets-ID => ny testversion
* befintlig hash-over-användartillhandahållen information (t.ex. e-post), nytt enhets-ID => befintlig testversion (med befintlig hash-over-användarinformation (t.ex. e-post))
* ny hash over användartillhandahållen information (t.ex. e-post), befintligt enhets-ID => befintlig testversion (med befintligt enhets-ID)
* befintlig hash-over-användartillhandahållen information (t.ex. e-post), befintligt enhets-ID => befintlig testversion

>[!NOTE]
>Validering och hashning av användarinformationen hanteras av Programmer, inte av Adobe.

**Funktionen Kampanjtillfälligt pass kan konfigureras baserat på följande egenskaper:**

* Användardefinierad informationsnyckel (t.ex. e-post)
* Antal resurser som användaren har rätt att använda
* TTL - tidsintervallet som användaren har rätt att använda det konfigurerade antalet resurser

### Användarmetadata {#user-metadata}

För att underlätta implementeringen av programmerarens program visas följande **användarmetadatainformation** i det tillfälliga kampanjpasset, med motsvarande nycklar (för att aktivera nycklarna, kontakta tve-support@adobe.com):

* **remain_resources**: antalet återstående resurser som den aktuella användaren har rätt att använda
* **used_assets**: listan med resurser som den aktuella användaren redan har förbrukat
* **expiration_date**: den aktuella användarens förfallodatum

### Hur ser visningstiden ut? {#compute-viewing-time}

Den tid som ett tillfälligt pass är giltigt motsvarar inte den tid en användare tillbringar med att visa innehåll i programmerarens program. Efter den första användarbegäran om auktorisering via tillfälligt kampanjpass beräknas en förfallotid genom att den inledande aktuella begärandetiden läggs till i TTL-värdet (tidsintervall för varaktighet) som anges av programmeraren.

### Autentisering och auktorisering {#authn-authz}

Autentisering och auktorisering kommunicerar inte med ett faktiskt MVPD för kampanjflöden för tillfälligt pass, **så alla auktoriseringsbegäranden lyckas** så länge som alla dessa villkor uppfylls:

* verifieringstoken är giltiga för angivna resurser
* antalet **used_assets** är lägre än gränsen som angetts av programmeraren
* Värdet **expiration_date** är efter aktuellt datum.

### Preflight-beteende {#preflight-beh}

När en preflight- eller preauktoriseringsbegäran görs för ett Kampanjtillfälligt pass-MVPD, kommer motsvarande preflight-svar som returneras att innehålla hela listan med resurser från Preflight-begäran som preflight-godkänd.

Logiken bakom detta är: Villkoren för godkännande av kampanjtillfälligt pass baseras på tids- och resursnummergränsen i stället för på specifika resurser. Om tidsbegränsningen uppfylls och resursgränsen inte överskrids, kommer de anropade resurserna att auktoriseras.

### SSO {#sso}

SSO är inte aktiverat för instanser av kampanjtillfälligt pass, som har konfigurerats med alternativet Autentisering per begärande aktiverat. Det innebär att användaren inte loggas in automatiskt när användaren växlar från program A till ett annat program B som båda är integrerade med samma tillfälliga kampanjpass.

### Utloggning {#logout}

Alla token på en enhet tas bort vid utloggning. Därför bör en övergång från Kampanjtillfälligt pass till ett vanligt MVPD som väljs av användaren inte vara beroende av den här implementeringen. Rekommendationen är att använda funktionen `setSelectedProvider(null)` för att rensa programtillståndet och sedan starta om autentiseringsflödet, vilket ger en bättre användarupplevelse.

### Flödesdiagram för tillfälligt kampanjpass {#promo-tempass-flowdia}

![Flödesdiagram för tillfälligt kampanjpass](../../../assets/promo-temp-pass-flow.png)

*Bild: Kampanjflöde för tillfälligt pass*

## Implementera tillfälligt kampanjpass {#impl-promo-tempass}

Kampanjtillfälligt pass kräver följande funktioner på klientsidan:

* **Information om användaridentifierare, till exempel spridning av e-postadress** (skickar användarens e-postadress i autentiserings- och auktoriseringsflödena). E-postmeddelandet krävs av Adobe Pass-autentiseringen för att binda autentiseringstoken och auktoriseringstoken (liknande fallet för `device_ID`, som krävs för alla anrop).
* **Tvinga autentisering** - Programmeraren kan tvinga fram ett autentiseringsflöde när användaren redan är autentiserad. Den här funktionen krävs för att tvinga en uppdatering av användarens metadata (användarens metadatanyckel **used_assets** innehåller antalet tillgängliga resurser) varje gång appen startas. Eftersom användaren kan logga in på flera enheter är användarens metadata på enheten inte tillförlitliga när appen startas, och vi måste uppdatera dem för att återspegla den aktuella statusen för den specifika användaren (identifieras med e-postadress).


>[!IMPORTANT]
>Autentisering med tvång är bara möjligt på iOS och Android.
>Adobe Pass Authentication har ingen inbyggd mekanism som stoppar den kostnadsfria strömningen efter att X-minuterna har passerat. Adobe Pass-autentiseringen upphör att utfärda **auktoriserings**- och **kortmedietoken** när användaren förbrukar Y-resurserna. Det är programmerare som ska begränsa åtkomsten när kampanjens tillfälliga pass upphör att gälla.

## Säkerhet {#security}

>[!IMPORTANT]
>Adobe lagrar ingen personligt identifierbar information (PII). Därför måste programmeraren ställa in en hash över den unika användarinformationen i Adobe Pass Authentication API:er.

**Hashning av användaridentifierarinformation**

Adobe rekommenderar att du använder **SHA-2**-familjen eller dess specifika **SHA-256** -, **SHA-512** -funktioner på data innan de skickas till Adobe.

Exempel: **SHA-256** over **&quot;user@domain.com&quot;** is **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a33 32b18b88d09069fb7&quot;**.

## Återställ eller rensa tillfälligt kampanjpass {#reset-promo-tempass}

Vissa affärsregler kräver en regelbunden rensning av kampanjens tillfälliga pass. Adobe Pass Authentication förser programmerare med ett *public*-webb-API, vilket beskrivs nedan:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protokoll: **https**</li><li>Värd:<ul><li>Utgåva: **mgmt.auth.adobe.com**</li><li>Prequal: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Sökväg: **/reset-tempass/v2/reset**</li><li>Frågeparametrar: **device_id=all&amp;request_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>headers: ApiKey: **1232293681726481**</li> <li>Svar:<ul><li>Slutfört: **HTTP 204**</li><li>Fel: **HTTP 400** för felaktiga begäranden, **HTTP 401** om ApiKey inte anges, **HTTP 403** om ApiKey är ogiltigt</li></ul></li></ul> |

Utöver kraven för att tömma tillfälligt pass använder det tillfälliga kampanjpasset hash-informationen för användaridentifieraren som skickades som **generisk_data** vid autentisering och auktorisering för rensning.

Hashen skickas, inte hela JSON:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Klienter som stöds {#supported-clients}

| Adobe Pass-autentiseringsklienter | Kampanjtillfälligt pass | Återställ verktyg | Stöder dedikerad svarskod/klientfel |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS Access Enabler | JA | JA | JA (från v 3.0.0) |
| IOS för inbyggd klient | JA | JA | JA (från v 1.10) |
| Android för inbyggd klient | JA | JA | JA |
| Klientlöst API | JA | JA | NEJ |


## Begränsningar {#limitations}

I det här avsnittet beskrivs de begränsningar som gäller för den aktuella implementeringen av Kampanjtillfälligt pass.

### Klientlös {#lim-clientless}

**Smarta enheter utan ett unikt enhets-ID**

Alla smarta enhetsappar kan inte tillhandahålla ett unikt enhets-ID. Om det inte finns något kan Adobe Pass-autentiseringen använda det UUID som genererats av Adobe Registration Code Service som unikt enhets-ID. Detta innebär att när användaren loggar ut tas autentiserings- och auktoriseringstoken bort. När användaren försöker autentisera igen kan användaren autentisera igen med annan användarinformation (t.ex. e-post). Adobe rekommenderar att du lägger till ett gränssnittsflöde som inte tillåter användaren att &quot;lura&quot; systemet och lägger till logik för att avgöra om det är en ny användare som begär en testversion eller en befintlig testversion.

**Återställer/rensar tillfälligt pass**

Återställ tillfälligt pass för klientlösa är inte tillgängligt i de specifika fallen för Xbox360 och Xbox One, eftersom dessa plattformar kräver ytterligare parsning av enhets-ID, vilket inte är möjligt i verktyget Återställ tillfälligt pass.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
