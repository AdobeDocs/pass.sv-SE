---
title: Förstå mått på serversidan
description: Förstå mått på serversidan
exl-id: 516884e9-6b0b-451a-b84a-6514f571aa44
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2207'
ht-degree: 0%

---

# Förstå mått på serversidan {#understanding-server-side-metrics}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.


## Introduktion {#intro}

I det här dokumentet beskrivs de mått på serversidan för Adobe Pass-autentisering som genereras av ESM-tjänsten (Entitlement Service Monitoring). Det beskriver inte samma händelser som de ser ur kundsidans perspektiv (vad programmerare skulle se om de skulle implementera en mättjänst som Adobe Analytics på sin sida/i sin tillämpning).

## Sammanfattning av händelser {#events_summary}

Följande händelser genereras från serversidan för Adobe Pass-autentisering:

* **Händelser som genererats i autentiseringsflödet**(En inloggning med MVPD)

   * Meddelande om AuthN-försök - Detta genereras när användaren skickas till MVPD-inloggningswebbplatsen.
   * Meddelande om väntande AuthN-meddelande - Om användaren lyckas logga in med sitt MVPD-dokument, genereras detta när användaren dirigeras tillbaka till Adobe Pass-autentisering.
   * AuthN-meddelande beviljad - Detta genereras när användaren är tillbaka på programmerarens webbplats och har hämtat autentiseringstoken från Adobe Pass-autentisering.
* **Auktoriseringsflöde** (Det är bara att kontrollera om du har en MVPD-fil)\
  *Krav:* En giltig AuthN-token
   * Meddelande om AuthZ-försök
   * Meddelande om AuthZ beviljad
* **Begäran om uppspelning har slutförts**\
  *Krav:* Giltiga AuthN- och AuthZ-tokens
   * Meddelande om en kontroll med Adobe Pass-autentisering
   * En uppspelningsbegäran kräver både en given autentisering och en given auktorisering


