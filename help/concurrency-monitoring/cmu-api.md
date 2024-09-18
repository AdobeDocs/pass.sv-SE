---
title: API-översikt
description: API-översikt
exl-id: 3fe6f6d8-5b2f-47e5-a8da-06fb18a5d46b
source-git-commit: 301825253b21746684df9b6372a239b03305d50e
workflow-type: tm+mt
source-wordcount: '2043'
ht-degree: 0%

---

# API för samtidighetsövervakning {#cmu-api-usage}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## API-översikt {#api-overview}

CMU (Concurrency Monitoring Usage) implementeras som ett WOLAP-projekt (webbaserad [Online Analytical Processing](http://en.wikipedia.org/wiki/Online_analytical_processing)). CMU är ett generiskt webb-API för affärsrapportering som backas upp av ett datalager. Det fungerar som ett HTTP-frågespråk som gör att vanliga OLAP-åtgärder kan utföras RESTfully.


>[!NOTE]
>
>CMU-API:t är inte allmänt tillgängligt. Kontakta din Adobe-representant om du har frågor om tillgänglighet.

CMU-API:t ger en hierarkisk vy över de underliggande OLAP-kubarna. Varje resurs ([dimension](/help/concurrency-monitoring/cm-usage-reports.md#dimensions-2-filter-metrics) i dimensionshierarkin, mappad som ett URL-sökvägssegment) genererar rapporter med (aggregerade) [mått](/help/concurrency-monitoring/cm-usage-reports.md#monitor-metrics) för den aktuella markeringen. Varje resurs pekar på sin överordnade resurs (för sammanslagning) och dess underresurser (för fördjupning). Segmentering och segmentering uppnås med frågesträngsparametrar som fäster dimensioner till specifika värden eller intervall.

REST API tillhandahåller tillgängliga data inom ett tidsintervall som anges i begäran (som faller tillbaka till standardvärdena om inget anges), enligt dimensionssökvägen, tillhandahållna filter och valda mätvärden. Tidsintervallet används inte för rapporter som inte innehåller tidsdimensioner (år, månad, dag, timme, minut, sekund).

Slutpunkts-URL-rotsökvägen returnerar sammanställda mått inom en enda post, tillsammans med länkarna till de tillgängliga detaljalternativen. API-versionen mappas som det avslutande segmentet i URI-sökvägen för slutpunkten. https://mgmt.auth.adobe.com/cmu/*v2* betyder till exempel att klienterna får åtkomst till WOLAP version 2.

De tillgängliga URL-sökvägarna kan identifieras via länkar i svaret. Giltiga URL-sökvägar hålls för att mappa en sökväg i det underliggande fördjupningsträdet som innehåller (pre-) aggregerade mått. En sökväg i formatet /dimension1/dimension2/dimension3 återspeglar en föraggning av dessa tre dimensioner (motsvarigheten till en SQL-sats GROUP BY dimension1, dimension2, dimension3). Om det inte finns någon sådan föraggning och systemet inte kan beräkna den direkt, returnerar API:t ett 404-svar som inte hittades.

### Detaljträd {#drill-down-tree}

I följande detaljerade träd visas dimensionerna (resurserna) som finns i CMU 2.0:

**Dimensioner tillgängliga för CM-innehavare**

![](assets/new_breakdown.png)

En `GET` till API-slutpunkten `https://mgmt.auth.adobe.com/cmu/v2` returnerar en representation som innehåller:

* Länkar till tillgängliga rotsökvägar:

  ```html
  <link rel="drill-down" href="/cmu/v2/dimensionA"/>
  <link rel="drill-down" href="/cmu/v2/dimensionB"/>
  ```

* En sammanfattning (aggregerade värden) för alla mått (i standardintervallet, eftersom inga frågesträngsparametrar anges, se nedan).

Efter en detaljerad sökväg (steg för steg): /dimensionA/year/month/day/dimensionX hämtar följande svar:

* Länkar till detaljalternativen&quot;dimensionY&quot; och&quot;dimensionZ&quot;
* En rapport som innehåller dagliga aggregat för varje värde av dimensionX

### **Filter**

Förutom datum-/tidsdimensionerna kan alla dimensioner som är tillgängliga för den aktuella projektionen (dimensionssökväg) filtreras genom att använda dess namn som en frågesträngsparameter.

Följande filtreringsalternativ är tillgängliga:

* **Lika med** filter anges genom att dimensionsnamnet anges till ett visst värde i frågesträngen.
* **IN**-filter kan anges genom att lägga till samma dimension-name-parameter flera gånger med olika värden: dimension=värde1&amp;dimension=värde2
* **Inte lika med**-filter måste använda! symbolen efter dimensionsnamnet som resulterar i &#39;!&#39;=&#39; &quot;operator&quot;: dimension!=värde
* **INTE IN**-filter kräver &#39;!=&#39; operatorn ska användas flera gånger, en gång för varje värde i uppsättningen: dimension!=värde1&amp;dimension!=värde2&amp;...


Det finns också en särskild användning för dimensionsnamnen i frågesträngen: Om dimensionsnamnet används som en frågesträngsparameter utan värde instruerar detta API att returnera en projektion som innehåller den dimensionen i rapporten.

Exempel på CMU-frågor:

| URL | SQL-motsvarighet |
|:---|:---|
| /dimension1/dimension2/dimension3?dimension1=värde1 | SELECT * from projection WHERE dimension1 = &#39;value1&#39; GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1=värde1&amp;dimension1=värde2 | SELECT * from projection WHERE dimension1 IN (&#39;value1&#39;, &#39;value2&#39;) GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=värde1 | SELECT * from projection WHERE dimension1 &lt;> &#39;value1&#39; GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=värde1&amp;dimension2!=värde2 | SELECT * from projection WHERE dimension1 NOT IN (&#39;value1&#39;, &#39;value2&#39;) GROUP BY dimension1, dimension2, dimension3 |
| Anta att det inte finns någon direkt sökväg: /dimension1/dimension3 men det finns en sökväg: /dimension1/dimension2/dimension3 </br></br> /dimension1?dimension3 | VÄLJ * FRÅN PROJEKTIONSGRUPP BY dimension1,dimension3 |

>[!NOTE]
>
>Ingen av dessa filtreringstekniker fungerar för datum-/tidsdimensioner. Det enda sättet att filtrera datum/tid-dimensioner är att ställa in parametrarna för start- och slutfrågesträngen (beskrivs nedan) på de värden som krävs.

Följande frågesträngsparametrar har reserverade betydelser för API (och kan därför inte användas som dimensionsnamn, annars går det inte att filtrera en sådan dimension).

Parametrar för CMU API-reserverad frågesträng:

| Parameter | Valfritt | Beskrivning | Standardvärde | Exempel |
|:--------------|:--------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------|
| access_token | Ja | Om IMS OAuth-skyddet är aktiverat kan IMS-token skickas antingen som en standardtoken för auktorisering Bearer eller som en frågesträngsparameter. | Ingen | access_token=XXXXXX |
| dimension-namn | Ja | Dimensionsnamn - antingen i den aktuella URL-sökvägen eller i en giltig undersökväg - värdet behandlas som ett likhetsfilter. Om inget värde anges kommer den angivna dimensionen att inkluderas i utdata även om den inte finns med eller intill den aktuella banan | Ingen | someDimension=someValue&amp;someOtherDimension |
| end | Ja | Sluttid för rapporten i millis | Serverns aktuella tid | end=2012-07-30 |
| format | Ja | Används för innehållsförhandling (med samma effekt men lägre prioritet än sökvägen &quot;extension&quot; - se nedan). | Ingen: innehållsförhandlingen provar andra strategier | format=json |
| limit | Ja | Maximalt antal rader som ska returneras | Standardvärde som rapporteras av servern i självlänken om ingen gräns anges i begäran | limit=1500 |
| mått | Ja | Kommaavgränsad lista med metriska namn som ska returneras. Den ska användas både för att filtrera en delmängd av tillgängliga mätvärden (för att minska nyttolaststorleken) och för att tvinga API att returnera en projektion som innehåller de begärda mätvärdena (i stället för standardprojektionen). | Alla mätvärden som är tillgängliga för den aktuella projektionen returneras om den här parametern inte anges. | metrics=m1,m2 |
| start | Ja | Starttid för rapporten som ISO8601. Servern fyller i den återstående delen om bara ett prefix anges: Exempel: start=2012 ger start=2012-01-01:00:00:00 | Rapporteras av servern i självlänken. Servern försöker att tillhandahålla rimliga standardinställningar baserat på den valda tidsperioden | start=2012-07-15 |


Den enda tillgängliga HTTP-metoden är GET. Stöd för OPTIONS/HEAD kan ges i framtida versioner.



## Statuskoder för CMU API {#cmu-api-status-codes}

| Statuskod | Orsaksfras | Beskrivning |
|:-----------|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200 | OK | Svaret kommer att innehålla länkarna &quot;roll-up&quot; och &quot;drill-down&quot; (om tillämpligt). Rapporten återges som ett attribut för resursen: ett kapslat element/egenskap för rapport. |
| 400 | Felaktig begäran | Svarstexten innehåller ett textmeddelande som förklarar vad som är fel med begäran.  Statusen 400 Dålig begäran åtföljs av en förklarande text i svarstexten (normal/textmedietyp) som ger användbar information om klientfelet. Förutom de triviala scenarierna, till exempel ogiltiga datumformat eller filter som tillämpas på icke-befintliga dimensioner, kommer systemet också att neka att svara på frågor som kräver att en stor mängd data returneras eller slås samman i farten. |
| 401 | Obehörig | Orsakas av en begäran som inte innehåller rätt OAuth-huvuden för att autentisera användaren |
| 403 | Förbjuden | Anger att begäran inte tillåts i den aktuella säkerhetskontexten. Detta inträffar när användaren är autentiserad men inte har åtkomst till den begärda informationen |
| 404 | Hittades inte | Inträffar om en ogiltig URL-sökväg anges med begäran. Detta bör aldrig inträffa om klienten följer länkarna &quot;drill-down&quot;/&quot;roll-up&quot; i 200 svar |
| 405 | Metoden tillåts inte | Signalerar att en metod som inte stöds användes i begäran. Även om det för närvarande bara finns stöd för metoden GET kan framtida versioner tillåta HEAD eller OPTIONS |
| 406 | Godkänns inte | Signalerar att en medietyp som inte stöds begärdes av klienten |
| 500 | Internt serverfel | &quot;Det här ska aldrig hända&quot; |
| 503 | Tjänsten är inte tillgänglig | Signalerar ett fel i programmet eller dess beroenden |

## Dataformat {#data-formats}

Data finns i följande format:

* JSON (standard)
* XML
* CSV
* HTML (för demoändamål)


Följande strategier för innehållsförhandling kan användas av klienter (prioriteten ges av positionen i listan - första saker först):

1. Ett &quot;filtillägg&quot; som läggs till i det sista segmentet i URL-sökvägen: t.ex. /cmu/v2/tenant/year/month/day.xml. Om URL:en innehåller en frågesträng måste tillägget komma före frågetecknet: `/cmu/v2/tenant/year/month/day.csv?mvpd=SomeMVPD`
1. En formatfrågesträngsparameter: t.ex. `/cmu/report?format=json`
1. Standardhuvudet för HTTP-godkännande: t.ex. `Accept: application/xml`

Både &quot;extension&quot; och frågeparametern stöder följande värden:

* xml
* json
* csv
* html

Om ingen medietyp har angetts i någon av strategierna skapar API-gränssnittet JSON-innehåll som standard.

## HAL (Hypertext Application Language) {#hypertext-app-lang}

För JSON och XML kodas nyttolasten som HAL, vilket beskrivs här: `http://stateless.co/hal_specification.html`.

Den faktiska rapporten (en kapslad tagg/egenskap som kallas&quot;rapport&quot;) består av den faktiska listan med poster som innehåller alla valda/tillämpliga mått och mått med deras värden, kodade enligt följande:

### JSON {#json}

```js
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML {#xml}

```xml
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report>
```

För XML- och JSON-format är fältordningen (mått och mått) i en post ospecificerad - men konsekvent (ordningen är densamma i alla poster). Klienter bör dock inte förlita sig på någon särskild ordning för fälten i en post.

Resurslänken (self-rel i JSON och href-resursattributet i XML) innehåller den aktuella sökvägen och frågesträngen som används för den infogade rapporten. Frågesträngen visar alla implicita och explicita parametrar så att nyttolasten uttryckligen anger vilket tidsintervall som används, eventuella implicita filter och så vidare. Resten av länkarna i resursen innehåller alla tillgängliga segment som kan följas för att detaljgranska aktuella data. En länk för sammanslagning kommer också att anges och den kommer att peka på den överordnade sökvägen (om sådan finns). Värdet `href` för länkarna för detaljnivå/rollup innehåller bara URL-sökvägen (den innehåller inte frågesträngen, så klienten måste lägga till den om det behövs). Observera att inte alla frågesträngsparametrar som används (eller är underförstådda) av den aktuella resursen kan användas för länkar av typen &quot;roll-up&quot; eller &quot;drill-down&quot; (filtren kan till exempel inte gälla för underresurser eller superresurser).

Exempel (under förutsättning att vi har ett enskilt mått som kallas klienter och att det finns en föraggning för `year/month/day/...`):

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.xml`

  ```xml
  <resource href="/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21">
    <links>
      <link rel="roll-up" href="/cmu/v2/year"/>
      <link rel="drill-down" href="/cmu/v2/year/month/day"/>
    </links>
    <report>
      <record month="6" year="2012" clients="205"/>
      <record month="7" year="2012" clients="466"/>
    </report>
  </resource>
  ```

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.json`

  ```js
  {
    "_links" : {
      "self" : {
        "href" : "/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21"
      },
      "roll-up" : {
        "href" : "/cmu/v2/year"
      },
      "drill-down" : {
        "href" : "/cmu/v2/year/month/day"
      }
    },
    "report" : [ {
      "month" : "6",
      "year" : "2012",
      "clients" : "205"
    }, {
      "month" : "7",
      "year" : "2012",
      "clients" : "466"
    } ]
  }
  ```

### CSV {#csv}

I CSV-dataformatet kommer inga länkar eller andra metadata (förutom rubrikraden) att anges. I stället kommer urvalsmetadata att anges i filnamnet, som kommer att följa detta mönster:

```html
report__<start-date>_<end-date>_<filter-values,...>.csv
```

CSV-filen innehåller en rubrikrad och sedan rapportdata som efterföljande rader. Rubrikraden innehåller alla mått följt av alla mått. Sorteringsordningen för rapportdata återspeglas i dimensionernas ordning. Om data sorteras efter D1 och sedan efter D2 ser därför CSV-huvudet ut så här: `D1, D2, ...metrics....`

Ordningen på fälten i rubrikraden återspeglar sorteringsordningen för tabelldata.

Exempel: https://mgmt.auth.adobe.com/cmu/v2/year/month.csv skapar en fil med namnet ```report__2012-07-20_2012-08-20_1000.csv``` med följande innehåll:

| År | Månad | Klienter |
|:----:|:-----:|:-------:|
| 2012 | 6 | 580 |
| 2012 | 7 | 231 |

## Datahastighet {#data-freshness}

Även om begäran innehåller en senast ändrad rubrik återspeglar den **INTE** tidpunkten då rapporten i brödtexten senast uppdaterades. De allmänna rapporterna beräknas regelbundet enligt följande regler:

* om tidsgranulariteten är **år** eller **månad** uppdateras rapporten varannan dag
* om tidsgranulariteten är **dag** uppdateras rapporten var tredje timme
* om tidsgranulariteten är **timme** uppdateras rapporten varje timme
* om tidsgranulariteten är **minut** uppdateras rapporten varje minut

Rapporterna **aktivitetsnivå** och **samtidighetsnivå** uppdateras varje dag, oavsett hur lång tid det tar.

## GZIP-komprimering {#gzip-compression}

Adobe rekommenderar starkt att du aktiverar GZIP-stöd i klienter som hämtar CMU-rapporter. Om du gör det minskar svarsstorleken avsevärt, vilket i sin tur minskar svarstiden. (Komprimeringsförhållandet för CMU-data ligger i intervallet 20-30.)

Om du vill aktivera gzip-komprimering i klienten anger du huvudet Accept-Encoding: enligt följande:

```
Accept-Encoding: gzip, deflate
```

## Relaterad information {#related-information}

* [CMU-översikt](/help/concurrency-monitoring/cm-usage-reports.md)
