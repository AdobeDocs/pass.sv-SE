---
title: Roku SSO Cookbook (REST API V2)
description: Roku SSO Cookbook (REST API V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Roku SSO Cookbook (REST API V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Adobe Pass Authentication REST API V2 har stöd för Platform Single Sign-On (SSO) för slutanvändare av klientprogram som körs på RokuOS.

Det här dokumentet fungerar som ett tillägg till den befintliga [REST API V2-översikten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) som ger en högnivåvy och det dokument som beskriver hur du implementerar [enkel inloggning med plattformsidentitetsflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Roku enkel inloggning med plattformsidentitetsflöden {#cookbook}

Adobe Pass Authentication samarbetar med Roku för att förbättra användarupplevelsen vid inloggning och för att underlätta enkel inloggning (SSO) i TV Everywhere-program för TV-prenumeranter.

### Förutsättningar {#prerequisites}

Kontrollera att Roku SSO är aktiverat innan du fortsätter med Roku enkel inloggning med plattformsidentitetsflöden. Roku SSO är aktiverat som standard såvida inte Programmer eller MVPD begär att SSO ska inaktiveras.

Varje programmerare kan aktivera eller inaktivera enkel inloggning (SSO) på Roku-plattformen för specifika integreringar via [Adobe Pass TVE Dashboard](https://experience.adobe.com/pass/authentication).

### Arbetsflöde {#workflow}

**Klient-till-server**

För programmeringsprogram som använder en klient-till-server-arkitektur för att integrera REST API V2, fungerar Roku SSO sömlöst utan ändringar.

RokuOS lägger automatiskt till två HTTP-huvuden i alla begäranden som skickas till slutpunkterna för Adobe Pass-autentisering.

**Server-till-server**

För programmeringsprogram som använder en Server-till-Server-arkitektur för att integrera REST API V2 måste programmeraren samordna med Roku-teamet för att konfigurera dessa huvuden så att de inkluderas i alla API-flöden som är kopplade till deras domän.

För att aktivera enkel inloggning mellan program och enheter bör det Roku-tillhandahållna prenumerant-ID:t användas i stället för enhets-ID:t när det skickas av programmet.

Mer information finns i följande dokumentation:

* [Header - X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Header - AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Kontakta Adobe om du vill ha mer information om sidhuvudenas format.

### Vanliga frågor {#faqs}

* **Hur fungerar enkel inloggning?**

  SSO fungerar i alla programmerarprogram som drivs av Adobe Pass Authentication på alla Roku-enheter som är kopplade till samma Roku-användare. Roku SSO tillåts inte för alla MVPD-program.


* **Kommer autentiserings-TTL:er att ändras?**

  Den första giltiga autentiseringstoken används för att utföra enkel inloggning och i det här fallet kommer alla andra program som autentiseras via enkel inloggning att använda samma TTL tills den upphör att gälla. När du navigerar från ett program till ett annat kommer det andra programmet att dela TTL-värdet för det första programmet som autentiseras.


* **Fungerar andra Adobe-funktioner som tidigare?**

  Alla Adobe Pass-autentiseringsfunktioner fungerar som tidigare.


* **Finns det en process för anmälan/avanmälan av programmerare som drar nytta av enkel inloggning på Roku-plattformen?**

  Detta blir en konfigurationsändring i Adobe TVE Dashboard. Varje programmerare kan aktivera eller inaktivera enkel inloggning på Roku-plattformen för specifika integreringar.


* **Vad är några vanliga problem?**

  Programmerarna bör kontrollera att deras nuvarande implementeringar baserade på Adobe REST API inte förhindrar Rokus platform-SSO.

  Nedan finns en lista över möjliga problem och hur de bör lösas.

| Problem | Möjlig orsak | Möjliga lösningar |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Ingen Roku SSO-rubrik har skickats till Adobe | Använda HTTP i stället för HTTPS för anrop till Adobe Pass-autentiseringsdomäner | Använd HTTPS |
| MVPD logotyp visas inte/uppdateras inte för SSO-tokens | Användargränssnittet förlitar sig på lokal lagring | Program bör uppdatera användargränssnittet (och lokal lagring, om det behövs) efter verifiering |
| Utloggningen har utlösts utan AuthZ | Programdesign | Programmet ska uppdateras så att det aldrig sker någon utloggning bakom kulisserna |
