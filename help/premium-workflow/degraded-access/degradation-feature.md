---
title: Försämringsfunktion
description: Försämringsfunktion
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Försämringsfunktion {#degradation-feature}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

I den dynamiska världen med live-sporter och större event är det viktigt att säkerställa en smidig tittarupplevelse. Hög trafik under dessa evenemang kan överbelasta autentiserings- och behörighetsslutpunkterna för Multichannel Video Programming Distributor (MVPD), vilket kan leda till förseningar och störningar.

Adobe Pass-autentisering löser dessa problem med **Försämringsfunktionen**, en lösning som gör det möjligt att tillfälligt kringgå specifika slutpunkter för MVPD autentisering och auktorisering. Den här funktionen är särskilt värdefull vid trafiktoppar, där svarstiderna kan försämras på grund av belastningen på MVPD system.

**Försämringsfunktionen** kan vara ett viktigt skydd för programmerare och säkerställa kontinuitet i tjänsten. Även om huvudmålgruppen omfattar live-sporter och nyhetskanaler, omfattar detta även alla programmerare som försöker minska risken för störningar orsakade av MVPD slutpunkter.

>[!IMPORTANT]
>
> Försämring av API är en premiumfunktion som kräver en aktuell licens från Adobe.

Genom att tillämpa en regel för nedbrytning kan programmerare tillfälligt aktivera automatisk autentisering eller automatisk auktorisering och säkerställa oavbruten åtkomst till innehållet under den period då nedgraderingen tillämpas. Försämringsåtgärder initieras alltid av en programmerare som bygger på fördefinierade avtal med sidoskyddsprogram. Även om Adobe för närvarande inte utlöser någon försämring direkt, kan framtida funktioner inkludera proaktiv hantering om servicenivåavtal (SLA) upprättas.

Den här funktionen är avsedd att användas tillsammans med ett [API för användningsövervakning](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md) och baseras på föravtal med distributörer av videoprogrammeringstjänster. Adobe Pass Authentication är ett kraftfullt verktyg för att balansera användarupplevelse, tillförlitlighet och driftskontroll under kritiska ögonblick.

>[!IMPORTANT]
>
> Den här funktionen tillåter inte att tjänsten Adobe Pass Authentication kringgås. Om Adobe Pass-autentisering inte är tillgänglig finns det ingen inbyggd mekanism i den här tjänsten som underlättar användaråtkomst. I sådana fall kan sajter eller applikationer välja att implementera sina egna alternativa routningslösningar för att behålla innehållsleveransen.

## API-åtkomst för degradering {#degradation-api-access}

Innan du kan komma åt [Försämrings-API](#degradation-api) måste du slutföra de nödvändiga stegen i DCR-processen (Dynamic Client Registration). Denna obligatoriska process ser till att du har den åtkomsttoken som krävs för att interagera med API:t för degradering.

Utförliga instruktioner finns i dokumentationen [Översikt över registrering av dynamiska klienter](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Försämring av API {#degradation-api}

Försämring-API:t är ett RESTful-API som gör att programmerare kan hantera nedbrytningsregler för specifika MVPD-program. API:t ger möjlighet att aktivera, ta bort och hämta status för de regler för nedbrytning som är aktiva.

Mer information om API:t för degradering finns i följande Zendesk-dokument [Adobe Pass Authentication | Försämring av API v3 &#x200B;](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) och leta efter den PDF-fil som ska laddas ned.

## REST API V2 {#rest-api-v2}

För att utnyttja funktionen för degradering måste du implementera koduppdateringar för att ändra hur ditt TV Everywhere-program (TVE) interagerar med Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

En utförlig guide om dessa uppdateringar och associerade arbetsflöden finns i dokumentationen för [degraderade åtkomstflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).