Antalet unika användare beskrivs i detalj i [Unika användare](#unique-users) nedan. Som en översikt gäller följande formler vanligtvis eftersom autentiserings- och auktoriseringssvar vanligtvis cachelagras:

* Antal AuthN-försök \> Antal AuthN som beviljats
* Antal AuthZ-försök \> Antal AuthZ som beviljats
* Antal AuthZ-försök \> Antal AuthN som beviljats (vanligtvis)
* Antal lyckade uppspelningsbegäranden \> Antal AuthZ som beviljats


### Exempel {#example}

I följande exempel visas serversidesstatistik för en månad för ett varumärke:

| Mått | MVPD 1 | MVPD 2 | … | MVPD n | Totalt |
| -------------------------- | ------ | ------ | - | ------ | ---------------------------------------------- |
| Slutförda autentiseringar | 1125 | 2892 |   | 2203 | SUM(MVP1+...MVPD n) |
| Slutförda auktoriseringar | 2527 | 5603 |   | 5904 | SUM(MVP1+...MVPD n) |
| Slutförda uppspelningsbegäranden | 4201 | 10518 |   | 10737 | SUM(MVP1+...MVPD n) |
| Unika användare | 1375 | 2400 |   | 2890 | SUM för alla användare för alla deduplicerade MVPD-filer\* |
| Försök till autentisering | 2147 | 3887 |   | 3108 | SUM(MVP1+...MVPD n) |
| Attempted Authorizations | 2889 | 6139 |   | 6039 | SUM(MVP1+...MVPD n) |

</br>

Avduplicering bör i det här fallet inte ha någon effekt eftersom olika MVPD-användare inte ska få samma användar-ID. När du gör en summa för två olika varumärken men samma MVPD-fil bör effekten av borttagning av dubbletter vara mycket större.

## Händelseutlösare {#event_triggers}

### Ny användare - fullständigt flöde {#new-user-full-flow}

I följande diagram beskrivs händelser och steg för en användare utan autentiseringstoken (en ny användare eller en användare som har en autentiseringstoken har gått ut):



![](assets/ae-flow-with-events.png)



I flödet ingår rundresor till distributörer av videoprogrammeringstjänster för både autentisering (#5 till \#7) och auktorisering (\#11).



När flödet är klart cachelagras autentiserings- och auktoriseringstoken på användarens enhet. TTL-värdena (Time-to-Live) för autentiseringstoken är mellan 6 timmar och 90 dagar. Ett AuthN-tokenförfallodatum tvingar automatiskt en AuthZ-token att upphöra. TTL-värdet för auktoriseringstoken är vanligtvis 24 timmar.

| Händelser på serversidan som utlösts | <ul><li>Autentiseringsförsök, väntande autentisering, beviljad autentisering</li><li>Auktoriseringsförsök, auktorisering beviljad</li><li>Begäran om slutförd uppspelning</li></ul> |
|---|---|


### Returnerande användare - AuthZ- och AuthN-token cachelagrade

För användare som har giltiga AuthZ- och AuthN-tokens cachelagrade utförs följande steg:


![](assets/ae-flow-tokens-cached-web.png)



Detta aktiveras automatiskt vid anrop `getAuthorization()`och endast omfattar kontroller med Adobe Pass-autentisering. MVPD är inte involverat i det här flödet.


| Händelser på serversidan som utlösts | * Uppspelningsbegäran lyckades |
|---|---|


### Returnerande användare - AuthN-token cachelagrade, AuthZ-token har upphört att gälla

För användare som fortfarande har giltiga AuthN-tokens utförs följande steg:

![](assets/ae-flow-authn-token-cached.png)


Detta flöde innebär en rundtur till MVPD.


| Händelser på serversidan som utlösts | <ul><li>Auktoriseringsförsök, auktorisering OK</li><li>Begäran om slutförd uppspelning</li> |
|---|---|

## Autentiseringshändelser {#authn_events}

### Autentiseringsförsök {#authentication-attempt}

Som framgår av diagrammet ovan aktiveras autentiseringshändelserna endast när användaren gör en rundtur till MVPD. Autentiseringshändelserna innehåller inte cachelagrade tokenautentiseringar.

Autentiseringsförsökshändelsen utlöses efter att användaren har klickat på ett visst MVPD från väljaren.

* Den första händelsen på MVPD-sidan som ligger nära detta är sidinläsningen
* Adobe Pass Authentication räknar inte upprepade inloggningsförsök från användaren på MVPD-sidan (felaktigt lösenord, försök igen)
* flera försök räknas som ett försök
* Vissa MVPD-dokument utför även auktorisering i autentiseringssteget, och användaren omdirigeras inte tillbaka om auktoriseringen misslyckas.

### Väntande autentisering {#authentication-pending}

Den här händelsen inträffar när omdirigeringsprocessen till Adobe Pass Authentication har startats.

### Autentisering beviljad {#authentication-granted}

Användaren är en känd prenumerant på MVPD, vanligtvis med en Pay TV-prenumeration, men ibland med enbart Internet-åtkomst. En lyckad autentisering kan inträffa antingen på grund av att användaren uttryckligen angav giltiga inloggningsuppgifter med sitt MVPD, eller på grund av att han/hon tidigare hade angett giltiga inloggningsuppgifter och kontrollerat &quot;kom ihåg mig&quot; (och den tidigare sessionen inte hade gått ut).

MVPD skickar därför ett positivt svar på autentiseringsbegäran till Adobe Pass Authentication och Adobe Pass Authentication skapar en *AuthN-token*.

* Autentisering cachelagras vanligtvis under en lång tid (en månad eller mer). På grund av detta kommer autentiseringshändelser inte längre att finnas förrän token upphör att gälla och flödet startas igen.
* När du kommer in från en annan webbplats/app via enkel inloggning aktiveras inga autentiseringshändelser.



### Komcast Authentication {#comcast-authentication}

Comcast har ett annat AuthN-flöde än resten av de alternativa programmeringsdokumenten.

Följande funktioner beskriver skillnaderna:

* **Sessionscookie, beteende**: Detta medför att alla autentiseringstoken tas bort fullständigt när användaren har stängt webbläsaren. Den här funktionen finns endast på webben. Huvudsyftet är att se till att din Comcast-session inte är beständig på osäkra/delade datorer. Effekten blir att fler autentiseringsförsök/beviljade flöden görs än för resten av de alternativa dokumentationsdokumenten.

* **AuthN per beställarID**: Comcast tillåter inte att AuthN-tillstånd cachas från ett begärande-ID till ett annat. På grund av detta måste varje webbplats/app gå till Comcast för att få en autentiseringstoken. Förutom att ta hänsyn till användarupplevelser, är effekten, som ovan, att fler autentiseringsförsök/händelser som beviljats kommer att genereras.

* **Passiv autentisering**: För att förbättra användarupplevelsen men ändå behålla funktionen AuthN per requestID inträffar ett passivt autentiseringsflöde i en dold iFrame. Användaren kommer inte att se något, men händelserna kommer fortfarande att utlösas som tidigare.

Om användaren klickar på &quot;kom ihåg mig&quot; på inloggningssidan för Comcast, kommer efterföljande besök på den här sidan (under en tvåveckorsperiod) bara att vara en snabb omdirigering. Annars måste användarna autentisera på sidan.

### Misslyckad autentisering {#unsuccessful-authentication}

En misslyckad autentisering är inte en händelse i sig i Adobe Pass-autentiseringen, men kan beräknas som skillnaden mellan försök och lyckade.

I versionen från maj 2013 kommer Adobe Pass Authentication att lägga till felkoder för misslyckade autentiseringar som beror på system- eller nätverksfel, inklusive DRM-fel (tokenbindning misslyckades) och LSO-fel (inget utrymme att skriva token osv.).

### Konverteringsgrad för autentisering {#authenitication-conversion-rate}

Ett intressant mått som programmerare kan spåra är konverteringsgraden för autentisering, beräknad som (AuthN-begäranden/AuthN-beviljad) %.

Nedan följer några kommentarer om mätvärdena:

* Eftersom det är ett händelsebaserat mått återspeglar det inte riktigt användarens unika konverteringsfrekvens - om en användare försöker åtta gånger och lyckas nionde gången - så kommer detta att avspegla sig mycket dåligt i konverteringsgraden ovan.
* I Adobe Pass Authentication (på serversidan) finns det inget sätt att beräkna en unik baserad autentiseringskonvertering.
* Om det finns automatiska AuthN-försök i webbplatsen/appen skevas även mätvärdena ovan.

## Auktoriseringshändelser {#authorization_events}

### Auktoriseringsförsök {#authorization_attempt}

Förutom att hämta en autentiseringstoken måste användarna också få en autentiseringstoken innan de kan spela upp innehåll. Detta sker vanligtvis efter autentisering eller om auktoriseringstoken förfaller. Eftersom kontrollen görs på serversidan (från Adobe Pass-autentiseringsservrar till MVPD-servrar) behöver användaren inte göra något.

### Tillstånd beviljad {#authorization-granted}

En&quot;auktorisering beviljad&quot; signalerar att den autentiserade användarens prenumeration innehåller den begärda programmeringen.

Observera att inte alla programmeringsdokument har stöd för ett separat auktoriseringssteg. För viss autentisering är autentiseringen lika med auktorisering. MVPD skickar ett svar på AuthZ-begäran via Adobe Pass Authentication, och Adobe Pass Authentication skapar en AuthZ-token.

* AuthZ-token cachelagras under en tidsperiod, vanligtvis 24 timmar Under den här perioden utlöses inga AuthZ-händelser.
* Vissa MVPD-program fungerar med auktoriseringar på tillgångsnivå, andra fungerar med auktoriseringar på kanalnivå - beroende på vilken som används utlöses fler eller färre AuthZ-händelser. Även för auktorisering på kanalnivå finns cachelagring - så om samma resurs begärs på mindre än 24 timmar aktiveras inga händelser.

### Behörighet nekad {#authorization-denied}

Om en auktorisering nekas har den autentiserade användaren ingen bekräftad prenumeration på den begärda programmeringen. Den troligaste orsaken är att kanalen inte ingår i användarens prenumerationspaket, men detta kan även återspegla en användare som bara har internetåtkomst från MVPD.

För vissa distributörer av videoprogrammeringstjänster kan användarna autentiseras trots att de bara har en Internetprenumeration från distributören (ingen betal-TV-prenumeration). Även om den kanal som användaren begär auktorisering för finns i baspaketet, kommer auktoriseringen i det här fallet att nekas.

Vissa MVPD-program erbjuder anpassade felmeddelanden för AuthZ-nekanden som kan innehålla erbjudanden om att uppgradera sitt paket.


### Konverteringsgrad för auktorisering {#authorization-conversion-rate}

Konverteringsgraden för autentisering kan beräknas som (AuthZ-begäranden/AuthZ-beviljad) %.

### Begäran om uppspelning har slutförts {#successful-play-request}

En användare som är både autentiserad och behörig får visa skyddat innehåll.

När uppspelningen är klar genererar Adobe Pass Authentication en kortlivad medietoken som försäkrar att användaren har rätt att visa den begärda videon. Programmeraren använder denna medietoken för ytterligare validering av den potentiella användaren. Medietoken spåras som lyckade uppspelningsbegäranden.

* Adobe Pass Authentication does *not* spåra om videouppspelningen faktiskt påbörjades efter generering av medietoken. Om det till exempel finns en geobegränsning för innehållet räknas transaktionen fortfarande som en lyckad uppspelningsbegäran, även om flödet faktiskt aldrig startar.
* Eftersom AuthN- och AuthZ-tokens cachelagrar MVPD-svaret under en tidsperiod, är händelsen för lyckad uppspelningsbegäran den vanligaste händelsen i mätvärdena.

## Unika användare {#unique-users}

### Definition {#definition}

Vid en lyckad autentisering spårar Adobe Pass Authentication förekomsten av en unik användare, baserat på det returnerade användar-ID:t för MVPD.  Värdet baseras på användarens inloggningsinformation, men det innehåller ingen identifierbar information.

Det här värdet skickas också till webbplatsen/appen i sendTrackingData-återanropet.

Detta värde kan vara beständigt på olika enheter (MVPD skapar samma värde för en viss användare, oavsett var inloggningen sker) eller tillfälligt (för varje inloggning genereras ett nytt värde som MVPD mappar i bakänden. Vanligtvis är de värden som tillhandahålls av MVPD till Adobe Pass Authentication beständiga mellan sessioner och enheter, men, som det noteras, är beständigheten varken garanterad eller validerad.

Det här värdet används som ett sätt att beräkna de unika användarna. Det rapporterade värdet (per begärande-ID/intervall/MVPD) dedupliceras för det aktuella intervallet. Summan av unika användare per dag skiljer sig alltså vanligtvis från månadsvärdet, där månadsvärdet har det lägre värdet.

Detta antal omfattar alla händelser från Adobe Pass-autentisering, minus autentiseringsförsök (som inte har något användar-ID) men inklusive försök till (och eventuellt misslyckade) auktoriseringar.

### Exempel {#examples}

#### Dag 1 {#day1}

Användare XYZ går till webbplatsen för att titta på en video.

Utlösta händelser:

* AuthN-försök (ingen unik användare ännu)
* AuthN beviljad
   * nu identifierar vi användaren unikt baserat på vad som returneras av MVPD, så det dagliga antalet unika användare ökas med 1
   * AuthN-token cachelagras i 30 dagar
* AuthZ-försök/beviljad händelse
   * AuthZ-token cachelagrad i 1 dag
* Händelsen för begäran om uppspelning har slutförts

#### Dag 1 (senare på) {#day1-later-on}

Användare XYZ tittar på en annan video.

Utlösta händelser:

* Händelsen för begäran om uppspelning (resten cachelagras)
* Ingen ökning av dagliga eller månatliga unika

#### Dag 3 {#day3}

Användare XYZ tittar på en annan video.

Utlösta händelser:

* AuthZ-försök/beviljad händelse
   * Sedan 1 dagars cachning från dag 1 har gått ut
* Händelsen för begäran om uppspelning (resten cachelagras)
* Dagens unika användare har ökat med 1 - månadskuvorna är fortfarande 1

#### Dag 31 {#day31}

Användare XYZ tittar på en annan video.

Samma som i dag 1 sedan AuthN-cachelagringen upphörde.

Om samma användare skulle misslyckas med auktoriseringen kommer antalet unika användare per månad fortfarande att ökas med 1 eftersom det finns två händelser som innehåller användar-ID:t - autentiseringen har beviljats och auktoriseringsförsöket.

### enkel inloggning (SSO) {#single-sign-on-sso}

I vissa fall kan antalet unika användare vara större än antalet lyckade autentiseringar. Detta är vanligtvis fallet när många användare kommer in via enkel inloggning från andra webbplatser/appar och bara behöver få behörighet för den aktuella webbplatsen/appen.

### Jämföra unika användare på klient- och serversidan {#comparing-client-side-and-server-side-unique-users}

Om användar-ID-värdet från `sendTrackingData()` används på klientsidan för att räkna unika användare. Klientsidans och serversidans nummer ska matcha.

Om skillnaderna är större är det vanligtvis följande orsaker som står för skillnaden:

* Video spelar uniques jämfört med alla event uniques. Som vi nämnt räknas Adobe Pass Authentication som unika användare för alla händelser utom AuthN-försök. Det innebär att om användaren bara autentiserar (på sidan) men inte visar en video, så utlöses ändå en ökning av antalet användare.

* Räkna användare som inte godkänts - Adobe Pass-autentisering räknar även dessa användare i det rapporterade numret.

<!--
## Related Information {#related-information}

- [Entitlement Service Monitoring API](/help/authentication/entitlement-service-monitoring-api.md)

-->
