---
title: Hämta åtkomsttoken
description: API för registrering av dynamisk klient - Hämta åtkomsttoken
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: be2b75d3dcde92c0b83700705892403291dcab2e
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# Hämta åtkomsttoken {#retrieve-access-token}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Implementeringen av API:t för registrering av dynamiska klienter begränsas av dokumentationen för [begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Begäran {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Kroppsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            Klientprogrammets identifierarsträng.
            <br/><br/>
            Mer information om hur du hämtar klientidentifierarsträngen finns i API-dokumentationen för <a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> Hämta klientautentiseringsuppgifter </a> .
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secrets</td>
      <td>
            Klientprogrammets hemliga sträng.
            <br/><br/>
            Mer information om hur du hämtar klienthemlighetssträngen finns i API-dokumentationen för <a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> Hämta klientinloggningsuppgifter </a> .
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            Den tilldelningstypssträng (t.ex. "client_credentials") som klientprogrammet kan använda för klientens token-slutpunkt.
            <br/><br/>
            Mer information om hur du hämtar tilldelningstypssträngen finns i API-dokumentationen för <a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> Hämta klientinloggningsuppgifter </a> .
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Genereringen av nyttolasten för enhetsinformation beskrivs i <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> -dokumentationen.
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

### Lyckades {#success}

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
               <td style="background-color: #DEEBFF;">id</td>
               <td>Den ogenomskinliga identifierare som kan användas för att spåra användaraktivitet.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>Det åtkomsttokenvärde som klientprogrammet måste använda för auktoriseringshuvudet.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>Tiden i millisekunder som åtkomsttoken utfärdades.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">förfaller_in</td>
               <td>Tiden i sekunder tills åtkomsttoken upphör att gälla.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>Tokentyp (t.ex. "innehavare").</td>
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
      <td>400</td>
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
      <td>
        Möjliga värden är:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Värde</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    Begäran är ogiltig på grund av en av följande orsaker:
                    <ul>
                        <li>Begäran saknar en obligatorisk parameter.</li>
                        <li>Begäran innehåller ett parametervärde som inte stöds (annat än anslagstyp).</li>
                        <li>Begäran upprepar en parameter.</li>
                        <li>Begäran innehåller flera autentiseringsuppgifter.</li>
                        <li>Begäran använder mer än en mekanism för att autentisera klienten.</li>
                        <li>Begäran har fel format.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>Klientens autentiseringsuppgifter är ogiltiga. Klienten måste hämta nya klientautentiseringsuppgifter och försöka igen. Mer information finns i API-dokumentationen för <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Hämta klientautentiseringsuppgifter</a>.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>Den anslagstyp som används är ogiltig.</td>
            </tr>
         </table>
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples}

### Hämta åtkomsttoken {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB Svar - lyckat]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1752148106221,
  "expires_in": 21600,
  "token_type": "bearer"
}
```

>[!TAB Svar - Fel]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
