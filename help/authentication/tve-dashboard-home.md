---
title: Kontrollpanel
description: Läs mer om startsidan för TVE Dashboard.
source-git-commit: 06c2e1e54515a2ec47722ba1f360467dadd1f73b
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Kontrollpanel {#dashboard}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

The **Kontrollpanel** i den vänstra panelen fungerar som hemsida för Adobe Pass Authentication TVE Dashboard.

Det finns två avsnitt på hemsidan:

* [Välkomstskärm](#welcome-screen)
* [Konfigurationsstatus](#configuration-status)

## Välkomstskärm {#welcome}

I det här avsnittet kan du komma åt den offentliga dokumentationen direkt från välkomstmeddelandet och visa en ögonblicksbild av dina aktuella konfigurationer.

* **Aktiva integreringar**: Antalet aktiva integreringar i den aktuella miljön. Välj **Visa mer i integreringsavsnittet** för att få tillgång till detaljerad information i [Integreringar](tve-dashboard-integrations.md) -avsnitt.
* **Aktiva kanaler**: Antalet aktiva kanaler i den aktuella miljön. Välj **Visa mer i avsnittet Kanaler** för att få tillgång till detaljerad information i [Kanaler](tve-dashboard-channels.md) -avsnitt.
* **Databasuppdateringar**: Antal konfigurationsändringar som gjorts i den aktuella miljön. Välj **Visa mer i avsnittet Ändringslogg** för att få tillgång till detaljerad information i [Ändringslogg](tve-dashboard-changes-log.md) -avsnitt.
* **ESM-kontrollpanel**: Håll utkik efter den kommande ESM Dashboard-panelen med djupgående mått på egenskapsanvändning i den aktuella miljön. Den här funktionen kommer att vara tillgänglig i framtida uppdateringar.

![Välkomstskärm](assets/welcome-screen.png)

*Välkomstskärm*

## Konfigurationsstatus {#conf-status}

I det här avsnittet presenteras 10 senaste konfigurationsändringar som inkluderar:

* **Ändringsbeskrivning**: En kort beskrivning av ändringen som valts av användaren.
* **Skickat av**: Det konto som ansvarar för ändringen.
* **Push-datum**: Det datum då ändringen gjordes.

![Konfigurationsstatus för en ändringslogg](assets/configuration-status.png)

*Konfigurationsstatus för en ändringslogg*

Om du vill visa hela listan med ändringar väljer du **Visa mer i ändringsloggen** längst ned till höger för att visa [Ändringslogg](tve-dashboard-changes-log.md) -avsnitt.
