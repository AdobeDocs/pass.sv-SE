---
title: Utvärdering av förebyggande av spårning Apple Safari
description: Utvärdering av förebyggande av spårning Apple Safari
source-git-commit: 579ce868b6ee94e1854bbc51145fc7840268db26
workflow-type: tm+mt
source-wordcount: '1826'
ht-degree: 0%

---

# Utvärdering av förebyggande av spårning - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Safari 10 {#safari10}

**Information**

Från och med Safari 10 kommer standardsekretessinställningarna för webbläsare att få funktionerna för enkel inloggning (SSO), enkel utloggning (SLO) och passiv autentisering att sluta fungera. Enkel inloggning (SSO) och passiv autentisering fungerar inte ens i samma session mellan flera flikar eller webbläsarfönster.

Dessa ändringar påverkar och påverkar Adobe Pass autentiseringsprocesser för följande versioner av AccessEnabler JavaScript SDK: v2 (version 2.x), v3 (version 3.x), v4 (version 4.x).

### Minska {#mitigation-safari10}

För att begränsa dessa begränsningar kan du instruera användaren att ändra sekretessinställningarna för webbläsaren Safari 10 och använda &quot;**Tillåt alltid**&quot; för &quot;**Cookies och webbplatsdata**&quot; på fliken Sekretess i webbläsaren från Inställningar, enligt bilden nedan.

