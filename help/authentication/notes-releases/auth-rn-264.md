---
title: Versionsinformation om Adobe Pass Authentication 2.64
description: Versionsinformation om Adobe Pass Authentication 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.64 {#authn-264-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-264}

* [Byggnummer](#build-number-264)
* [Versionsöversikt](#release-overview-264)

### Byggnummer {#build-number-264}

Adobe Pass-autentisering: adobe-pass-**2.64**

Releasedatum: **11/08/2022 - 11/10/2022**

### Versionsöversikt {#release-overview-264}

* Infrastrukturuppdateringar, avsedda att minska svarstiderna på servern och förbättra systemets övergripande prestanda.
* Förbättringar av den nya plattformsidentifieringsmekanismen.
* Möjlighet att blockera oombedda autentiseringssvar från MVPD-program där parametern &quot;in_response_to&quot; saknas i SAML-försäkran.

#### Felkorrigeringar

* Korrigerad felaktig formatering av vissa äldre TempPass-token.
* Åtgärdade mindre problem med autentiseringsflödet på andra skärmen.
