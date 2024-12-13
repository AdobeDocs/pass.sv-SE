---
title: Eskaleringsprocedurer
description: Eskaleringsprocedurer
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Eskaleringsprocedurer {#escalation-procedures}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
> 
>Ring hotline: **+1-205-693-9813** och skicka ett e-postmeddelande till **tve-support@adobe.com** inklusive **URGENT - INCIDENT** i ämnesraden.

## Introduktion {#introduction}

I det här dokumentet beskrivs supportrutinerna för större incidenter (**SEVERITY 1** -nivå) som påverkar Adobe Pass-autentisering, Adobe Pass Concurrency Monitoring och dess partners.


## Definition av en ALLVARLIGHETSincident på 1-nivå {#definition-of-a-severity-1-level-incident}

En incident på **ALLVARLIGARE nivå 1** är en **LIVE**-nivå som **inträffar i produktionsmiljön** och som inte tillåter att autentiserings- och/eller auktoriseringsflödena för en kanal och en MVPD slutförs, vilket påverkar ett stort antal prenumeranter på MVPD som utför flödet.


## Exempel på incidenter av typen SEVERITY 1 {#examples-of-severity-1-incidentcs}

* Aktiveraren för produktionsåtkomst som finns på `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (eller `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) är inte tillgänglig.

* För en viss MVPD omdirigeras/visas inte längre inloggningssidan när användaren har valt MVPD (i någon av de webbläsare som stöds).

* Partnern får ett stort antal rapporter om att användarna inte kan autentisera/auktorisera med en viss MVPD.

* Under autentiseringsprocessen fastnar användaren på en felsida i Adobe utan möjlighet att återinitiera autentiserings-/auktoriseringsflödet.


| Exempel på vad som är **NOT** en allvarlig händelse 1 |
|---|
| När det gäller frågor av denna typ kommer Adobe att ge stöd för utredningar, men det rör sig inte om incidenter med allvarlighetsgrad 1:<ul><li>En eller ett fåtal prenumeranter kan inte utföra flödet på grund av ett problem med Flashens version (Flash saknas, blockering av Flashar, fel Flash).</li><li>En eller ett fåtal prenumeranter kan inte autentisera sig och finns kvar på MVPD inloggningssida.</li><li>En eller några prenumeranter är autentiserade men kan inte spela upp videor.</li><li>Ett/ett fåtal/alla prenumeranter får ett JavaScript-fel på Programmer-webbplatsen</li></ul> |

## Allvarlighetsgrad 1 Eskaleringsflöden {#severity-1-escalation-flows}

Allvarlighetsgrad 1-incidenter kan initieras av Adobe eller en Adobe Pass-autentiseringspartner. Stegen för varje steg visas nedan.

### Partnerinitierat flöde {#partner-initiated-flow}

1. Partnern har identifierat en incident av allvarlighetsgrad 1 (som beskrivs ovan) som kräver omedelbar åtgärd från Adobe.
1. Partnern skickar ett e-postmeddelande till **tve-support@adobe.com** inklusive **URGENT - INCIDENT** i ämnesraden och lägger till följande information:
   * Titel
   * Beskrivning och steg för att återskapa
   * OS/webbläsare
   * SDK &amp; Version
   * Enheter som påverkas
   * % användare påverkas
   * HTTP-spårning eller enhetsloggar som visar problemet
   * (valfritt) Alla skärmbilder eller videoklipp som visar problemet
1. Om Adobe inte svarar på biljetten inom 30 minuter ringer partnern följande nummer:
   **1-205-693-9813**
   >[!IMPORTANT]
   >Om du inte tar med&quot;URGENT-INCIDENT&quot; i biljettens titel hämtas den inte av vårt meddelandesystem**.

### Adobe-initierat flöde {#adobe-initiated-flow}

#### ...för ett Adobe Pass-autentiseringsproblem {#adobe-initiated-flow-authn-issue}

1. Adobe identifierar ett internt problem och öppnar en biljett med vårt spårningssystem.

1. Adobe meddelar sin programansvarige och tekniska kontakt med partnern och anger biljettnumret och den beräknade effekten av ärendet.

1. Adobe arbetar för att åtgärda incidenten och håller alla berörda parter informerade.

#### ...för ett partnerproblem (Programmer/MVPD) {#adobe-initiated-flow-partner-issue}

1. Adobe identifierar ett problem som är relaterat till integrationen med en MVPD eller på någon av Programmerarens webbplatser.

1. Adobe meddelar den berörda partnern <u>efter supportrutinerna som gäller för den partnern</u> och öppnar en biljett till partnerns supportorganisation.

1. Om Adobe under konsekvensanalysen upptäcker att problemet hör till ett av de i förväg överenskomna besluten om incidentscenarier, se **I förväg överenskomna beslut om incidentscenarier**, kommer det att agera därefter utan att vänta på partnerns indata.

1. Adobe väntar på uppdateringar från partnern och ett meddelande från partnern när tjänsten har återställts.

## Föröverenskomna beslut om incidentscenarier {#pre-agreed-descn}

Det finns vissa situationer då en standardåtgärd kommer att utföras när det aktuella scenariot inträffar:

|   | Scenario | Beskrivning | Åtgärder |
|---|---|---|---|
| S1 | Adobe identifierar ett problem med MVPD integrering under normala produktionsåtgärder. | Under normala produktionsåtgärder identifierar Adobe ett problem med en av de alternativa dokumentationsdokumenten som gör det omöjligt att utföra autentiserings-/auktoriseringsflödena (t.ex. utgångna certifikat, utgångna SAML-svar, stängda portar, ändrade parametrar) | - Adobe skall underrätta MVPD och Programmerarna om detta.  </br> </br> - Adobe inaktiverar denna MVPD för alla berörda programmerare. </br> </br> - Adobe öppnar en biljett med MVPD enligt det överenskomna supportförfarandet med den MVPD |
| S2 | Adobe aktiverar en ny MVPD for a Programmer och Programmer tillåter MVPD före startdatumet. | Adobe aktiverar en ny MVPD för en programmerares webbplats, och webbplatsen visar redan den nya MVPD i väljaren, även om det inte var meningen. | - Adobe ska meddela Programmeraren om att nya MVPD visas i väljaren före det schemalagda datumet. </br> </br> - Programmeraren kommer att vidta åtgärder för att ta bort den från väljaren om det behövs. |
| S3 | Adobe aktiverar en ny MVPD for a Programmer även om MVPD inte är redo att börja producera | Adobe aktiverar en ny MVPD for a Programmer, men MVPD har ännu inte distribuerat stödet för integreringen så autentiserings-/auktoriseringsflödena kan inte utföras | - Adobe utför distributionen endast om programmeraren </br> tillfrågas om det </br> - Programmeraren ansvarar för att säkerställa tillstånd för MVPD när alla tester har utförts. |

## Förväntningar om respons vid incidenter av allvarlighetsgrad 1 {#response-expectations-for-severity-one-incidents}

* Inledande svar: 30 minuter (24/7)
* Åtgärdsplan: 1 timme (24/7)
* Upplösning: ASAP (24/7)
