---
title: REST API V2-ordlista
description: REST API V2-ordlista
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1744'
ht-degree: 0%

---

# REST API V2-ordlista {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Det här dokumentet innehåller definitioner för termer som används vid integrering av Adobe Pass Authentication REST API V2.

>[!MORELIKETHIS]
>
> * [Ordlista för dynamisk klientregistrering (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Ordlista {#glossary-terms}

### A {#a}

#### Autentisering {#authentication}

Autentiseringen är en process som gör det möjligt för en användare att bevisa sin identitet för en [programmerare](#programmer) att få åtkomst till skyddat innehåll ([resurs](#resource)) efter att ha verifierat användarprenumerationen med [MVPD](#mvpd).

#### Autentiseringskod {#code}

Autentiseringskoden är ett Adobe Pass-autentiseringskoncept som lagrar ett unikt värde som genereras när en användare initierar [autentiseringsprocessen](#authentication) och unikt identifierar en användares [autentiseringssession](#session) tills autentiseringsprocessen är slutförd.

Autentiseringskoden kan användas av både ett [primärt (programmerare) program](#primary-application) eller ett [sekundärt (programmerare) program](#secondary-application) för att slutföra [autentiseringsprocessen](#authentication), hämta information om [autentiseringssessionen](#session) eller för att få åtkomst till användarprofilen [](#profile).

Synonym med den tidigare termen som används i registreringskoden.

#### Autentiseringssession {#session}

Autentiseringssessionen är ett Adobe Pass-autentiseringskoncept som lagrar information om användarens autentiseringsprocess som har startats (eller fortsatt) från ett [Programmer](#programmer) -program och som identifieras unikt av en [autentiseringskod](#code).

Autentiseringssessionen kan även indikera [Programmer](#programmer)-programmet som ska fortsätta med [auktoriseringsprocessen](#authorization) som nästa fas i [berättigandeflödet](#entitlement) om användaren redan är autentiserad.

#### Behörighet {#authorization}

Behörigheten är en process som gör att en användare kan komma åt skyddat innehåll ([resource](#resource)) från en [Programmer](#programmer) -katalog baserat på den ägda [MVPD](#mvpd) -prenumerationen efter att användarrättigheterna har verifierats med [MVPD](#mvpd) .

### C {#c}

#### Konfiguration {#configuration}

Konfigurationen är ett Adobe Pass-autentiseringskoncept som lagrar information om integreringsinställningarna [Programmer](#programmer) och [MVPD](#mvpd) och som kan användas under [autentiseringsprocessen](#authentication) när användaren uppmanas att välja sin [TV-leverantör](#tv-provider) i en lista över aktiva integreringar.

### D {#d}

#### Beslut {#decision}

Beslutet är ett Adobe Pass-autentiseringskoncept som lagrar information om processfrågan [MVPD](#mvpd) [authentication](#authorization) eller [preauthentication](#preauthorization) för att tillåta eller neka användaråtkomst till ett [Programmer](#programmer) -skyddat innehåll.

#### Försämring {#degradation}

Nedbrytningen är en Adobe Pass-autentiseringsfunktion som gör att en användare kan komma åt skyddat innehåll även när [MVPD](#mvpd) upplever en tjänststörning.

Mer information finns i [Översikt över API:t för degradering](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md).

#### Enhets-ID {#device-id}

Enhets-ID är en unik identifierare som är bunden till användarens enhet och måste tillhandahållas av [Programmer](#programmer) -programmet i alla faser av [entitlement](#entitlement) -flödet.

### E {#e}

#### Tillstånd {#entitlement}

Behörigheten är ett Adobe Pass Authentication-koncept som innehåller tillgängliga flöden och funktioner som hjälper en användare att gå igenom olika faser för att få tillgång till skyddat innehåll, från [autentisering](#authentication), [förauktorisering](#preauthorization), [auktorisering](#authorization) och slutligen [utloggning](#logout).

#### Förbättrad felkod {#enhanced-error-code}

Den utökade felkoden är ett Adobe Pass Authentication-koncept som ger ytterligare information om felet som uppstod när en begäran bearbetades.

Mer information finns i dokumentationen för [Förbättrade felkoder](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

### H {#h}

#### HBA {#hba}

HBA (Home-Based Authentication) är en process där en konsument automatiskt får åtkomst till [TV Everywhere (TVE)](#tve)-innehåll på utvalda enheter som är anslutna till hemnätverket, som är en del av platsen i prenumerationskontraktet.

### I {#i}

#### Identitetsleverantör {#identity-provider}

Identitetsleverantören är ett företag som tillhandahåller identitetstjänster till konsumenter via kabel-, satellit- eller internetbaserade tjänster i kontexten för [TV Everywhere (TVE)](#tve).

Synonym med [MVPD](#mvpd) och [TV-leverantör](#tv-provider).

### L {#l}

#### Utloggning {#logout}

Utloggningen är en process som gör att en användare kan avsluta sin autentiserade [profil](#profile) inom Adobe Pass-autentisering och uppdatera [Programmer](#programmer)-programmet så att det återspeglar användarens status.

### M {#m}

#### Medietoken {#media-token}

Medietoken är en token som genereras av Adobe Pass Authentication som ett resultat av auktoriseringen [Decision](#decision) som ska ge åtkomst till skyddat innehåll.

Medietoken skickas till [Programmer](#programmer), som sedan validerar den för att säkerställa åtkomstsäkerheten för den [resursen](#resource).

Synonym med den tidigare termen använde kort auktoriseringstoken.

#### Media Token Verifier {#media-token-verifier}

Verifieraren för medietoken är ett bibliotek som distribueras av Adobe Pass Authentication och som ansvarar för att verifiera äktheten för en [medietoken](#media-token).

Mer information finns i dokumentationen för [Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

#### MVPD {#mvpd}

Distributören av flerkanalsvideo (MVPD) är ett företag som tillhandahåller tv-tjänster till konsumenter via kabel, satellit eller internetbaserade tjänster.

MVPD identifieras av ett unikt värde som definieras under introduktionsprocessen mellan MVPD och Adobe.

Synonym med [TV-leverantören](#tv-provider) och [identitetsleverantören](#identity-provider).

### P {#p}

#### Partner {#partner}

Partnern är ett företag som tillhandahåller en tjänst eller ett ramverk till en [programmerare](#programmer) för att möjliggöra en enkel inloggning.

Partnern identifieras av ett unikt värde (t.ex.&quot;äpple&quot;) som definieras under introduktionsprocessen mellan partnern och Adobe.

#### Förhandsauktorisering {#preauthorization}

Förhandsauktoriseringen är en process som gör att en användare kan förhandsgranska en delmängd av [resources](#resource) från en [Programmer](#programmer)-katalog som han eller hon skulle ha åtkomst till efter att ha verifierat användarrättigheterna med [MVPD](#mvpd) .

Synonym med [Preflight](#preflight).

#### Preflight {#preflight}

Preflight är en process som gör det möjligt för en användare att förhandsgranska en delmängd av [resources](#resource) från en [Programmer](#programmer)-katalog som de skulle ha åtkomst till efter att ha verifierat användarrättigheterna med [MVPD](#mvpd) .

Synonym med [Förhandsauktorisering](#preauthorization).

#### Primär (programmerare) applikation {#primary-application}

Det primära programmet refererar till ett [Programmer](#programmer)-program som initierar [autentisering](#authentication), men som kanske inte kan slutföra processen genom att använda en [användaragent](#user-agent) för att navigera till inloggningssidan för [MVPD](#mvpd) .

#### Profil {#profile}

Profilen är ett Adobe Pass-autentiseringskoncept som lagrar information om användarens autentiseringsstart- och slutdatum, [användarens metadata](#user-metadata) tillsammans med andra fält som anger metoden för att erhålla autentiseringen (t.ex. &quot;normal&quot;, &quot;degraderad&quot;, &quot;tillfällig&quot;, &quot;enkel inloggning&quot; osv.).

Synonym med den tidigare termen som används som autentiseringstoken.

#### Programmer {#programmer}

Programmer är ett företag som levererar innehåll till konsumenter via egna kanaler (varumärken) över olika plattformar.

Programmeraren grupperar flera ägda kanaler (varumärken) som [tjänstleverantörer](#service-provider) i sin integrering med Adobe Pass Authentication.

#### Proxy MVPD {#proxy-mvpd}

Proxyföretaget MVPD är ett företag som tillhandahåller identitetstjänster för andra distributörer av videoprogrammeringstjänster och som är direkt integrerat med Adobe Pass-autentisering.

#### Proxierad MVPD {#proxied-mvpd}

Den proxyanslutna MVPD är ett företag som inte har någon direkt integrering med Adobe Pass Authentication, men som integreras via en [proxy-MVPD](#proxy-mvpd).

#### Plattformsidentitet {#platform-identity}

Plattformsidentiteten är en unik plattformsidentifierarnyttolast som genereras av en tjänst eller ett ramverk (bibliotek) som är bundet till användarens enhet och som tillhandahålls [Programmer](#programmer) för att möjliggöra en inloggningsanvändarupplevelse.

Mer information finns i dokumentationen för [enkel inloggning med plattformsidentitetsflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

### R {#r}

#### Resurs {#resource}

Resursen är ett skyddat innehåll som en användare försöker få åtkomst till från en [programmeringskatalog](#programmer).

Resursen identifieras av ett unikt värde som överenskommits mellan Programmer och MVPD.

Mer information finns i dokumentationen för [Skyddade resurser](/help/authentication/integration-guide-programmers/features-standard/entitlements/protected-resources.md#identifiers).

### S {#s}

#### SAML {#saml}

SAML (Security Assertion Markup Language) är en öppen standard för utbyte av autentiserings- och auktoriseringsdata mellan parter, i synnerhet mellan en [identitetsleverantör](#identity-provider) och en [tjänstleverantör](#sp).

#### Sekundärt program (programmerare) {#secondary-application}

Det sekundära programmet refererar till ett [Programmer](#programmer)-program som kan slutföra [autentiseringsprocessen](#authentication) genom att använda en [användaragent](#user-agent) för att navigera till inloggningssidan för [MVPD](#mvpd) .

Det sekundära programmet kan köras på samma enhet som det primära programmet eller på en annan (sekundär) enhet, och i så fall kallas inloggningsfunktionen för &quot;autentisering på andra skärmen&quot;.

#### Tjänsttoken {#service-token}

Tjänsttoken är en unik användaridentifierare som genereras av en tjänst eller ett ramverk (bibliotek) som är bunden till användaren och tillhandahålls [Programmer](#programmer) för att aktivera en inloggningsanvändarupplevelse.

Mer information finns i dokumentationen för [enkel inloggning med tjänsttokenflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

#### Tjänsteleverantör {#service-provider}

Tjänsteleverantören är en kanal (ett varumärke) som ägs av en [programmerare](#programmer).

Tjänsteleverantören identifieras av ett unikt värde som definieras under introduktionsprocessen mellan Programmer och Adobe.

Synonym med den tidigare termen använt begärar-ID.

#### SLO {#slo}

En enkel inloggning (SLO) är en process som gör att en användare kan logga ut från alla program som ingick i [enkel inloggning (SSO)](#sso).

#### SP {#sp}

Tjänsteleverantören (SP) refererar till den roll som spelas av Adobe Pass-autentisering för en [programmerare](#programmer) i en integrering med en [MVPD](#mvpd).

#### SSO {#sso}

enkel inloggning (SSO) är en process som gör att en användare kan autentisera en gång och få åtkomst till flera [programmerarprogram](#programmer) utan att behöva autentisera sig för vart och ett av dem.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

Grundläggande TempPass är en Adobe Pass-autentiseringsfunktion som gör att en användare kan komma åt skyddat innehåll under en begränsad tid utan att behöva autentisera med en [MVPD](#mvpd).

Mer information finns i dokumentationen för [Tillfälligt pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md).

#### TempPass-kampanj {#temp-pass-promotional}

Kampanjen TempPass är en Adobe Pass-autentiseringsfunktion som gör att en användare kan få åtkomst till skyddat innehåll för ett maximalt antal resurser och en begränsad tid utan att behöva autentisera med en [MVPD](#mvpd).

Mer information finns i dokumentationen för [Kampanjtillfälligt pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md).

#### TTL {#ttl}

TTL (time to live) är ett värde som anger hur lång tid en underliggande enhet är giltig för.

TTL-värdet kan anges för en [åtkomsttoken](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), en [profil](#profile), ett auktoriserings- [beslut](#decision) eller en [medietoken](#media-token).

#### TVE {#tve}

TV Everywhere (TVE) är en branschnisch där kunderna kan få tillgång till sina favoritprogram på tv, filmer och annat innehåll på flera enheter, som smarttelefoner, surfplattor, bärbara datorer och många andra.

#### TVE Dashboard {#tve-dashboard}

TV Everywhere-instrumentpanelen (TVE) är ett Adobe Pass-autentiseringsverktyg som tillhandahålls [programmerare](#programmer) för att hantera deras konfiguration och data.

Mer information finns i dokumentationen för [TVE Dashboard User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

#### TV-leverantör {#tv-provider}

TV-leverantören är ett företag som tillhandahåller tv-tjänster till konsumenter via kabel, satellit eller internetbaserade tjänster.

TV-leverantören identifieras av ett unikt värde som definieras under introduktionsprocessen mellan tv-leverantören och Adobe.

Synonym med [MVPD](#mvpd) och [identitetsleverantör](#identity-provider).

### U {#u}

#### Användaragent {#user-agent}

Användaragenten refererar till en webbläsare eller liknande komponent (plattformsspecifik) som kan navigera på webben och återge inloggningssidan för [MVPD](#mvpd).

#### Användar-ID {#user-id}

Användar-ID är en unik identifierare som är bunden till användaren och härstammar från autentiseringsprocessen [MVPD](#mvpd).

#### Användarmetadata {#user-metadata}

Användarens metadata refererar till användarspecifika attribut (t.ex. postnummer, föräldraklassificering, användar-ID:n) som underhålls av [MVPD](#mvpd) och tillhandahålls av Adobe Pass Authentication som en del av en [profil](#profile).

Mer information finns i dokumentationen för [Användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

### V {#v}

#### VSA {#vsa}

Video Subscriber Account (VSA) är ett ramverk som utvecklats av Apple och som tillhandahålls [Programmer](#programmer) för att möjliggöra en enkel inloggning.

Mer information finns i dokumentationen för [Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount) och [enkel inloggning med partnerflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
