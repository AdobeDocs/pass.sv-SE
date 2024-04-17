---
title: Versionsinformation om Adobe Pass Authentication 2.70
description: Versionsinformation om Adobe Pass Authentication 2.70
source-git-commit: 30e2507c0622b882744cf8291525c388575df32f
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.70 {#pt-authn-270-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-270}

* [Byggnummer](#build-number-270)
* [Nya funktioner](#new-features-270)

### Byggnummer {#build-number-270}

Adobe Pass-autentisering: adobe-pass-**2,70**
Utgivningsdatum: **04/23/2024 - 04/25/2024**

### Nya funktioner {#new-features-270}

#### Diverse {#misc}

* Patched security vulnerabilities.
* Förbättringar av API-tjänsten för degradering.
   * Använd DCR som säkerhetsmekanism för API för nedgradering.
   * Mer information finns här: [Översikt över API-nedgradering](degradation-api-overview.md)

#### REST API:er {#rest-apis}

* Fortsatt utveckling till nya REST API:er.
   * En kommande dedikerad release kommer att innehålla nya slutpunkter och flöden, som kommer att tillkännages i ett separat meddelande.
   * Dokumentationen för dessa nya API:er uppdateras.
