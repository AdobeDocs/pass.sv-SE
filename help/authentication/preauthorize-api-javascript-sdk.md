---
title: Förhandsauktorisera
description: JavaScript förauktorisera
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 6c328eb2c635a1d76fc7dae8148a4de291c126e0
workflow-type: tm+mt
source-wordcount: '1465'
ht-degree: 0%

---

# Förhandsauktorisera {#js-preauthorize}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#preauth-overview}

API-metoden för förauktorisering ska användas av program för att få beslut om förauktorisering för en eller flera resurser. Förauktoriserings-API-begäran ska användas för användargränssnittstips och/eller innehållsfiltrering. En faktisk begäran om auktoriserings-API måste göras innan användaråtkomst till de angivna resurserna tillåts.

Om ett oväntat fel (till exempel nätverksproblem och slutpunkten för MVPD-auktorisering inte är tillgänglig) inträffar när en förauktoriserings-API-begäran bearbetas av Adobe Pass Authentication-tjänster, inkluderas en eller flera separata feluppgifter för de berörda resurserna som en del av förauktoriserings-API-svarsresultatet.

### public preauthorized(request: PreauthorizedRequest, callback: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Beskrivning:** Den här metoden ska användas av program för att hämta autentiserade användares (informativa) beslut om förauktorisering från Adobe Pass-autentiseringstjänsten för att visa specifika skyddade resurser, i det primära syftet att dekorera programmets användargränssnitt (t.ex. ange åtkomststatus med lås- och upplåsningsikoner).

**Tillgänglighet:** v4.4.0+

**Parametrar:**

* `PreauthorizeRequest`: Det Builder-objekt som används för att definiera begäran
* `AccessEnablerCallback`: återanrop som används för att returnera API-svaret
* `PreauthorizeResponse`: Objektet som används för att returnera API-svarsinnehållet

### class PreAuthzeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizedRequestBuilder {#set-res-preath-req-buildr}

* Anger listan med resurser som du vill få beslut om förauktorisering för.
* Det är obligatoriskt att ange det för användning av förauktoriserat API.
* Varje element i listan måste vara en sträng som representerar resurs-ID eller mediets RSS-fragment som måste avtalas med MVPD.
* Den här metoden anger bara informationen i kontexten för den aktuella `PreauthorizeRequestBuilder`-objektinstansen, som är mottagare för det här metodanropet.

* Om du vill skapa en faktisk `PreauthorizeRequest` kan du ta en titt på metoden för `PreauthorizeRequestBuilder`:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` resurser. Listan med resurser som du vill få förauktoriseringsbeslut för.
* `@returns {PreauthorizeRequestBuilder}` Referensen till samma `PreauthorizeRequestBuilder`-objektinstans, som är mottagare av metodanropet.
* Den gör detta för att möjliggöra skapande av metodkedja.

#### disableFeatures(...features: string[]): PreauthorizedRequestBuilder {#disabl-featres-preauth-req-buildr}

* Anger de funktioner som du vill ska inaktiveras när du fattar beslut om förauktorisering.
* Den här funktionen anger bara informationen i kontexten för den aktuella `PreauthorizeRequestBuilder`-objektinstansen, som är mottagare för det här funktionsanropet.
* Om du vill skapa en faktisk `PreauthorizeRequest` kan du ta en titt på funktionen hos `PreauthorizeRequestBuilder`:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` funktioner. Den uppsättning funktioner som du vill ska inaktiveras.
* `@returns` Referensen till samma `PreauthorizeRequestBuilder`-objektinstans, som är mottagare av funktionsanropet.
* Den gör detta för att möjliggöra skapandet av funktionskedjor.

#### build(): PreauthorizedRequest {#preauth-req}

* Skapar och hämtar referensen för en ny `PreauthorizeRequest`-objektinstans.
* Den här metoden instansierar ett nytt `PreauthorizeRequest`-objekt varje gång det anropas.
* Den här metoden använder de värden som angetts i förväg i kontexten för den aktuella `PreauthorizeRequestBuilder`-objektinstansen, som är mottagare av det här metodanropet.
* Observera att denna metod inte ger några biverkningar.
* Därför ändras inte SDK:s tillstånd eller tillståndet för objektinstansen `PreauthorizeRequestBuilder`, som är mottagare för det här metodanropet.
* Det innebär att efterföljande anrop av den här metoden för samma mottagare skapar olika nya `PreauthorizeRequest`-objektinstanser, men med samma information, om värdena som anges till `PreauthorizeRequestBuilder` inte ändras mellan anropen.
* Om du inte behöver uppdatera någon av den angivna informationen (resurser och cachelagring) kan du återanvända PreauthorizedRequest-instansen för flera användningar av API:t för förauktorisering.
* `@returns {PreauthorizeRequest}`

### interface AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(result: T); {#on-response-result}

* Svarsåteranrop som anropas av SDK när förauktoriserings-API-begäran slutfördes.
* Resultatet är antingen ett lyckat resultat eller ett felresultat som innehåller en status.
* `@param {T} result`

#### onFailure(result: T); {#on-failure-result}

* Det gick inte att anropa ett anrop från SDK när förauktoriserings-API-begäran inte kunde hanteras.
* Resultatet är ett felresultat som innehåller en status.
* `@param {T} result`

### class PreAuthzeResponse {#preauth-response-class}

#### Offentlig status: Status; {#public-status}

* Returnerar: Ytterligare statusinformation (tillstånd) om fel uppstår.
* Kan innehålla ett `null`-värde.

#### offentliga beslut: Beslut []; {#public-decisions}

* Returnerar: Listan med förauktoriseringsbeslut. Ett beslut för varje resurs.
* Listan kan vara tom om den misslyckas.

