---
title: API-slutpunkter
description: Fullständig lista över API:er för övervakning av samtidig användning
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 0%

---


# API-slutpunkter

## Hantering av kärnsession

| Slutpunkt | Metod | Beskrivning |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | Skapa en ny direktuppspelningssession |
| `/sessions/{idp}/{subject}/{session}` | POST | Skicka pulsslag för att hålla sessionen vid liv |
| `/sessions/{idp}/{subject}/{session}` | DELETE | Avsluta en session |
| `/runningStreams/{idp}/{subject}` | GET | Hämta alla aktiva sessioner för ett ämne |

## Metadatahantering

| Slutpunkt | Metod | Beskrivning |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | Hämta obligatoriska metadatafält för programmet |
