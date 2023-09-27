---
title: iOS/tvOS Cookbook
description: iOS/tvOS Cookbook
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2404'
ht-degree: 0%

---

# iOS/tvOS SDK Cookbook {#iostvos-sdk-cookbook}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

I det här dokumentet beskrivs de tillståndsarbetsflöden som kan implementeras av en programmerares program på den översta nivån via de API:er som exponeras av iOS/tvOS AccessEnabler-biblioteket.

Adobe Pass Authentication-berättigandelösningen för iOS/tvOS är uppdelad i två domäner:

* Användargränssnittets domän - det här är det övre programlagret som implementerar användargränssnittet och använder tjänsterna som tillhandahålls av AccessEnabler-biblioteket för att ge åtkomst till begränsat innehåll.

* Domänen AccessEnabler - det är här som berättigandearbetsflödena implementeras i form av:

   * Nätverksanrop gjorda till Adobe serverdel
   * Affärslogik för arbetsflödena för autentisering och auktorisering
   * Hantering av olika resurser och bearbetning av arbetsflödestillstånd (t.ex. tokencache)

Målet med AccessEnabler-domänen är att dölja alla komplexa berättigandearbetsflöden och ge programmet i det övre lagret (via AccessEnabler-biblioteket) en uppsättning enkla berättigandeprinciper som du använder för berättigandearbetsflöden:

1. Ange identitet för begärande
1. Kontrollera och hämta autentisering mot en viss identitetsleverantör
1. Kontrollera och få behörighet för en viss resurs
1. Utloggning
1. Apple SSO flödar genom att proxera Apple VSA-ramverket

Nätverksaktiviteten för AccessEnabler äger rum i en egen tråd, så gränssnittstråden blockeras aldrig. Det innebär att den tvåvägskommunikationskanal som finns mellan de två programdomänerna måste följa ett fullständigt asynkront mönster:

* Gränssnittets programlager skickar meddelanden till AccessEnabler-domänen via API-anropen som visas av AccessEnabler-biblioteket.
* AccessEnabler svarar på UI-lagret via callback-metoderna i protokollet AccessEnabler som UI-lagret registreras med AccessEnabler-biblioteket.

## Konfigurera Experience Cloud ID-tjänsten (besökar-ID) {#visitorIDSetup}

