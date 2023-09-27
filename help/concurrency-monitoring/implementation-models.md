---
title: Implementeringsmodeller
description: Implementeringsmodeller
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---


# Implementeringsmodeller {#imp-models}

## Principer på serversidan {#ss-policies}

I den här modellen kommer CM att användas som beslutsunderlag och därmed delegeras åtkomstbeslutet till tjänsten.

Eftersom klienten inte bör anta vilka principer som tillämpas, måste implementeringen kontrollera beslutet om sessionsinitiering samt regelbundet, under uppspelning från pulsslagssvar.
