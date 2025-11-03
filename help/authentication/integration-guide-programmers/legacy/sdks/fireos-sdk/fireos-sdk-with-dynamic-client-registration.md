---
title: Amazon FireOS SDK med dynamisk klientregistrering
description: Amazon FireOS SDK med dynamisk klientregistrering
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 0%

---


# (Äldre) Amazon FireOS SDK med dynamisk klientregistrering {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

</br>

## <span id=""></span>Introduktion {#Intro}

FireOS AccessEnabler SDK for FireTV har ändrats för att aktivera autentisering utan sessionscookies. I takt med att fler och fler webbläsare begränsar åtkomsten till cookies behövdes en annan metod för att tillåta autentisering.

**FireOS SDK 3.0.4** ersätter den aktuella programregistreringsmekanismen baserat på autentisering av begärande-ID och sessionscookie med [Översikt över registrering av dynamisk klient](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


## API-ändringar {#API}

### Factory.getInstance

**Beskrivning:** Instansierar åtkomstaktiveringsobjektet. Det ska finnas en enda instans av Access Enabler per programinstans.

| API-anrop: konstruktor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        returnerar AccessEnablerException |

**Tillgänglighet:** v3.0+

**Parametrar:**

- *appContext*: Android-programkontext
- *softwareStatement*: värde från TVE Dashboard eller *null* om &quot;software\_statement&quot; har angetts i strings.xml
- *redirectUrl* : för FireTV-implementeringar ska den här parametern vara null. Alla inställningar för det här attributet ignoreras.

**Anteckningar**

- invalid softwareStatement kommer att göra så att programmet inte initierar AccessEnabler eller registrerar program för Adobe Pass Authentication och authentication
- redirectUrl-parametern för FireTV anges av SDK till adobepass://android.app eftersom autentiseringen hanteras av den unika AccessEnabler-instansen.

### setRequestor

**Beskrivning:** Anger kanalens identitet. Varje kanal tilldelas ett unikt ID när den registreras hos Adobe för Adobe Pass autentiseringssystem. När det gäller enkel inloggning och fjärrtoken kan autentiseringstillståndet ändras när programmet är i bakgrunden. Det går att anropa setRequestor igen när programmet försätts i förgrunden för att synkronisera med systemtillståndet (hämta en fjärrtoken om enkel inloggning är aktiverad eller ta bort den lokala token om en utloggning inträffar under tiden).

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till kanalens identitet. Serversvaret används internt av åtkomstaktiveringskoden. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för programmet via callback-funktionen setRequestorComplete().

Om parametern *urls* inte används anger det resulterande nätverksanropet standardtjänstleverantörens URL: Adobe Release Production Environment.

Om ett värde anges för parametern *urls*, anger det resulterande nätverksanropet alla URL:er som anges i parametern *urls* som mål. Alla konfigurationsbegäranden aktiveras samtidigt i olika trådar. Den första svararen har företräde när listan över MVPD kompileras. För varje MVPD i listan kommer Access Enabler ihåg URL:en till den associerade tjänsteleverantören. Alla efterföljande tillståndsbegäranden dirigeras till den URL som är associerad med tjänstleverantören som parats med mål-MVPD under konfigurationsfasen.

| API-anrop: konfiguration för begärare |
| --- |
| public void setRequestor(String requestedId) |

**Tillgänglighet:** v3.0+

| API-anrop: konfiguration för begärare |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Tillgänglighet:** v3.0+

**Parametrar:**

- *requestedID*: Det unika ID som är associerat med kanalen. Skicka det unika ID som tilldelats av Adobe till din webbplats när du första gången registrerar dig hos tjänsten Adobe Pass Authentication.
- *urls*: Valfri parameter. Som standard används Adobes tjänstleverantör (http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. Då består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder MVPD.

Föråldrat:

- *signedRequestorID*: En kopia av begärande-ID:t som har signerats digitalt med din privata nyckel. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Återanrop har utlösts:** `setRequestorComplete()`

</br>

### utloggning

**Beskrivning:** Använd den här metoden för att initiera utloggningsflödet. Utloggningen är resultatet av en serie HTTP-omdirigeringsåtgärder på grund av att användaren måste loggas ut både från Adobe Pass autentiseringsservrar och från MVPD servrar. Det innebär att det här flödet öppnar ett ChromeCustomTab-fönster för att köra utloggningen.

| API-anrop: initiera utloggningsflödet |
| --- |
| public void logOut() |

**Tillgänglighet:** v3.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus()`

## Programmeringsimplementeringsflöde {#Progr}

### **1. Registrera program**

1. Hämta programvaran\_statement från Adobe Pass ( TVE Dashboard )
1. Det finns två alternativ för att skicka dessa värden till Adobe Pass SDK:
   - Lägg till i strings.xml:

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - Anropa AccessEnabler.getInstance(appContext,softwareStatement, null)



### **2. Konfigurera programmet**

- a. setRequestor(request\_id)

  SDK utför följande åtgärder:

   - registreringsprogram: med **software\_statement** får SDK en **client\_id, client\_secrets, client\_id\_issued\_at, redirect\_uris, grant\_types**. Den här informationen lagras i programmets interna lagring.
   - hämta en **access\_token** med client\_id, client\_secrets och grant\_type=&quot;client\_credentials&quot;. Denna åtkomst\_token kommer att användas för alla samtal som SDK gör till Adobe Pass-servrar.

| Tokenfelsvar: |  |  |
|--- | --- | --- |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Begäran saknar en obligatorisk parameter, innehåller ett parametervärde som inte stöds (annat än anslagstyp), upprepar en parameter, innehåller flera autentiseringsuppgifter, använder mer än en mekanism för autentisering av klienten eller har på annat sätt fel format. |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Klientautentiseringen misslyckades eftersom klienten var okänd. SDK *MÅSTE* registreras hos auktoriseringsservern igen. |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | Den autentiserade klienten har inte behörighet att använda den här behörighetstypen. |

- om en MVPD kräver passiv autentisering öppnas en WebView som körs passivt med den MVPD och stängs när allt är klart

- b. checkAuthentication()

   - *true* : gå till auktorisering
   - *false* : gå till Select MVPD

- c. getAuthentication : SDK kommer att inkludera **access_token** i anropsparametrarna

   - mvpd kom ihåg : gå till setSelectedProvider(mvpd\_id)
   - mvpd är inte markerat: displayProviderDialog
   - mvpd selected : go to setSelectedProvider(mvpd\_id)

- d. setSelectedProvider

   - mvpd\_id-autentiserings-url har lästs in i ChromeCustomTabs
   - inloggningen lyckades : delegate.setAuthenticationStatus ( SUCCESS)
   - inloggningen avbröts: återställa MVPD-markering
   - URL-schemat har etablerats som &quot;adobepass://android.app&quot; för hämtning när autentiseringen är klar

- e. get/checkAuthorization: SDK inkluderar **access\_token **i header som Authorization: Bearer **access\_token**

- om auktoriseringen lyckas, kommer ett anrop att göras för att erhålla medietoken

- f. utloggning:

   - SDK kommer att ta bort giltig token för den aktuella begäraren (autentiseringar som erhållits av andra program och inte via enkel inloggning fortsätter att gälla)
   - SDK öppnar Chrome anpassade flikar för att nå mvpd\_id-utloggningsslutpunkten. När du är klar stängs Chrome anpassade flikar
   - URL-schemat är &quot;adobepass://logout&quot; för att fånga in tidpunkten då utloggningen är klar
   - utlösaren utlöser en sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) och ett återanrop : setAuthenticationStatus(0,&quot;Logout&quot;)



**Obs!** eftersom varje anrop kräver en **access_token** hanteras eventuella felkoder nedan i SDK.

| Felsvar |  |  |
|--- | --- | --- |
| invalid_request | 400 | Begäran har fel format. SDK bör sluta ringa anrop till servern. |
| invalid_client | 403 | Klient-ID:t har inte längre behörighet att utföra begäranden. SDK MÅSTE utföra klientregistreringen igen. |
| access_deny | 401 | access_token är inte giltig. SDK MÅSTE begära en ny access_token. |
