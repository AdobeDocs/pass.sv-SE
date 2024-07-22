---
title: Aktivera Adobe Pass Entitlement Services för en programmerare på Xbox 360 och XboxOne ClientCloud
description: Aktivera Adobe Pass Entitlement Services för en programmerare på Xbox 360 och XboxOne ClientCloud
exl-id: ff7254de-9ea4-4c27-a186-d1c2eea12222
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Aktivera Adobe Pass Entitlement Services för en programmerare på Xbox 360 och XboxOne ClientCloud {#enabling-primetime-entitlement-services-for-a-programer-on-xbox-360-and-xboxone-clientless}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.




1. Programmeraren skapar en Zendesk-biljett för att aktivera klientlösa lösningar för Xbox 360/One for Adobe Pass Authentication genom att tillhandahålla följande information:

   1. Plattform: t.ex. Xbox 360, Xbox One

   1. Begärande-ID: t.ex. netgeo, CNN osv.

1. Adobe skapar X509-certifikat och konfigurerar den privata nyckeln och lösenordet i slutet.

1. Adobe tillhandahåller det offentliga certifikatet (av X509-certifikat) till Programer på biljetten eller via e-post.

1. Programmeraren måste sedan installera det offentliga certifikatet på GDNP-portalen för den app som är registrerad på Microsoft.

1. Programmeraren begär sedan JWT (Java Web Token) eller STS Token för XboxOne respektive 360 från tjänsten Microsoft Xbox Live, som krypteras med det offentliga X509-certifikatet som tillhandahålls i steg 3.

1. Detta är de tokens som innehåller det unika enhets-ID:t för Xbox-enheter. Inkludera token (JWT eller STS) i auktoriseringshuvudet med en x-parameter enligt nedan:

   1. För Xbox 360 måste XSTS-token vara Base64-kodad innan du skickar till Adobe Pass betal-TV-autentisering.
   1. För Xbox One är JWT redan korrekt kodad, så ingen extra kodning bör ske.

1. Alla API-anrop från Xbox-enheten ska innehålla auktoriseringshuvudet med den ovannämnda token i x-parametern.



>[!NOTE]
>
>Speciellt på Xbox finns några unika krav som rör digital signering. Enhets-ID:t för XBox-konsolen ingår i XSTS-token.  För Xbox 360 är detta en krypterad SAML-försäkran; för Xbox One är detta en krypterad JWT. XBox-konsolappen skickar hela XSTS-token till Adobe Pass betal-TV-autentisering. Adobe Pass betal-TV-autentisering dekrypterar token med dess offentliga nyckel, tolkar token och extraherar deviceId från den.

>[!NOTE]
>
>På grund av den stora längden på XSTS-token har XBox-konsolen en teknisk begränsning: den kan inte skicka token som HTTP-GET-parameter till Adobe Pass API:er för betal-TV-autentisering. För att hantera detta tillåter Adobe Pass betal-TV-autentisering att XSTS-token skickas som en del av HTTP-huvudet &quot;Authorization&quot; när API:erna anropas. XSTS-token måste krypteras med den offentliga nyckeln från X.509-certifikatet som utfärdas till programmeraren från Adobe Pass betal-TV-autentisering. Adobe Pass betal-TV-autentisering lagrar den associerade privata nyckeln och använder den för att dekryptera XSTS-token och extrahera deviceId från den.
