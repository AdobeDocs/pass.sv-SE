---
title: Apple SSO - översikt
description: Apple SSO - översikt
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1256'
ht-degree: 0%

---

# Apple SSO - översikt {#apple-sso-overview}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Apple ger användarna möjlighet att logga in på sitt TV-leverantörskonto på enhetssystemnivå, vilket eliminerar behovet av att autentisera appvis.

Adobe Pass Authentication samarbetade med Apple för att skapa användarupplevelsen för Partner Single Sign-On (SSO) i TV Everywhere-ekosystemet för iPhone-, iPad- och Apple TV-ägare.

För att kunna dra nytta av SSO-användarupplevelsen (Single Sign-On) på en Apple-enhet finns det en lista med krav som beskrivs nedan och som måste fyllas i.

Slutresultatet bör skapa en upplevelse som överensstämmer med följande användarflöden, som vi rekommenderar att du undersöker innan du börjar utveckla programmet:

* enkel inloggning (SSO) [användarflöden för iPhone- och iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)-enheter.
* enkel inloggning (SSO) [användarflöden för Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)-enheter.

## Förutsättningar {#apple-sso-prerequisites}

Krav för introduktion kan gälla för en eller flera enheter som deltar i TVE-verksamheten, till exempel programmerare, distributörer av videoprogrammeringstjänster, Adobe Pass-autentisering eller Apple.

### Programmer {#apple-sso-prerequisites-programmer}

För att kunna dra nytta av SSO-användarupplevelsen (Single Sign-On) måste en programmerare

* Kontakta Apple för att aktivera [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) som en del av ditt Apple Team ID och konfigurera [Video Subscriber Single Sign-On Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) som en del av ditt Apple Developer Account.

   * Använd Xcode version 8 eller senare och iOS/tvOS version 10 eller senare.

* Aktivera enkel inloggning (SSO) för varje önskad integrering och plattform (iOS/tvOS) via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) genom att ange egenskapen `Enable Single Sign On` till `Yes`.

| Aktivera enkel inloggning i Adobe | Apple **On-board (stöds)** MVPD-filer | Apple **Picker** MVPD-filer | Apple **Inte integrerat (stöds inte)** MVPD-program |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Ja (aktiverad) | Autentiserings- och utloggningsflödena omfattar både Apple- och Adobe Pass-autentiseringslösningar, medan alla andra flöden (auktorisering, förauktorisering, metadata osv.) endast hanteras av Adobe Pass Authentication. | Autentiserings- och utloggningsflödena återgår till de vanliga flöden som enbart hanteras av Adobe Pass Authentication. | Autentiserings- och utloggningsflödena återgår till de vanliga flöden som enbart hanteras av Adobe Pass Authentication. |
| Nej (inaktiverat) | Autentiserings- och utloggningsflödena återgår till de vanliga flöden som enbart hanteras av Adobe Pass Authentication. | Autentiserings- och utloggningsflödena återgår till de vanliga flöden som enbart hanteras av Adobe Pass Authentication. | Autentiserings- och utloggningsflödena återgår till de vanliga flöden som enbart hanteras av Adobe Pass Authentication. |

* Integrera användarflödena för enkel inloggning (SSO) med någon av följande lösningar som Adobe Pass Authentication erbjuder för slutanvändare av klientprogram som körs på iOS, iPadOS eller tvOS.

   * Adobe Pass Authentication REST API V2 har stöd för enkel inloggning (SSO) för partner.

     Läs [dokumentationen för Apple SSO Cookbook (REST API V2)](apple-sso-cookbook-rest-api-v2.md).

   * Adobe Pass Authentication REST API V1 har stöd för enkel inloggning (SSO) för partner.

     Mer information finns i [Apple SSO Cookbook (REST API V1)](apple-sso-cookbook-rest-api-v1.md) -dokumentationen.

   * Adobe Pass Authentication AccessEnabler iOS/tvOS SDK har stöd för enkel inloggning (SSO) för partner.

     Mer information finns i [Apple SSO Cookbook (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md) -dokumentationen.

### MVPD {#apple-sso-prerequisites-mvpd}

För att kunna dra nytta av SSO-användarupplevelsen (Single Sign-On) måste ett enda MVPD:

* Kontakta Apple för att starta introduktionsprocessen på Apple sida.

   * Begär teknisk dokumentation om hur man integrerar och utvecklar en JavaScript TVML-applikation som kan hantera användarens inloggningsformulär.

* Kontakta Adobe Pass Authentication för att initiera introduktionsprocessen på Adobe.

   * Ange strängvärdet som representerar den identifierare för TV-leverantör som tilldelats av Apple under introduktionsprocessen.

## Vanliga frågor {#FAQ}

* Om något går fel i arbetsflödet för Apple SSO, kan programmet som använder Adobe Pass Authentication AccessEnabler iOS/tvOS SDK återställas till det vanliga autentiseringsflödet?

  Detta är möjligt men kräver att en konfigurationsändring utförs via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) för att ställa in **Aktivera enkel inloggning** på **NO** för den önskade integrationen och plattformen (iOS/tvOS). Observera att klientprogrammet bara kommer att bekräfta konfigurationsändringen efter att API:t [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) har anropats.


