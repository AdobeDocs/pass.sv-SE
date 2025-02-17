---
title: Versionsinformation om Adobe Pass Authentication 2.64.1
description: Versionsinformation om Adobe Pass Authentication 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.64.1 {#authn-264-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-2641}

* [Byggnummer](#build-number-2641)
* [Versionsöversikt](#release-overview-2641)

### Byggnummer {#build-number-2641}

Adobe Pass-autentisering: adobe-pass-**2.64.1**

Releasedatum: **01/31/2023 - 02/02/2023**

### Versionsöversikt {#release-overview-2641}

I den här versionen har följande lagts till:

* Möjlighet att blockera oombedda autentiseringssvar från MVPD-program där parametern &quot;in_response_to&quot; saknas i SAML-försäkran.
* En förbättrad validering av parametern redirect_url för att uppfylla säkerhetskraven.
* En förbättrad loggning av enhetsinformation för autentiseringsbegäranden på andra skärmen med hjälp av information från den första enheten.
