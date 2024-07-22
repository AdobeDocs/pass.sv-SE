---
title: Fördelar med att använda parametern deviceType utan klient i Adobe Pass-autentiseringsmått
description: Fördelar med att använda parametern deviceType utan klient i Adobe Pass-autentiseringsmått
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Fördelar med att använda parametern deviceType utan klient i Adobe Pass-autentiseringsmått {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Kontext

Parametern `deviceType` från klientlöst API används, i förekommande fall, i Adobe Pass-autentiseringsmått som exponeras via [Entitlement Service Monitoring](/help/authentication/entitlement-service-monitoring-overview.md), även om den är valfri.

Med tanke på att anslutningen mellan parametern `deviceType` och dess **-fördelar** på Adobe Pass-autentiseringsmått inte angavs från början, är omfattningen av den här TechNote att lägga till mer information om dem.

## Förklaring

Parametern `deviceType` fanns i det klientlösa API:t sedan den första versionen, men dess konsekvenser för Adobe Pass-autentiseringsmått lades till i en senare version.



>[!IMPORTANT]
>
>Om parametern `deviceType` är rätt inställd har den följande **förmån** i övervakningen av berättigandetjänsten: den erbjuder mätvärden som är [nedbrutna per enhetstyp](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) när klientlös används, så att olika typer av analyser kan utföras för t.ex. Roku, AppleTV, Xbox osv.


Mer information om API:t för övervakning av berättigandetjänster finns i [fördjupningsträdet](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) som illustrerar [dimensionerna](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) (resurserna) som finns i ESM 2.0.

>[!NOTE]
>
>Innehållet i den här tekniska anteckningen lades också till i [klientlöst API](#clientless_device_type).




## Implementering

För att du ska kunna dra full nytta av Adobe Pass autentiseringsmått finns det två typer av [klientlösa API:er](#web_srvs_summary) som för närvarande används och som måste ha rätt `deviceType`-uppsättning:

1. API:er som har `regcode` som en obligatorisk parameter och använder parametern `deviceType` som ställdes in när `regcode` skapades, med följande API-anrop:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. API:er som har `deviceType` som en valfri parameter:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorized](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorized](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logOut](#init_logout)

Vi rekommenderar att du använder parametern `deviceType` och skickar rätt klientlös enhetstyp för alla API:er.
