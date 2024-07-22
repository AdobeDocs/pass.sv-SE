---
title: Eskaleringsprocedurer för övervakning av samtidig användning
description: Eskaleringsprocedurer för övervakning av samtidig användning
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Eskaleringsprocedurer för övervakning av samtidig användning {#esc-procedures}

>[!NOTE]
>
>Ring hotline: +1-205-693-9813 och skicka ett e-postmeddelande till `tve-support@adobe.com` med&quot;URGENT - INCIDENT&quot; på ärenderaden.


## Introduktion {#cm-escalation-intro}

I det här dokumentet beskrivs supportrutinerna för större incidenter (**SEVERITY 1** -nivå) som påverkar Adobe Pass-autentisering, Adobe Pass Concurrency Monitoring och dess partners.

## Definition av eskaleringsallvarlighetsgrad 1-nivå {#defn-escl-sevrityone-level}

En incident på **ALLVARLIGARE nivå 1** är en **LIVE** -situation, **inträffar i produktionsmiljön**, som inte tillåter att autentiserings- och/eller auktoriseringsflödena för en kanal och en MVPD slutförs, vilket påverkar ett stort antal prenumeranter på det MVPD som utför flödet.

## Exempel på incidenter med allvarlighetsgrad 1 {#exampl-sevone-incident}

* Aktiveraren för produktionsåtkomst som finns på <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> är inte tillgänglig.

* För ett visst MVPD-dokument dirigerar inte längre Adobe om/visar inloggningssidan efter att användaren har valt MVPD (i någon av webbläsarna som stöds).

* Partnern får ett stort antal rapporter om att användarna inte kan autentisera/auktorisera med ett specifikt MVPD.

* Under autentiseringsprocessen fastnar användaren på en felsida i Adobe utan möjlighet att återinitiera autentiserings-/auktoriseringsflödet.


## Exempel på vad som är *NOT* en allvarlig händelse 1 {#exampl-not-sev1}

*För problem av den här typen ger Adobe stöd för utredningar, men de är inte incidenter av typen Allvarlighetsgrad 1:*

* En eller ett fåtal prenumeranter kan inte utföra flödet på grund av ett problem med Flashens version (Flash saknas, blockering av Flashar, fel Flash).
* En eller ett fåtal prenumeranter kan inte autentisera sig och finns kvar på inloggningssidan för MVPD.
* En eller några prenumeranter är autentiserade men kan inte spela upp videor.
* Ett/ett fåtal/alla prenumeranter stöter på ett JavaScript-fel på Programmer-webbplatsen.

## Allvarlighetsgrad 1 Eskaleringsflöden {#sevone-escalation-flows}

Allvarlighetsgrad 1-incidenter kan initieras av Adobe eller en Adobe Pass-autentiseringspartner. Stegen för varje steg visas nedan.

### Partnerinitierat flöde {#partner-initiated-flow}

1. Partnern identifierar en incident av allvarlighetsgrad 1 (som beskrivs ovan) som kräver omedelbar Adobe.

1. Partnern skickar ett e-postmeddelande till tve-support@adobe.com med&quot;URGENT - INCIDENT&quot; på ämnesraden och lägger till följande information:

   * Titel
   * Beskrivning och steg för att återskapa
   * OS
   * Webbläsare
   * Flashens version
   * (valfritt) Alla skärmbilder eller videoklipp som visar problemet

1. Om Adobe inte svarar på biljetten inom 30 minuter ringer partnern numret nedan:

   * **1-205-693-9813**


**Om du inte inkluderar &quot;URGENT-INCIDENT&quot; i biljettens titel hämtas den inte av vårt meddelandesystem.**

### Adobe initierat flöde {#adobe-initiated-flow}

**...för ett Adobe Pass-autentiseringsproblem**

1. Adobe identifierar ett internt problem och öppnar en biljett med vårt spårningssystem.

1. Adobe meddelar sin programansvarige och tekniska kontakt, med angivande av biljettnumret och den beräknade effekten av ärendet.

1. Adobe arbetar för att åtgärda incidenten och håller alla berörda parter informerade.


**...för ett partnerproblem (Programmer/MVPD)**

1. Adobe identifierar ett problem som rör integreringen med ett sidoskydd eller på en av Programmerarens sajter.

1. Adobe meddelar den berörda partnern **efter supportrutinerna som gäller för den partnern** och öppnar en biljett till partnerns supportorganisation.

1. Om Adobe under konsekvensanalysen konstaterar att problemet hör till ett av de på förhand överenskomna besluten om incidentscenarier (se avsnittet&quot;Förhandsöverenskomna beslut om incidentscenarier&quot; nedan), kommer det att agera därefter utan att vänta på partnern1. Indata.

1. Adobe väntar på uppdateringar från partnern och ett meddelande från partnern när tjänsten har återställts.

### Föröverenskomna beslut om incidentscenarier {#pre-agreed-decisions}

Det finns vissa situationer då en standardåtgärd kommer att utföras när det aktuella scenariot inträffar:

|    | Scenario | Beskrivning | Åtgärder |
|:---:|:---|:---|:---|
| S1 | Adobe identifierar ett problem med integreringen av ett sidoskydd under normala produktionsåtgärder. | Under normala produktionsåtgärder identifierar Adobe ett problem med en av de alternativa dokumentationsdokumenten som gör det omöjligt att utföra autentiserings-/auktoriseringsflödena (t.ex. utgångna certifikat, utgångna SAML-svar, stängda portar, ändrade parametrar) | Adobe ska underrätta de berörda programmerarna och distributörerna om detta. Adobe kommer att avaktivera denna MVPD för alla berörda programmerare. Adobe kommer att öppna en biljett med det godkända sidoskyddet enligt det överenskomna stödförfarandet med detta sidoskydd |
| S2 | Adobe aktiverar ett nytt MVPD för en programmerare och programmeraren vitlistar MVPD före startdatumet. | Adobe aktiverar ett nytt PDF-dokument för en programmerares webbplats och webbplatsen visar redan det nya PDF-dokumentet i väljaren, även om det inte var meningen. | Adobe meddelar programmeraren om det nya MVPD-värdet som visas i väljaren före det schemalagda datumet. Programmeraren kommer vid behov att ta bort den från väljaren. |
| S3 | Adobe aktiverar ett nytt MVPD för en programmerare även om MVPD inte är redo att gå i produktion | Adobe aktiverar ett nytt MVPD för en programmerare, men MVPD har ännu inte distribuerat stödet för integreringen så autentiserings-/auktoriseringsflödena kan inte utföras | Adobe kommer endast att genomföra driftsättningen om programmeraren så begär. Programmeraren ansvarar för att vitlistningen av det virtuella dokumentationsdokumentet görs när alla tester har utförts. |

### Förväntningar om respons vid incidenter av allvarlighetsgrad 1 {#response-expectations}

* Inledande svar: 30 minuter (24/7)
* Åtgärdsplan: 1 timme (24/7)
* Upplösning: ASAP (24/7)
