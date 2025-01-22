---
title: Testa autentiserings- och auktoriseringsflöden med Adobe API test site
description: Testa autentiserings- och auktoriseringsflöden med Adobe API test site
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 811feba1f2476bdfacb20e332e33df7f7ae8ac00
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# (Äldre) Så här testar du autentiserings- och auktoriseringsflöden med hjälp av Adobe API Test site {#How-to-test-auth-flows}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

För att testa AuthN- och AuthZ-flödena har vi förberett en **API-testwebbplats** som du har till ditt förfogande. Vårt supportteam ger dig gärna information om sina uppgifter. Kontakta oss på **tve-support@adobe.com**.


## Del I {#part-I}

Gå direkt till del II om du vill testa mot RELEASE-miljön.  Om du vill testa i förkvalificeringsmiljön läser du [Konfigurera miljön och testningen i Pre-Quality](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

## Del II

Utför följande steg efter att ha slutfört del I:


1. Öppna webbsida: [Testa mellanlagrings-API](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Läs in åtkomstaktivering med:
   * Välj i listrutan vilken version av AccessEnabler som behövs (v3 eller v4), varifrån du vill komma åt den (testning eller produktion) och om den ska vara i felsökningsläge
   * Ange programsatsen som du vill testa med v4
   * Klicka sedan på knappen **Läs in åtkomstaktivering**.
1. Ställ nu in beställar-ID-värdet till **beställar-ID** och klicka på knappen setRequestor.
1. Tryck sedan på knappen &quot;getAuthentication&quot; och vänta tills visningsväljaren visas.
1. Välj **MVPD** i väljaren.
1. Ange dina autentiseringsuppgifter på inloggningssidan för **MVPD**.
1. När du har omdirigerat dig tillbaka gör du om steg 1 till 3
1. När du har gjort om steg 3 på setAuthenticationStatus bör du se värdet 1. Om autentiseringen inte fungerar visas dialogrutan MVPD.
1. Om du vill testa auktoriseringen anger du **resource** som du vill auktorisera i indatafältet till höger om knapparna checkAuthorization och getAuthorization och klickar på getAuthorization.
1. Därför visas resursen i textrutan&quot;setToken&quot;-\>&quot;resource id&quot; och i textrutan&quot;setToken&quot;-\>&quot;token&quot; visas shortAuthorizationToken, vilket innebär att authZ lyckades.
1. Nu kan du klicka på &quot;utloggningsknappen&quot; för att ta bort token.
