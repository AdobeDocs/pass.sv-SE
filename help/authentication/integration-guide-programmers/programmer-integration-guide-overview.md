---
title: Integreringsguide för programmerare
description: Integreringsguide för programmerare
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 0%

---

# Integreringsguide för programmerare {#programmer-integration-guide}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här integreringsguiden är avsedd för innehållsleverantörer (programmerare) som tänker integrera med Adobe® Pass Authentication.

I dagens digitala landskap kan tittarna få åtkomst till internet när som helst, var som helst och begära åtkomst till det skyddade innehållet. De kanske vill titta på ett engångsevenemang eller söka rätt att strömma en hel tv-serie som ni sänder.

Innan du beviljar åtkomst till skyddat innehåll måste du avgöra om tittaren är berättigad till det. Viktiga frågor:

* **Har visningsprogrammet en aktiv prenumeration med en distributör av flerkanalsvideo (MVPD)?**
* **Innehåller den prenumerationen din programmering?**

## Adobe Pass Authentication for TV Everywhere {#adobe-pass-authentication-for-tv-everywhere}

För programmerare är det inte alltid så enkelt att avgöra behörighet. MVPD är förvaringsinstitut för sina kunders ID-data och åtkomstbehörigheter. Programmerare kan abonnera på ett stort antal olika programmeringsskyltar, som alla fungerar med unika system. Dessa svårigheter gör det både tekniskt utmanande och resurskrävande att verifiera berättigandet.

![Användarberättigandet bestäms direkt av programmeraren](../assets/user-ent-by-progr.png){align="center"}

*Användarberättigandet bestäms direkt av programmeraren*

Adobe Pass Authentication underlättar på ett säkert sätt berättigandetransaktioner mellan programmerare och distributörer av videoprogrammeringstjänster, vilket gör det snabbt, enkelt och säkert att tillhandahålla skyddat innehåll till berättigade tittare.

![Användarrättigheter förmedlas av Adobe Pass-autentisering](../assets/user-ent-mediatedby-authn.png){align="center"}

*Användarrättigheter förmedlas av Adobe Pass-autentisering*

Adobe Pass Authentication fungerar som en proxy och underlättar tillståndsflödet mellan programmerare och distributörer genom att erbjuda säkra och enhetliga gränssnitt för båda parter.

För programmerare tillhandahåller Adobe Pass Authentication API:er som en del av en **Standard**- eller en **Premium**-nivå:

