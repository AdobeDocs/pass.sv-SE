---
title: Begränsningsmekanism
description: Begränsningsmekanism
source-git-commit: 1390c09d3de6b4608cdcc97b3f053cce71e84639
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---


# Begränsningsmekanism {#throttling-mechanism}

## Introduktion {#introduction}

Adobe måste i sin roll som personuppgiftsbiträde vidta lämpliga åtgärder för att se till att våra kunders användare använder resurser på rätt sätt och att tjänsten inte översvämmas av onödiga API-förfrågningar. För detta har vi infört en begränsningsmekanism.\
Ett program för övervakning av samtidig användning kan användas av flera användare och en användare kan ha flera sessioner. Tjänsten kommer därför att ha konfigurerade gränser för antalet godkända samtal per användare/session inom ett visst tidsintervall.\
När gränsen har nåtts markeras förfrågningarna med en specifik svarsstatus (HTTP 429 för många förfrågningar). Alla efterföljande anrop som görs efter att svaret&quot;429 för många förfrågningar&quot; har tagits emot bör utföras med minst en minuts nedladdningsperiod för att säkerställa att de får ett giltigt svar från verksamheten.

## Mekanismöversikt {#mechanism-overview}

Mekanismen avgör det maximala antalet godkända anrop för varje slutpunkt för övervakning av samtidighet inom ett visst tidsintervall.
När det maximala antalet samtal har nåtts svarar vår tjänst med &quot;429 För många förfrågningar&quot;. Tjänsten behöver ytterligare 60 sekunder för att initiera gränsen igen till sitt högsta värde.

Följande slutpunkter har konfigurerats med begränsning:
1. Skapa en ny session: POST /sessioner/{idp}/{subject}
2. Heartbeat-samtal: POST /sessioner/{idp}/{subject}/{sessionId}
3. Avsluta en session: DELETE /sessioner/{idp}/{subject}/{sessionId}

Begränsningen är konfigurerad på två nivåer:
1. session: samma unika {sessionId} parameter skickad `Heartbeat` ringa och `Terminate a session` ring.
2. användare: samma unika {subject} parameter skickad `Create a new session` ring.

Begränsningen för begränsning av sessionsnivå är satt till 200 begäranden inom en minut.\
Begränsningen för begränsning av användarnivå är satt till 200 förfrågningar inom en minut.\
Båda dessa begränsningar (begränsning av sessionsnivå och begränsning av användarnivå) kan konfigureras och vi uppdaterar dem om de kommer att nås via giltiga integreringsscenarier. Vi rekommenderar att supportteamet kontaktas för detta.

Här är ett scenario för begränsning av sessionsnivå:

| Tid | Begär att få skicka till CM | Antal begäranden | Svar från CM | Förklaring |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Andra 10 | POST /sessions/idp1/subject1/session1 | 50 | Alla samtal får&quot;202 Godkänt&quot; | 50 samtal har förbrukats från gränsen |
| Andra 50 | POST /sessions/idp1/subject1/session1 | 151 | 150 samtal får&quot;202 Godkänt&quot; och 1 samtal får&quot;429 För många förfrågningar&quot; | 200 samtal tas bort från gränsen och 1 samtal får 429 svar |
| Andra 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 samtal tar emot&quot;429 För många förfrågningar&quot; | Inga samtal i gränsen är tillgängliga än |
| Andra 70 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 samtal får&quot;202 Godkänt&quot; | Begränsningen är inställd på 200 tillgängliga samtal eftersom 60 sekunder har gått sedan andra 10 |

och ett scenario för begränsning på användarnivå:

| Tid | Begär att få skicka till CM | Antal begäranden | Svar från CM | Förklaring |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Andra 10 | POST /sessioner/idp1/subject1 | 50 | 50 samtal får&quot;202 Godkänt&quot; | 50 samtal har förbrukats från gränsen |
| Andra 50 | POST /sessioner/idp1/subject1 | 151 | 150 samtal får&quot;202 Godkänt&quot; och 1 samtal får&quot;429 För många förfrågningar&quot; | 200 samtal tas bort från gränsen och 1 samtal får 429 svar |
| Andra 61 | POST /sessioner/idp1/subject1 | 1 | 1 samtal tar emot&quot;429 För många förfrågningar&quot; | Inga samtal i gränsen är tillgängliga än |
| Andra 70 | POST /sessioner/idp1/subject1 | 1 | 1 samtal får&quot;202 Godkänt&quot; | Begränsningen är inställd på 200 tillgängliga samtal eftersom 60 sekunder har gått sedan andra 10 |


## Rekommendationer om kundintegrering {#customer-integration-recommendations}

Med en korrekt implementering får kunderna inte svaret&quot;429 alltför många förfrågningar&quot;.\
Adobe rekommenderar dock att varje kund hanterar svaret&quot;429 för många förfrågningar&quot; på rätt sätt med hjälp av den tekniska information som presenteras ovan.
