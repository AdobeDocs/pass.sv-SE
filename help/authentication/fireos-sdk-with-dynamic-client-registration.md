---
title: Amazon FireOS SDK med dynamisk klientregistrering
description: Amazon FireOS SDK med dynamisk klientregistrering
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# Amazon FireOS SDK med dynamisk klientregistrering {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## <span id=""></span>Introduktion {#Intro}

FireOS AccessEnabler SDK för FireTV har ändrats för att aktivera autentisering utan sessionscookies. I takt med att fler och fler webbläsare begränsar åtkomsten till cookies behövdes en annan metod för att tillåta autentisering.

**FireOS SDK 3.0.4** ersätter den aktuella appregistreringsmekanismen baserat på den signerade begärande-ID och sessionscookie-autentiseringen med [Dynamisk klientregistrering](/help/authentication/dynamic-client-registration.md).


## API-ändringar {#API}

### Factory.getInstance

**Beskrivning:** Instansierar Access Enabler-objektet. Det ska finnas en enda instans av Access Enabler per programinstans.

| API-anrop: konstruktor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        returnerar AccessEnablerException |

**Tillgänglighet:** v3.0+

**Parametrar:**

- *appContext*: Android-programkontext
- *softwareStatement*: värde från TVE Dashboard eller *null* om &quot;software\_statement&quot; är inställt i strings.xml
- *redirectUrl* : för FireTV-implementeringar ska den här parametern vara null. Alla inställningar för det här attributet ignoreras.

**Anteckningar**

- invalid softwareStatement kommer att göra så att programmet inte initierar AccessEnabler eller registrerar program för Adobe Pass Authentication och authentication
- redirectUrl-parametern för FireTV anges av SDK till adobepass://android.app eftersom autentiseringen hanteras av den unika AccessEnabler-instansen.

### setRequestor

**Beskrivning:** Fastställer kanalens identitet. Varje kanal tilldelas ett unikt ID när den registreras med Adobe för Adobe Pass autentiseringssystem. När det gäller enkel inloggning och fjärrtoken kan autentiseringstillståndet ändras när programmet är i bakgrunden. Det går att anropa setRequestor igen när programmet försätts i förgrunden för att synkronisera med systemtillståndet (hämta en fjärrtoken om enkel inloggning är aktiverad eller ta bort den lokala token om en utloggning inträffar under tiden).

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till kanalens identitet. Serversvaret används internt av åtkomstaktiveringskoden. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för programmet via callback-funktionen setRequestorComplete().

Om *urls* parametern används inte, det resulterande nätverksanropet har standardtjänstleverantörens URL som mål: produktionsmiljön för lansering av Adobe.

Om ett värde anges för *urls* parametern, aktiverar det resulterande nätverksanropet alla URL:er som anges i *urls* parameter. Alla konfigurationsbegäranden aktiveras samtidigt i olika trådar. Den första svararen har företräde när listan över MVPD kompileras. För varje MVPD i listan kommer åtkomstaktiveringen att komma ihåg URL:en för den associerade tjänstleverantören. Alla efterföljande tillståndsbegäranden dirigeras till den URL som är associerad med tjänstleverantören som parats med mål-MVPD under konfigurationsfasen.

| API-anrop: konfiguration för begärare |
| --- |
| public void setRequestor(String requestedId) |

**Tillgänglighet:** v3.0+

| API-anrop: konfiguration för begärare |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Tillgänglighet:** v3.0+

**Parametrar:**

