---
title: Preflight-auktorisering
description: Preflight-auktorisering
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---

# Preflight-auktorisering {#preflight-authorization}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Ökning {#overview}

Den här funktionen ger en enkel behörighetskontroll för flera resurser. Syftet med den här enkla kontrollen är att dekorera användargränssnittet (till exempel att ange åtkomststatus med lås- och upplåsningsikoner). Preflight-auktoriseringen är så enkel och effektiv som möjligt, så att ett enda API-anrop ger auktoriseringsstatusen för en lista över resurser. Observera att den här funktionen inte är auktoritativ när det gäller att auktorisera en resurs.

A `getAuthorization(resource)` eller `checkAuthorization(resource)` fortfarande måste anropas innan uppspelning tillåts.

Preflight-auktoriseringen har också stöd för ett annat användningsfall där programmeraren måste begära behörighet för flera resurs-ID:n för att tillåta uppspelning av ett medieinnehåll. Programmeraren kan göra en inledande preflight-kontroll av de nödvändiga resurserna, och beroende på svaret, kan misslyckas tidigt om affärsvillkoren inte uppfylls.

En lista över MVPD som stöder preflight-auktorisering finns i [MVPD Preflight-auktorisering](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) sida.

>[!NOTE]
>
> Observera att användningen av den här funktionen för de sidoskydd som inte har fullständigt stöd för preflight-auktorisering måste överenskommas i förväg med Adobe och distributörer. Användningen av preflight-auktorisering för dessa MVPD kommer att hamna i det&quot;värsta scenariot&quot; som beskrivs [här](/help/authentication/mvpd-preflight-authz.md#intro) och kan leda till prestandaproblem och långsam svarstid. </br>
> Observera också att användningen av preflight-behörighet med **mer än fem resurser måste uttryckligen godkännas av Adobe**.

## AccessEnabler Preflight API {#AE_pre_api}

AccessEnabler visar ett API-/callback-funktionspar för att implementera preflight-auktorisering. API-anropet tar en enda parameter som består av en lista med resurser. Callback-funktionen tar en parameter som representerar de faktiska auktoriserade resurserna. Följande exempel finns i ActionScriptet, men anropen finns i alla varianter av AccessEnabler.

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

Anropa den här funktionen i AccessEnabler-objektet för att begära auktoriseringsstatus för en lista över resurser.

Parametern resources är en lista över resurser som auktoriseringen ska kontrolleras för. Varje element i listan ska vara en sträng som representerar resurs-ID:t. Resurs-ID har samma begränsningar som resurs-ID i `getAuthorization()` anrop, det vill säga, det värde som fastställts mellan Programmer och MVPD, eller ett mediets RSS-fragment, har bestämts. Observera att Adobe Pass Authentication inte hanterar resurser på något sätt, förutom ett tunt medieringslager som kan omvandla resursformat beroende på vad som stöds i MVPD.

### preauthorizedResources(Array:authorizedResources) {#preauthRes}

Det här är en callback-funktion som måste implementeras i programmerarens program i det övre lagret. AccessEnabler anropar den här funktionen efter att listan över auktoriserade resurser har beräknats.


### JS-exempel

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## Implementeringsinformation {#details}

- [Preflight med ChannelID](#preflight_using_channelID)
- [POST / Förhandsauktorisera](#post)
- [Lagring](#storage)

API-anropet försöker hitta en cachelagrad lista över auktoriserade resurser för den aktuella användaren i klientens lokala lagring. Om det inte finns någon cachelagrad lista görs ett HTTPS-anrop till AdobePass-servrarna för att hämta listan.

Cachelagringsfunktionen förbättrar prestandatiden vid efterföljande anrop genom att helt hoppa över nätverksanropet. Dessutom kan den cachelagrade listan fyllas i i förväg som en del av autentiseringsprocessen.  (Information om hur du konfigurerar det här scenariot finns i [Integrering av preflight-auktorisering](/help/authentication/authz-usecase.md#preflight_authz_int) under Authorization i MVPD Integration Guide).

Dessutom kan den cachelagrade resurslistan användas för att optimera auktoriseringsflödet, i den meningen att om det finns en cachelagrad resurslista, `checkAuthorization()` kan kontrollera det innan ett nätverksanrop görs. Om resursen inte finns med i listan över förauktoriserade resurser kan kontrollen misslyckas utan att Adobe Pass autentiseringsservrar behöver anropas.


### Preflight med ChannelID {#preflight_using_channelID}

Från och med Adobe Pass Authentication 2.4.1 fungerar preflight-flödet enligt följande:

1. Under autentiseringen läser Adobe Pass-autentiseringen `channelIID` -element från MVPD:s SAML-svar och använder det här värdet för att ange `authorizedResources` -element i autentiseringstoken.
1. Innanför `checkPreauthorizedResources()` API-funktion, Adobe Pass Authentication kontrollerar om `authorizedResources` -elementet är inställt.
1. Om `authorizedResources` är inställt, läser Adobe Pass Authentication det värdet och utför en korsning mellan resurslistan från `authorizedResources` -element och en lista över resurser som tagits emot från `checkPreauthorizedResources()` parameter.  Resultatet av denna korsning är den slutliga listan över förauktoriserade resurser.
1. Om `authorizedResources` -elementet har inte angetts, kör det tidigare implementerade flödet där listan med resurser som tagits emot från `checkPreauthorizedResources()` parametern skickas till PreAuthorizationServlet. Den här servern utför auktoriseringsanrop till MVPD-slutpunkterna och returnerar listan över förauktoriserade resurser.

### Exempel på preflight med ChannelID

I exemplet nedan visas ett exempel på kanalutbud. Observera att namnet på attributet som används för att skicka kanallistan kan skilja sig från ett MVPD till ett annat:

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


The `authorizedResources` -elementet från autentiseringselementet visas så här:

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

Programmeraren kör `checkPreauthorizedResources()` API-anrop, skickar följande parameterlista:</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- TruTV&quot;
- &quot;fbc-fox&quot;

Den aktuella preflight-implementeringen utför överlappningen med listan över resurser i `authorizedResources` -element och returnerar den här listan:

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- TruTV&quot;



**Obs!** Observera att skärningen inte är skiftlägeskänslig.



### POST / Förhandsauktorisera {#post}

Anropet utförs automatiskt av AccessEnabler när det inte finns någon cachelagrad, auktoriserad resurslista.


#### Begäran {#req}

| Parameter | Typ | Obligatoriskt | Beskrivning |
| --- | --- | --- | --- |
| `authentication_token` | string | JA | Autentiseringstoken. |
| `resource_id` | string | JA | En enda resurs. Detta kan anges flera gånger, en gång för varje element i resursarrayen som anges i `checkPreauthorizedResources()` API-anrop. |


**Obs!** Det maximala antalet begärda resurser kan konfigureras.
**Högsta standardvärde är 5.**


#### Svar {#resp}

Svaret som den förauktoriserade servern skickar tillbaka har följande format:

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### Lagring {#storage}

En lista med förauktoriserade resurser som AccessEnabler får från tjänsteleverantören. Den här listan över resurser:

- Lagras tillsammans med AuthN- och AuthZ-tokens
- Är giltig så länge som användaren finns på samma webbplats eller tills AuthN-token upphör att gälla
- Hämtas på nytt varje gång användaren hamnar på en ny Adobe Pass-autentiseringsintegrerad webbplats

Varje post innehåller det resurs-ID som användaren är förauktoriserad för.

Till exempel:


| Resurs-ID | Auktoriserad |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Den här listan heter &quot;preauktoriseringscache&quot;.

#### Flöde {#flow}

1. Programmerarens app/webbplats skapar en `checkPreauthorizedResources(resourceList)` ring.
1. AccessEnabler verifierar autentiseringstoken för auktoriserade resurser:
   1. Om autentiseringstoken innehåller auktoriserade resurser är den här listan auktoritativ och inget anrop bör göras för att få den här informationen. Resurser från resourceList söks igenom i listan över auktoriserade resurser på autentiseringstoken och endast de som hittades returneras av `preauthorizedResources()` återanrop.
   1. Om autentiseringstoken INTE innehåller auktoriserade resurser - `resourceList` jämförs med listan med resurser i cachen för förhandsauktorisering.
      1. Om listan innehåller samma resurser innebär det att ett anrop till servern redan har gjorts och att svaret redan finns i cachen för förauktorisering. Endast auktoriserade resurser returneras av `preauthorizedResources()` återanrop.
      1. Om listan INTE innehåller samma resurser måste klienten anropa servern för att erhålla auktoriseringstillståndet för resurserna i resourceList. Svaret hämtas och lagras i cachen för förauktorisering, vilket helt ersätter de gamla resurserna. Endast auktoriserade resurser returneras av `preauthorizedResources()` återanrop.


#### Listhämtning {#listRetrieve}

När en `checkPreauthorizedResources()` anropas kontrolleras listan över resurser som ska verifieras för auktorisering mot cachen för förauktorisering. Om listan innehåller samma uppsättning resurser anropas inte tjänsteleverantören eftersom alla resurser som behövs för att aktivera `preauthorizedResources()` återanrop finns redan i cachen.


#### logOut() {#logout}

Cachen för förhandsauktorisering töms vid utloggning.


## Beroenden {#depends}

Preflight-API:ts prestanda beror på specifika MVPD-implementeringar.  Implementeringsalternativ finns i [Integrering av preflight-auktorisering](/help/authentication/authz-usecase.md#preflight_authz_int) under Authorization i MVPD Integration Guide.


## Säkerhet {#security}

Klient-API:erna är tillgängliga för alla programmerare.

Implementeringen använder HTTPS som transport, men för att ha ett lättare anrop används inga ytterligare säkerhetsåtgärder (ingen signering, inga FAXS).

**Obs!** Använd INTE detta API på ett auktoritativt sätt för att avgöra om en användare ska beviljas åtkomst till en skyddad resurs. Syftet med denna API är att dekorera användargränssnittet och/eller preflight-granska affärsbeslut. The `getAuthorization()` och `checkAuthorization()` anrop ska alltid göras innan uppspelning tillåts.


## Kompatibilitet {#compat}

Den här funktionen stöds i alla varianter av AccessEnabler: AS, JS, AIR, iOS, Android, Xbox (i AuthN-flöde på andra skärmen).

Preflight-auktorisering stöder inte förauktoriserade resurser som innehåller CDATA-avsnitt. Fokus på det aktuella preflight-systemet är att stödja kanalnivåfiltrering. Resurser med CDATA-avsnitt är sannolikt resurser på tillgångsnivå. Preflight stöder enkla `mrss` resurser för förhandsauktorisering på kanalnivå, så länge de inte innehåller CDATA.

## Integrering med andra funktioner {#integ_w_other_features}

En eventuell optimering kan ske i auktoriseringsflödet för att misslyckas snabbt om resursen inte finns med i listan över förauktoriserade resurser.

I den här optimeringen integreras serverns preflight-slutpunkt med degraderingsmekanismen.

Om en regel av typen &quot;AuthN All&quot; finns på plats för MVPD och Requestor, kommer preflight-slutpunkten endast att spegla alla resurser som tagits emot i begäran.

Detsamma gäller om&quot;AuthZ All&quot; är aktiverat för minst en av resurserna som finns i begäran.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
