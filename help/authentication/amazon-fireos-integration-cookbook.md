---
title: Amazon FireOS Integration Cookbook
description: Amazon FireOS Integration Cookbook
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1432'
ht-degree: 0%

---

# Amazon FireOS Integration Cookbook {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>


## Introduktion {#intro}

I det här dokumentet beskrivs de tillståndsarbetsflöden som kan implementeras av en programmerares program på den översta nivån via de API:er som exponeras av Amazon FireOS AccessEnabler-biblioteket.

Adobe Pass Authentication-berättigandelösningen för Amazon FireOS är uppdelad i två domäner:

- Användargränssnittets domän - det här är det övre programlagret som implementerar användargränssnittet och använder tjänsterna som tillhandahålls av AccessEnabler-biblioteket för att ge åtkomst till begränsat innehåll.
- Domänen AccessEnabler - det är här som berättigandearbetsflödena implementeras i form av:
   - Nätverksanrop gjorda till Adobe serverdel
   - Affärslogik för arbetsflödena för autentisering och auktorisering
   - Hantering av olika resurser och bearbetning av arbetsflödestillstånd (t.ex. tokencache)

Målet med AccessEnabler-domänen är att dölja alla komplexa berättigandearbetsflöden och ge programmet i det övre lagret (via AccessEnabler-biblioteket) en uppsättning enkla berättigandeprinciper som du använder för berättigandearbetsflöden:

1. Ange identitet för begärande
1. Kontrollera och hämta autentisering mot en viss identitetsleverantör
1. Kontrollera och få behörighet för en viss resurs
1. Utloggning

Nätverksaktiviteten för AccessEnabler sker i en annan tråd så att gränssnittstråden aldrig blockeras. Det innebär att den tvåvägskommunikationskanal som finns mellan de två programdomänerna måste följa ett fullständigt asynkront mönster:

