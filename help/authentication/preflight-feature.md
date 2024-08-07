---
title: Preflight-funktion, Så här aktiverar, felsöker eller fastställer du problemet
description: Preflight-funktion, Så här aktiverar, felsöker eller fastställer du problemet
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Preflight-funktion: Så här aktiverar, felsöker eller fastställer du problemet {#preflight-feature}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Adobe Pass Authentication beräknar preAuthorizeResources på ett annat sätt. PreAuthorization API har en ny implementering. Den här implementeringen ersätter den gamla lösningen som består av att endast göra flera auktoriseringsanrop.
Det externa gränssnittet för PreAuthorization API är oförändrat, inga uppdateringar krävs i programmerarens program.

Preflight-resurserna beräknas på tre sätt:

* **Kör och koppla metod till MVPD**: Detta innebär att Adobe gör flera auktoriseringsanrop till MVPD (klienten måste fortfarande göra ett preflight-anrop).
* **Kanaljustering**: MVPD visar kanalraden för den inloggade användaren i SAML-autentiseringssvaret och Adobe returnerar de auktoriserade resurserna baserat på detta. SAML authN-svaret i SAML-spåraren bör visa den listan.
* **Flerkanalsauktorisering**: Klient- och Adobe-autentiseringen anropar båda ett enda anrop till MVPD för en uppsättning resurser.

Oavsett vilket MVPD-värde som används kommer klientprogrammet att göra ett enda anrop till preflight-slutpunkten (checkPreauthorizedResources API) och skicka en uppsättning resurs-ID:n. Baserat på ett av de ovanstående sätten som stöds av MVPD, returnerar Adobe de förauktoriserade resurs-ID:n.

Om Preflight baseras på förgrunds- och kopplingsmetoden kontrollerar Adobe Pass Authentication-backend ett värde som angetts för det maximala förauktoriseringsanropet i dess konfiguration. Detta konfigureras av Adobe.

Standardvärdet för konfiguration med max antal förauktoriseringsanrop är 5, vilket innebär att det endast går att skicka 5 resurser i Preflight för förauktoriserings- och join-MVPD-anropen. Om du skickar fler än 5 resurser genereras ett undantag och en null-lista returneras. Detta är det förväntade beteendet. Vi kan konfigurera detta till vilket värde som helst om det virtuella dokumentfönstret inte stöder kanalindelning eller flerkanalsauktorisering, men bara efter att ha konsulterat dem eftersom flera auktoriseringsanrop för gaffel och join ökar deras laddningstid.

Detta är alltså saker som man bör tänka på när man aktiverar/felsöker preflight för ett PDF-dokument:

* Den metod som MVPD stöder (gaffel och join, channel line-up eller multi-channel).
* Om bara gaffel och join stöds måste programmeraren tillfrågas om hur många resurs-ID den ska skicka i Preflight-anropet.
* Dokumentationsdokumentet måste konsulteras och måste få veta hur det påverkar hur många förgreningar och samtidiga auktoriseringsanrop som görs. Efteråt måste värdet konfigureras i config om det är större än 5.

**Begränsning**

Observera att vi inte får tillbaka något resurs-ID från Preflight-anropet för vissa MVPD-program som AT&amp;T &amp; TWC om något av resurs-ID:n är ett falskt ID eller ett okänt ID i listan över resurs-ID:n som de skickar i preflight-anropet, även om vi har giltiga och auktoriserade resurser i den listan.
