---
title: Hämta klientautentiseringsuppgifter
description: API för registrering av dynamisk klient - Hämta klientautentiseringsuppgifter
exl-id: 0b39768b-25b8-47b9-8080-59c56fb829fb
source-git-commit: 110e8519d6c042cc38de3fbefcd34297b6edcfad
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Hämta klientautentiseringsuppgifter {#retrieve-client-credentials}

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
      <td>/o/client/register</td>
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
      <td style="background-color: #DEEBFF;">software_statement</td>
      <td>
            Programsatsen som är associerad med det registrerade programmet som har skapats och hämtats från <a href="https://experience.adobe.com/#/pass/authentication">Adobe Pass TVE Dashboard</a>.
            <br/><br/>
            Hanteringen av registrerade program beskrivs i <a href="../dynamic-client-registration-overview.md"> Översikt över registrering av dynamiska klienter </a> .
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirect_uri</td>
      <td>Omdirigerings-URI som är associerad med den plats som användaragenten navigerar till när autentiseringsflödet är slutfört.</td>
      <td>valfri</td>
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
         Det måste vara application/json;charset=utf-8.
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
         Om det anges måste det vara application/json;charset=utf-8.
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
               <td style="background-color: #DEEBFF;">client_id</td>
               <td>Klientprogrammets identifierarsträng.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_secrets</td>
               <td>Klientprogrammets hemliga sträng.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_id_Issu_at_at</td>
               <td>Den tidpunkt då klientprogrammets identifierare utfärdades.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">redirect_uris</td>
               <td>Arrayen med URI-strängar för omdirigering som klientprogrammet kan använda i omdirigeringsbaserade flöden.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">grant_types</td>
               <td>De tilldelningstypssträngar som klientprogrammet kan använda för klienttokens slutpunkt.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">omfattningar</td>
               <td>Omfattningssträngarna som definierar de Adobe Pass-autentiserings-API:er som klientprogrammet kan använda.</td>
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
                        <li>Begäran innehåller ett parametervärde som inte stöds.</li>
                        <li>Begäran upprepar en parameter.</li>
                        <li>Begäran har fel format.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_redirect_uri</td>
               <td>Begäran innehåller ett ogiltigt värde för omdirigerings-URI.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_software_statement</td>
               <td>Begäran innehåller ett ogiltigt värde för programsatsen.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unapproved_software_statement</td>
               <td>Begäran innehåller ett värde för programsatsen som inte är godkänd för användning av Adobe Pass autentiseringsserver.</td>
            </tr>
         </table>
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples}

### Hämta klientautentiseringsuppgifter {#samples-retrieve-client-credentials}

>[!BEGINTABS]

>[!TAB Begäran]

```HTTPS
POST /o/client/register HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/json
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

{
    "software_statement": "eyJhbGciOiJSUzI1NiJ9.
        eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
        bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
        aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
        GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
        zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
        5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
        fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
        U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
        IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
    "redirect_uri": "adobepass://com.programmer"  
 }
```

>[!TAB Svar - lyckat]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
    "client_id": "s6BhdRkqt3",
    "client_secret": "t7AkePiru4",
    "redirect_uris": [
        "app://com.programmer.adobe#sdasdsadas"
    ],
    "grant_types": [
        "client_credentials"
    ],
    "scopes": [
        "api:client:v2"
    ],
    "client_id_issued_at": 1723227212
}
```

>[!TAB Svar - Fel]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
