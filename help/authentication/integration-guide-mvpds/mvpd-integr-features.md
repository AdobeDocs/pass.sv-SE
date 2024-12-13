---
title: MVPD integreringsfunktioner
description: MVPD integreringsfunktioner
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 2%

---

# MVPD integreringsfunktioner

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#mvpd-int-features-overview}

Adobe Pass Authentication stöder de nya standarderna för TV Everywhere. Den är kompatibel med specifikationen **CableLabs OLCA (Online Content Access)** som innehåller tekniska krav och arkitektur för leverans av video till en Pay TV-kund från onlinekällor. Adobe deltog i det gemensamma CableLabs-projektet för interopt-testning i juni 2011 och klarade testprocessen för en implementering av en tjänsteleverantör.  Adobe är också en aktiv medlem av **OATC (Open Authentication Technical Consortium)** och deltar i flera av underkommittéernas projekt för att utarbeta specifikationer som en del av det organet.

Efter att ha beskrivit Adobe Pass Authentication OLCA-kompatibilitet och Adobe-deltagande i OATC är det också viktigt att notera att Adobe Pass Authentication faktiskt är&quot;protokollagnostic&quot;.  Men i det här skedet av TVE-eran är Adobe Pass Authentication definitivt inriktat på OLCA-standarderna.  Standarderna beskriver för närvarande överenskomna sätt för de olika TVE-spelarna (programmerare, programmerare och tjänsteleverantörer) att implementera TVE-funktioner. Många av dessa funktioner listas i tabellerna nedan, med länkar till relaterade sidor som innehåller detaljer och exempel på hur funktionerna ska implementeras.

Tabellerna är avsedda att driva integreringsprocessen för Adobe Pass Authentication mot enhetlig funktionalitet i alla MVPD-integreringar. I prioriteringskolumnen rangordnas funktionerna i A, B och C:

* S - Dessa måste ha funktioner för den första utrullningen av en integrering.
* B - Detta är viktiga förbättringar av den inledande integreringen som ska läggas till efter den första utrullningen.
* C - Detta är ytterligare förbättringar av integreringen som kan implementeras efter B-kraven.


## 1. De viktigaste funktionerna {#core-func-features}


