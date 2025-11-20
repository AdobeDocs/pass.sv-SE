---
title: Eskaleringsprocedurer för övervakning av samtidig användning
description: Eskaleringsprocedurer för övervakning av samtidig användning
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Eskaleringsprocedurer för övervakning av samtidig användning {#esc-procedures}

>[!NOTE]
>
>Ring hotline: +1-657-312-4623 och skicka ett e-postmeddelande till `tve-support@adobe.com` med&quot;URGENT - INCIDENT&quot; i ämnesraden.


## Introduktion {#cm-escalation-intro}

I det här dokumentet beskrivs supportrutinerna för större incidenter (**SEVERITY 1** -nivå) som påverkar Adobe Pass-autentisering, Adobe Pass Concurrency Monitoring och dess partners.

## Definition av eskaleringsallvarlighetsgrad 1-nivå {#defn-escl-sevrityone-level}

En incident på **ALLVARLIGARE nivå 1** är en **LIVE**-nivå som **inträffar i produktionsmiljön** och som inte tillåter att autentiserings- och/eller auktoriseringsflödena för en kanal och en MVPD slutförs, vilket påverkar ett stort antal prenumeranter på MVPD som utför flödet.

## Exempel på incidenter med allvarlighetsgrad 1 {#exampl-sevone-incident}

* Aktiveraren för produktionsåtkomst som finns på <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> är inte tillgänglig.

* För en viss MVPD omdirigeras/visas inte längre inloggningssidan när användaren har valt MVPD (i någon av de webbläsare som stöds).

* Partnern får ett stort antal rapporter om att användarna inte kan autentisera/auktorisera med en viss MVPD.

* Under autentiseringsprocessen fastnar användaren på en Adobe-felsida utan möjlighet att återinitiera autentiserings-/auktoriseringsflödet.


## Exempel på vad som är *NOT* en allvarlig händelse 1 {#exampl-not-sev1}

*För problem av den här typen kommer Adobe att tillhandahålla stöd för undersökningar, men de är inte incidenter med allvarlighetsgrad 1:*

* En eller ett fåtal prenumeranter kan inte utföra flödet på grund av ett problem med Flash-versionen (Flash, Flash-blockerare saknas, fel Flash-version).
* En eller ett fåtal prenumeranter kan inte autentisera sig och finns kvar på MVPD inloggningssida.
* En eller några prenumeranter är autentiserade men kan inte spela upp videor.
* Ett/ett fåtal/alla prenumeranter stöter på ett JavaScript-fel på Programmer-webbplatsen.

## Allvarlighetsgrad 1 Eskaleringsflöden {#sevone-escalation-flows}

Allvarlighetsgrad 1-incidenter kan initieras av antingen Adobe eller en Adobe Pass-autentiseringspartner. Stegen för varje steg visas nedan.

### Partnerinitierat flöde {#partner-initiated-flow}

1. Partnern identifierar en incident av allvarlighetsgrad 1 (som beskrivs ovan) som kräver Adobe omedelbar åtgärd.

1. Partnern skickar ett e-postmeddelande till tve-support@adobe.com med&quot;URGENT - INCIDENT&quot; på ämnesraden och lägger till följande information:

   * Titel
   * Beskrivning och steg för att återskapa
   * OS
   * Webbläsare
   * Flash-version
   * (valfritt) Alla skärmbilder eller videoklipp som visar problemet

1. Om Adobe inte svarar på biljetten inom 30 minuter ringer partnern numret nedan:

   * **1-205-693-9813**


**Om du inte inkluderar &quot;URGENT-INCIDENT&quot; i biljettens titel hämtas den inte av vårt meddelandesystem.**

### Adobe-initierat flöde {#adobe-initiated-flow}

**...för ett Adobe Pass-autentiseringsproblem**

1. Adobe identifierar ett internt problem och öppnar en biljett med vårt spårningssystem.

1. Adobe meddelar partnerns programledare och tekniska kontaktperson och anger biljettnumret och den beräknade effekten av problemet.

1. Adobe arbetar för att åtgärda incidenten och håller alla berörda parter informerade.


**...för ett partnerproblem (Programmer/MVPD)**

1. Adobe identifierar ett problem som är relaterat till integrationen med en MVPD eller någon av Programmerarens webbplatser.

1. Adobe meddelar den berörda partnern **efter de supportprocedurer som gäller för den partnern** och öppnar en biljett till partnerns supportorganisation.

1. Om Adobe under konsekvensanalysen identifierar att problemet hör till ett av de på förhand överenskomna besluten om incidentscenarier (se avsnittet&quot;Förhandsöverenskomna beslut om incidentscenarier&quot; nedan), kommer det att agera därefter utan att vänta på partnern1. Indata.

1. Adobe väntar på uppdateringar från partnern och ett meddelande från partnern när tjänsten har återställts.

### Föröverenskomna beslut om incidentscenarier {#pre-agreed-decisions}

Det finns vissa situationer då en standardåtgärd kommer att utföras när det aktuella scenariot inträffar:

|    | Scenario | Beskrivning | Åtgärder |
|:--:|:------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe identifierar ett problem med MVPD integrering under normala produktionsåtgärder. | Under normala produktionsåtgärder identifierar Adobe ett problem med en av de alternativa dokumentationsdokumenten som gör det omöjligt att utföra autentiserings-/auktoriseringsflödena (t.ex. utgångna certifikat, utgångna SAML-svar, stängda portar, ändrade parametrar) | Adobe kommer att meddela de berörda MVPD- och Programmerarna. Adobe inaktiverar denna MVPD för alla programmerare som påverkas. Adobe kommer att öppna en biljett med MVPD enligt det överenskomna supportförfarandet med MVPD |
| S2 | Adobe aktiverar en ny MVPD for a Programmer och Programmer vitlistar MVPD före startdatumet. | Adobe aktiverar en ny MVPD för en programmerare och webbplatsen visar redan den nya MVPD-appen i väljaren, även om det inte var meningen. | Adobe meddelar Programmeraren om att den nya MVPD-versionen visas i väljaren före det schemalagda datumet. Programmeraren kommer vid behov att ta bort den från väljaren. |
| S3 | Adobe aktiverar en ny MVPD for a Programmer även om MVPD inte är redo att börja producera | Adobe aktiverar en ny MVPD for a Programmer, men MVPD har ännu inte distribuerat stödet för integreringen så autentiserings-/auktoriseringsflödena kan inte utföras | Adobe kommer endast att driftsätta programmet om man begär det. Programmeraren ansvarar för att MVPD vitlistas när alla tester är klara. |

### Förväntningar om respons vid incidenter av allvarlighetsgrad 1 {#response-expectations}

* Inledande svar: 30 minuter (24/7)
* Åtgärdsplan: 1 timme (24/7)
* Upplösning: ASAP (24/7)
