---
title: Autentisering med OAuth 2.0-protokollet
description: Autentisering med OAuth 2.0-protokollet
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# Autentisering med OAuth 2.0-protokollet

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Även om SAML fortfarande är det viktigaste protokollet som används för autentisering av amerikanska distributörer och företag i allmänhet, finns det en tydlig trend mot att övergå till OAuth 2.0 som det primära autentiseringsprotokollet. OAuth 2.0-protokollet (https://tools.ietf.org/html/rfc6749) utvecklades främst för konsumentsajter och antogs snabbt av internetgianter som Facebook, Google &amp; Twitter.

OAuth 2.0 är oerhört framgångsrik och detta har lett till att företag långsamt har uppgraderat sin infrastruktur för att stödja den.



## Fördelar med att gå över till OAuth 2.0 {#adv-oauth2}

På en hög nivå erbjuder OAuth 2.0-protokollet samma funktioner som SAML-protokollet, men det finns vissa viktiga skillnader.

Ett av detta är det faktum att uppdateringstokenflödet kan användas som ett sätt att uppdatera autentiseringen bakom kulisserna. Detta gör att IdP (MVPD:er i det här fallet) kan behålla kontrollen samtidigt som en bra användarupplevelse tillåts eftersom användaren inte längre behöver logga in ofta på grund av säkerhetsproblem.

Protokollet ger också större flexibilitet när det gäller de data som exponeras som en tjänsteleverantör kan nu använda tokens för att komma åt andra API:er för att få extra information. Detta resulterar i sin tur i ett&quot;chattier&quot;-protokoll för TVE-användningsfall, men ger den flexibilitet som behövs för komplexa arbetsflöden.





## Krav för att byta till OAuth 2.0 {#oauth-req}

För att stödja autentisering med OAuth 2.0 måste MVPD uppfylla följande krav:

Först och främst måste MVPD se till att det stöder flödet [Authorization Code Grant](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html) .

När MVPD har bekräftat att det stöder flödet måste vi få följande information:

* slutpunkten för autentisering
   * slutpunkten kommer att tillhandahålla auktoriseringskoden som senare kommer att användas i utbyte mot uppdaterings- och åtkomsttoken
* slutpunkten för /token
   * detta ger en uppdateringstoken och åtkomsttoken
   * uppdateringstoken måste vara stabil (den får inte ändras varje gång vi begär en ny åtkomsttoken
   * MVPD måste tillåta flera aktiva åtkomsttoken för varje uppdateringstoken
   * den här slutpunkten utbyter även en uppdateringstoken för en åtkomsttoken
* vi behöver en **slutpunkt för användarprofilen**
   * den här slutpunkten tillhandahåller användar-ID, som måste vara unikt för ett konto och inte ska innehålla någon personligt identifierbar information
* **/utloggning** slutpunkt (valfritt)
   * Adobe Pass Authentication kommer att dirigera om till denna slutpunkt, tillhandahålla en omdirigerings-URI till MVPD. På denna slutpunkt kan MVPD rensa cookies på klientdatorn eller tillämpa önskad logik för utloggning
* vi rekommenderar starkt att du har stöd för auktoriserade klienter (klientappar som inte utlöser någon användarauktoriseringssida)
* vi kommer också att behöva:
   * **clientID** och **klienthemlighet** för integreringskonfigurationerna
   * **tid till live**-värden (TTL) för uppdateringstoken och åtkomsttoken
   * Vi kan förse MVPD med en URI för auktoriseringsåteranrop och inloggningsåteranrop. Vid behov kan vi även tillhandahålla en lista över IP-adresser som ska vitlistas i brandväggsinställningarna.


## Autentiseringsflöde {#authn-flow}

I autentiseringsflödet kommunicerar Adobe Pass Authentication med MVPD om det protokoll som valts i konfigurationen. OAuth 2.0-flödet visas i bilden nedan:



![Diagram som visar autentiseringsflödet i Adobe-autentisering som kommunicerar med MVPD på det protokoll som valts i konfigurationen.](/help/authentication/assets/authn-flow.png)

**Figur 1: OAuth 2.0-autentiseringsflöde**



## Autentiseringsbegäran och svar {#authn-req-response}

I korthet följer autentiseringsflödet för MVPD-program som stöder protokollet OAuth 2.0 följande steg:

1. Slutanvändaren navigerar till Programmerarens webbplats och väljer att logga in med sina MVPD-uppgifter
1. AccessEnabler installerades på programmerarens sida med en autentiseringsbegäran i form av en HTTP-begäran till Adobe Pass Authentication-slutpunkten, som Adobe Pass Authentication-slutpunkten omdirigerar till MVPD-auktoriseringsslutpunkten.
1. MVPD-slutpunkten för auktorisering skickar en auktoriseringskod till Adobe Pass-slutpunkten för autentisering
1. Adobe Pass Authentication använder den auktoriseringskod som tas emot för att begära en uppdateringstoken och en åtkomsttoken från MVPD tokenslutpunkt
1. Ett anrop om att hämta användarinformation och metadata kan skickas till användarprofilens slutpunkt om användarinformationen inte ingår i token
1. Autentiseringstoken skickas till slutanvändaren som nu kan bläddra på webbplatsen Programmer

   >[!NOTE]
   >
   >Uppdateringstoken används för att hämta en ny åtkomsttoken när den aktuella åtkomsttoken har blivit ogiltig eller går ut.


>[!IMPORTANT]
>
>Uppdateringstoken får inte ändras när den byts ut mot en åtkomsttoken.

Den här begränsningen beror på klientflödena som inte tillåter att servern uppdaterar AuthNToken, som för OAuth 2.0-protokollet även innehåller uppdateringstoken.

Ett typiskt auktoriseringsflöde utför ett utbyte av den uppdateringstoken som sparats i AuthNToken för en åtkomsttoken som sedan används för att utföra auktoriseringsanropet i namnet på den användare som autentiserades först. Om auktoriseringsservern (MVPD) skulle ändra uppdateringstoken och ogiltigförklara den gamla, kommer vi inte att kunna uppdatera den giltiga AuthNToken. Därför måste programmeringsgränssnitten ha stöd för stabila uppdateringstoken för att kunna konfigurera OAuth 2.0-integreringar för dem.


## Migrerar från SAML till OAuth 2.0 {#saml-auth2-migr}

Migrering av integreringar från SAML till OAuth 2.0 kommer att utföras av Adobe och MVPD. Programmeraren behöver inte göra några tekniska ändringar, även om programmeraren kanske vill kontrollera/testa sammärkesfunktionen på MVPD inloggningssida. Ur MVPD synvinkel krävs slutpunkterna och annan information som efterfrågas i Oauth 2.0-kraven.

För att **bevara enkel inloggning** kommer de användare som redan har en autentiseringstoken som hämtats via SAML fortfarande att betraktas som autentiserade och deras förfrågningar dirigeras via den gamla SAML-integreringen.

Ur ett tekniskt perspektiv:

1. Adobe möjliggör en OAuth 2.0-integrering mellan programmeraren och MVPD, UTAN att SAML-integreringen tas bort.
1. Efter aktiveringen kommer alla nya användare att använda OAuth 2.0-flöden.
1. Användare som redan har autentiserats och som redan har en lokal AuthN-token som innehåller SAML subject-id, dirigeras automatiskt av Adobe via SAML-integreringen.
1. För användare i steg 3 kommer Adobe att behandla dem som nya användare och uppföra sig som användare i steg 2 när deras SAML-genererade AuthN-token upphör att gälla.
1. Adobe kommer att granska användningsmönstren för att avgöra när SAML-integreringen kan inaktiveras på ett säkert sätt.
