---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass-autentisering
user-guide-description: Adobe Pass-autentisering är en berättigandelösning för TV Everywhere, som tillhandahåller ett modulärt ramverk för att avgöra om någon som begär åtkomst till en resurs är berättigad till den.
source-git-commit: 68e572797a39327682ea7f491ef2109374fadc2e
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 4%

---


# Adobe Pass-autentisering Hjälp {#authentication}

+ [Översikt över Adobe Pass-autentisering](home.md)
+ Adobe Pass Authentication concepts {#authentication-concepts}
   + [Tekniskt papper](technical-paper.md)
   + [Översikt för programmerare](programmer-overview.md)
   + [MVPD - översikt](mvpd-overview.md)
+ Kickstart-stödlinjer {#kickstart-guides}
   + [Programmeraren kickstart](programmer-kickstart-guide.md)
   + [MVPD - startguide](mvpd-kickstart-guide.md)
+ Integreringsguide för programmerare {#programmer-integration-guide}
   + [Översikt över guiden Integrering av programmerare](programmer-integration-guide-overview.md)
   + [Programmerarens tillståndsflöde](entitlement-flow.md)
   + [Användningsexempel för programmerare](programmer-use-cases.md)
   + [Skicka klientinformation (enhet, anslutning och program)](passing-client-information-device-connection-and-application.md)
   + REST API {#restapi}
      + [REST API - översikt](rest-api-overview.md)
      + [REST API Cookbook (Server-to-Server)](rest-api-cookbook-servertoserver.md)
      + [REST API Cookbook (klient-till-server)](rest-api-cookbook-clienttoserver.md)
      + Referens för resterande API {#rest-api-reference}
         + [REST API-referens](rest-api-reference.md)
         + [Registreringskodförfrågan](registration-code-request.md)
         + [Returregistreringspost](return-registration-record.md)
         + [Ta bort registreringspost](delete-registration-record.md)
         + [Ange MVPD-lista](provide-mvpd-list.md)
         + [Initiera autentisering](initiate-authentication.md)
         + [Kontrollera autentiseringstoken](check-authentication-token.md)
         + [Hämta autentiseringstoken](retrieve-authentication-token.md)
         + [Initiera auktorisering](initiate-authorization.md)
         + [Hämta auktoriseringstoken](retrieve-authorization-token.md)
         + [Hämta kort medietoken](obtain-short-media-token.md)
         + [Kontrollera autentiseringsflöde med andra skärmens webbapp](check-authentication-flow-by-second-screen-web-app.md)
         + [Hämta lista över förauktoriserade resurser](retrieve-list-of-preauthorized-resources.md)
         + [Hämta lista över förauktoriserade resurser per webbapp för sekundär skärm](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [Initiera utloggning](initiate-logout.md)
         + [Användarmetadata](user-metadata.md)
         + [Hämta profilbegäran](retrieve-profilerequest.md)
         + [Tokenutbyte](token-exchange.md)
         + [Kostnadsfri förhandsgranskning för tillfälligt pass och tillfälligt kampanjpass](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK - översikt](javascript-sdk-overview.md)
         + [JavaScript SDK Cookbook](javascript-sdk-cookbook.md)
         + [API-referens för JavaScript SDK](javascript-sdk-api-reference.md)
         + Riktlinjer {#js-sdk-guidelines}
            + [Inloggning och utloggning utan uppdatering](refreshless-login-and-logout.md)
         + JavaScript API {#js-api}
            + [Förhandsauktorisera](js-preauthorize.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [iOS/tvOS SDK - översikt](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK Cookbook](iostvos-sdk-cookbook.md)
         + [API-referens för iOS/tvOS SDK](iostvos-sdk-api-reference.md)
         + Riktlinjer {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS - programregistrering](iostvos-application-registration.md)
            + Riktlinjer för migrering {#migration-guidelines}
               + [Migreringshandbok för iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [Förhandsauktorisera](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK - översikt](android-sdk-overview.md)
         + [Android SDK Cookbook](android-sdk-cookbook.md)
         + [Android SDK API-referens](android-sdk-api-reference.md)
         + Riktlinjer {#androidguidelines}
            + [Android-programregistrering](android-application-registration.md)
            + [Android SDK med registrering av dynamisk klient](android-sdk-with-dynamic-client-registration.md)
         + Android API{#androidapi}
            + [Förhandsauktorisera](preauthorize-android.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS SSO - startguide för programmerare](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Amazon FireOS SSO med Clientless API Cookbook](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Amazon FireOS Technical Overview](amazon-fireos-technical-overview.md)
         + [Amazon FireOS Integration Cookbook](amazon-fireos-integration-cookbook.md)
         + [API-referens för Amazon FireOS](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS - programregistrering](amazon-fireos-application-registration.md)
         + [FireOS SDK med dynamisk klientregistrering](fireos-sdk-with-dynamic-client-registration.md)
   + Plattforms-SSO {#platform-sso}
      + APPLE SSO {#apple-sso}
         + [Apple SSO - översikt](apple-sso-overview.md)
         + [Apple SSO Cookbook (REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO Cookbook (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + Metadata för innehåll {#content-metadata}
      + [Identifierar skyddad resurs](identify-protected-resources.md)
   + Integrering med innehållsserver {#content-serv-int}
      + [Integrera Media Token Verifier](media-token-verifier-int.md)
   + Tillägg {#appendices}
      + [Felsökningstips](appendix-b-debugging-tips.md)
+ Integreringsguide för MVPD {#mvpd-int-guide}
   + [Integreringsfunktioner](mvpd-integr-features.md)
   + [Autentisering](authn-usecase.md)
   + [Autentisering med OAuth 2.0-protokollet](authn-oauth2-protocol.md)
   + [Behörighet](authz-usecase.md)
   + [Preflight-behörighet](mvpd-preflight-authz.md)
   + [MVPD-utloggning](usecase-mvpd-logout.md)
   + [Utbyte av metadata](mvpd-content-metadata-exchange.md)
   + [Utbyte av användarmetadata](mvpd-user-metadata-exchng.md)
   + [Proxy MVPD-webbtjänst](proxy-mvpd-webserv.md)
   + [Integrering av MVPD SAML-proxy](proxy-mvpd-saml-int.md)
   + [Tjänstleverantörsomfång](serv-provider-scoping.md)
   + [MVPD tillåter IP-adresser](mvpd-listing-ip-addres.md)
+ Adobe Pass autentiseringsfunktioner {#auth-features}
   + Integrering med Adobe Analytics {#analytics-int}
      + [Integrera data på serversidan för Adobe Pass Authentication i Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Använda Experience Cloud-ID i Adobe Pass-autentisering](exp-cloud-id-authn.md)
   + Tillståndsövervakning {#entitlement-service-monitoring}
      + [Tillståndsövervakning - översikt](entitlement-service-monitoring-overview.md)
      + [API för kontroll av berättigandetjänster](entitlement-service-monitoring-api.md)
      + [Mätvärden på serversidan](understanding-serverside-metrics.md)
   + Tillfälligt pass {#temp-pass}
      + [Tillfälligt pass](temp-pass.md)
      + [Kampanjtillfälligt pass](promotional-temp-pass.md)
   + Enkel inloggning {#sso}
      + [Stöd för enkel inloggning](sso-support.md)
      + [Enkel inloggning via passiv autentisering](sso-passive-authn.md)
   + Hembaserad autentisering {#home-based-auth}
      + [Hembaserad autentisering för TV Everywhere](home-based-authn-tve.md)
      + [HBA-status för MVPD](hba-status-mvpds.md)
   + Användarmetadata {#user-metadat}
      + [Användarmetadata](user-metadata-feature.md)
   + [Preflight-behörighet](preflight-authz.md)
   + Felrapportering {#error-reportn}
      + [Felrapportering](error-reporting.md)
      + [Förbättrade felkoder](enhanced-error-codes.md)
   + Klientregistrering {#client-regn}
      + [Dynamisk klientregistrering](dynamic-client-registration.md)
      + [API för registrering av dynamisk klient](dynamic-client-registration-api.md)
      + [Registreringshantering för dynamisk klient](dynamic-client-registration-management.md)
   + Försämringstjänst {#degrn-service}
      + [Översikt över API-nedgradering](degradation-api-overview.md)
   + Integritetsberedskap {#privacy-readiness}
      + [Översikt över stöd för Privecy](privacy-supp-overview.md)
      + [Hur man gör en sekretessförfrågan](make-privacy-req.md)
+ Tips och felsökning {#tips-troubleshoot}
   + [Tillåt PDF-filer i urvalsdialogrutan](allow-mvpd-selectn-dialog.md)
   + [Förhindra att PDF-filer visas i urvalsdialogrutan](prevent-mvpd-selectn-dialog.md)
+ Support {#support}
   + [Eskaleringsprocedurer](escalation-procedures.md)
   + [Övervaka Adobe Pass Adobe PayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [Lägsta systemkrav](minimum-system-requirements.md)
+ Versionsinformation {#release-notes}
   + [Versionskommentarer för Adobe Pass Authentication 2.68](auth-rn-268.md)
   + [Versionsinformation om Adobe Pass Authentication 2.67](auth-rn-267.md)
   + [Versionsinformation om Adobe Pass Authentication 2.66](auth-rn-266.md)
   + [Versionsinformation om Adobe Pass Authentication 2.65.1](auth-rn-2651.md)
   + [Versionskommentarer för Adobe Pass Authentication 2.65](auth-rn-265.md)
   + [Versionsinformation om Adobe Pass Authentication 2.64.1](auth-rn-2641.md)
   + [Versionsinformation om Adobe Pass Authentication 2.64](auth-rn-264.md)
   + [Versionskommentarer för Adobe Pass Authentication 2.63](auth-rn-263.md)
   + [Versionsinformation om Adobe Pass Authentication 2.62.1](auth-rn-2621.md)
   + Versionsinformation om iOS/tvOS SDK  {#release-notes-ios}
      + [Adobe Pass Authentication iOS / tvOS 3.8.2 Release Notes](authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.1 versionsinformation](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0 Release Notes](authn-rn-ios-tvos-370.md)
   + Versionsinformation för Android SDK {#release-notes-android}
      + [Versionsinformation om Adobe Pass-autentisering Android 3.7.3](authn-rn-android-373.md)
+ Tekniska anteckningar {#tech-notes}
   + Adobe Pass Authentication SDKs {#primetime-authentication-sdks}
      + [Frågor och svar om certifikat](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [JS SDK-begränsningar för Safari-webbläsare](js-sdk-limitations-for-safari-browser.md)
         + [Cookies Updates - SameSite and Secure flags](cookies-updates--samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Åtkomstaktivera Android SDK enkel inloggning (SSO) i Android 10-program](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass Authentication and the Android 6 &quot;Marshmallow&quot; New Permissions Model](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [Stöd för WKWebView i iOS SDK 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [Stöd för SFSafariViewController i iOS SDK 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [enkel inloggning på iOS när Adobe Pass Authentication Access Enabler används](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS-autentiseringsfel - adobepass.ios.app hittades inte](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Återställ tillfälligt pass i iOS](reset-temp-pass-on-ios.md)
         + [Felsöka AccessEnabler iOS/tvOS SDK med hjälp av apploggarna i konsolen](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg](accessenabler-iostvos-370-upgrade-path.md)
   + Godkänn autentiseringsmiljöer {#primetime-authentication-environments}
      + [Förstå Adobe-miljöer](understanding-the-adobe-environments.md)
      + [Konfigurera din miljö och testning i Pre-Qual](setting-up-your-environment-and-testing-in-prequal.md)
      + [Testa autentiserings- och auktoriseringsflöden med Adobe API-testwebbplats](test-authn-authz-flows-using-adobes-api-test-site.md)
   + Klientlöst API {#clientless-api}
      + [Kundfri API-implementering - felkoder/meddelanden med trolig orsak/orsak](clientless-api-implementation-error-codes--messages-with-probable-reason--cause.md)
      + [Klientlöst API-flöde i frånvaro av enhets-ID](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Klientlös: Undvik att använda &#39;&amp;&#39;reg_code i /authenticate Request](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Aktivera Adobe Pass Entitlement Services för en programmerare på Xbox 360 och XboxOne ClientCloud](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Typ och mått av klientlös enhet](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + Användarupplevelse {#user-exp}
      + [Migrera inloggningssidan för MVPD från iFrame till popup-fönster](migr-mvpd-login-iframe-popup.md)
      + [Preflight-funktion: Så här aktiverar, felsöker eller fastställer du problemet](preflight-feature.md)
   + Verktyg {#tools-and-utilities}
      + [Använda Charles Proxy](using-charles-proxy.md)
   + Concepts {#concepts}
      + [Förstå användar-ID:n](understanding-user-ids.md)
+ [Användarhandbok för TVE Dashboard](tve-dashboard-user-guide.md)
+ [Ordlista](glossary.md)
