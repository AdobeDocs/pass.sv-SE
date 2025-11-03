---
title: Apple SSO Cookbook (REST API V1)
description: Apple SSO Cookbook (REST API V1)
exl-id: 072a011f-e1bb-4d3e-bcb5-697f2d1739cc
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# (Äldre) Apple SSO Cookbook (REST API V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Adobe Pass Authentication REST API V1 har stöd för Partner Single Sign-On (SSO) för slutanvändare av klientprogram som körs på iOS, iPadOS eller tvOS.

Det här dokumentet fungerar som ett tillägg till den befintliga dokumentationen för REST API V1, som finns [här](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md).

## Cookbook {#apple-sso-cookbook-rest-api-v1-cookbook}

För att du ska kunna dra nytta av Apple SSO-användarupplevelsen måste programmet integrera det [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) som utvecklats av Apple, och för Adobe Pass Authentication REST API V1-kommunikationen måste programmet följa stegen nedan.

### Behörighet {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>Pro Tips!</u>** Strömningsprogrammet måste begära åtkomst till användarens prenumerationsinformation som har sparats på enhetsnivå, och användaren måste ge programmet behörighet att fortsätta, på samma sätt som att ge åtkomst till enhetens kamera eller mikrofon. Den här behörigheten måste begäras per program med Apple [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) och enheten sparar användarens val.

>[!TIP]
>
> **<u>Pro Tips!</u>** Vi rekommenderar att du uppmuntrar användare som vägrar ge åtkomst till prenumerationsinformation genom att förklara fördelarna med Apple användarupplevelse med enkel inloggning, men tänk på att användaren kan ändra sitt beslut genom att gå till programinställningarna (åtkomst till tv-provider) eller till *`Settings -> TV Provider`* på iOS och iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS.

