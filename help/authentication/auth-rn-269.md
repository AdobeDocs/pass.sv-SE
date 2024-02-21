---
title: Versionsinformation om Adobe Pass Authentication 2.69
description: Versionsinformation om Adobe Pass Authentication 2.69
source-git-commit: 4417c5c50873c73c615f9845cadd4a42c26f3068
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

Adobe Pass-autentisering: adobe-pass-**2,69**
Utgivningsdatum: **02/27/2024 - 02/29/2024**

### Nya funktioner {#new-features-269}

#### Diverse {#misc}

* Patched security vulnerabilities.
* Förbättringar av Återställ säkerhetslager för tillfälligt pass med DCR (Dynamic Client Registration).
   * Mer information finns här: [Återställ tillfälligt pass](reset-temp-pass.md)
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
   * Följ länken för mer information: [Versionsinformation om Adobe Pass-autentisering JavaScript 4.7.0](authn-rn-javascript-470.md)
