---
title: Apple SSO Cookbook (REST API)
description: Apple SSO Cookbook (REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 0%

---

# Apple SSO Cookbook (REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#Introduction}

Adobe Pass Authentication REST API har stöd för plattformsautentisering med enkel inloggning (SSO) för slutanvändare av klientprogram som körs på iOS, iPadOS eller tvOS via det vi kallar Apple SSO-arbetsflöde.

Observera att det här dokumentet fungerar som ett tillägg till den befintliga REST API-dokumentationen, som du hittar [här](/help/authentication/rest-api-reference.md).

</br>

## Cookbooks {#Cookbooks}

För att dra nytta av Apple SSO-användarupplevelse måste ett program integrera [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) när det gäller kommunikationen med Adobe Pass Authentication REST API, som utvecklats av Apple, måste programmet följa den tipssekvens som presenteras nedan.

</br>

### Autentisering {#Authentication}

- [Finns det en giltig Adobe-autentiseringstoken?](#Is_there_a_valid_Adobe_authentication_token)
- [Är användaren inloggad via enkel inloggning för plattform?](#Is_the_user_logged_in_via_Platform_SSO)
- [Hämta Adobe-konfiguration](#Fetch_Adobe_configuration)
- [Starta arbetsflödet för enkel inloggning på plattformen med Adobe config](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [Har användaren loggat in?](#Is_user_login_successful)
- [Hämta en profilbegäran från Adobe för det valda MVPD-programmet](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Vidarebefordra Adobe-begäran till enkel inloggning för plattformen för att erhålla profilen](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Exchange-plattformens SSO-profil för en Adobe-autentiseringstoken](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Genereras Adobe-token korrekt?](#Is_Adobe_token_generated_successfully)
- [Starta autentiseringsarbetsflöde för andra skärmen](#Initiate_second_screen_authentication_workflow)
- [Fortsätt med auktoriseringsflöden](#Proceed_with_authorization_flows)



![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)

</br>

#### Steg: &quot;Finns det en giltig token för autentisering i Adobe?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via [Adobe Pass-autentisering](/help/authentication/check-authentication-token.md) service.

</br>

#### Steg:&quot;Är användaren inloggad via plattformens SSO?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) ramverk.

- Programmet måste då kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätt bara om användaren tillåter det.
- Ansökan måste då lämna in en [förfrågan](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för information om prenumerationskonto.
- Programmet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) information.



>[!TIP]
>
> **<u>Pro Tip:</u>** Följ kodfragmentet och observera kommentarerna.

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
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

</br>

#### Steg: &quot;Hämta Adobe-konfiguration&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via [Adobe Pass-autentisering](/help/authentication/provide-mvpd-list.md) service.


>[!TIP]
>
> **<u>Pro Tip:</u>** Observera egenskaperna för MVPD: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* och lägg märke till kommentarerna i kodfragment från andra steg.

</br>

#### Steg&quot;Initiera SSO-arbetsflöde för plattformen med Adobe config&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) ramverk.

- Programmet måste då kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätt bara om användaren tillåter det.
- Ansökan måste innehålla en [delegera](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) för VSAccountManager.
- Ansökan måste då lämna in en [förfrågan](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för information om prenumerationskonto.
- Programmet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) information.



>[!TIP]
>
> **<u>Pro Tip:</u>** Följ kodfragmentet och observera kommentarerna.


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
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Steg:&quot;Har användarinloggningen slutförts?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Pro Tip:</u>** Observera kodfragmentet i [&quot;Starta arbetsflödet för enkel inloggning på plattformen med Adobe config&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config) steg. Användarinloggningen lyckas om *`vsaMetadata!.accountProviderIdentifier`* innehåller ett giltigt värde och det aktuella datumet har inte passerat *`vsaMetadata!.authenticationExpirationDate`* värde.

</br>

#### Steg&quot;Hämta en profilförfrågan från Adobe för det valda MVPD&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via Adobe Pass Authentication [Profilbegäran](/help/authentication/retrieve-profilerequest.md) service.

>[!TIP]
>
> **<u>Pro Tip:</u>** Observera att den leverantörsidentifierare som hämtas från Video Subscriber Account-ramverket representerar *`platformMappingId`* när det gäller konfigurationen av Adobe Pass-autentisering. Därför måste programmet fastställa egenskapsvärdet för MVPD ID med hjälp av *`platformMappingId`* genom Adobe Pass-autentisering [Ange MVPD-lista](/help/authentication/provide-mvpd-list.md) service.

</br>

#### Steg:&quot;Vidarebefordra Adobe-begäran till plattformens SSO för att hämta profilen&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) ramverk.


- Programmet måste då kontrollera [behörighet att komma åt](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) användarens prenumerationsinformation och fortsätt bara om användaren tillåter det.
- Ansökan måste då lämna in en [förfrågan](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) för information om prenumerationskonto.
- Programmet måste vänta och bearbeta [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) information.



>[!TIP]
>
> **<u>Pro Tip:</u>** Följ kodfragmentet och observera kommentarerna.

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
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Steg:&quot;Byt ut plattformens SSO-profil för en autentiseringstoken för Adobe&quot; {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via Adobe Pass Authentication [Tokenutbyte](/help/authentication/token-exchange.md) service.


>[!TIP]
>
> **<u>Pro Tip:</u>** Observera kodfragmentet i [&quot;Vidarebefordra Adobe-begäran till enkel inloggning för plattformen för att erhålla profilen&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) steg. Detta *`vsaMetadata!.samlAttributeQueryResponse!`* representerar *`SAMLResponse`* som måste skickas vidare [Tokenutbyte](/help/authentication/token-exchange.md) och kräver strängändring och kodning (*Base64* kodade och *URL* kodas därefter) innan samtalet görs.

</br>

#### Steg:&quot;Har Adobe-token genererats utan fel?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via Adobe Pass-autentisering [Tokenutbyte](/help/authentication/token-exchange.md) ett lyckat svar, som *`204 No Content`*, vilket anger att token har skapats och är klar att användas för auktoriseringsflödena.

</br>

#### Steg:&quot;Initiera ett andra autentiseringsarbetsflöde för skärmen&quot; {#Initiate_second_screen_authentication_workflow}

**Viktigt:** Terminologi för autentisering på andra skärmen är lämplig för AppleTV, medan terminologi för autentisering på första skärmen/för det reguljära autentiseringsarbetsflödet är lämpligare för iPhone och iPad.


>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via Adobe Pass Authentication

[Registreringskodförfrågan](/help/authentication/registration-code-request.md), [Initiera autentisering](/help/authentication/initiate-authentication.md) och [REST API Retrieve Authentication Token](/help/authentication/retrieve-authentication-token.md) eller [Kontrollera autentiseringstoken](/help/authentication/check-authentication-token.md) tjänster.


>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementeringen/implementeringarna av tvOS.

- Ansökan måste [få en registreringskod](/help/authentication/registration-code-request.md) och presentera den för slutanvändaren på den första enheten (skärmen).
- Programmet måste då starta [avfråga för att bekräfta autentiseringsstatus](/help/authentication/retrieve-authentication-token.md) på den första enheten (skärmen) efter att registreringskoden erhållits.
- Ett annat program måste [initiera autentisering](/help/authentication/initiate-authentication.md) på en andra enhet (skärm) när registreringskoden används.
- Programmet måste då sluta [avsökning](/help/authentication/retrieve-authentication-token.md) på den första enheten (skärmen) när autentiseringstoken genereras.



>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementering/implementering av iOS/iPadOS.

- Ansökan måste [få en registreringskod](/help/authentication/registration-code-request.md) som inte ska visas för slutanvändaren på den första enheten (skärmen).
- Ansökan måste [initiera autentisering](/help/authentication/initiate-authentication.md) på den första enheten (skärmen) med registreringskoden och en [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) eller en [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -komponenten.
- Programmet måste då starta [avfråga för att känna till autentiseringsstatus](/help/authentication/retrieve-authentication-token.md) på den första enheten (skärmen) efter [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) eller [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) stängs.
- Programmet måste då sluta [avsökning](/help/authentication/retrieve-authentication-token.md) på den första enheten (skärmen) när autentiseringstoken genereras.

</br>

#### Steg:&quot;Fortsätt med auktoriseringsflöden&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via Adobe Pass Authentication [Initiera auktorisering](/help/authentication/initiate-authorization.md) och [Hämta kort medietoken](/help/authentication/obtain-short-media-token.md) tjänster.

</br>

### Utloggning {#Logout}

The [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) ramverket innehåller inte något API för att programmässigt logga ut personer som har loggat in på sitt TV-leverantörskonto på enhetssystemnivå. För att utloggningen ska få full effekt måste slutanvändaren därför uttryckligen logga ut från *`Settings -> TV Provider`* på iOS/iPadOS eller *`Settings -> Accounts -> TV Provider`* på tvOS. Det andra alternativet som användaren skulle ha möjlighet att återkalla behörigheten att få åtkomst till användarens prenumerationsinformation från det specifika avsnittet för programinställningar (TV-leverantörsåtkomst).

>[!TIP]
>
> **<u>Tips:</u>** Implementera detta via Adobe Pass Authentication [Anrop av användarmetadata](/help/authentication/user-metadata.md) och [Utloggning](/help/authentication/initiate-logout.md) tjänster.


>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementeringen/implementeringarna av tvOS.


- Programmet måste avgöra om autentiseringen har skett som ett resultat av en inloggning via plattformens SSO eller inte, med hjälp av &quot;*tokenSource&quot;* [användar-metadata](/help/authentication/user-metadata.md) från Adobe Pass autentiseringstjänst.
- Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> Accounts -> TV Provider`* på tvOS **endast** om *&quot;tokenSource&quot;* värdet är lika med *Apple&quot;.*
- Ansökan måste [initiera utloggningen](/help/authentication/initiate-logout.md) från tjänsten Adobe Pass Authentication med ett direkt HTTP-anrop. Detta skulle inte underlätta sessionsrensning på MVPD-sidan.



>[!TIP]
>
> **<u>Pro Tip:</u>** Följ stegen nedan för implementering/implementering av iOS/iPadOS.

- Programmet måste avgöra om autentiseringen har skett som ett resultat av en inloggning via plattformens SSO eller inte, med hjälp av &quot;*tokenSource&quot;* [användar-metadata](/help/authentication/user-metadata.md) från Adobe Pass autentiseringstjänst.
- Programmet måste instruera/uppmana användaren att explicit logga ut från *`Settings -> TV Provider`* på iOS/iPadOS **endast** om *&quot;tokenSource&quot;* värdet är lika med *&quot;Apple&quot;*.
- Ansökan måste [initiera utloggningen](/help/authentication/initiate-logout.md) från tjänsten Adobe Pass Authentication med en [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) eller en [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) -komponenten. Detta underlättar sessionssanering på den mobila dokumentationssidan.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
