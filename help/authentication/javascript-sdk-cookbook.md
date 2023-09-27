---
title: JavaScript SDK Cookbook
description: JavaScript SDK Cookbook
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# JavaScript SDK Cookbook {#javascript-sdk-cookbook}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

I det här dokumentet beskrivs de tillståndsarbetsflöden som en programmers program implementerar för en JavaScript-integrering med Adobe Pass Authentication-tjänsten. Länkar till JavaScript API Reference ingår i hela dokumentet.

Observera även att [Relaterad information](#related) -avsnittet innehåller en länk till en uppsättning JavaScript-kodexempel.

## Tillståndsflöden {#entitlement}

1. [Förutsättningar](#prereq)
2. [Startflöde](#startup)
3. [Autentiseringsflöde](#authn)
4. [Auktoriseringsflöde](#authz)
5. [Visa medieflöde](#logout)

</br>

![](assets/javascript-flows.png)


## Förutsättningar {#prereq}

**Beroenden:**

- Adobe Pass Authentication Library (AccessEnabler) samarbetar med kontohanteraren för Adobe Pass-autentisering för att ordna detta.
- Giltigt begärande-ID för Adobe Pass-autentisering. Kontakta din kontohanterare för Adobe Pass-autentisering för att ordna detta.

Skapa callback-funktioner:

- `entitlementLoaded`
</br>

**Utlösare:** AccessEnabler har läst in och slutfört initieringen.

- `displayProviderDialog(mvpds)`

  **Utlösare:** `getAuthentication(),` bara om användaren inte har valt en leverantör (ett MVPD) och ännu inte är autentiserad. Parametern mvpds är en matris med providers som är tillgängliga för användaren.

- `setAuthenticationStatus(status, errorcode)`

  **Utlösare:**
   - `checkAuthentication()`varje gång.
   - `getAuthentication()` endast om användaren redan är autentiserad och har valt en leverantör.

  Status som returneras är lyckad eller misslyckad. Felkoden beskriver typen av fel.

- `createIFrame(width, height)`

  **Utlösare:** `setSelectedProvider(providerID)`bara om den valda providern är konfigurerad att visas i en IFrame.

  >[!NOTE]
  >
  >En provider är konfigurerad att återge sin autentiseringsskärm som antingen en omdirigering eller i en iFrame, och programmeraren måste ta hänsyn till båda.

- `sendTrackingData(event, data)`

  **Utlösare:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  The `event` parametern anger vilken berättigandehändelse som har inträffat, `data` parameter är en lista med värden som relaterar till händelsen.
- `setToken(token, resource)`
  **Utlösare:** `checkAuthorization()`och `getAuthorization()` efter en auktorisering för att visa en resurs.   The `token` parametern är den kortlivade medietoken, `resource` -parametern är det innehåll som användaren har behörighet att visa.

- `tokenRequestFailed(resource, code, description)`
  **Utlösare:**`checkAuthorization()` och`getAuthorization()`  efter en misslyckad auktorisering.\
  The `resource` parametern är det innehåll som användaren försöker visa, `code` parametern är felkoden som anger vilken typ av fel som inträffat. `description` -parametern beskriver felet som är associerat med felkoden.

- `selectedProvider(mvpd)`

  **Utlösare:** [`getSelectedProvider()`](#$getSelProv `mvpd` -parametern ger information om den leverantör som användaren har valt.

- `setMetadataStatus(metadata, key, arguments)`

  **Utlösare:** `getMetadata().`\
  The `metadata` parametern innehåller de specifika data som du har begärt. Nyckelparametern är nyckeln som används i `getMetadata()`begäran, och `arguments` parametern är samma ordlista som skickas till `getMetadata()`.


## 2. Startflöde

**I. Läs in AccessEnabler JavaScript:**

**För mellanlagringsprofil**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

eller...

**För produktionsprofil**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Utlösare:** När initieringen är klar anropar Adobe Pass-autentisering `entitlementLoaded()` callback-funktion. Det här är ingångspunkten för programmets kommunikation med AccessEnabler.


**II.** Utlysning `setRequestor()`fastställa programmerarens identitet, skicka in programmerarens `requestorID` och (valfritt) en array med slutpunkter för Adobe Pass-autentisering.

**Utlösare:** Ingen, men aktiverar `displayProviderDialog()` att anropas vid behov.


**III.** Utlysning `checkAuthentication()` för att kontrollera om det finns en befintlig autentisering utan att initiera hela [autentiseringsflöde].  Om samtalet lyckas kan du fortsätta direkt till `authorization flow`.  Om inte går du vidare till `authentication flow`.

**Beroende:** Ett samtal till `setRequestor()`(detta beroende gäller även för alla efterföljande anrop).

**Utlösare:** `setAuthenticationStatus()` callback

</br>

## 3. Autentiseringsflöde</span>


**Beroende:** Ett samtal till `setRequestor()`(detta beroende gäller även för alla efterföljande anrop).


Utlysning `getAuthentication()` för att hämta autentiseringsstatus ELLER för att utlösa leverantörens autentiseringsflöde.

**Utlösare:**

- `displayProviderDialog()`om användaren ännu inte har autentiserats
- `setAuthenticationStatus()` om autentisering redan har utförts

Autentiseringsflödet har slutförts när AccessEnabler anropar `setAuthenticationStatus()`med `isAuthenticated == 1`.

## 4. Auktoriseringsflöde {#authz}

**Beroenden:**

- Ett samtal till `setRequestor()` (detta beroende gäller även för alla efterföljande anrop).
- Giltiga resurs-ID:n som avtalats med MVPD:n. Observera att resurs-ID:n ska vara samma som de som används på andra enheter eller plattformar och ska vara samma för alla programmeringsgränssnitten.

Utlysning `getAuthorization()` och skicka ResourceID för det begärda mediet. Ett lyckat anrop returnerar en kort medietoken, som bekräftar att användaren har behörighet att visa det begärda mediet.

- Om anropet skickas: Användaren har en giltig AuthN-token och användaren har behörighet att titta på det begärda mediet.
- Om anropet misslyckas: Undersök undantaget som utlöses för att avgöra dess typ (AuthN, AuthZ eller något annat):
- Om anropet var ett AuthN-fel startar du om AuthN-flödet.
- Om anropet var ett AuthZ-fel har användaren inte behörighet att titta på det begärda mediet och någon typ av felmeddelande ska visas för användaren.
- Om det finns något annat fel (anslutningsfel, nätverksfel osv.) visar sedan ett felmeddelande för användaren.

Använd Media Token Verifier för att validera den shortMediaToken som returneras från en lyckad `getAuthorization()` ring.


**Beroende:** Short Media Token Verifier (ingår i AccessEnabler-biblioteket)

- Om valideringen godkänns: Visa/spela upp det begärda mediet för användaren.
- Om den misslyckas: AuthZ-token var ogiltig, ska mediebegäran avvisas och ett felmeddelande ska visas för användaren.

## 5. Visa medieflöde {#logout}

- Användaren väljer de media som ska visas.
   - Är media skyddade?
      - Din app kontrollerar om mediet är skyddat:
         - Om mediet är skyddat startar din app autentiseringsflödet (AuthZ) ovan.
         - Om mediet inte är skyddat fortsätter du med Visa media-flödet.
         - Uppspelningsmedia

## Konfigurera besökar-ID {#visitorID}

Konfigurera en [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) värdet är mycket viktigt ur analyssynpunkt. När ett EC-besökarID-värde har angetts skickar SDK den här informationen tillsammans med varje nätverksanrop och Adobe Pass Authentication-tjänsten samlar in den här informationen. På så sätt kan du korrelera analysdata från tjänsten Adobe Pass Authentication med andra analysrapporter som du kan ha från andra program eller webbplatser. Information om hur du konfigurerar EC visitorID finns [här](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Observera att detta funktionalitetsstöd är tillgängligt från och med JS SDK version 3.1.0.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
