---
title: Account IQ ordlista
description: En ordlista med produktterminologier.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Produktbegrepp och ordlista {#glossary}

## Vanliga terminologer i D2C och TV Everywhere

Följande produktterminologier och deras definitioner är gemensamma för alla [versioner av Account IQ](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

En kontrollpanel med diagram som delar upp det aktuella segmentets poängdelningar i kategorierna Mycket låg, Låg, Moderat, Hög och Mycket hög.

### [!UICONTROL Action] {#action-def}

En direkt eller indirekt händelse som är associerad med en [operation](#operation-def) som påverkar egenskaperna (till exempel delningspoäng eller antalet enheter som används) för ett relaterat åtgärdssegment.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

En kontrollpanel med diagram som delar upp det aktuella segmentets delningsresultat i kategorierna Mycket låg, Låg, Måttlig, Hög och Mycket hög, tillsammans med varje kategori i procent av det totala antalet direktuppspelningar för segmentet.

### [!UICONTROL AuthN] {#authn-def}

Antalet autentiseringsförsök. Ett autentiseringsförsök är den process genom vilken en användare försöker logga in med D2C-tjänsten eller MVPD. För TV Everywhere-användare omdirigeras användaren till sitt valda MVPD, där de identifierar sig själva för MVPD - vanligtvis med användarnamn och lösenord.

### [!UICONTROL AuthN OK] {#authn-ok-def}

Antal lyckade autentiseringar. En lyckad autentisering inträffar när en användares identifiering bekräftas av en D2C-tjänst eller en MVPD. För TV Everywhere-användare innebär detta att användaren omdirigeras tillbaka till programmeringsappen eller webbplatsen.

### [!UICONTROL Cluster] {#cluster-def}

Ett kluster är en samling platser och enheter. Klustren skapas genom att hitta gemensamma platser mellan olika enheter. De enheter som har setts på en gemensam plats anses tillhöra samma kluster. Två enheter kan finnas i samma kluster även om de inte har gemensamma platser, men kan anslutas via andra enheters platser.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Ett kluster utan statiska enheter.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Ett kluster som har minst en statisk enhet.

### [!UICONTROL Concurrency] {#consurrency-def}

Samstämmigheten definieras av två (eller flera) strömmar som spelas upp samtidigt eller mycket nära i tiden, så att intervallet mellan dem inte kan justeras genom att färdas med normal hastighet.
Samtidig användning beräknas med den maximala hastigheten (engelska mil/tim) mellan två olika kluster. En användare anses ha samtidig användning om han har en hastighet som är större än 124 m/tim på en sträcka som är mindre än 124 miles eller om han har en hastighet som är större än 400 m/tim på en sträcka som är större än 124 miles. Avståndet beräknas mellan platser från olika kluster. Samtidig användning tillåts i samma kluster.

### [!UICONTROL Device] {#device-def}

En maskinvaruprodukt för digital video som kan spela upp direktuppspelat innehåll. Exempel: smarttelefoner, bärbara och stationära datorer, spelkonsoler och smarta tv-apparater.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

Utvärderingsperiod är tiden från början av åtgärden som är kopplad till Operation till slutet av åtgärden eller dess mätning.

### [!UICONTROL Geographical Span] {#geographical-span-def}

Avståndet mellan de längsta punkterna i en uppsättning platser.

### [!UICONTROL Granularity] {#granularity-def}

I förhållande till tidsintervall anges periodens storlek, t.ex. en vecka eller en månad.

### [!UICONTROL IP] {#ip-def}

Den Internetprotokolladress som tilldelats en enhet av en Internetleverantör. Till exempel kabelleverantör och celltjänstleverantör.

### [!UICONTROL Location] {#location-def}

En unik punkt på jorden. Det kallas också geolokalisering för en viss spelbegäran med en precision på 1000m x 1000m (en kvadratkm).

### [!UICONTROL Media Company] {#media-company-def}

Media Company är ett företag som äger en grupp medienätverk.

### [!UICONTROL Metric] {#metric}

Metrisk är ett attribut för prenumerantkontot (t.ex. deras MVPD, programmerarna och kanalerna för det innehåll de direktuppspelar och antalet enheter de använder).

### [!UICONTROL Mobile device] {#mobile-device-def}

En enhet med hög mobilitet. Till exempel mobiltelefon och surfplatta.

### Åtgärd {#operation-def}

Åtgärden är en post som skapats för att spåra effekten av en viss [åtgärd](#action-def) på ett associerat segment. Ett exempel på en åtgärd kan vara en gräns för hur många samtidiga strömmar som tillåts för konton som identifieras av segmentet.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Ett värde som hjälper användarna att förstå omfattningen av lösenordsdelning och som gör att de kan agera snabbt.

### [!UICONTROL Play Request] {#play-requests-def}

Motsvarar en strömstart. Den här händelsen markerar början på ett användardirektuppspelat innehåll.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

Det kallas även Användning från delade konton och är ett värde som beräknas baserat på antalet uppspelningsbegäranden som görs av varje konto, viktat på varje kontos delningssannolikhet. Det kallas även Användning av riskindex för delade konton.

### [!UICONTROL Segment] {#segmet-def}

Segment är en uppsättning konton som uppfyller de användardefinierade villkor som anges av de valda måtten. &quot;användare i områden A, B, C, D eller E som har fler än tre enheter&quot;.

### [!UICONTROL Sharing level] {#sharing-level-def}

Det kallas även riskindex - konton eller riskindex för delade konton, och är ett värde som beräknas baserat på ett genomsnitt av delningssannolikheten som beräknats för varje konto i det aktuella segmentet som har direktuppspelats minst en gång under det valda tidsintervallet.

### [!UICONTROL Static device] {#static-device-def}

En enhet med mycket låg mobilitet. Exempel: spelkonsol, övre ruta och tv-apparat.

### [!UICONTROL Time interval] {#time-interval-def}

Det kallas även punkt och är tidsfönstret som innehåller aktiviteten för uppspelningsbegäran som representeras i användargränssnittet och tabellerna från början till slut.

### [!UICONTROL Trend] {#trend-def}

Procentskillnaden i det associerade måttet (till exempel procent av totala uppspelningsbegäranden) mellan den aktuella och föregående perioden.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Antalet unika konton som har direktuppspelats minst en gång under en angiven period.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Mer än 37 spelförfrågningar per månad.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Färre än 9 uppspelningsbegäranden per månad.

#### [!UICONTROL Regular user] {#regular-user-def}

Från 9 till 37 uppspelningsbegäranden per månad.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

En av en begränsad uppsättning kategorietiketter som används på ett konto och som bäst karakteriserar kontots användare i form av sociala grupper eller beteenden (t.ex. en liten familj, en resenär eller en dator, social delning osv.).

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

En videokategori är en specifik etikett som kvalificerar videons typ. Exempel: källregion, innehållstyper som VOD eller live, event eller specifika etiketter som team.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

En videokategori definieras av den kombination av programmerare, kanal och MVPD som är kopplad till strömmen.

### [!UICONTROL Zip Code] {#zip-code-def}

The U.S. Postal code associated with locations within the U.S.
<!--calculated metrics-->


## TV Everywhere-specifika terminologer

### [!UICONTROL AuthZ] {#authz-def}

Auktorisering, eller antalet begäranden om auktorisering. En begäran om behörighet är den process genom vilken en programmerare begär tillstånd från en MVPD via Adobe att börja direktuppspela en användares begärda innehåll. MVPD tilldelar vanligtvis begäran baserat på de innehållsrättigheter som är kopplade till användarens MVPD-prenumeration (t.ex. om kanalen som är kopplad till innehållet finns i användarens prenumeration). Vissa svar på begäran om tillstånd cachas av Adobe, vilket gör att Adobe kan svara direkt utan att skicka begäran vidare till det enhetliga dokumentationsdokumentet.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

Antal godkända auktoriseringar.

### [!UICONTROL Channel] {#channel-def}

Channel, som också kallas Property, är en tematiskt relaterad källa för videoinnehåll. Traditionellt representera en distinkt, numeriskt adresserbar kontinuerlig videomatning från ett videoflöde som är av typen MVPD. Kanalen mappas direkt till en tillgänglig innehållskanal som är tillgänglig för prenumeranterna via deras Set Top Box (STB).

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Ett värde som beräknas för var och en av riskindexen (konton, användning, totalt) för alla programmerare och distributörer av videoprogrammeringstjänster under det valda tidsintervallet.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

En typ av delningsanalys där utvärderingen av ett konto är begränsad till händelser som inträffar direkt hos programmerarna i det valda segmentet.  Normalt utvärderas alla kontohändelser, vilket ger en mycket mer korrekt uppskattning av delning.  Vissa MVPD-data är strukturerade på ett sätt som bara tillåter analys av isoleringsläge.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, som också kallas distributör, är en aggregator, återförsäljare och distributör av medieföretagets videoinnehåll.

### [!UICONTROL Programmer] {#programmer-def}

Programmer, även kallat Network, är ett företag som är dotterbolag till ett större företag (företag) som äger och förvaltar en eller flera kanaler.

### [!UICONTROL requestorID] {#requestorid-def}

Det ID som ett medieföretag använder för att identifiera sig själva eller ett dotterbolag till ett MVPD.  Beroende på programmeringsimplementeringen kan detta mappas till ett medieföretag, en programmerare eller en kanal. Traditionellt är detta mappat till en kanal.  I och med att man skapar Pseudo-kanaler som MML (March Madness Live) och tekniskt drivna åtgärder för att åtgärda MVPD-drivna databegränsningar, har man börjat bli mer kopplad till Media Company.

### [!UICONTROL resourceID] {#resource-id-def}

Innehållet som begärts av slutanvändaren. Traditionellt har detta identifierat den kanal som är associerad med innehållet som användaren har begärt.  Systemförbättringar gör att det ID:t kan representera specifika program (t.ex. med specifika klassificeringar), och ID:t fortsätter att identifiera den associerade kanalen.


