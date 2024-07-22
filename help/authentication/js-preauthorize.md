---
title: Förauktorisera
description: Förauktorisera JavaScript
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1465'
ht-degree: 0%

---

# Förauktorisera {#js-preauthorize}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. För användning av detta API krävs aktuell licens från Adobe. Obehörig användning är inte tillåten.

## Översikt {#preauth-overview}

Metoden Preauthorised API ska användas av program för att erhålla beslut om förauktorisering för en eller flera resurser. Begäran om förauktorisering av API bör användas för gränssnittstips och/eller innehållsfiltrering. En faktisk begäran om auktoriserings-API måste göras innan användaren kan få åtkomst till de angivna resurserna.

Om ett oväntat fel (till exempel nätverksproblem och slutpunkt för MVPD-auktorisering inte är tillgänglig) inträffar när en förauktoriserad API-begäran bearbetas av Adobe Pass-autentiseringstjänsterna, inkluderas en eller flera separata felinformation för de berörda resurserna som en del av resultatet av förauktoriserad API.

### public preauthorized(request: PreauthorateRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Beskrivning:** Den här metoden används av program för att hämta autentiserade användares (informativa) beslut om förauktorisering från Adobe Pass Authentication-tjänsten för att visa specifika skyddade resurser. Det primära syftet är att dekorera programmets användargränssnitt (t.ex. att ange åtkomststatus med lås- och upplåsningsikoner).

**Tillgänglighet:** v4.4.0+

**Parametrar:**

* `PreauthorizeRequest`: Builder-objekt används för att definiera förfrågan
* `AccessEnablerCallback`: Återanrop används för att returnera API-svaret
* `PreauthorizeResponse`: Objekt som används för att returnera API-svarsinnehållet

### Klassen PreauthorisationRequestBuilder {#preath-req-builder-class}

#### setResources(resources: sträng[]): PreauthorisationRequestBuilder {#set-res-preath-req-buildr}

* Anger listan över resurser som du vill hämta beslut om förauktorisering för.
* Det är obligatoriskt att ange den för användning av förauktoriserat API.
* Varje element i listan måste vara en sträng som representerar antingen resurs-ID-värdet eller mediets RSS-fragment som måste avtalas med MVPD.
* Den här metoden anger endast informationen i kontexten för den aktuella objektinstansen `PreauthorizeRequestBuilder`, som är mottagare för det här metodanropet.

* Om du vill skapa en faktisk `PreauthorizeRequest` kan du ta en titt på `PreauthorizeRequestBuilder`s metod:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` resurser. Listan över resurser som du vill hämta beslut om förauktorisering för.
* `@returns {PreauthorizeRequestBuilder}` Referensen till samma `PreauthorizeRequestBuilder`-objektinstans, som är mottagaren av metodanropet.
* Det gör det möjligt att skapa metodlänkning.

#### disableFeatures(...features: sträng[]): PreauthoriseraRequestBuilder {#disabl-featres-preauth-req-buildr}

* Anger de funktioner som du vill ska inaktiveras när du hämtar beslut om förauktorisering.
* Den här funktionen ställer endast in informationen i kontexten för den aktuella `PreauthorizeRequestBuilder`-objektinstansen, som är mottagaren av det här funktionsanropet.
* Om du vill skapa en faktisk `PreauthorizeRequest` kan du ta en titt på `PreauthorizeRequestBuilder`s funktion:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` funktioner. Den uppsättning funktioner som du vill inaktivera.
* `@returns` Referensen till samma `PreauthorizeRequestBuilder`-objektinstans, som är mottagaren av funktionsanropet.
* Detta görs för att möjliggöra skapandet av funktionskedjning.

#### build(): PreauthorizedRequest {#preauth-req}

