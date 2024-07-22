---
title: Förstå användar-ID:n
description: Förstå användar-ID:n
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 0%

---

# Förstå användar-ID:n {#understanding-user-ids}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Varje användare som initierar ett tillståndsflöde är kopplad till ett enda unikt användar-ID. Under ett tillståndsflöde kan dock ett användar-ID visas på olika sätt, beroende på vilket API du hämtar ID från.

`sessionGUID` i Short Media Token är den säkra formen av användar-ID, som är tillgänglig via `sendTrackingData()`-anropet. I alla aktuella integreringar är detta ett beständigt GUID för användaren över tid och enheter. Källan för GUID börjar med användar-ID:t från det autentiseringssvar som kommer från MVPD. Men vissa programmeringsgränssnitten kan ändra sig i framtiden och börja skicka ett tillfälligt GUID. Om en programmerare vill säkerställa att användar-ID:t för MVPD-källan i AuthN-svaret är beständigt, bör de ordna det i sina avtal med MVPD-program.

Här beskrivs olika sätt som användar-ID representeras i Adobe Pass autentiserings-API:erna:

| Egenskap | Syfte | Hård | Digitalt signerad | Beskrivning |
| --- | --- | --- | --- | --- |
| sendTrackingData(), GUID, egenskap | Spårning/analys | Ja | Nej | - MVPD-användar-ID:t, hashas av Adobe. Användar-ID:t går inte att spåra tillbaka till källan till MVPD. </br> </br> - Den här formen av ID:t har inte signerats digitalt, så den är inte säker för att förhindra bedrägeri. Men det räcker bra för analyser.  </br> </br> - Den här formen av användar-ID tillhandahålls på klientsidan för alla händelser som genereras av Adobe Pass Authentication i AuthN/AuthZ-flödet. |
| Egenskapen sessionGUID för Short Media Token | Bedrägerispårning av samtidig användning | Ja | Ja | - Detta är samma som användar-ID:t via sendTrackingData(), men det här signeras digitalt för att skydda dess integritet och är tillräckligt bra för att kunna användas för bedrägerispårning. </br> </br> - Den är avsedd att bearbetas på serversidan när du har använt vårt valideringsbibliotek och kan analyseras för att upptäcka eventuella bedrägerimönster innan videoströmmen släpps till klienten.  Det är programmeraren som bestämmer om något av detta ska utföras. |
| getMetadata(), egenskap för användar-ID | Kontolänkning, bedrägeriutredning med MVPD | Nej | Nej | - Den här egenskapen gör att Adobe kan visa MVPD-användar-ID:t för den faktiska källan för programmeraren. </br> </br> - I Adobe-konfigurationen kan den anges som krypterad eller inte (beroende på inställningen för MVPD). Om den är krypterad krypteras den med den offentliga nyckeln från Programmerarens certifikat som tillhandahålls Adobe, så att den inte visas tydligt för kunden. </br> </br> - Detta ger programmeraren det faktiska användar-ID:t från MVPD, så det är något som kan användas för kontolänkning eller bedrägeriutredning direkt med MVPD. |


**Slutsats**

* I allmänhet tillhandahåller MVPD ett beständigt unikt ID <u> och skickar det till Adobe vid lyckad autentisering</u>. Den är i allmänhet enhetlig i alla nätverk. Undantaget är Comcast MVPD, som tillhandahåller olika användar-ID för varje kanal.

* MVPD-användar-ID:t innehåller inte PII och är INTE ett kontonummer. Den behöver inte exponeras i krypterad form eftersom vi har verifierat med alla de alternativa dokumentationsdokumenten att ingen PII skickas.

Hur du använder användar-ID beror på användningsfallet:

* Om du behöver det för spårning/analys är det mest praktiska stället att hämta det från `sendTrackingData()`.
* Om du behöver det på serversidan för att kunna frigöra dataströmmar, utföra bedrägeri eller använda data kan du hämta det från Media Token-valideraren.
* Om du behöver det för kontolänkning och djupare bedrägerier bör du kontakta Adobe för att få reda på om de finns tillgängliga.
