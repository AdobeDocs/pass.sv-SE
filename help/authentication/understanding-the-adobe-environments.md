---
title: Förstå Adobe-miljöer
description: Förstå Adobe-miljöer
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Förstå Adobe-miljöer {#understanding-the-adobe-environments}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den officiella dokumentationen som beskriver Adobe-miljöerna finns tillgänglig [Konfigurera miljön och testningen i Pre-qual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

Miljöerna i Adobe, sammanfattas med några ord:

Adobe har två miljöer: **Förkvalificering** och **Frigör**.

* I förkvalificeringsmiljön förbereder vi den nya versionen som ska släppas.

* Den aktuella versionen finns i versionsmiljön.

Varje miljö har två profiler: **mellanlagring** och **produktion**.

* Mellanlagringsprofilen ansluts till MVPD-testservern
* Produktionsprofilen ansluter till produktionsprofilen för videofilmsprogram.

Anledningen till att de två profilerna finns är att vi förbereder nya partners för publicering i staging-profilen, och de vill testa systemet med antingen den kommande versionen (förkvalificering) eller med releasen (mer stabil).

Om en partner vill testa den nya versionen finns det ytterligare några steg som måste utföras. Se, [Konfigurera miljön och testningen i Pre-qual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

Genom att följa stegen ovan kan du vara säker på att den kommande releasen kommer att testas i förkvalificeringsmiljön.
