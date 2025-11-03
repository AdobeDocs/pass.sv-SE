---
title: MVPD kickstart guide
description: MVPD kickstart guide
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# MVPD kickstart guide {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här guiden är avsedd för distributörer av flerkanalsvideo (MVPD) som planerar att integrera med Adobe® Pass Authentication.

I det här dokumentet beskrivs de viktigaste initiala stegen för att säkerställa en smidig och effektiv start på integrationsprocessen. Syftet är att förtydliga förväntningarna och ge vägledning om hur vi kommer att samarbeta med partner för att uppnå framgångsrika integreringar.

Adobe tillhandahåller en rad resurser som hjälper dig att integrera med Adobe Pass Authentication. Se **&quot;Du kommer att uppge&quot;** och **&quot;Adobe kommer att tillhandahålla&quot;** omnämnanden från varje avsnitt nedan.

>[!CAUTION]
>
> Varje gång en användare initierar ett tillståndsflöde tilldelas de ett enda ogenomskinligt, unikt användar-ID. Detta ID, som kommer från MVPD, används för att identifiera användaren i en programmerarapp.
>
> <br/>
>
> Användar-ID:t får inte innehålla någon personligt identifierbar information (PII) eller data som kan användas fristående eller i kombination med andra uppgifter för att identifiera, kontakta eller hitta användaren.

## Installationsprocess {#setup-process}

Installationsprocessen omfattar bland annat följande steg:

![Integreringsprocess för Adobe® Pass-autentisering](/help/authentication/assets/mvpd-int-lifecycle.png)

*Integreringsprocess för Adobe® Pass-autentisering*

### Kickoff {#kickoff}

**Du anger** under startfasen:

* **Visningsnamn**

  Det här är en sträng som visas på programmerarens webbplatser eller i program när användaren uppmanas att välja sin leverantör av betal-TV.

* **Logotyp-URL**

  Det här är en fil som mäter 112 x 33 pixlar och innehåller logotypen som visas på programmerarens webbplatser eller i program när användaren uppmanas att välja leverantör av betal-TV.

* **TTL (Time-to-Live)**

  TTL är ett värde som vanligtvis anges av MVPD som en del av autentiserings- eller auktoriseringsprocesserna. Adobe kan dock åsidosätta dessa TTL-värden och ange olika värden beroende på vad som överenskommits av både Programmer och MVPD.

* **Uppsättningar med autentiseringsuppgifter**

  Detta är referenser som används för att autentisera och auktorisera, eller enbart autentisera, användaren med MVPD. Vanligtvis består dessa autentiseringsuppgifter av ett användarnamn och ett lösenord, som måste anges för båda profilerna (mellanlagring och produktion).

### SAML (Metadata Exchange) {#metadata-exchange-saml}

**Adobe tillhandahåller** under metadatautbytesfasen:

* **Metadata för mellanlagringsmiljö**

  Adobe SP-metadata kan hämtas från https://sp.auth-staging.adobe.com/sp/metadata.

* **Metadata för produktionsmiljö**

  Adobe SP-metadata kan hämtas från https://sp.auth.adobe.com/sp/metadata.

**Du anger** under metadatautbytesfasen:

* **Mellanlagringsmetadata**

  MVPD metadata för testmiljön.

* **Produktionsmetadata**

  MVPD metadata för produktionsmiljön.

### Anslutningar {#connectivity}

**Du kommer att tillhandahålla** ett sätt att tillåtslista IP-adresser från Adobe, eftersom Adobe Pass Authentication kräver att brandväggar tillåter trafik via port 80 och 443 för att kunna få åtkomst till begränsade resurser under både autentiserings- och auktoriseringsprocesser.

**Du kommer att tillhandahålla** en distribution i mellanlagringsprofilen för att testa anslutningen.

### Utveckling {#development}

