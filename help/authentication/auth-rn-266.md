---
title: Versionsinformation om Adobe Pass Authentication 2.6
description: Versionsinformation om Adobe Pass Authentication 2.6
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.6 {#authn-266-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-266}

* [Byggnummer](#build-number-266)
* [Versionsöversikt](#release-overview-266)

### Byggnummer {#build-number-266}

Adobe Pass-autentisering: adobe-pass-**2.66.0.1**
Releasedatum: **07/11/2023 - 07/13/2023**

### Versionsöversikt {#release-overview-266}

I den här versionen har vi fortsatt interna uppdateringar för det nya REST API:t.

#### Felkorrigeringar {#release-overview-bugfixes-266}

* Åtgärdade utloggningsflödet för SAML-baserade MVPD-filer, där parametern RelayState saknades i utloggningsbegäran. Konfigurationsuppdateringar efter lanseringen görs för att återställa utloggningsflödet för berörda MVPD-filer.
* Lagt till möjligheten att uppdatera SSL-certifikat i vår konfiguration för SOAP åtkomstslutpunkter.
* Korrigerade ett hörnfall där felaktiga data loggades i fältet Programmer i vissa ESM-rapporter.