* Skapar och hämtar referensen för en ny `PreauthorizeRequest`-objektinstans.
* Den här metoden instansierar ett nytt `PreauthorizeRequest`-objekt varje gång det anropas.
* Den här metoden använder de värden som angetts i förväg i kontexten för den aktuella objektinstansen `PreauthorizeRequestBuilder`, som är mottagare för det här metodanropet.
* Kom ihåg att denna metod inte ger några biverkningar.
* Därför ändras inte SDK-tillståndet eller tillståndet för objektinstansen `PreauthorizeRequestBuilder`, som är mottagare för det här metodanropet.
* Det innebär att efterföljande anrop av den här metoden för samma mottagare skapar olika nya `PreauthorizeRequest`-objektinstanser, men har samma information, om värdena som är inställda på `PreauthorizeRequestBuilder` inte ändras mellan anropen.
* Om du inte behöver uppdatera någon av den angivna informationen (resurser och cachelagring) kan du återanvända instansen PreauthorateRequest för flera användningsområden av API:et för förauktorisering.
* `@returns {PreauthorizeRequest}`

### gränssnitt AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(resultat: T); {#on-response-result}

* Svarsanrop anropades av SDK när API-begäran för förauktorisering slutfördes.
* Resultatet är antingen ett lyckat eller ett felresultat som innehåller en status.
* `@param {T} result`

#### onFailure(resultat: T); {#on-failure-result}

* Det gick inte att anropa återanrop från SDK när API-begäran för förauktorisering inte kunde hanteras.
* Resultatet är ett felresultat som innehåller en status.
* `@param {T} result`

### klass PreauthorisateResponse {#preauth-response-class}

#### Offentlig ställning: Status. {#public-status}

* Returnerar: Ytterligare statusinformation (tillstånd) om fel uppstår.
* Kan innehålla ett `null`-värde.

#### offentliga beslut: Beslut[]; {#public-decisions}

* Returnerar: Listan över beslut om förauktorisering. Ett beslut för varje resurs.
* Listan kan vara tom vid fel.

### klassstatus {#class-status}

#### Offentlig ställning: nummer. {#public-status-numbr}

* HTTP-svarets statuskod som dokumenteras i RFC 7231.
* Kan vara 0 om `Status` kommer från SDK i stället för från Adobe Pass Authentication Services.

#### offentlig kod: nummer. {#public-code-numbr}

* Standardfelkoden för Adobe Pass-autentiseringstjänster.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### offentligt meddelande:sträng; {#public-msg-string}

* Det detaljerade meddelande som i vissa fall tillhandahålls av MVPD-auktoriseringsslutpunkterna eller av regler för programmerarnedbrytning.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### offentlig information:sträng; {#public-details-strng}

* Innehåller ett detaljerat meddelande som i vissa fall tillhandahålls av auktoriseringsslutpunkterna för MVPD eller av reglerna för programmerarnedbrytning.
* Kan innehålla en tom sträng eller ett `null`-värde.


#### public helpUrl:sträng; {#public-help-url-string}

* Den URL som länkar till mer information om varför tillståndet/felet inträffade och möjliga lösningar.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### offentlig spårning:sträng; {#public-trace-string}

* Den unika identifieraren för det här svaret, som kan användas när du kontaktar support för att identifiera specifika problem i mer komplexa scenarier.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### Offentlig åtgärd:sträng; {#public-action-string}

* Den rekommenderade åtgärden för att avhjälpa situationen.
   * **ingen**: Tyvärr finns det ingen fördefinierad åtgärd för att åtgärda det här problemet. Det kan tyda på att det offentliga API:et har anropats på ett felaktigt sätt
   * **Konfiguration**: En konfigurationsändring krävs via TVE-instrumentpanelen eller genom att kontakta support.
   * **programregistrering**: Programmet måste registrera sig igen.
   * **Autentisering**: Användaren måste autentisera eller återautentisera.
   * **auktorisering**: Användaren måste erhålla auktorisering för den specifika resursen.
   * **degradering**: Någon form av degradering bör användas.
   * **Försök igen**: Ett nytt försök att utföra förfrågan kan lösa problemet
   * **försök igen-efter**: Ett nytt försök att utföra begäran efter den angivna tidsperioden kan lösa problemet.
* Kan innehålla en tom sträng eller ett `null`-värde.

### klassbeslut {#class-decision}

#### public id:sträng; {#public-id-string}

* Det resurs-ID som beslutet hämtades för.

#### Auktoriserat offentligt: booleskt värde; {#public-auth-boolean}

* Värdet på den flagga som anger om beslutet har vunnit laga kraft eller inte.

#### Offentligt fel: Status; {#public-error-status}

* Ytterligare statusinformation (tillstånd) om något fel har inträffat. Kan innehålla ett `null`-värde.