### klassstatus {#class-status}

#### Offentlig status: nummer; {#public-status-numbr}

* Statuskoden för HTTP-svar enligt RFC 7231.
* Kan vara 0 om `Status` kommer från SDK i stället för Adobe Pass autentiseringstjänster.

#### Offentlig kod: nummer. {#public-code-numbr}

* Standardfelkoden för Adobe Pass Authentication Services.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### offentligt meddelande: sträng; {#public-msg-string}

* Det detaljerade meddelandet som i vissa fall tillhandahålls av MVPD-tillståndsslutpunkterna eller av reglerna för programmerarnedbrytning.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### offentlig information: sträng; {#public-details-strng}

* Innehåller ett detaljerat meddelande som i vissa fall tillhandahålls av MVPD-tillståndsslutpunkterna eller av reglerna för programmerarnedbrytning.
* Kan innehålla en tom sträng eller ett `null`-värde.


#### public helpUrl: string; {#public-help-url-string}

* Den URL som länkar till mer information om varför det här tillståndet/felet uppstod och möjliga lösningar.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### public trace: sträng; {#public-trace-string}

* Den unika identifieraren för det här svaret, som kan användas när support kontaktas för att identifiera specifika problem i mer komplexa scenarier.
* Kan innehålla en tom sträng eller ett `null`-värde.

#### offentlig åtgärd: sträng, {#public-action-string}

* Rekommenderade åtgärder för att åtgärda situationen.
   * **ingen**: Tyvärr finns det ingen fördefinierad åtgärd för att åtgärda problemet. Detta kan tyda på ett felaktigt anrop av det offentliga API:t
   * **konfiguration**: En konfigurationsändring krävs via TVE-instrumentpanelen eller genom att kontakta support.
   * **programregistrering**: Programmet måste registrera sig igen.
   * **autentisering**: Användaren måste autentisera eller autentisera igen.
   * **auktorisering**: Användaren måste erhålla auktorisering för den specifika resursen.
   * **nedbrytning**: En viss form av nedbrytning bör användas.
   * **försök igen**: Om du försöker igen kanske problemet kan lösas
   * **försök igen**: Problemet kan kanske lösas om du försöker utföra begäran igen efter den angivna tidsperioden.
* Kan innehålla en tom sträng eller ett `null`-värde.

### klassbeslut {#class-decision}

#### publikt ID: sträng; {#public-id-string}

* Resurs-ID som beslutet togs för.

#### auktoriserad offentlig: boolesk, {#public-auth-boolean}

* Värdet på flaggan som anger om beslutet har lyckats eller inte.

#### allmänt fel: Status; {#public-error-status}

* Ytterligare statusinformation om ett fel inträffar. Kan innehålla ett `null`-värde.

## Exempel på klientimplementering {#client-imp-example}

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


## Exempel på scenarier {#scenario-examples}

### Scenario 1: Alla begärda resurser har auktoriserats {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Handikappade</td>
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


### Scenario 2: Vissa begärda resurser har auktoriserats. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Handikappade</td>
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
    &quot;id&quot;: &quot;RES 03&quot;,
    &quot;authorized&quot;: true
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Aktiverad</td>
    <td>

    “JavaScript
    {
    &quot;decision&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;AUTHORIZED&quot;: sant
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;AUTHORIZED&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preAUTHORIZATION_DENED_BY_MVPD&quot;,
    &quot;message&quot;: &quot;MVPD har returnerat ett \&quot;Neka\&quot;-beslut vid begäran om förhandsauktorisering för den angivna resursen.&quot;,
    &quot;hjälpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;åtgärd&quot;: &quot;ingen&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;auktoriserad&quot;: sant
    },
    ]
    }
    
    “

</td>
  </tr>
</tbody>


### Scenario 3: Ingen av de begärda resurserna auktoriserades. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Handikappade</td>
    <td>

    “JavaScript
    
    {
    &quot;decision&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;AUTHORIZED&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;AUTHORIZED&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;auktoriserad&quot;: false
    }
    ]
    
    
    “

</td>
  </tr>

<tr>
    <td>Aktiverad</td>
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
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Handikappade/aktiverade</td>
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

### Scenario 5: Ogiltig klientbegäran - tomma resurser har angetts. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Handikappade/aktiverade</td>
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
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Aktiverad</td>
    <td>

    “JavaScript
    {
    &quot;decision&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;AUTHORIZED&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;kod&quot;: &quot;network_started_error&quot;,
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

### Scenario 7: Förhandsauktoriseringsflödet anropades utan en giltig AuthN-session.

<table>
<thead>
  <tr>
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Handikappade/aktiverade</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;Det gick inte att hämta autentiseringssessionen som är associerad med denna begäran. Användaren måste återautentisera med en MVPD som stöds för att kunna fortsätta.&quot;,
    &quot;åtgärd&quot;: &quot;autentisering&quot;
    },
    &quot;beslut&quot;: []
    }
    
    “

</td>
  </tr>
</tbody>
</table>



### Scenario 8: Förhandsauktoriseringsflödet anropades innan setRequestor-anropet slutfördes

<table>
<thead>
  <tr>
    <th>Förbättrad felkodflagga</th>
    <th>Svar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Handikappade/aktiverade</td>
    <td>

    “JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;kod&quot;: &quot;request_not_configuration&quot;,
    &quot;message&quot;: &quot;The requestSor är ännu inte konfigurerad, vilket är en förutsättning för att du ska kunna använda något annat API än setRequestor-API.&quot;,
    &quot;åtgärd&quot;: &quot;återförsök&quot;
    ,
    &quot;decision&quot;: []
    }
    “

</td>
  </tr>
</tbody>
</table>
