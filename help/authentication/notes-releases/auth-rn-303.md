---
title: Versionsinformation om Adobe Pass Authentication 3.0.3
description: Versionsinformation om Adobe Pass Authentication 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.0.3 {#authn-303-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-303}

* [Byggnummer](#build-number-303)
* [Versionsöversikt](#release-overview-303)

### Byggnummer {#build-number-303}

Adobe Pass-autentisering: adobe-pass-**3.0.3**

Releasedatum: **10/29/2024 - 10/31/2024**

### Versionsöversikt {#release-overview-303}

#### REST API v2

##### Code

* REST API V2-förbättringar (sedan Adobe Pass 3.0 huvudversion levererade [REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* `notBefore` och `notAfter` fält på `/sessions/{code}` har lagts till för att returnera information om verifieringskodens giltighet.
* Förbättrad plattformsidentifiering.

##### Dokumentation

* Om du vill börja med det nya REST API v2 läser du dokumentet [REST API v2 Overview](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

##### verktyg

* Om du vill testa det nya REST API v2 kan du gå till den nya Adobe Pass-autentiseringssidan från webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass).
