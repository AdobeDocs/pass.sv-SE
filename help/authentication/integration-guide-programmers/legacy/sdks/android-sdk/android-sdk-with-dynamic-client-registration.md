---
title: Android SDK med Dynamic Client Registration
description: Android SDK med Dynamic Client Registration
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---

# (Äldre) Android SDK med registrering av dynamisk klient {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

## Introduktion {#Intro}

Android AccessEnabler SDK för Android har ändrats för att aktivera autentisering utan att använda sessionscookies. I takt med att fler och fler webbläsare begränsar åtkomsten till cookies måste en annan metod användas för att tillåta autentisering.

För Android begränsar användningen av Chrome anpassade flikar åtkomsten till cookies från andra program.

>I **Android SDK 3.0.0** introduceras:

- dynamisk klientregistrering ersätter den aktuella appregistreringsmekanismen baserat på autentisering av signerad begärande-ID och sessionscookie
- Chrome anpassade flikar för autentiseringsflöden

>[!NOTE]
>
>För äldre Android-versioner utan stöd för Chrome anpassade flikar används WebView-autentiseringen på samma sätt som i tidigare versioner av AccessEnabler SDK.


## Dynamisk klientregistrering {#DCR}

Android SDK v3.0+ använder den dynamiska klientregistreringsproceduren som definieras i [Översikt över dynamisk klientregistrering](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


## Demo {#Demo}

Titta på [det här webbinariet](https://my.adobeconnect.com/pzkp8ujrigg1/) som ger mer information om funktionen och innehåller en demonstration om hur du hanterar programsatser med TVE Dashboard och hur du testar genererade programsatser med ett demoprogram som tillhandahålls av Adobe som en del av Android SDK.

## API-ändringar {#API}


### Factory.getInstance

**Beskrivning:** Instansierar åtkomstaktiveringsobjektet. Det ska finnas en enda instans av Access Enabler per programinstans.

| API-anrop: konstruktor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        returnerar AccessEnablerException |


**Tillgänglighet:** v3.0+

**Parametrar:**

- *appContext*: Android-programkontext
- softwareStatement: värde från TVE Dashboard eller *null* om &quot;software\_statement&quot; har angetts i strings.xml
- redirectUrl : unique url, en av domänerna i omvänd ordning som uttryckligen lades till i TVE Dashboard eller *null* om &quot;redirect\_uri&quot; anges i strings.xml

Obs! Ogiltig softwareStatement eller redirectUrl gör att programmet inte initierar AccessEnabler eller registrerar program för Adobe Pass-autentisering och -auktorisering
</br>
Obs! Parametern redirectUrl eller redirect\_uri i strings.xml ska vara värdet för domänen som läggs till i TVE Dashboard för programmet i omvänd ordning (för t.ex.: för domänen adobe.com som läggs till i TVE Dashboard ska redirectUrl vara &#39;com.adobe&#39;.


### setRequestor

**Beskrivning:** Anger kanalens identitet. Varje kanal tilldelas ett unikt ID när den registreras hos Adobe för Adobe Pass autentiseringssystem. När det gäller enkel inloggning och fjärrtoken kan autentiseringstillståndet ändras när programmet är i bakgrunden. Det går att anropa setRequestor igen när programmet försätts i förgrunden för att synkronisera med systemtillståndet (hämta en fjärrtoken om enkel inloggning är aktiverad eller ta bort den lokala token om en utloggning inträffar under tiden).

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till kanalens identitet. Serversvaret används internt av åtkomstaktiveringskoden. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för programmet via callback-funktionen setRequestorComplete().

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

- *requestedID*: Det unika ID som är associerat med kanalen. Skicka det unika ID som tilldelats av Adobe till din webbplats när du först registrerade dig för Adobe Pass autentiseringstjänst.
- *urls*: Valfri parameter. Som standard används Adobes tjänstleverantör [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. Då består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder MVPD.

Föråldrat:

- *signedRequestorID*: En kopia av begärande-ID:t som har signerats digitalt med din privata nyckel. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Återanrop har utlösts:** `setRequestorComplete()`

### utloggning

**Beskrivning:** Använd den här metoden för att initiera utloggningsflödet. Utloggningen är resultatet av en serie HTTP-omdirigeringsåtgärder på grund av att användaren måste loggas ut både från Adobe Pass autentiseringsservrar och från MVPD servrar. Det innebär att det här flödet öppnar ett ChromeCustomTab-fönster för att köra utloggningen.

| API-anrop: initiera utloggningsflödet |
| --- |
| public void logOut() |

**Tillgänglighet:** v3.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus()`
</br></br>

## Programmeringsimplementeringsflöde {#Progr}

### **1. Registrera program**

a. Hämta software\_statement och redirect\_uri från Adobe Pass ( TVE Dashboard )

b. Det finns två alternativ för att skicka dessa värden till Adobe Pass SDK:

Lägg till i strings.xml:

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Anropa AccessEnabler.getInstance(appContext,softwareStatement,
redirectUrl)


### &#x200B;2. Konfigurera program

a. setRequestor(request\_id)

SDK kommer att göra följande:

- registreringsprogram: med **software\_statement** får SDK en **client\_id, client\_secrets, client\_id\_issued\_at, redirect\_uris, grant\_types**. Den här informationen lagras i programmets interna lagring.

- hämta en **access\_token** med client\_id, client\_secrets och grant\_type=&quot;client\_credentials&quot;. Denna åtkomst\_token kommer att användas för alla samtal som SDK gör till Adobe Pass-servrar

**Token Error Responses:**

| Felsvar | | |
| --- | --- | --- |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Begäran saknar en obligatorisk parameter, innehåller ett parametervärde som inte stöds (annat än anslagstyp), upprepar en parameter, innehåller flera autentiseringsuppgifter, använder mer än en mekanism för autentisering av klienten eller har på annat sätt fel format. |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Klientautentiseringen misslyckades eftersom klienten var okänd. SDK MÅSTE registrera sig på auktoriseringsservern igen. |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | Den autentiserade klienten har inte behörighet att använda den här behörighetstypen. |

- om en MVPD kräver passiv autentisering öppnas en anpassad Chrome-flik för att köra passivt med den MVPD:n och stängs när allt är klart

b. checkAuthentication()

- true : gå till Authorization
- false: gå till Select MVPD

c. getAuthentication : SDK inkluderar **access_token** i anropsparametrar

- mvpd kom ihåg : gå till setSelectedProvider(mvpd_id)
- mvpd är inte markerat: displayProviderDialog
- mvpd selected : go to setSelectedProvider(mvpd_id)

d. setSelectedProvider

- mvpd\_id-autentiserings-url har lästs in i ChromeCustomTabs
- inloggningen lyckades : delegate.setAuthenticationStatus ( SUCCESS)
- inloggningen avbröts: återställa MVPD-markering
- URL-schemat har etablerats som &quot;adobepass://redirect_uri&quot; för hämtning när autentiseringen är klar

e. get/checkAuthorization: SDK inkluderar **access_token** i huvudet som Authorization: Bearer **access_token**

- om auktoriseringen lyckas, kommer en uppmaning att inhämta
medietoken

f. utloggning:

- SDK kommer att ta bort giltig token för den aktuella begäraren (autentiseringar som erhållits av andra program och inte via enkel inloggning fortsätter att gälla)
- SDK öppnar Chrome anpassade flikar för att nå mvpd_id-utloggningsslutpunkten. När du är klar stängs Chrome anpassade flikar
- URL-schemat är &quot;adobepass://logout&quot; för att fånga in tidpunkten då utloggningen är klar
- utlöser en sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) och ett återanrop : setAuthenticationStatus(0,&quot;Logout&quot;)

**Obs!** eftersom varje anrop kräver en **access_token,** möjliga felkoder nedan hanteras i SDK.


| Felsvar | | |
| --- | ---|--- |
| invalid_request | 400 | Begäran har fel format. SDK bör sluta ringa anrop till servern. |
| invalid_client | 403 | Klient-ID:t har inte längre behörighet att utföra begäranden. SDK MÅSTE utföra klientregistreringen igen. |
| access_deny | 401 | Åtkomst\_token är inte giltig. SDK MÅSTE begära en ny access_token. |
