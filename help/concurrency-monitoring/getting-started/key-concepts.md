---
title: Viktiga begrepp
description: Lär dig de grundläggande begreppen för övervakning av samtidig användning, inklusive sessioner, policyer, metadata med mera
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# Viktiga begrepp {#key-concepts}

Det är viktigt att förstå de centrala begreppen för övervakning av samtidig användning (Concurrency Monitoring) för att implementeringen ska lyckas. Den här guiden förklarar de grundläggande byggstenarna och hur de fungerar tillsammans.

## Kärnbegrepp {#core-concepts}

### Sessioner

En **session** representerar en aktiv videoströmningsinstans. Tänk på det som en biljett som gör att en användare kan titta på innehållet.

#### Sessionslivscykel

1. **Skapande** - När användaren börjar titta på innehåll
2. **Aktiv** - När användaren tittar (bevaras av hjärtslag)
3. **Uppsägning** - När användaren slutar titta på eller sessionen upphör

#### Sessionsegenskaper

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### Profiler

**Principer** är affärsregler som definierar begränsningar och begränsningar för samtidig direktuppspelning. De avgör när användare kan starta nya strömmar.

#### Principkomponenter

- **Regler** - Specifika begränsningar och villkor
- **Metadatakrav** - Vilka data krävs
- **Utvärderingslogik** - Hur principen tillämpas

#### Exempel

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### Metadata

**Metadata** ger kontext om varje session. Den innehåller information om enheten, innehållet, användaren och programmet.

#### Metadatakategorier

| Kategori | Exempel | Syfte |
|-----------------|------------------------------------------|---------------------------------|
| **Enhet** | `deviceId`, `deviceType`, `osName` | Identifiera och kategorisera enheter |
| **Innehåll** | `channel`, `contentType`, `assetId` | Beskriv vad som bevakas |
| **Användare** | `subject`, `subscriptionTier` | Användarspecifik information |
| **Program** | `applicationName`, `applicationPlatform` | Appspecifik information |
| **Plats** | `country`, `hba` | Geografisk information och nätverksinformation |

#### Obligatoriska och valfria metadata

- **Obligatoriska metadata** - måste anges för att sessionen ska kunna skapas
- **Valfria metadata** - Kan anges för utökade profiler
- **Standardmetadata** - fördefinierade fält tillgängliga för alla program
- **Anpassade metadata** - programspecifika fält som du definierar

## Identitet och autentisering {#identity-and-authentication}

### Identitetsleverantör (IdP)

En **identitetsleverantör** är den tjänst som autentiserar användare och tillhandahåller deras identitetsinformation. I Adobe Pass-integration är detta vanligtvis en MVPD.

#### IdP-exempel

- Kabelföretag (Comcast, Charts osv.)
- Satellitleverantörer (DirectTV, Dish)
- Direktuppspelningstjänster (Netflix, Hulu)
- Mobiloperatörer (Verizon, AT&amp;T)

### Ämne

Ett **ämne** är den unika identifieraren för en användare inom en identitetsleverantör. Detta är vanligtvis användarens konto-ID eller prenumerations-ID.

## Sessionshantering {#session-management}

### Hjärtslag

**Heartbeats** är periodiska API-anrop som håller sessionerna aktiva. De säger till systemet att &quot;den här användaren fortfarande tittar&quot;.

#### Krav för pulsslag

- **Frekvens** - var 60:e sekund (rekommenderas)
- **Syfte** - Håll sessionen aktiv, uppdatera metadata
- **Uppsägning** - Sessionen förfaller om pulsslag avbryts


### Sessionsavslut

Sessioner kan avslutas på flera sätt:

1. **Användaren slutar titta på** - DELETE-slutpunkten anropas
2. **Sessionen upphör** - Inga pulsslag har tagits emot inom tidsgränsen
3. **Principöverträdelse** - Ny session avbryter gammal session (FIFO)
4. **Fjärravslutning** - en annan enhet avbryter sessionen

## Policyutvärdering {#policy-evaluation}

### Hur policyer fungerar

1. **Användaren begär en ny ström**
2. **Systemet utvärderar principer** mot aktuell användning
3. **Beslut har fattats** - Tillåt eller neka
4. **Svaret har skickats** - Information om lyckad eller konflikt

## Konfliktlösning {#conflict-resolution}

### Vad är en konflikt?

En **konflikt** inträffar när en användare försöker starta en ny ström men skulle överskrida sina begränsningar.

### Konfliktsvar

När en konflikt inträffar returnerar systemet ett 409-svar i konflikt med detaljerad information:

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### Strategier för resolution

#### LIFO (senaste in, första ut)

- **Gamla sessioner är skyddade**
- **Ny session är blockerad**
- **Enklare gränssnitt**
- **Bättre för utökad visning**


#### FIFO (först in, först ut)

- **Ny session avbryter en befintlig session**
- **Användaren väljer vilken session som ska stoppas**
- **Mer komplext användargränssnitt krävs**
- **Bättre för innehållsväxling**

## Program och innehavare {#applications-and-tenants}

### Program

Ett **program** är ett program som integreras med Concurrency Monitoring. Varje program har ett unikt ID och kan ha egna profiler.

#### Programexempel

- Mobilapp (iOS/Android)
- Webbprogram
- Smart TV-app
- Set-top box application

### Klientorganisation

En **klientorganisation** är en organisation som äger ett eller flera program. Innehavare kan dela policyer mellan olika program.

#### Innehavarstruktur

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## Miljö {#environments}

### Mellanlagringsmiljö

- **Syfte** - Utveckling och testning
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **Referenser** - Testa program-ID:n
- **Data** - endast testdata

### Produktionsmiljö

- **Syfte** - Live-användartrafik
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **Autentiseringsuppgifter** - ID för produktionsprogram
- **Data** - Real user data

## Integreringsflöde {#integration-flow}

### Grundläggande flöde

1. **Användarautentisering** - Autentisera med Adobe Pass
2. **Skapa session** - Skapa CM-session med användarmetadata
3. **Hantering av pulsslag** - Skicka periodiska pulsslag
4. **Sessionsavslut** - Avsluta när användaren slutar titta

### Felhantering

1. **404 Det gick inte att hitta** - Hantera begäranden med sessions-ID som inte har genererats av CM-tjänsten
2. **409 Konflikter** - Hantera policyöverträdelser
3. **410 Borttagen** - Hantera sessionsavslutning
4. **Nätverksfel** - Hantera anslutningsproblem
5. **Autentiseringsfel** - Hantera autentiseringsproblem


## Gemensam terminologi {#common-terminology}

| Villkor | Definition |
|-----------------|--------------------------------------------|
| **Session** | Aktiv videoströmningsinstans |
| **Princip** | Affärsregel som definierar användningsbegränsningar |
| **Metadata** | Sammanhangsberoende information om sessioner |
| **IDP** | Identitetsleverantör (autentiseringstjänst) |
| **Ämne** | Användaridentifierare inom en IDP |
| **pulsslag** | Periodiska samtal för att hålla sessionen aktiv |
| **Konflikt** | Principfel vid start av ny ström |
| **LIFO** | Konfliktlösning för senaste ingång, första utgång |
| **FIFO** | Konfliktlösning för första in, första ut |
| **Klientorganisation** | Organisationens ägande program |
| **Program** | Program som använder CM |

