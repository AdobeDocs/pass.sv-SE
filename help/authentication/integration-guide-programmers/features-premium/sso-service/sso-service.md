---
title: Adobe tjänst för enkel inloggning
description: Läs mer om Adobe Pass SSO-tjänst som möjliggör smidig autentisering på flera enheter och program.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 1%

---


# Adobe tjänst för enkel inloggning {#sso-service}

Det här dokumentet beskriver användningsfall, slutpunkter och API för Adobe Single Sign-On Service.

**Aktuell version - 1.0.0**

## Omfång {#scope}

Adobe Pass SSO Service möjliggör smidig autentisering på flera enheter och tillämpningar, vilket ger en enhetlig användarupplevelse samtidigt som säkerhets- och efterlevnadsstandarder upprätthålls. Den här tjänsten tillgodoser det växande behovet av plattformsoberoende autentisering i dagens multienhetsströmmande landskap.

## Ökning {#overview}

### Vad det är {#what-it-is}

En heltäckande lösning för enkel inloggning som gör att användare kan autentisera en gång och hantera autentiseringsöverföringen på ett välgrundat sätt till andra enheter

### Aktuella utmaningar för autentisering {#current-challenges}

* Användaren måste autentisera med varje direktuppspelningstjänst han har en prenumeration på
* Användaren måste autentisera separat på varje enhet eller i varje program
* På grund av svårigheter med att skriva ett lösenord i autentiseringsflödet på vissa plattformar kan detta leda till ökad avhoppsfrekvens

### Möjligheter till en Adobe Pass SSO-tjänst {#opportunity}

* Ökning av antalet direktuppspelningstjänster som kräver egen autentisering
* Ökande efterfrågan på sömlösa upplevelser över olika enheter
* Ökat antal strömningsenheter per hushåll
* Behovet av enhetlig autentisering per hushåll

### Fördelar {#business-benefits}

#### Innehållsleverantörer {#content-providers}

* **Ökat användarengagemang** - En sömlös upplevelse leder till längre sessionstider
* **Minskad friktion** - Lägre autentiseringshinder ökar innehållskonsumtionen
* **Förbättrad lagring** - Bättre användarupplevelse minskar bortfallet
* **Kostnadsminskning** - Färre supportärenden relaterade till autentiseringsproblem

#### Slutanvändare {#end-users}

* **Sömlös upplevelse** - Autentisera en gång, åtkomst överallt
* **Tidsbesparing** - Inga upprepade inloggningsprocesser
* **Enhetsflexibilitet** - Växla mellan enheter utan avbrott
* **Enhetlig upplevelse** - Enhetlig autentisering på alla plattformar

#### IdP (MVPD, Telcos etc.) {#idps}

* MVPD-program kan informeras om ytterligare enheter med en autentiserad enkel inloggning
* **Förbättrad användarnöjdhet** - Förbättrad autentiseringsupplevelse
* **Minskad supportbelastning** - färre autentiseringsrelaterade supportsamtal
* **Konkurrensfördel** - Bättre användarupplevelse än konkurrenterna

## Användningsexempel {#use-cases}

### Identitetsmappning {#identity-mapping}

Tjänsten gör det möjligt att länka ett D2C- och TVE-konto i samma app.

Fler direktuppspelningstjänster är byggpaket som ska säljas till en tredje part (MVPD/virtuell MVPD/Telco, osv.). Användarna kan behöva hantera flera konton i samma app. För att skapa en sömlös autentiseringsupplevelse måste en tjänst koppla samman dessa konton för att förenkla inloggningen.

Användaren autentiserar med sitt D2C-konto och autentiserar sedan en gång med ett annat konto (t.ex. MVPD). Med vår tjänst kommer dessa konton att länkas, vilket innebär att kommande autentiseringar på olika enheter bara kan göras med D2C-kontot.

### Enhetsövergripande enkel inloggning {#cross-device-sso}

Autentisering på en tv-ansluten enhet kan vara mer krånglig än på en mobiltelefon. En bra användarupplevelse är att autentisera via telefonen och sedan skicka autentiseringen till den smarta tv:n.

## Nyckelkomponenter {#key-components}