>[!TIP]
>
> **<u>Pro Tips!</u>** Vi rekommenderar att du begär användarens tillstånd när programmet försätts i förgrunden eftersom programmet kan kontrollera om användaren har [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation när som helst innan användarautentisering krävs.

### Autentisering {#apple-sso-cookbook-rest-api-v1-authentication}

* [Finns det en giltig Adobe-autentiseringstoken?](#step1)
* [Är användaren inloggad via enkel inloggning för partner?](#step2)
* [Hämta Adobe-konfiguration](#step3)
* [Starta enkel inloggning för partner med Adobe-konfiguration](#step4)
* [Har användaren loggat in?](#step5)
* [Hämta en profilförfrågan från Adobe för den valda MVPD](#step6)
* [Vidarebefordra Adobe-begäran till Partner SSO för att erhålla profilen](#step7)
* [Byt SSO-profil för partner för en Adobe-autentiseringstoken](#step8)
* [Har Adobe-token genererats?](#step9)
* [Starta ett arbetsflöde för vanlig autentisering](#step10)
* [Fortsätt med auktoriseringsflöden](#step11)

![](/help/authentication/assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Steg:&quot;Finns det en giltig Adobe-autentiseringstoken?&quot; {#step1}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via Adobe Pass-autentiseringsmediet [Kontrollera autentiseringstoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) API-tjänsten.

#### Steg:&quot;Är användaren inloggad via enkel inloggning för partner?&quot; {#step2}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* Programmet måste kontrollera om användaren har [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
* Programmet måste skicka en [förfrågan](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation.
* Programmet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.

>[!TIP]
>
> **<u>Pro Tip:</u>** Följ kodfragmentet och observera kommentarerna extra.

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### Steg: &quot;Hämta Adobe-konfiguration&quot; {#step3}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via Adobe Pass-autentiseringen [Tillhandahåll API-tjänsten MVPD List](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md).

>[!TIP]
>
> **<u>Pro Tip:</u>** Observera MVPD-egenskaperna: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* och observera de kommentarer som presenteras i kodfragment från andra steg.

#### Steg&quot;Initiera SSO-arbetsflöde för partner med Adobe config&quot; {#step4}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).

* Programmet måste kontrollera om användaren har [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
* Programmet måste tillhandahålla [delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) för VSAccountManager.
* Programmet måste skicka en [förfrågan](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation.
* Programmet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.

>[!TIP]
>
> **<u>Pro Tip:</u>** Följ kodfragmentet och observera kommentarerna extra.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Steg:&quot;Har användarinloggningen slutförts?&quot; {#step5}

>[!TIP]
>
> **<u>Pro Tips!</u>** Observera kodfragmentet i [&quot;Starta enkel inloggning för partner med Adobe config&quot;](#step4) -steget. Användarinloggningen lyckas om *`vsaMetadata!.accountProviderIdentifier`* innehåller ett giltigt värde och det aktuella datumet inte har passerat värdet *`vsaMetadata!.authenticationExpirationDate`*.

#### Steg&quot;Hämta en profilförfrågan från Adobe för den valda MVPD&quot; {#step6}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via API-tjänsten [Profilbegäran](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md) för Adobe Pass-autentisering.

>[!TIP]
>
> **<u>Pro Tips!</u>** Observera att den provider-identifierare som hämtas från Video Subscriber Account Framework representerar *`platformMappingId`* i form av Adobe Pass-autentiseringskonfiguration. Därför måste programmet fastställa egenskapsvärdet för MVPD-id med hjälp av värdet *`platformMappingId`* via API-tjänsten Adobe Pass Authentication [Provide MVPD List](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) .

#### Steg:&quot;Vidarebefordra Adobe-begäran till enkel inloggning för partner för att erhålla profilen&quot; {#step7}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount).


* Programmet måste kontrollera om användaren har [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätta endast om användaren tillåter det.
* Programmet måste skicka en [förfrågan](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för prenumerantkontoinformation.
* Programmet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)-informationen.

>[!TIP]
>
> **<u>Pro Tip:</u>** Följ kodfragmentet och observera kommentarerna extra.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Steg:&quot;Byt ut partnerns SSO-profil för en Adobe-autentiseringstoken&quot; {#step8}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via API-tjänsten för Adobe Pass-autentisering [Token Exchange](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md).

>[!TIP]
>
> **<u>Pro Tips!</u>** Observera kodfragmentet från [&quot;Vidarebefordra Adobe-begäran till enkel inloggning hos partner för att få fram profilen&quot;](#step7) steget. Denna *`vsaMetadata!.samlAttributeQueryResponse!`* representerar *`SAMLResponse`*, som måste skickas på [Token Exchange](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) och som kräver strängmanipulering och kodning (*Base64*-kodad och *URL*-kodad efteråt) innan anropet görs.

#### Steg:&quot;Har Adobe-token genererats utan fel?&quot; {#step9}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via Adobe Pass-autentiseringsmediet [Token Exchange](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) lyckades. Svaret blir *`204 No Content`*, vilket anger att token skapades och är klar att användas för auktoriseringsflödena.

#### Steg:&quot;Starta ett regelbundet autentiseringsarbetsflöde&quot; {#step10}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via Adobe Pass-autentisering [Registreringskodbegäran](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md), [Initiera autentisering](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) och [Hämta autentiseringstoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) eller [Kontrollera autentiseringstoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) API-tjänster.

>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementering/implementering av tvOS.

* Programmet måste [hämta en registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) och presentera den för slutanvändaren på den första enheten (skärmen).
* Programmet måste starta [avsökningen för att bekräfta autentiseringstillståndet](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) på den första enheten (skärmen) när registreringskoden har hämtats.
* Ett annat program måste [initiera autentisering](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) på en andra enhet (skärm) när registreringskoden används.
* Programmet måste stoppa [avsökningen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) på den första enheten (skärmen) när autentiseringstoken genereras.

>[!TIP]
>
> **<u>Pro Tips!</u>** Följ stegen nedan för implementering/implementering av iOS/iPadOS.

* Programmet måste [hämta en registreringskod](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) som inte ska visas för slutanvändaren på den första enheten (skärmen).
* Programmet måste [initiera autentisering](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) på den första enheten (skärmen) med registreringskoden och en [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) eller en [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -komponent.
* Programmet måste starta [avsökningen för att känna till autentiseringstillståndet](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) på den första enheten (skärmen) när [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) eller [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -komponenten stängs.
* Programmet måste stoppa [avsökningen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) på den första enheten (skärmen) när autentiseringstoken genereras.

#### Steg:&quot;Fortsätt med auktoriseringsflöden&quot; {#step11}

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via Adobe Pass-autentiseringen [Initiera auktorisering](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) och [Hämta API-tjänster för Short Media Token](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md).

### Utloggning {#apple-sso-cookbook-rest-api-v1-logout}

[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) innehåller inget API för att programmässigt logga ut personer som har loggat in på sitt TV-leverantörskonto på enhetssystemnivå. För att utloggningen ska få full effekt måste slutanvändaren därför uttryckligen logga ut från *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS. Det andra alternativet som användaren skulle ha möjlighet att återkalla behörigheten att få åtkomst till användarens prenumerationsinformation från det specifika avsnittet för programinställningar (TV-leverantörsåtkomst).

>[!TIP]
>
> **<u>Tips!</u>** Implementera detta via Adobe Pass-autentiseringen [Anrop av användarmetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) och [Logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) API-tjänster.

>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementering/implementering av tvOS.

* Programmet måste avgöra om autentiseringen har skett som ett resultat av en inloggning via partnerns SSO eller inte, med hjälp av *tokenSource* [användarens metadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) från Adobe Pass-autentiseringstjänsten.
* Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> Accounts -> TV Provider`* på tvOS **only** om värdet *&quot;tokenSource&quot;* är lika med *Apple&quot;.*
* Programmet måste [initiera utloggningen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) från Adobe Pass-autentiseringstjänsten med ett direkt HTTP-anrop. Detta underlättar inte sessionssanering på MVPD sida.

>[!TIP]
>
> **<u>Pro Tips!</u>** Följ stegen nedan för implementering/implementering av iOS/iPadOS.

* Programmet måste avgöra om autentiseringen har skett som ett resultat av en inloggning via partnerns SSO eller inte, med hjälp av *tokenSource* [användarens metadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) från Adobe Pass-autentiseringstjänsten.
* Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> TV Provider`* på iOS/iPadOS **only** om värdet *&quot;tokenSource&quot;* är lika med *&quot;Apple&quot;*.
* Programmet måste [initiera utloggningen](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) från Adobe Pass-autentiseringstjänsten med en [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) eller en [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -komponent. Detta underlättar sessionssanering på MVPD sida.