* Adobe Pass standard-API för autentisering:
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium-API:er för Adobe Pass-autentisering:
   * [Återställ API för tillfälligt pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass-funktion](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [Försämring av API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Försämringsfunktion](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [API för tillståndsövervaknings-API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### Användningsexempel {#use-cases}

I det här avsnittet beskrivs ytterligare de fall av programvaruintegrering som stöds av Adobe Pass Authentication:

* Programmerare (TVE) med ett enda kanalnätverk

  På så sätt kan programmeraren ge tittarna åtkomst till innehållet från ett enprofilerat kanalnätverk i en TVE-applikation.

* Programmerare (TVE) med flera kanalnätverk

  På så sätt kan programmeraren ge tittarna åtkomst till innehållet från flera kanalnätverk i ett enda TVE-program.

* Programmer (TVE) för specialevent

  På så sätt kan programmeraren ge tittarna åtkomst till innehåll från specialhändelser som kanske inte finns i MVPD tillståndsdatabas, som vanliga kanaler.

| **Fas** | **Prioritet** | **Använd skiftläge** | **Dokument** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Autentisering** | **Hög** | Autentisering | Mer information finns i de dokument som lagts samman under avsnittet [Autentiseringsfas](#authentication-phase). |
|                      | **Hög** | Hembaserad autentisering (HBA) | Mer information finns i [Hembaserad autentisering](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md). |
|                      | **Hög** | enkel inloggning (SSO) | Mer information finns i de dokument som sammanställts under avsnittet [enkel inloggning (SSO)](#sso). |
|                      | **Hög** | Välj MVPD | Mer information finns i de dokument som lagts samman under avsnittet [Konfigurationsfas](#configuration-phase). |
|                      | **Medium** | Inloggningssida för MVPD | MVPD-program kan förse inloggningssidor med varumärken som är specifika för programmeraren eller tjänsteleverantören, inklusive stöd för standardspråkinställningar. |
|                      | **Hög** | Konfigurera TTL-värden per plattform | Mer information finns i [Användarhandboken för integrering av TVE-instrumentpanelen](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows). |
| **Förhandsauktorisering** | **Låg** | Preauktorisering (Preflight-auktorisering) | Mer information finns i de dokument som lagts samman under avsnittet [Förhandsauktoriseringsfas](#preauthorization-phase). |
|                      | **Medium** | Förbättrade felkoder | Mer information finns i [Förbättrade felkoder](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
| **Behörighet** | **Hög** | Behörighet | Mer information finns i de dokument som lagts samman under avsnittet [Auktoriseringsfas](#authorization-phase). |
|                      | **Hög** | Distinkt kanalauktorisering | Gör det möjligt för användare att komma åt innehåll från flera kanalnätverk i ett enda TVE-program. Programmerarna kan göra kanalspecifika behörighetsanrop för att verifiera berättigandet. |
|                      | **Låg** | Auktorisering på tillgångsnivå | Gör det möjligt för distributörer av videoprogrammeringstjänster att samla in detaljerade analyser för enskilda innehållsresurser under auktoriseringen. |
|                      | **Medium** | Förbättrade felkoder | Mer information finns i [Förbättrade felkoder](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
|                      | **Hög** | Programmer Federated Player - med behörighet på sidnivå | Mer information finns i [Medietoken](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Medium** | Programmer Federated Player - med intern spelarbehörighet | Mer information finns i [Medietoken](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Hög** | Syndikerad spelare - på MVPD Portal med behörighet på sidnivå | Mer information finns i [Medietoken](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Låg** | Föräldrakontroll - innehållsklassificeringar i auktoriseringsbegäranden | Gör att Programmeraren kan inkludera innehållsklassificeringar som en del av auktoriseringsbegäran till MVPD som är användbara för auktorisering på tillgångsnivå. |
|                      | **Låg** | Föräldrakontroll - innehållsfiltrering baserad på användarattribut | Gör att programmeraren kan kontrollera den högsta tillåtna innehållsklassificeringen för en användare och filtrera det tillgängliga innehållet därefter. |
| **Logga ut** | **Medium** | Utloggning | Mer information finns i de dokument som lagts samman under avsnittet [Utloggningsfas](#logout-phase). |

## Tillståndsflöde {#entitlement-flow}

Berättigandeflödet är en serie steg som ett programmeringsprogram (TVE) måste slutföra för att strömma skyddat innehåll. Flödet består av följande faser:

* [Registreringsfas](#registration-phase)
* [Konfigurationsfas](#configuration-phase)
* [Autentiseringsfas](#authentication-phase)
* [(Valfritt) Förhandsauktoriseringsfas](#preauthorization-phase)
* [Auktoriseringsfas](#authorization-phase)
* [Utloggningsfas](#logout-phase)

Vid en användares första besök i ett program (TVE) följer tillståndsflödet den angivna ordningen. Vid efterföljande besök kan programmet dock kringgå vissa steg baserat på status för registreringen eller autentiseringen och de tillämpliga visningsprofilerna.

Om du vill ha en detaljerad genomgång av tillståndsflödet och dess faser kan du fortsätta läsa det här dokumentet och sedan läsa de medföljande riktlinjerna för cookbook för ytterligare information:

* [REST API V2 Cookbook (klient-till-server)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [REST API V2 Cookbook (Server-to-Server)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> Programmeringsprogram (TVE) används i det här dokumentet för att tillsammans hänvisa till de typer av program som körs på olika plattformar (webbläsare, mobila enheter, tv-anslutna enheter osv.) som stöds av Adobe Pass Authentication.

### Registreringsfas {#registration-phase}

Syftet med registreringsfasen är att registrera klientprogrammet mot Adobe Pass-autentisering genom processen [Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

DCR-processen (Dynamic Client Registration) kräver att klientprogrammet hämtar ett par klientautentiseringsuppgifter och en åtkomsttoken som slutmål för registreringsfasen.

**API:er**

* [Hämta klientautentiseringsuppgifter](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Hämta åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flöden**

* [Dynamiskt klientregistreringsflöde](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**Vanliga frågor**

* [Vanliga frågor om registreringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general).

### Konfigurationsfas {#configuration-phase}

Syftet med konfigurationsfasen är att ge klientprogrammet en lista över de MVPD-program som det är aktivt integrerat med tillsammans med konfigurationsinformation som sparats av Adobe Pass Authentication för varje MVPD.

Konfigurationsfasen fungerar som ett nödvändigt steg för autentiseringsfasen när klientprogrammet måste be användaren att välja sin TV-leverantör.

**API:er**

* [Hämta konfiguration för en viss tjänsteleverantör](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**Vanliga frågor**

* [Vanliga frågor om konfigurationsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general).

>[!TIP]
>
> TVE-programmet bör innehålla ett MVPD-gränssnitt så att användarna enkelt kan identifiera och välja sin TV-leverantör.

### Autentiseringsfas {#authentication-phase}

Syftet med autentiseringsfasen är att ge klientprogrammet möjlighet att verifiera användarens identitet med MVPD och få information om användarens metadata.

Autentiseringsfasen fungerar som ett nödvändigt steg för förauktoriseringsfasen eller auktoriseringsfasen när klientprogrammet behöver spela upp innehåll.

Lyckad autentisering genererar en profil som är kopplad till programmet, enheten och tjänstleverantören och som även innehåller information om användarens metadata.

**Steg på hög nivå**

Följande steg beskriver de övergripande stegen i händelse av en SAML-integrering:

1. **Programmerarens programinläsning (webbplats)**\
   Användaren navigerar till programmerarens program (webbplats), som integrerar Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Begäran om skyddat innehåll**\
   När användaren försöker få åtkomst till skyddat innehåll visas en lista med MVPD:er som användaren kan välja mellan.

1. **Initiering av autentiseringsbegäran**\
   När MVPD har valts omdirigeras användaren till en Adobe Pass-autentiseringsserver. Här genereras en krypterad SAML-autentiseringsbegäran för den valda MVPD om en SAML-integrering används. Denna begäran skickas för programmerarens räkning till MVPD. Beroende på vilket MVPD-system som används, dirigeras användarens webbläsare antingen till inloggningssidan för MVPD eller så bäddas en iFrame-inloggningssida in i programmerarens program.

1. **MVPD-inloggning**\
   MVPD godkänner begäran och presenterar inloggningsgränssnittet, antingen via redirect eller iFrame.

1. **Användarinloggning och validering**\
   Användaren loggar in med sina MVPD-uppgifter. MVPD validerar användarens prenumerationsstatus och skapar en egen HTTP-session.

1. **MVPD-svar på Adobe Pass-autentisering**\
   När valideringen är klar genererar MVPD ett SAML-svar (krypterat) och skickar tillbaka det till Adobe Pass Authentication.

1. **Profilgenerering**\
   Adobe Pass Authentication verifierar SAML-svaret, genererar en användarprofil som cachelagras och dirigerar om användaren tillbaka till programmerarens program (webbplats).

**API:er**

* [Skapa autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Återuppta autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Hämta autentiseringssession](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Utför autentisering i användaragent](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Hämta profiler](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Hämta profil för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Hämta profil för specifik kod](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Flöden**

* [Grundläggande autentiseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Grundläggande autentiseringsflöde som utförs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om autentiseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

>[!TIP]
>
> TVE-programmet bör förmedla användarens autentiseringsstatus tydligt, till exempel genom att visa sin MVPD-logotyp tillsammans med&quot;låsta&quot; eller&quot;olåsta&quot; ikoner för att indikera tillgängligheten för skyddat innehåll.

#### enkel inloggning (SSO) {#single-sign-on}

**API:er**

* [Hämta partnerautentiseringsbegäran](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Skapa och hämta profil med partnerautentiseringssvar](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**Flöden**

* [Samlad inloggning med partnerflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [Samlad inloggning med plattformsidentitetsflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [Enkel inloggning med tjänsttoken-flöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### (Valfritt) Förhandsauktoriseringsfas {#preauthorization-phase}

Syftet med förauktoriseringsfasen är att ge klientprogrammet möjlighet att presentera en delmängd av resurser från sin katalog som användaren skulle ha rätt till.

Fas för förhandsauktorisering kan förbättra användarupplevelsen när användaren öppnar klientprogrammet för första gången eller navigerar till ett nytt avsnitt.

**API:er**

* [Hämta förauktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flöden**

* [Grundläggande förauktoriseringsflöde som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om förauktoriseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

>[!TIP]
>
> TVE-programmet bör tydligt skilja mellan begränsat innehåll och godkänt innehåll genom att använda visuella indikatorer, t.ex. en&quot;låsikon&quot; för begränsat innehåll och en&quot;olåst&quot;-ikon för godkänt innehåll.

### Auktoriseringsfas {#authorization-phase}

Syftet med auktoriseringsfasen är att ge klientprogrammet möjlighet att spela upp resurser som användaren begär efter att ha verifierat sina rättigheter med MVPD.

Godkänd auktorisering genererar ett beslut som även innehåller en medietoken som tillhandahålls till programmerprogrammet (TVE) av säkerhetsskäl.

**Steg på hög nivå**

I följande steg beskrivs de högnivåsteg:

1. **Hantering av resursidentifierare**\
   Det skyddade innehållet identifieras av en [resursidentifierare](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) som kan vara en enkel sträng eller en mer komplex struktur. Identifieraren är fördefinierad och överenskommen av Programmer och MVPD. Programmerarens program skickar resursidentifieraren till Adobe Pass-autentiseringen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **MVPD Authorization Check**\
   Adobe Pass autentiseringsserver kommunicerar med MVPD autentiseringsslutpunkt med hjälp av standardiserade protokoll.

1. **MVPD-svar på Adobe Pass-autentisering**\
   När valideringen är klar bekräftar MVPD att användaren har rätt (eller inte) att få tillgång till innehållet och skickar ett svar tillbaka till Adobe Pass Authentication.

1. **Generering av beslut och medietoken**\
   Adobe Pass Authentication verifierar svaret, genererar ett [beslut](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) som cachelagras och returnerar beslutet som innehåller en medietoken till programmerarens program (webbplats).

1. **Verifiering av innehållsåtkomst**\
   Programmerarens program använder [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) för att bekräfta att rätt användare använder rätt innehåll. När den har validerats får användaren åtkomst att visa det skyddade innehållet.

**API:er**

* [Hämta auktoriseringsbeslut](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flöden**

* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om auktoriseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

>[!TIP]
>
> TVE-programmet bör tydligt skilja mellan begränsat innehåll och godkänt innehåll genom att använda visuella indikatorer, t.ex. en&quot;låsikon&quot; för begränsat innehåll och en&quot;olåst&quot;-ikon för godkänt innehåll.

### Utloggningsfas {#logout-phase}

Syftet med utloggningsfasen är att ge klientprogrammet möjlighet att avsluta användarens autentiserade profil inom Adobe Pass-autentisering på användarens begäran.

**API:er**

* [Initiera utloggning för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flöden**

* [Grundläggande utloggningsflöde som har utförts i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**Vanliga frågor**

* [Vanliga frågor om utloggningsfas](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

#### Enkel utloggning (SLO) {#single-logout}

**Flöden**

* [Enkelt utloggningsflöde](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

## Förstå berättiganden {#understanding-entitlements}

Adobe Pass lösning för autentisering handlar om att skapa berättiganden - specifika datadelar som genereras när autentiserings- och auktoriseringsarbetsflödena har slutförts. Dessa rättigheter ger åtkomst till skyddat innehåll men har en begränsad livslängd. När ett berättigande upphör att gälla måste det förnyas genom att autentiserings- eller auktoriseringsprocesserna återupptas.

Mer information om berättiganden finns i följande dokument:

* **Profiler**

  När autentiseringen är klar skapar Adobe Pass Authentication en autentiserad profil (&quot;långvarig&quot;) som är kopplad till det program, den enhet och den tjänsteleverantörsidentifierare som begär autentisering (begärande-ID).

* **[Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  När autentiseringen är klar (och i vissa fall även efter behörigheten) får Adobe Pass Authentication in användarmetadata från MVPD som kan visa dem för det begärande programmet.

* **[Beslut](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  När auktoriseringen är klar skapar Adobe Pass Authentication ett auktoriseringsbeslut (&quot;långlivad&quot;) som är kopplat till det begärande programmet, enheten, tjänstleverantörens identifierare (beställarens identifierare) och en specifik skyddad resurs (resursidentifierare).

* **[Medietoken](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  När auktoriseringen är klar skapar Adobe Pass Authentication en medietoken (&quot;kortlivad&quot;) som är associerad med en lyckad spelbegäran och ger stöd för branschens bästa metoder för att minska bedrägerier (t.ex. direktuppspelning).

TTL-värdena (time-to-live) för profiler och beslut baseras på avtal mellan programmerare och leverantörer av betal-TV, som är överens om ett värde som passar alla bäst.
