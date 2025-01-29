---
title: Android SDK API Reference
description: Android SDK API Reference
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '4560'
ht-degree: 0%

---

# (Äldre) Android SDK API Reference {#android-sdk-api-reference}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

## Introduktion {#intro}

Det här dokumentet innehåller information om de metoder och återanrop som används för autentisering med Android SDK for Adobe Pass, som stöds av Adobe Pass Authentication version 1.7 och senare. De metoder och återanropsfunktioner som beskrivs här definieras i huvudfilerna AccessEnabler.h och EntitlementDelegate.h.

Besök [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) för den senaste Android AccessEnabler SDK.


**Obs!** Adobe Pass-autentiseringsteamet uppmanar dig att endast använda Adobe Pass-autentiserings *publika* API:er:

- Offentliga API:er är tillgängliga *och har testats fullt ut* för alla klienttyper som stöds. För alla offentliga funktioner ser vi till att varje klienttyp har en motsvarande version av de associerade metoderna.</span>
- Offentliga API:er måste vara så stabila som möjligt för att ge stöd för bakåtkompatibilitet och se till att partnerintegreringar inte bryts. Men för *icke*-publika API:er reserverar vi oss för att ändra deras signatur vid en framtida tidpunkt. Om du stöter på ett visst flöde som inte stöds genom en kombination av de aktuella API-anropen för Adobe Pass-autentisering är det bästa sättet att tala om det för oss. Med tanke på dina behov kan vi ändra de offentliga API:erna och tillhandahålla en stabil lösning som går framåt.