## Exempel på implementering av klient {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Scenarioexempel {#scenario-examples}

### Scenario 1: Alla begärda resurser har auktoriserats {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Inaktiverat</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Scenario 2: Vissa begärda resurser auktoriserades. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Inaktiverat</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;beslut&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    
    ]
    
    
    

</td>
  </tr>

<tr>
    <td>Aktiverat</td>
    <td>

    &quot;JavaScript
    {
    &quot;beslut&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;kod&quot;: &quot;preauthorisation_protected_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD har returnerat ett \&quot;Neka\&quot;-beslut vid begäran om förauktorisering för den angivna resursen.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;åtgärd&quot;: &quot;ingen&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;auktoriserad&quot;: true
    },
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Scenario 3: Ingen av de begärda resurserna har auktoriserats. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Inaktiverat</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;beslut&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;auktoriserad&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES0 2&quot;,
    &quot;auktoriserad&quot;: falskt
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;auktoriserad&quot;: falskt
    
    ]
    
    
    &quot;

</td>
  </tr>

<tr>
    <td>Aktiverat</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;beslut&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;auktoriserad&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorisation_protected_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD har returnerat ett \&quot;Deny\&quot;-beslut vid begäran om förauktorisering för den angivna resursen.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorisation_protected_by_mvm pd,
    &quot;meddelande&quot;: &quot;MVPD har returnerat ett \&quot;Neka\&quot;-beslut vid begäran om förauktorisering för den angivna resursen.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;åtgärd&quot;: &quot;none&quot;
    }
    },
    
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;maximum_execution_time_överstiged&quot;,
    &quot;message&quot;: &quot;Begäran slutfördes inte i maximal tillåten tid. Ett nytt försök att utföra förfrågan kan lösa problemet.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;åtgärd&quot;: &quot;försök igen&quot;
    }
    }
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Scenario 4: Felaktig klientbegäran - inga resurser har angetts. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Inaktiverat/aktiverat</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;Begäran misslyckades på grund av ett internt fel.&quot;,
    &quot;detaljer&quot;: &quot;Obligatorisk sträng [] parametern &#39;resource&#39; finns inte&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decimaler&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 5: Felaktig klientbegäran - tomma resurser har angetts. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Inaktiverat/aktiverat</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;Resursparametern saknas&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;, 
    &quot;åtgärd&quot;: &quot;ingen&quot;
    },
    &quot;beslut&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 6: Nätverksfel. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Aktiverat</td>
    <td>

    &quot;JavaScript
    {
    &quot;beslut&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;auktoriserad&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_Received_error&quot;,
    &quot;message&quot;: &quot;Det uppstod ett läsfel när svaret hämtades från den associerade partnertjänsten. Ett nytt försök att utföra förfrågan kan lösa problemet.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;:,
    &quot;fel&quot;: {
    &quot;status&quot;: 403,
    &quot;kod&quot;: &quot;network_Received_error&quot;,
    &quot;meddelande&quot;: &quot;Det uppstod ett läsfel när svaret hämtades från den associerade partnertjänsten. Ett nytt försök att utföra förfrågan kan lösa problemet.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;åtgärd&quot;: &quot;försök igen&quot;
    }
    }
    ]
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Scenario 7: Förauktoriseringsflödet anropades utan en giltig AuthN-session.

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Inaktiverat/aktiverat</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;Det gick inte att hämta autentiseringssessionen som är associerad med denna begäran. Användaren måste autentisera igen med ett MVPD som stöds för att kunna fortsätta.&quot;,
    &quot;åtgärd&quot;: &quot;autentisering&quot;
    },
    &quot;beslut&quot;: []
    }
    
    &quot;

</td>
  </tr>
</tbody>
</table>



### Scenario 8: Förauktoriseringsflödet anropades innan setRequestor-anropet slutfördes

<table>
<thead>
  <tr>
    <th>Flagga för förbättrad felkod</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Inaktiverat/aktiverat</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configure&quot;,
    &quot;message&quot;: &quot;Den begärande API:n är ännu inte konfigurerad, vilket är en förutsättning för att använda något API utöver setRequestor .&quot;,
    &quot;åtgärd&quot;: &quot;försök igen&quot;
    },
    &quot;beslut&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>
