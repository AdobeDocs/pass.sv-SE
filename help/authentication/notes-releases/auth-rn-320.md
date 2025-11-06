---
title: Versionsinformation om Adobe Pass Authentication 3.2.0
description: Versionsinformation om Adobe Pass Authentication 3.2.0
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 3.2.0 {#authn-320-rn}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-320}

* [Byggnummer](#build-number-320)
* [Versionsöversikt](#release-overview-320)

### Byggnummer {#build-number-320}

Adobe Pass-autentisering: adobe-pass-**3.2.0**

Releasedatum: **06/10/2025 - 06/12/2025**

### Versionsöversikt {#release-overview-320}

#### REST API v2

* En ny orsak, `missing_parameters_fallback`, har lagts till för fall där parametrar saknas i [sessionens API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)-svar.
* Ett nytt fält, &quot;device&quot;, har lagts till i [Sessions-API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) -svaret.

#### Nya funktioner

* Expandera API:t för nedgradering för att lägga till möjlighet att tillämpa nedbrytningsregler för flera PDF-filer i ett enda anrop

#### Felkorrigeringar

* Ett problem som gjorde att autentiseringsflödet inte kunde slutföras i fall där bearbetningen av användarens metadata misslyckades har åtgärdats.
* Ett problem har korrigerats där auktoriseringsorsaken inte beräknades korrekt.
* Ett problem har korrigerats där upstreamUserID inte fanns i profilsvaret.
* Korrigerade ett problem som gjorde att autentiseringsbegäran inte inkluderade omfångsinformation för initieringsbegäranden för REST API V2.
