---
title: Roku SSO - översikt
description: Om Roku SSO
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Roku SSO-översikt {#overview}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#roku-sso-intro}

Det här dokumentet beskriver den information som krävs för att dra nytta av Single Sign On-funktionen på Roku-enheter. Adobe Pass Authentication samarbetar med Roku för att förbättra användarupplevelsen vid inloggning och för att underlätta enkel inloggning på TV Everywhere-program för TV-prenumeranter.

Lösningen baseras på Adobe Pass Authentication-API:t för klientlösa REST, så de flesta programmerare behöver inte uppdatera sina program för att kunna utnyttja enkel inloggning.

## Aktivera Roku SSO {#enable-roku-sso}

Roku SSO är aktiverat som standard såvida inte programmeraren eller MVPD-begäran för enkel inloggning har inaktiverats.

Varje programmerare kan aktivera/inaktivera enkel inloggning på Roku-plattformen för specifika integreringar.

### Vad en programmerare bör göra för Roku SSO att fungera {#make-roku-sso-work}

För programmerarprogram som implementerar REST API på klientenheter fungerar Roku SSO utan ändringar. Roku OS lägger till två HTTP-huvuden vid alla förfrågningar till slutpunkterna för Adobe Pass-autentisering.

För programmerarprogram som implementerar en Server-till-server-lösning för REST api bör programmeraren kontakta Roku-teamet och be om en konfiguration där dessa två rubriker ska skickas till deras domän för alla API-flöden.

Ett prenumerant-ID som tillhandahålls på nytt ska användas i stället för det enhets-ID som skickas av programmet för att säkerställa enkel inloggning mellan program (och enheter).

Mer information om formatet på sidhuvudena får du av Adobe.

### Möjliga problem {#possible-issues}

Programmerarna bör kontrollera att deras nuvarande implementeringar baserade på Adobe Klientless REST API inte förhindrar Rokus platform-SSO. Nedan finns en lista över möjliga problem och hur de bör lösas.

| Problem | Möjlig orsak | Möjliga lösningar |
|-|-|-|
| Ingen Roku SSO-rubrik har skickats till Adobe | Använda HTTP i stället för HTTPS för anrop till Adobe Pass-autentiseringsdomäner | Använd HTTPS |
| MVPD-logotypen visas inte/uppdateras inte för SSO-tokens | Användargränssnittet förlitar sig på lokal lagring | Program bör uppdatera användargränssnittet (och lokal lagring, om det behövs) efter verifiering |
| Utloggningen har utlösts utan AuthZ | Programdesign | Programmet ska uppdateras så att det aldrig sker någon utloggning bakom kulisserna |

## Vanliga frågor {#faq}

* **Hur fungerar enkel inloggning?**

  SSO fungerar i alla programmerarprogram som drivs av Adobe Pass Authentication på alla Roku-enheter som är kopplade till samma Roku-användare.
Roku SSO tillåts inte för alla MVPD-program.

* **Kommer autentiserings-TTL:er att ändras?**

  Den första giltiga autentiseringstoken används för att utföra enkel inloggning och i det här fallet kommer alla andra program som autentiseras via enkel inloggning att använda samma TTL tills den upphör att gälla. När du navigerar från ett program till ett annat kommer det andra programmet att dela TTL-värdet för det första programmet som autentiseras.

* **Fungerar andra Adobe-funktioner som tidigare?**

  Alla Adobe Pass-autentiseringsfunktioner fungerar som tidigare.

* **Finns det en process för anmälan/avanmälan av programmerare som drar nytta av enkel inloggning på Roku-plattformen?**

  Detta kommer att bli en konfigurationsändring i Adobe TV Dashboard. Varje programmerare kan aktivera/inaktivera enkel inloggning på Roku-plattformen för specifika integreringar.