Konfigurera [EXPERIENCE CLOUD ID](https://experienceleague.adobe.com/docs/id-service/using/home.html) värdet är viktigt från [!DNL Analytics] synvinkel. En gång en `visitorID` är inställt skickar SDK den här informationen tillsammans med varje nätverksanrop och [!DNL Adobe Pass] Autentiseringsservern samlar in den här informationen. Du kan korrelera analysen från Adobe Pass autentiseringstjänst med andra analysrapporter som du kan ha från andra program eller webbplatser. Information om hur du konfigurerar besökar-ID finns [här](#setOptions).

## Tillståndsflöden {#entitlement}

S.  [Förutsättningar](#prereqs) </br>
B.  [Startflöde](#startup_flow) </br>
C.  [Autentiseringsflöde med Apple SSO](#authn_flow_wo_applesso)  </br>
D.  [Autentiseringsflöde med Apple SSO på iOS](#authn_flow_with_applesso) </br>
E.  [Autentiseringsflöde med Apple SSO på tvOS](#authn_flow_with_applesso_tvOS) </br>
F.  [Auktoriseringsflöde](#authz_flow) </br>
G.  [Visa medieflöde](#media_flow) </br>
H.  [Utloggningsflöde utan Apple SSO](#logout_flow_wo_AppleSSO) </br>
Jag.  [Utloggningsflöde med Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Förutsättningar {#prereqs}

1. Skapa callback-funktioner:
   * `setRequestorComplete()` </br>
   * Utlöses av [setRequestor()](#$setReq), returnerar om åtgärden lyckades eller misslyckades. </br>
   * Slutfört visar att du kan fortsätta med berättigandeanrop.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Utlöses av [`getAuthentication()`](#$getAuthN) endast om användaren inte har valt en leverantör (MVPD) och ännu inte är autentiserad. </br>
      * The `mvpds` är en array med providers som är tillgängliga för användaren.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Utlöses av `checkAuthentication()` varje gång. </br>
      * Utlöses av [`getAuthentication()`](#$getAuthN) endast om användaren redan är autentiserad och har valt en leverantör. </br>
      * Status som returneras är lyckad eller misslyckad. Felkoden beskriver typen av fel.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Utlöses av [`getAuthentication()`](#$getAuthN) efter att användaren har valt ett MVPD. The `url` parameter anger platsen för MVPD:s inloggningssida.

   * `sendTrackingData(event, data)` </br>
      * Utlöses av `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * The `event` parametern anger vilken berättigandehändelse som har inträffat, `data` parameter är en lista med värden som relaterar till händelsen.

   * `setToken(token, resource)`

      * Utlöses av [checkAuthorization()](#checkAuthZ) och [getAuthorization()](#$getAuthZ) efter en auktorisering för att visa en resurs.
      * The `token` parametern är den kortlivade medietoken, `resource` -parametern är det innehåll som användaren har behörighet att visa.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Utlöses av [checkAuthorization()](#checkAuthZ) och [getAuthorization()](#$getAuthZ) efter en misslyckad auktorisering.
      * The `resource` parametern är det innehåll som användaren försöker visa, `code` parametern är felkoden som anger vilken typ av fel som inträffat. `description` -parametern beskriver felet som är associerat med felkoden.

   * `selectedProvider(mvpd)` </br>
      * Utlöses av [`getSelectedProvider()`](#getSelProv).
      * The `mvpd` -parametern ger information om den leverantör som användaren har valt.

   * `setMetadataStatus(metadata, key, arguments)`
      * Utlöses av `getMetadata().`
      * The `metadata` parametern innehåller de specifika data som du har begärt; `key` parametern är nyckeln som används i [getMetadata()](#getMeta) begäran, och `arguments` parametern är samma ordlista som skickas till [getMetadata()](#getMeta).

   * [`preauthorizedResources(authorizedResources)`](#preauthResources)

      * Utlöses av [`checkPreauthorizedResources()`](#checkPreauth).

      * The `authorizedResources` -parametern visar de resurser som användaren har behörighet att visa.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Utlöses av [getAuthentication()](#getAuthN) när den aktuella begäraren har stöd för minst en MVPD som har stöd för enkel inloggning.
      * Parametern viewController är Apple SSO-dialogrutan och måste presenteras på huvudvykontrollanten.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Utlöses av en användaråtgärd (genom att välja Avbryt eller Andra TV-leverantörer i Apple SSO-dialogruta).
      * Parametern viewController är Apple SSO-dialogrutan och måste stängas av från huvudvykontrollanten.

![](assets/iOS-flows.png)

### B. Startflöde {#startup_flow}

1. Starta programmet på den översta nivån.</br>
1. Initiera Adobe Pass-autentisering </br>

   a. Utlysning [`init`](#$init) för att skapa en instans av Adobe Pass Authentication AccessEnabler.
   * **Beroende:** Adobe Pass Authentication Native iOS/tvOS Library (AccessEnabler)

   b. Utlysning `setRequestor()` fastställa programmerarens identitet, skicka in programmerarens `requestorID` och (valfritt) en array med slutpunkter för Adobe Pass-autentisering. För tvOS måste du även ange den offentliga nyckeln och hemligheten. Se [Klientlös dokumentation](#create_dev) för mer information.

   * **Beroende:** Giltigt Adobe Pass Authentication RequestorID (Använd din kontohanterare för Adobe Pass-autentisering för att ordna detta).

   * **Utlösare:**
     [setRequestorComplete()](#$setReqComplete) återanrop.

   >[!NOTE]
   >
   >Inga berättigandebegäranden kan slutföras förrän identiteten för den som gjorde begäran har etablerats fullständigt. Detta innebär att [`setRequestor()`](#$setReq)  körs fortfarande, alla efterföljande berättigandebegäranden. Till exempel: [`checkAuthentication()`](#checkAuthN) är blockerade.

   Du har två implementeringsalternativ: När begärarens identifieringsinformation skickas till backend-servern kan gränssnittets programlager välja en av följande två metoder: </br>

   1. Vänta på att utlösaren för [`setRequestorComplete()`](#setReqComplete) callback (ingår i AccessEnabler-delegaten). Det här alternativet ger den största säkerheten för att [`setRequestor()`](#$setReq) slutförd, så det rekommenderas för de flesta implementeringar.

   1. Fortsätt utan att vänta på att utlösaren för [`setRequestorComplete()`](#setReqComplete) återanrop och börja utfärda tillståndsansökningar. Dessa anrop (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) köas av AccessEnabler-biblioteket, som gör att de faktiska nätverksanropen görs efter [`setRequestor()`](#$setReq). Det här alternativet kan ibland avbrytas om nätverksanslutningen till exempel är instabil.

1. Utlysning `checkAuthentication()` för att kontrollera om det finns en befintlig autentisering utan att initiera det fullständiga autentiseringsflödet.  Om samtalet lyckas kan du gå direkt till auktoriseringsflödet. Om inte, fortsätter du till autentiseringsflödet.

   * **Beroende:** Ett samtal till [setRequestor()](#$setReq) (detta beroende gäller även för alla efterföljande anrop).

   * **Utlösare:** [setAuthenticationStatus()](#$setAuthNStatus) återanrop.


### C. Autentiseringsflöde utan Apple SSO {#authn_flow_wo_applesso}

1. Utlysning [`getAuthentication()`](#$getAuthN) för att initiera autentiseringsflödet eller för att få en bekräftelse på att användaren redan är autentiserad.

   **Utlösare:**

   * The [setAuthenticationStatus()](#$setAuthNStatus) återanrop, om användaren redan är autentiserad. I det här fallet går du direkt till [Auktoriseringsflöde](#authz_flow).

   * The [displayProviderDialog()](#$dispProvDialog) återanrop, om användaren inte har autentiserats ännu.

1. Presentera användaren med listan över leverantörer som skickats till
   [`displayProviderDialog()`](#dispProvDialog).

1. När användaren har valt en leverantör hämtar du URL:en för användarens MVPD från `navigateToUrl:` eller `navigateToUrl:useSVC:` callback och öppna en `UIWebView/WKWebView` eller `SFSafariViewController` och dirigera kontrollenheten till webbadressen.

1. Via `UIWebView/WKWebView` eller `SFSafariViewController` som initierades i föregående steg, kommer användaren till MVPD:s inloggningssida och inloggningsuppgifter. Flera omdirigeringsåtgärder utförs inom den kontrollenheten.</br>

>[!NOTE]
>
>Nu har användaren möjlighet att avbryta autentiseringsflödet. Om detta inträffar ansvarar ditt UI-lager för att informera AccessEnabler om händelsen genom att anropa [setSelectedProvider()](#setSelProv) med `null` som en parameter. Detta gör att AccessEnabler kan rensa upp dess interna tillstånd och återställa autentiseringsflödet.

1. När användaren har loggat in identifierar programlagret inläsningen av en specifik anpassad URL. Observera att den här anpassade URL:en är ogiltig och inte avsedd för att styrenheten ska läsa in den. Det får endast tolkas av programmet som en signal om att autentiseringsflödet har slutförts och att det är säkert att stänga `UIWebView/WKWebView` eller `SFSafariViewController` styrenhet. Om `SFSafariViewController`kontrollenheten måste användas. Den anpassade URL:en definieras av **`application's custom scheme`** (till exempel`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), annars definieras den här anpassade URL:en av **`ADOBEPASS_REDIRECT_URL`** konstant (det vill säga `adobepass://ios.app`).

1. Stäng styrenheten UIWebView/WKWebView eller SFSafariViewController och anropa AccessEnablers `handleExternalURL:url` API-metod, som instruerar AccessEnabler att hämta autentiseringstoken från backend-servern.

1. (Valfritt) Utlysning [`checkPreauthorizedResources(resources)`](#$checkPreauth) för att kontrollera vilka resurser användaren har behörighet att visa. The `resources` parametern är en array med skyddade resurser som är associerad med användarens autentiseringstoken. En användning för auktoriseringsinformation som hämtas från användarens MVPD är att dekorera användargränssnittet (t.ex. låsta/olåsta symboler bredvid skyddat innehåll).

   * **Utlösare:** [`preauthorizedResources()`](#preauthResources) callback
   * **Körningspunkt:** Efter det slutförda autentiseringsflödet

1. Om autentiseringen lyckades fortsätter du till auktoriseringsflödet.

### D. Autentiseringsflöde med Apple SSO på iOS {#authn_flow_with_applesso}

1. Utlysning [`getAuthentication()`](#$getAuthN) för att initiera autentiseringsflödet eller för att få en bekräftelse på att användaren redan är autentiserad.
   **Utlösare:**

   * The [presentTvProviderDialog()](#presentTvDialog) återanrop, om användaren inte är autentiserad och den aktuella begäraren har minst ett MVPD som stöder enkel inloggning. Om inga MVPD-program stöder enkel inloggning används det klassiska autentiseringsflödet.

1. När användaren har valt en provider får AccessEnabler-biblioteket en autentiseringstoken med information från Apple VSA-ramverk.

1. The [setAuthenticationsStatus()](#setAuthNStatus) återanrop aktiveras. Nu bör användaren autentiseras med Apple SSO.

1. [Valfritt] Utlysning [`checkPreauthorizedResources(resources)`](#$checkPreauth) för att kontrollera vilka resurser användaren har behörighet att visa. The `resources` parametern är en array med skyddade resurser som är associerad med användarens autentiseringstoken. En användning för auktoriseringsinformation som hämtas från användarens MVPD är att dekorera användargränssnittet (t.ex. låsta/olåsta symboler bredvid skyddat innehåll).

   * **Utlösare:** [`preauthorizedResources()`](#preauthResources) callback
   * **Körningspunkt:** Efter det slutförda autentiseringsflödet

1. Om autentiseringen lyckades fortsätter du till auktoriseringsflödet.

### E. Autentiseringsflöde med Apple SSO på tvOS {#authn_flow_with_applesso_tvOS}

1. Utlysning [`getAuthentication()`](#$getAuthN) för att initiera autentiseringsflödet eller för att få en bekräftelse på att användaren redan är autentiserad.
   **Utlösare:**
   * The [`presentTvProviderDialog()`](#presentTvDialog) återanrop, om användaren inte är autentiserad och den aktuella begäraren har minst ett MVPD som stöder enkel inloggning. Om inga MVPD-program stöder enkel inloggning används det klassiska autentiseringsflödet.

1. När användaren har valt en leverantör [`status()`](#status_callback_implementation) återanrop anropas. En registreringskod kommer att anges och AccessEnabler-biblioteket börjar avfråga servern för att erhålla en andra skärmautentisering.

1. Om den angivna registreringskoden användes för att autentisera på den andra skärmen [`setAuthenticatiosStatus()`](#setAuthNStatus) återanrop aktiveras. Nu bör användaren autentiseras med Apple SSO.
1. [Valfritt] Utlysning [`checkPreauthorizedResources(resources)`](#$checkPreauth) för att kontrollera vilka resurser användaren har behörighet att visa. The `resources` parametern är en array med skyddade resurser som är associerad med användarens autentiseringstoken. En användning för auktoriseringsinformation som hämtas från användarens MVPD är att dekorera användargränssnittet (t.ex. låsta/olåsta symboler bredvid skyddat innehåll).

   * **Utlösare:** [`preauthorizedResources()`](#preauthResources) callback

   * **Körningspunkt:** Efter det slutförda autentiseringsflödet
1. Om autentiseringen lyckades fortsätter du till auktoriseringsflödet.

### F. Auktoriseringsflöde {#authz_flow}

1. Utlysning [getAuthorization()](#$getAuthZ) för att initiera auktoriseringsflödet.

   * **Beroende:** Giltiga resurs-ID:n som avtalats med MVPD:n.
   * Resurs-ID:n ska vara samma som de som används på andra enheter eller plattformar och ska vara samma för alla programmeringsgränssnitten. Mer information om resurs-ID:n finns i [Identifiera skyddade resurser](/help/authentication/identify-protected-resources.md)

1. Validera autentisering och auktorisering.

   * Om [getAuthorization()](#$getAuthZ) anropet lyckades: Användaren har giltiga AuthN- och AuthZ-tokens (användaren är autentiserad och har behörighet att titta på det begärda mediet).

   * If [getAuthorization()](#$getAuthZ) misslyckas: Undersök undantaget som utlöses för att fastställa dess typ (AuthN, AuthZ eller något annat):
      * Om det var ett autentiseringsfel (AuthN) startar du om autentiseringsflödet.
      * Om det var ett auktoriseringsfel (AuthZ) har användaren inte behörighet att titta på det begärda mediet och någon typ av felmeddelande ska visas för användaren.
      * Om det finns någon annan typ av fel (anslutningsfel, nätverksfel osv.) visar sedan ett felmeddelande för användaren.

1. Validera den korta medietoken.\
   Använd Adobe Pass Authentication Media Token Verifier-biblioteket för att verifiera den kortlivade medietoken som returneras från [getAuthorization()](#$getAuthZ) ring ovan:

   * Om valideringen lyckas: Spela upp det begärda mediet för användaren.
   * Om valideringen misslyckas: AuthZ-token var ogiltig, ska mediebegäran avvisas och ett felmeddelande ska visas för användaren.


1. Återgå till det normala programflödet.

### G. Visa medieflöde {#media_flow}

1. Användaren väljer de media som ska visas.
1. Är mediet skyddat? Programmet kontrollerar om det valda mediet är skyddat:

   * Om det valda mediet är skyddat startar programmet [Auktoriseringsflöde](#authz_flow) ovan.

   * Om det valda mediet inte är skyddat kan du spela upp mediet för användaren.

### H. Utloggningsflöde utan Apple SSO {#logout_flow_wo_AppleSSO}

1. Utlysning [`logout()`](#$logout) för att logga ut användaren. AccessEnabler rensar bort alla cachelagrade värden och token. När cacheminnet har rensats gör AccessEnabler ett serveranrop för att rensa sessionerna på serversidan. Observera, att eftersom serveranropet kan resultera i en SAML-omdirigering till IdP (detta tillåter sessionsrensning på IdP-sidan), måste det här anropet följa alla omdirigeringar. Därför måste anropet hanteras inuti en UIWebView/WKWebView- eller SFSafariViewController-styrenhet.

   a. I samma mönster som autentiseringsarbetsflödet skickar AccessEnabler-domänen en en begäran till gränssnittets programlager via `navigateToUrl:` eller `navigateToUrl:useSVC:` återanrop för att skapa en UIWebView/WKWebView- eller SFSafariViewController-styrenhet och instruera den att läsa in URL:en som finns i återanropets `url` parameter. Det här är URL:en för utloggningsslutpunkten på backend-servern.

   b. Programmet måste övervaka aktiviteten i `UIWebView/WKWebView or SFSafariViewController` och identifiera tidpunkten då en viss anpassad URL läses in, allt eftersom den går igenom flera omdirigeringar. Observera att den här anpassade URL:en är ogiltig och inte avsedd för att styrenheten ska läsa in den. Det får endast tolkas av programmet som en signal om att utloggningsflödet har slutförts och att det är säkert att stänga `UIWebView/WKWebView` eller `SFSafariViewController` styrenhet. När kontrollenheten läser in den här anpassade URL:en måste programmet stänga `UIWebView/WKWebView or SFSafariViewController` och anropa AccessEnabler `handleExternalURL:url`API-metod. Om `SFSafariViewController`kontrollenheten måste användas. Den anpassade URL:en definieras av **`application's custom scheme`** (till exempel `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), annars definieras den här anpassade URL:en av **`ADOBEPASS_REDIRECT_URL`**  konstant (det vill säga `adobepass://ios.app`).

   >[!NOTE]
   >
   >Utloggningsflödet skiljer sig från autentiseringsflödet på så sätt att användaren inte behöver interagera med UIWebView/WKWebView eller SFSafariViewController på något sätt. Användargränssnittets programlager använder en UIWebView/WKWebView eller SFSafariViewController för att se till att alla omdirigeringar följs. Det är därför möjligt (och rekommenderas) att göra kontrollenheten osynlig (dvs. dold) under utloggningsprocessen.


### I. Utloggningsflöde med Apple SSO {#logout_flow_with_AppleSSO}

1. Utlysning [`logout()`](#$logout) för att logga ut användaren.
1. The [status()](#status_callback_implementation) återanrop anropas med ID VSA203.
1. Användaren bör även instrueras att logga in från systeminställningarna. Om du inte gör det kommer det att resultera i en omautentisering när programmet startas om.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
