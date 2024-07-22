---
title: API-referens för inbyggda Amazon FireOS-klienter
description: API-referens för inbyggda Amazon FireOS-klienter
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '3428'
ht-degree: 0%

---

# API-referens för inbyggda Amazon FireOS-klienter {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Introduktion {#intro}

Det här dokumentet innehåller information om de metoder och återanrop som används av Amazon FireOS SDK för Adobe Pass-autentisering, som stöds av Adobe Pass-autentisering. De metoder och återanropsfunktioner som beskrivs här definieras i huvudfilerna AccessEnabler.h och EntitlementDelegate.h.

Information om den senaste SDK:n för Amazon FireOS AccessEnabler finns i <https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library>.

>[!NOTE]
>
>Adobe Pass-autentiseringsteamet uppmanar dig att endast använda API:er för Adobe Pass-autentisering *public*:

- Offentliga API:er är tillgängliga *och har testats fullt ut* för alla klienttyper som stöds. För alla offentliga funktioner ser vi till att varje klienttyp har en motsvarande version av de associerade metoderna.
- Offentliga API:er måste vara så stabila som möjligt för att ge stöd för bakåtkompatibilitet och se till att partnerintegreringar inte bryts. Men för *icke*-publika API:er reserverar vi oss för att ändra deras signatur vid en framtida tidpunkt. Om du stöter på ett visst flöde som inte stöds genom en kombination av de aktuella API-anropen för Adobe Pass-autentisering är det bästa sättet att tala om det för oss. Med tanke på dina behov kan vi ändra de offentliga API:erna och tillhandahålla en stabil lösning som går framåt.

## Amazon FireOS SDK API {#api}