## ANDROID API {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- förauktorisera
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [utloggning](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [selectedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

### Factory.getInstance {#getInstance}

**Beskrivning:** Instansierar åtkomstaktiveringsobjektet. Det ska finnas en enda instans av Access Enabler per programinstans.

| API-anrop: konstruktor |
| --- |
| **public static** AccessEnabler **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **utlöser** AccessEnablerException <br><br>**public static** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**utlöser** AccessEnablerException |

**Tillgänglighet:** v3.1.2+

**Parametrar:**

- *appContext*: Android-programkontext.
- env\_url: för testning med Adobe staging environment kan env\_url anges till sp.auth-staging.adobe.com

**Inaktuell:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Beskrivning:** Anger programmerarens identitet. Varje programmerare tilldelas ett unikt ID när den registreras hos Adobe för Adobe Pass autentiseringssystem. När det gäller enkel inloggning och fjärrtoken kan autentiseringstillståndet ändras när programmet är i bakgrunden. Det går att anropa setRequestor igen när programmet försätts i förgrunden för att synkronisera med systemtillståndet (hämta en fjärrtoken om enkel inloggning är aktiverad eller ta bort den lokala token om en utloggning inträffar under tiden).

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till programmerarens identitet. Serversvaret används internt av åtkomstaktiveringskoden. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för programmet via callback-funktionen setRequestorComplete().

Om parametern *urls* inte används anger det resulterande nätverksanropet standardtjänstleverantörens URL: Adobe Release/Production Environment.

Om ett värde anges för parametern *urls*, anger det resulterande nätverksanropet alla URL:er som anges i parametern *urls* som mål. Alla konfigurationsbegäranden aktiveras samtidigt i olika trådar. Den första svararen har företräde när listan över MVPD kompileras. För varje MVPD i listan kommer Access Enabler ihåg URL:en till den associerade tjänsteleverantören. Alla efterföljande tillståndsbegäranden dirigeras till den URL som är associerad med tjänstleverantören som parats med mål-MVPD under konfigurationsfasen.

| API-anrop: konfiguration för begärare |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Tillgänglighet:** v3.0+

| API-anrop: konfiguration för begärare |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Tillgänglighet:** v3.0+


**Parametrar:**

- *requestedID*: Det unika ID som är associerat med programmeraren. Skicka det unika ID som tilldelats av Adobe till din webbplats när du först registrerade dig hos Adobe Pass autentiseringstjänst.

- *signedRequestorID*: En kopia av begärande-ID:t som har signerats digitalt med din privata nyckel. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *urls*: Valfri parameter. Som standard används Adobes tjänstleverantör (http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. Då består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder MVPD.

**Återanrop har utlösts:** `setRequestorComplete()`

Föråldrat:

    public void setRequestor(String requestId, String signedRequestorId)
    
    public void setRequestor (String requestId, String signedRequestorId, ArrayList&lt;String> urls)

[Tillbaka till Android API...](#api)

### setRequestorComplete {#setRequestorComplete}

**Beskrivning:** Återanrop som utlöses av åtkomstaktiveraren och som informerar programmet om att konfigurationsfasen är slutförd. Detta är en signal om att programmet kan börja utfärda tillståndsbegäranden. Inga berättigandebegäranden kan utfärdas av programmet förrän konfigurationsfasen är slutförd.

| Återanrop: konfigurationen för begärande har slutförts |
| --- |
| java public void setRequestorComplete(int-status) |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *status*: Kan ha något av följande värden:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - konfigurationsfasen har slutförts
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - konfigurationsfasen misslyckades
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - konfigurationsfasen har slutförts
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - konfigurationsfasen misslyckades

**Utlöses av:** `setRequestor()`

[Tillbaka till Android API...](#api)

### setOptions {#setOptions}

**Beskrivning:** Konfigurerar globala SDK-alternativ. Det accepterar en **karta\&lt;String, String\>** som ett argument. Värdena från kartan skickas till servern tillsammans med alla nätverksanrop som SDK gör.

Värdena skickas till servern oberoende av det aktuella flödet (autentisering/auktorisering). Om du vill ändra värdena kan du anropa den här metoden när som helst.

| API-anrop: setOptions |
| --- |
| public void setOptions(HashMap&lt;String,String> options) |

**Tillgänglighet:** v1.9.2+

**Parametrar:**

- *alternativ*: En karta&lt;String, String> som innehåller globala SDK-alternativ. Följande alternativ är tillgängliga:
   - **applicationProfile** - Den kan användas för att skapa serverkonfigurationer baserat på det här värdet.
   - **ap_vi** - Experience Cloud-ID (visitorID). Det här värdet kan användas senare för avancerade analysrapporter.
   - **ap_ai** - Advertising-id
   - **device_info** - Klientinformation som beskrivs här: [Överför klientinformationsenhetsanslutning och program](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Till början...](#apis)


### checkAuthentication {#checkAuthN}

**Beskrivning:** Kontrollerar autentiseringsstatusen. Det gör du genom att söka efter en giltig autentiseringstoken i det lokala tokenlagringsutrymmet. Den här metoden utför inga nätverksanrop och vi rekommenderar att du anropar den på huvudtråden. Den används av programmet för att fråga om användarens autentiseringsstatus och uppdatera användargränssnittet i enlighet med detta (d.v.s. uppdatera användargränssnittet för inloggning/utloggning). Autentiseringsstatusen meddelas programmet via återanropet [*setAuthenticationStatus()*](#setAuthNStatus).

Om en MVPD stöder funktionen &quot;Autentisering per begärande&quot; kan flera autentiseringstoken lagras på en enhet.  Mer information om den här funktionen finns i avsnittet [Riktlinjer för cachelagring](#$caching) i Android tekniska översikt.

| API-anrop: kontrollera autentiseringsstatus |
| --- |
| public void checkAuthentication() |

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus()`

[Tillbaka till Android API...](#api)


### getAuthentication {#getAuthN}

**Beskrivning:** Startar det fullständiga autentiseringsarbetsflödet. Det börjar med att kontrollera autentiseringsstatusen. Om autentiseringen inte redan har autentiserats startas tillståndsdatorn för autentiseringsflödet:

- Om det senaste autentiseringsförsöket lyckades hoppas MVPD-markeringsfasen över och återanropet [*navigateToUrl()*](#navigagteToUrl) aktiveras. Programmet använder det här återanropet för att instansiera WebView-kontrollen som visar MVPD inloggningssida.
- Om det senaste autentiseringsförsöket misslyckades eller om användaren uttryckligen loggade ut, utlöses callback-funktionen [*displayProviderDialog()*](#displayProviderDialog). Programmet använder det här återanropet för att visa användargränssnittet för val i MVPD. Ditt program krävs också för att återuppta autentiseringsflödet genom att informera åtkomstaktiveringsbiblioteket om användarens MVPD-val via metoden [setSelectedProvider()](#setSelectedProvider) .

När användarens inloggningsuppgifter verifieras på MVPD inloggningssida måste ditt program övervaka de omdirigeringsåtgärder som utförs medan användaren autentiseras på inloggningssidan för MVPD. När rätt inloggningsuppgifter anges omdirigeras WebView-kontrollen till en anpassad URL som definieras av konstanten *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* . Denna URL är inte avsedd att läsas in av WebView. Programmet måste tolka denna URL och tolka händelsen som en signal om att inloggningsfasen är slutförd. Den bör sedan lämna över kontrollen till Access Enabler för att slutföra autentiseringsflödet (genom att anropa metoden *getAuthenticationToken()* ).

Om en MVPD stöder funktionen &quot;Autentisering per begärande&quot; kan flera autentiseringstoken lagras på en enhet (en per programmerare).  Mer information om den här funktionen finns i avsnittet [Riktlinjer för cachelagring](#$caching) i Android tekniska översikt.

Slutligen kommuniceras autentiseringsstatusen till programmet via callback-funktionen *setAuthenticationStatus()*.



| API-anrop: initierar autentiseringsflödet |
| --- |
| public void getAuthentication() |

**Tillgänglighet:** v1.0+

| API-anrop: initierar autentiseringsflödet |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Tillgänglighet:** v1.8+

**Parametrar:**

- *forceAuthn*: En flagga som anger om autentiseringsflödet ska startas, oavsett om användaren redan är autentiserad eller inte.
- *data*: En karta som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Tillbaka till Android API...](#api)

### displayProviderDialog {#displayProviderDialog}

**Beskrivning** Återanrop utlöses av Access Enabler för att informera programmet om att lämpliga gränssnittselement måste initieras så att användaren kan välja önskad MVPD. I återanropet finns en lista med MVPD-objekt med ytterligare information som kan hjälpa dig att skapa den valda gränssnittspanelen korrekt (t.ex. URL:en som pekar på MVPD logotyp, visningsnamn osv.)

När användaren har valt önskad MVPD måste programmet i det övre lagret återuppta autentiseringsflödet genom att anropa *setSelectedProvider()* och skicka det ID för MVPD som motsvarar användarens val.

>[!NOTE]
>
> Avbryter autentiseringsflödet
> </br></br>
> Observera att detta är en punkt där användaren kan trycka på knappen &quot;Bakåt&quot;, vilket motsvarar att autentiseringsflödet avbryts. I ett sådant fall måste ditt program anropa metoden `setSelectedProvider()` och skicka *null* som parameter för att ge Access Enabler möjlighet att återställa sin autentiseringstillståndsdator.

| Återanrop: visa användargränssnittet för val av MVPD |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Tillgänglighet:** v1.0+

**Parametrar**:

- *mvpds*: Lista med MVPD-objekt som innehåller MVPD-relaterad information som programmet kan använda för att skapa MVPD-element för markeringsgränssnitt.

**Utlöses av:** `getAuthentication(), getAuthorization()`

[Tillbaka till Android API...](#api)


### setSelectedProvider {#setSelectedProvider}

**Beskrivning:** Den här metoden anropas av ditt program för att informera Access Enabler om användarens val i MVPD. Programmet kan använda den här metoden för att välja eller ändra tjänsteleverantören som används för autentisering.

Om den valda MVPD är en TempPass MVPD autentiseras den automatiskt med den MVPD-versionen utan att du behöver anropa getAuthentication() efteråt.

Observera att detta inte är möjligt för kampanjtillfälligt pass där extra parametrar anges för getAuthentication()-metoden.

När *null* skickas som en parameter antar Access Enabler att användaren har avbrutit autentiseringsflödet (d.v.s. tryckt på knappen &quot;Bakåt&quot;) och svarar genom att återställa autentiseringstillståndsdatorn och genom att anropa *setAuthenticationStatus()* -återanropet med felkoden `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR`.

| API-anrop: ange den valda providern |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Tillbaka till Android API...](#api)


### navigateToUrl {#navigagteToUrl}

**Borttagen:** NavigateToUrl används från och med Android SDK 3.0 endast om det inte finns någon anpassad Chrome-flik på enheten

**Beskrivning:** Återanrop utlöses av Access Enabler som informerar programmet om att MVPD inloggningssida måste visas för att användaren ska kunna ange sina inloggningsuppgifter. Access Enabler skickar URL:en till inloggningssidan för MVPD som parameter. Programmet krävs för att instansiera en WebView-kontroll och dirigera den till den här URL:en. Dessutom måste programmet övervaka de URL:er som läses in av WebView-kontrollen och avbryta omdirigeringsåtgärden för den anpassade URL:en som definieras av konstanten `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)`. Vid den här händelsen måste programmet antingen stänga eller dölja WebView-kontrollen och återlämna kontrollen till åtkomstaktiveringsbiblioteket genom att anropa metoden *getAuthenticationToken()*. Åtkomstaktiveraren slutför autentiseringsflödet genom att hämta autentiseringstoken från backend-servern och lagra den lokalt i tokenlagringen.

>[!WARNING]
>
> **Autentiseringsflödet avbryts** <br>Observera att det här är en punkt där användaren kan trycka på knappen &quot;Bakåt&quot;, vilket motsvarar att autentiseringsflödet avbryts. I ett sådant fall måste ditt program anropa metoden _setSelectedProvider()_ som skickar _null_ som parameter och ge åtkomstaktiveraren möjlighet att återställa sin autentiseringstillståndsdator.

| Återanrop: visa MVPD inloggningssida |
| --- |
| public void navigateToUrl(String url) |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *url*: URL:en som pekar på MVPD inloggningssida

**Utlöses av:** `getAuthentication(), setSelectedProvider()`

[Tillbaka till Android API...](#api)


### getAuthenticationToken {#getAuthNToken}

**Föråldrat:** Från och med Android SDK 3.0 används den här metoden inte längre från programmet eftersom Chrome anpassad flik används för autentisering.

**Beskrivning:** Slutför autentiseringsflödet genom att begära en autentiseringstoken från backend-servern. Den här metoden ska bara anropas av ditt program som svar på en händelse där WebView-kontrollen som är värd för MVPD inloggningssida omdirigeras till den anpassade URL som definieras av konstanten `AccessEnabler.ADOBEPASS_REDIRECT_URL`.

| API-anrop: hämta autentiseringstoken |
| --- |
| public void getAuthenticationToken() |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *cookies*: Cookies som har angetts på måldomänen (en referensimplementering finns i demoprogrammet i SDK).

**Återanrop har utlösts:** `setAuthenticationStatus()`, `sendTrackingData()`

[Tillbaka till Android API...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Beskrivning:** Återanrop utlöses av åtkomstaktiveraren som informerar
tillämpningen av autentiseringsflödets status. Det finns många
platser där det här flödet kan misslyckas, antingen som ett resultat av användarens
interaktion eller på grund av andra oförutsedda scenarier (t.ex. nätverk
anslutningsproblem osv.). Det här återanropet informerar programmet om
autentiseringsflödets status för lyckade/misslyckade, men även
vid behov lämna ytterligare information om orsaken till felet.

| Återanrop: rapportera autentiseringsflödets status |
| --- |
| public void setAuthenticationStatus(int-status, String errorCode) |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *status*: Kan ha något av följande värden:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - autentiseringsflödet har slutförts
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - autentiseringsflödet misslyckades
- *kod*: Felorsak. Om *status* är `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` är *code* en tom sträng (d.v.s. definierad av konstanten `AccessEnablerConstants.USER_AUTHENTICATED`). Vid fel kan den här parametern ha något av följande värden:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - Användaren är inte autentiserad. Som svar på metodanropet *checkAuthentication()* när det inte finns någon giltig autentiseringstoken i den lokala tokencachen.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler har återställt autentiseringstillståndsdatorn efter att programmet i det övre skiktet skickade *null* till `setSelectedProvider()` för att avbryta autentiseringsflödet.  Förmodligen har användaren avbrutit autentiseringsflödet (d.v.s. tryckt på knappen &quot;Bakåt&quot;).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - Autentiseringsflödet misslyckades på grund av exempelvis att nätverket inte är tillgängligt eller att användaren uttryckligen avbröt autentiseringsflödet.

**Utlöses av:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Tillbaka till Android API...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Föråldrat:** Från och med Android SDK 3.6 ersätter förauktoriserings-API checkPreauthorizedResources och ger utökade felkoder.

**Beskrivning:** Den här metoden används av programmet för att avgöra om användaren redan har behörighet att visa specifika skyddade resurser. Det främsta syftet med den här metoden är att hämta information som ska användas för att dekorera användargränssnittet (till exempel indikera åtkomststatus med lås- och upplåsningsikoner).

| API-anrop: ange den valda providern |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Tillgänglighet:** v1.3+

**Parametrar:** Parametern `resources` är en array med resurser för vilka auktorisering ska kontrolleras. Varje element i listan ska vara en sträng som representerar resurs-ID:t. Resurs-ID har samma begränsningar som resurs-ID:t i `getAuthorization()`-anropet, d.v.s. det ska vara ett avtalat värde mellan Programmer och MVPD eller ett mediets RSS-fragment.

**Återanropet har utlösts:** `preauthorizedResources()`

[Tillbaka till Android API...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Föråldrat:** Från och med Android SDK 3.6 ersätter förauktoriserings-API checkPreauthorizedResources och ger utökade felkoder.

**Beskrivning:** Den här metoden används av programmet för att avgöra om användaren redan har behörighet att visa specifika skyddade resurser. Det främsta syftet med den här metoden är att hämta information som ska användas för att dekorera användargränssnittet (till exempel indikera åtkomststatus med lås- och upplåsningsikoner).

| API-anrop: ange den valda providern |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Tillgänglighet:** v3.1+

**Parametrar:** Parametern `resources` är en array med resurser för vilka auktorisering ska kontrolleras. Varje element i listan ska vara en sträng som representerar resurs-ID:t. Resurs-ID har samma begränsningar som resurs-ID:t i `getAuthorization()`-anropet, d.v.s. det ska vara ett avtalat värde mellan Programmer och MVPD eller ett mediets RSS-fragment.

Parametern `cache` anger om cachelagrat preauktoriseringssvar kan användas eller inte. Som standard är cache-minnet true och SDK returnerar ett tidigare cachelagrat svar om det är tillgängligt.

**Återanropet har utlösts:** `preauthorizedResources()`

[Tillbaka till Android API...](#api)

### preauthorizedResources {#preauthResources}

**Föråldrat:** Från och med Android SDK 3.6 ersätter förauktoriserings-API checkPreauthorizedResources och ger utökade felkoder. callback-funktionen preauthorizedResources anropas inte för det nya API:t.


**Beskrivning:** Återanrop utlöses av checkPreauthorizedResources(). Visar en lista över resurser som användaren redan har behörighet att visa.

| API-anrop: ange den valda providern |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Tillgänglighet:** v1.3+

**Parametrar:** Parametern `resources` är en array med resurser som användaren redan har behörighet att visa.

**Utlöses av:** `checkPreauthorizedResources()`

[Tillbaka till Android API...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Beskrivning:** Den här metoden används av programmet för att kontrollera auktoriseringsstatusen. Det börjar med att kontrollera autentiseringsstatusen först. Om den inte autentiseras aktiveras callback-funktionen *setTokenRequestFailed()* och metoden avslutas. Om användaren är autentiserad utlöses även auktoriseringsflödet. Se information om metoden *getAuthorization()*.

| API-anrop: kontrollera auktoriseringsstatus |
| --- |
| public void checkAuthorization(String resourceId) |

**Tillgänglighet:** v1.0+

| API-anrop: kontrollera auktoriseringsstatus |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Tillgänglighet:** v1.8+

**Parametrar:**

- *resourceId*: ID:t för resursen som användaren begär auktorisering för.
- *data*: En karta som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Tillbaka till Android API...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Beskrivning:** Den här metoden används av programmet för att initiera auktoriseringsflödet. Om användaren inte redan är autentiserad initieras även autentiseringsflödet. Om användaren autentiseras fortsätter Access Enabler att utfärda begäranden för auktoriseringstoken (om det inte finns någon giltig auktoriseringstoken i det lokala token-cachen) och för den kortlivade medietoken. När den korta medietoken har hämtats anses auktoriseringsflödet vara slutfört. Återanropet *setToken()* aktiveras och den korta medietoken levereras som en parameter till programmet. Om auktoriseringen misslyckas av någon anledning aktiveras callback-funktionen *tokenRequestFailed()* och felkoden och informationen anges.

| API-anrop: initiera auktoriseringsflödet |
| --- |
| public void getAuthorization(String resourceId) |

**Tillgänglighet:** v1.0+

| API-anrop: initiera auktoriseringsflödet |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Tillgänglighet:** v1.8+

**Parametrar:**

- *resourceId*: ID:t för resursen som användaren begär auktorisering för.
- *data*: En karta som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Ytterligare återanrop har utlösts** <br> Den här metoden kan även utlösa följande återanrop (om autentiseringsflödet också initieras): *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**OBS! Använd checkAuthorization() i stället för getAuthorization() när det är möjligt. Metoden getAuthorization() startar ett fullständigt autentiseringsflöde (om användaren inte är autentiserad), vilket kan leda till en komplicerad implementering från programmeraren.**

[Tillbaka till Android API...](#api)


### setToken {#setToken}

**Beskrivning:** Återanrop som utlöses av åtkomstaktiveraren och som informerar programmet om att auktoriseringsflödet har slutförts. Den kortlivade medietoken levereras också som en parameter.

| Återanrop: auktoriseringsflödet har slutförts |
| --- |
| public void setToken(String-token, String resourceId) |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *token*: Den kortlivade medietoken
- *resourceId*: Resursen som auktoriseringen hämtades för

**Utlöses av:** `checkAuthorization()`, `getAuthorization()`


[Tillbaka till Android API...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Beskrivning:** Återanrop som utlöses av Access Enabler och som informerar programmet i det övre lagret om att auktoriseringsflödet misslyckades.

| Återanrop: auktoriseringsflödet misslyckades |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *resourceId*: Resursen som auktoriseringen hämtades för
- *errorCode*: Felkod som är associerad med felscenariot. Möjliga värden:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - Användaren kunde inte auktorisera för den angivna resursen
- *errorDescription*: Ytterligare information om felscenariot. Om den här beskrivande strängen inte är tillgänglig av någon anledning skickar Adobe Pass Authentication en tom sträng **(&quot;&quot;)**.

  Strängen kan användas av en MVPD för att skicka anpassade felmeddelanden eller försäljningsrelaterade meddelanden. Om en prenumerant nekas behörighet för en resurs kan MVPD skicka ett meddelande som:&quot;Du har för närvarande inte åtkomst till den här kanalen i ditt paket. Om du vill uppgradera ditt paket klickar du här.&quot; Meddelandet skickas av Adobe Pass Authentication via det här återanropet till programmeraren, som har möjlighet att visa eller ignorera det. Adobe Pass Authentication kan också använda den här parametern för att ge ett meddelande om det tillstånd som kan ha orsakat ett fel. &quot;Ett nätverksfel uppstod t.ex. vid kommunikation med leverantörens behörighetstjänst.&quot;

**Utlöses av:** `checkAuthorization(), getAuthorization()`

[Tillbaka till Android API...](#api)

### utloggning {#logout}

**Beskrivning:** Använd den här metoden för att initiera utloggningsflödet. Utloggningen är resultatet av en serie HTTP-omdirigeringsåtgärder på grund av att användaren måste loggas ut både från Adobe Pass autentiseringsservrar och från MVPD servrar. Därför kan det här flödet inte slutföras med en enkel HTTP-begäran som utfärdas av biblioteket för Access Enabler. SDK använder anpassade Chrome-flikar för att utföra HTTP-omdirigeringsåtgärder. Det här flödet visas för användaren och stängs när det är klart

| API-anrop: initiera utloggningsflödet |
| --- |
| public void logOut() |

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:**

- `navigateToUrl()` för SDK-version före 3.0
- `setAuthenticationStatus()` för SDK version > 3.0


[Tillbaka till Android API...](#api)


### getSelectedProvider {#getSelectedProvider}

**Beskrivning:** Använd den här metoden för att fastställa den valda providern.

| API-anrop: fastställa vilken MVPD som är vald |
| --- |
| public void getSelectedProvider() |

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `selectedProvider()`

[Tillbaka till Android API...](#api)


### <span id="selectedProvider"></span>selectedProvider

**Beskrivning:** Återanrop som utlöses av Access Enabler och som ger information om den MVPD som är vald till programmet.

| Återanrop: information om den valda MVPD |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Tillgänglighet:** v1.0+

**Parametrar:**

- *mvpd*: Objekt som innehåller information om den valda MVPD

**Utlöses av:** `getSelectedProvider()`

[Tillbaka till Android API...](#api)


### getMetadata {#getMetadata}

**Beskrivning:** Använd den här metoden för att hämta information som exponeras som metadata av åtkomstaktiveringsbiblioteket. Programmet kan komma åt den här informationen genom att tillhandahålla ett sammansatt MetadataKey-objekt.

| API-anrop: fråga AccessEnabler om metadata |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Tillgänglighet:** v1.0+

Programmerarna har två typer av metadata:

- Statiska metadata (autentiseringstoken TTL, auktoriseringstoken TTL och enhets-ID)
- Användarmetadata (användarspecifik information, t.ex. användar-ID och postnummer, som skickas från en MVPD till en användares enhet under autentiserings- och/eller auktoriseringsflödena)

**Parametrar:**

- *metadataKey*: En datastruktur som kapslar in en nyckel- och args-variabel, med följande innebörd:
   - Om nyckeln är `METADATA_KEY_USER_META` och args innehåller ett SerializableNameValuePair-objekt med namnet = `METADATA_ARG_USER_META` och värdet = `[metadata_name]` ställs frågan efter användarens metadata. Aktuell lista över tillgängliga metadatatyper för användare:
      - `zip` - Postnummer

      - `householdID` - Hushållsidentifierare. Om en MVPD inte stöder underkonton är detta identiskt med `userID`.

      - `maxRating` - Högsta föräldraklassificering för användaren

      - `userID` - användaridentifieraren. Om en MVPD stöder underkonton och användaren inte är huvudkontot, kommer `userID` att vara annorlunda än `householdID`.

      - `channelID` - En lista över kanaler som användaren har rätt att visa
   - Om nyckeln är `METADATA_KEY_DEVICE_ID` ställs frågan för att hämta aktuellt enhets-ID. Observera att den här funktionen är inaktiverad som standard och programmerare bör kontakta Adobe för att få information om aktivering och avgifter.
   - Om nyckeln är `METADATA_KEY_TTL_AUTHZ` och args innehåller ett SerializableNameValuePair-objekt med namnet = `METADATA_ARG_RESOURCE_ID` och värdet = `[resource_id]`, görs frågan för att hämta förfallotiden för den auktoriseringstoken som är associerad med den angivna resursen.
   - Om nyckeln är `METADATA_KEY_TTL_AUTHN` görs frågan för att hämta förfallotiden för autentiseringstoken.



>[!NOTE]
>
>För SDK 3.4.0 är konstanterna `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` tillgängliga från com.adobe.adobepass.accessibility.api.profile.UserProfileService.



>[!NOTE]
>
>Vilka användarmetadata som är tillgängliga för en programmerare beror på vad en MVPD gör tillgänglig.  Listan utökas ytterligare när nya metadata blir tillgängliga och läggs till i Adobe Pass autentiseringssystem.

**Återanrop har utlösts:** [`setMetadataStatus()`](#setMetadaStatus)

**Mer information:** [Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

[Tillbaka till Android API...](#api)

### setMetadataStatus {#setMetadaStatus}

**Beskrivning:** Återanrop som utlöses av Access Enabler som levererar begärda metadata via ett *getMetadata()*-anrop.

| Återanrop: resultatet av begäran om hämtning av metadata |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *key*: MetadataKey-objektet som innehåller nyckeln som metadatavärdet begärs för och associerade parametrar (se demoprogrammet för en referensimplementering).
- *result*: Ett sammansatt objekt som innehåller begärda metadata. Objektet har följande fält:
   - *simpleResult*: En sträng som representerar metadatavärdet när begäran gjordes för autentiserings-TTL, auktoriserings-TTL eller enhets-ID. Det här värdet är null om begäran gjordes för användarmetadata.

   - *userMetadataResult*: Ett objekt som innehåller Java-representationen av en nyttolast för JSON-användarmetadata.\
     Exempel:

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

översätts till Java som:

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**Den faktiska strukturen för användarens metadataobjekt liknar följande:**

```json
          {
              updated: 1334243471,
              encrypted: ["encryptedProp"],
              data: {
                  zip: ["12345", "34567"],
                  maxRating: { 
                      "MPAA": "PG-13",
                      "VCHIP": "TV-Y", 
                      "URL": "http://exam.pl/e/manage/ratings"
                  },
                  householdID: "3456",
                  userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                  channelID: ["channel-1", "channel-2"]
              }
          }
```

Det här värdet är null när begäran gjordes för enkla metadata (Authentication TTL, Authorization TTL eller Device ID).

- *krypterad*: Booleskt värde som anger om de hämtade metadata är krypterade eller inte. Den här parametern är bara viktig för användarmetadatabegäranden, den har ingen betydelse för statiska metadata (t.ex. autentiserings-TTL) som alltid tas emot okrypterat. Om den här parametern är inställd på True, är det programmeraren som ska hämta det okrypterade användarmetadatavärdet genom att utföra en RSA-dekryptering med den privata nyckeln för vitlistning (samma privata nyckel som används för signering av begärande-ID:t i [`setRequestor`](#setRequestor)-anropet).

**Utlöses av:** [`getMetadata()`](#getMetadata)

**Mer information:** [Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)


[Tillbaka till Android API...](#api)


### getVersion {#getVersion}

**Beskrivning:** Den här metoden kan användas för att hämta versionen av AccessEnabler-biblioteket.

| API-anrop: get AccessEnabler-version |
| --- |
| ```public static String getVersion()``` |


[Tillbaka till Android API...](#api)

</br>

## Spårningshändelser {#tracking}

Åtkomstaktiveraren utlöser ett extra återanrop som inte nödvändigtvis är relaterat till berättigandeflödena. Det är valfritt att implementera återanropsfunktionen för händelsespårning med namnet *sendTrackingData()*, men programmet kan spåra specifika händelser och sammanställa statistik som antalet lyckade/misslyckade autentiserings-/autentiseringsförsök. Nedan finns specifikationen för callback-funktionen *sendTrackingData()*:


### sendTrackingData {#sendTrackingData}

**Beskrivning:** Återanrop som utlöses av åtkomstaktiveraren som signalerar till programmet om förekomsten av olika händelser, till exempel slutförande/misslyckande av autentiserings-/auktoriseringsflöden. Enhetstypen, klienttypen Access Enabler och operativsystemet rapporteras också av sendTrackingData().

>[!WARNING]
>
> Enhetstypen och operativsystemet härleds genom användning av ett offentligt Java-bibliotek ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) och användaragentsträngen. Observera att denna information endast tillhandahålls som ett grovt sätt att dela upp mätvärden för drift i enhetskategorier, men att Adobe inte kan ta något ansvar för felaktiga resultat. Använd den nya funktionen i enlighet med detta.


- Möjliga värden för enhetstypen:
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Möjliga värden för klienttypen Åtkomstaktivering:
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| Återanrop: spåra händelser |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *event*: händelsen som spåras. Det finns tre typer av spårningshändelser:
   - **permissionDetection:** när en auktoriseringstokenbegäran returnerar (händelsetypen är `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** när en autentiseringskontroll inträffar (händelsetypen är `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** när användaren väljer en MVPD i MVPD-markeringsformuläret (händelsetypen är `EVENT_MVPD_SELECTION`)
- *data*: ytterligare data som är associerade med den rapporterade händelsen. Dessa data presenteras i form av en lista med värden.

Här följer instruktioner för tolkning av värdena i *data*
array:

- För händelsetyp *`EVENT_AUTHN_DETECTION`:*
   - **0** - Anger om tokenbegäran lyckades (true/false) och om ovanstående är true:
   - **1** - MVPD ID-sträng
   - **2** - GUID (md5 hashed)
   - **3** - Token finns redan i cache (sant/falskt)
   - **4** - Enhetstyp
   - **5** - klienttyp för åtkomstaktivering
   - **6** - Operativsystemtyp

- För händelsetyp `EVENT_AUTHZ_DETECTION`
   - **0** - Anger om tokenbegäran lyckades (true/false) och om den lyckades:
   - **1** - MVPD ID
   - **2** - GUID (md5 hashed)
   - **3** - Token finns redan i cache (sant/falskt)
   - **4** - Fel
   - **5** - information
   - **6** - Enhetstyp
   - **7** - klienttyp för åtkomstaktivering
   - **8** - Operativsystemtyp

- För händelsetyp `EVENT_MVPD_SELECTION`
   - **0** - ID för den valda MVPD
   - **1** - Enhetstyp
   - **2** - klienttyp för åtkomstaktivering
   - **3** - Operativsystemtyp

**Utlöses av:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Tillbaka till Android API...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
