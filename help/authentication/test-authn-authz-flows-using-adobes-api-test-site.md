---
title: Testa autentiserings- och auktoriseringsflöden med Adobe API test site
description: Testa autentiserings- och auktoriseringsflöden med Adobe API test site
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Testa autentiserings- och auktoriseringsflöden med Adobe API Test site {#How-to-test-auth-flows}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

För att testa AuthN- och AuthZ-flödena har vi förberett en **API-testwebbplats** som står till ditt förfogande. Vårt supportteam ger dig gärna information om sina uppgifter. Du kan kontakta oss på **support@tve.zendesk.com**.


## Del I {#part-I}

Gå direkt till del II om du vill testa mot RELEASE-miljön.  För att testa i förkvalificeringsmiljön, se [Konfigurera din miljö och testning i Pre-Qual](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## Del II

Utför följande steg efter att ha slutfört del I:


1. Öppna webbsida: [Testa mellanlagrings-API](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Läs in åtkomstaktivering från följande URL:
   * [Åtkomstaktivering, javascript för mellanlagring](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * ELLER
   * [Åtkomstaktivering javascript för produktion](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * Klicka sedan på&#x200B;**Aktivera inläsningsåtkomst**&quot;.
1. Ställ nu in värdet för begärande-ID till **requestID**&quot; och klicka på knappen &quot;setRequestor&quot;.
1. Tryck sedan på knappen &quot;getAuthentication&quot; och vänta tills visningsväljaren visas.
1. Välj &quot;**MVPD**&quot; från väljaren.
1. Ange dina inloggningsuppgifter på&#x200B;**MVPD**&quot; inloggningssida.
1. När du har omdirigerat dig tillbaka gör du om steg 1 till 3
1. När du har gjort om steg 3 på setAuthenticationStatus bör du se värdet 1. Om autentiseringen inte fungerar visas dialogrutan för MVPD.
1. Om du vill testa auktoriseringen anger du &quot;**requestID**&quot; och klicka på knappen &quot;getAuthorization&quot;.
1. Därför visas resursen i textrutan&quot;setToken&quot;-\>&quot;resource id&quot; och i textrutan&quot;setToken&quot;-\>&quot;token&quot; visas shortAuthorizationToken, vilket innebär att authZ lyckades.
1. Nu kan du klicka på &quot;utloggningsknappen&quot; för att ta bort token.