**Adobe kommer att ge** teknisk tid för nära samarbete med MVPD för att säkerställa att den tekniska integrationen är korrekt etablerad. I den här processen ingår utveckling av anpassad kod som är anpassad efter MVPD specifika krav.

### Distribution i mellanlagring {#deployment-staging}

**Adobe kommer att tillhandahålla** en version med de nödvändiga koduppdateringarna som först kommer att distribueras i PRE-QUAL-mellanlagringsmiljön. Under den här fasen kommer de nödvändiga konfigurationsändringarna också att implementeras för att integrera MVPD med tjänstleverantören `TestDistributors` för testningsändamål.

**Du och Adobe kommer att tillhandahålla** kvalitetskontrolltid (QA) för att säkerställa att integreringen testas korrekt i PRE-QUAL-mellanlagringsmiljön. Efter den här fasen kommer MVPD att flyttas till testmiljön RELEASE för ytterligare testning med en riktig programmerare.

### Driftsättning i produktion {#deployment-production}

**Du kommer att tillhandahålla** en distribution i produktionsprofilen för att testa anslutningen.

**Adobe kommer att tillhandahålla** en version med de nödvändiga koduppdateringarna som ska distribueras i produktionsmiljön PRE-QUAL.

**Du och Adobe kommer att tillhandahålla** tid för kvalitetssäkring (QA) för att säkerställa att integreringen kan testas med hjälp av produktionsprofilen. Om allt är bra kan Adobe flytta integreringen till produktionsmiljön RELEASE (&quot;live&quot;), som är tillgänglig för alla användare.

>[!IMPORTANT]
>
> När integreringen är klar i RELEASE-produktionsmiljön är det av största vikt att behålla en optimal kundupplevelse. För att effektivt kunna hantera servernedvända scenarier måste distributörerna tillhandahålla detaljerad eskaleringsprocedurdokumentation till Adobe för att kunna hantera sådana problem.
>
> I gengäld ser Adobe till att distributörerna får den senaste versionen av Adobe Pass eskaleringsprocess för autentisering, vilket ger en smidig problemlösning.

## Tillgång till miljöer {#access-environments}

**Adobe ger** åtkomst till miljöer för olika faser av utvecklingsprocessen:

* **Förhandskvalificering (FÖRE-KVAL)**

  PRE-QUAL-miljön är värd för nästa release-kandidaten och fungerar som den första integrationsplattformen för nya partners. Innan vi går över till RELEASE får partners tid att testa sin integrering i PRE-QUAL.

* **Utgåva (UTGÅVA)**

  I RELEASE-miljön finns den aktuella (stabila) produktionsversionen.

Mer information om hur du använder dessa miljöer finns i [Förstå Adobe-miljöer](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md) -dokumentationen.

>[!IMPORTANT]
> 
> Konfigurationsändringar i dessa miljöer måste uttryckligen begäras via din Adobe-representant efter den etablerade ändringsbegärandeprocessen.

## Tillgång till kundsupport {#access-customer-support}

**Adobe ger** åtkomst till vårt kundsupportsystem via [Zendesk](https://tve.zendesk.com/home). För att få tillgång till Zendesk måste du registrera dig och skapa ett konto på https://tve.zendesk.com/home.

Adobe Pass Authentication-teamet är tillgängligt för att beskriva eventuella frågor eller tekniska problem som kan uppstå under integreringsprocessen. Kontakta oss på [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Åtkomst till dokumentation {#access-documentation}

**Adobe ger** åtkomst till vår offentliga dokumentation via [Adobe Experience League](https://experienceleague.adobe.com/en/docs/pass/authentication/home).

Adobe Pass-autentiseringsteamet tillhandahåller omfattande dokumentation om tillgängliga funktioner och arbetsflöden i avsnittet [Integreringsguide för MVPD-program](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md). I innehållsförteckningen under det här avsnittet finns länkar till detaljerad information om varje ämne.

## Tillgång till testverktyget {#access-testing-tool}

**Adobe ger** åtkomst till vårt API:er via webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass/).
