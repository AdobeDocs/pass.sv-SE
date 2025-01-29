---
title: Hembaserad autentisering för TV överallt
description: Hembaserad autentisering för TV överallt
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---

# Hembaserad autentisering för TV överallt

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Vad är hembaserad autentisering? {#whatis-home-based-authn}

HBA (Home Based Authentication) är en TV Everywhere-funktion som gör det möjligt för betal-TV-prenumeranter att visa tv-innehåll online utan att ange MVPD-inloggningsuppgifter när de är hemma, vilket avsevärt förbättrar användarupplevelsen av autentiseringsflödet.

Hembaserad autentiseringsdefinition av OATC (Open Authentication Technology Committee):&quot;Automatisk autentisering hemma är den process genom vilken en MVPD/OVD använder hemnätverkets egenskaper (eller identifierare som automatiskt är tillgängliga mellan enheter i hemnätverket) för att autentisera vilket prenumerantkonto som är kopplat till hemnätverket så att användare inte behöver ange inloggningsuppgifter manuellt när de skapar en TVE-session för åtkomst av TVE-skyddat innehåll.&quot;



Mer information om HBA och branschstandarderna finns i [OATC-användningsfall och -krav](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} och **OATC User Experience Guidelines for HBA**.

>[!NOTE]
>
>Vissa HBA-flöden ingår i Premium Workflow-paketet. Kontakta din Adobe Pass-säljare om du är intresserad av att använda den här funktionen.

## Därför är HBA viktigt för dig {#why-hba}

HBA är viktigt eftersom det praktiskt taget tar bort inloggningsbarriären för tittarna som är hemma och redan har en kabelprenumeration. Hembaserad autentisering kan också avsevärt öka tittarnas engagemang och ge en bättre användarupplevelse för ert TV Everywhere-innehåll.

För närvarande lyckas nästan hälften av försöken att logga in inte.

När värdbussadaptern aktiverades av en av de fem vanligaste versionerna av dokumentationsprofilerna ökade autentiseringskonverteringen **med 40 %** (från 45 % till 63 %)

![](../../../assets/authn-conv-pre-post.png)

Nedan ser du också inloggningskonverteringsgraden för en kanal som är integrerad med olika MVPD-program: de som har aktiverat HBA för den och de som inte har HBA. Konverteringsgraden för dem med HBA är betydligt högre än för dem utan HBA.

![](../../../assets/hba-vs-non-hba.png)

Sex månader efter att vi aktiverat HBA för de flesta av kanalerna som är integrerade med denna MVPD har vi märkt en 82-procentig ökning av antalet unika användare (antalet användare som använder TV Everywhere-kanaler via denna MVPD nästan fördubblats).

2w3Som du kan se i diagrammet nedan så har andra videoprogrammeringsprogram som inte hade aktiverat HBA bara ökat med 26 % bland de unika användarna under de senaste sex månaderna.

![](../../../assets/unique-visitors-incr.png)

Från våra data, som samlats in 6 månader före och 6 månader efter att HBA aktiverats, har vi sett en stor ökning av tittarnas engagemang för de kanaler som var HBA-aktiverade. Praktiskt taget alla användare från MVPD som har aktiverat HBA tenderar att se i genomsnitt 30 % mer innehåll än de som inte har HBA aktiverat.

![](../../../assets/user-engagement-increase.png)

## Stöd för Adobe Pass Authentication HBA {#auth-hba-support}

I det här avsnittet beskrivs HBA-stödet från Adobe Pass Authentication, beteendet hos Adobe Pass Authentication-plattformarna i HBA-flöden och även teknisk information som är användbar vid implementering av HBA.

Adobe Pass autentiseringsfunktioner som stöder HBA

* Möjlighet att ange olika autentiserings-TTL för HBA jämfört med icke-HBA-autentisering (kräver också stöd från MVPD)
* Möjlighet att automatiskt välja en MVPD (hoppa över MVPD-väljaren) om autentiseringen gick ut. Detta är praktiskt särskilt när HBA TTL är små.
* Möjlighet att visa programmerarna om autentiseringen var HBA eller inte (kräver också stöd från MVPD)

### HBA User Experience on Adobe Pass Authentication platforms {#hba-user-exp}

Följande tabeller innehåller information om användarupplevelsen för de plattformar som stöds när HBA är aktiverat och när HBA inte är aktiverat:

| Användarflöde - Plattformstyp | swf, iOS, Android |
|---|---|
| Med HBA aktiverat | När användarna är hemma autentiseras de automatiskt. När HBA AuthN-token har upphört att gälla autentiseras användarna automatiskt. |
| Utan HBA | Användare ombeds att välja sin MVPD och ange sina autentiseringsuppgifter, även om de är hemma. När AuthN-token har upphört att gälla måste användare ange sina autentiseringsuppgifter igen. |

| Användarflöde - Plattformstyp | js, Windows (inbyggt) |
|---|---|
| Med HBA aktiverat | När användarna är hemma autentiseras de automatiskt. När HBA AuthN-token har upphört att gälla måste användaren välja sin MVPD igen från väljaren och autentiseras automatiskt. |
| Utan HBA | Användarna uppmanas att välja sin MVPD och ange sina inloggningsuppgifter, även om de är hemma. När AuthN-token har upphört att gälla måste användaren ange sina inloggningsuppgifter igen. |

| Användarflöde - Plattformstyp | Klientlös REST API (autentisering på andra skärmen) |
|---|---|
| Med HBA aktiverat | När användare är hemma och använder en klientlös REST API-app autentiseras de automatiskt på den andra skärmenheten efter att registreringskoden har angetts och deras MVPD har valts. När HBA AuthN-token upphör att gälla återautentiseras användare automatiskt (på den andra skärmenheten). |
| Utan HBA | Användarna uppmanas att välja sin MVPD och ange sina inloggningsuppgifter, även om de är hemma. När AuthN-token har upphört att gälla måste användaren ange sina inloggningsuppgifter igen. |

### Teknisk information om implementering av värdbussadapter {#tech-details-hba}

#### OAuth 2.0-protokoll {#oauth-2-protocol}

I HBA-flödet för MVPD-program som är integrerade med OAuth 2.0-autentiseringsprotokollet utfärdar MVPD en uppdateringstoken och Adobe utfärdar en HBA-autentiseringstoken:

* Uppdateringstoken har en TTL som bestäms av MVPD affärskrav.
* TTL **för HBA-autentiseringstoken måste vara mindre än eller lika med** TTL för uppdateringstoken.


*Beskrivning av HBA-autentiseringsflödet för OAuth 2.0-protokollet*


| Användaråtgärder | Systemåtgärder |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Användaren navigerar till programmerarens webbplats. När du försöker spela upp en videofilm visas MVPD-väljaren. Användaren väljer sin MVPD och klickar på Logga in. | En bakgrundskontroll utförs. MVPD tillämpar sina regler för användaridentifiering (mappa t.ex. användarens IP-adress till MAC-adressen för modem som tillhandahålls av distributören eller bredbandsanslutna digitalboxar). |
| En skärm som visas i cirka 3 sekunder visas. En interstitiell sida kan visas som informerar användaren om att han/hon automatiskt loggar in med sitt MVPD-konto. | <ol><li>AccessEnabler, som är installerat på programmeraren, skickar en autentiseringsbegäran (som en HTTP-begäran) till slutpunkten för Adobe Pass-autentisering.</li><li>Slutpunkten för Adobe Pass-autentisering dirigerar om begäran till slutpunkten för MVPD-autentisering. <br />**Obs!** Begäran innehåller parametern `hba_flag` (försök med HBA = true) som signalerar att MVPD ska försöka utföra HBA-autentisering.</li><li>MVPD-autentiseringsslutpunkten skickar en auktoriseringskod till Adobe Pass-autentiseringsslutpunkten.</li><li>Adobe Pass Authentication använder auktoriseringskoden för att begära en uppdateringstoken och en åtkomsttoken från MVPD tokenslutpunkt.</li><li>MVPD skickar ett autentiseringsbeslut och parametern `hba_status` (true/false) i `id_token`.</li><li>Ett anrop till slutpunkten för MVPD-användarprofilen skickas för att visa nyckeln [hba_status i användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).</li><li>MVPD ställer in TTL för uppdateringstoken till ett värde som MVPD har godkänt och Adobe ställer in AuthN-token TTL till ett värde som är mindre eller lika med uppdateringstokens värde.</li></ol> |
| Användaren är autentiserad och kan nu bläddra bland det berättigade TV Everywhere-innehållet. | Autentiseringstoken skickas till användaren som nu kan bläddra på programmerarens webbplats. |

#### SAML-protokoll {#saml-protocol}

Beskrivning av HBA-autentiseringsflödet för SAML-autentiseringsprotokollet

| Användaråtgärder | Systemåtgärder |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Användaren navigerar till programmerarens webbplats. När du försöker spela upp en videofilm visas MVPD-väljaren. Användaren väljer sin MVPD och klickar på Logga in. | En bakgrundskontroll utförs. MVPD tillämpar sina regler för användaridentifiering (mappa t.ex. användarens IP-adress till MAC-adressen för modem som tillhandahålls av distributören eller bredbandsanslutna digitalboxar). |
| En skärm som visas i cirka 3 sekunder visas. En interstitiell sida kan visas som informerar användaren om att han/hon automatiskt loggar in med sitt MVPD-konto. | <ol><li>AccessEnabler, som är installerat på programmeraren, skickar en autentiseringsbegäran (som en HTTP-begäran) till slutpunkten för Adobe Pass-autentisering.</li><li>Slutpunkten för Adobe Pass-autentisering dirigerar om begäran till slutpunkten för MVPD-autentisering.</li><li>MVPD bör skicka ett autentiseringsbeslut i form av ett SAML-svar som ska innehålla HBA-flaggan: hba_status (true/false).</li><li>Ett anrop till slutpunkten för MVPD-användarprofilen skickas för att visa nyckeln [hba_status i användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).</li></ol> |
| Användaren är autentiserad och kan nu bläddra bland det berättigade TV Everywhere-innehållet. | Autentiseringstoken skickas till användaren som nu kan bläddra på programmerarens webbplats. |


## Så här aktiverar du HBA {#how-to-activate-hba}

* **OAuth-protokoll:**
   * Information om hur du aktiverar HBA finns i [Användarhandbok för Adobe Pass TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* **SAML-protokoll:** Hembaserad autentisering har aktiverats på MVPD-sidan. Programmeraren eller Adobe kräver inga åtgärder.
Mer information om de MVPD-program som stöder hembaserad autentisering finns i [HBA-status för MVPD-filer](/help/authentication/integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md).

## Vanliga frågor {#faqs}


**Fråga:** Varför separationen mellan Home Based Authentication med SAML- och OAuth2-protokoll?

**Svar:** HBA-flödet skiljer sig åt för de två protokollen. Ur programmerarens perspektiv finns det inget behov av åtgärder för att säkerställa att HBA är aktiverat för SAML MVPD, medan HBA för OAuth2 MVPD kan aktiveras eller inaktiveras i Adobe Pass TVE Dashboard.



**Fråga:** Måste användare fylla i användarnamn och lösenord första gången de autentiserar när HBA är aktiverat?

**Svar:** Nej, användarnamn och lösenord krävs inte.



**Fråga:** Hur tillämpar du föräldrakontroll?

**Svar 1:** Adobe kan inaktivera HBA för integrering med kanaler som behöver godkännande av föräldrakontroll.

**Svar 2:** Adobe arbetar med OATC i ett UX-dokument och rekommenderar hur du konfigurerar HBA-upplevelsen med föräldrakontroller.



**Fråga:** Har leverantörerna som stöder HBA kortare TTL-fönster för HBA än de gör för vanlig autentisering?

**Svar:** TTL-inställningen kan konfigureras. Vi rekommenderar att du anger en kortare TTL för HBA-autentiseringstoken för att förhindra felhantering.


## Användbar information {#useful-info}

* [HBA (Instant Access) Recommendations](http://www.ctamtve.com/instantaccess){target=_blank} - av CTAM
* [Exempel på implementering av HBA i programmerarappen](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346){target=_blank} av Adobe
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* [Hembaserad autentisering - användningsfall och krav](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} av OATC
* [Hembaserad autentiseringsinformation](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640){target=_blank} - av Adobe
* [Autentisering med OAuth 2.0-protokollet](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
* [Autentisering med SAML MVPD](/help/authentication/integration-guide-mvpds/authn-usecase.md)
* [Användarhandbok för Adobe Pass TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* [hba_status Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)
