---
title: Amazon FireOS Integration Cookbook
description: Amazon FireOS Integration Cookbook
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1424'
ht-degree: 0%

---

# Amazon FireOS Integration Cookbook {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>


## Introduktion {#intro}

I det här dokumentet beskrivs de tillståndsarbetsflöden som en programmers program på den översta nivån kan implementera via de API:er som exponeras av Amazon FireOS `AccessEnabler`-biblioteket.

Adobe Pass Authentication-berättigandelösningen för Amazon FireOS är uppdelad i två domäner:

- Användargränssnittsdomänen - det här är det övre programlagret som implementerar användargränssnittet och använder tjänsterna som tillhandahålls av biblioteket `AccessEnabler` för att ge åtkomst till begränsat innehåll.
- Domänen `AccessEnabler` - här implementeras berättigandearbetsflödena i form av:
   - Nätverksanrop gjorda till Adobe serverdel
   - Affärslogik för arbetsflödena för autentisering och auktorisering
   - Hantering av olika resurser och bearbetning av arbetsflödestillstånd (t.ex. tokencache)

Målet för domänen `AccessEnabler` är att dölja alla komplexiteter i tillståndsarbetsflödena och att tillhandahålla en uppsättning enkla berättigandeprinciper till programmet i det övre lagret (via biblioteket `AccessEnabler`). Med den här processen kan du implementera tillståndsarbetsflöden:

1. Ange identiteten för begärande.
1. Kontrollera och hämta autentisering mot en viss identitetsleverantör.
1. Kontrollera och få behörighet för en viss resurs.
1. Logga ut.

Nätverksaktiviteten för `AccessEnabler` äger rum i en annan tråd, så gränssnittstråden blockeras aldrig. Det innebär att den tvåvägskommunikationskanal som finns mellan de två programdomänerna måste följa ett fullständigt asynkront mönster:

- Gränssnittets programlager skickar meddelanden till domänen `AccessEnabler` via API-anrop som exponeras av biblioteket `AccessEnabler`.
- `AccessEnabler` svarar på UI-lagret via återanropsmetoderna i protokollet `AccessEnabler` som UI-lagret registrerar med biblioteket `AccessEnabler`.

## Tillståndsflöden {#entitlement}

