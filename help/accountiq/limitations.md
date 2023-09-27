---
title: Begränsningar och kända fel
description: Kända fel i produkten.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Kända fel och begränsningar {#known-issues}

Adobe strävar efter att erbjuda robust funktionalitet och smidiga användarupplevelser genom sina erbjudanden. Den aktuella versionen (version 1.0) av konto-IQ ger användnings- och prenumerationsdelningsanalyser till direktuppspelningsleverantörer med hög grad av förtroende. Följande begränsningar kommer dock att åtgärdas i kommande versioner.

* När du definierar kohorter på kontrollpanelen eller rapportsidorna finns det för närvarande inget alternativ för att lägga till mått som **antal enheter** för att förfina segmentet. Den här funktionen kommer att finnas tillgänglig i en framtida version.

* Vid beräkning av poängdelning för enskilda konton har konto-IQ en konservativ metod som gör att företag kan agera på delning med stor tillförlitlighet. Detta tillvägagångssätt tenderar dock att underskatta det totala beloppet för delning när det slås samman över många konton.

* The [Generell delningspoäng](/help/accountiq/dashboard.md#overall-sharing-score) för närvarande bara faktorer i [Delningsnivå](/help/accountiq/dashboard.md#sharing-level) och [Användning från delade konton](/help/accountiq/dashboard.md#usage-from-shared-accounts). Framtida versioner kommer att ta hänsyn till ytterligare mätvärden.

* När du definierar kohorter på kontrollpanelen eller rapportsidorna saknar väljarna för programmeringsdokumentskydd och kanaler sökmekanismen redan nu.

* När du definierar kohorter på kontrollpanelen eller på rapportsidorna finns det en begränsning för att välja upp till 10 programmerare och programmerare (eller enskilda kanaler).

* Alternativet att exportera kontostatistik är begränsat till att exportera 1 000 konton från och med nu.

* Alternativet som ska väljas [Segmenttyp](#segment-type) när åtgärder definieras begränsas till **Fast antal konton**. The **Variabelt antal konton** i en senare version.

* Avsnitten Benchmarking, Detection Models, Segments, Snapshots och Rules i den vänstra navigeringen är för närvarande inaktiverade och kommer att vara tillgängliga i en kommande version.

* När du skapar [Operationer](/help/accountiq/operation-affecting-user-segment.md)kan du bara identifiera två typer av [Åtgärder](/help/accountiq/operation-affecting-user-segment.md) från och med nu - regler för övervakning av samtidig användning och externa åtgärder.

* För närvarande kan åtgärder bara skapas och [schemalagd](/help/accountiq/operation-affecting-user-segment.md#action). I framtida versioner kan du pausa, återuppta och hantera dem helt.

* På grund av den mer begränsade uppsättning data som används återspeglar isoleringsläget inte riktigt mängden delning. Därför går det inte att jämföra MVPD i isoleringsläge med något annat MVPD-värde. <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* När du definierar ett nytt [segment](/help/accountiq/segments-timeframe.md) för en åtgärd kan du lägga till mätvärden. Men om du markerar ett sparat segment kan du inte lägga till fler mått för att förfina segmentet.

* Grund- och tidsbildruteväljaren är begränsad till en vecka eller en månad, vilket innebär att data kan utvärderas endast för en vecka eller en månad.

* Fördefinierade intervall är för närvarande inaktiverade i granularitets- och tidsbildruteväljare och kommer att vara tillgängliga i en framtida version.
