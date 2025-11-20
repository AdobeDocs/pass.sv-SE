---
title: Policyinformationspunkt
description: Policyinformationspunkt
exl-id: 964bb28d-cfef-4a37-b6c4-10cc59be0b47
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Policyinformationspunkt {#pip}

>[!NOTE]
>
>Den här sidan är inaktuell eftersom den gäller den tidigare versionen av API som inte längre rekommenderas för nya integreringar

I följande diagram visas flödet om kunden väljer **principinformationspunkten**, i vilket fall CM bara används för att fråga aktiviteten och all åtkomstlogik är inbäddad i klientprogrammet):

![](../assets/pip-workflow.png)



Bilden nedan visar hur ströminventering fungerar för en användare som tittar på innehåll från två enheter.

![](../assets/pip-sequence.png)

I korthet är det vanliga meddelandeflödet följande:

1. För en användare som aldrig har använt tjänsten läses en webbsida in/ett program öppnas, där ett program med Concurrency Monitoring Service gör ett sessionsinitieringsanrop.
1. Övervakningstjänsten för samtidig användning returnerar den nya strömresursen för hjärtslag samt den aktuella användaraktiviteten.
1. Under videouppspelning gör det instrumenterade programmet hjärtslagsanrop till Concurrency Monitoring Service, vilket visar att användaren för närvarande använder en video.
1. Andra instrumenterade program kan göra statusfrågeanrop till Concurrency Monitoring Service, som returnerar den aktuella användaraktiviteten.
1. När videouppspelningen avslutas kan det instrumenterade programmet göra ett pulsslagsanrop med &quot;event=stop&quot;, vilket anger att videon har stoppats och att den aktuella strömmen inte längre ska räknas som en aktiv ström.