* Vet programmet när en autentisering har skett som ett resultat av en inloggning via Apple SSO?

  Den här informationen är tillgänglig som en del av användarens metadatanyckel: *tokenSource*, som bör returnera strängvärdet: &quot;Apple&quot; i det här fallet.


* Vet programmet när en autentisering har skett som ett resultat av en inloggning via Apple SSO i ett annat program?

  Informationen är inte tillgänglig.


* Vad händer om en användare loggar in genom att gå till *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS med ett MVPD som inte är integrerat med programmet?

  När användaren startar programmet autentiseras inte användaren via arbetsflödet för enkel inloggning i Apple. Därför måste programmet återgå till det regelbundna autentiseringsflödet och presentera en egen MVPD-väljare.


* Vad händer om en användare loggar in genom att gå till *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS med ett MVPD som har **Aktivera enkel inloggning** inställt på **NO** via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) för iOS/tvOS-plattformen?

  När användaren startar programmet autentiseras inte användaren via arbetsflödet för enkel inloggning i Apple. Därför måste programmet återgå till det regelbundna autentiseringsflödet och presentera en egen MVPD-väljare.


* Vad händer om en användare har ett MVPD-program som inte har introducerats (stöds inte) av Apple, men som finns i Apple-väljaren?

  När användaren startar programmet väljer användaren bara MVPD via Apple SSO-arbetsflöde utan att slutföra autentiseringsflödet. Därför måste programmet återgå till det reguljära autentiseringsflödet, men kan använda det redan valda MVPD.


* Vad händer om en användare har ett MVPD som inte har anammats (stöds inte) av Apple?

  När användaren startar programmet väljer användaren alternativet &quot;Andra TV-leverantörer&quot; via arbetsflödet för enkel inloggning i Apple. Därför måste programmet återgå till det regelbundna autentiseringsflödet och presentera en egen MVPD-väljare.


* Vad händer om en användare har ett MVPD som bryts ned via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)?

  När användaren startar programmet autentiseras användaren via nedbrytningsmekanismen och inte via Apple SSO-arbetsflöde. Användarupplevelsen bör vara sömlös, medan programmet informeras via varningskoden *N010* om det använder Adobe Pass Authentication AccessEnabler iOS/tvOS SDK.


* Kommer MVPD-användar-ID att ändras mellan Apple SSO och SSO-autentiseringsflöden som inte kommer från Apple?

  Förväntningen är att användar-ID inte ändras, men måste verifieras för varje vald leverantör.


* Kommer autentiserings-TTL att ändras?

  Adobe Pass Authentication fortsätter att respektera de TTL:er som krävs av programmerarna för deras integrering med varje MVPD. När du navigerar från ett programmeringsprogram till ett annat programmeringsprogram via Apple SSO, kommer det andra programmet att ha en TTL för motsvarande programmeringsprogram x MVPD-integrering (det delar inte TTL-värdet för det första programmet som autentiseras)

|                                      | Adobe Pass-autentiserings-TTL har gått ut | Adobe Pass Authentication TTL valid |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Token för Apple-enhet har upphört att gälla** | användaren är INTE autentiserad (MVPD-väljaren ska visas) | användaren är autentiserad och TTL är den återstående tiden för denna Adobe Pass Authentication-token/profil |
| **Apple enhetstoken TTL giltig** | användaren är tyst autentiserad och hämtar en annan Adobe Pass-autentiseringstoken/-profil med den TTL som anges i TVE Dashboard | användaren är autentiserad och TTL är den återstående tiden för denna Adobe Pass Authentication-token/profil |
