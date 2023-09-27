---
title: API för registrering av dynamisk klient
description: API för registrering av dynamisk klient
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# API för registrering av dynamisk klient {#dynamic-client-registration-api}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

För närvarande finns det två sätt på vilka Adobe Pass Authentication identifierar och registrerar program:

* webbläsarbaserade klienter registreras via tillåtet [domänlista](/help/authentication/programmer-overview.md)
* Inbyggda programklienter, som iOS och Android-program, registreras via den signerade beställarens mekanism.

Adobe Pass Authentication föreslår en ny metod för att registrera program. Denna mekanism beskrivs i följande stycken.

## Mekanismen för ansökningsregistrering {#appRegistrationMechanism}

### Tekniska skäl {#reasons}

Autentiseringsmekanismen i Adobe Pass Authentication förlitade sig på sessionscookies, men på grund av [Android Chrome, anpassade flikar](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}kan detta mål inte uppnås längre.

Med tanke på dessa begränsningar inför Adobe en ny registreringsmekanism för alla sina kunder. Den baseras på OAuth 2.0 RFC och består av följande steg:

1. Hämtar programsatsen från TVE Dashboard
1. Hämtar klientautentiseringsuppgifter
1. Hämtar åtkomsttoken

### Hämtar programsats från TVE Dashboard {#softwareStatement}

För varje program du släpper måste du ha en programsats. Alla programsatser tillhandahålls via TVE Dashboard när programmen har skapats. Programsatsen ska distribueras tillsammans med programmet på användarens enhet.

>[!IMPORTANT]
>
>När du använder en programsats behövs inte längre den signerade ID-mekanismen för begärande.

Mer information om hur du skapar programsatser finns på [Klientregistrering på TVE Dashboard](/help/authentication/dynamic-client-registration.md).

### Hämta klientautentiseringsuppgifter {#clientCredentials}

När du har hämtat en programsats från TVE Dashboard måste du registrera programmet på Adobe Pass autentiseringsserver. Gör detta genom att utföra ett /register-anrop och hämta din unika klientidentifierare.

**Begäran**

| HTTP-samtal |                    |
|-----------|--------------------|
| bana | /o/client/register |
| method | POST |

| fält |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | Programsatsen som skapades i TVE Dashboard. | obligatoriskt |
| redirect_uri | Den URI som programmet använder för att slutföra autentiseringsflödet. | valfri |

| begäranrubriker |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | obligatoriskt |
| X-Device-Info | Enhetsinformationen enligt definitionen i Skicka information om enhet och anslutning | obligatoriskt |
| Användaragent | Användaragenten | obligatoriskt |

**Svar**

| svarsrubriker |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | obligatoriskt |

| svarsfält |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | Sträng | obligatoriskt |
| client_secrets | Sträng | obligatoriskt |
| client_id_Issu_at_at | long | obligatoriskt |
| redirect_uris | lista med strängar | obligatoriskt |
| grant_types | lista med strängar<br/> **godkänt värde**<br/> `client_credentials`: Används av osäkra klienter, till exempel Android SDK. | obligatoriskt |
| fel | **godkända värden**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unapproved_software_statement</li></ul> | obligatoriskt i ett felflöde |


#### Felsvar {#error-response}

Om ett fel inträffar svarar registreringsservern med en HTTP 400-statuskod (felaktig begäran) och inkluderar följande parametrar i svaret:

| statuskod | svarsorgan | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Begäran saknar en obligatorisk parameter, innehåller ett parametervärde som inte stöds, upprepar en parameter eller också har den fel format. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | Redirect_uri tillåts inte för den här klienten baserat på dess registrerade program. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | Programsatsen är ogiltig. |
| HTTP 400 | {&quot;error&quot;: &quot;unapproved_software_statement&quot;} | Det gick inte att hitta software_id i konfigurationen. |

#### Exempel på klientautentiseringsuppgifter {#client-credentials-example}

**Begäran:**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
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
  "redirect_uri": "adobepass://com.programmer"  }
```

**Svar:**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**Felsvar:**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### Hämtar åtkomsttoken {#accessToken}

När du har hämtat den unika klientidentifieraren (klient-ID och klienthemlighet) för ditt program måste du hämta en åtkomsttoken. Åtkomsttoken är en obligatorisk OAuth 2.0-token som används för att anropa Adobe Pass Authentication API:er.

>[!NOTE]
>
>För närvarande har åtkomsttokens 24 timmar på sig att leva.

**Begäran**


| **HTTP-samtal** | |
| --- | --- |
| bana | `/o/client/token` |
| method | POST |

| **frågeparametrar** | |
| --- | --- |
| `grant_type` | Mottaget i klientregistreringsprocessen.<br/> **Godkänt värde**<br/>`client_credentials`: Används för osäkra klienter, som Android SDK. |
| `client_id` | Klient-ID som hämtats i klientregistreringsprocessen. |
| `client_secret` | Klient-ID som hämtats i klientregistreringsprocessen. |

**Svar**

| svarsfält | | |
| --- | --- | --- |
| `access_token` | Det åtkomsttokenvärde som du bör använda för att anropa Adobe Pass API:er | obligatoriskt |
| `expires_in` | Tiden i sekunder tills access_token upphör att gälla | obligatoriskt |
| `token_type` | Typen av token **innehavare** | obligatoriskt |
| `created_at` | Utgivningstiden för token | obligatoriskt |
| **svarsrubriker** | | |
| `Content-Type` | application/json | obligatoriskt |

**Felsvar**

Om ett fel inträffar svarar auktoriseringsservern med en HTTP 400-statuskod (felaktig begäran) och inkluderar följande parametrar i svaret:

| statuskod | svarsorgan | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | Begäran saknar en obligatorisk parameter, innehåller ett parametervärde som inte stöds (annat än anslagstyp), upprepar en parameter, innehåller flera autentiseringsuppgifter, använder mer än en mekanism för autentisering av klienten eller har på annat sätt fel format. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | Klientautentiseringen misslyckades eftersom klienten var okänd. SDK MÅSTE registreras på auktoriseringsservern igen. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | Den autentiserade klienten har inte behörighet att använda den här behörighetstypen. |

#### Hämtar ett exempel på åtkomsttoken: {#obt-access-token}

**Begäran:**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**Svar:**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**Felsvar:**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## Utför autentiseringsbegäranden {#autheticationRequests}

Använd åtkomsttoken för att utföra Adobe Pass [API-anrop för autentisering](/help/authentication/initiate-authentication.md). Åtkomsttoken måste läggas till i API-begäran på något av följande sätt:

* genom att lägga till en ny frågeparameter i begäran. Den nya parametern anropas **access_token**.

* genom att lägga till en ny HTTP-rubrik i begäran: Authorization: Bearer. Vi rekommenderar att du använder HTTP-huvudet eftersom frågesträngar ofta visas i serverloggar.

Om ett fel uppstår kan följande felsvar returneras:

| Felsvar |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | Begäran har fel format. |
| invalid_client | 403 | Klient-ID:t har inte längre behörighet att utföra begäranden. Nya klientautentiseringsuppgifter ska genereras. |
| access_deny | 401 | access_token är inte giltig (utgången eller ogiltig). Klienten MÅSTE begära en ny access_token. |

### Exempel på utförande av autentiseringsbegäranden:

**Skickar åtkomsttoken som begärandeparameter:**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**Skickar åtkomsttoken som HTTP-huvud:**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**Felsvar som svarstext:**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**Felsvar som URL-parametrar:**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
