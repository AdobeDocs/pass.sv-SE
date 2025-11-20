---
title: API-referens - översikt
description: Fullständig referens för API:t för övervakning av samtidig användning, inklusive slutpunkter, autentisering och svarsformat
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# API-referens - översikt {#api-reference-overview}

API:t för övervakning av samtidig användning (Concurrency Monitoring) innehåller ett RESTful-gränssnitt för hantering av direktuppspelningssessioner och genomförande av principer för samtidig användning. Den här referensen innehåller fullständig dokumentation för alla slutpunkter, autentiseringsmetoder, fråge-/svarsformat och felhantering.

## API-bas-URL

### Produktionsmiljö

```
https://streams.adobeprimetime.com/v2/
```

### Mellanlagringsmiljö

```
https://streams-stage.adobeprimetime.com/v2/
```

**Obs!** Använd alltid mellanlagringsmiljön för utveckling och testning. Produktionsautentiseringsuppgifter anges först efter lyckad integration i testversionerna.

## Autentisering

Alla API-anrop kräver HTTP Basic Authentication med dina programautentiseringsuppgifter:

- **Användarnamn:** Ditt program-ID (tillhandahålls av Adobe)
- **Lösenord:** Tom sträng

### Exempel på autentiseringsrubrik

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## Standarder för svarsformat

### Slutförda svar

Alla lyckade svar följer den här strukturen:

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Felsvar

Alla felsvar följer den här strukturen:

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### Utvärderingsresultatformat

När profiler utvärderas (särskilt för 409 konflikter) innehåller svaren ett utvärderingsresultat:

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Vanliga HTTP-statuskoder

| Code | Beskrivning | Vid returnerad |
|------|----------------------|------------------------------------------------|
| 200 | OK | Slutförda GET-begäranden |
| 202 | Skapad/accepterad | Sessionen har skapats/pulsslag har registrerats |
| 400 | Felaktig begäran | Ogiltiga parametrar eller obligatoriska fält saknas |
| 401 | Obehörig | Ogiltig eller saknad autentisering |
| 403 | Förbjuden | Otillräckliga behörigheter |
| 404 | Sessions-ID hittades inte | Sessions-ID har inte genererats av CM-tjänsten |
| 409 | Konflikt | Policyöverträdelse (gräns för samtidig användning har nåtts) |
| 410 | Borta | Sessionen har upphört eller avslutats |
| 429 | För många förfrågningar | Hastighetsgränsen har överskridits |

## Metoder för parameteröverföring

### Sökvägsparametrar

Obligatoriska parametrar som är en del av URL-sökvägen:

1. `{idp}` - ID för identitetsleverantör
2. `{subject}` - användaridentifierare (vanligtvis från Adobe Pass)
3. `{sessionId}` - Sessions-ID (returneras i platshuvudet)

### Ytterligare parametrar

Valfria parametrar skickas i URL:en:

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### Formulärdata (POST/PUT)

Metadata och sessionsdata i begärandetexten:

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### Sidhuvuden

Specialparametrar som skickas i HTTP-rubriker:

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## Felhantering av bästa praxis

### 409 Konflikthantering

När du får ett 409-konfliktsvar:

1. **Tolka utvärderingsresultatet** för att förstå policyöverträdelsen
2. **Extrahera konfliktinformation** från `associatedAdvice`
3. **Presentera alternativ för användaren** baserat på din LIFO/FIFO-strategi
4. **Använd avslutningskoder** om LIFO-beteende ska implementeras

### 410 Borttagning

När du får ett svar på 410 Gone:

1. **Kontrollera om svaret har en brödtext** - indikerar fjärravslutning
2. **Tolka råd** för att förstå varför sessionen avslutades
3. **Uppdatera användargränssnittet** så att sessionens avslutning återspeglas
4. **Hantera smidigt** - sessionen kan ha nått tidsgränsen naturligt
5. **Starta en ny session** - initiera en ny session om det anses lämpligt

### Hastighetsbegränsning

När du får 429 för många begäranden:

1. **Begränsa anropsfrekvensen** till upp till 200 begäranden per minut, vilket är den högsta nivån som accepteras av CM
2. **Skicka pulsslag med nödvändigt intervall**, vilket är en per minut.

## Testverktyg

### Interaktiv API Explorer

Använd vårt [Swagger-gränssnitt](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) för interaktiv testning:

1. Ange ditt program-ID i det övre högra hörnet
2. Klicka på Utforska för att ange autentisering
3. Testa slutpunkter med riktiga parametrar
4. Visa exempel på begäran/svar
