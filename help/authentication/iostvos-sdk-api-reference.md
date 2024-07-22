---
title: API-referens för iOS/tvOS
description: API-referens för iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '6933'
ht-degree: 0%

---

# API-referens för iOS/tvOS SDK {#iostvos-sdk-api-reference}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

Den här sidan beskriver de metoder och callback-funktioner som finns i iOS/tvOS-klientens för Adobe Pass-autentisering. Metoderna och callback-funktionerna som beskrivs här definieras i rubrikfilerna `AccessEnabler.h` och `EntitlementDelegate.h`. Du hittar dem i iOS AccessEnabler SDK här: `[SDK directory]/AccessEnabler/headers/api/`


Associerad dokumentation:

* En beskrivning av det grundläggande berättigandeflödet för Adobe Pass-autentisering finns i [Tillståndsflöde](/help/authentication/entitlement-flow.md).
* En stegvis genomgång av hur du implementerar Adobe Pass
autentiseringsberättigandeflödet med detta API finns i [iOS Integration Cookbook](/help/authentication/iostvos-sdk-cookbook.md).
* Den senaste versionen av iOS AccessEnabler SDK finns i [iOS Native Access Enabler Library](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe rekommenderar att du endast använder API:erna för Adobe Pass-autentisering *public*:
>
>* Offentliga API:er är tillgängliga och fullt testade för alla klienttyper som stöds. För alla offentliga funktioner ser vi till att varje klienttyp har en motsvarande version av de associerade metoderna.
>* Offentliga API:er måste vara så stabila som möjligt för att ge stöd för bakåtkompatibilitet och se till att partnerintegreringar inte bryts. Men för icke-offentliga API:er förbehåller vi oss rätten att ändra signaturen när som helst. Om du stöter på ett visst flöde som inte stöds genom en kombination av de aktuella API-anropen för Adobe Pass-autentisering är det bästa sättet att tala om det för oss. Med tanke på dina behov kan vi ändra de offentliga API:erna och tillhandahålla en stabil lösning som går framåt.

</br>

## API-referens {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - instansierar AccessEnabler-objektet.

* **[DEPRECATED]** [init](#init) - Instansierar AccessEnabler-objektet.

* [`setOptions:options:`](#setOptions) - Konfigurerar globala SDK-alternativ som profile eller visitorID.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Anger programmerarens identitet.

* **[DEPRECATED]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Fastställer programmerarens identitet.

* **[DEPRECATED]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Fastställer programmerarens identitet.

* [`setRequestorComplete:`](#setReqComplete) - Informerar ditt program om att konfigurationsfasen är slutförd.

* [`checkAuthentication`](#checkAuthN) - Kontrollerar den aktuella användarens autentiseringsstatus.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Startar det fullständiga autentiseringsarbetsflödet.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) - Startar det fullständiga autentiseringsarbetsflödet.

* [`displayProviderDialog:`](#dispProvDialog) - Informerar ditt program om att instansiera lämpliga gränssnittselement så att användaren kan välja ett MVPD.

* [`setSelectedProvider:`](#setSelProv) - Informerar AccessEnabler om användarens MVPD-val.

* [`navigateToUrl:`](#nav2url) - Informerar ditt program om att användaren måste få tillgång till inloggningssidan för MVPD.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Informerar ditt program om att användaren behöver få tillgång till inloggningssidan för MVPD via SFSafariViewController

* [`handleExternalURL:url`](#handleExternalURL) - Slutför autentiserings-/utloggningsflödet.

* **[DEPRECATED]** [`getAuthenticationToken`](#getAuthNToken) - Begär en autentiseringstoken från backend-servern.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informerar ditt program om autentiseringsflödets status.

* [`checkPreauthorizedResources:`](#checkPreauth) - Anger om användaren redan har behörighet att visa specifika skyddade resurser.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Anger om användaren redan har behörighet att visa specifika skyddade resurser.

* [`preauthorizedResources:`](#preauthResources) - Tillhandahåller en lista över resurser som användaren redan har behörighet att visa.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Kontrollerar den aktuella användarens auktoriseringsstatus.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Initierar auktoriseringsflödet.

* [`setToken:forResource:`](#setToken) - Informerar programmet om att auktoriseringsflödet har slutförts.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Informerar programmet om att auktoriseringsflödet misslyckades.

* [`logout`](#logout) - Initierar utloggningsflödet.

* [`getSelectedProvider`](#getSelProv) - Bestämmer den markerade providern.

* [`selectedProvider:`](#selProv) - Levererar information om det MVPD som är valt till ditt program.

* [`getMetadata:`](#getMeta) - Hämtar information som exponeras som metadata av AccessEnabler-biblioteket.

* [`presentTvProviderDialog:`](#presentTvDialog) - Informerar ditt program om att visa Apple SSO-dialogrutan.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Informerar ditt program om att dölja Apple SSO-dialogrutan.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Levererar metadata som begärts av ett [`getMetadata:`](#getMeta)-anrop.

* [`sendTrackingData:forEventType:`](#sendTracking) - Levererar information om spårningsdata.

* [`MVPD`](#mvpd) - Klassen MVPD. [Innehåller information om MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Instansierar AccessEnabler-objektet. Det ska finnas en enda AccessEnabler-instans per programinstans.

| **API-anrop: iOS AccessEnabler-konstruktor** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Tillgänglighet:** v3.0+

**Parametrar:**

* **softwareStatement:** En sträng som identifierar programmet i Adobe. Ta reda på hur ni får en programvaruöversikt.

[Till början...](#apis)



### init - [DEPRECATED]{#init}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Instansierar AccessEnabler-objektet. Det ska finnas en enda AccessEnabler-instans per programinstans.

| API-anrop: iOS AccessEnabler-konstruktor |
| --- |
| ```- (id) init;``` |

**Tillgänglighet:** v1.0+ **Till:** v3.0

**Parametrar:** Inga

[Till början...](#apis)

</br>

### setOptions:options {#setOptions}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Konfigurerar globala SDK-alternativ. Det accepterar en NSDictionary som ett argument. Värdena från ordlistan skickas till servern tillsammans med alla nätverksanrop som SDK gör.

**Obs!** Värdena skickas till servern oberoende av det aktuella flödet (autentisering/auktorisering). Om du vill ändra värdena kan du anropa den här metoden när som helst.

| API-anrop: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Tillgänglighet:** v2.3.0+

**Parametrar:**

* *options*: En NSDictionary som innehåller globala SDK-alternativ. Följande alternativ är tillgängliga:
   * **applicationProfile** - Den kan användas för att skapa serverkonfigurationer baserat på det här värdet.
   * **visitorID** - Experience Cloud ID-tjänsten. Det här värdet kan användas senare för avancerade analysrapporter.
   * **handleSVC** - Boolean som anger om programmeraren ska hantera SFSafariViewControllers. Mer information finns i [Stöd för SFSafariViewController i iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md).
      * Om värdet är **false** kommer SDK automatiskt att ge slutanvändaren en SFSafariViewController. SDK kommer att gå vidare till inloggningssidans URL för MVPD.
      * Om värdet är **true** kommer SDK **NOT** automatiskt att presentera en SFSafariViewController för slutanvändaren. SDK utlöser ytterligare **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Klientinformation enligt beskrivningen i [Skicka klientinformation](/help/authentication/passing-client-information-device-connection-and-application.md).

[Till början...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Anger programmerarens identitet. Varje programmerare tilldelas ett unikt ID när den registreras hos Adobe för Adobe Pass autentiseringssystem. När det gäller enkel inloggning och fjärrtoken kan autentiseringstillståndet ändras när programmet är i bakgrunden, setRequestor kan anropas igen när programmet placeras i förgrunden för att synkronisera med systemtillståndet (hämta en fjärrtoken om enkel inloggning är aktiverad eller ta bort den lokala token om en utloggning sker under tiden).

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till programmerarens identitet. Serversvaret används internt av koden AccessEnabler. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för ditt program via callback-funktionen `setRequestorComplete:`.

Om parametern `urls` inte används anger det resulterande nätverksanropet standardtjänstleverantörens URL: Adobe RELEASE/produktionsmiljö.


Om ett värde anges för parametern `urls`, anger det resulterande nätverksanropet alla URL:er som anges i parametern `urls` som mål. Alla konfigurationsbegäranden aktiveras samtidigt i olika trådar. Den första svararen har företräde när listan över MVPD kompileras. För varje MVPD i listan kommer AccessEnabler ihåg URL:en för den associerade tjänstleverantören. Alla efterföljande tillståndsbegäranden dirigeras till den URL som är associerad med tjänstleverantören som parats med mål-MVPD under konfigurationsfasen.

| API-anrop: konfiguration för begärare |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Tillgänglighet:** v3.0+

| API-anrop: konfiguration för begärare |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Tillgänglighet:** v3.0+

**Parametrar:**

* *requestedID*: Det unika ID som är associerat med programmeraren. Skicka det unika ID som tilldelats av Adobe till din webbplats när du först registrerar dig hos Adobe Pass autentiseringstjänst.
* *urls*: Valfri parameter. Som standard används Adobes tjänstleverantör (http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. När detta görs består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder det MVPD.

>[!NOTE]
>
>Om det anropas utan parametern `serviceProviders` hämtar biblioteket konfigurationen från standardtjänstleverantören (det vill säga `https://sp.auth.adobe.com` för produktionsprofilen eller `https://sp.auth-staging.adobe.com` för mellanlagringsprofilen). Om parametern `serviceProviders` anges måste den vara en matris med URL:er. Konfigurationsinformationen hämtas från alla angivna slutpunkter och sammanfogas. Om det finns duplicerad information i olika tjänstleverantörssvar löses konflikten till förmån för den snabbaste svarsservern (det vill säga servern med den kortaste svarstiden har företräde).

**Återanrop har utlösts:** [`setRequestorComplete:`](#setReqComplete)

[Till början...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [DEPRECATED] {#setReq}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Anger programmerarens identitet. Varje programmerare tilldelas ett unikt ID när den registreras hos Adobe för Adobe Pass autentiseringssystem. När det gäller enkel inloggning och fjärrtoken kan autentiseringstillståndet ändras när programmet är i bakgrunden. Det går att anropa setRequestor igen när programmet försätts i förgrunden för att synkronisera med systemtillståndet (hämta en fjärrtoken om enkel inloggning är aktiverad eller ta bort den lokala token om en utloggning inträffar under tiden).

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till programmerarens identitet. Serversvaret används internt av koden AccessEnabler. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för ditt program via callback-funktionen `setRequestorComplete:`.

Om parametern `urls` inte används anger det resulterande nätverksanropet standardtjänstleverantörens URL: Adobe RELEASE/produktionsmiljö.

Om ett värde anges för parametern `urls`, anger det resulterande nätverksanropet alla URL:er som anges i parametern `urls` som mål. Alla konfigurationsbegäranden aktiveras samtidigt i olika trådar. Den första svararen har företräde när listan över MVPD kompileras. För varje MVPD i listan kommer AccessEnabler ihåg URL:en för den associerade tjänstleverantören. Alla efterföljande tillståndsbegäranden dirigeras till den URL som är associerad med tjänstleverantören som parats med mål-MVPD under konfigurationsfasen.

| API-anrop: konfiguration för begärare |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Tillgänglighet:** v1.0+ **Till:** v3.0

| API-anrop: konfiguration för begärare |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Tillgänglighet:** v1.0+ **Till:** v3.0

**Parametrar:**

* *requestedID*: Det unika ID som är associerat med programmeraren. Skicka det unika ID som tilldelats av Adobe till din webbplats när du först registrerade dig hos Adobe Pass autentiseringstjänst.
* *signedRequestorID*: **Den här parametern finns i iOS AccessEnabler version 1.2 och senare.** En kopia av begärande-ID som signeras digitalt med din privata nyckel. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Valfri parameter. Som standard används Adobes tjänstleverantör (http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. När detta görs består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder det MVPD.

**Obs!** Om det anropas utan parametern `serviceProviders` hämtar biblioteket konfigurationen från standardtjänstleverantören (det vill säga `https://sp.auth.adobe.com` för produktionsprofilen eller `https://sp.auth-staging.adobe.com` för mellanlagringsprofilen). Om parametern `serviceProviders` anges måste den vara en matris med URL:er. Konfigurationsinformationen hämtas från alla angivna slutpunkter och sammanfogas. Om det finns duplicerad information i olika tjänstleverantörssvar löses konflikten till förmån för den snabbaste svarsservern (d.v.s. servern med den kortaste svarstiden har företräde).

**Återanrop har utlösts:** [`setRequestorComplete:`](#setReqComplete)


[Till början...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [DEPRECATED] {#setReq_tvos}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Anger programmerarens identitet. Varje programmerare tilldelas ett unikt ID när den registreras hos Adobe för Adobe Pass autentiseringssystem. Den här inställningen ska endast utföras en gång under programmets livscykel.

Serversvaret innehåller en lista över MVPD:er tillsammans med viss konfigurationsinformation som är kopplad till programmerarens identitet. Serversvaret används internt av koden AccessEnabler. Endast åtgärdens status (d.v.s. SUCCESS/FAIL) visas för ditt program via callback-funktionen `setRequestorComplete:`.

Om parametern `urls` inte används anger det resulterande nätverksanropet standardtjänstleverantörens URL: Adobe RELEASE/produktionsmiljö.

Om ett värde anges för parametern `urls`, anger det resulterande nätverksanropet alla URL:er som anges i parametern `urls` som mål. Alla konfigurationsbegäranden aktiveras samtidigt i olika trådar. Den första svararen har företräde när listan över MVPD kompileras. För varje MVPD i listan kommer AccessEnabler ihåg URL:en för den associerade tjänstleverantören. Alla efterföljande tillståndsbegäranden dirigeras till den URL som är associerad med tjänstleverantören som parats med mål-MVPD under konfigurationsfasen.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: konfiguration för begärare</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**Tillgänglighet:** v2.0+ **Till:** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: konfiguration för begärare</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v2.0+ **Till:** v3.0

**Parametrar:**

* *requestedID*: Det unika ID som är associerat med programmeraren. Skicka det unika ID som tilldelats av Adobe till din webbplats första gången   har registrerats med tjänsten Adobe Pass Authentication.
* *signedRequestorID*: **Den här parametern finns i iOS AccessEnabler   version 1.2 och senare.** En kopia av begärande-ID som signeras digitalt med din privata nyckel. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Valfri parameter; som standard är Adobe-tjänstleverantören   används (http://sp.auth.adobe.com/). Med den här arrayen kan du ange slutpunkter för autentisering och auktoriseringstjänster som tillhandahålls av Adobe (olika instanser kan användas i felsökningssyfte). Du kan använda detta för att ange flera instanser av Adobe Pass Authentication-tjänstprovidern. När detta görs består MVPD-listan av slutpunkterna från alla tjänsteleverantörer. Varje MVPD är kopplat till den snabbaste tjänsteleverantören, dvs. den leverantör som svarade först och som stöder det MVPD.
* secrets och publicKey: Den hemliga och offentliga nyckel som används för att signera andra skärmanrop. Mer information finns i [dokumentationen om klientlösa](#create_dev).

Om det anropas utan parametern `serviceProviders` hämtar biblioteket konfigurationen från standardtjänstleverantören (d.v.s. `https://sp.auth.adobe.com` för produktionsprofilen eller https://sp.auth-staging.adobe.com för mellanlagringsprofilen). Om parametern `serviceProviders` anges måste den vara en matris med URL:er. Konfigurationsinformationen hämtas från alla angivna slutpunkter och sammanfogas. Om det finns duplicerad information i olika tjänstleverantörssvar löses konflikten till förmån för den snabbaste svarsservern (d.v.s. servern med den kortaste svarstiden har företräde).

**Återanrop har utlösts:** [`setRequestorComplete:`](#setReqComplete)

[Till början...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop som utlöses av AccessEnabler som informerar programmet om att konfigurationsfasen är slutförd. Detta är en signal om att programmet kan börja utfärda tillståndsbegäranden. Inga berättigandebegäranden kan utfärdas av programmet förrän konfigurationsfasen är slutförd.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: konfigurationen för begärande har slutförts</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Tillgänglighet:** v1.0+

**Parametrar**:

* *status*: kan ha något av följande värden:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - konfigurationsfasen har slutförts
   * `ACCESS_ENABLER_STATUS_ERROR` - konfigurationsfasen misslyckades

**Utlöses av:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Till början...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Kontrollerar den aktuella användarens autentiseringsstatus.
Det gör du genom att söka efter en giltig autentiseringstoken på den lokala
tokenlagringsutrymme. Den här metoden utför inga nätverksanrop och vi rekommenderar att du anropar den på huvudtråden.
Den används av programmet för att fråga användarens autentiseringsstatus och
uppdatera användargränssnittet (d.v.s. uppdatera användargränssnittet för inloggning/utloggning). The
autentiseringsstatus meddelas till programmet via
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)-återanropet.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: kontrollera autentiseringsstatus</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Till början...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Startar det fullständiga autentiseringsarbetsflödet. Det börjar med att kontrollera autentiseringsstatusen. Om autentiseringen inte redan har autentiserats startas tillståndsdatorn för autentiseringsflödet:

* om det senaste autentiseringsförsöket lyckades,   markeringsfasen hoppas över och   [`navigateToUrl:`](#nav2url)-återanropet aktiveras. The   programmet använder det här återanropet för att instansiera WebView-kontrollen som visar användaren med MVPD:s inloggningssida. **[Obs! Från och med Access Enabler 1.5 är den här funktionen inte tillgänglig på grund av en begränsning i SDK].**
* om det senaste autentiseringsförsöket misslyckades eller om användaren uttryckligen loggade ut, är återanropet [`displayProviderDialog:`](#dispProvDialog)   utlöses. Programmet använder det här återanropet för att visa användargränssnittet för MVPD-val. Ditt program måste också återuppta autentiseringsflödet genom att informera AccessEnabler-biblioteket om användarens MVPD-val via metoden [`setSelectedProvider:`](#setSelProv).

När inloggningsuppgifterna för användaren verifieras på MVPD-inloggningssidan måste programmet övervaka de omdirigeringsåtgärder som utförs medan användaren autentiserar sig på MVPD-inloggningssidan. När rätt autentiseringsuppgifter anges omdirigeras WebView-kontrollen till en anpassad URL som definieras av konstanten `ADOBEPASS_REDIRECT_URL`. Denna URL är inte avsedd att läsas in av WebView. Programmet måste tolka denna URL och tolka händelsen som en signal om att inloggningsfasen är slutförd. Den bör sedan lämna över kontrollen till AccessEnabler för att slutföra autentiseringsflödet (genom att anropa metoden [handleExternalURL](#handleExternalURL) ).

Slutligen kommuniceras autentiseringsstatusen till programmet via återanropet [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initierar autentiseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initierar autentiseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Tillgänglighet:** v1.9+

**Parametrar:**

* *forceAuthn*: En flagga som anger om autentiseringsflödet ska startas, oavsett om användaren redan är autentiserad eller inte.
* *data*: En ordlista som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Till början...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Startar det fullständiga autentiseringsarbetsflödet. Det börjar med att kontrollera autentiseringsstatusen. Om autentiseringen inte redan har autentiserats startas tillståndsdatorn för autentiseringsflödet:

* [presentTvProviderDialog()](#presentTvDialog) anropas om den aktuella begäraren har minst ett MVPD som stöder enkel inloggning. Om inget MVPD har stöd för enkel inloggning, kommer det klassiska autentiseringsflödet att börja och filterparametern ignoreras.
* När användaren har slutfört Apple SSO-flödet aktiveras [`dismissTvProviderDialog()`](#dismissTvDialog) och autentiseringsprocessen slutförs.

Slutligen kommuniceras autentiseringsstatusen till programmet via återanropet [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

**Tillgänglighet:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initierar autentiseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initierar autentiseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>
 </div></td>
</tr>
</tbody>
</table>



**Parametrar:**

* *forceAuthn*: En flagga som anger om autentiseringsflödet ska startas, oavsett om användaren redan är autentiserad eller inte.
* *data*: En ordlista som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.
* filter: En ordlista med två listor med MVPD-ID:n som ska visas i Apple SSO-dialogrutan. Alla MVPD som inte stöder enkel inloggning ignoreras, men ordningen respekteras. Ordlistan måste ha två nycklar:
   * TV\_PROVIDERS: En lista med alla MVPD-filer som ska visas i väljaren
   * FEATURED\_TV\_PROVIDERS: En lista med alla MVPD-filer som ska markeras som aktuella i väljaren. MVPD-värden i den här listan måste också anges i listan TV\_PROVIDERS.

**Tillgänglighet:** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initierar autentiseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initierar autentiseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parametrar:**

* *forceAuthn*: En flagga som anger om autentiseringsflödet ska startas, oavsett om användaren redan är autentiserad eller inte.
* *data*: En ordlista som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.
* filter: En lista med MVPD-ID:n som ska visas i Apple SSO-dialogrutan. Alla MVPD som inte stöder enkel inloggning ignoreras, men ordningen respekteras.

**Återanrop har utlösts:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Till början...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop som utlöses av AccessEnabler för att informera programmet om att lämpliga gränssnittselement måste instansieras så att användaren kan välja önskat MVPD. I återanropet finns en lista med MVPD-objekt med ytterligare information som kan hjälpa dig att skapa den valda gränssnittspanelen korrekt (t.ex. URL:en som pekar på MVPD:s logotyp, visningsnamn osv.)

När användaren har valt önskat MVPD måste programmet i det övre lagret återuppta autentiseringsflödet genom att anropa `setSelectedProvider:` och skicka det ID för MVPD som motsvarar användarens val.

**Avbryter autentiseringsflödet** - Detta är en punkt där användaren kan trycka på knappen &quot;Bakåt&quot;, vilket motsvarar att avbryta autentiseringsflödet. I så fall måste ditt program anropa metoden [setSelectedProvider:](#setSelProv) och skicka null som parameter för att ge AccessEnabler möjlighet att återställa sin autentiseringstillståndsdator.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: visa användargränssnittet för MVPD-markering</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Tillgänglighet:** v1.0+

**Parametrar**:

* *mvpds*: lista över MVPD-objekt som innehåller MVPD-relaterad information som programmet kan använda för att skapa gränssnittselement för MVPD-val.

**Utlöses av:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Till början...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Den här metoden anropas av ditt program för att informera åtkomstaktiveraren om användarens MVPD-val. Programmet kan använda den här metoden för att välja eller ändra tjänsteleverantören som används för autentisering.

Om det valda MVPD-programmet är ett TempPass MVPD autentiseras det automatiskt med det MVPD-programmet utan att du behöver anropa getAuthentication() efteråt.

Observera att detta inte är möjligt för kampanjtillfälligt pass där extra parametrar anges för getAuthentication()-metoden.

När *null* skickas som en parameter antar Access Enabler att användaren har avbrutit autentiseringsflödet (d.v.s. tryckt på knappen &quot;Bakåt&quot;) och svarar genom att återställa autentiseringstillståndsdatorn och genom att anropa [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)-återanropet med felkoden `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR`.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: ange den valda providern</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Till början...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning:** Återanrop som utlöses av AccessEnabler för att begära att programmet instansierar en UIWebView/WKWebView-kontrollant och att läsa in URL:en som anges i callback-objektets **`url`** -parameter. Återanropet skickar parametern **`url`** som representerar URL:en för autentiseringsslutpunkten eller URL:en för utloggningsslutpunkten.

När kontrollenheten UIWebView/WKWebView` `går igenom flera omdirigeringar måste programmet övervaka kontrollantens aktivitet och upptäcka när den läser in en specifik anpassad URL som definieras av konstanten `ADOBEPASS_REDIRECT_URL ` (dvs. `adobepass://ios.app`). Observera att den här anpassade URL:en är ogiltig och inte avsedd för att styrenheten ska läsa in den. Det får endast tolkas av programmet som en signal om att autentiserings- eller utloggningsflödet har slutförts och att det är säkert att stänga kontrollenheten. När kontrollenheten läser in den här anpassade URL:en måste ditt program stänga UIWebView/WKWebView och anropa AccessEnablers `handleExternalURL:url `API-metod.

**Obs!** Observera att vid autentiseringsflödet är detta en punkt där användaren kan trycka på knappen &quot;Bakåt&quot;, vilket motsvarar att autentiseringsflödet avbryts. I ett sådant fall måste ditt program anropa metoden [setSelectedProvider:](#setSelProv) som skickar **`nil`** som parameter och ge AccessEnabler en chans att återställa sin autentiseringstillståndsdator.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: visa inloggningssida för MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar**:

* *url*: URL:en som pekar på MVPD:s inloggningssida

**Utlöses av:** [setSelectedProvider:](#setSelProv)



[Till början...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning:** Återanrop utlöses av AccessEnabler i stället för `navigateToUrl:` återanrop om ditt program tidigare aktiverade manuell Safari View Controller (SVC)-hantering via anropet [ setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) och endast för MVPD-program som kräver Safari View Controller (SVC). För alla andra MVPD-filer anropas motringningen `navigateToUrl:`. Mer information om hur Safari View Controller (SVC) ska hanteras finns i [Stöd för SFSafariViewController (SVC) i iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md).

Ungefär som för `navigateToUrl:`-återanropet aktiveras `navigateToUrl:useSVC:` av AccessEnabler för att begära att programmet instansierar en `SFSafariViewController`-kontrollant och för att läsa in URL:en som anges i återanropets **`url`**-parameter. Återanropet skickar parametern **`url`** som representerar URL:en för autentiseringsslutpunkten eller URL:en för utloggningsslutpunkten, och parametern **`useSVC`** som anger att programmet måste använda `SFSafariViewController`.

När kontrollenheten `SFSafariViewController` går igenom flera omdirigeringar måste programmet övervaka kontrollenhetsaktiviteten och identifiera tidpunkten när den läser in en specifik anpassad URL som definieras av din `application's custom scheme` (t.ex.** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Observera att den här anpassade URL:en är ogiltig och inte avsedd för att styrenheten ska läsa in den. Det får endast tolkas av programmet som en signal om att autentiserings- eller utloggningsflödet har slutförts och att det är säkert att stänga kontrollenheten. När kontrollenheten läser in den här anpassade URL:en måste ditt program stänga `SFSafariViewController` och anropa AccessEnablers `handleExternalURL:url `API-metod.

**Obs!** Observera att vid autentiseringsflödet är detta en punkt där användaren kan trycka på knappen &quot;Bakåt&quot;, vilket motsvarar att autentiseringsflödet avbryts. I ett sådant fall måste ditt program anropa metoden [setSelectedProvider:](#setSelProv) som skickar **`nil`** som parameter och ge AccessEnabler en chans att återställa sin autentiseringstillståndsdator.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: visa inloggningssidan för MVPD i SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:**v 3.2+

**Parametrar**:

* *url:* den URL som pekar på MVPD:s inloggningssida
* *useSVC:* Anger om URL:en ska läsas in i SFSafariViewController.

**Utlöses av:**[ setOptions:](#setOptions) före [setSelectedProvider:](#setSelProv)

[Till början...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Den här metoden anropas av ditt program för att slutföra autentiserings- eller utloggningsflödet. Den här metoden ska anropas direkt efter att programmet identifierar den tidpunkt då `UIWebView/WKWebView or SFSafariViewController`-kontrollenheten omdirigeras till en specifik anpassad URL. Om ditt program måste använda en `SFSafariViewController `kontrollant definieras den anpassade URL:en av din `application's custom scheme` (t.ex. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), annars definieras den här anpassade URL:en av konstanten `ADOBEPASS_REDIRECT_URL ` (t.ex. `adobepass://ios.app`).

Om autentiseringsflödet är aktiverat slutför AccessEnabler flödet genom att hämta autentiseringstoken från backend-servern och lagra den lokalt i tokenlagringen. AccessEnabler informerar programmet om att autentiseringsflödet är slutfört genom att anropa `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)-->-återanropet med statuskoden 1, vilket anger att det lyckades. Om det uppstår ett fel under körningen av de här stegen aktiveras `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)-->-återanropet med statuskoden 0, vilket anger autentiseringsfel och en motsvarande felkod.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: slutföra autentiserings- eller utloggningsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v3.0+

**Parametrar:**

* *url*: Den spärrade URL:en från ` UIWebView/WKWebView or SFSafariViewController `-kontrollen är en sträng.


**Återanrop har utlösts:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Till början...](#apis)

</br>

#### getAuthenticationToken - [DEPRECATED] {#getAuthNToken}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Slutför autentiseringsflödet genom att begära en autentiseringstoken från backend-servern. Den här metoden ska bara anropas av programmet som svar på en händelse där WebView-kontrollen som är värd för MVPD-inloggningssidan omdirigeras till den anpassade URL som definieras av konstanten `ADOBEPASS_REDIRECT_URL`.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: hämta autentiseringstoken</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+ **Till:** v3.0

**Parametrar:** Inga

**Återanrop har utlösts:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Till början...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop utlöses av AccessEnabler som informerar programmet om autentiseringsflödets status. Det finns många ställen där det här flödet kan misslyckas, antingen som ett resultat av användarinteraktion eller på grund av andra oförutsedda scenarier (t.ex. nätverksanslutningsproblem). Det här återanropet informerar programmet om autentiseringsflödets status för lyckad/misslyckad, och ger även ytterligare information om felorsaken när det behövs.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: rapportera autentiseringsflödets status</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**Tillgänglighet:** v1.0+

**Parametrar**:

* *status*: kan ha något av följande värden:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - autentiseringsflödet har slutförts
   * `ACCESS_ENABLER_STATUS_ERROR` - autentiseringsflödet misslyckades
* *kod*: felorsak. Om *status* är `ACCESS_ENABLER_STATUS_SUCCESS` är *code* en tom sträng (d.v.s. definierad av konstanten `USER_AUTHENTICATED`). Vid fel kan den här parametern ha något av följande värden:
   * `USER_NOT_AUTHENTICATED_ERROR` - Användaren är inte autentiserad. Som svar på metodanropet [checkAuthentication:](#checkAuthN) när det inte finns någon giltig autentiseringstoken i den lokala tokencachen.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler har återställt       autentiseringstillståndsdator efter programmet i det övre skiktet       skickade *null* till [`setSelectedProvider:`](#setSelProv) för att avbryta autentiseringsflödet.  Förmodligen har användaren avbrutit autentiseringsflödet (d.v.s. tryckt på knappen &quot;Bakåt&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - Autentiseringsflödet misslyckades på grund av exempelvis att nätverket inte är tillgängligt eller att användaren uttryckligen avbröt autentiseringsflödet.

**Utlöses av:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Till början...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Den här metoden används av programmet för att avgöra om användaren redan har behörighet att visa specifika skyddade resurser. Det främsta syftet med den här metoden är att hämta information som ska användas för att dekorera gränssnittet **(till exempel att ange åtkomststatus med lås- och upplåsningsikoner).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: ange den valda providern</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.3+

**Parametrar:**

* *resurser:*-matris med resurser som auktoriseringen ska kontrolleras för. Varje element i listan ska vara en sträng som representerar resurs-ID:t. The     Resurs-ID har samma begränsningar som resurs-ID:t i anropet, det vill säga det ska fastställas ett avtalat värde mellan Programmer och MVPD eller ett mediets RSS-fragment.

**Återanropet har utlösts:** [`preauthorizedResources:`](#preauthResources)

[Till början...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Den här metoden används av programmet för att avgöra om användaren redan har behörighet att visa specifika skyddade resurser. Det främsta syftet med den här metoden är att hämta information som ska användas för att dekorera användargränssnittet (till exempel indikera åtkomststatus med lås- och upplåsningsikoner). Parametern **cache** kontrollerar om det interna cacheminnet används för att matcha resurser.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: ange den valda providern</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**Tillgänglighet:** v3.1+



**Parametrar:**

* *resurser:*-matris med resurser som auktoriseringen ska kontrolleras för. Varje element i listan ska vara en sträng som representerar resurs-ID:t. Resurs-ID har samma begränsningar som resurs-ID:t i `getAuthorization:`-anropet, d.v.s. det ska vara ett avtalat värde mellan Programmer och MVPD eller ett mediets RSS-fragment.
* *cache:* Boolean anger om den interna cachen ska användas för att matcha resurser. Om värdet är false kommer cachen att ignoreras, vilket resulterar i serveranrop varje gång detta API anropas.

**Återanropet har utlösts:** [`preauthorizedResources:`](#preauthResources)

[Till början...](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning:** Återanrop utlöstes av `checkPreauthorizedResources:`. Visar en lista över resurser som användaren redan har behörighet att visa.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: ange den valda providern</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.3+

**Parametrar:**

* `resources`: en array med resurser som användaren redan har behörighet att visa.

**Utlöses av:** [`checkPreauthorizedResources:`](#checkPreauth)



[Till början...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Den här metoden används av programmet för att kontrollera auktoriseringsstatusen. Det börjar med att kontrollera autentiseringsstatusen först. Om den inte autentiseras utlöses [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed)-återanropet och metoden avslutas. Om användaren är autentiserad utlöses även auktoriseringsflödet. Se information om metoden [`getAuthorization:`](#getAuthZ).


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: kontrollera auktoriseringsstatus</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: kontrollera auktoriseringsstatus</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.9+

**Parametrar:**

* *resource*: ID:t för resursen som användaren begär auktorisering för.
* *data*: En ordlista som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Till början...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Den här metoden används av programmet för att initiera auktoriseringsflödet. Om användaren inte redan är autentiserad initieras även autentiseringsflödet. Om användaren autentiseras fortsätter AccessEnabler att utfärda begäranden för auktoriseringstoken (om det inte finns någon giltig auktoriseringstoken i det lokala token-cachen) och för den kortlivade medietoken. När den korta medietoken har hämtats anses auktoriseringsflödet vara slutfört. Återanropet [`setToken:forResource:`](#setToken) aktiveras och den korta medietoken levereras som en parameter till programmet. Om auktoriseringen av någon anledning misslyckas, aktiveras återanropet [`tokenRequestFailed:forEventType:`](#tokenReqFailed) och felkoden/informationen anges.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initiera auktoriseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initiera auktoriseringsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**Tillgänglighet:** v1.9+

**Parametrar:**

* *resource*: ID:t för resursen som användaren begär auktorisering för.
* *data*: En ordlista som består av nyckelvärdepar som ska skickas till Pay-TV-pass-tjänsten. Adobe kan använda dessa data för att aktivera framtida funktioner utan att ändra SDK.

**Återanrop har utlösts:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Ytterligare återanrop har utlösts:**\
Den här metoden kan även utlösa följande återanrop (om autentiseringsflödet också initieras): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Använd `checkAuthorization:` / `checkAuthorization:withData:` i stället för `getAuthorization:` / `getAuthorization:withData:` när det är möjligt. Metoden `getAuthorization:` / `getAuthorization:withData:` startar ett fullständigt autentiseringsflöde (om användaren inte är autentiserad) och detta kan leda till en komplicerad implementering på programmerarens sida.

[Till början...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop utlöses av AccessEnabler som informerar programmet om att auktoriseringsflödet har slutförts. Den kortlivade medietoken levereras också som en parameter.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: auktoriseringsflödet har slutförts</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar**:

* *token*: den kortlivade medietoken
* *resource*: den resurs som auktoriseringen hämtades för

**Utlöses av:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Till början...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop utlöses av AccessEnabler som informerar programmet i det övre lagret om att auktoriseringsflödet misslyckades.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: auktoriseringsflödet misslyckades</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar**:

* *resource*: Resursen som auktoriseringen hämtades för.
* *kod*: Felkoden som är associerad med felscenariot. Möjliga värden:
   * `USER_NOT_AUTHORIZED_ERROR` - användaren kunde inte auktorisera
för angiven resurs
* *description*: Ytterligare information om felscenariot. Om den här beskrivande strängen inte är tillgänglig av någon anledning skickar Adobe Pass Authentication en tom sträng **(&quot;&quot;)**.\
  Strängen kan användas av ett MVPD-program för att skicka anpassade felmeddelanden eller försäljningsrelaterade meddelanden. Om en prenumerant nekas behörighet för en resurs kan MVPD skicka ett meddelande som:&quot;Du har för närvarande inte åtkomst till den här kanalen i ditt paket. Om du vill uppgradera ditt paket klickar du **här**.&quot; Meddelandet skickas av Adobe Pass Authentication via det här återanropet till programmeraren, som har möjlighet att visa eller ignorera det. Adobe Pass Authentication kan också använda den här parametern för att ge ett meddelande om det tillstånd som kan ha orsakat ett fel. Exempel:&quot;Ett nätverksfel uppstod vid kommunikation med leverantörens auktoriseringstjänst&quot;.

**Utlöses av:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Till början...](#apis)

</br>

### utloggning {#logout}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Den här metoden anropas av programmet för att initiera utloggningsflödet. Utloggningen är resultatet av en serie HTTP-omdirigeringsåtgärder på grund av att användaren måste loggas ut både från Adobe Pass autentiseringsservrar och från MVPD-servrarna. Eftersom det här flödet inte kan slutföras med en enkel HTTP-begäran som utfärdas av AccessEnabler-biblioteket, måste en `UIWebView/WKWebView or SFSafariViewController`-styrenhet instansieras för att kunna följa HTTP-omdirigeringsåtgärderna.

Utloggningsflödet skiljer sig från autentiseringsflödet på så sätt att användaren inte behöver interagera med `UIWebView/WKWebView or SFSafariViewController`-styrenheten på något sätt. Därför rekommenderar Adobe att du gör kontrollen osynlig (dvs. dold) under utloggningsprocessen.

Ett mönster som liknar autentiseringsflödet används. IOS AccessEnabler utlöser återanropet `navigateToUrl:` eller `navigateToUrl:useSVC:` för att skapa en `UIWebView/WKWebView or SFSafariViewController`-kontrollant och för att läsa in URL:en som anges i återanropets `url`-parameter. Det här är URL:en för utloggningsslutpunkten på backend-servern. För tvOS AccessEnabler anropas varken callback-funktionen `navigateToUrl:` eller callback-funktionen `navigateToUrl:useSVC:`.

När programmet går igenom flera omdirigeringar måste du övervaka aktiviteten för `UIWebView/WKWebView or SFSafariViewController `kontrollanten och identifiera tidpunkten när den läser in en specifik anpassad URL. Observera att den här anpassade URL:en är ogiltig och inte avsedd för att styrenheten ska läsa in den. Det får endast tolkas av programmet som en signal på att utloggningsflödet har slutförts och att det är säkert att stänga kontrollenheten. När kontrollenheten läser in den här anpassade URL:en måste programmet stänga kontrollenheten och anropa AccessEnablers `handleExternalURL:url `API-metod. Om ditt program måste använda en `SFSafariViewController `kontrollant definieras den anpassade URL:en av din `application's custom scheme` (t.ex. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), annars definieras den här anpassade URL:en av konstanten `ADOBEPASS_REDIRECT_URL ` (t.ex. `adobepass://ios.app`).

I slutet anropar AccessEnabler återanropet [`setAuthenticationStatus()`](#setAuthNStatus) med statuskoden 0, vilket anger att utloggningsflödet lyckades.

**Obs!** Om användaren är inloggad med Apple SSO aktiveras VSA203-statusen. I så fall bör användaren instrueras att även logga ut från systeminställningarna. Om du inte gör det kommer det att resultera i en omautentisering när programmet startas om.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: initiera utloggningsflödet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Till början...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Använd den här metoden för att fastställa den valda providern.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: avgöra vilket MVPD som är valt</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar:** Inga

**Återanrop har utlösts:** [`selectedProvider:`](#selProv)

[Till början...](#apis)

</br>

### selectedProvider {#selProv}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop som utlöses av AccessEnabler som skickar information om det aktuella MVPD-värdet till programmet.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: information om det aktuella MVPD-värdet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar**:

* *mvpd*: objekt som innehåller information om det MVPD som är valt

**Utlöses av:** [`getSelectedProvider`](#getSelProv)

[Till början...](#apis)

</br>

### getMetadata: {#getMeta}

**Fil:** AccessEnabler/headers/AccessEnabler.h

**Beskrivning:** Använd den här metoden för att hämta information som exponeras som metadata av AccessEnabler-biblioteket. Programmet kan komma åt dessa data genom att tillhandahålla en ordlistebaserad *key*-indataparameter.

Programmerarna har två typer av metadata:

* Statiska metadata (autentiseringstoken TTL, auktoriseringstoken TTL och enhets-ID)
* Användarmetadata (användarspecifik information, t.ex. användar-ID, postnummer; kan skickas från ett MVPD till en användares enhet under autentiserings- och auktoriseringsflödena)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API-anrop: fråga AccessEnabler om metadata</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar:**

* *keyDictionary*: en ordlistedatastruktur med följande
format:
   * Om nyckeln är `METADATA_OPCODE_KEY` och värdet är `METADATA_AUTHENTICATION` ställs frågan för att erhålla förfallotiden för autentiseringstoken.
   * Om nyckeln är `METADATA_OPCODE_KEY` och värdet är `METADATA_AUTHORIZATION` **och**\
     nyckeln är `METADATA_RESOURCE_ID_KEY` och värdet är ett visst resurs-ID. Frågan görs för att hämta förfallotiden för den auktoriseringstoken som är associerad med den angivna resursen.
   * Om nyckeln är `METADATA_OPCODE_KEY` och värdet är `METADATA_DEVICE_ID` ställs frågan för att hämta aktuellt enhets-ID. Observera att den här funktionen är inaktiverad som standard och programmerare bör kontakta Adobe för att få information om aktivering och avgifter.
   * Om nyckeln är `METADATA_OPCODE_KEY` och värdet är `METADATA_USER_META` **och** nyckeln är `METADATA_USER_META_KEY` och värdet är namnet på metadata, görs frågan för användarens metadata. Listan över tillgängliga metadatatyper för användare:
      * `zip` - Lista med postnummer
      * `householdID` - Hushållsidentifierare. Om ett MVPD inte stöder underkonton är detta identiskt med `userID`.
      * `maxRating` - En samling med maximala föräldraklassificeringar för användaren
      * `userID` - användaridentifieraren. Om ett MVPD-program stöder underkonton och användaren inte är huvudkontot, kommer `userID` att vara annorlunda än `householdID.`
      * `channelID` - En lista över kanaler som en användare har rätt att visa.

  >[!NOTE]
  >
  >Vilka faktiska användarmetadata som är tillgängliga för en programmerare beror på vad ett separat programmeringsdokument (MVPD) erbjuder. Listan utökas när nya metadata blir tillgängliga och läggs till i Adobe Pass autentiseringssystem.

**Återanrop har utlösts:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Mer information:** [Användarmetadata](/help/authentication/user-metadata.md)

[Till början...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop som utlöses av AccessEnabler efter anrop av [getAuthentication()](#getAuthN) om den aktuella begäraren har stöd för minst ett MVPD med SSO-stöd.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: resultatet av SSO-flöden</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v2.0+

**Parametrar**:

* viewController: representerar Apple SSO-dialogrutan. Denna viewController måste visas på skärmen.

**Utlöses av:** [`getAuthentication`](#getAuthN)

**Mer information:** [iOS/tvOS enkel inloggning](#presentTvDialog)

[Till början...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop som utlöses av AccessEnabler när användaren stänger Apple SSO-dialogrutan.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: resultatet av SSO-flöden</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v2.0+

**Parametrar**:

* viewController: representerar Apple SSO-dialogrutan. Denna viewController måste tas bort från skärmen.

**Utlöses av:** Användaråtgärd

**Mer information:** [iOS/tvOS enkel inloggning](#presentTvDialog)

[Till början...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop som utlöses av AccessEnabler som levererar de metadata som efterfrågas via ett [`getMetadata:`](#getMeta)-anrop.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Återanrop: resultatet av begäran om hämtning av metadata</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**Tillgänglighet:** v1.0+

**Parametrar**:

* *metadata*: Begärda metadata. Det här värdet är ett `NSString` när det gäller statiska metadata (autentiserings-TTL, auktoriserings-TTL, enhets-ID).  Det är ett komplext objekt när användarspecifika metadata begärs. Det här komplexa objektet är vanligtvis mål-C-representationen av en JSON-nyttolast (t.ex. &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [&quot;150&quot;, &quot;320&quot;]&#39; översätts i mål-C som NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;15 0&quot;, &quot;320&quot;).   Exempelmetadata-JSON-objekt:

```JSON
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
```

* *krypterad*: Ett booleskt värde som anger om de hämtade metadata är krypterade eller inte. Den här parametern är bara viktig för användarmetadatabegäranden, den har ingen betydelse för statiska metadata (t.ex. autentiserings-TTL) som alltid tas emot okrypterat. Om den här parametern är inställd på true, är det programmeraren som ska hämta det okrypterade användarmetadatavärdet genom att utföra en RSA-dekryptering med den privata nyckeln för vitlistning (samma privata nyckel som används för signering av det begärande ID:t i anropen [`setRequestor:setSignedRequestorId:`](#setReq) och `setRequestor:setSignedRequestorId:serviceProviders: `).

* *nyckel*: Nyckeln som används för att formulera begäran om hämtning av metadata.

* *arguments*: Samma ordlista som skickades till anropet [`getMetadata:`](#getMeta). Detta tillhandahålls för att programmet ska kunna matcha förfrågningarna med svaren.

**Utlöses av:** [`getMetadata:`](#getMeta)

**Mer information:** [Användarmetadata](/help/authentication/user-metadata.md)


[Till början...](#apis)

</br>

### MVPD {#mvpd}

**Fil:** AccessEnabler/headers/model/MVPD.h

**Beskrivning** Beskriver MVPD-objektet. Kan användas för att få information om MVPD:s egenskaper.

**Tillgänglighet:** v1.0+ [egenskapen boardingStatus är tillgänglig från v2.2]

**Egenskaper**:

* (NSString) ID - MVPD-ID.
* (NSString) displayName - MVPD-namnet. [Det här bör användas för att visa i väljaren]
* (NSString) logoURL - MVPD-logotypadressen.
* (BOOL) enablePlatformServices - Om värdet är true stöder MVPD SSO-tjänster som [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus - kan ha tre värden:
   * nil - MVPD stöder inte Apple SSO.
   * PICKER - MVPD kan visas i Apple-väljaren, men autentiseringsflödet görs av Adobe.
   * STÖDS - MVPD stöds fullt ut av Apple och kommer att använda Apple SSO-token.

[Till början...](#apis)

</br>

## Spårningshändelser {#tracking}

AccessEnabler utlöser en extra återanrop som inte nödvändigtvis är relaterad till berättigandeflödena. Det är valfritt att implementera callback-funktionen [`sendTrackingData()`](#sendTracking), men det gör att programmet kan spåra specifika händelser och sammanställa statistik som antalet lyckade/misslyckade autentiserings-/autentiseringsförsök.

### sendTrackingData:forEventType: {#sendTracking}

**Fil:** AccessEnabler/headers/EntitlementDelegate.h

**Beskrivning** Återanrop som utlöses av AccessEnabler som signalerar till programmet om förekomsten av olika händelser, till exempel slutförande/misslyckande av autentiserings-/auktoriseringsflöden. Med Adobe Pass Authentication 1.6 rapporteras enhetstypen, klienttypen AccessEnabler och operativsystemet av [`sendTrackingData()`](#sendTracking). Återanropet [`sendTrackingData()`](#sendTracking) är fortfarande bakåtkompatibelt.

**Återanrop: spåra händelser**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Tillgänglighet:** v1.0+

**Obs!** Enhetstypen och operativsystemet härleds genom användning av ett offentligt Java-bibliotek (<http://java.net/projects/user-agent-utils>) och användaragentsträngen. Observera att denna information endast tillhandahålls som ett grovt sätt att dela upp mätvärden för drift i enhetskategorier, men att Adobe inte kan ta något ansvar för felaktiga resultat. Använd den nya funktionen i enlighet med detta.

* Möjliga värden för enhetstypen:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Möjliga värden för klienttypen AccessEnabler:
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Parametrar**:

* *event*: koden för händelsen som spåras. Det finns tre typer av spårningshändelser:
   * **permissionDetection:** när en auktoriseringstokenbegäran returnerar (händelsen är `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** när en autentiseringskontroll inträffar (händelsen är `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** när användaren väljer ett MVPD i MVPD-markeringsformuläret (händelsen är `TRACKING_GET_SELECTED_PROVIDER`)
* *data*: ytterligare data som är associerade med den rapporterade händelsen. Dessa data presenteras i form av en lista med värden.

**Utlöses av:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instruktioner för tolkning av värdena i *data* -arrayen:

* För trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Anger om tokenbegäran lyckades (true/false) och om den lyckades:
   * **1** - MVPD ID-sträng
   * **2** - GUID (md5 hashed)
   * **3** - Token finns redan i cache (sant/falskt)
   * **4** - Enhetstyp
   * **5** - klienttypen AccessEnabler
   * **6** - Operativsystemtyp

* För trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Anger om tokenbegäran lyckades (true/false) och om den lyckades:
   * **1** - MVPD ID
   * **2** - GUID (md5 hashed)
   * **3** - Token finns redan i cache (sant/falskt)
   * **4** - Fel
   * **5** - information
   * **6** - Enhetstyp
   * **7** - klienttypen AccessEnabler
   * **8** - Operativsystemtyp
* För trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID för det MVPD som är markerat
   * **1** - Enhetstyp
   * **2** - klienttypen AccessEnabler
   * **3** - Operativsystemtyp

</br>
