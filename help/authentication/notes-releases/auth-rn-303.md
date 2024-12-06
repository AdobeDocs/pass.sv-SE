---
title: Versionsinformation om Adobe Pass Authentication 3.0.3
description: Versionsinformation om Adobe Pass Authentication 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.0.3 {#pt-authn-303-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-303}

* [Byggnummer](#build-number-303)
* [Versionsöversikt](#release-overview-303)

### Byggnummer {#build-number-2651}

Adobe Pass-autentisering: adobe-pass-**3.0.3**
Releasedatum: **2024-10-31**

### Nya funktioner {#new-features-303}

#### REST API v2 {#rest-apis}

##### Code

* REST API V2-förbättringar (sedan Adobe Pass 3.0 huvudversion levererade [REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* `notBefore` och `notAfter` fält på `/sessions/{code}` har lagts till för att returnera information om verifieringskodens giltighet.
* Förbättrad plattformsidentifiering.

##### Dokumentation

* Om du vill börja med det nya REST API v2 läser du dokumentet [REST API v2 Overview](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) .

##### verktyg

* Om du vill testa det nya REST API v2 kan du gå till den nya Adobe Pass-autentiseringssidan från webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass).
