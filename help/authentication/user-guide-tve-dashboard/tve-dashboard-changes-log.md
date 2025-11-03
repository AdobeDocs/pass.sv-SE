---
title: Ändringslogg
description: Se hur en administratör kan övervaka konfigurationsändringarna i TVE Dashboard.
exl-id: 9b53a61b-679f-491e-90f3-5d827e21b32c
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Ändringslogg {#changes-log}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Med delen **Ändringslogg** på TVE-instrumentpanelen kan du visa konfigurationsändringar som skickats till Adobe Pass autentiseringsmiljö via TVE-instrumentpanelen. Du kan också jämföra två olika konfigurationsändringar.

På fliken **Ändringslogg** i den vänstra panelen visas en lista med alla konfigurationsändringar som gjorts via ett specifikt konto på TVE-instrumentpanelen. Den här listan över ändringar innehåller följande information:

* **Ändra beskrivning**: En kort beskrivning av omfattningen av konfigurationsändringen.
* **Publicerad av**: Ett e-post-ID för den användare som ansvarar för ändringen.
* **Push-datum**: Datumet för konfigurationsändringen.
* **Push-status**: Anger om push-åtgärden lyckades, väntar eller misslyckades.

## Jämför ändringar {#compare-changes}

Så här jämför du ändringar:

1. Välj två konfigurationsändringar i listan som du vill jämföra.

   ![Jämför konfigurationsändringar](/help/authentication/assets/tve-dashboard/new-tve-dashboard/review/review-changes-compare-button.png)

   *Jämför konfigurationsändringar*

1. Välj **Jämför** längst upp till höger på skärmen.

   Avsnittet **Konfigurationsändringar** visar entitetstyp, enhets-ID, egenskap och status för ändringsåtgärden för varje ändring.

1. Håll muspekaren över den konfigurationsändring som du vill visa.

1. Välj **Visa** för att komma åt de ändrade värdena.

   ![Visa konfigurationsändringar](/help/authentication/assets/tve-dashboard/new-tve-dashboard/review/review-changes-view-button.png)

   *Visa konfigurationsändringar*

Följande är ett exempel på en ändring som har gjorts i den valda konfigurationen. Du kan visa skillnaden mellan de gamla och nya värdena i ändringen.

![Gammalt och nytt värde](/help/authentication/assets/tve-dashboard/new-tve-dashboard/review/review-change-modal-view.png)

*Gammalt och nytt värde*
