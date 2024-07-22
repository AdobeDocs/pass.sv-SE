---
title: Versionsinformation om Adobe Pass Authentication 2.69
description: Versionsinformation om Adobe Pass Authentication 2.69
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.69 {#pt-authn-269-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-269}

* [Byggnummer](#build-number-269)
* [Nya funktioner](#new-features-269)

### Byggnummer {#build-number-269}

Adobe Pass-autentisering: adobe-pass-**2.69**
Releasedatum: **02/27/2024 - 02/29/2024**

### Nya funktioner {#new-features-269}

#### Diverse {#misc}

* Patched security vulnerabilities.
* Förbättringar av Återställ säkerhetslager för tillfälligt pass med DCR (Dynamic Client Registration).
   * Du hittar mer information här: [Återställ tillfälligt pass](reset-temp-pass.md)
* Förbättringar av rapporter om plattformsidentifiering.

#### REST API:er {#rest-apis}

* Fortsatt utveckling till nya REST API:er.
   * En kommande dedikerad release kommer att innehålla nya slutpunkter och flöden, som kommer att tillkännages i ett separat meddelande.
   * Dokumentationen för dessa nya API:er uppdateras.

#### TVE Dashboard {#tve-dashboard}

* Fortsatt utveckling till nya TVE Dashboard.
   * En kommande dedikerad version kommer att presentera den nya TV-instrumentpanelen, som kommer att tillkännages i ett separat meddelande.
   * Uppdatering av dokumentation för användning av den nya TVE-instrumentpanelen pågår.

#### JavaScript SDK 4.7.0 {#js-sdk}

* Borttagen borttagen version 2.0.1 av Access Enabler JavaScript SDK på grund av säkerhetsproblem.
   * Följ länken om du vill ha mer information: [Versionsinformation om Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
