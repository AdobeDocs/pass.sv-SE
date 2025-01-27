---
title: Skapa autentiseringssession
description: REST API V2 - Skapa autentiseringssession
exl-id: bb2a6bb4-0778-4748-a674-df9d0e8242c8
source-git-commit: 5cb14959d6e9af91252316fbdd14ff33d813089b
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 0%

---

# Skapa autentiseringssession {#create-authentication-session}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> REST API V2-implementeringen begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

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
        Den ursprungliga domänen för det program som kör MVPD-inloggning.
        <br/><br/>
        Om plattformen för direktuppspelningsenheten har begränsningar för att tillhandahålla ett värde, måste ett program återuppta autentiseringssessionen och ange ett giltigt värde.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Den slutliga omdirigerings-URL som användaragenten navigerar till när autentiseringsflödet för MVPD är slutfört.
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
      <td>Genereringen av mottagarens tokennyttolast beskrivs i rubrikdokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">autentisering</a>.</td>
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
      <td>Genereringen av nyttolasten för enhetsidentifieraren beskrivs i rubrikdokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Genereringen av nyttolasten för enhetsinformation beskrivs i rubrikdokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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
        Genereringen av nyttolasten för enkel inloggning för plattformsidentitetsmetoden beskrivs i rubrikdokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a>.
        <br/><br/>
        Mer information om enkla inloggningsaktiverade flöden med en plattformsidentitet finns i dokumentationen för <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> enkel inloggning med plattformsidentitetsflöden </a> .
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-token</td>
      <td>
        Genereringen av nyttolasten för enkel inloggning för Service Token-metoden beskrivs i rubrikdokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
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
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">Översikt över registrering av dynamisk klient</a>.
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
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Förbättrade felkoder</a>.
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
                    <li><b>Autentisera</b><br/>Direktuppspelningsenheten eller en annan enhet måste öppna den angivna URL:en i en användaragent.</li>
                    <li><b>CV</b><br/>Direktuppspelningsenheten eller en annan enhet måste tillhandahålla de saknade parametrarna och återuppta autentiseringssessionen med hjälp av koden.</li>
                    <li><b>auktorisera</b><br/>Direktuppspelningsenheten kan fortsätta direkt med beslutsflöden.</li>
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
                    <li><b>interaktiv</b><br/>Flödet fortsätter med en navigering till den angivna URL:en med en användaragent.</li>
                    <li><b>direct</b><br/>Flödet fortsätter med ett direkt anrop till den angivna URL:en med en HTTP-klient som är tillgänglig för klientimplementeringen.</li>
                  </ul>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">reasonType</td>
               <td>
                  Den typ av orsak som används som förklarar actionName.
                  <br/><br/>
                  Möjliga värden är:
                  <ul>
                    <li><b>ingen</b></li>
                    <li><b>autentiserad</b></li>
                    <li><b>tillfällig</b></li>
                    <li><b>nedtonad</b></li>
                    <li><b>authenticatedSSO</b></li>
                    <li><b>pfs_fallback</b></li>
                    <li><b>configuration_fallback</b></li>
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
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>Tidsstämpeln före vilken autentiseringskoden inte är giltig.</td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>Tidsstämpeln efter vilken autentiseringskoden inte är giltig.</td>
               <td>valfri</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>Svarstexten kan innehålla ytterligare felinformation som följer dokumentationen för <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples}

### 1. Skapa en autentiseringssession utan att saknade parametrar.

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/v2/REF30/sessions HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "none",
    "url": "/api/v2/authenticate/REF30/8ER640M",
    "code": "8ER640M",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 2. Skapa en autentiseringssession utan parametrar

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/v2/REF30/sessions HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "resume",
    "actionType": "direct",
    "reasonType": "none",
    "url": "/api/v2/REF30/sessions/8ER640M",
    "missingParameters": ["mvpd", "domain", "redirectUrl"],
    "code": "8ER640M",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 3. Skapa en autentiseringssession när en giltig profil redan finns.

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/v2/REF30/sessions HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "authenticated",
    "url": "/api/v2/REF30/decisions/authorize/Cablevision",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30"
}
```

### 4. Skapa en autentiseringssession med grundläggande eller kampanjtillagd TempPass (krävs inte)

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/v2/REF30/sessions HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=TempPass_TEST40&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "temporary",
    "url": "/api/v2/REF30/decisions/authorize/TempPass_TEST40",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "TempPass_TEST40",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]

### 5. Skapa en autentiseringssession när nedgradering tillämpas

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /api/v2/REF30/sessions HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "degraded",
    "url": "/api/v2/REF30/decisions/authorize/Cablevision",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]
