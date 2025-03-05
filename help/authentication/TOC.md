---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass-autentisering
user-guide-description: Adobe Pass-autentisering är en berättigandelösning för TV Everywhere, som tillhandahåller ett modulärt ramverk för att avgöra om någon som begär åtkomst till en resurs är berättigad till den.
source-git-commit: d8097b8419aa36140e6ff550714730059555fd14
workflow-type: tm+mt
source-wordcount: '1248'
ht-degree: 2%

---


# Hjälp för Adobe Pass-autentisering {#authentication}

+ [Adobe Pass-autentisering](home.md)
+ [Produktmeddelanden](product-announcements.md)
+ Produktreleaser {#product-releases}
   + 2025 {#2025}
      + [Versionsinformation om Adobe Pass Authentication 3.1.0](notes-releases/auth-rn-310.md)
      + [Versionsinformation om Adobe Pass Authentication JavaScript 4.7.1](notes-releases/authn-rn-javascript-471.md)
   + 2024 {#2024}
      + [Versionsinformation om Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md)
      + [Versionskommentarer för Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md)
      + [Versionsinformation om Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md)
      + [Versionskommentarer för Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md)
      + [Versionsinformation om Adobe Pass Authentication JavaScript 4.7.0](notes-releases/authn-rn-javascript-470.md)
      + [Adobe Pass Authentication iOS / tvOS 3.9.2 versionsinformation](notes-releases/authn-rn-ios-tvos-392.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.4 versionsinformation](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#2023}
      + [Versionskommentarer för Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md)
      + [Versionsinformation om Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md)
      + [Versionsinformation om Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md)
      + [Versionsinformation om Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md)
      + [Versionskommentarer för Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md)
      + [Versionsinformation om Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.3 versionsinformation](notes-releases/authn-rn-ios-tvos-383.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.2 versionsinformation](notes-releases/authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.1 versionsinformation](notes-releases/authn-rn-ios-tvos-381.md)
      + [Versionsinformation om Adobe Pass Authentication Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + 2022 {#2022}
      + [Versionsinformation om Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md)
      + [Versionskommentarer för Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md)
      + [Versionsinformation om Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md)
      + [Versionsinformation om Adobe Pass Authentication JavaScript 4.6.0](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#2021}
      + [Versionsinformation om Adobe Pass Authentication JavaScript 4.4.0](notes-releases/authn-rn-javascript-440.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0 versionsinformation](notes-releases/authn-rn-ios-tvos-370.md)
+ Kickstart {#kickstart}
   + [Tekniskt papper](kickstart/technical-paper.md)
   + [Programmeraren kickstart](kickstart/programmer-kickstart-guide.md)
   + [MVPD kickstart guide](kickstart/mvpd-kickstart-guide.md)
   + [Supportprocedurer Frågor och svar](kickstart/support-procedures-faqs.md)
+ Integreringsguide för programmerare {#integration-guide-programmers}
   + [Integreringsguide för programmerare](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Systemkrav](integration-guide-programmers/minimum-system-requirements.md)
   + [Begränsningsmekanism](integration-guide-programmers/throttling-mechanism.md)
   + REST API:er {#rest-apis}
      + REST API DCR {#rest-api-dcr}
         + [Översikt över registrering av dynamisk klient](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + [Ordlista för registrering av dynamisk klient](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
         + [Vanliga frågor om registrering av dynamiska klienter](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)
         + API:er {#rest-api-dcr-apis}
            + [Hämta klientautentiseringsuppgifter](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Hämta åtkomsttoken](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Flöden {#rest-api-dcr-flows}
            + [Dynamiskt klientregistreringsflöde](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [REST API V2 - översikt](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [REST API V2-ordlista](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [REST API V2 - frågor och svar](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API:er {#rest-api-v2-apis}
            + [Översikt över REST API V2-API:er](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + Konfiguration {#rest-api-v2-configuration-apis}
               + [Hämta konfiguration för en viss tjänsteleverantör](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sessioner {#rest-api-v2-sessions-apis}
               + [Skapa autentiseringssession](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Återuppta autentiseringssession](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Hämta autentiseringssession](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Utför autentisering i användaragent](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Profiler {#rest-api-v2-profiles-apis}
               + [Hämta profiler](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Hämta profil för specifik mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Hämta profil för specifik kod](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + Beslut {#rest-api-v2-decisions-apis}
               + [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Hämta förauktoriseringsbeslut med hjälp av en specifik mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + Utloggning {#rest-api-v2-logout-apis}
               + [Initiera utloggning för specifik mvpd](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + Enkel inloggning för partner {#rest-api-v2-partner-single-sign-on-apis}
               + [Hämta partnerautentiseringsbegäran](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Skapa och hämta profil med partnerautentiseringssvar](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Flöden {#rest-api-v2-flows}
            + [Översikt över REST API V2-flöden](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Grundläggande åtkomstflöden {#rest-api-v2-basic-access-flows}
               + [Grundläggande profilflöden som utförs i det primära programmet](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [Grundläggande profiler som körs i sekundärt program](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Grundläggande autentiseringsflöde som utförs i det primära programmet](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Grundläggande autentiseringsflöde som utförs i sekundärt program](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Grundläggande auktoriseringsflöde som utförs i primärt program](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Grundläggande utloggningsflöde som har utförts i det primära programmet](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Försämrade åtkomstflöden {#rest-api-v2-degraded-access-flows}
               + [Försämrade åtkomstflöden](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Tillfälliga åtkomstflöden {#rest-api-v2-temporary-access-flows}
               + [Tillfälliga åtkomstflöden](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Åtkomstflöden för enkel inloggning {#rest-api-v2-single-sign-on-access-flows}
               + [Samlad inloggning med partnerflöden](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Samlad inloggning med plattformsidentitetsflöden](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Enkel inloggning med tjänsttoken-flöden](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Enkelt utloggningsflöde](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Cookbooks {#rest-api-v2-cookbooks}
            + [REST API V2 Cookbook (klient-till-server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [REST API V2 Cookbook (Server-to-Server)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Bilaga {#rest-api-v2-appendix}
            + Rubriker {#rest-api-v2-appendix-headers}
               + [Header - Authorization](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [Header - AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [Header - X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [Header - AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [Header - Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [Header - AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [Header - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Standardfunktioner {#standard-features}
      + Tillstånd {#entitlements}
         + [Användarmetadata](integration-guide-programmers/features-standard/entitlements/user-metadata.md)
         + [Beslut](integration-guide-programmers/features-standard/entitlements/decisions.md)
         + [Medietoken](integration-guide-programmers/features-standard/entitlements/media-tokens.md)
      + Felrapportering {#error-reporting}
         + [Förbättrade felkoder](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
      + Enkel inloggningsåtkomst {#sso-access}
         + Enkel inloggning för partner {#partner-sso}
            + Apple enkel inloggning {#apple-sso}
               + [Apple SSO - översikt](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Apple SSO Cookbook (REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
         + Plattform enkel inloggning {#platform-sso}
            + Amazon enkel inloggning {#amazon-sso}
               + [Amazon SSO Cookbook (REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
            + Kör enkel inloggning {#roku-sso}
               + [Roku SSO-översikt](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
      + Hembaserad autentiseringsåtkomst {#hba-access}
         + [Hembaserad autentisering (HBA)](integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)
      + Sekretesssupport {#privacy-support}
         + [Översikt över stöd för Privecy](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Hur man gör en sekretessförfrågan](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Premium-funktioner {#features-premium}
      + Tillfällig åtkomst {#temporary-access}
         + [TempPass-funktion](integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
      + Försämrad åtkomst {#degraded-access}
         + [Försämringsfunktion](integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
      + ESM {#esm}
         + [Tillståndsövervakning - översikt](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [API för kontroll av berättigandetjänster](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Mätvärden på serversidan](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analyser {#analytics}
         + [Integrera data på serversidan för Adobe Pass Authentication i Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Använda Experience Cloud ID i Adobe Pass-autentisering](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + Äldre {#legacy}
      + (Äldre) REST API V1 {#rest-api-v1}
         + [(Äldre) REST API V1 - översikt](integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
         + [(Äldre) REST API V1-referens](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + (Äldre) API:er {#rest-api-v1-apis}
            + [(Äldre) Registreringskodbegäran](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [(Äldre) Returregistreringspost](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [(Äldre) Ta bort registreringspost](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [(Äldre) Ange MVPD-lista](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [(Äldre) Initiera autentisering](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [(Äldre) Kontrollera autentiseringstoken](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [(Äldre) Hämta autentiseringstoken](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [(Äldre) Initiera auktorisering](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [(Äldre) Hämta auktoriseringstoken](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [(Äldre) Hämta kort medietoken](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [(Äldre) Kontrollera autentiseringsflöde med andra skärmens webbapp](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [(Äldre) Hämta lista över förauktoriserade resurser](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [(Äldre) Hämta lista över förauktoriserade resurser via webbappen för sekundär skärm](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [(Äldre) Initiera utloggning](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [(Äldre) Användarmetadata](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [(Äldre) Hämta profilbegäran](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [(Äldre) Tokenutbyte](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [(Äldre) Kostnadsfri förhandsvisning för tillfälligt pass och kampanjtillfälligt pass](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + (Äldre) Cookbooks {#rest-api-v1-cookbooks}
            + [(Äldre) REST API V1-kokbok (klient-till-server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [(Äldre) REST API V1-kokbok (server till server)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + (Äldre) SDK:er {#sdks}
         + (Äldre) JavaScript SDK {#javascript-sdk}
            + [(Äldre) JavaScript SDK - översikt](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [(Äldre) JavaScript SDK Cookbook](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [(Äldre) JavaScript SDK API Reference](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [(Äldre) JavaScript SDK API PreAuthorize](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + (Äldre) Riktlinjer {#javascript-sdk-guidelines}
               + [(Äldre) Uppdatera utan inloggning och utloggning](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (Äldre) iOS/tvOS SDK {#ios-tvos-sdk}
            + [(Äldre) iOS/tvOS SDK - översikt](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [(Äldre) iOS/tvOS SDK Cookbook](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [(Äldre) API-referens för iOS/tvOS SDK](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [(Äldre) iOS/tvOS SDK API-förhandsauktorisering](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + (Äldre) Riktlinjer {#ios-tvos-sdk-guidelines}
               + [(Äldre) iOS/tvOS-programregistrering](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [(Äldre) Migreringshandbok för iOS/tvOS v3.x](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [(Äldre) iOS/tvOS Storage Integrity Checks](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + (Äldre) Android SDK {#android-sdk}
            + [(Äldre) Android SDK - översikt](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [(Äldre) Android SDK Cookbook](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [(Äldre) Android SDK API Reference](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [(Äldre) Android SDK API PreAuthorize](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + (Äldre) Riktlinjer {#android-sdk-guidelines}
               + [(Äldre) Android Application Registration](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [(Äldre) Android SDK med registrering av dynamisk klient](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (Äldre) FireOS SDK {#fireos-sdk}
            + [(Äldre) Amazon FireOS Technical Overview](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [(Äldre) Amazon FireOS Integration Cookbook](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [(Äldre) API-referens för Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [(Äldre) Amazon FireOS-programregistrering](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [(Äldre) FireOS SDK med registrering av dynamisk klient](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [(Äldre) Amazon FireOS SSO - startguide för programmerare](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
      + (Äldre) Klientinformation {#client-information}
         + [(Äldre) Skicka klientinformation (enhet, anslutning och program)](integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)
      + (Äldre) Felrapportering {#error-reporting}
         + [(Äldre) Felrapportering](integration-guide-programmers/legacy/error-reporting/error-reporting.md)
      + (Äldre) Enkel inloggningsåtkomst {#sso-access}
         + [(Äldre) Stöd för enkel inloggning](integration-guide-programmers/legacy/sso-access/sso-support.md)
         + [(Äldre) enkel inloggning via passiv autentisering](integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)
         + [(Äldre) Amazon SSO Cookbook (REST API V1)](integration-guide-programmers/legacy/sso-access/amazon-sso-cookbook-rest-api-v1.md)
         + [(Äldre) Apple SSO Cookbook (REST API V1)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)
         + [(Äldre) Apple SSO Cookbook (iOS/tvOS SDK)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)
      + (Äldre) TVE-instrumentpanel {#tve-dashboard}
         + [(Äldre) Användarhandbok för TVE Dashboard](integration-guide-programmers/legacy/tve-dashboard/tve-dashboard-user-guide.md)
      + (Äldre) Tech Notes {#tech-notes}
         + (Äldre) REST API V1 {#rest-api-v1}
            + [(Äldre) API-implementering utan klient - Felkoder/meddelanden med trolig orsak/orsak](integration-guide-programmers/legacy/notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
            + [(Äldre) Klientlöst API-flöde i frånvaro av enhets-ID](integration-guide-programmers/legacy/notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
            + [(Äldre) Klientlös: Undvik att använda &#39;&amp;&#39;reg_code i /authenticate Request](integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
            + [(Äldre) Aktivera Adobe Pass Entitlement Services för en programmerare på Xbox 360 och XboxOne Clientless](integration-guide-programmers/legacy/notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
            + [(Äldre) Typ och mått av klientlös enhet](integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
         + (Äldre) SDK:er {#sdks}
            + [(Äldre) certifikatfrågor och svar](integration-guide-programmers/legacy/notes-technical/certificates-qa.md)
            + [(Äldre) Förstå användar-ID](integration-guide-programmers/legacy/notes-technical/understanding-user-ids.md)
            + (Äldre) JavaScript SDK {#javascript-sdk}
               + [(Äldre) Tracking Prevention Assessment - Apple Safari](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-apple-safari.md)
               + [(Äldre) Tracking Prevention Assessment - Google Chrome](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-google-chrome.md)
               + [(Äldre) Cookies Updates - SameSite and Secure flags](integration-guide-programmers/legacy/notes-technical/cookies-updates-samesite-and-secure-flags.md)
               + [(Äldre) Felsökningstips](integration-guide-programmers/legacy/notes-technical/appendix-b-debugging-tips.md)
            + (Äldre) Android SDK {#android-sdk}
               + [(Äldre) Åtkomst Aktivera Android SDK enkel inloggning (SSO) i Android 10-program](integration-guide-programmers/legacy/notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
               + [(Äldre) Adobe Pass Authentication and the Android 6 &quot;Marshmallow&quot; New Permissions Model](integration-guide-programmers/legacy/notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
            + (Äldre) iOS/tvOS SDK {#ios-tvos-sdk}
               + [(Äldre) Stöd för WKWebView i iOS SDK 3.1+](integration-guide-programmers/legacy/notes-technical/wkwebview-support-on-ios-sdk-31.md)
               + [(Äldre) Stöd för SFSafariViewController i iOS SDK 3.2+](integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
               + [(Äldre) enkel inloggning på iOS när Adobe Pass Authentication Access Enabler används](integration-guide-programmers/legacy/notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
               + [(Äldre) iOS-autentiseringsfel - adobepass.ios.app kan inte hittas](integration-guide-programmers/legacy/notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
               + [(Äldre) Återställ tillfälligt pass på iOS](integration-guide-programmers/legacy/notes-technical/reset-temp-pass-on-ios.md)
               + [(Äldre) Felsöka AccessEnabler iOS/tvOS SDK med hjälp av apploggar för konsolen](integration-guide-programmers/legacy/notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
               + [(Äldre) AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg](integration-guide-programmers/legacy/notes-technical/accessenabler-iostvos-370-upgrade-path.md)
         + (Äldre) Användarupplevelse {#user-experience}
            + [(Äldre) Så här migrerar du MVPD inloggningssida från iFrame till popup-fönster](integration-guide-programmers/legacy/notes-technical/migr-mvpd-login-iframe-popup.md)
            + [(Äldre) Preflight-funktion: Så här aktiverar, felsöker eller identifierar du problemet](integration-guide-programmers/legacy/notes-technical/preflight-feature.md)
            + [(Äldre) Tillåt PDF-filer i urvalsdialogrutan](integration-guide-programmers/legacy/notes-technical/allow-mvpd-selectn-dialog.md)
            + [(Äldre) Förhindra att PDF-filer visas i urvalsdialogrutan](integration-guide-programmers/legacy/notes-technical/prevent-mvpd-selectn-dialog.md)
         + (Äldre) Felsökning {#troubleshooting}
            + [(Äldre) Använda Charles Proxy](integration-guide-programmers/legacy/notes-technical/using-charles-proxy.md)
            + [(Äldre) Övervaka Adobe Pass Adobe PayTV Pass](integration-guide-programmers/legacy/notes-technical/monitoring-adobe-pay-tv-pass.md)
            + [(Äldre) Så här testar du autentiserings- och auktoriseringsflöden med hjälp av testwebbplatsen för Adobe API](integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
+ Integreringsguide för MVPD:er {#integration-guide-mvpds}
   + [Integreringsguide för MVPD](integration-guide-mvpds/mvpd-integration-guide-overview.md)
   + [Autentisering](integration-guide-mvpds/authn-usecase.md)
   + [Autentisering med OAuth 2.0-protokollet](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Behörighet](integration-guide-mvpds/authz-usecase.md)
   + [Preflight-behörighet](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [MVPD Logout](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Utbyte av metadata](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Utbyte av användarmetadata](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Proxy MVPD webbtjänst](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Proxy MVPD SAML-integrering](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Tjänstleverantörsomfång](integration-guide-mvpds/serv-provider-scoping.md)
   + [MVPD tillåt IP-adresser](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Användarhandbok för TVE Dashboard {#user-guide-tve-dashboard}
   + [Översikt över TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Miljö](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Kontrollpanel](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Kanaler](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programmerare](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Integreringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Rapporter](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Ändringslogg](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
+ Tech Notes {#tech-notes}
   + Miljöer {#environments}
      + [Förstå Adobe-miljöer](notes-technical/environments/understanding-the-adobe-environments.md)
      + [Konfigurera din miljö och testning i Pre-Qual](notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)