| Nej. | Funktion | Beskrivning | Prioritet | Anteckningar |
|------|---------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [Programmerarinitierad inloggning](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md) | Visningsprogrammet väljer MVPD och initierar autentiseringsflödet (AuthN) från Programmerarens webbplats eller tillämpning. | A+ |                                                                                                                                                          |
| 1,2 | [Kanalbaserad autentisering](/help/authentication/integration-guide-mvpds/authz-usecase.md) | När autentiseringen är klar kan auktoriseringen (AuthZ) ske i bakgrunden, med bara en nätverkskanal-ID och en användaridentifierare skickas till MVPD. | A+ |                                                                                                                                                          |
| 1,3 | UserID-beständighet | MVPD tillhandahåller ett dolt, beständigt användar-ID. | A |                                                                                                                                                          |
| 1,4 | [Stöd för enkel inloggning](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-support.md) | Visningsprogrammet loggar in med MVPD på webbplatsen för ett varumärke och kan sedan gå till ett annat varumärke och inte uppmanas att logga in igen. AuthN-sessionen delas mellan varumärkena. SSO-stöd gäller både webbplatser och mobilapplikationer/enhetsapplikationer.  För MVPD-program krävs att antingen ett användar-ID eller någon annan användartoken som kan användas för AuthZ för olika varumärken returneras. | A |                                                                                                                                                          |
| 1,5 | Enhetsoptimerad användarupplevelse vid inloggning (UX) | MVPD har stöd för att ändra storlek på inloggningsskärmen så att den passar dimensionerna för den enhet som vyn används på. | A |                                                                                                                                                          |
| 1,6 | Standardlogotyp för MVPD-väljare | MVPD tillhandahåller en URL till en standardlogotyp med lämpliga dimensioner (112x33 pixlar). | A | MVPD bör vara värd för logotypen och den bör cachas av CDN. |
| 1,7 | [Tjänstleverantörsomfång](/help/authentication/integration-guide-mvpds/serv-provider-scoping.md) | MVPD stöder överföring av varumärkesidentifieraren (Requestor value) i AuthN-begäran. | A- | Detta aktiverar en tjänsteleverantörsspecifik inloggningsfunktion. |
| 1,8 | [Auktorisering för flera kanaler](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight-multich-authz) | MVPD har stöd för en programmerare som skickar flera kanaler i en enda auktoriseringsbegäran. | B+ |                                                                                                                                                          |
| 1,9 | iFrame- eller JS Pop-up-baserad autentisering | Programmeraren kan integrera inloggningsflödet i en iFrame- eller popup-upplevelse i stället för en HTTP-omdirigering. | B |                                                                                                                                                          |
| 1,10 | [Programmerarinitierad utloggning](/help/authentication/integration-guide-mvpds/usecase-mvpd-logout.md) | Visningsprogrammet har en autentiserad session både på Programmerarens webbplats eller i appen och med MVPD. Visningsprogrammet kan initiera en federerad utloggning från Programmerarens webbplats och utloggningen rensar även sessionen på MVPD-portalen. | B | Ser till att delade datorer skyddas mot missbruk ur programmeringsperspektivet. |
| 1,11 | [Felmeddelanden för anpassad auktorisering](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) | MVPD skickar en egen felsträng som är lämplig för programmerarens externa webbplats eller app som ska visas för användaren. | B | Aktiverar merförsäljningsscenarier |
| 1,12 | **Användarmetadata i autentiseringssvar** | MVPD AuthN-svaret kan innehålla användarmetadata som fungerar som tips för personalisering av användarens upplevelse under tillståndsflödet. Detta krav möjliggör tips om föräldrakontroll från MVPD till programmeraren. | B- |                                                                                                                                                          |
| 1,13 | MVPD-initierad autentisering | Visningsprogrammet slutför en AuthN-session på MVPD-portalen och navigerar sedan till Programmerarens TV-webbplats. Användaren uppmanas inte att ange MVPD-väljaren och autentiseras automatiskt. | B- |                                                                                                                                                          |
| 1,14 | UserID-omfång | MVPD användar-ID bör finnas i två former - en för programmerare och en för hela Adobe för bedrägeri.  På så sätt kan Adobe dela det programmeraromfattande MVPD UserID utan ytterligare kryptering/krånglighet. | C |                                                                                                                                                          |
| 1,15 | Tillgångsbaserad auktorisering | När AuthN har slutförts kan AuthZ inträffa i bakgrunden genom att skicka strukturerade data som kan innehålla nätverk, bildspel, resurser, klassificeringar av föräldrakontroll och mycket mer efter behov. Detta aktiverar föräldrakontroll för alla AuthZ-anrop från Programmer till MVPD. | C |                                                                                                                                                          |
| 1,16 | MVPD-initierad utloggning | Visningsprogrammet har en autentiserad session både på Programmerarens webbplats eller i appen och med MVPD. Visningsprogrammet kan initiera en federerad utloggning från MVPD webbplats som även rensar sessionen på alla externa programmerarwebbplatser. | C |                                                                                                                                                          |
| 1,17 | Auktoriseringsskyldigheter | MVPD tillhandahåller ytterligare villkor i AuthZ-svaret, till exempel loggning, eller en uppdaterad OLCA (Maximum Parental Control Rating). | C |                                                                                                                                                          |
| 1,18 | IP-adresskontext | MVPD kräver att IP-adressen skickas explicit. För AuthZ, där anropen är på serversidan, ger detta MVPD spårningsinformation om var användaren kommer ifrån i AuthZ-anropet. | C |                                                                                                                                                          |
| 1,19 | TTL-värde (Token Persistence Time-to-live) dynamiskt definierat | MVPD kan ställa in TTL för Adobe Pass-autentiseringstoken dynamiskt via egenskaper i svaret, så att Adobe är utanför slingan för TTL-ändringar. | C- |                                                                                                                                                          |
| 1,20 | Enhetstyp | MVPD stöder överföring av enhetstypen i AuthN- eller AuthZ-begäran. Den här egenskapen informerar MVPD om vilken typ av enhet som innehållet kommer att förbrukas på, så att de kan justera TTL för AuthN- eller AuthZ-token så att den anpassas efter enhetens egna säkerhetsöverväganden. | C- | Detta är användbart för den klientlösa plattformen, där det kan vara svårt att visa alla enhetstyper som en konfigurationsinställning på sidan Adobe Pass-autentisering. |



## 2. Funktioner {#operational-features}

| Nej | Funktion | Beskrivning | Prioritet | Anteckningar |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | Drifttid dygnet runt | Ett MVPD-avtal som ska användas dygnet runt och mäta sig med det över tiden. | A |       |
| 2,2 | Testa autentiseringsuppgifter för varje programmerare | MVPD har olika testkonton för respektive programmerare. | A |       |
| 2,3 | Schemaläggning för förfallodatum och förnyelse av certifikat | En MVPD-dokumenterad plan med ett schema för att säkerställa att SAML-certifikat är aktuella. | A- |       |
| 2,4 | Genomsnittlig svarstid under 250 ms/svar | Ett MVPD-avtal för att eftersträva låg latens och mäta sig med det över tid. | A- |       |
| 2,5 | Hosting Own MVPD Picker Logo | MVPD måste ha sin egen logotyp och den bör skyddas av CDN-cachning. | B |       |
| 2,6 | Supportavtal för eskalering | En MVPD-dokumenterad eskaleringsplan som hålls uppdaterad och granskas minst en gång i kvartalet. | A |       |
| 2,7 | Utgångsskydd | En samplan med MVPD med Adobe om nedbrytningsåtgärder som kan användas generellt efter behov. | B |       |
| 2,8 | Underhåll separata mellanlagrings- och produktionsslutpunkter | Ett MVPD-integrationstest som inte kräver värdfilsförfalskning för att testa mellanlagringsintegreringen. | B |       |
| 2,9 | Ytterligare QA-slutpunkt | MVPD har en ytterligare QA IdP-integrering för utveckling av gemensamma nya funktioner.  Om vi stöttar detta blir det mindre troligt att vi behöver UAT-specialförfrågningar för testning av appbutikscertifikat. | C |       |