- *requestID*: Det unika ID som är associerat med kanalen. Skicka det unika ID som tilldelats av Adobe till din webbplats när du först registrerar dig hos Adobe Pass autentiseringstjänst.
- *urls*: Valfri parameter. Som standard används Adobes tjänsteleverantör (http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. När detta görs består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder det MVPD.

Föråldrat:

- *signedRequestorID*: En kopia av begärande-ID som signeras digitalt med din privata nyckel. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Återanrop utlösta:** `setRequestorComplete()`

</br>

### utloggning

**Beskrivning:** Använd den här metoden för att initiera utloggningsflödet. Utloggningen är resultatet av en serie HTTP-omdirigeringsåtgärder på grund av att användaren måste loggas ut både från Adobe Pass autentiseringsservrar och från MVPD-servrarna. Det innebär att det här flödet öppnar ett ChromeCustomTab-fönster för att köra utloggningen.

| API-anrop: initiera utloggningsflödet |
| --- |
| public void logOut() |

**Tillgänglighet:** v3.0+

**Parametrar:** Ingen

**Återanrop utlösta:** `setAuthenticationStatus()`

## Programmeringsimplementeringsflöde {#Progr}

### **1. Registrera program**

1. Hämta programvaran\_statement från Adobe Pass ( TVE Dashboard )
1. Det finns två alternativ för att skicka dessa värden till Adobe Pass SDK:
   - Lägg till i strings.xml:

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - Anropa AccessEnabler.getInstance(appContext,softwareStatement, null)



### **2. Konfigurera program**

- a. setRequestor(request\_id)

  SDK kommer att utföra följande åtgärder:

   - registrera program: använda **software\_statement** får SDK en **client\_id, client\_secrets, client\_id\_issued\_at, redirect\_uris, grant\_types**. Den här informationen lagras i programmets interna lagring.
   - få en **access\_token** med client\_id, client\_secrets och grant\_type=&quot;client\_credentials&quot;. Denna åtkomst\_token kommer att användas för varje anrop från SDK till Adobe Pass-servrar.

| Tokenfelsvar: |  |  |
|--- | --- | --- |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;invalid\_request&quot;} | Begäran saknar en obligatorisk parameter, innehåller ett parametervärde som inte stöds (annat än anslagstyp), upprepar en parameter, innehåller flera autentiseringsuppgifter, använder mer än en mekanism för autentisering av klienten eller har på annat sätt fel format. |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Klientautentiseringen misslyckades eftersom klienten var okänd. SDK *MÅSTE* registrera med auktoriseringsservern igen. |
| HTTP 400 (Ogiltig begäran) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | Den autentiserade klienten har inte behörighet att använda den här behörighetstypen. |

- om ett MVPD kräver passiv autentisering öppnas en WebView för att köras passivt med detta MVPD och stängs när det är klart

- b. checkAuthentication()

   - *true* : gå till Authorization
   - *false* : gå till Select MVPD

- c. getAuthentication : SDK kommer att innehålla **access_token** i anropsparametrar

   - mvpd kom ihåg : gå till setSelectedProvider(mvpd\_id)
   - mvpd är inte markerat: displayProviderDialog
   - mvpd selected : go to setSelectedProvider(mvpd\_id)

- d. setSelectedProvider

   - mvpd\_id-autentiserings-url har lästs in i ChromeCustomTabs
   - inloggningen lyckades : delegate.setAuthenticationStatus ( SUCCESS)
   - inloggningen avbröts: återställ MVPD-val
   - URL-schemat har etablerats som &quot;adobepass://android.app&quot; för hämtning när autentiseringen är klar

- e. get/checkAuthorization: SDK kommer att innehålla **access\_token **i huvudet som Authorization: Bearer **access\_token**

- om auktoriseringen lyckas, kommer ett anrop att göras för att erhålla medietoken

- f. utloggning:

   - SDK kommer att ta bort giltig token för den aktuella begäraren (autentiseringar som erhållits av andra program och inte via enkel inloggning kommer att förbli giltiga)
   - SDK öppnar anpassade Chrome-flikar för att nå mvpd\_id-utloggningsslutpunkten. När du är klar stängs de anpassade Chrome-flikarna
   - URL-schemat är &quot;adobepass://logout&quot; för att fånga in tidpunkten då utloggningen är klar
   - utlösaren utlöser en sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) och ett återanrop : setAuthenticationStatus(0,&quot;Logout&quot;)



**Obs!** eftersom varje samtal kräver **access_token** kan eventuella felkoder nedan hanteras i SDK:n.

| Felsvar |  |  |
|--- | --- | --- |
| invalid_request | 400 | Begäran har fel format. SDK:n bör sluta utföra anrop till servern. |
| invalid_client | 403 | Klient-ID:t har inte längre behörighet att utföra begäranden. SDK MÅSTE utföra klientregistreringen igen. |
| access_deny | 401 | access_token är inte giltig. SDK MÅSTE begära en ny access_token. |
