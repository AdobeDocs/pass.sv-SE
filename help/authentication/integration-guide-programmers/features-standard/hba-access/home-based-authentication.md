---
title: Hembaserad autentisering (HBA)
description: Hembaserad autentisering (HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# Hembaserad autentisering (HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

HBA (Home-Based Authentication) är en TV Everywhere-funktion som gör det möjligt för betal-TV-prenumeranter att få åtkomst till TV-innehåll online utan att ange MVPD-autentiseringsuppgifter när de är anslutna till hemnätverket, vilket avsevärt förbättrar autentiseringsprocessen.

Enligt Open Authentication Technology Committee (OATC):

> &quot;Automatisk autentisering hemma är den process genom vilken en MVPD/OVD använder hemnätverkets egenskaper (eller identifierare som automatiskt är tillgängliga mellan enheter i hemnätverket) för att autentisera vilket abonnentkonto som är associerat med hemnätverket, vilket eliminerar behovet av att användare anger inloggningsuppgifter manuellt när en TVE-session initieras för att få åtkomst till skyddat innehåll.&quot;

Mer information om HBA (Home-Based Authentication) och relevanta branschstandarder finns i följande resurser:

>[!MORELIKETHIS]
>
> * [Direktåtkomst (HBA) via CTAM](http://www.ctamtve.com/instantaccess)
> * [Hembaserad autentisering - användningsfall och krav från OATC](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * [Hembaserad autentiseringsinformation från Adobe](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)
> * [Autentisering med OAuth2-aktiverade MVPD:er](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [Autentisering med SAML aktiverade MVPD:er](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## HBA-fördelar {#hba-benefits}

HBA (Home-Based Authentication) är en nyckelfunktion som tar bort inloggningsbarriären för tittare hemma med en aktiv kabelprenumeration. Detta hinder är en stor utmaning för TV Everywhere-tjänsterna, där nästan hälften av alla inloggningsförsök leder till fel.

HBA kan förbättra tittarinteraktionen avsevärt, vilket ger en smidig och överlägsen användarupplevelse när det gäller att få åtkomst till TV Everywhere-innehåll.

## Stöd för HBA {#hba-support}

>[!IMPORTANT]
>
> Om du är intresserad av att utnyttja HBA-funktioner kan du kontakta din säljare av Adobe Pass Authentication för mer information eftersom vissa HBA-flöden ingår i Premium Workflow-paketet.

HBA stöds av ett antal MVPD-program som är integrerade med Adobe Pass Authentication, men för att kunna utnyttja HBA måste du kanske vidta ytterligare åtgärder.

**SAML MVPDs**

För SAML MVPD aktiveras HBA endast på MVPD.

**OAuth2 MVPDs**

För OAuth2 MVPD-program kan HBA aktiveras och inaktiveras via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) genom att följa stegen i användarhandboken för [TVE Dashboard Integrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

### MVPD {#hba-support-mvpds}

**SAML MVPDs**

I följande tabell visas en översikt över de SAML-aktiverade MVPD-programmen som stöder HBA:

| MVPD | Tillgängliga grundläggande funktioner? | Skickar flagga vid autentiseringssvar? |
|--------------|--------------------------------|----------------------------------------|
| DirectTV | Ja | Nej |
| Danska | Ja | Nej |
| Spektrum | Ja | Ja |
| Cox | Ja | Nej |
| AT&amp;T | Ja | Nej |
| Verizon | Ja | Ja |
| Cablevision | Ja | Nej |
| Mediacom | Ja | Nej |
| Midkontinent | Ja | Nej |
| Massilon | Ja | Nej |
| AlticeOne | Ja | Ja |

**OAuth2 MVPDs**

I följande tabell visas en översikt över OAuth2-aktiverade MVPD-filer som stöder HBA:

| MVPD | Tillgängliga grundläggande funktioner? | Skickar flagga vid autentiseringssvar? |
|---------|--------------------------------|----------------------------------------|
| Comcast | Ja | Ja |

### Adobe Pass-autentisering {#hba-support-adobe-pass-authentication}

I det här avsnittet beskrivs HBA-aktiverad upplevelse och det finns information om stödet i Adobe Pass Authentication. Viktiga funktioner som:

* **Identifiering av värdbussadapter:** Möjlighet att ange för programmerare om autentiseringen var värdbussadapter eller icke-värdbussadapter (kräver stöd från MVPD).
* **Konfigurerbara autentiserings-TTL:er:** Möjlighet att ange olika autentiseringsvärden för TTL (Time-To-Live) för HBA jämfört med icke-HBA-autentiseringar (kräver MVPD-stöd).

Följande tabell ger en översikt på hög nivå över användarupplevelsen i ett HBA-autentiseringsflöde och det reguljära autentiseringsflödet (ej HBA):

| Autentiseringstyp | Användarupplevelse |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | Användarna väljer sin MVPD och autentiseras automatiskt när de är anslutna till sitt hemnätverk. När autentiseringen går ut måste användarna välja sin MVPD igen, varefter de automatiskt autentiseras. |
| Normal (icke-värdbussadapter) | Användarna väljer sin MVPD och uppmanas att ange sina inloggningsuppgifter oavsett anslutning till ett hemnätverk. När autentiseringen går ut måste användarna återvälja sin MVPD och ange sina inloggningsuppgifter igen. |

**SAML MVPDs**

I följande tabell ges en översikt över HBA-implementeringen om SAML-aktiverade MVPD-program är aktiverade:

| Användaråtgärder | Systemåtgärder |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Användaren försöker spela upp en video.<br/><br/>MVPD-väljaren visas.<br/><br/>Användaren väljer sin MVPD och fortsätter att logga in. | En bakgrundskontroll utförs där MVPD tillämpar sina regler för användaridentifiering. Detta kan till exempel innebära att användarens IP-adress mappas till MAC-adressen för distributörsetablerade modem eller bredbandsanslutna digitalboxar. |
| En skärm som varar i cirka 3 sekunder visas.<br/><br/>En interaktiv sida kan informera användaren om att han/hon loggas in automatiskt med sitt MVPD-konto. | Programmet öppnar autentiserings-URL:en i en användaragent och initierar en HTTP-begäran till Adobe Pass Authentication-slutpunkten.<br/><br/>Slutpunkten för Adobe Pass-autentisering vidarebefordrar begäran till slutpunkten för MVPD-autentisering via en omdirigering av användaragent.<br/><br/>MVPD förväntas skicka ett autentiseringsbeslut i form av ett SAML-svar som innehåller HBA-flaggan (`hba_status`) med värdet &quot;true&quot; eller &quot;false&quot;.<br/><br/>Adobe Pass-autentiseringsbackend skickar en begäran till MVPD-användarprofilens slutpunkt om att `hba_status`-flaggan ska visas som en del av [användarens metadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md). |
| Användaren är autentiserad och kan nu bläddra bland det berättigade TV Everywhere-innehållet. | Programmet hämtar användarprofilen och kan fortsätta med beslutsflödet för att spela upp innehållet. |

**OAuth2 MVPDs**

I följande tabell ges en översikt över HBA-implementeringen om OAuth2-aktiverade MVPD-program är aktiverade:

| Användaråtgärder | Systemåtgärder |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Användaren försöker spela upp en video.<br/><br/>MVPD-väljaren visas.<br/><br/>Användaren väljer sin MVPD och fortsätter att logga in. | En bakgrundskontroll utförs där MVPD tillämpar sina regler för användaridentifiering. Detta kan till exempel innebära att användarens IP-adress mappas till MAC-adressen för distributörsetablerade modem eller bredbandsanslutna digitalboxar. |
| En skärm som varar i cirka 3 sekunder visas.<br/><br/>En interaktiv sida kan informera användaren om att han/hon loggas in automatiskt med sitt MVPD-konto. | Programmet öppnar autentiserings-URL:en i en användaragent och initierar en HTTP-begäran till Adobe Pass Authentication-slutpunkten.<br/><br/>Slutpunkten för Adobe Pass-autentisering vidarebefordrar begäran till slutpunkten för MVPD-autentisering via en omdirigering av användaragent.<br/><br/>Slutpunkten för MVPD-autentisering skickar en auktoriseringskod till slutpunkten för Adobe Pass-autentisering.<br/><br/>Adobe Pass-autentisering använder auktoriseringskoden för att begära en uppdateringstoken och en åtkomsttoken från MVPD tokenslutpunkt.<br/><br/>MVPD förväntas skicka ett autentiseringsbeslut som innehåller HBA-flaggan (`hba_status`) med värdet &quot;true&quot; eller &quot;false&quot; som en del av `id_token`.<br/><br/>Adobe Pass-autentiseringsbackend skickar en begäran till MVPD-användarprofilens slutpunkt om att `hba_status`-flaggan ska visas som en del av [användarens metadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).<br/><br/>MVPD ställer in TTL för uppdateringstoken till ett värde som MVPD har godkänt och Adobe ställer in TTL för autentisering till ett värde som är mindre eller lika med värdet för uppdateringstoken. |
| Användaren är autentiserad och kan nu bläddra bland det berättigade TV Everywhere-innehållet. | Programmet hämtar användarprofilen och kan fortsätta med beslutsflödet för att spela upp innehållet. |

>[!IMPORTANT]
>
> I HBA-flödet för MVPD-program som använder OAuth 2.0-autentiseringsprotokollet utfärdar MVPD en uppdateringstoken med en TTL som definieras av företagets affärskrav, medan Adobe utfärdar en HBA-autentiseringstoken med en TTL som inte får överskrida uppdateringstokens TTL.

## Vanliga frågor {#faqs}

1. Varför är det skillnad på HBA-implementering för SAML- och OAuth2-protokoll?

   Separationen mellan HBA (Home-Based Authentication) för SAML- och OAuth2-protokoll finns eftersom dessa protokoll fungerar annorlunda när det gäller autentiseringsmekanismer, konfiguration och implementeringsflexibilitet. För SAML MVPD krävs ingen åtgärd från programmeraren för att aktivera HBA, medan HBA för OAuth2 MVPD kan aktiveras eller inaktiveras via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

1. När HBA är aktiverat, behöver användare fortfarande ange sitt användarnamn och lösenord under den första autentiseringen?

   Nej, användarnamn och lösenord krävs inte.

1. Hur kan ni genomdriva föräldrakontroll?

   Adobe Pass Authentication kan inaktivera värdbussadaptern för integrering med kanaler som kräver godkännande av föräldrakontroll. Vi arbetar också med OATC i ett UX-dokument som rekommenderar hur man ställer in HBA-upplevelsen med föräldrakontroll.

1. Använder leverantörer som stöder HBA kortare TTL-fönster för HBA jämfört med vanlig autentisering?

   TTL-inställningen kan konfigureras. Vi rekommenderar att du ställer in en kortare TTL för MVPD-integrering med HBA för att förhindra felhantering.
