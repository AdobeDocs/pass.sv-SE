---
title: Preflight-auktorisering
description: Preflight-auktorisering
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Preflight-auktorisering {#preflight-authorization}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Ökning {#overview}

Den här funktionen ger en enkel behörighetskontroll för flera resurser. Syftet med den här enkla kontrollen är att dekorera användargränssnittet (till exempel att ange åtkomststatus med lås- och upplåsningsikoner). Preflight-auktoriseringen är så enkel och effektiv som möjligt, så att ett enda API-anrop ger auktoriseringsstatusen för en lista över resurser. Observera att den här funktionen inte är auktoritativ när det gäller att auktorisera en resurs.

Ett `getAuthorization(resource)`- eller `checkAuthorization(resource)`-anrop MÅSTE fortfarande utföras innan uppspelning tillåts.

Preflight-auktoriseringen har också stöd för ett annat användningsfall där programmeraren måste begära behörighet för flera resurs-ID:n för att tillåta uppspelning av ett medieinnehåll. Programmeraren kan göra en inledande preflight-kontroll av de nödvändiga resurserna, och beroende på svaret, kan misslyckas tidigt om affärsvillkoren inte uppfylls.

En lista över MVPD som stöder preflight-auktorisering finns på sidan [MVPD Preflight Authorization](/help/authentication/mvpd-preflight-authz.md#preflight_support_list).

>[!NOTE]
>
> Observera att användningen av den här funktionen för de sidoskydd som inte har fullständigt stöd för preflight-auktorisering måste överenskommas i förväg med Adobe och distributörer. Användningen av preflight-auktorisering för dessa MVPD hamnar i det värsta scenariot som beskrivs [här](/help/authentication/mvpd-preflight-authz.md#intro) och kan leda till prestandaproblem och långsam svarstid. </br>
> Observera också att Adobe **uttryckligen måste godkänna användningen av preflight-auktorisering med** fler än 5 resurser.

## AccessEnabler Preflight API {#AE_pre_api}

AccessEnabler visar ett API-/callback-funktionspar för att implementera preflight-auktorisering. API-anropet tar en enda parameter som består av en lista med resurser. Callback-funktionen tar en parameter som representerar de faktiska auktoriserade resurserna. Följande exempel finns i ActionScriptet, men anropen finns i alla varianter av AccessEnabler.

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

Anropa den här funktionen i AccessEnabler-objektet för att begära auktoriseringsstatus för en lista över resurser.

Parametern resources är en lista över resurser som auktoriseringen ska kontrolleras för. Varje element i listan ska vara en sträng som representerar resurs-ID:t. Resurs-ID har samma begränsningar som resurs-ID:t i `getAuthorization()`-anropet, det vill säga det värde som har fastställts mellan Programmer och MVPD, eller ett medie-RSS-fragment. Observera att Adobe Pass Authentication inte hanterar resurser på något sätt, förutom ett tunt medieringslager som kan omvandla resursformat beroende på vad som stöds i MVPD.

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

Cachelagringsfunktionen förbättrar prestandatiden vid efterföljande anrop genom att helt hoppa över nätverksanropet. Dessutom kan den cachelagrade listan fyllas i i förväg som en del av autentiseringsprocessen.  (Information om hur du konfigurerar det här scenariot finns i [Integrering med preflight-auktorisering](/help/authentication/authz-usecase.md#preflight_authz_int) i auktoriseringsavsnittet i MVPD-integreringshandboken).

Dessutom kan den cachelagrade resurslistan användas för att optimera auktoriseringsflödet, i den meningen att om det finns en cachelagrad resurslista kan `checkAuthorization()` kontrollera den innan ett nätverksanrop görs. Om resursen inte finns med i listan över förauktoriserade resurser kan kontrollen misslyckas utan att Adobe Pass autentiseringsservrar behöver anropas.


### Preflight med ChannelID {#preflight_using_channelID}

Från och med Adobe Pass Authentication 2.4.1 fungerar preflight-flödet enligt följande:

1. Under autentiseringen läser Adobe Pass Authentication elementet `channelIID` från MVPD:s SAML-svar och använder det här värdet för att ange elementet `authorizedResources` i autentiseringstoken.
1. I API-funktionen `checkPreauthorizedResources()` kontrollerar Adobe Pass Authentication om elementet `authorizedResources` har angetts.
1. Om elementet `authorizedResources` anges läser Adobe Pass-autentiseringen det värdet och utför en korsning mellan resurslistan från elementet `authorizedResources` och listan över resurser som tagits emot från parametern `checkPreauthorizedResources()`.  Resultatet av denna korsning är den slutliga listan över förauktoriserade resurser.
1. Om elementet `authorizedResources` inte har angetts kör du det tidigare implementerade flödet, där listan med resurser som har tagits emot från parametern `checkPreauthorizedResources()` skickas till PreAuthorizationServlet. Den här servern utför auktoriseringsanrop till MVPD-slutpunkterna och returnerar listan över förauktoriserade resurser.

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


Elementet `authorizedResources` från autentiseringselementet visas enligt följande:

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

Programmeraren kör API-anropet `checkPreauthorizedResources()` och skickar följande parameterlista:</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- TruTV&quot;
- &quot;fbc-fox&quot;

Den aktuella preflight-implementeringen utför överlappningen med listan över resurser från elementet `authorizedResources` och returnerar den här listan:

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
| `resource_id` | string | JA | En enda resurs. Detta kan anges flera gånger, en gång för varje element i resursarrayen som anges i API-anropet `checkPreauthorizedResources()`. |


**Obs!** Det går inte att konfigurera det maximala antalet begärda resurser.
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
- Är giltig så länge som användaren finns på samma webbplats, eller tills
AuthN-token upphör att gälla
- Hämtas på nytt varje gång användaren loggar in på en ny Adobe Pass
autentiseringsintegrerad webbplats

Varje post innehåller det resurs-ID som användaren är förauktoriserad för.

Exempel:


| Resurs-ID | Auktoriserad |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Den här listan heter &quot;preauktoriseringscache&quot;.

#### Flöde {#flow}

1. Programmerarens app/webbplats gör ett `checkPreauthorizedResources(resourceList)`-anrop.
1. AccessEnabler verifierar autentiseringstoken för auktoriserade resurser:
   1. Om autentiseringstoken innehåller auktoriserade resurser är den här listan auktoritativ och inget anrop bör göras för att få den här informationen. Resurser från resourceList genomsöks i listan över auktoriserade resurser på autentiseringstoken och endast de som hittades returneras av callback-funktionen `preauthorizedResources()`.
   1. Om autentiseringstoken INTE innehåller auktoriserade resurser jämförs `resourceList` med listan över resurser i cachen för förauktorisering.
      1. Om listan innehåller samma resurser innebär det att ett anrop till servern redan har gjorts och att svaret redan finns i cachen för förauktorisering. Endast auktoriserade resurser returneras av återanropet `preauthorizedResources()`.
      1. Om listan INTE innehåller samma resurser måste klienten anropa servern för att erhålla auktoriseringstillståndet för resurserna i resourceList. Svaret hämtas och lagras i cachen för förauktorisering, vilket helt ersätter de gamla resurserna. Endast auktoriserade resurser returneras av återanropet `preauthorizedResources()`.


#### Listhämtning {#listRetrieve}

När en `checkPreauthorizedResources()` anropas kontrolleras listan över resurser som ska verifieras för auktorisering mot cachen för förauktorisering. Om listan innehåller samma uppsättning resurser anropas inte tjänstprovidern eftersom alla resurser som behövs för att utlösa återanropet `preauthorizedResources()` redan finns i cachen.


#### logOut() {#logout}

Cachen för förhandsauktorisering töms vid utloggning.


## Beroenden {#depends}

Preflight-API:ts prestanda beror på specifika MVPD-implementeringar.  Implementeringsalternativ finns i [Integrering av preflight-auktorisering](/help/authentication/authz-usecase.md#preflight_authz_int) i avsnittet Auktorisering i MVPD-integreringshandboken.


## Säkerhet {#security}

Klient-API:erna är tillgängliga för alla programmerare.

Implementeringen använder HTTPS som transport, men för att ha ett lättare anrop används inga ytterligare säkerhetsåtgärder (ingen signering, inga FAXS).

**Obs!** Använd INTE detta API på ett auktoritativt sätt för att avgöra om en användare ska beviljas åtkomst till en skyddad resurs. Syftet med denna API är att dekorera användargränssnittet och/eller preflight-granska affärsbeslut. `getAuthorization()`- och `checkAuthorization()`-anropen ska alltid utföras innan uppspelning tillåts.


## Kompatibilitet {#compat}

Den här funktionen stöds i alla varianter av AccessEnabler: AS, JS, AIR, iOS, Android, Xbox (i AuthN-flöde på andra skärmen).

Preflight-auktorisering stöder inte förauktoriserade resurser som innehåller CDATA-avsnitt. Fokus på det aktuella preflight-systemet är att stödja kanalnivåfiltrering. Resurser med CDATA-avsnitt är sannolikt resurser på tillgångsnivå. Preflight stöder enkla `mrss`-resurser för förauktorisering på kanalnivå, så länge de inte innehåller CDATA.

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