* **Service Token API** - nyckelkomponent som hanterar enkel inloggning på ett säkert sätt
* **List-API** - Programmet kan hjälpa användaren att förstå listan med enheter i ekosystemet
* **Länk-API** - Programmet gör att användaren kan lägga till fler enheter i ekosystemet
* **Avlänka API** - Programmet gör att användaren kan ta bort enheter i ekosystemet

## Användningsexempel - detaljerad {#use-cases-detailed}

### D2C-TVE SSO {#d2c-tve-sso}

Det här användningsexemplet möjliggör länkning mellan D2C- och TVE(MVPD)-autentiseringsuppgifter på en enhet och utnyttjar den länkade profilen på andra enheter i samma program.

![D2C-TVE SSO-flöde](../../../assets/sso_service_d2c_1.png)

![Enhetsövergripande SSO-flöde](../../../assets/sso_service_d2c_2.png)

### Enhetsövergripande enkel inloggning {#cross-device-sso-detailed}

Det här användningsexemplet gör det möjligt för användare som redan har autentiserats på en enhet att återanvända den autentiserade profilen på samma D2C- eller TVE-program som är installerade på andra enheter (programmet måste implementera Adobe Pass REST V2 för autentisering och auktorisering).

## Integrera Adobe SSO Service i en D2C-TVE-applikation {#integration}

### Steg 1 - Hämta en vanlig identifierare {#step-1}

För att integrera Adobe SSO Service bör en programimplementering upprätta ett unikt och beständigt ID som ska användas som vanligt ID SSO-attribut i X-SSO-ID. Detta kan du få genom att autentisera en användare med en D2C-tjänst och behålla ett attribut om autentiseringen.

### Steg 2 - Hämta en servicetoken {#step-2}

Om du använder den gemensamma identifieraren i X-SSO-ID i POST /serviceToken-slutpunkten hämtas en signerad JWT med följande nyttolast:

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

Tjänsttoken har &quot;iat&quot; - utfärdad vid och &quot;exp&quot; - förfallotid inom vilket intervall tjänsttoken är giltig. Om giltigheten går ut kan en ny JWT-fil hämtas med GET /serviceToken-slutpunkten med den utgångna Service Token-tjänsten.

### Steg 3 - Autentisera med Adobe Pass REST API V2 med en TVE MVPD {#step-3}