- Gränssnittets programlager skickar meddelanden till AccessEnabler-domänen via API-anropen som visas av AccessEnabler-biblioteket.
- AccessEnabler svarar på UI-lagret via callback-metoderna i protokollet AccessEnabler som UI-lagret registreras med AccessEnabler-biblioteket.

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

      - Utlöses av `setRequestor()`, returnerar om åtgärden lyckades eller misslyckades.     Slutfört visar att du kan fortsätta med berättigandeanrop.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Utlöses av `getAuthentication()` endast om användaren inte har valt en leverantör (MVPD) och ännu inte är autentiserad. The `mvpds` är en array med providers som är tillgängliga för användaren.

   - [`setAuthenticationStatus(status, orsak)`](#$setAuthNStatus)

      - Utlöses av `checkAuthentication()` varje gång. Utlöses av `getAuthentication()` endast om användaren redan är autentiserad och har valt en leverantör.

      - Status som returneras är autentiserad eller inte autentiserad. Orsaken beskriver ett autentiseringsfel eller en utloggningsåtgärd.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Metoden ignoreras i AmazonFireOS SDK och används på Android-plattformar där den aktiveras av `getAuthentication()` efter att användaren har valt ett MVPD.  The `url` parameter anger platsen för MVPD:s inloggningssida.

   - [`sendTrackingData(event, data)`](#$sendTrackingData)

      - Utlöses av `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
The `event` parametern anger vilken berättigandehändelse som har inträffat; `data` parameter är en lista med värden som relaterar till händelsen.

   - [`setToken(token, resource)`](#$setToken)

      - Utlöses av `checkAuthorization()` och `getAuthorization()` efter en auktorisering för att visa en resurs.
      - The `token` parametern är den kortlivade medietoken, `resource` -parametern är det innehåll som användaren har behörighet att visa.

   - [`tokenRequestFailed(resource, code, description)`](#$tokenRequestFailed)

      - Utlöses av `checkAuthorization()` och `getAuthorization()` efter en misslyckad auktorisering.
      - The `resource` parametern är det innehåll som användaren försöker visa, `code` parametern är felkoden som anger vilken typ av fel som inträffat. `description` -parametern beskriver felet som är associerat med felkoden.

   - [`selectedProvider(mvpd)`](#$selectedProvider)

      - Utlöses av `getSelectedProvider()`.
      - The `mvpd` -parametern ger information om den leverantör som användaren har valt.

   - [`setMetadataStatus(metadata, nyckel, argument)`](#$setMetadataStatus)

      - Utlöses av `getMetadata().`
      - The `metadata` parametern innehåller de specifika data som du har begärt; `key` parametern är nyckeln som används i `getMetadata()` begäran, och `arguments` parametern är samma ordlista som skickas till `getMetadata()`.

   - [&quot;preauthorizedResources(resources)`](#$preauthResources)

      - Utlöses av `checkPreauthorizedResources()`.
      - The `authorizedResources` -parametern visar de resurser som användaren har behörighet att visa.


![](assets/android-entitlement-flows.png)


### B. Startflöde {#startup_flow}

1. Starta programmet på den översta nivån.
1. Initiera Adobe Pass-autentisering.

   1. Utlysning [`getInstance`](#$getInstance) för att skapa en instans av Adobe Pass Authentication AccessEnabler.

      - **Beroende:** Adobe Pass Authentication Native Amazon FireOS Library (AccessEnabler)

   1. Utlysning` setRequestor()` fastställa programmerarens identitet, skicka in programmerarens `requestorID` och (valfritt) en array med slutpunkter för Adobe Pass-autentisering.

      - **Beroende:** Giltigt Adobe Pass Authentication RequestorID (Använd din kontohanterare för Adobe Pass-autentisering för att ordna detta.)

      - **Utlösare:** setRequestorComplete(), återanrop

   Inga berättigandebegäranden kan slutföras förrän identiteten för den som gjorde begäran har etablerats fullständigt. Detta innebär att när setRequestor() fortfarande körs, kommer alla efterföljande berättigandebegäranden (till exempel`checkAuthentication()`) är blockerade.

   Du har två implementeringsalternativ: När begärarens identifieringsinformation skickas till backend-servern kan gränssnittets programlager välja en av följande två metoder:</p>

   1. Vänta på att utlösaren för `setRequestorComplete()` callback (ingår i AccessEnabler-delegaten).  Det här alternativet ger den största säkerheten för att `setRequestor()` slutförd, så det rekommenderas för de flesta implementeringar.
   1. Fortsätt utan att vänta på att utlösaren för `setRequestorComplete()` återanrop och börja utfärda tillståndsansökningar. Dessa anrop (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) köas av AccessEnabler-biblioteket, som gör att de faktiska nätverksanropen görs efter `setRequestor()`. Det här alternativet kan ibland avbrytas om nätverksanslutningen till exempel är instabil.

1. Utlysning [checkAuthentication()](#$checkAuthN) för att kontrollera om det finns en befintlig autentisering utan att initiera det fullständiga autentiseringsflödet.  Om samtalet lyckas kan du gå direkt till auktoriseringsflödet.  Om inte, fortsätter du till autentiseringsflödet.

- **Beroende:** Ett samtal till `setRequestor()` (detta beroende gäller även för alla efterföljande anrop).

- **Utlösare:** setAuthenticationStatus(), återanrop

### C. Autentiseringsflöde {#authn_flow}

1. Utlysning [`getAuthentication()`](#$getAuthN) för att initiera autentiseringsflödet eller för att få en bekräftelse på att användaren redan är autentiserad.

   **Utlösare:**

   - Callback-funktionen setAuthenticationStatus(), om användaren redan är autentiserad.  I det här fallet går du direkt till [Auktoriseringsflöde](#authz_flow).
   - Callback-funktionen displayProviderDialog(), om användaren inte har autentiserats ännu.

1. Presentera användaren med listan över leverantörer som skickats till `displayProviderDialog()`.
1. När användaren har valt en leverantör öppnas providersidan så att användaren kan logga in.

   >[!NOTE]
   >
   >Nu har användaren möjlighet att avbryta autentiseringsflödet. Om detta inträffar rensar AccessEnabler det interna tillståndet och återställer autentiseringsflödet.

1. När användaren har loggat in stängs WebView.
1. ring `getAuthenticationToken(),` som instruerar AccessEnabler att hämta autentiseringstoken från backend-servern.
1. [Valfritt] Utlysning [`checkPreauthorizedResources(resources)`](#$checkPreauth) för att kontrollera vilka resurser användaren har behörighet att visa. The `resources` parametern är en array med skyddade resurser som är associerad med användarens autentiseringstoken.

   **Utlösare:** `preAuthorizedResources()` callback\
   **Körningspunkt:** Efter det slutförda autentiseringsflödet

1. Om autentiseringen lyckades fortsätter du till auktoriseringsflödet.


### D. Auktoriseringsflöde {#authz_flow}

1. Utlysning [`getAuthorization()`](#$getAuthZ) för att initiera auktoriseringsflödet.

   Beroende: Giltiga resurs-ID:n som har avtalats med MVPD:n.

   **Obs!** Resurs-ID:n ska vara samma som de som används på andra enheter eller plattformar och ska vara samma för alla programmeringsgränssnitten.

1. Validera autentisering och auktorisering.

   - Om `getAuthorization()` anropet lyckades: Användaren har giltiga AuthN- och AuthZ-tokens (användaren är autentiserad och har behörighet att titta på det begärda mediet).
   - If `getAuthorization()` misslyckas: Undersök undantaget som utlöses för att fastställa dess typ (AuthN, AuthZ eller något annat):
      - Om det var ett autentiseringsfel (AuthN) startar du om autentiseringsflödet.
      - Om det var ett auktoriseringsfel (AuthZ) har användaren inte behörighet att titta på det begärda mediet och någon typ av felmeddelande ska visas för användaren.
      - Om det finns någon annan typ av fel (anslutningsfel, nätverksfel osv.) visar sedan ett felmeddelande för användaren.

1. Validera den korta medietoken.

   Använd Adobe Pass Authentication Media Token Verifier-biblioteket för att verifiera den kortlivade medietoken som returneras från `getAuthorization()` ring ovan:

   - Om valideringen lyckas: Spela upp det begärda mediet för användaren.
   - Om valideringen misslyckas: AuthZ-token var ogiltig, ska mediebegäran avvisas och ett felmeddelande ska visas för användaren.

1. Återgå till det normala programflödet.

### E. Visa medieflöde {#media_flow}

1. Användaren väljer de media som ska visas.
1. Är mediet skyddat?  Programmet kontrollerar om det valda mediet är skyddat:
   - Om det valda mediet är skyddat startar programmet [Auktoriseringsflöde](#authz_flow) ovan.
   - Om det valda mediet inte är skyddat kan du spela upp mediet för användaren.

### F. Utloggningsflöde {#logout_flow}

1. Utlysning [`logout()`](#$logout) för att logga ut användaren. AccessEnabler tar bort alla cachelagrade värden och token som användaren fått för det aktuella MVPD-värdet för alla begärare som delar inloggningen via enkel inloggning. När cacheminnet har rensats gör AccessEnabler ett serveranrop för att rensa sessionerna på serversidan.  Observera, att eftersom serveranropet kan resultera i en SAML-omdirigering till IdP (detta tillåter sessionsrensning på IdP-sidan), måste det här anropet följa alla omdirigeringar. Därför hanteras det här anropet inuti en WebView-kontroll, som inte är synlig för användaren.

   **Obs!** Utloggningsflödet skiljer sig från autentiseringsflödet på så sätt att användaren inte behöver interagera med WebView på något sätt. Det är därför möjligt (och rekommenderas) att göra WebView-kontrollen osynlig (dvs. dold) under utloggningsprocessen.
