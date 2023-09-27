---
title: Apple SSO Cookbook (iOS/tvOS SDK)
description: Apple SSO Cookbook (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Apple SSO Cookbook (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#Introduction}

Adobe Primetime Authentication AccessEnabler iOS/tvOS SDK har stöd för plattformsautentisering med enkel inloggning (SSO) för slutanvändare av klientprogram som körs på iOS, iPadOS eller tvOS via det vi kallar Apple SSO-arbetsflöde.

Observera att det här dokumentet fungerar som ett tillägg till den befintliga dokumentationen för AccessEnabler iOS/tvOS SDK, som du hittar [här](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Cookbook {#Cookbook}

För att dra nytta av Apple SSO-användarupplevelse måste ett program integrera AccessEnabler iOS/tvOS SDK och följa de tips som presenteras nedan.

</br>

### Förutsättningar {#Prerequisites}

</br>

#### Behörighet

>[!TIP]
>
> **<u>Pro Tip:</u>** För att få tillgång till användarens prenumerationsinformation måste användaren ge programmet behörighet att fortsätta, på samma sätt som att ge åtkomst till enhetens kamera eller mikrofon. Den här behörigheten måste begäras per program och enheten kommer att spara användarens val. Tänk på att användaren kan ändra sitt beslut genom att gå till programinställningarna (behörighet för tv-leverantör) eller till avsnittet från *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS.

>[!TIP]
>
> **<u>Pro Tip:</u>** Vi rekommenderar att du begär användarens tillstånd när programmet försätts i förgrundstillstånd, men det är bara ett förslag eftersom programmet kan söka efter [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation när som helst innan användarautentisering krävs. Dessutom kommer API:erna för AccessEnabler iOS/tvOS SDK automatiskt att begära användarens tillstånd när användaren behöver det.

>[!TIP]
>
> **<u>Pro Tip:</u>** Om användaren inte ger åtkomst till sin prenumerationsinformation eller om kommunikationen med Video Subscriber Account-ramverket misslyckas, kommer AccessEnabler iOS/tvOS SDK att återgå till det vanliga autentiseringsflödet.

>[!TIP]
>
> **<u>Pro Tip:</u>** Vi rekommenderar att man uppmuntrar användare som vägrar ge tillstånd att få tillgång till prenumerationsinformation genom att förklara fördelarna med SSO (Single Sign-On). Tänk på att användaren kan ändra sitt beslut genom att gå till programinställningarna (behörighet för tv-leverantör) eller till avsnittet från *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS.


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

</br>

#### Återanrop

>[!TIP]
>
> **<u>Pro Tip:</u>** Implementera följande lista med [återanrop](/help/authentication/iostvos-sdk-api-reference.md) som är specifika för Apple SSO-arbetsflöde.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Återanrop utlöses när Apple MVPD-väljaren ska öppnas.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Återanrop utlöses när Apple MVPD-väljaren stängs.

</br>

#### Felrapportering

>[!TIP]
>
> **<u>Pro Tip:</u>** Implementera följande lista med [avancerade felkoder](/help/authentication/error-reporting.md) som är specifika för Apple SSO-arbetsflöde.

- ***N003*** - Användaren valde alternativet &quot;Annan TV-leverantör&quot; i Apple MVPD-väljaren.
- ***N004*** - Användaren har valt en tv-leverantör i Apple MVPD-väljaren, vilket inte stöds (integrering eller enkel inloggning inaktiverad) av den aktuella begäraren.
- ***N005*** - Användaren bestämde sig för att avbryta den vanliga MVPD-väljaren eller Apple MVPD-väljaren.
- ***VSA403*** - Användarens TV-leverantörsbehörighet nekas för programmet.
- ***VSA404*** - Användarens TV-leverantörsbehörighet är inte fastställd för programmet.
- ***VSA503*** - Metadatabegäran för Video Subscriber Account misslyckades, mer kontext finns i *message* fält.
- ***AAPL / APPL_ERROR*** - Metadatabegäran för Video Subscriber Account misslyckades, mer kontext finns i *information* fält.

</br>

### Autentisering {#Authentication}

>[!TIP]
>
> **<u>Tips:</u>** Följ stegen nedan för implementering/implementering av iOS/iPadOS/tvOS.

1. Ansökan måste [initialize](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK.
1. Ansökan måste [ange den aktuella identifieraren för begärande](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Viktigt:** Detta andra steg kan utlösa en [avancerad felkod](/help/authentication/error-reporting.md) som är specifikt för Apple SSO-arbetsflöde, om **något av följande är sant**:

   - ***VSA403*** - Användarens TV-leverantörsbehörighet nekas för programmet.
   - ***VSA404*** - Användarens TV-leverantörsbehörighet är inte fastställd för programmet.
   - ***APPL*** - Ett fel uppstod i kommunikationen mellan AccessEnabler iOS/tvOS SDK och ramverket för videoprenumerantkontot.

   I det andra steget försöker du i tysthet byta ut Apple SSO-profilen mot en Adobe-autentiseringstoken, om **alla ovanstående är falska** och **alla följande är true**:

   - Användarens TV-leverantörsbehörighet beviljas för programmet.
   - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå.
   - AccessEnabler iOS/tvOS SDK tog emot användarens identifierare för tv-leverantör från ramverket för videoprenumerantkontot.
   - Användarens TV-leverantörsintegrering med programmet aktiveras via Adobe Primetime TVE Dashboard.
   - Användarens TV-leverantör enkel inloggning med programmet aktiveras via Adobe Primetime TV-instrumentpanelen.
   - Användarens TV-leverantör fungerar inte på Adobe Primetime TV Dashboard.
   - AccessEnabler iOS/tvOS SDK tog emot användarens SAML-svar från Video Subscriber Account Framework.

   **<u>Pro Tip:</u>** Det andra steget utlöser inga andra återanrop förutom [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) återanrop eftersom autentisering inte initierades explicit av programmet.

1. Ansökan måste [kontrollera autentiseringsstatus](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Viktigt:** Det tredje steget kan utlösa en [avancerad felkod](/help/authentication/error-reporting.md) som är specifikt för Apple SSO-arbetsflöde, om **något av följande är sant**:

   - ***VSA403** - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantörsbehörighet nekas för programmet.
   - ***VSA404** - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantörsbehörighet är inte fastställd för programmet.
   - ***APPL\_ERROR** - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men ett fel uppstod i kommunikationen mellan AccessEnabler iOS/tvOS SDK och ramverket för videoprenumerantkontot.

   **Viktigt:** Detta tredje steg kommer att aktivera [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback med *status* lika med 0, om **något av följande är sant**:

   - Användaren är inte inloggad på sitt TV-leverantörskonto på enhetssystemnivå eller via ett regelbundet autentiseringsflöde.
   - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå eller via det reguljära autentiseringsflödet, men användarens TV-leverantörs autentiseringstoken TTL har passerat.
   - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå eller via ett regelbundet autentiseringsflöde, men användarens TV-leverantörsintegrering med programmet inaktiveras via Adobe Primetime TV-instrumentpanel.
   - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantör enkel inloggning med programmet är inaktiverad via Adobe Primetime TV Dashboard.
   - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantörsbehörighet nekas för programmet.
   - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantörsbehörighet är inte fastställd för programmet.
   - Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men ett fel uppstod i kommunikationen mellan AccessEnabler iOS/tvOS SDK och ramverket för videoprenumerantkontot.

   **Viktigt:** Detta tredje steg kommer att aktivera [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback med *status* lika med 1, om **alla ovanstående är falska.**


1. Ansökan måste [initiera autentiseringen](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) om föregående kontroll av autentiseringsstatus utlöste [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) callback med *status* lika med 0.

   **<u>Pro Tip:</u>** Implementera ett av följande AccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) eller [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Viktigt:** Detta fjärde steg skulle kunna utlösa en [avancerad felkod](/help/authentication/error-reporting.md) som är specifikt för Apple SSO-arbetsflöde, om **något av följande är sant**:

   - ***VSA403*** - Användarens TV-leverantörsbehörighet nekas för programmet.
   - ***VSA404*** - Användarens TV-leverantörsbehörighet är inte fastställd för programmet.
   - ***VSA503*** - Ett fel uppstod i kommunikationen mellan AccessEnabler iOS/tvOS SDK och ramverket för videoprenumerantkontot.
   - ***N003*** - Användaren valde alternativet &quot;Annan TV-leverantör&quot; i Apple MVPD-väljaren.
   - ***N004*** - Användaren har valt en tv-leverantör i Apple MVPD-väljaren, vilket inte stöds (integrering eller enkel inloggning inaktiverad) av den aktuella begäraren.
   - ***N005*** - Användaren bestämde sig för att avbryta den vanliga MVPD-väljaren eller Apple MVPD-väljaren.

   **Viktigt:** Detta fjärde steg återgår till det reguljära autentiseringsflödet genom att aktivera [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) callback och **en** av ovanstående [avancerade felkoder](/help/authentication/error-reporting.md), om **en av ovanstående är sann**.

   **Viktigt:** Detta fjärde steg återgår till det reguljära autentiseringsflödet genom att aktivera [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) eller [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) callback och **ingen** av ovanstående [avancerade felkoder](/help/authentication/error-reporting.md), om användaren har valt en tv-leverantör som inte har stöd för Apple SSO, men som finns i Apple MVPD-väljaren.

   **<u>Pro Tip:</u>** AccessEnabler iOS/tvOS SDK anropar tyst [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API, om användaren har valt en tv-leverantör som inte har stöd för Apple SSO, men som finns i Apple MVPD-väljaren.

   **Viktigt:** Detta fjärde steg skulle försöka att tyst byta ut Apple SSO-profilen mot en Adobe-autentiseringstoken om det skulle hända **alla ovanstående är falska** och **alla följande är true**:

   - Användarens TV-leverantörsbehörighet beviljas för programmet.
   - Användaren är inloggad/loggar för närvarande in på sitt TV-leverantörskonto på enhetssystemnivå.
   - AccessEnabler iOS/tvOS SDK tog emot användarens identifierare för tv-leverantör från ramverket för videoprenumerantkontot.
   - Användarens TV-leverantörsintegrering med programmet aktiveras via Adobe Primetime TVE Dashboard.
   - Användarens TV-leverantör enkel inloggning med programmet aktiveras via Adobe Primetime TV-instrumentpanelen.
   - Användarens TV-leverantör fungerar inte på Adobe Primetime TV Dashboard.
   - AccessEnabler iOS/tvOS SDK tog emot användarens SAML-svar från Video Subscriber Account Framework.



>**<u>Pro Tip:</u>** Detta fjärde steg kommer att utlösa [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) återanrop, oavsett *status* eftersom autentisering initierades explicit av programmet.


</br>

### Metadata {#Metadata}

Programmet har möjlighet att avgöra om autentiseringen har skett som ett resultat av en inloggning via plattformens SSO eller inte, med hjälp av &quot;*tokenSource&quot;* [användar-metadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) API från AccessEnabler iOS/tvOS SDK.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Utloggning {#Logout}

The [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) ramverket innehåller inte något API för att programmässigt logga ut personer som har loggat in på sitt TV-leverantörskonto på enhetssystemnivå. För att utloggningen ska få full effekt måste slutanvändaren därför uttryckligen logga ut från *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS. Det andra alternativet som användaren skulle ha möjlighet att återkalla behörigheten att få åtkomst till användarens prenumerationsinformation från det specifika avsnittet för programinställningar (TV-leverantörens behörighetsåtkomst).

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via AccessEnabler iOS/tvOS SDK [utloggning](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementeringen/implementeringarna av tvOS.

- Ansökan måste [initiera utloggningen](/help/authentication/iostvos-sdk-api-reference.md#logout) från AccessEnabler iOS/tvOS SDK. Detta skulle inte underlätta sessionsrensning på MVPD-sidan.
- Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> Accounts -> TV Provider`* på tvOS endast om [*VSA203* statuskoden har utlösts](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementering/implementering av iOS/iPadOS.

- Ansökan måste [initiera utloggningen](/help/authentication/iostvos-sdk-api-reference.md#logout) från AccessEnabler iOS/tvOS SDK. Detta underlättar sessionssanering på den mobila dokumentationssidan.
- Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> TV Provider`* på iOS/iPadOS endast om [*VSA203* statuskoden har utlösts](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
