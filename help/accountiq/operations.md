---
title: Operationer i konto IQ
description: I Account IQ ingår att vidta åtgärder för att utföra automatisering och gruppåtgärder på prenumerantkonton och spåra deras effekter.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Operationer {#operations-tab-next-steps}

När du har analyserat prenumerantens användningsmönster och identifierat förekomster av lösenordsdelning för ett markerat segment med hjälp av [!DNL Account IQ]-analyser, kan du vidta riktade åtgärder via fokuserade procedurer som kallas åtgärder i [!DNL Account IQ].

Med **åtgärder** kan du effektivt spåra och hantera delning av autentiseringsuppgifter med en grupp konton för att minska lösenordsdelning och förbättra upplevelsen för värdefulla prenumeranter.

Du kan tillämpa åtgärder på ett definierat [segment](/help/accountiq/product-concepts.md#segment-def) för att adressera lösenordsdelning inom ett specifikt [tidsintervall](/help/accountiq/product-concepts.md#time-interval-def) och schemalägga åtgärden att köras vid ett framtida datum. Dessa åtgärder omfattar begränsningar för att minimera lösenordsdelning eller begränsningar för icke-delade konton.

När du använder åtgärder kan du inte bara ange åtgärder och deras omfattning, utan även mäta deras resultat.

Genom att utvärdera resultaten kan ni förfina er strategi för att optimera effekterna, oavsett om det är genom att konvertera låntagare, minska utbytet av autentiseringsuppgifter eller minska bortfallet.

Du kan utföra olika funktioner med åtgärder:

* [Visa åtgärdsrapporter](#operation-reports)
* [Skapa en ny åtgärd](#create-new-operation)
* [Stoppa åtgärd](#stop-operation)

## Visa åtgärdsrapporter {#operation-reports}

Du kan granska effekterna av en åtgärd med hjälp av åtgärdsrapporter. Om du vill visa åtgärdsrapporten väljer du fliken **Åtgärder** under **Åtgärder** i den vänstra panelen av Account IQ-programmet. En lista över åtgärder som är tillgängliga i systemet visas. Du kan komma åt nyckelinformation om varje åtgärd i tabellformat. Detaljerna är följande:

* Åtgärdens namn
* Aktuell status (som Schemalagd, Körs, Avslutad, Fel eller Stoppad)
* Procent slutförda förlopp
* Målgrupp eller segment som åtgärden ska tillämpas på
* Typ av åtgärd som har valts för åtgärden
* Startdatum för åtgärden
* Slutdatum för åtgärden
* Datum när åtgärden skapades
* Senaste ändringsdatum för åtgärden

![](assets/operations-page.png)

*Lista och information om befintliga åtgärder i Account IQ*

Välj önskat **åtgärdsnamn** i listan över åtgärder. Följande rapporter visas:

### Åtgärdsprestanda {#operation-performance}

Åtgärdens prestanda ger en översta radavläsning som sammanfattar antalet konton som påverkas, operationens förlopp och det övergripande delningsresultatet för kontona i segmentet under åtgärdens [utvärderingsperiod](/help/accountiq/product-concepts.md#evaluation-period-def).

![Åtgärdens prestandarapport](assets/operation-performance.png)

*Åtgärdens prestandarapport*

**A.** Konton som påverkas **B.** Åtgärdsförlopp **C.** Allmän delningspoäng

#### Påverkade konton {#impacted-accounts}

Det här numret visar antalet abonnentkonton som påverkas av åtgärden som utförts under åtgärdens utvärderingsperiod.

#### Åtgärdsförlopp {#operation-progress}

Den här mätaren visar antalet dagar och procentandelen av åtgärden som slutfördes utanför det planerade schemat.

#### Total delning {#overall-sharing-score}

Det här linjediagrammet representerar det [övergripande delningsresultatet](/help/accountiq/data-panels.md#overall-sharing-score), som inkluderar delningsnivån och användningen från delade konton under varje vecka under åtgärdens utvärderingsperiod.

### Åtgärdseffekt: konton i segment {#impact-accounts}

Den här rapporten visas som ett skiktat stapeldiagram som illustrerar effekten av en åtgärd över tid.

![Åtgärdens påverkan på konton i segmentdiagram](assets/accounts-in-segment.png)

*Åtgärdens påverkan på konton i segmentdiagram*

X-axeln representerar åtgärdens [utvärderingsperiod](/help/accountiq/product-concepts.md#evaluation-period-def), medan y-axeln anger statusen för konton i åtgärdens segment. Varje stapel i diagrammet är uppdelad i tre färger:

* Rosa representerar antalet konton som uppfyller segmentets villkor som används i den här åtgärden.

* Blue representerar antalet aktiva konton som ursprungligen fanns i segmentet men som inte uppfyllde segmentets villkor under varje vecka eller månad i åtgärdens [utvärderingsperiod](/help/accountiq/product-concepts.md#evaluation-period-def).

* Grå representerar konton som var inaktiva under utvärderingsperioden.

>[!NOTE]
>
>Den första rosa stapeln representerar antalet konton som uppfyller villkoren för operationssegmentet i början av utvärderingsperioden.

Med tiden visar diagrammet förändringar i kontots beteende i förhållande till de ursprungliga villkoren (t.ex. har en delningssannolikhet på mer än 90 och fler än 5 enheter har blivit inaktiva).

### Åtgärdseffekt: mått för delade konton {#impact-shared-accounts}

Måtten för delade konton ger en översikt över delningsnivå och uppspelningsbegäranden från prenumerantkonton i åtgärdens segment under åtgärdens [utvärderingsperiod](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Delningsnivå {#share-level}

Det här linjediagrammet representerar [delningsnivån](/help/accountiq/data-panels.md#sharing-level) varje vecka under åtgärdens utvärderingsperiod.

![Raddiagram för delningsnivå](assets/share-level.png){width="550" align="left"}

*Raddiagram för delningsnivå*

#### Antal uppspelningsbegäranden {#play-requests}

Det här linjediagrammet representerar [uppspelningsbegäranden](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) varje vecka i åtgärdens utvärderingsperiod.

![Antal linjediagram för uppspelningsbegäranden](assets/number-play-requests.png){width="550" align="left"}

*Antal linjediagram för uppspelningsbegäranden*

### Åtgärdseffekt: allmänna användningsmått {#impact-general-usage}

De allmänna användningsmåtten ger en översikt över det genomsnittliga antalet enheter, IP-adresser och platser i åtgärdens segment under åtgärdens [utvärderingsperiod](/help/accountiq/product-concepts.md#evaluation-period-def).

#### Antal enheter {#devices}

Det här linjediagrammet representerar det genomsnittliga [antalet enheter](/help/accountiq/general-usage-reports.md#devices-week-account) varje vecka i åtgärdens utvärderingsperiod.

![Antal enheter, linjediagram](assets/number-devices.png){width="550" align="left"}

*Antal enheter, linjediagram*

#### Antal IP-adresser och platser {#IPs-locations}

Det här linjediagrammet representerar det genomsnittliga [antalet IP-adresser](/help/accountiq/general-usage-reports.md#ip-week-account) och [platser](/help/accountiq/general-usage-reports.md#locations-week-account) varje vecka i åtgärdens utvärderingsperiod.

![Antal IP-adresser och platslinjediagram](assets/number-ips-locations.png){width="550" align="left"}

*Antal IP-adresser och platslinjediagram*

Om du vill stänga rapporten och gå tillbaka till huvudsidan **Åtgärder** väljer du fliken **Åtgärder** under **Åtgärder** i den vänstra panelen.

## Skapa ny åtgärd {#create-new-operation}

När du går till fliken **Åtgärder** under **Åtgärder** i den vänstra panelen väljer du **Skapa ny åtgärd** högst upp på sidan **Åtgärder** .

Följ instruktionerna i följande avsnitt för att skapa en ny åtgärd:

* [Operationsinformation](#operation-details)
* [Segment](#segment)
* [Åtgärd](#action)
* [Schema](#schedule)

### Operationsinformation {#operation-details}

I det här avsnittet skriver du åtgärdens namn i **Åtgärdsnamn**.

>[!TIP]
>
>Beskriv syftet med åtgärden eller åtgärdstypen i **åtgärdsnamnet** för snabb identifiering. Alternativet **Lägg till beskrivning och taggar** är tillgängligt i framtida versioner.

![Lägg till åtgärdsnamn i åtgärdsinformation](assets/operation-details.png)

*Lägg till åtgärdsnamn*

### Segment {#segment}

I det här avsnittet klickar du på **Markera segment** och väljer ett segment som du vill använda den här åtgärden på. Lär dig [hur du markerar ett segment](/help/accountiq/segments-timeinterval.md#segment-selection).

När du har markerat ett segment ska du använda <img alt= "expandera segmentsammanfattning" src="./assets/expand-segment-summary.svg" width="25"> om du vill visa den detaljerade segmentsammanfattningen. Läs mer om [segmentsammanfattning](segments-timeinterval.md#segment-summary).

![Välj segment och tidsintervall](assets/select-segment-timeinterval.png)

*Välj segment och tidsintervall*

>[!NOTE]
>
>De [videokategorier](product-concepts.md#video-category-def) som visas i den föregående bilden, till exempel **MVPDs**, **Programmers** och **Channels**, representerar etiketterna som används i TV Everywhere-versionen av Account IQ. Om du är inloggad som en D2C-tjänst visar etiketterna företagets specifika videokategorier.

Använd vid behov ikonen <img alt= "redigera segment" src="./assets/edit-segment.svg" width="25"> om du vill redigera det markerade segmentet eller  Ikonen <img alt= "skapa nytt segment" src="./assets/create-new-segment.svg" width="25"> om du vill skapa ett nytt segment. Mer information finns i instruktionerna för att [skapa ett nytt segment](work-with-segments.md#create-new-segment) eller [redigera ett segment](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**Segmenttypen** med namnet **[!UICONTROL Fixed number of accounts]** är markerad som standard. Alternativet att välja **[!UICONTROL Variable number of accounts]** är tillgängligt i kommande versioner.

Välj **Kornighet och tidsintervall** om du vill övervaka åtgärden under en viss period. Läs mer om [hur du väljer granularitet och tidsintervall](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Åtgärd {#action}

I det här avsnittet väljer du en **åtgärd** som du vill utföra på det markerade segmentet i listrutan.

![Välj typ av åtgärd](assets/apply-actions.png)

*Välj typ av åtgärd*

Det finns två alternativ:

* Välj **CM-princip** för det samtidighetsövervakningssystem som är integrerat med Account IQ.

* Välj **Externa åtgärder** om du vill skapa och bearbeta arbetsflöden utanför Account IQ som inte är integrerade med Account IQ-systemet.

>[!NOTE]
>
>Externa åtgärder kanske inte alltid har att göra med lösenordsdelning, men de kan ändå påverka det, till exempel lanseringen av en ny årstid.

### Schema {#schedule}

I det här avsnittet väljer du **Startdatum** och **Slutdatum** i datumväljaren för att ange aktiveringen för åtgärden.

>[!IMPORTANT]
>
>För närvarande är standardaktiveringen **Startdatum** och **Slutdatum** inställd på **På datum**. Alternativet att välja **När ett villkor är uppfyllt** och **manuellt** är tillgängligt i kommande versioner.

>[!NOTE]
>
>Kontrollera att både startdatum och slutdatum är i linje med granulariteten som valts för utvärdering i **steg 4**.

* Om du har valt granularitet aggregerat efter veckor, väljer du start- och slutdatum i veckor (till exempel Vecka 10).
* Om du har valt granularitet aggregerad per månad väljer du start- och slutdatum i månader.

![Välj startdatum och slutdatum i datumväljaren](assets/add-schedule.png)

*Välj Startdatum och Slutdatum i datumväljaren*

**A.** Startdatumväljare **B.** Slutdatumväljare

>[!NOTE]
>
>**Startdatumet** måste vara senare än både utvärderingsperioden och det aktuella datumet, medan **slutdatumet** måste vara senare än startdatumet och det aktuella datumet för att schemalägga och köra åtgärder i den kommande perioden.

Välj **Spara åtgärd** längst upp på sidan **Åtgärder** om du vill bearbeta en ny åtgärd.

## Stoppa åtgärd {#stop-operation}

Du kan bara stoppa de åtgärder som för närvarande har statusen **Kör**. Så här avbryter du en befintlig åtgärd:

1. Gå till fliken **Åtgärder** under **Åtgärder** i den vänstra navigeringen i Account IQ-programmet.
1. Välj menyn **Alternativ** för den åtgärd som du vill stoppa.

   ![Välj en alternativmeny för att avbryta åtgärden](assets/stop-operation.png)

   *Välj Alternativ-menyn för att avbryta åtgärden*

1. Välj **Stopp**.