- [getInstance](#getInstance)
- [setOptions](#fire_setOption)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- [checkPreauthorizedResources](#checkPreauth)
- [preauthorizedResources](#preauthResources)
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

</br>

### Factory.getInstance {#getInstance}

**Beskrivning:** Instansierar åtkomstaktiveringsobjektet. Det ska finnas en enda instans av Access Enabler per programinstans.

| API-anrop: konstruktor |
| --- |
| ```public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException```<br><br> |
| ```public static AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl) throws AccessEnablerException``` |

**Tillgänglighet:** v3.0+


**Parametrar:**

- *appContext*: Programkontext för Amazon Fire OS.
- softwareStatement
- redirectUrl : för FireOS ignoreras parametervärdet och ställs in på standard: adobepass://android.app
- env_url: för testning med Adobe staging environment kan env\_url anges till sp.auth-staging.adobe.com

**Inaktuell:**

```java
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```


### setRequestor {#setRequestor}

**Beskrivning:** Anger programmerarens identitet. Varje programmerare tilldelas ett unikt ID när den registreras hos Adobe för Adobe Pass autentiseringssystem. Den här inställningen ska endast utföras en gång under programmets livscykel.

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till programmerarens identitet. Serversvaret används internt av åtkomstaktiveringskoden. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för programmet via callback-funktionen setRequestorComplete().

Om parametern *urls* inte används anger det resulterande nätverksanropet standardtjänstleverantörens URL: Adobe Release/Production Environment.

Om ett värde anges för parametern *urls*, anger det resulterande nätverksanropet alla URL:er som anges i parametern *urls* som mål. Alla konfigurationsbegäranden aktiveras samtidigt i olika trådar. Den första svararen har företräde när listan över MVPD kompileras. För varje MVPD i listan kommer åtkomstaktiveringen att komma ihåg URL:en för den associerade tjänstleverantören. Alla efterföljande tillståndsbegäranden dirigeras till den URL som är associerad med tjänstleverantören som parats med mål-MVPD under konfigurationsfasen.

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
- *urls*: Valfri parameter. Som standard används Adobes tjänstleverantör (http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. När detta görs består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder det MVPD.

**Återanrop har utlösts:** `setRequestorComplete()`



**Inaktuell:**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestorComplete {#setRequestorComplete}

**Beskrivning:** Återanrop som utlöses av åtkomstaktiveraren och som informerar programmet om att konfigurationsfasen är slutförd. Detta är en signal om att programmet kan börja utfärda tillståndsbegäranden. Inga berättigandebegäranden kan utfärdas av programmet förrän konfigurationsfasen är slutförd.

| Återanrop: konfigurationen för begärande har slutförts |
| --- |
| ```public void setRequestorComplete(int status)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *status*: Kan ha något av följande värden:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - konfiguration
fasen har slutförts
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - konfiguration
fas misslyckades

**Utlöses av:** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**Beskrivning:** Konfigurerar globala SDK-alternativ. Det accepterar en **karta\&lt;String, String\>** som ett argument. Värdena från kartan skickas till servern tillsammans med alla nätverksanrop som SDK gör.

Värdena skickas till servern oberoende av det aktuella flödet (autentisering/auktorisering). Om du vill ändra värdena kan du anropa den här metoden när som helst.



| API-anrop: setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**Tillgänglighet:** v3.0+

**Parametrar:**

- *alternativ*: En karta\&lt;String, String\> som innehåller globala SDK-alternativ. Följande alternativ är tillgängliga:
   - **applicationProfile** - Den kan användas för att skapa serverkonfigurationer baserat på det här värdet.
   - **ap\_vi** - Experience Cloud ID-tjänsten. Det här värdet kan användas senare för avancerade analysrapporter.
   - **device\_info** - Enhetsinformation enligt beskrivningen i **Överför enhetsinformation till cookbook**

</br>

### checkAuthentication {#checkAuthN}

**Beskrivning:** Kontrollerar autentiseringsstatusen. Det gör du genom att söka efter en giltig autentiseringstoken i det lokala tokenlagringsutrymmet. Om du anropar den här metoden utförs inga nätverksanrop. Den används av programmet för att fråga om användarens autentiseringsstatus och uppdatera användargränssnittet i enlighet med detta (d.v.s. uppdatera användargränssnittet för inloggning/utloggning). Autentiseringsstatusen meddelas programmet via återanropet [*setAuthenticationStatus()*](#setAuthNStatus).

Om ett MVPD-dokument har stöd för funktionen &quot;Authentication per Requestor&quot; kan flera autentiseringstoken lagras på en enhet.

| API-anrop: kontrollera autentiseringsstatus |
| --- |
| ```public void checkAuthentication()``` |

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**Beskrivning:** Startar det fullständiga autentiseringsarbetsflödet. Det börjar med att kontrollera autentiseringsstatusen. Om autentiseringen inte redan har autentiserats startas tillståndsdatorn för autentiseringsflödet:

- Om det senaste autentiseringsförsöket lyckades hoppas valfasen över och en WebView-kontroll visar användarens inloggningssida.
- Om det senaste autentiseringsförsöket misslyckades eller om användaren uttryckligen loggade ut, utlöses callback-funktionen [*displayProviderDialog()*](#displayProviderDialog). Programmet använder det här återanropet för att visa användargränssnittet för MVPD-val. Ditt program måste också återuppta autentiseringsflödet genom att informera åtkomstaktiveringsbiblioteket om användarens MVPD-val via metoden [setSelectedProvider()](#setSelectedProvider) .

Om ett MVPD-dokument har stöd för funktionen &quot;Authentication per Requestor&quot; kan flera autentiseringstoken lagras på en enhet (en per Programmer).

Slutligen kommuniceras autentiseringsstatusen till programmet via callback-funktionen *setAuthenticationStatus()*.

| API-anrop: initierar autentiseringsflödet |
| --- |
| ```public void getAuthentication()``` |

**Tillgänglighet:** v1.0+

| API-anrop: initierar autentiseringsflödet |
| --- |
| ```public void getAuthentication(boolean forceAuthN, Map<String, Object> genericData)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *forceAuthn*: En flagga som anger om autentiseringsflödet ska startas, oavsett om användaren redan är autentiserad eller inte.
- *data*: En karta som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**Beskrivning** Återanrop som utlöses av Access Enabler för att informera programmet om att lämpliga gränssnittselement måste instansieras så att användaren kan välja önskat MVPD. I återanropet finns en lista med MVPD-objekt med ytterligare information som kan hjälpa dig att skapa den valda gränssnittspanelen korrekt (t.ex. URL:en som pekar på MVPD:s logotyp, visningsnamn osv.)

När användaren har valt önskat MVPD måste programmet i det övre lagret återuppta autentiseringsflödet genom att anropa *setSelectedProvider()* och skicka ID:t för det MVPD som motsvarar användarens val.


| **Återanrop: visa användargränssnittet för MVPD-markering** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**Tillgänglighet:** v1.0+

**Parametrar**:

- *mvpds*: Lista med MVPD-objekt som innehåller MVPD-relaterad information som programmet kan använda för att skapa gränssnittselement för MVPD-val.

**Utlöses av:** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**Beskrivning:** Den här metoden anropas av ditt program för att informera åtkomstaktiveraren om användarens MVPD-val. När *null* skickas som en parameter återställdes det aktuella MVPD-värdet till null.

| **API-anrop: ange den valda providern** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**Tillgänglighet:**v 1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**Beskrivning:** Återanrop som utlöses av åtkomstaktiveraren i Android SDK. Den ska ignoreras på Amazon FireOS SDK.

| **Återanrop: visa MVPD-inloggningssida** |
| --- |
| ```public void navigateToUrl(String url)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *url*: URL:en som pekar på MVPD:s inloggningssida

**Utlöses av:** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**Beskrivning:** Slutför autentiseringsflödet genom att begära en autentiseringstoken från backend-servern.

| **API-anrop: hämta autentiseringstoken** |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *cookies*: Cookies som har angetts på måldomänen (se demoprogrammet i SDK för en referensimplementering).

**Återanrop har utlösts:** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**Beskrivning:** Återanrop utlöses av åtkomstaktiveraren som informerar programmet om autentiseringsstatusen. Det finns många platser där autentiseringsflödet kan misslyckas, antingen som ett resultat av användarinteraktion eller på grund av andra oförutsedda scenarier (t.ex. nätverksanslutningsproblem). Det här återanropet informerar programmet om autentiseringsstatus för lyckade/misslyckade, och ger även ytterligare information om felorsaken när det behövs.

Det här återanropet signalerar också när utloggningsflödet är klart.

| **Återanrop: rapportera autentiseringsflödets status** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *status*: Kan ha något av följande värden:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - autentiseringsflödet har slutförts
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - autentiseringsflödet misslyckades
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - utloggning
- *kod*: Orsak till presenterad status. Om *status* är `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` är *code* en tom sträng (d.v.s. definierad av konstanten `AccessEnabler.USER_AUTHENTICATED`). Om den inte är autentiserad kan den här parametern ha något av följande värden:
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - Användaren är inte autentiserad. Som svar på metodanropet *checkAuthentication()* när det inte finns någon giltig autentiseringstoken i den lokala tokencachen.
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler har återställt autentiseringstillståndsdatorn efter att programmet i det övre skiktet skickade *null* till `setSelectedProvider()` för att avbryta autentiseringsflödet.  Förmodligen har användaren avbrutit autentiseringsflödet (d.v.s. tryckt på knappen &quot;Bakåt&quot;).
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - Autentiseringsflödet misslyckades på grund av exempelvis att nätverket inte är tillgängligt eller att användaren uttryckligen avbröt autentiseringsflödet.
   - `AccessEnabler.LOGOUT` - Användaren är inte autentiserad på grund av en utloggningsåtgärd.

**Utlöses av:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorizedResources {#checkPreauth}

**Beskrivning:** Den här metoden används av programmet för att avgöra om användaren redan har behörighet att visa specifika skyddade resurser. Det främsta syftet med den här metoden är att hämta information som ska användas för att dekorera användargränssnittet (till exempel indikera åtkomststatus med lås- och upplåsningsikoner).

| **API-anrop: ange den valda providern** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Tillgänglighet:** v1.0+

**&lt;Parametrar:** Parametern `resources` är en array med resurser för vilka auktorisering ska kontrolleras. Varje element i listan ska vara en sträng som representerar resurs-ID:t. Resurs-ID har samma begränsningar som resurs-ID:t i `getAuthorization()`-anropet, d.v.s. det ska vara ett avtalat värde mellan Programmer och MVPD eller ett mediets RSS-fragment.

**Återanropet har utlösts:** `preauthorizedResources()`

</br>

### preauthorizedResources {#preauthResources}

**Beskrivning:** Återanrop utlöses av checkPreauthorizedResources(). Visar en lista över resurser som användaren redan har behörighet att visa.

| **API-anrop: ange den valda providern** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Tillgänglighet:**v 1.0+

**Parametrar:** Parametern `resources` är en array med resurser som användaren redan har behörighet att visa.

**Utlöses av:** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**Beskrivning:** Den här metoden används av programmet för att kontrollera auktoriseringsstatusen. Det börjar med att kontrollera autentiseringsstatusen först. Om den inte autentiseras aktiveras callback-funktionen *setTokenRequestFailed()* och metoden avslutas. Om användaren är autentiserad utlöses även auktoriseringsflödet. Se information om metoden *getAuthorization()*.

| **API-anrop: kontrollera auktoriseringsstatus** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**Tillgänglighet:** v1.0+

| **API-anrop: kontrollera auktoriseringsstatus** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *resourceId*: ID:t för resursen som användaren begär auktorisering för.
- *data*: En karta som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**Beskrivning:** Den här metoden används av programmet för att initiera auktoriseringsflödet. Om användaren inte redan är autentiserad initieras även autentiseringsflödet. Om användaren autentiseras fortsätter Access Enabler att utfärda begäranden för auktoriseringstoken (om det inte finns någon giltig auktoriseringstoken i det lokala token-cachen) och för den kortlivade medietoken. När den korta medietoken har hämtats anses auktoriseringsflödet vara slutfört. Återanropet *setToken()* aktiveras och den korta medietoken levereras som en parameter till programmet. Om auktoriseringen misslyckas av någon anledning aktiveras callback-funktionen *tokenRequestFailed()* och felkoden och informationen anges.

| **API-anrop: initiera auktoriseringsflödet** |
| --- |
| ```public void getAuthorization(String resourceId)``` |

**Tillgänglighet:** v1.0+

| **API-anrop: initiera auktoriseringsflödet** |
| --- |
| ```public void getAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *resourceId*: ID:t för resursen som användaren begär auktorisering för.
- *data*: En karta som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **Ytterligare återanrop har utlösts** <br>Den här metoden kan även utlösa följande återanrop (om autentiseringsflödet också initieras): _setAuthenticationStatus()_, _displayProviderDialog()_ |

**OBS! Använd checkAuthorization() i stället för getAuthorization() när det är möjligt. Metoden getAuthorization() startar ett fullständigt autentiseringsflöde (om användaren inte är autentiserad), vilket kan leda till en komplicerad implementering från programmeraren.**

</br>

### setToken {#setToken}

**Beskrivning:** Återanrop som utlöses av åtkomstaktiveraren och som informerar programmet om att auktoriseringsflödet har slutförts. Den kortlivade medietoken levereras också som en parameter.

| **Återanrop: auktoriseringsflödet har slutförts** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**Tillgänglighet:**v 1.0+

**Parametrar:**

- *token*: Den kortlivade medietoken
- *resourceId*: Resursen som auktoriseringen hämtades för

**Utlöses av:** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**Beskrivning:** Återanrop som utlöses av Access Enabler och som informerar programmet i det övre lagret om att auktoriseringsflödet misslyckades.

| **Återanrop: autentiseringsflödet misslyckades** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *resourceId*: Resursen som auktoriseringen hämtades för
- *errorCode*: Felkod som är associerad med felscenariot. Möjliga värden:
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - Användaren kunde inte auktorisera för den angivna resursen
- *errorDescription*: Ytterligare information om felscenariot. Om den här beskrivande strängen inte är tillgänglig av någon anledning skickar Adobe Pass Authentication en tom sträng >**(&quot;&quot;)**.  Strängen kan användas av ett MVPD-program för att skicka anpassade felmeddelanden eller försäljningsrelaterade meddelanden. Om en prenumerant nekas behörighet för en resurs kan MVPD skicka ett meddelande som:&quot;Du har för närvarande inte åtkomst till den här kanalen i ditt paket. Om du vill uppgradera ditt paket klickar du här.&quot; Meddelandet skickas av Adobe Pass Authentication via det här återanropet till programmeraren, som har möjlighet att visa eller ignorera det. Adobe Pass Authentication kan också använda den här parametern för att ge ett meddelande om det tillstånd som kan ha orsakat ett fel. &quot;Ett nätverksfel uppstod t.ex. vid kommunikation med leverantörens behörighetstjänst.&quot;

**Utlöses av:** `checkAuthorization(), getAuthorization()`

</br>

### utloggning {#logout}

**Beskrivning:** Använd den här metoden för att initiera utloggningsflödet. Utloggningen är resultatet av en serie HTTP-omdirigeringsåtgärder på grund av att användaren måste loggas ut både från Adobe Pass autentiseringsservrar och från MVPD-servrarna.

| **API-anrop: initiera utloggningsflödet** |
| --- |
| ```public void logout()``` |

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** Ingen

</br>

### getSelectedProvider {#getSelectedProvider}

**Beskrivning:** Använd den här metoden för att fastställa den valda providern.

| **API-anrop: fastställa det aktuella MVPD** |
| --- |
| ```public void getSelectedProvider()``` |

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `selectedProvider()`

</br>

### selectedProvider {#selectedProvider}

**Beskrivning:** Återanrop som utlöses av Access Enabler och som levererar information om det aktuella MVPD-värdet till programmet.

| **Återanrop: information om det MVPD som är markerat** |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *mvpd*: Objekt som innehåller information om det MVPD som är markerat

**Utlöses av:** `getSelectedProvider()`

</br>

### getMetadata {#getMetadata}

**Beskrivning:** Använd den här metoden för att hämta information som exponeras som metadata av åtkomstaktiveringsbiblioteket. Programmet kan komma åt den här informationen genom att tillhandahålla ett sammansatt MetadataKey-objekt.

| **API-anrop: fråga AccessEnabler efter metadata** |
| --- |
| ```public void getMetadata(MetadataKey metadataKey)``` |

**Tillgänglighet:** v1.0+

Programmerarna har två typer av metadata:

- Statiska metadata (autentiseringstoken TTL, auktoriseringstoken TTL och enhets-ID)
- Användarmetadata (användarspecifik information, t.ex. användar-ID och postnummer, som skickas från ett MVPD till en användares enhet under autentiserings- och/eller auktoriseringsflödena)

**Parametrar:**

- *metadataKey*: En datastruktur som kapslar in en nyckel- och args-variabel, med följande innebörd:
   - Om nyckeln är `METADATA_KEY_TTL_AUTHN` görs frågan för att hämta förfallotiden för autentiseringstoken.
   - Om nyckeln är `METADATA_KEY_TTL_AUTHZ` och args innehåller ett SerializableNameValuePair-objekt med namnet = `METADATA_ARG_RESOURCE_ID` och värdet = `[resource_id]`, görs frågan för att hämta förfallotiden för den auktoriseringstoken som är associerad med den angivna resursen.
   - Om nyckeln är `METADATA_KEY_DEVICE_ID` ställs frågan för att hämta aktuellt enhets-ID. Observera att den här funktionen är inaktiverad som standard och programmerare bör kontakta Adobe för att få information om aktivering och avgifter.
   - Om nyckeln är `METADATA_KEY_USER_META` och args innehåller ett SerializableNameValuePair-objekt med namnet = `METADATA_KEY_USER_META` och värdet = `[metadata_name]` ställs frågan efter användarens metadata. Aktuell lista över tillgängliga metadatatyper för användare:
      - `zip` - Postnummer
      - `householdID` - Hushållsidentifierare. Om ett MVPD inte stöder underkonton är detta identiskt med `userID`.
      - `maxRating` - Högsta föräldraklassificering för användaren
      - `userID` - användaridentifieraren. Om ett MVPD-dokument har stöd för underkonton och användaren inte är huvudkontot,
      - `channelID` - En lista över kanaler som användaren har rätt att visa

Vilka faktiska användarmetadata som är tillgängliga för en programmerare beror på vad ett separat programmeringsdokument (MVPD) erbjuder.  Listan utökas ytterligare när nya metadata blir tillgängliga och läggs till i Adobe Pass autentiseringssystem.

**Återanrop har utlösts:** [`setMetadataStatus()`](#setMetadaStatus)

**Mer information:** [Användarmetadata](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**Beskrivning:** Återanrop som utlöses av Access Enabler som levererar begärda metadata via ett *getMetadata()*-anrop.

| **Återanrop: resultatet av begäran om hämtning av metadata** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *key*: MetadataKey-objektet som innehåller nyckeln som metadatavärdet begärs för och associerade parametrar (se demoprogrammet för en referensimplementering).
- *result*: Ett sammansatt objekt som innehåller begärda metadata. Objektet har följande fält:
   - *simpleResult*: En sträng som representerar metadatavärdet när begäran gjordes för autentiserings-TTL, auktoriserings-TTL eller enhets-ID. Det här värdet är null om begäran gjordes för användarmetadata.

   - *userMetadataResult*: Ett objekt som innehåller Java-representationen av en nyttolast för JSON-användarmetadata. Exempel:

     ```json
     {
     "street": "Main Avenue",
     "buildings": ["150", "320"]
     }
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

**Mer information:** [Användarmetadata](/help/authentication/user-metadata.md)

</br>

## getVersion {#getVersion}

**Beskrivning:** Använd den här metoden för att hämta den aktuella versionen av AccessEnabler

| **API-anrop: get AccessEnabler version** |
| --- |
| ```public static String getVersion()``` |

## Spårningshändelser {#tracking}

Åtkomstaktiveraren utlöser ett extra återanrop som inte nödvändigtvis är relaterat till berättigandeflödena. Det är valfritt att implementera återanropsfunktionen för händelsespårning med namnet *sendTrackingData()*, men programmet kan spåra specifika händelser och sammanställa statistik som antalet lyckade/misslyckade autentiserings-/autentiseringsförsök. Nedan finns specifikationen för callback-funktionen *sendTrackingData()*:

### sendTrackingData {#sendTrackingData}

**Beskrivning:** Återanrop som utlöses av åtkomstaktiveraren som signalerar till programmet om förekomsten av olika händelser, till exempel slutförande/misslyckande av autentiserings-/auktoriseringsflöden. Enhetstypen, klienttypen Access Enabler och operativsystemet rapporteras också av sendTrackingData().

>[!WARNING]
>
> Enhetstypen och operativsystemet härleds genom användning av ett offentligt Java-bibliotek (http://java.net/projects/user-agent-utils) och användaragentsträngen. Observera att denna information endast tillhandahålls som ett grovt sätt att dela upp mätvärden för drift i enhetskategorier, men att Adobe inte kan ta något ansvar för felaktiga resultat. Använd den nya funktionen i enlighet med detta.

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
   - `tvos`
   - `android`
   - `firetv`

| Återanrop: spåra händelser |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Tillgänglighet:** v1.0+

**Parametrar:**

- *event*: händelsen som spåras. Det finns tre typer av spårningshändelser:
   - **permissionDetection:** när en auktoriseringstokenbegäran returnerar (händelsetypen är `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** när en autentiseringskontroll inträffar (händelsetypen är `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** när användaren väljer ett MVPD i MVPD-markeringsformuläret (händelsetypen är `EVENT_MVPD_SELECTION`)
- *data*: ytterligare data som är associerade med den rapporterade händelsen. Dessa data presenteras i form av en lista med värden.

Här följer instruktioner för tolkning av värdena i arrayen *data*:

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
   - **0** - ID för det MVPD som är markerat
   - **1** - Enhetstyp
   - **2** - klienttyp för åtkomstaktivering
   - **3** - Operativsystemtyp

**Utlöses av:** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
