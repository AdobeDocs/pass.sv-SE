---
title: Supportprocedurer Frågor och svar
description: Supportprocedurer Frågor och svar
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 0ab1fc212752dd4a4d6e12a4ab1287ef74e4a282
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Supportprocedurer Frågor och svar {#support-procedures-faqs}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

I det här dokumentet finns vanliga frågor och svar om supportprocedurer för allvarliga incidenter (nivå SEVERITY 1) som påverkar Adobe Pass Authentication och dess partners.

## Vanliga frågor {#faqs}

### Vad är en incident på 1-nivå? {#support-procedures-faqs-1}

Incidenter på nivå 1 (SEVERITY 1) är en verklighetstrogen situation i produktionsmiljön som förhindrar att autentiserings- eller auktoriseringsflöden slutförs för en kanal och en MVPD, vilket påverkar ett stort antal abonnenter.

Exempel på incidenter av typen SEVERITY 1

* Under autentiseringsprocessen omdirigeras användaren inte till inloggningssidan när användaren har valt MVPD i en webbläsare som stöds.

* Under autentiseringsprocessen fastnar användaren på en Adobe-felsida utan möjlighet att återinitiera autentiseringsflödet.

* Partnern får ett antal rapporter om att användare inte kan autentisera eller auktorisera med en viss MVPD.

* Produktionen Access Enabler på https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js är inte tillgänglig.

### Vad är en incident på nivå 1 utan ALLVARLIGHET?

Adobe kommer att stödja utredningar av dessa problem, men de betraktas inte som incidenter på nivå 1 av ALLVARLIGHET:

* En eller ett fåtal prenumeranter kan inte autentisera sig och finns kvar på MVPD inloggningssida.

* En eller några prenumeranter är autentiserade men kan inte spela upp videor.

* Vissa eller alla prenumeranter stöter på ett JavaScript-fel på Programmer-webbplatsen.

### Hur hanteras incidenter på nivå 1 av SEVERITY?

Incidenter på nivå SEVERITY 1 kan initieras av antingen Adobe eller en Adobe Pass Authentication-partner. Stegen för varje steg beskrivs nedan.

**Partnerinitierat flöde**

1. Partnern identifierar en incident på Allvarlighetsgrad 1-nivå som kräver Adobe omedelbar åtgärd.

1. Partnern skickar ett e-postmeddelande till **tve-support@adobe.com** inklusive **URGENT - INCIDENT** i ämnesraden och lägger till följande information:
   * Titel
   * Beskrivning och steg för att återskapa
   * OS/webbläsare
   * SDK &amp; Version
   * Enheter som påverkas
   * % användare påverkas
   * HTTP-spårning eller enhetsloggar som visar problemet
   * (valfritt) Alla skärmbilder eller videoklipp som visar problemet

1. Om Adobe inte svarar på biljetten inom en period kan partnern ringa följande nummer: **1-657-312-4623**.

>[!IMPORTANT]
>
> Om du inte tar med&quot;URGENT-INCIDENT&quot; i biljettens titel hämtas den inte av vårt meddelandesystem.

**Adobe-initierat flöde**

För ett Adobe Pass-autentiseringsproblem:

1. Adobe identifierar ett internt problem och öppnar en biljett i vårt spårningssystem.

1. Adobe meddelar partnerns programledare och tekniska kontaktperson och anger biljettnumret och den beräknade effekten av problemet.

1. Adobe arbetar för att lösa incidenten och håller alla berörda parter informerade.

För ett partnerproblem (Programmer/MVPD):

1. Adobe identifierar ett problem som är relaterat till integrationen med en MVPD eller någon av Programmerarens webbplatser.

1. Adobe meddelar den berörda partnern efter de supportförfaranden som gäller för den partnern och öppnar en biljett till partnerns supportorganisation.

1. Om Adobe under konsekvensanalysen identifierar att problemet omfattas av ett av de på förhand överenskomna besluten om incidentscenarier, kommer att agera därefter utan att vänta på partnerns synpunkter.

1. Adobe väntar på uppdateringar från partnern och meddelar när tjänsten har återställts.

### Vad är i förväg överenskomna beslut om incidentscenarier?

Vissa situationer med standardåtgärder som utförs om scenariot inträffar:

|    | Scenario | Beskrivning | Åtgärder |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe identifierar ett problem med MVPD integrering under normala produktionsåtgärder. | Under normala produktionsåtgärder identifierar Adobe ett problem med en av de alternativa dokumentationsdokumenten som gör det omöjligt att utföra autentiserings-/auktoriseringsflödena (t.ex. utgångna certifikat, utgångna SAML-svar, stängda portar, ändrade parametrar) | Adobe kommer att meddela de berörda MVPD- och Programmerarna.  </br></br> Adobe inaktiverar denna MVPD för alla berörda programmerare. </br></br> Adobe öppnar en biljett med MVPD enligt det överenskomna supportförfarandet med den MVPD |
| S2 | Adobe aktiverar en ny MVPD for a Programmer och Programmer tillåter MVPD före startdatumet. | Adobe aktiverar en ny MVPD för en programmerare och webbplatsen visar redan den nya MVPD-appen i väljaren, även om det inte var meningen. | Adobe meddelar Programmeraren om att den nya MVPD-versionen visas i väljaren före det schemalagda datumet. </br></br>-programmeraren kommer att vidta åtgärder för att ta bort den från väljaren om det behövs. |
| S3 | Adobe aktiverar en ny MVPD for a Programmer även om MVPD inte är redo att börja producera | Adobe aktiverar en ny MVPD for a Programmer, men MVPD har ännu inte distribuerat stödet för integreringen så autentiserings-/auktoriseringsflödena kan inte utföras | Adobe utför distributionen endast om programmeraren </br></br> begär det. Programmeraren ansvarar för att MVPD tillstånd säkerställs när alla tester har utförts. |
