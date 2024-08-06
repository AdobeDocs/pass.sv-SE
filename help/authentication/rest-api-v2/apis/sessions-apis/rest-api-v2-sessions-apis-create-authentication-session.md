---
title: Skapa autentiseringssession
description: REST API V2 - Skapa autentiseringssession
source-git-commit: cf479236cbd43d5e4585bf0eac18a12dbe4bc6c4
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 0%

---


# Skapa autentiseringssession - test {#create-authentication-session}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
>REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/throttling-mechanism.md).

## Begäran {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/api/v2/{serviceProvider}/sessioner</td>
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
      <td>Den interna unika identifierare som är associerad med tjänsteleverantören under introduktionsprocessen.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kroppsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        Den interna unika identifierare som är associerad med identitetsleverantören under introduktionsprocessen.
        <br/><br/>
        Om plattformen för direktuppspelningsenheten har begränsningar för att tillhandahålla ett värde, måste ett program återuppta autentiseringssessionen och ange ett giltigt värde.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Den ursprungliga domänen för det program som utför MVPD-inloggning.
        <br/><br/>
        Om plattformen för direktuppspelningsenheten har begränsningar för att tillhandahålla ett värde, måste ett program återuppta autentiseringssessionen och ange ett giltigt värde.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Den slutliga omdirigerings-URL som användaragenten navigerar till när autentiseringsflödet för det virtuella dokumentfönstret är slutfört.
        <br/><br/>
        Värdet måste vara URL-kodat.
        <br/><br/>
        Om plattformen för direktuppspelningsenheten har begränsningar för att tillhandahålla ett värde, måste ett program återuppta autentiseringssessionen och ange ett giltigt värde.
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
      <td>Genereringen av mottagarens tokennyttolast beskrivs i dokumentationen för <a href="../../../dynamic-client-registration-api.md">Dynamisk klientregistrering</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Godkänd medietyp för resurserna som skickas.
         <br/><br/>
         Det måste vara application/x-www-form-urlencoded.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>Genereringen av nyttolasten för enhetsidentifieraren beskrivs i dokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Genereringen av nyttolasten för enhetsinformation beskrivs i <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> -dokumentationen.
         <br/><br/>
         Vi rekommenderar att du alltid använder den när programmets enhetsplattform tillåter explicit tillhandahållande av giltiga värden.
         <br/><br/>
         När detta anges sammanfogas Adobe Pass Authentication-backend explicit med extraherade värden implicit (som standard).
         <br/><br/>
         Om det inte anges kommer Adobe Pass Authentication-serverdelen att använda extraherade värden implicit (som standard).
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Direktuppspelningsenhetens IP-adress.
         <br/><br/>
         Vi rekommenderar starkt att du alltid använder det för server-till-server-implementeringar, särskilt när anropet görs av programmeringstjänsten i stället för av direktuppspelningsenheten.
         <br/><br/>
         För implementeringar från klient till server skickas direktuppspelningsenhetens IP-adress implicit.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token</td>
      <td>
        Genereringen av nyttolasten för enkel inloggning för plattformsidentitetsmetoden beskrivs i <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a> -dokumentationen.
        <br/><br/>
        Mer information om enkla inloggningsaktiverade flöden med en plattformsidentitet finns i dokumentationen för <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> enkel inloggning med plattformsidentitetsflöden </a> .
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-token</td>
      <td>
        Genereringen av nyttolasten för enkel inloggning för Service Token-metoden beskrivs i dokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Mer information om enkla inloggningsaktiverade flöden med en tjänsttoken finns i dokumentationen för <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md"> enkel inloggning med tjänsttoken flows </a> .
      <td>valfri</td>
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

## Svar {#response}

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
        Svarstexten innehåller information om nästa åtgärd som krävs för att utföra autentisering.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="../../../enhanced-error-codes.md">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen för <a href="../../../dynamic-client-registration-api.md">registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Metoden tillåts inte</td>
      <td>
        HTTP-metoden är ogiltig. Klienten måste använda en HTTP-metod som är tillåten för den begärda resursen och försök igen. Mer information finns i avsnittet <a href="#request">Begäran</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Internt serverfel</td>
      <td>
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="../../../enhanced-error-codes.md">Förbättrade felkoder</a>.
      </td>
   </tr>
</table>

### Lyckades {#success}

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
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON-objekt med följande attribut:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Den åtgärd som strömningsenheten måste utföra för att autentiseringsflödet ska kunna slutföras.
                  <br/><br/>
                  Möjliga värden är:
                  <ul>
                    <li><b>autentisera</b></li>
                    <ul>
                        <li>Direktuppspelningsenheten eller någon annan enhet måste öppna den angivna URL:en i en användaragent.</li>
                    </ul>
                    <li><b>meritförteckning</b></li>
                    <ul>
                        <li>Direktuppspelningsenheten eller någon annan enhet måste tillhandahålla de saknade parametrarna och återuppta autentiseringssessionen med hjälp av koden.</li>
                    </ul>
                    <li><b>auktorisera</b></li>
                    <ul>
                        <li>Direktuppspelningsenheten kan fortsätta direkt med beslutsflöden.</li>
                    </ul>
                  </ul>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Den typ av interaktion som direktuppspelningsenheten måste utföra för att fortsätta flödet med den åtgärd som anges av attributet actionName.
                  <br/><br/>
                  Möjliga värden är:
                  <ul>
                    <li><b>interaktiv</b></li>
                    <ul>
                        <li>Flödet fortsätter med en navigering till den angivna URL:en med en användaragent.</li>
                    </ul>
                    <li><b>direkt</b></li>
                    <ul>
                        <li>Flödet fortsätter med ett direkt anrop till den angivna URL:en med en HTTP-klient som är tillgänglig för klientimplementeringen.</li>
                    </ul>
                  </ul>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>De saknade parametrar som måste anges för att det grundläggande autentiseringsflödet ska kunna slutföras.</td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>Den URL som klientprogrammet behöver för att navigera i.</td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">kod</td>
               <td>Autentiseringskoden som kan användas i ett sekundärt program för att återuppta autentiseringssessionen.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Den ogenomskinliga identifierare som kan användas för att spåra användaraktivitet.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Den interna unika identifierare som är associerad med identitetsleverantören under introduktionsprocessen.</td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>Den interna unika identifierare som är associerad med tjänsteleverantören under introduktionsprocessen.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
         </table>
      </td>
      <td><i>obligatoriskt</i></td>
</table>

### Fel {#error}

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
      <td style="background-color: #DEEBFF;">fel</td>
      <td>Felet ger ytterligare information som följer dokumentationen för <a href="../../../enhanced-error-codes.md">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples}

### 1. Skapa en autentiseringssession och ange värden för alla obligatoriska parametrar

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "authenticate",
   "actionType" : "interactive",
   "url" : "/v2/authenticate/REF30/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 2. Skapa en autentiseringssession och ange värden för inga eller några obligatoriska parametrar

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "resume",
   "actionType" : "direct",
   "url" : "/v2/REF30/sessions/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "missingParameters" : ["mvpd","domain","redirectUrl"],
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 3. Skapa en autentiseringssession samtidigt som du anger ett värde för mvpd-parametern och för vilken det redan finns en giltig autentiserad profil

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "profile",
   "actionType" : "direct",
   "url" : "/v2/REF30/profiles/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 4. Skapa en autentiseringssession samtidigt som du anger ett värde för mvpd-parametern och för vilken en försämring finns

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/v2/REF30/decisions/authorize",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