## 3. Autentiseringsupplevelsefunktioner {#authn-exp-features}


| Nej | Funktion | Beskrivning | Prioritet | Anteckningar |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Autentiseringskonvertering över minimal förväntning | MVPD garanterar en lägsta konverteringsgrad som ger belägg för funktionaliteten (5 %) och en rimlig nivå (30 %). | A |                                                                                                                                                           |
| 3,2 | Inline lösenordsåterställning | Med MVPD kan du återställa lösenord i det federerade AuthN-flödet. | A |                                                                                                                                                           |
| 3,3 | Registrering av infogat konto | Med MVPD kan du skapa ett nytt kontoinfogat till ett federerat AuthN-flöde. | A |                                                                                                                                                           |
| 3,4 | Onlinehjälp/support | MVPD tillhandahåller ett hjälpmedel för att hjälpa till under det federerade AuthN-flödet. | A |                                                                                                                                                           |
| 3,5 | Modembaserad autentisering hemma | MVPD autentiserar automatiskt en enhet när den finns i det lokala nätverket för en registrerad modell (endast ISP MVPD). | B | Det här är en lägre prioritet eftersom det är en optimering som många ännu inte kan stödja och som ger upphov till vissa problem när det gäller begränsning av bedrägerier och föräldrakontroll |

Nu kan du importera kod från markeringstabellen direkt med dialogrutan Arkiv/Klistra in tabelldata...


## 4. Analysfunktioner {#analytics-features}


| Nej | Funktion | Beskrivning | Prioritet |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Justering av autentiseringskonverteringsfunktion | MVPD har ett mätvärde för AuthN-begäranden som börjar med SAML AuthN-begäran - som justeras mot Adobe Pass Authentication and Programmer AuthN-begäranstatistik. | A |
| 4,2 | Unika användare | Användare som har autentiserats och deduplicerats i en månadsvis uppdatering över flera dagar. | A |
| 4,3 | Tidszonsredovisning | Rapporterna innehåller tidszon för när dagen ändras. | A |





## 5. Funktioner för att minska bedrägerier {#fraud-mitgn-features}


| Nej | Funktion | Beskrivning | Prioritet | Anteckningar |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Validering av videoprenumerant vid autentisering | MVPD ser till att användaren är en giltig videoprenumerant under AuthN-flödet. | A |                                                                                                |
| 5,2 | Hastighetsbegränsning för autentisering/auktorisering | MVPD har stöd för att begränsa användningen av vissa användarkonton i sina webbtjänster när så är lämpligt. | B |                                                                                                |
| 5,3 | Beständigt användar-ID med globalt omfång för Adobe | MVPD ser till att Adobe har ett användar-ID som kan spåras mellan programmerare för att upptäcka bedrägerier. Detta behöver inte tillhandahållas kunden direkt. | B | Online Multimedia Authorization Protocol (OMAP) Specification and Real User Monitoring (RUM). |
| 5,4 | Validering av samtidig användning | MVPD har ett sätt att spåra och begränsa den samtidiga användningen av prenumerantkontot utöver en affärströskel. | B |                                                                                                |

## P1. Proxyspecifika funktioner {#proxy-sp-func-features}

| Nej | Beskrivning | Prioritet | Anteckningar |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy som ansvarar för att uppdatera listan över undergruppsdokument är korrekt | A |                                                   |
| P 1.2 | Proxy levererar en logotyp i lämplig storlek för varje subMVPD | A | Vissa live-subMVPD-program har inte rätt logotypstorlek |
| P 1.3 | Proxy levererar rätt profilerad inloggningssida för varje subMVPD | A |                                                   |
| P 1.4 | Proxy som är ansvarig för specifika tjänsteleverantörsnamn med subMVPD-lista | B |                                                   |
| P 1.5 | Proxy anger alla subMVPD-egenskaper korrekt (iFrame-storlek, TTL, logotyp etc) | A |                                                   |
| P 1.6 | Proxy anger ett separat enhets-ID | B |                                                   |

## P2. Proxyfunktioner {#proxy-op-features}

| Nej | Beskrivning | Prioritet |
|-------|----------------------------------------------------------------------------------------|----------|
| P 2.1 | API-nyckeln är skyddad | A |
| P 2.2 | Testa autentiseringsuppgifter för den första subMVPD som används i direktintegrering för varje ny begäran | A |
| P 2.3 | subMVPD-listor är korrekta och fullständiga per beställare | A |
