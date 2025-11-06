---
title: TempPass-funktion
description: TempPass-funktion
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# TempPass-funktion {#temp-pass-feature}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

TempPass är en mångsidig funktion som ger programmerare möjlighet att ge tillfälligt åtkomst till det skyddade innehållet för användare utan giltiga MVPD-kontouppgifter. Det är ett effektivt verktyg för att engagera tittarna, oavsett om det handlar om grundläggande åtkomstscenarier eller riktade kampanjer.

TempPass är en kraftfull lösning för programmerare som vill:

* **Engagera tittare:** Erbjud en försmak av premiuminnehåll för att locka nya prenumeranter.
* **Kör kampanjer:** Kör riktade kampanjer för att öka exponeringen och skapa varumärkeslojalitet.
* **Behåll kontrollen:** Hantera åtkomstperioder, framtvinga begränsningar och återställ åtkomsten efter behov för att passa affärsmålen.

TempPass-funktionen levereras genom att en pseudo-MVPD (som kallas ytterligare&quot;Temp Pass&quot;) introduceras i Adobe Pass Authentication-serverkonfigurationen som en integrering med den deltagande programmeraren. TempPass-funktionen är tillgänglig i två konfigurationer:

* [Basic TempPass](#basic-temp-pass) för tidsbaserad åtkomst.
* [Befordrad TempPass](#promotional-temp-pass) för flexibel kampanjdriven åtkomst.

>[!IMPORTANT]
>
> TempPass-funktionen är en premiumfunktion och kräver en aktuell licens från Adobe.

Följande tabell innehåller en kort jämförelse av de grundläggande och kampanjmässiga TempPass-funktionerna:

| **Funktion** | **Basic TempPass** | **Befordrad TempPass** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **Åtkomst till innehåll** | <ul><li>Tidsbaserad</li></ul> | <ul><li>Tidsbaserad</li><li>Begränsat till ett maximalt antal resurser</li></ul> |
| **Åtkomstsäkerhet baserad på** | <ul><li>Enhets-ID</li></ul> | <ul><li>Enhets-ID</li><li>Hash med angiven användaridentifierarinformation (t.ex. e-post)</li></ul> |
| **Förbättrade felkoder** | Tillgänglig | Tillgänglig |
| **TempPass-återställningsfunktion** | Tillgänglig | Tillgänglig |

>[!IMPORTANT]
> 
> Adobe Pass Authentication innehåller inte någon inbyggd mekanism som automatiskt stoppar den pågående strömmen när den tilldelade tiden (X minuter) har gått ut. Programmerarna måste se till att åtkomstbegränsningarna efterlevs när TempPass upphör att gälla under en pågående ström.

Oavsett om du håller på att ta en smygtitt på ditt innehållsbibliotek eller marknadsför en markeringshändelse får du verktygen du behöver för att utöka din publik samtidigt som du behåller kontrollen över åtkomsten.

## Grundläggande TempPass {#basic-temp-pass}

Med den grundläggande TempPass-funktionen kan programmerare ge tidsbegränsad åtkomst till innehåll och ta hänsyn till olika scenarier:

* **Korta förhandsvisningar:** Erbjud korta förhandsgranskningar, t.ex. en 10-minuters daglig åtkomstperiod, för att locka potentiella prenumeranter.
* **Händelsebaserad åtkomst:** Aktivera längre åtkomst för större händelser, till exempel en 4-timmars session.
* **Kombinationsåtkomst:** Blanda och matcha varaktighet, till exempel en inledande utökad visningsperiod följt av kortare dagliga förhandsvisningar under flera dagar.

Vissa händelser kan kräva kostnadsfri tillgång till innehåll i olika faser, t.ex. en inledande, förlängd åtkomstperiod (t.ex. 4 timmar), följt av kortare, kostnadsfria åtkomstintervall (t.ex. 10 minuter varje dag). För att implementera detta scenario måste programmerarna samordna med sin Adobe-representant för att konfigurera två TempPass MVPD-program som är anpassade efter deras behov.

Om du till exempel vill ha en inledande 4-timmars kostnadsfri session följt av 10 minuters kostnadsfria sessioner kan Adobe konfigurera för Programmeraren:

* **TempPass1**: Konfigureras med en TTL (Time-To-Live) på 4 timmar för att täcka den inledande perioden för fri åtkomst.
* **TempPass2**: Konfigureras med en TTL (Time-To-Live) på 10 minuter för de följande dagliga kostnadsfria åtkomstintervallen.

TempPass2 måste återställas till 00:00 timmar varje dag för att säkerställa korrekt funktionalitet för daglig åtkomst.

### Funktionsinformation {#basic-temp-pass-feature-details}

**Konfigurationsparametrar:**

* **TTL (Time-To-Live):** Programmerare kan ange åtkomstvaraktighet. Den här klockbaserade TTL:n upphör att gälla oavsett visningstid.

**Användar-ID:**

Den grundläggande TempPass-funktionen använder enhetsidentifieraren som en användaridentifieringsparameter.

Följande tabell visar hur användaridentifieringsparametrarna påverkar användarens testupplevelse:

| Enhetsidentifierare | Resultat |
|-------------------|----------------|
| Nytt | Ny testversion |
| Befintlig | Befintlig testversion |

**Visa tidsberäkning:**

TTL representerar längden från den initiala tiden för auktoriseringsbegäran till förfallotiden, oberoende av den faktiska tiden för visning av innehåll. Varje framtida begäran kontrollerar den aktuella servertiden mot den lagrade förfallotiden för att auktorisera åtkomst.

**Autentisering:**

Autentisering krävs inte för Basic TempPass, vilket gör att du kan fortsätta direkt till auktoriseringssteget.

**Behörighet:**

Eftersom det inte sker någon interaktion med en faktisk MVPD godkänner MVPD Basic &quot;Temp Pass&quot; alla resurser eftersom TempPass är giltigt. Om auktoriseringen lyckas gäller fortfarande medietokenverifieringsbiblioteket för verifiering av medietoken och för att säkerställa resursvalidering innan uppspelning av innehåll initieras.

Auktoriseringsbeslutet baseras på användaridentifieringsparametrarna och den konfigurerade TTL-nivån. För att en resurs ska kunna auktoriseras måste följande villkor uppfyllas av en giltig begäran:

* **Oförbrukad varaktighet:** Förfallotiden beräknas genom att den inledande tiden för auktoriseringsbegäran (sparad i våra databaser) läggs till i den konfigurerade TTL-tiden. Den aktuella servertiden jämförs med den här förfallotiden för att avgöra om TempPass fortfarande är giltigt.

Om en användare överskrider den konfigurerade TTL-gränsen kan de inte längre visa innehåll på samma enhet om inte TempPass-inställningen återställs.

**Förhandsauktorisering:**

När en begäran om förhandsauktorisering görs för ett grundläggande tillfälligt MVPD returnerar svaret hela listan med resurser från begäran som korrekt förauktoriserade. Detta beteende återspeglar logiken för godkännande, eftersom villkoren för godkännande baseras på tidsgränser, snarare än specifika resurser. Så länge tidsbegränsningen är giltig, kommer de begärda resurserna att auktoriseras.

**Utloggning:**

Utloggning krävs inte för Basic TempPass, vilket gör att du kan växla direkt till autentiseringssteget med en faktisk användare som använder MVPD.

**Spåra data och analyser:**

Under det grundläggande TempPass-flödet använder spårningsdata en hashad version av enhetsidentifieraren med MVPD-identifieraren inställd på&quot;Temp Pass&quot;. Programmerarna bör skilja TempPass-statistik från MVPD standardmått i sina analysimplementeringar.

## TempPass för kampanjerbjudande {#promotional-temp-pass}

Den här kampanjfunktionen TempPass utökar funktionerna i det grundläggande TempPass-paketet, som är särskilt utformat för att köra kampanjkampanjer. Med den här funktionen kan programmerare engagera användare genom att ge åtkomst till ett fördefinierat antal VOD-titlar under en viss tidsperiod efter att ha samlat in en giltig användaridentifiering, till exempel en e-postadress.

TempPass-kampanj innehåller alla funktioner i grundläggande TempPass, med extra flexibilitet för:

* Definiera det maximala antalet VOD-titlar som är tillgängliga under kampanjperioden.
* Ange den tidsperiod under vilken kampanjåtkomsten är giltig.

När en användare har överskridit de fördefinierade åtkomstgränserna (antal VOD-titlar eller varaktighet) kan han/hon inte längre visa innehåll på samma enhet eller med samma användaridentifierare om inte TempPass har återställts.

### Funktionsinformation {#promotional-temp-pass-feature-details}

**Konfigurationsparametrar:**

* **Användarinformationsnyckel:** Nyckeln som används för att kommunicera den användarangivna identifieraren, till exempel en e-postadress (nyckeln är e-post).
* **Antal resurser:** Definierar hur många VOD-titlar en användare kan komma åt.
* **TTL (Time-To-Live):** Den varaktighet under vilken användaren kan använda de tillåtna resurserna.

**Användar-ID:**

Funktionen Promotional TempPass använder hash-värdet för den användaridentifierare som anges ovanpå enhetsidentifieraren som användaridentifieringsparametrar.

>[!IMPORTANT]
>
> Valideringen och hashningen av användarangiven identifierare hanteras av Programmer, inte av Adobe. Adobe lagrar ingen personligt identifierbar information. Programmeraren ansvarar därför för att generera och skicka en hash med den unika användaridentifierare som anges när han eller hon interagerar med Adobe Pass autentiserings-API:erna.

Adobe rekommenderar att du använder **SHA-2**-familjen eller dess specifika **SHA-256** -, **SHA-512** -funktioner på data innan de skickas till Adobe. Exempel: **SHA-256** over **&quot;user@domain.com&quot;** is **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a33 32b18b88d09069fb7&quot;**.

Följande tabell visar hur användaridentifieringsparametrarna påverkar användarens testupplevelse:

| Hash för användarangiven identifierare | Enhetsidentifierare | Resultat |
|-------------------------------|-------------------|---------------------------------------------------------|
| Nytt | Nytt | Ny testversion |
| Befintlig | Nytt | Befintlig testversion (baserad på användarangiven identifierarhash) |
| Nytt | Befintlig | Befintlig testversion (baserad på enhets-ID) |
| Befintlig | Befintlig | Befintlig testversion |

**Visa tidsberäkning:**

TTL representerar längden från den initiala tiden för auktoriseringsbegäran till förfallotiden, oberoende av den faktiska tiden för visning av innehåll. Varje framtida begäran kontrollerar den aktuella servertiden mot den lagrade förfallotiden för att auktorisera åtkomst.

**Autentisering:**

Autentisering krävs inte för Promotional TempPass, vilket gör att du kan fortsätta direkt till auktoriseringssteget.

Som stöd för implementeringen av programmerarens program visar Promotional TempPass följande användarmetadatainformation, som du når via motsvarande nycklar:

* **`remaining_resources`**: Anger det antal resurser som användaren fortfarande har rätt att använda.
* **`used_assets`**: Visar en lista över resurser som användaren redan har förbrukat.
* **`expiration_date`**: Visar förfallodatumet för användarens tillfälliga kampanjpass.

**Behörighet:**

Eftersom det inte sker någon interaktion med en faktisk MVPD godkänner MVPD för kampanj &quot;Temp Pass&quot; alla resurser när TempPass är giltigt. Om auktoriseringen lyckas gäller fortfarande medietokenverifieringsbiblioteket för verifiering av medietoken och för att säkerställa resursvalidering innan uppspelning av innehåll initieras.

Auktoriseringsbeslutet baseras på användaridentifieringsparametrar, det konfigurerade antalet resurser och TTL. För att en resurs ska kunna auktoriseras måste följande villkor uppfyllas av en giltig begäran:

* **Oförbrukad varaktighet:** Förfallotiden beräknas genom att den inledande tiden för auktoriseringsbegäran (sparad i våra databaser) läggs till i den konfigurerade TTL-tiden. Den aktuella servertiden jämförs med den här förfallotiden för att avgöra om TempPass fortfarande är giltigt.
* **Oförbrukade resurser:** Antalet förbrukade resurser spåras (sparas i våra databaser). Antalet förbrukade resurser jämförs med det konfigurerade antalet resurser för att avgöra om TempPass fortfarande är giltigt.

Om en användare överskrider det konfigurerade TTL-värdet eller antalet resurser, kommer han/hon inte längre att kunna visa innehåll på samma enhet eller med samma användaridentifierare, såvida inte TempPass-objektet återställs.

**Förhandsauktorisering:**

När en begäran om förhandsauktorisering görs för ett tillfälligt kampanjtillstånd för MVPD returnerar svaret hela listan med resurser från begäran som auktoriserade. Detta beteende återspeglar logiken för tillstånd, eftersom villkoren för tillstånd baseras på tidsgränser och det totala antalet resurser som används, snarare än specifika resurser. Så länge tidsbegränsningen är giltig och resursgränsen inte har överskridits, kommer de begärda resurserna att auktoriseras.

**Utloggning:**

Utloggning krävs inte för Promotional TempPass, vilket gör att du kan växla direkt till autentiseringssteget med en faktisk MVPD-användare.

**Spåra data och analyser:**

Under kampanjen TempPass-flöde använder spårningsdata en hashad version av enhetsidentifieraren med MVPD-identifieraren inställd på&quot;Temp Pass&quot;. Programmerarna bör skilja TempPass-statistik från MVPD standardmått i sina analysimplementeringar.

## Återställ TempPass API-åtkomst {#reset-tempass-api-access}

Innan du får åtkomst till Reset TempPass API måste du slutföra de nödvändiga stegen i DCR-processen (Dynamic Client Registration). Denna obligatoriska process ser till att du har den åtkomsttoken som krävs för att interagera med Reset TempPass API.

Utförliga instruktioner finns i dokumentationen [Översikt över registrering av dynamiska klienter](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Återställ TempPass API - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

För att återställa ett visst TempPass för en enhet eller alla enheter ger Adobe Pass Authentication programmerare ett API som fungerar för både Basic och Promotional TempPass.

### Begäran {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">värd</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Frågeparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">beställare_id</td>
      <td>Den interna unika identifierare som är associerad med tjänsteleverantören under introduktionsprocessen.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>Den interna unika identifierare som är associerad med TempPass under introduktionsprocessen.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            Enhetsidentifieraren som den här återställningsåtgärden är giltig för.
            <br/><br/>
            Om inget värde anges gäller återställningsåtgärden alla enheter.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av nyttolasten för innehavartoken beskrivs i dokumentationen för <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Hämta åtkomsttoken</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

### Svar {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Inget innehåll</td>
      <td>
        Återställningen lyckades.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Förbjuden</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta nya klientautentiseringsuppgifter och en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
</table>

### Exempel {#reset-tempass-v3-reset-samples}

#### Återställ TempPass för en specifik enhet {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### Återställ TempPass för alla enheter {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## Återställ TempPass API - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

För att återställa ett specifikt TempPass för en generisk nyckel (användartillhandahållen identifierarhash) eller alla nycklar, tillhandahåller Adobe Pass Authentication programmerare ett API som fungerar för Promotional TempPass.

### Begäran {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">värd</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Frågeparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">beställare_id</td>
      <td>Den interna unika identifierare som är associerad med tjänsteleverantören under introduktionsprocessen.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>Den interna unika identifierare som är associerad med TempPass under introduktionsprocessen.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">key</td>
      <td>
            Den användarangivna identifierarhash som återställningsåtgärden är giltig för.
            <br/><br/>
            Om inget värde anges gäller återställningsåtgärden alla användare.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av nyttolasten för innehavartoken beskrivs i dokumentationen för <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Hämta åtkomsttoken</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

### Svar {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Inget innehåll</td>
      <td>
        Återställningen lyckades.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Förbjuden</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta nya klientautentiseringsuppgifter och en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
</table>

### Exempel {#reset-tempass-v3-reset-generic-samples}

#### Återställ TempPass för en viss nyckel {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### Återställ TempPass för alla nycklar {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

För att utnyttja funktionen TempPass måste du implementera koduppdateringar för att ändra hur ditt TV Everywhere-program (TVE) interagerar med Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

En utförlig guide om dessa uppdateringar och associerade arbetsflöden finns i dokumentationen för [Temporära åtkomstflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