![](assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Information**

>[!IMPORTANT]
>
>All information ovan från avsnitt Safari 10 gäller fortfarande för Safari 11.

Från och med Safari 11 introduceras [Intelligent spårningsförebyggande](https://webkit.org/blog/7675/intelligent-tracking-prevention/)(ITP), en teknik som använder heuristik för att förhindra spårning mellan webbplatser. Dessa heuristika påverkar hur cookies från tredje part lagras och spelas upp på nätverksanrop, vilket innebär att Safari blockerar cookies från tredje part i kommunikationen mellan klient och servermodell beroende på vilken ITP-mekanism som aktiveras.

Adobe Pass autentiseringstjänst använder och förlitar sig på cookies som en del av autentiseringsprocessen **för att fungera**. I situationer där autentiseringsprocessen sker automatiskt (t.ex. Tillfälligt pass) eller i implementeringar som använder iFrames eller &quot;omdirigeringsfri&quot; funktionalitet, betraktas Adobe cookies som cookies från tredje part och blockeras som standard. I alla andra fall använder Safari en maskininlärningsalgoritm som kan flagga alla tjänstcookies i Adobe Pass Authentication som spårningscookies, vilket innebär att ITP blockerar.

Sammanfattningsvis kan det hända att en användare av webbläsaren Safari 11 inte kan autentisera på en webbplats som stöder Adobe Pass Authentication efter aktiveringen av ITP-mekanismen (Intelligent Tracking Prevention), särskilt när användaren använder flera webbplatser som stöder Adobe Pass Authentication. Därför kan användarens autentiseringsupplevelse vara oväntad och odefinierad, från oförmåga till inloggning, till kortare än förväntad autentiseringstid.

Dessa ändringar påverkar och påverkar Adobe Pass autentiseringsprocesser för följande versioner av AccessEnabler JavaScript SDK: v2 (version 2.x), v3 (version 3.x).

### Minska {#mitigation-safari11}

För både AccessEnabler JavaScript SDK v3 (version 3.x) och AccessEnabler JavaScript SDK v4 (version 4.x) innehåller biblioteket en mekanism som kan identifiera de situationer där användarens autentisering blockerades på grund av att nödvändiga cookies saknas. I dessa situationer utlöser biblioteket ett specifikt felanrop [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference), som skickas tillbaka till webbplatsen med aktiverad Adobe Pass-autentisering för att användas som en signal som instruerar användaren att vidta åtgärder som kan minska problemet. För att kunna dra nytta av denna mekanism måste webbplatsen implementera [Felrapportering](/help/authentication/error-reporting.md) -specifikation.

För AccessEnabler JavaScript SDK v2 (version 2.x) har biblioteket inte den mekanism som beskrivs ovan, och därför kan webbplatsen som stöder Adobe Pass Authentication inte signaleras när användaren ska instrueras att vidta åtgärder för att åtgärda problemet.

Listan över åtgärder som kan mildra ovannämnda problem **gäller för alla tre versionerna** av AccessEnabler JavaScript SDK.

När [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) felåteranrop tas emot av den som implementerar webbplatsen. Användaren bör instrueras att inaktivera ITP (Intelligent Tracking Prevention) och aktivera cookies från tredje part genom att:

* Om Mac OS X High Sierra och senare används avmarkering av **Förhindra spårning över flera webbplatser**&quot; för &quot;**Webbplatsspårning**&quot; på fliken Sekretess i webbläsaren från Inställningar, enligt bilden nedan.

  ![](assets/uncheck-prvnt-cr-st-tr-safari11.png)


* För Mac OS X Sierra och tidigare: Kontrollera &quot;**Tillåt alltid**&quot; för &quot;**Cookies och webbplatsdata**&quot; på fliken Sekretess i webbläsaren från Inställningar, enligt bilden nedan.

  ![](assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Information**

>[!IMPORTANT]
>
>All information ovan från avsnitt Safari 10 och avsnitt Safari 11 gäller fortfarande för Safari 12.

I det här avsnittet beskrivs kompatibilitetsproblemen för **AccessEnabler JavaScript SDK version 4.x** på Safari 12.

>[!NOTE]
>
>Tänk på att om AccessEnabler JavaScript SDK version 2.x och AccessEnabler JavaScript SDK version 3.x båda använder cookies från tredje part för autentiseringsprocesserna, och på grund av ITP- och tredjepartsprinciper som börjar med Safari 11 kan användarens autentiseringsprocess vara oväntad och odefinierad, från oförmåga till inloggning, till kortare än förväntat autentiseringslängd.


### Certifierad funktionalitet i AccessEnabler JavaScript SDK v4 (version 4.x) i Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

**Autentisering** flöden som använder användarinteraktion fungerar alltid, även om användarens webbläsare har cookies från tredje part inaktiverade, eftersom AccessEnabler JavaScript SDK från och med version 4.0 inte längre använder cookies från tredje part för autentiseringsprocesserna.

>[!NOTE]
>
>Användaren MÅSTE interagera med webbplatsen för att kunna öppna popup-fönster för inloggning och/eller interagera med inloggningssidan för MVPD.

**Autentisering/preflight/Användarmetadata** -åtgärder fungerar fullt ut, förutsatt att användaren redan är autentiserad.

### Kända fel i AccessEnabler JavaScript SDK v4 (version 4.x) i Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO och SLO

   * På grund av hur localStorage implementeras i Safari från och med Safari 10 kan JS SDK inte längre dela inloggningstillståndet via en gemensam domän iFrame. Det innebär att användaren måste logga in på alla webbplatser som använder AccessEnabler JavaScript SDK. Utloggning tar inte heller bort autentiseringstoken över webbplatser, och därför måste användaren logga ut från varje webbplats som är aktiverad för Adobe Pass-autentisering.

* Temporärt pass

   * För tillfälliga pass använder AccessEnabler JavaScript SDK en individualiseringsmekanism för att låsa en autentiseringstoken till en specifik enhet (webbläsarinstans). På grund av nya mekanismer i Safari 12, som utformats för att förhindra spårning, använder vi det fingeravtryck vi använder i personaliseringsmekanismen **kommer att vara samma för alla användare som har samma IP-adress**. Vi tar faktiskt klientens IP-adress i beaktande för personalisering, men även detta påverkar användare som delar samma offentliga IP-adress. För dessa användare kommer vi att beräkna samma individualiserings-ID och det tillfälliga passet kommer att vara knutet till det. Det innebär att när en sådan användare använder ett tillfälligt pass har ingen annan åtkomst till det \! Detta påverkar särskilt företagsanvändare, utbildningsinstitutioner eller andra organisationer som har flera användare som använder NAT eller en gemensam proxy för att få tillgång till internet.

>[!NOTE]
>
>Det här problemet påverkar bara användare om den som implementerar använder ett tillfälligt pass som ett resultat av användarinteraktion, annars kan autentisering med tillfälligt pass **Automatiska flöden** nedan.

* Automatiska flöden

   * Autentiseringsflöden som har gjorts i ett automatiserat läge utan användarinteraktion lyckas inte i Safari 12 när JS SDK 4.0 används. Observera att den kommande JS SDK 4.1 åtgärdar alla problem med automatiserade flöden.

Användningsfall som påverkas av detta problem:

* Automatisk autentisering med TempPass (kostnadsfri förhandsgranskning) - för sådana flöden genererar SDK ett N130-fel.

* Passiv autentisering (misslyckas tyst) - användaren uppmanas att välja detta MVPD och ange inloggningsuppgifter

### Minska {#mitigation-safari12}

**SSO och SLO**

Det finns ingen känd begränsning tillgänglig eller möjlig för tillfället. Apple introducerade ett &quot;Lagringsåtkomst-API&quot; i Safari 12 (`https://webkit.org/blog/8124/introducing-storage-access-api`), men den aktuella implementeringen gäller inte för localStorage utan endast för cookies. API:t kräver dessutom användarinteraktion för att kunna användas, och när du väl har använt det får användaren också en behörighetsdialogruta som liknar den nedan.

![](assets/permission-dialog-apple.png)


I nuläget uppfyller dessa Safari-krav/-uppmaningar inte våra UX-krav och vi har inte något konsekvent beteende som i andra webbläsare, där enkel inloggning&quot;fungerar&quot; när vi har sparat en token i en gemensam domän localStorage.

**Temporärt pass**

För att minska personaliseringsproblemen och få en användarinteraktion rekommenderar vi att du använder **[Kampanjtillfälligt pass](/help/authentication/promotional-temp-pass.md)** på ett interaktivt sätt och ange minst en ytterligare information om användaren (till exempel e-postadress).

## Safari 13 {#safari13}

**Information**

>[!IMPORTANT]
>
>All information ovan från avsnitt Safari 10 till avsnitt Safari 12 gäller fortfarande för Safari 13.


Från och med Safari 13 introducerar webbläsaren nya ändringar i [Intelligent spårningsförebyggande](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP), vilket gör det heuristiskt bakom mekanismen hårdare när det gäller att flagga cookies från tredje part som spårningscookies, för att förhindra spårning mellan webbplatser.

Så som beskrivs i tidigare avsnitt använder och förlitar sig Adobe Pass Authentication-tjänsten på cookies från tredje part som en del av autentiseringsprocesserna när implementerare använder AccessEnabler JavaScript SDK v2 (version 2.x) och AccessEnabler JavaScript SDK v3 (version 3.x). Jämfört med tidigare versioner av webbläsaren Safari när ITP sparade in efter att ha ägnat en stund åt att &quot;lära sig&quot; om interaktionen mellan användaren och de berörda parterna (programmerarens webbplatser och Adobe) blockerar webbläsaren Safari 13 från de första cookies från tredje part som anses spåra cookies i kommunikationen mellan klient och servermodell.

Sammanfattningsvis kan en användare av webbläsaren Safari 13 med största sannolikhet inte initiera nya autentiseringar på en Adobe Pass Authentication-aktiverad webbplats som använder en äldre version av AccessEnabler JavaScript SDK, v2 (version 2.x) eller v3 (version 3.x). Detta sker på grund av att alla nödvändiga tjänstcookies för Adobe Primetime Authentication blockeras av ITP, vilket gör att tjänsten inte kan uppfylla autentiseringsbegäran.

AccessEnabler JavaScript SDK v4 (version 4.x)-biblioteket använder inte cookies från tredje part för autentiseringsprocessen, och därför påverkas inte åtgärderna i det på något sätt av ändringarna i Safari 13.

### Minska {#mitigation-safari13}

Först och främst rekommenderar vi starkt **migrering till AccessEnabler JavaScript SDK version 4.x** att ha ett stabilt och förutsägbart beteende i Safari.

För det andra innehåller biblioteket, för AccessEnabler JavaScript SDK v3 (version 3.x), en mekanism som kan identifiera de situationer där användarautentisering blockerades på grund av att nödvändiga cookies saknas. I dessa fall utlöser biblioteket ett specifikt felanrop ([N130](/help/authentication/error-reporting.md#advanced-error-codes-reference)) som skickas tillbaka till webbplatsen med aktiverad Adobe Pass-autentisering för att användas som en signal som instruerar användaren att vidta åtgärder som kan minska problemet. För att kunna dra nytta av denna mekanism måste webbplatsen implementera [Felrapportering](/help/authentication/error-reporting.md) -specifikation.

För AccessEnabler JavaScript SDK v2 (version 2.x) har biblioteket inte den mekanism som beskrivs ovan, och därför kan webbplatsen som stöder Adobe Pass Authentication inte signaleras när användaren ska instrueras att vidta åtgärder för att åtgärda problemet.

När [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) felåteranrop tas emot av den som implementerar webbplatsen. Användaren bör instrueras att inaktivera ITP (Intelligent Tracking Prevention) och aktivera cookies från tredje part genom att:

* Om Mac OS X High Sierra och senare används avmarkering av **Förhindra spårning över flera webbplatser**&quot; för &quot;**Webbplatsspårning**&quot; på fliken Sekretess i webbläsaren från Inställningar, enligt bilden nedan.

  ![](assets/prvnt-cross-site-tr-safari13.png)

* För Mac OS X Sierra och tidigare: Kontrollera</span>han &quot;**Tillåt alltid**&quot; för &quot;**Cookies och webbplatsdata**&quot; på fliken Sekretess i webbläsaren från Inställningar, enligt bilden nedan.

  ![](assets/always-allow-safari13.png)
