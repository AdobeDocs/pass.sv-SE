---
title: Isoleringsläge MVPD
description: Lär dig mer om programmerare som arbetar med isoleringsläge för videoprogrammerare för TV Everywhere
source-git-commit: 5639319ce8915f0c33d927ca9554c405b3b2e87d
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---


# programmerare för Isoleringsläge för videoprogrammerare för TV Everywhere {#isolation-mode-tve}

>[!IMPORTANT]
>
> Begränsning av MVPD-värden för isoleringsläge gäller endast för programmerare för TV Everywhere.

I Isoleringsläge identifierar programmerings- och videoprogrammerare (till exempel Xfinity) konsekvent abonnenter på olika enheter baserat på deras interaktioner med specifika programmerare. I standardläget identifierar programmeringsprogrammeringsgränssnitten konsekvent abonnenter på olika enheter oavsett vilka programmerare som är inblandade.

Här är ett exempel:

![](assets/isolation-diff-new.png)

*Isoleringsläge MVPD identifierar fyra olika abonnenter i stället för två*

* Om en prenumerant B i ett Isolation Mode MVPD (t.ex. Xfinity) får åtkomst till det innehåll som erbjuds av två olika programmerare med samma enhet kopplas olika identifierare till de två olika åtkomstförsöken. Det verkar som om det finns två olika prenumeranter som får åtkomst till programmerarnas innehåll (L och M i figuren).

* Om prenumerant B får åtkomst till innehåll som erbjuds av två olika programmerare för standardprogrammeringsdokument, associerar MVPD en enda åtkomstidentifierare för båda åtkomstförsöken.

* MVPD-filer (t.ex. Xfinity) i isoleringsläge identifierar inte konsekvent en prenumerant, även om prenumeranten använder samma enhet för olika programmerare.

För att förhindra förvrängning av data som orsakas av att en prenumerant räknas som flera prenumeranter på grund av åtkomst till olika programmerare, begränsar Isoleringsläge den rapporterade aktiviteten om en programmerare till enbart deras program.

Programmer L kan till exempel bara visa data baserat på aktiviteten för Identiteter W och Y, och ignorerar Identiteter X och Z i den föregående bilden.

>[!IMPORTANT]
>
> Nackdelen är att programmeraren L inte längre kan dela information som samlats in om prenumeranter A och B på grund av aktivitet med någon annan programmerare än L.

I isoleringsläge beräknas delningspoäng och tillhörande mätvärden enbart utifrån aktiviteten hos enheter som direktuppspelas från den valda programmerarens och kanalens applikationer. Delningspoängen och sannolikheterna beräknas från dataströmmen startar på de valda kanalerna.

Systemet arbetar automatiskt i isoleringsläge när det valda segmentet innehåller ett MVPD-värde för isoleringsläge som identifierar enskilda prenumeranter som flera prenumeranter vid direktuppspelning från olika programmerare. Alla diagram och diagram för dessa segment återspeglar resultatet av detta ändrade beteende.

>[!IMPORTANT]
>
> Beteendet i isoleringsläge är inte kompatibelt med standardläget. Isoleringsläge MVPD kan inte blandas med andra MVPD och vice versa.

Om du vill skapa ett segment som analyseras i isoleringsläge drar du Isoleringsläget MVPD, till exempel **Xfinity**, till segmentdefinitionens MVPD-avsnitt.

>[!NOTE]
>
> Eftersom det inte går att blanda flerkanalsläge (MVPD) med andra flerkanalsläge (MVPD), kommer segmentdefinitionens MVPD-avsnitt inte att tillåta att ytterligare ett flerkanalsläge dras dit.

![](assets/xfinity-in-segment.png)

*Xfinity-markering i isoleringsläge*

>[!IMPORTANT]
>
> Kontodelning är mer relevant när den mäts för direktuppspelning i alla programmerarens program. Förvänta dig lägre **Dela bakgrundsmusik** och vissa variationer i mätvärdena i isoleringsläge.

![](assets/aggregate-sharing-isolation.png)

*Dela sannolikhetsmätare i isoleringsläge*

I ovanstående mått visas endast 9 % av alla konton som delas, och bland dessa används endast 11 % av innehållet. På grund av de naturligt lägre poängen bör resultaten i isoleringsläge tolkas annorlunda än resultaten i standardläge.
