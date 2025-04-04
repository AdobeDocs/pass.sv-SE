---
title: REST API Cookbook (klient-till-server)
description: Återställ API-cookbook-klienten till servern.
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# (Äldre) REST API Cookbook (klient-till-server) {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

## Ökning {#overview}

Det här dokumentet innehåller stegvisa instruktioner för programmerarens tekniker att integrera en&quot;smart enhet&quot; (spelkonsol, smart TV-app, digitalbox osv.) med Adobe Pass Authentication med REST API-tjänster. Denna klient-till-server-metod, som använder REST-API:er i stället för en klient-SDK, ger bredare stöd för olika plattformar där det inte skulle vara möjligt att utveckla ett stort antal unika SDK:er. En utförlig teknisk översikt över hur den klientlösa lösningen fungerar finns i [Översikt över klientlös teknik](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md).


Strategin kräver två komponenter (direktuppspelningsapp och AuthN-app) för att slutföra de nödvändiga flödena: start-, registrerings-, auktoriserings- och vymedieflöden i direktuppspelningsappen och autentiseringsflödet i din AuthN-app.

### Begränsningsmekanism

REST-API:t för Adobe Pass-autentisering styrs av en [begränsningsmekanism](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Komponenter {#components}

I en fungerande klient-till-server-lösning ingår följande komponenter:



| Typ | Komponent | Beskrivning |
| --- | --- | --- |
| Strömmande enhet | Strömmande app | Programmeringsprogrammet som finns på användarens direktuppspelningsenhet och spelar upp autentiserad video. |
| | \[Valfritt\] AuthN-modul | Om direktuppspelningsenheten har en användaragent (t.ex. en webbläsare) ansvarar AuthN-modulen för att autentisera användaren på MVPD IdP. |
| \[Valfritt\] AuthN-enhet | AuthN-app | Om direktuppspelningsenheten inte har någon användaragent (t.ex. webbläsare) är AuthN-programmet ett webbprogram för programmerare som nås från en separat användares enhet via en webbläsare. |
| Adobe Infrastructure | Adobe Pass Service | En tjänst som integreras med MVPD IdP- och AuthZ-tjänsten och som ger autentiserings- och auktoriseringsbeslut. |
| MVPD Infrastructure | MVPD IdP | En MVPD-slutpunkt som tillhandahåller autentiseringsbaserad autentisering för att validera användarens identitet. |
| | MVPD AuthZ-tjänst | En MVPD-slutpunkt som ger auktoriseringsbeslut baserat på användarens prenumerationer, föräldrakontroll osv. |

## Flöden{#flows}

### Dynamic Client Registration (DCR)

Adobe Pass använder DCR för att säkra klientkommunikationen mellan ett programmeringsprogram eller en server och Adobe Pass tjänster. DCR-flödet är separat och beskrivs i [Översikt över registrering av dynamiska klienter](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) -dokumentationen.


### Appflöden för direktuppspelning (smart enhet)

![](../../../../assets/smart-device-app-flow.png)

#### Startflöde

1. Ditt program startas och det inledande användargränssnittet läses in.

2. Hämta/generera ett enhets-ID.

3. Utfärda ett Check-authentication-anrop för att se om enheten redan är autentiserad.  Till exempel: [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

4. Om anropet `checkauthn` lyckas fortsätter du till auktoriseringsflödet från steg 2 och framåt.  Starta registreringsflödet om det inte fungerar.



#### Registreringsflöde

1. Skaffa en registreringskod och URL som användaren kan använda för att få åtkomst till inloggningsappen för den andra skärmen och visa dessa för användaren:

   a. Skicka en POST-begäran till Adobe Registration Code Service och skicka ett hash-kodat enhets-ID och en &quot;Registration URL&quot;.  Till exempel: [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)

   b. Ange den returnerade registreringskoden och URL-adressen till användaren.

   c. Instruera användaren att växla till en webbkompatibel enhet, navigera till URL:en och ange sedan registreringskoden.



#### Auktoriseringsflöde

1. Användaren återgår från den andra skärmappen och trycker på knappen &quot;Fortsätt&quot; på enheten. Du kan också implementera en avsökningsmekanism för att kontrollera autentiseringsstatusen, men Adobe Pass Authentication rekommenderar att du använder knappmetoden Fortsätt framför avsökningen. <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)--> Exempel: [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)

2. Skicka en GET-begäran till auktoriseringstjänsten för Adobe Pass-autentisering för att initiera auktorisering. Till exempel: `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* Om svaret anger att åtgärden lyckades: Användaren har en giltig AuthN-token OCH användaren har behörighet att titta på det begärda mediet (det finns en giltig AuthZ-token för den här användaren).

* Om svaret indikerar ett fel: Undersök det undantag som genereras för att avgöra dess typ (AuthN, AuthZ eller något annat):

   * Om det var ett AuthN-fel startar du om registreringsflödet.

   * Om det var ett AuthZ-fel har användaren inte behörighet att titta på det begärda mediet och någon typ av felmeddelande ska visas för användaren.

   * Om något annat fel uppstod (anslutningsfel, nätverksfel osv.) visas ett felmeddelande för användaren.



#### Visa medieflöde

1. Presentera mediealternativ. Användaren väljer de media som ska visas.

2. Är mediet skyddat?

   a. Din app kontrollerar om mediet är skyddat.

   b. Om mediet är skyddat startar din app auktoriseringen
(AuthZ) Flöde ovanför.

   c. Om mediet inte är skyddat kan du spela upp mediet för
användare.

3. Spela upp mediet.


### Appflöde för AuthN (andra skärmen)

![](../../../../assets/secnd-screen-authn-flow.png)

1. Hämta en lista över MVPD för den här användaren. Till exempel: [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)

1. Initiera autentiseringsflödet.  Till exempel: [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)

1. Kontrollera om autentiseringen lyckades. Till exempel:[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

1. Skicka tillbaka användaren till din Smart Device-app för att slutföra auktoriseringsflödet.

## Enkel inloggning för partner {#partner-sso}

Vissa enheter har dedikerat stöd för enkel inloggning (SSO) för partner:

* [APPLE SSO](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)

## Plattform för enkel inloggning {#platform-sso}

Vissa enheter har dedikerat stöd för enkel inloggning (SSO) på plattformen:

* [AMAZON SSO](../../sso-access/amazon-sso-cookbook-rest-api-v1.md)

## TempPass och Promotional TempPass för REST API {#temppass}

För TempPass- och Promotional TempPass-implementeringar där användaren inte behöver ange inloggningsuppgifter kan autentisering implementeras direkt i Streaming App.

**Om du vill använda det här API:t måste direktuppspelningsappen se till att enhets-ID är unikt eftersom det används för att identifiera token, tillsammans med valfria extra data.**


![](../../../../assets/temp-pass-promo-temppass.png)
