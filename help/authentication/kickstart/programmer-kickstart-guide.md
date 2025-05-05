---
title: Programmeraren kickstart
description: Programmeraren kickstart
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 37858fa83aecbdf443a4a6058c78e4f9246eee42
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Programmeraren kickstart {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här guiden är avsedd för innehållsleverantörer (programmerare) som tänker integrera Adobe® Pass Authentication i sina webbplatser eller tillämpningar.

I det här dokumentet beskrivs de viktigaste initiala stegen för att säkerställa en smidig och effektiv start på integrationsprocessen. Syftet är att förtydliga förväntningarna och ge vägledning om hur vi kommer att samarbeta med partner för att uppnå framgångsrika integreringar.

Adobe tillhandahåller en rad resurser som hjälper dig att integrera Adobe Pass Authentication i din webbplats eller tillämpning. Se **&quot;Du kommer att uppge&quot;** och **&quot;Adobe kommer att tillhandahålla&quot;** omnämnanden från varje avsnitt nedan.

## Installationsprocess {#setup-process}

Installationsprocessen omfattar bland annat följande steg:

![Integreringsprocess för autentisering i Adobe® Pass](../assets/progr-flow-int-lifecycle.png)

*Integreringsprocess för autentisering i Adobe® Pass*

**Du anger** under startfasen:

* **Tjänstleverantör (begärande-ID)**

  Detta är en sträng som unikt identifierar varumärket för webbplatsen eller det program som begär Adobe Pass Authentication. Själva strängen är godtycklig men måste avtalas mellan Adobe och programmeraren

* **Kanalinformation**

  Detta är en uppsättning strängar som används för att identifiera innehållskanaler som begärts av tjänsteleverantören. I många fall är kanal- och tjänsteleverantören densamma. En enda identifierare kan dock representera flera innehållskanaler. De här kanalnamnssträngarna ska justeras mot motsvarande kabel-TV-kanaler. Observera att vissa MVPD-program kan validera det här värdet under autentiserings- och/eller auktoriseringsprocessen.

* **Domännamn**

  Listan kommer att innehålla de faktiska domännamnen som anges för Adobe för att representera tjänsteleverantören. Det ser till att bara dina auktoriserade domäner har åtkomst till Adobe Pass Authentication med dina metadata. Se till att du anger och tydligt identifierar domännamn för både produktions- och testmiljöer, eftersom dessa kan skilja sig åt.

**Du kommer att tillhandahålla** via MVPD:

* **Uppsättningar med autentiseringsuppgifter**

  Detta är referenser som används för att autentisera och auktorisera, eller enbart autentisera, användaren med MVPD. Vanligtvis består dessa uppgifter av ett användarnamn och lösenord som MVPD skickar till dig för båda profilerna (testning och produktion).

* **Resursidentifierare**

  Det här är unika identifierare för innehållskanaler, program, avsnitt eller resurser som tjänsteleverantören vill skydda. Dessa identifierare används för att begära auktoriseringsbeslut och måste överenskommas med MVPD.

>[!IMPORTANT]
>
> Programmeraren ansvarar för att samordna med MVPD för att slutföra alla nödvändiga affärsavtal. Under tiden kommer Adobe Pass Authentication att samarbeta med MVPD för att säkerställa att den tekniska integreringen är korrekt etablerad:
>
> * **Ny MVPD**
>
>     Om MVPD inte är integrerat med Adobe måste man utveckla anpassad kod som bygger på MVPD specifika krav. MVPD kommer inte att vara tillgängligt förrän denna utveckling är klar, och produkttestningen med denna MVPD kan inte fortsätta.
>
> * **Befintlig MVPD**
>
>     Om MVPD redan är integrerat med Adobe effektiviseras anslutningsprocessen avsevärt. I de flesta fall kan anslutningen upprättas snabbt med hjälp av konfigurationsjusteringar i stället för genom omfattande utveckling.
>
> Alla integreringar kräver gemensamma kvalitetskontroller, inklusive testning av MVPD, eftersom slutanvändaren i slutändan är kund hos MVPD. Samordningen av testcykler beror ofta på MVPD resurstillgänglighet, vilket kan medföra potentiella förseningar.

## Tillgång till kundsupport {#access-customer-support}

**Adobe ger** åtkomst till vårt kundsupportsystem via [Zendesk](https://tve.zendesk.com/home). För att få tillgång till Zendesk måste du registrera dig och skapa ett konto på https://tve.zendesk.com/home. Det finns ingen gräns för hur många användare du kan registrera. När du har registrerat dig kan du visa och dela kommentarer på alla skickade biljetter.

Adobe Pass autentiseringsteam är tillgängliga för att hjälpa dig med eventuella frågor eller tekniska problem som du kan stöta på under integreringsprocessen. Kontakta oss på [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Åtkomst till dokumentation {#access-documentation}

**Adobe ger** åtkomst till vår offentliga dokumentation via [Adobe Experience League](https://experienceleague.adobe.com/sv/docs/pass/authentication/home).

Adobe Pass Authentication-teamet tillhandahåller omfattande dokumentation om tillgängliga funktioner och API:er i avsnittet [Integreringsguide för programmerare](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md). I innehållsförteckningen under det här avsnittet finns länkar till detaljerad information om varje ämne.

## Tillgång till testverktyget {#access-testing-tool}

**Adobe ger** åtkomst till vårt API:er via webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass/).

## Åtkomst till konfigurationshanteringsverktyget {#access-configuration-management-tool}

**Adobe ger** åtkomst till ett självbetjäningsverktyg för att hantera din konfiguration och dina data via [Adobe Pass TVE Dashboard](https://experience.adobe.com/pass/authentication).

Adobe Pass autentiseringsteam tillhandahåller omfattande dokumentation om användningen av TVE Dashboard under avsnittet [Användarhandbok för TVE Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md). I innehållsförteckningen under det här avsnittet finns länkar till detaljerad information om varje ämne.
