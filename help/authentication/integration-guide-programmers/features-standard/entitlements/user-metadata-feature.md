---
title: Metadatafunktion för användare
description: Metadatafunktion för användare
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Användarmetadata {#user-metadata}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>
</br>

## Introduktion {#intro}

Funktionen för användarmetadata gör att programmerare kan komma åt olika typer av användarspecifika data som hanteras av programmeringsgränssnitt.  Metadatatyper för användare omfattar postnummer, föräldrabetyg, användar-ID med mera.  *Metadata för användare* är ett tillägg till tidigare tillgängliga *statiska*-metadata (TTL för autentiseringstoken, TTL för auktoriseringstoken och enhets-ID).


Nyckelpunkter för användarmetadata:

- Skickades till programmerarens program under autentiserings- och auktoriseringsflödena
- Värden sparas i token
- Värdena kan normaliseras om olika PDF-filer tillhandahåller data i olika format
- Vissa parametrar kan krypteras med programmerarens nyckel (t.ex. zip-kod), se [Användarmetadatacertifikat för kryptering](user-metadata-certificate.md) för krypteringscertifikatgenerering
- Specifika värden är tillgängliga av Adobe via en konfigurationsändring

## Hämta användarmetadata {#obtaining}

Användarmetadata är tillgängliga för programmerare via AccessEnabler `getMetadata()`-funktionen och via `/usermetadata`-slutpunkten i det klientlösa API:t.  Mer information om hur du använder `getMetadata()` och dess motsvarande återanrop `setMetadataStatus()` finns i dokumentationen för plattforms-API (eller för slutpunkterna och parametrarna som används i det klientlösa API:t).

Programmerare får metadata genom att ange en nyckel för den typ av metadata som de vill hämta: `getMetadata(key)`.

Metadata returneras enligt följande: `setMetadataStatus(key, encrypted, data)`:

| Parameter | Typ | Beskrivning |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | Sträng | Anger vilken typ av metadata som begärts |
| `encrypted` | Boolean | En boolesk flagga som anger om&quot;värdet&quot; är krypterat eller inte. Om värdet är &quot;true&quot; är &quot;value&quot; en JSON Web Encrypted-representation av det faktiska värdet. |
| `data` | Objekt | Ett JSON-objekt som innehåller representationen av metadata |



Dataparameterns struktur, värdena varierar mellan olika typer:

| Nyckel | Värdetyp | Exempel | Beskrivning |
| --- | --- | --- | --- |
| `zip` | JSON-matris | \[&quot;77754&quot;, &quot;12345&quot;\] | Postnummer |
| `householdID` | JSON-sträng | &quot;1o7241p&quot; | Hushållsidentifierare. Om MVPD inte stöder underkonton är detta identiskt med `userID` |
| `maxRating` | JSON-objekt | { MPAA: &quot;NR&quot;, <br> VCHIP: &quot;X&quot;, <br> URL: &quot;http://manage.my/parental&quot; } | Högsta föräldraklassificering för användaren |
| `userID` | JSON-sträng | &quot;1o7241p&quot; | Användar-ID. Om ett MVPD-dokument har stöd för underkonton och användaren inte är huvudkontot, skiljer sig `userID` från `householdID`. |
| `channelID` | JSON-matris | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | En lista över kanaler som en användare har rätt att visa |
| `is_hoh` | JSON-sträng | &quot;1&quot; | En flagga som identifierar om en användare är chef för hushållet |
| `encryptedZip` | JSON-sträng | &quot;&quot; | Comcast visar postnumret som är krypterat |
| `typeID` | JSON-sträng | Primär | En flagga som identifierar om användarkontot är ett primärt/sekundärt konto |
| `primaryOID` | JSON-sträng | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Hushållsidentifierare. Om `typeID` är primär innehåller värdet för `userID` |
| hba_status | Boolean | &quot;true&quot; &quot;false&quot; | En boolesk flagga som anger om Hembaserad autentisering är aktiverat för en viss integrering |
| allowMirroring | Boolean | &quot;true&quot; &quot;false&quot; | Anger om skärmspegling är tillåten för den här enheten eller inte |

>[!NOTE]
>
> **Obs!** Om dataparametern är krypterad, vilket vanligtvis är fallet för **zip-nyckeln**, blir representationen av metadatanyckeln en JSON-sträng i stället för en array eller ett objekt.


**Viktigt!** De faktiska användarmetadata som är tillgängliga för en programmerare beror på vad ett MVPD-program gör tillgängligt.  Juridiska avtal måste undertecknas med distributörer av videoprogrammeringstjänster innan känsliga användarmetadata (t.ex. postnummer) görs tillgängliga i produktionsmiljön.

</br>


| Namn | Information | Kräver kryptering | Kommentar |
| --- | --- | --- | --- |
| Användar-ID | Enligt MVPD | Nej | Detta är det värde som sedan hashas av Adobe och visas i medietoken och i callback-funktionen sendTrackingData().<br><br>Kraschen i det här fallet utfördes av historiska orsaker<br><br>Detta ID kan vara ett hushåll-ID eller ett underkonto-ID. Detta är vanligtvis inte angivet, det är bara det ID som är knutet till inloggningen som användes vid tidpunkten (vilket kan vara ett primärt eller ett underkonto) |
| Överordnat användar-ID | Tillhandahålls av MVPD som endast ska användas för samtidighetsövervakningsflöden | Nej | Det här värdet används när samtidighetsgränser används för webbplatser och appar som använder MVPD och Programmer. <br><br>ID:t kan även innehålla principer för övervakning av samtidig användning<br><br>För de flesta programmeringsgränssnitten är det här värdet lika med användar-ID:t |
| Hushållets användar-ID | tillhandahålls av det sidoskydd som huvudsakligen ska användas för föräldrakontrollflöden | Nej | ID som gör det möjligt för programmerare att förstå användningen av hushållet jämfört med underkontot.<br><br>Det används ibland som ersättning för Föräldrakontroll om det inte finns några verkliga klassificeringar (om användaren var inloggad med hushållskontot kan han/hon titta på, annars visas inte klassificerat innehåll)<br><br>Det finns många variationer i flera sidopaneler för hur detta visas - hushållets användar-ID, huvud för hushåll-ID, huvud för hushållets flagga osv. |
| Hushållschef | Flaggsignering om aktuellt konto är hushållshuvud eller inte | Nej | se ovan |
| Typ-ID/Primärt ID | Hushållskontoidentifierare | Nej | AT&amp;T-specifika indikatorer för hushållschef.<br><br>Typ-ID = Flagga som identifierar om användarkontot är primärt/sekundärt konto<br><br>Primärt OID = Hushållsidentifierare. Om TypeID är Primary innehåller värdet för userID |
| Maximal klassificering | Högsta tillåtna klassificering för aktuellt konto | Nej | Används av programmerare för att filtrera bort innehåll som inte är lämpligt för konto.<br><br>Har MPAA- eller VCHIP-klassificeringar |
| Kanalrad upp | Lista över tillgängliga kanaler i användarens paket | Nej | Används för att snabbt tillåta/ta bort olika kanaler från portaler som samlar ihop flera nätverk </br></br> *Observera att preflight-auktorisering i allmänhet ger större flexibilitet för den här användningen och bör användas i stället* <br><br>OLCA-specifikationen tillåter detta som en AttributeStatement i AuthN-svaret |
| HBA-status | Anger om autentisering har skett via HBA | Nej |     |
| Postnummer | Användarens postnummer för fakturering | Ja | Används för utsändning eller porthändelser<br><br>Kan även tillhandahållas med AuthZ-svaret för snabba uppdateringar<br><br>Känsliga data, kräver juridiska avtal för MVPD |
| Krypterat postnummer | Användarens postnummer för fakturering (Comcast) | Ja | Som ovan, men krypterat av Comcast |
| Språk | Inställningar för användarspråk | Nej | Används för att visa meddelanden i enlighet med användarens inställningar |
| Tillåt spegling | Anger om skärmspegling är tillåten för den här enheten eller inte | Nej |     |



## Tillgängliga metadata {#available_metadata}

I följande tabell visas det aktuella tillståndet för användarens metadata i Adobe Pass Authentication-ekosystemet:


|     | **Legal **<br><br>**Agreement **<br><br>**Signerat (endast zip)** | **Användar-ID **<br><br>**på AuthN** | **Zipcode **<br><br>**i AuthN/Z** | **Klassificering **<br><br>**på AuthN/Z** | **Hushålls **<br><br>**ID för AuthN/Z** | **Kanal-ID på AuthN** | **Hushållschef på AuthN** | **Typ-ID på AuthN** | **Primärt OID på AuthN** | Språk | Överför användar-ID **på AuthN** | HBA-status | OnNet | inHome | Tillåt spegling i AuthZ | **Anteckningar** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Formalt namn** | n/a | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primärOID | språk | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. För AuthN - OiosamlMetadataParser måste ändras så att det nya attributet aktiveras för alla parsrar <br>2.  För AuthZ - Det finns inget generiskt sätt eftersom auktoriseringsimplementeringen är MVPD-specifik |
| **Kryptering krävs** | n/a | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| **Känslig** | n/a | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |     |     |
| **Adobe IdP** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja** | **Ja** | **Ja** | **Ja** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Inget juridiskt avtal behövs. Det kan aktiveras. |
| **Synkroniserare** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Rättsligt avtal täcker inte alla proxyservrar för MVPD.**   <br> Detta är ett generiskt stöd för Synacor - kanske inte har lagts till i alla deras MVPD-program. |
| Danska | **Nej** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Delar samma lista som alla Synacor MVPD, plus upstreamUserID. |
| Comcast | **Nej** | **Ja** | **Nej** | **Ja (endast AuthZ)** | **Ja (endast AuthZ)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** |     |
| **AT&amp;T** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Ja** | **Ja** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| **Cablevision** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| **HTC** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| **Proxyspan** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| **Proxyrensning** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthZ)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| Rogers | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| RCN | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| Stadga | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| Verizon | **Nej** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** |     |
| Eastlink | **Nej** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| Proxy GLDS | **Nej** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| DTV | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| COX | **Nej** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| Cogeco | **Nej** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |     |
| Videotron | **Nej** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Ja*** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Exponerar houseID med samma värde som userID |
| Spektrum | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Ja** |     |
| **Alla andra **<br><br>**MVPD** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Inget juridiskt avtal ännu, känsliga metadata är inte tillgängliga för produktion.** <br>För alla PDF-filer är användar-ID tillgängligt utan extra arbete. |


Listan över användarmetadatatyper utökas allt eftersom nya typer blir tillgängliga och läggs till i Adobe Pass autentiseringssystem.

## Kodexempel {#code_samples}

- [Kodexempel 1](#code_sample1)
- [Code Sample 2 (Mock getMetadata)](#code_sample2)


### Kodexempel 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### Code Sample 2 (Mock getMetadata) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