1. [Förutsättningar](#prereqs)
1. [Startflöde](#startup_flow)
1. [Autentiseringsflöde](#authn_flow)
1. [Auktoriseringsflöde](#authz_flow)
1. [Visa medieflöde](#media_flow)
1. [Utloggningsflöde](#logout_flow)

### A. Förutsättningar {#prereqs}

1. Skapa callback-funktioner:
   - [`setRequestorComplete()`](#$setRequestorComplete)

      - Utlöses av `setRequestor()` och returnerar om åtgärden lyckades eller misslyckades.     Slutfört visar att du kan fortsätta med berättigandeanrop.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Utlöses endast av `getAuthentication()` om användaren inte har valt en leverantör (MVPD) och ännu inte är autentiserad. Parametern `mvpds` är en matris med providers som är tillgängliga för användaren.

   - [`setAuthenticationStatus(status, orsak)`](#$setAuthNStatus)

      - Utlöses av `checkAuthentication()` varje gång. Utlöses endast av `getAuthentication()` om användaren redan är autentiserad och har valt en leverantör.

      - Status som returneras är autentiserad eller inte autentiserad. Orsaken beskriver ett autentiseringsfel eller en utloggningsåtgärd.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Metoden ignoreras i AmazonFireOS SDK och används på Android-plattformar där utlöses av `getAuthentication()` efter att användaren har valt en MVPD.  Parametern `url` anger platsen för MVPD:s inloggningssida.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Utlöses av `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
Parametern `event` anger vilken berättigandehändelse som inträffade. Parametern `data` är en lista med värden som relaterar till händelsen.

   - [`setToken(token, resource)`](#$setToken)

      - Utlöses av `checkAuthorization()` och `getAuthorization()` efter en auktorisering för att visa en resurs.
      - Parametern `token` är den kortlivade medietoken. Parametern `resource` är det innehåll som användaren har behörighet att visa.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - Utlöses av `checkAuthorization()` och `getAuthorization()` efter en misslyckad auktorisering.
      - Parametern `resource` är det innehåll som användaren försökte visa. Parametern `code` är felkoden som anger vilken typ av fel som uppstod. Parametern `description` beskriver felet som är associerat med felkoden.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - Utlöses av `getSelectedProvider()`.
      - Parametern `mvpd` ger information om providern som valts av användaren.

   - [`setMetadataStatus(metadata, nyckel, argument)`](#$setMetadataStatus)

      - Utlöses av `getMetadata().`
      - Parametern `metadata` innehåller de specifika data som du har begärt, parametern `key` är nyckeln som används i begäran `getMetadata()` och parametern `arguments` är samma ordlista som skickades till `getMetadata()`.

   - [&quot;preauthorizedResources(resources)`](#$preauthResources)

      - Utlöses av `checkPreauthorizedResources()`.
      - Parametern `authorizedResources` visar de resurser som användaren har behörighet att visa.


![](assets/android-entitlement-flows.png)


### B. Startflöde {#startup_flow}

1. Starta programmet på den översta nivån.
1. Initiera Adobe Pass-autentisering.

   1. Anropa [`getInstance`](#$getInstance) om du vill skapa en enda instans av Adobe Pass-autentiseringen `AccessEnabler`.

      - **Beroende:** Adobe Pass Authentication Native Amazon FireOS Library (`AccessEnabler`)

   1. Anropa ` setRequestor()` för att upprätta identifieringen av programmeraren; skicka en matris med Adobe Pass Authentication-slutpunkter till programmeraren `requestorID` och (valfritt).

      - **Beroende:** Giltigt begärande-ID för Adobe Pass-autentisering (använd din kontohanterare för Adobe Pass-autentisering för att ordna detta.)

      - **Utlösare:** setRequestorComplete()-återanrop

   Inga berättigandebegäranden kan slutföras förrän identiteten för den som gjorde begäran har etablerats fullständigt. Detta innebär att när setRequestor() fortfarande körs blockeras alla efterföljande berättigandebegäranden (till exempel `checkAuthentication()`).

   Du har två implementeringsalternativ: När begärarens identifieringsinformation skickas till backend-servern kan gränssnittets programlager välja en av följande två metoder:</p>

   1. Vänta på att återanropet `setRequestorComplete()` aktiveras (ingår i delegaten `AccessEnabler`).  Det här alternativet ger den största säkerheten för att `setRequestor()` har slutförts, så det rekommenderas för de flesta implementeringar.
   1. Fortsätt utan att vänta på att återanropet `setRequestorComplete()` ska utlösas och börja utfärda tillståndsbegäranden. Dessa anrop (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) köas av biblioteket `AccessEnabler`, vilket gör att de faktiska nätverksanropen görs efter `setRequestor()`. Det här alternativet kan ibland avbrytas om nätverksanslutningen till exempel är instabil.

1. Anropa [checkAuthentication()](#$checkAuthN) om du vill söka efter en befintlig autentisering utan att initiera det fullständiga autentiseringsflödet.  Om samtalet lyckas kan du gå direkt till auktoriseringsflödet.  Om inte, fortsätter du till autentiseringsflödet.

- **Beroende:** Ett lyckat anrop till `setRequestor()` (det här beroendet gäller även för alla efterföljande anrop).

- **Utlösare:** setAuthenticationStatus() återanrop

### C. Autentiseringsflöde {#authn_flow}

1. Ring [`getAuthentication()`](#$getAuthN) för att initiera autentiseringsflödet eller för att få en bekräftelse på att användaren redan är autentiserad.

   **Utlösare:**

   - Callback-funktionen setAuthenticationStatus(), om användaren redan är autentiserad.  I det här fallet går du direkt till [auktoriseringsflödet](#authz_flow).
   - Callback-funktionen displayProviderDialog(), om användaren inte har autentiserats ännu.

1. Presentera användaren med listan över leverantörer som skickats till `displayProviderDialog()`.
1. När användaren har valt en leverantör öppnas providersidan så att användaren kan logga in.

   >[!NOTE]
   >
   >Nu har användaren möjlighet att avbryta autentiseringsflödet. Om detta inträffar rensar `AccessEnabler` det interna tillståndet och återställer autentiseringsflödet.

1. När användaren har loggat in stängs WebView.
1. anropa `getAuthenticationToken(),` som instruerar `AccessEnabler` att hämta autentiseringstoken från serverdelen.
1. [Valfritt] samtal [`checkPreauthorizedResources(resources)`](#$checkPreauth) för att kontrollera vilka resurser användaren har behörighet att visa. Parametern `resources` är en matris med skyddade resurser som är associerad med användarens autentiseringstoken.

   **Utlösare:** `preAuthorizedResources()` återanrop\
   **Körningspunkt:** Efter det slutförda autentiseringsflödet

1. Om autentiseringen lyckades fortsätter du till auktoriseringsflödet.


### D. Auktoriseringsflöde {#authz_flow}

1. Anropa [`getAuthorization()`](#$getAuthZ) för att initiera auktoriseringsflödet.

   Beroende: Giltiga resurs-ID:n som har avtalats med MVPD:n.

   **Obs!** Resurs-ID:n ska vara samma som de som används på andra enheter eller plattformar och ska vara samma för alla flerkanalsprogram.

1. Validera autentisering och auktorisering.

   - Om anropet `getAuthorization()` lyckas: Användaren har giltiga AuthN- och AuthZ-tokens (användaren är autentiserad och har behörighet att titta på det begärda mediet).
   - Om `getAuthorization()` misslyckas: Undersök undantaget som utlösts för att fastställa dess typ (AuthN, AuthZ eller något annat):
      - Om det var ett autentiseringsfel (AuthN) startar du om autentiseringsflödet.
      - Om det var ett auktoriseringsfel (AuthZ) har användaren inte behörighet att titta på det begärda mediet och någon typ av felmeddelande ska visas för användaren.
      - Om det finns någon annan typ av fel (anslutningsfel, nätverksfel osv.) visar sedan ett felmeddelande för användaren.

1. Validera den korta medietoken.

   Använd Adobe Pass Authentication Media Token Verifier-biblioteket för att verifiera den kortlivade medietoken som returnerats från `getAuthorization()`-anropet ovan:

   - Om valideringen lyckas: Spela upp det begärda mediet för användaren.
   - Om valideringen misslyckas: AuthZ-token var ogiltig, ska mediebegäran avvisas och ett felmeddelande ska visas för användaren.

1. Återgå till det normala programflödet.

### E. Visa medieflöde {#media_flow}

1. Användaren väljer de media som ska visas.
1. Är mediet skyddat?  Programmet kontrollerar om det valda mediet är skyddat:
   - Om det valda mediet är skyddat startar programmet [auktoriseringsflödet](#authz_flow) ovan.
   - Om det valda mediet inte är skyddat kan du spela upp mediet för användaren.

### F. Utloggningsflöde {#logout_flow}

1. Ring [`logout()`](#$logout) för att logga ut användaren. `AccessEnabler` rensar bort alla cachelagrade värden och token som har hämtats av användaren för det aktuella MVPD-värdet på alla begärande som delar inloggningen via enkel inloggning. När cacheminnet har rensats gör `AccessEnabler` ett serveranrop för att rensa sessionerna på serversidan.  Observera, att eftersom serveranropet kan resultera i en SAML-omdirigering till IdP (detta tillåter sessionsrensning på IdP-sidan), måste det här anropet följa alla omdirigeringar. Därför hanteras det här anropet inuti en WebView-kontroll, som inte är synlig för användaren.

   **Obs!** Loggoutflödet skiljer sig från autentiseringsflödet på så sätt att användaren inte behöver interagera med WebView på något sätt. Det är därför möjligt (och rekommenderas) att göra WebView-kontrollen osynlig (dvs. dold) under utloggningsprocessen.
