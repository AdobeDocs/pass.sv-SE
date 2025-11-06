---
title: Begränsningsmekanism
description: Begränsningsmekanism
exl-id: 15236570-1a75-42fb-9bba-0e2d7a59c9f6
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# Begränsningsmekanism {#throttling-mechanism}

## Introduktion {#introduction}

Adobe måste i sin roll som personuppgiftsbiträde vidta lämpliga åtgärder för att se till att våra kunders användare använder resurser på rätt sätt och att tjänsten inte översvämmas av onödiga API-förfrågningar. För detta har vi infört en begränsningsmekanism.
Ett program för övervakning av samtidig användning kan användas av flera användare och en användare kan ha flera sessioner. Tjänsten kommer därför att ha konfigurerade gränser för antalet godkända samtal per användare/session inom ett visst tidsintervall.
När gränsen har nåtts markeras förfrågningarna med en specifik svarsstatus (HTTP 429 för många förfrågningar). Alla efterföljande anrop som görs efter att ett svar på&quot;429 Alltför många begäranden&quot; har tagits emot bör utföras med minst en minuts nedkylningsperiod för att säkerställa att det får ett giltigt svar från verksamheten.

## Mekanismöversikt {#mechanism-overview}

Mekanismen avgör det maximala antalet godkända anrop för varje slutpunkt för övervakning av samtidighet inom ett visst tidsintervall.
När det maximala antalet samtal har nåtts svarar vår tjänst med &quot;429 För många förfrågningar&quot;. 429-svarsrubriken&quot;Förfaller&quot; innehåller tidsstämpeln när nästa anrop anses vara giltigt eller när begränsningen upphör att gälla. Just nu upphör begränsningen efter en   från första 429 svaret.

Följande slutpunkter har konfigurerats med begränsning:
1. Skapa en ny session: POST /sessions/{idp}/{subject}
2. Heartbeat-anrop: POST /sessions/{idp}/{subject}/{sessionId}
3. Avsluta en session: DELETE /sessions/{idp}/{subject}/{sessionId}

Begränsningen är konfigurerad på två nivåer:
1. session: samma unika {sessionId}-parameter skickades i `Heartbeat` anrop och `Terminate a session` anrop.
2. användare: samma unika {subject}-parameter skickades i `Create a new session`-anropet.

Begränsningen för begränsning av sessionsnivå är satt till 200 begäranden inom en minut.\
Begränsningen för begränsning av användarnivå är satt till 200 förfrågningar inom en minut.\
Båda dessa begränsningar (begränsning av sessionsnivå och begränsning av användarnivå) kan konfigureras och vi uppdaterar dem om de kommer att nås via giltiga integreringsscenarier. Vi rekommenderar att supportteamet kontaktas för detta.

**Scenario för begränsning av sessionsnivå:**

| Tid | Begär att få skicka till CM | Antal begäranden | Svar från CM | Förklaring |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Andra 10 | POST /sessions/idp1/subject1/session1 | 50 | Alla samtal får&quot;202 Godkänt&quot; | 50 samtal har förbrukats från gränsen |
| Andra 50 | POST /sessions/idp1/subject1/session1 | 151 | 150 samtal får&quot;202 Godkänt&quot; och 1 samtal får&quot;429 För många förfrågningar&quot; | 200 samtal tas bort från gränsen och 1 samtal får 429 svar |
| Andra 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 samtal tar emot&quot;429 För många förfrågningar&quot; | Inga samtal i gränsen är tillgängliga än |
| Andra 70 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 samtal får&quot;202 Godkänt&quot; | Begränsningen är inställd på 200 tillgängliga samtal eftersom 60 sekunder har gått sedan andra 10 |

**Scenario för begränsning på användarnivå:**

| Tid | Begär att få skicka till CM | Antal begäranden | Svar från CM | Förklaring |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Andra 10 | POST /sessions/idp1/subject1 | 50 | 50 samtal får&quot;202 Godkänt&quot; | 50 samtal har förbrukats från gränsen |
| Andra 50 | POST /sessions/idp1/subject1 | 151 | 150 samtal får&quot;202 Godkänt&quot; och 1 samtal får&quot;429 För många förfrågningar&quot; | 200 samtal tas bort från gränsen och 1 samtal får 429 svar |
| Andra 61 | POST /sessions/idp1/subject1 | 1 | 1 samtal tar emot&quot;429 För många förfrågningar&quot; | Inga samtal i gränsen är tillgängliga än |
| Andra 70 | POST /sessions/idp1/subject1 | 1 | 1 samtal får&quot;202 Godkänt&quot; | Begränsningen är inställd på 200 tillgängliga samtal eftersom 60 sekunder har gått sedan andra 10 |

**429 Exempel på svar:**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## Rekommendationer om kundintegrering {#customer-integration-recommendations}

Med en korrekt implementering får kunderna inte svaret&quot;429 alltför många förfrågningar&quot;.
Adobe rekommenderar dock att varje kund hanterar svaret&quot;429 för många förfrågningar&quot; på rätt sätt med hjälp av den tekniska information som presenteras ovan. När svaret hanteras bör rubriken&quot;Förfaller&quot; användas för att avgöra när nästa giltiga begäran ska skickas.