Autentiseringen med Adobe Pass ska implementeras med tjänsttoken: [REST API V2 - tokenflöden för enkel inloggning](https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### Steg 4 - Länka en annan enhet {#step-4}

Från det autentiserade programmet kan en länk skapas för att användas på en annan enhet med /link API

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

&quot;Koden&quot; är en kort livslång länkkod i form av en sekvens med 6 siffror som användaren kommer att infoga i ett oautentiserat program på en sekundär enhet

### Steg 5 - Hämta autentisering med enkel inloggning {#step-5}

På en annan enhet kan programmet, när användaren har introducerat koden,

* hämta identitet från D2C-tjänsten
* hämta MVPD-profil från Adobe Pass med REST V2 API

MVPD-profilen kommer att vara giltig genom enkel inloggning under den period som den initiala autentiseringen gjordes.

Om MVPD gör profilen ogiltig eller om användaren väljer att logga ut från MVPD, har programmet som använder Adobe Pass REST API V2 inte längre någon profilpost och bör kräva att användaren autentiserar sig igen med MVPD.

### Steg 6 - Hantera enkel inloggning på andra enheter {#step-6}

Ett program kan hämta information om alla andra enheter som är länkade till samma gemensamma identifierare med hjälp av /list API.

Du kan när som helst ta bort en enhet från enkel inloggning genom att bryta länken mellan den enheten och API:t /unlink.

## API:er {#apis}

### Tjänsttoken-API {#service-token-api}

#### Beskrivning {#service-token-description}

Tjänsttoken-API:t kan användas för att begära och hantera tjänsttokens som aktiverar enkel inloggning (SSO) mellan flera program eller enheter. Dessa tjänsttokens identifierar autentiserade profiler (SSO-profiler) och är nödvändiga för att upprätta och underhålla SSO-anslutningar.

>[!WARNING]
>
>Tjänsttokens innehåller känslig autentiseringsinformation. Program måste hantera dessa tokens på ett säkert sätt och aldrig exponera dem för otillförlitliga parter.

Service Token API innehåller två primära slutpunkter:

* **POST /api/{serviceProvider}/serviceToken** - Hämta en nyskapad JWS-tjänsttoken
* **GET /api/{serviceProvider}/serviceToken** - Uppdatera en befintlig JWS-tjänsttoken

Om Service Token API-begäran inte kunde hanteras på grund av ett fel i Adobe Pass Authentication Services, kommer ytterligare felinformation att inkluderas som en del av API-svaret.

#### POST - serviceToken {#post-service-token}

##### Begäran {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sökvägsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Tjänstleverantörens identifierare som token begärs för.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av mottagarens tokennyttolast beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">autentisering</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>
         Genereringen av nyttolasten för enhetsidentifieraren beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.
         <br/><br/>
         Den här identifieraren används som standard-SSO-identifierare när X-SSO-ID inte anges.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Enhetsinformationen som angetts i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a>.
         <br/><br/>
         <b> </b> Rekommenderas starkt när programmets enhetsplattform tillåter att giltiga värden anges explicit.
         <br/><br/>
         Adobe Pass Authentication-serverdelen sammanfogar explicit angivna värden med implicit extraherade värden. Om det inte anges används extraherade standardvärden.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         Den länkkod som associerar den här begäran med en befintlig autentiserad profil. När det anges kommer svaret att innehålla en tjänsttoken för enkel inloggning med profilen som genererade länkkoden.
         <br/><br/>
         Detta används vanligtvis när ett sekundärt program eller en sekundär enhet vill ansluta till en autentiserad profil från ett primärt program eller en primär enhet.
      </td>
      <td>krävs om x-sso-id inte anges</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         Den vanliga identifieraren som programmet begär att basera enkel inloggning på.
         <br/><br/>
         Om det anges används den här identifieraren för att skapa en gemensam enkel inloggningsprofil för olika enheter och/eller program.
      </td>
      <td>krävs om x-sso-link inte anges</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Acceptera</td>
      <td>
         Medietypen som accepteras av klientprogrammet.
         <br/><br/>
         Om det anges måste det vara application/json.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Användaragent</td>
      <td>Användaragenten för klientprogrammet.</td>
      <td>valfri</td>
   </tr>
</table>

##### Svar {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Skapad</td>
      <td>
        Tjänsttoken genererades och returnerades i svarstexten.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Internt serverfel</td>
      <td>
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
</table>

###### Lyckades {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>HTTP-status (t.ex. "CREATED")</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>En Base64-kodad JSON Web Signature (JWS) som innehåller tjänstens token. Denna token kan användas i efterföljande API-anrop för att identifiera den autentiserade profilen och aktivera SSO-funktioner.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms eller 0 vid fel</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms eller 0 vid fel</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

###### Fel {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Svarstexten kan innehålla ytterligare felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples-post-service-token}

### &#x200B;1. Begär en ny tjänsttoken (med SSO-ID)

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. Begär en ny tjänsttoken (med SSO-länk)

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### Begäran {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sökvägsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Tjänstleverantörens identifierare som token begärs för.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av mottagarens tokennyttolast beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">autentisering</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-token</td>
      <td>
         En tidigare inhämtad tjänsttoken som måste uppdateras.
         <br/><br/>
         Denna token måste vara giltig eller ha gått ut nyligen för att kunna uppdateras.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Acceptera</td>
      <td>
         Medietypen som accepteras av klientprogrammet.
         <br/><br/>
         Om det anges måste det vara application/json.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Användaragent</td>
      <td>Användaragenten för klientprogrammet.</td>
      <td>valfri</td>
   </tr>
</table>

##### Svar {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Tjänsttoken uppdaterades och returnerades i svarstexten.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken eller tjänsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken eller tjänsttoken och försöka igen. Mer information finns i dokumentationen <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Internt serverfel</td>
      <td>
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
</table>

###### Lyckades {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>HTTP-status (t.ex. "OK")</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>En Base64-kodad JSON Web Signature (JWS) som innehåller den uppdaterade tjänsttoken. Denna token kan användas i efterföljande API-anrop för att identifiera den autentiserade profilen och aktivera SSO-funktioner.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms eller 0 vid fel</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms eller 0 vid fel</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

###### Fel {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Svarstexten kan innehålla ytterligare felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples-get-service-token}

### &#x200B;1. Begäran om att uppdatera en tjänsttoken

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### Länk-API {#link-api}

#### Beskrivning {#link-description}

Länk-API:t kan användas för att begära en länkkod (eller QR-kod) som kan aktivera enkel inloggning (SSO) mellan flera program eller enheter. Med den här länkkoden kan användare ansluta ett nytt program eller en ny enhet till en befintlig autentiserad profil (SSO-profil), vilket ger en sömlös SSO-upplevelse mellan program och enheter.

Länk-API kräver att en giltig tjänsttoken anges i AD-Service-Token-huvudet.

Den genererade länkkoden visas vanligtvis för användare på ett primärt program eller en enhet och anges på ett sekundärt program eller en sekundär enhet för att upprätta SSO-anslutningen. Länkkoden har en begränsad giltighetsperiod (vanligtvis 5-30 minuter) och är avsedd för engångsbruk.

Om Link API-begäran inte kunde hanteras på grund av ett fel i Adobe Pass Authentication Services inkluderas ytterligare felinformation som en del av Link API-svarsresultatet.

#### POST - länk {#post-link}

##### Begäran {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sökvägsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Tjänstleverantörens identifierare som token begärs för.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av mottagarens tokennyttolast beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">autentisering</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>Genereringen av nyttolasten för enhetsidentifieraren beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-token</td>
      <td>
         Genereringen av tjänsttoken beskrivs i Service Token API-dokumentationen.
         <br/><br/>
         Denna tjänsttoken identifierar den autentiserade profil som länkkoden ska genereras för.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Acceptera</td>
      <td>
         Medietypen som accepteras av klientprogrammet.
         <br/><br/>
         Om det anges måste det vara application/json.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Användaragent</td>
      <td>Användaragenten för klientprogrammet.</td>
      <td>valfri</td>
   </tr>
</table>

##### Svar {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Skapad</td>
      <td>
        Länkkoden genererades och returnerades i svarstexten.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Internt serverfel</td>
      <td>
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
</table>

###### Lyckades {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>HTTP-status (t.ex. "CREATED")</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">link</td>
      <td>En kort numerisk eller alfanumerisk kod som kan användas på ett sekundärt program eller en sekundär enhet för att upprätta en SSO-anslutning.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Tidsstämpeln (i millisekunder sedan epoken) när länkkoden blir giltig.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Tidsstämpeln (i millisekunder sedan epoken) när länkkoden upphör att gälla.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

###### Fel {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Svarstexten kan innehålla ytterligare felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples-post-link}

### &#x200B;1. Begär en länkkod för en befintlig autentiserad profil

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### Bryt länk-API {#unlink-api}

#### Beskrivning {#unlink-description}

Avlänks-API:t kan användas för att begära att en eller flera enheter tas bort från en autentiserad profil (SSO-profil). Med detta API kan användare koppla bort enheter från SSO-konfigurationen, vilket ger kontroll över vilka enheter som har tillgång till deras autentiserade profil.

>[!WARNING]
>
>Avlänknings-API kräver att en giltig tjänsttoken anges i AD-Service-Token-huvudet.

Om det inte gick att utföra avlänkning av API-begäran på grund av ett fel i Adobe Pass Authentication Services inkluderas ytterligare felinformation som en del av avlänks-API-svarsresultatet.

#### POST - bryt länk {#post-unlink}

##### Begäran {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sökvägsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Tjänstleverantörens identifierare.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kroppsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">enheter</td>
      <td>
         Matris med enhetsidentifierare som ska avlänkas.
         <br/><br/>
         Exempel: <br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av mottagarens tokennyttolast beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">autentisering</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Godkänd medietyp för resurserna som skickas.
         <br/><br/>
         Det måste vara application/json.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>Genereringen av nyttolasten för enhetsidentifieraren beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-token</td>
      <td>
         Genereringen av tjänsttoken beskrivs i Service Token API-dokumentationen.
         <br/><br/>
         Denna tjänsttoken identifierar den autentiserade profil som enheterna kommer att kopplas från för.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Acceptera</td>
      <td>
         Medietypen som accepteras av klientprogrammet.
         <br/><br/>
         Om det anges måste det vara application/json.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Användaragent</td>
      <td>Användaragenten för klientprogrammet.</td>
      <td>valfri</td>
   </tr>
</table>

##### Svar {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        De begärda enheterna har kopplats från SSO-konfigurationen.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Metoden tillåts inte</td>
      <td>
        HTTP-metoden är ogiltig. Klienten måste använda en HTTP-metod som är tillåten för den begärda resursen och försök igen.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Internt serverfel</td>
      <td>
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
</table>

###### Lyckades {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Information om åtgärdsresultat: "OK"</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         Lista över ej länkade enheter.
         <br/><br/>
         Exempel: <br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

###### Fel {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Svarstexten kan innehålla ytterligare felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples-post-unlink}

### &#x200B;1. Begär att avlänka enheter från en SSO-profil

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. Begäran om att avlänka enheter delvis lyckades

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### List-API {#list-api}

#### Beskrivning {#list-description}

List-API:t kan användas för att hämta en lista över enheter som är anslutna till en autentiserad profil (SSO-profil). Med detta API kan användare och program visa vilka enheter som ingår i SSO-konfigurationen, vilket ger synlighet och hanteringsfunktioner för deras autentiserade upplevelse på flera enheter.

>[!WARNING]
>
>List-API kräver att en giltig tjänsttoken anges i AD-Service-Token-huvudet.

List-API:t returnerar information om varje enhet i den autentiserade profilen (SSO-profil), inklusive enhetsidentifierare och metadata som kan hjälpa användare att identifiera sina enheter. Den här informationen kan användas för att hjälpa användare att fatta välgrundade beslut om vilka enheter som ska finnas kvar i SSO-konfigurationen.

Om List API-begäran inte kunde hanteras på grund av ett fel i Adobe Pass Authentication Services inkluderas ytterligare felinformation som en del av List API-svarsresultatet.

#### GET - lista {#get-list}

##### Begäran {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sökvägsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Tjänstleverantörens identifierare.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av mottagarens tokennyttolast beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">autentisering</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>Genereringen av nyttolasten för enhetsidentifieraren beskrivs i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-token</td>
      <td>
         Genereringen av tjänsttoken beskrivs i Service Token API-dokumentationen.
         <br/><br/>
         Denna tjänsttoken identifierar den autentiserade profil som enhetslistan ska hämtas för.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Acceptera</td>
      <td>
         Medietypen som accepteras av klientprogrammet.
         <br/><br/>
         Om det anges måste det vara application/json.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Användaragent</td>
      <td>Användaragenten för klientprogrammet.</td>
      <td>valfri</td>
   </tr>
</table>

##### Svar {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Code</th>
      <th style="background-color: #EFF2F7;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Listan över enheter i SSO-konfigurationen hämtades och returnerades i svarstexten.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Översikt över registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Metoden tillåts inte</td>
      <td>
        HTTP-metoden är ogiltig. Klienten måste använda en HTTP-metod som är tillåten för den begärda resursen och försök igen.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Internt serverfel</td>
      <td>
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.
      </td>
   </tr>
</table>

###### Lyckades {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">enheter</td>
      <td>
         JSON innehåller en karta över nyckel- och värdepar.
         <br/><br/>
         <b> Key: </b> deviceId - Nyttolasten för enhetsidentifieraren enligt beskrivningen i rubrikdokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier"> AP-Device-Identifier </a>
         <br/><br/>
         <b>Värde:</b> attribut - JSON innehåller en karta över attribut för enhetsmetadata inklusive:
         <ul>
            <li>enhetstyp</li>
            <li>plattform</li>
            <li>användaragent</li>
            <li>andra relevanta metadata som hjälper till att identifiera enheten</li>
         </ul>
         Värdena för attributen kan vara enkla (sträng, heltal, boolesk osv.)
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

###### Fel {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Brödtext</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>Svarstexten kan innehålla ytterligare felinformation som följer dokumentationen för <a href="https://experienceleague.adobe.com/sv/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples-get-list}

### &#x200B;1. Begär att enheter ska listas i en SSO-profil

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. Begär att enheter utan länkade enheter ska listas

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## Felkoder {#error-codes}

### Felsvarsstruktur {#error-structure}

Alla felsvar innehåller följande fält:

| Fält | Typ | Beskrivning |
|:---|:---|:---|
| status | Heltal | HTTP-statuskod (400, 401, 500 osv.) |
| kod | Sträng | Maskinläsbar felkod |
| message | Sträng | Beskrivning som kan läsas av människor |
| åtgärd | Sträng | Föreslagen åtgärd för klienten |
| helpUrl | Sträng | Dokumentationslänk |
| trace | Sträng | Unikt begärande-ID (UUID) för korrelation |

**Exempel på felsvar**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=sv-SE",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**Exempel på lyckade svar**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>Slutförda svar innehåller INTE något felfält. Felsvar innehåller INTE fält för lyckade åtgärder.

### Katalog för felkoder {#error-catalog}

#### Allmänna fel

| kod | status | message | åtgärd |
|:---|:---|:---|:---|
| token_invalid | 400 | Angiven token är ogiltig | get_new_token |
| token_expirate | 401 | Token har gått ut | get_new_token |
| header_missing | 400 | Ett obligatoriskt huvud saknas | check_headers |
| obehörig | 401 | Obehörig åtkomst | ingen |
| internal_error | 500 | Ett internt fel inträffade | ingen |

#### Valideringsfel

| kod | status | message | åtgärd |
|:---|:---|:---|:---|
| request_null | 400 | Begäranobjektet får inte vara null | ingen |

#### POST ServiceToken-fel

| kod | status | message | åtgärd |
|:---|:---|:---|:---|
| header_missing | 400 | Antingen x-sso-id eller x-sso-link header krävs för POST-begäranden | check_headers |
| header_missing | 400 | AP-Device-Identifier-huvud krävs för POST-begäranden | check_headers |

#### GET ServiceToken-fel

| kod | status | message | åtgärd |
|:---|:---|:---|:---|
| header_missing | 400 | AD-Service-Token-huvud krävs för GET-begäranden | check_headers |
| header_invalid | 401 | Ogiltig JWT-signatur i AD-Service-Token | get_new_token |
| header_invalid | 401 | Fel vid validering av JWT-signatur | get_new_token |
| header_invalid | 401 | JWT-ämne (sub) saknas eller är tomt i AD-Service-Token | get_new_token |
| header_invalid | 401 | Fel vid extrahering av JWT-objekt | get_new_token |

#### Länkverifieringsfel

| kod | status | message | åtgärd |
|:---|:---|:---|:---|
| header_missing | 401 | AD-Service-Token-huvud krävs för länkbegäranden | check_headers |
| header_invalid | 401 | Ogiltig JWT-signatur i AD-Service-Token | get_new_token |
| header_invalid | 401 | Fel vid validering av JWT-signatur | get_new_token |

#### Bryt länk till verifieringsfel

| kod | status | message | åtgärd |
|:---|:---|:---|:---|
| header_missing | 401 | AD-Service-Token-huvud krävs för avlänkningsbegäranden | check_headers |
| header_invalid | 401 | Ogiltig JWT-signatur i AD-Service-Token | get_new_token |
| header_invalid | 401 | Fel vid validering av JWT-signatur | get_new_token |
| header_invalid | 401 | JWT-ämne (sub) saknas eller är tomt i AD-Service-Token | get_new_token |
| request_invalid | 400 | Enhetslistan får inte vara null eller tom | check_request_body |

#### Lista verifieringsfel

| kod | status | message | åtgärd |
|:---|:---|:---|:---|
| header_missing | 401 | AD-Service-Token-huvud krävs för listbegäranden | check_headers |
| header_invalid | 401 | Ogiltig JWT-signatur i AD-Service-Token | get_new_token |
| header_invalid | 401 | Fel vid validering av JWT-signatur | get_new_token |
| header_invalid | 401 | JWT-ämne (sub) saknas eller är tomt i AD-Service-Token | get_new_token |
| header_invalid | 401 | Fel vid extrahering av JWT-objekt | get_new_token |

