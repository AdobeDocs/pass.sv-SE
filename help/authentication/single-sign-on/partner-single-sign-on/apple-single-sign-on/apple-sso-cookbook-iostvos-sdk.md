---
title: Apple SSO Cookbook (iOS/tvOS SDK)
description: Apple SSO Cookbook (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1811'
ht-degree: 0%

---

# Apple SSO Cookbook (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Adobe Pass Authentication AccessEnabler iOS/tvOS SDK har stöd för Partner Single Sign-On (SSO) för slutanvändare av klientprogram som körs på iOS, iPadOS eller tvOS.

Det här dokumentet fungerar som ett tillägg till den befintliga dokumentationen för AccessEnabler iOS/tvOS SDK, som finns [här](/help/authentication/iostvos-sdk-api-reference.md).

## Cookbook {#apple-sso-cookbook-iostvos-sdk-cookbook}

För att dra nytta av Apple SSO-användarupplevelse måste programmet integrera iOS/tvOS SDK för AccessEnabler och följa stegen nedan.

### Förutsättningar {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Behörighet {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>Pro Tips!</u>** Strömningsprogrammet måste begära åtkomst till användarens prenumerationsinformation som har sparats på enhetsnivå, och användaren måste ge programmet behörighet att fortsätta, på samma sätt som att ge åtkomst till enhetens kamera eller mikrofon. Den här behörigheten måste begäras per program med Apple [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) och enheten sparar användarens val.

>[!TIP]
>
> **<u>Pro Tips!</u>** Vi rekommenderar att du uppmuntrar användare som vägrar ge åtkomst till prenumerationsinformation genom att förklara fördelarna med Apple användarupplevelse med enkel inloggning, men tänk på att användaren kan ändra sitt beslut genom att gå till programinställningarna (åtkomst till tv-provider) eller till *`Settings -> TV Provider`* på iOS och iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS.

>[!TIP]
>
> **<u>Pro Tips:</u>** Strömningsprogrammet kan begära användarens tillstånd när programmet försätts i förgrundstillstånd, eftersom programmet kan kontrollera om användaren har [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation när som helst innan användarautentisering krävs.

>[!TIP]
>
> **<u>Pro Tip:</u>** Om användaren inte ger åtkomst till sin prenumerationsinformation eller om kommunikationen med Video Subscriber Account Framework misslyckas, kommer AccessEnabler iOS/tvOS SDK att återgå till det vanliga autentiseringsflödet.

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

#### Återanrop {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>Pro Tip:</u>** Implementera följande lista över [återanrop](/help/authentication/iostvos-sdk-api-reference.md) som är specifika för Apple SSO-arbetsflöde.

* [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Återanropet utlöses när Apple MVPD-väljaren ska öppnas.
* [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Återanropet utlöses när Apple MVPD-väljaren kommer att stängas.

#### Felrapportering {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>Pro Tips:</u>** Implementera följande lista med [avancerade felkoder](/help/authentication/error-reporting.md) som är specifika för arbetsflödet för Apple SSO.

* ***N003*** - Användaren valde alternativet &quot;Annan TV-leverantör&quot; i Apple MVPD-väljaren.
* ***N004*** - Användaren valde en TV-leverantör i Apple MVPD-väljaren, vilket inte stöds (integrering eller enkel inloggning inaktiverad) av den aktuella begäraren.
* ***N005*** - Användaren bestämde sig för att avbryta den vanliga MVPD-väljaren eller Apple MVPD-väljaren.
* ***VSA403*** - Användarens TV-leverantörsbehörighet nekas för programmet.
* ***VSA404*** - Användarens TV-leverantörsbehörighet är inte definierad för programmet.
* ***VSA503*** - Metadatabegäran för videoprenumerantkontot misslyckades. Mer kontext anges i fältet *message*.
* ***AAPL/APPL_ERROR*** - Metadatabegäran för videoprenumerantkontot misslyckades, mer kontext anges i fältet *details*.

### Autentisering {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Tips!</u>** Följ stegen nedan för implementeringen/implementeringen/implementeringarna av iOS/iPadOS/tvOS.

1. Programmet måste [initiera](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK.


1. Programmet måste [ange den aktuella begärande-identifieraren](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Viktigt!** Det här andra steget kan utlösa en [avancerad felkod](/help/authentication/error-reporting.md) som är specifik för Apple SSO-arbetsflöde, om **något av följande är sant**:

   * ***VSA403*** - Användarens TV-leverantörsbehörighet nekas för programmet.
   * ***VSA404*** - Användarens TV-leverantörsbehörighet är inte definierad för programmet.
   * ***APPL*** - Ett fel uppstod i kommunikationen mellan AccessEnabler iOS/tvOS SDK och Video Subscriber Account Framework.

   Det andra steget skulle försöka att tyst byta ut Apple SSO-profilen mot en Adobe-autentiseringstoken, om **alla ovanstående är falska** och **alla följande är sanna**:

   * Användarens TV-leverantörsbehörighet beviljas för programmet.
   * Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå.
   * AccessEnabler iOS/tvOS SDK tog emot användarens identifierare för TV-leverantör från Video Subscriber Account Framework.
   * Användarens TV-leverantörsintegrering med programmet aktiveras via Adobe Primetime TVE Dashboard.
   * Användarens TV-leverantör enkel inloggning med programmet aktiveras via Adobe Primetime TV-instrumentpanelen.
   * Användarens TV-leverantör fungerar inte på Adobe Primetime TV Dashboard.
   * AccessEnabler iOS/tvOS SDK tog emot användarens SAML-svar från Video Subscriber Account Framework.

   **<u>Pro Tip:</u>** Det andra steget utlöser inga andra återanrop förutom återanropet [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) eftersom autentiseringen inte initierades explicit av programmet.


1. Programmet måste [kontrollera autentiseringsstatusen](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Viktigt!** Det här tredje steget kan utlösa en [avancerad felkod](/help/authentication/error-reporting.md) som är specifik för Apple SSO-arbetsflöde, om **något av följande är sant**:

   * ***VSA403** - Användaren är inloggad på sitt TV-leverantörskonto på
enhetssystemnivån, men användarens TV-leverantörsbehörighet är
nekas för programmet.
   * ***VSA404** - Användaren är inloggad på sitt TV-leverantörskonto på
enhetssystemnivån, men användarens TV-leverantör har behörighet
är inte definierad för programmet.
   * ***APPL\_ERROR** - Användaren är inloggad på sin TV-leverantör
på enhetssystemnivå, men kommunikationen mellan
AccessEnabler iOS/tvOS SDK och Video Subscriber Account
ett fel uppstod i ramverket.

   **Viktigt:** Det tredje steget utlöser återanropet [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) med *status* lika med 0, om **något av följande är sant**:

   * Användaren är inte inloggad på sitt TV-leverantörskonto på enhetssystemnivå eller via ett regelbundet autentiseringsflöde.
   * Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå eller via det reguljära autentiseringsflödet, men användarens TV-leverantörs autentiseringstoken TTL har passerat.
   * Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå eller via ett regelbundet autentiseringsflöde, men användarens TV-leverantörsintegrering med programmet inaktiveras via Adobe Primetime TV-instrumentpanel.
   * Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantör enkel inloggning med programmet är inaktiverad via Adobe Primetime TV Dashboard.
   * Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantörsbehörighet nekas för programmet.
   * Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men användarens TV-leverantörsbehörighet är inte fastställd för programmet.
   * Användaren är inloggad på sitt TV-leverantörskonto på enhetssystemnivå, men ett fel uppstod i kommunikationen mellan AccessEnabler iOS/tvOS SDK och Video Subscriber Account Framework.

   **Viktigt!** Det tredje steget utlöser återanropet [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) med *status* lika med 1, om **alla ovanstående är falska.**


1. Programmet måste [initiera autentiseringen](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) om den föregående autentiseringsstatuskontrollen utlöste återanropet [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) med *status* som är lika med 0.

   **<u>Pro Tip:</u>** Implementera ett av följande AccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) eller [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Viktigt!** Det här fjärde steget kan utlösa en [avancerad felkod](/help/authentication/error-reporting.md) som är specifik för Apple SSO-arbetsflöde, om **något av följande är sant**:

   * ***VSA403*** - Användarens TV-leverantörsbehörighet nekas för programmet.
   * ***VSA404*** - Användarens TV-leverantörsbehörighet är inte definierad för programmet.
   * ***VSA503*** - Ett fel uppstod i kommunikationen mellan AccessEnabler iOS/tvOS SDK och Video Subscriber Account Framework.
   * ***N003*** - Användaren valde alternativet &quot;Annan TV-leverantör&quot; i Apple MVPD-väljaren.
   * ***N004*** - Användaren valde en TV-leverantör i Apple MVPD-väljaren, vilket inte stöds (integrering eller enkel inloggning inaktiverad) av den aktuella begäraren.
   * ***N005*** - Användaren bestämde sig för att avbryta den vanliga MVPD-väljaren eller Apple MVPD-väljaren.

   **Viktigt!** Det här fjärde steget återgår till det reguljära autentiseringsflödet genom att utlösa callback-funktionen [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) och **en** av de [avancerade felkoderna](/help/authentication/error-reporting.md) ovan, om **en av de ovanstående är true**.

   **Viktigt!** Det här fjärde steget återgår till det reguljära autentiseringsflödet genom att utlösa callback-funktionen [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) eller [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) och **none** i de [avancerade felkoderna](/help/authentication/error-reporting.md) om användaren har valt en TV-leverantör som inte stöder Apple SSO, men som finns i Apple MVPD-väljaren.

   **<u>Pro Tips:</u>** AccessEnabler iOS/tvOS SDK anropar [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) i tysthet API:t om användaren väljer en TV-leverantör som inte stöder Apple SSO, men som finns i Apple MVPD-väljaren.

   **Viktigt!** Det här fjärde steget skulle försöka att tyst byta ut Apple SSO-profilen mot en Adobe-autentiseringstoken, om **alla ovanstående är falska** och **alla följande är sanna**:

   * Användarens TV-leverantörsbehörighet beviljas för programmet.
   * Användaren är inloggad/loggar för närvarande in på sitt TV-leverantörskonto på enhetssystemnivå.
   * AccessEnabler iOS/tvOS SDK tog emot användarens identifierare för TV-leverantör från Video Subscriber Account Framework.
   * Användarens TV-leverantörsintegrering med programmet aktiveras via Adobe Primetime TVE Dashboard.
   * Användarens TV-leverantör enkel inloggning med programmet aktiveras via Adobe Primetime TV-instrumentpanelen.
   * Användarens TV-leverantör fungerar inte på Adobe Primetime TV Dashboard.
   * AccessEnabler iOS/tvOS SDK tog emot användarens SAML-svar från Video Subscriber Account Framework.

   **<u>Pro Tip:</u>** Det här fjärde steget utlöser callback-funktionen [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus), oavsett resultatet av *status*, eftersom autentiseringen initierades explicit av programmet.

### Metadata {#apple-sso-cookbook-iostvos-sdk-metadata}

Programmet har möjlighet att avgöra om autentiseringen har skett till följd av en inloggning via enkel inloggning för partner eller inte, med hjälp av *tokenSource* [användarens metadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) från AccessEnabler iOS/tvOS SDK.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Utloggning {#apple-sso-cookbook-iostvos-sdk-logout}

[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) innehåller inget API för att programmässigt logga ut personer som har loggat in på sitt TV-leverantörskonto på enhetssystemnivå. För att utloggningen ska få full effekt måste slutanvändaren därför uttryckligen logga ut från *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS. Det andra alternativet som användaren skulle ha möjlighet att återkalla behörigheten att få åtkomst till användarens prenumerationsinformation från det specifika avsnittet för programinställningar (TV-leverantörens behörighetsåtkomst).

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via iOS/tvOS SDK-API:t [logOut](/help/authentication/iostvos-sdk-api-reference.md#logout) för AccessEnabler.

>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementering/implementering av tvOS.

* Programmet måste [initiera utloggningen](/help/authentication/iostvos-sdk-api-reference.md#logout) från AccessEnabler iOS/tvOS SDK. Detta skulle inte underlätta sessionsrensning på MVPD-sidan.
* Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> Accounts -> TV Provider`* på tvOS endast om [*VSA203*-statuskoden aktiveras](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Pro Tips!</u>** Följ stegen nedan för implementering/implementering av iOS/iPadOS.

* Programmet måste [initiera utloggningen](/help/authentication/iostvos-sdk-api-reference.md#logout) från AccessEnabler iOS/tvOS SDK. Detta underlättar sessionssanering på den mobila dokumentationssidan.
* Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> TV Provider`* på iOS/iPadOS endast om [*VSA203*-statuskoden aktiveras](/help/authentication/error-reporting.md).
