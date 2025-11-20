---
title: Implementeringsmodeller
description: Implementeringsmodeller
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Implementeringsmodeller {#imp-models}

## Principer på serversidan {#ss-policies}

I den här modellen kommer CM att användas som beslutsunderlag och därmed delegeras åtkomstbeslutet till tjänsten.

Eftersom klienten inte bör anta vilka principer som tillämpas, måste implementeringen kontrollera beslutet om sessionsinitiering samt regelbundet, under uppspelning från pulsslagssvar.
