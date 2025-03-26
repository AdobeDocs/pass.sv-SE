---
title: Profiler
description: Profiler
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Profiler {#profiles}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Profiler skapas med Adobe Pass-autentiseringen [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) när en användare autentiserar med sin Pay TV-leverantör (MVPD).

Profiltypen varierar beroende på vilken autentiseringsmetod som används:

* **Normal**

  Skapas med grundläggande autentisering.

* **Apple SSO**

  Skapas via enkel inloggning (SSO) med Apple Video Subscriber Account Framework.

* **Plattforms-SSO**

  Skapas via enkel inloggning (SSO) med en plattformsidentitet.

* **Tjänsttoken-SSO**

  Skapas via enkel inloggning (SSO) med en tjänsttoken.

Profilerna lagrar nyckeldata som gör att klientprogrammen kan:

* Fastställ användarens autentiseringsstatus.
* Identifiera den autentiseringsmetod som används.
* Identifiera identitetsleverantören.
* Åtkomst till [användarmetadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

Profiler lagras säkert i Adobe Pass Authentication-serverdelen och är länkade till den begärande program-, enhets- och tjänstleverantörens identifierare. De är fortfarande giltiga under en begränsad tidsperiod, enligt definitionen i [TTL (Authentication Time-to-Live)](#authentication-ttl-management).

## TTL-hantering (Time-to-Live) för autentisering {#authentication-ttl-management}

TTL (Authentication Time-to-Live) anger hur länge en användare ska vara autentiserad innan han/hon behöver autentisera igen. Denna tidsram är begränsad och måste avtalas med MVPD representanter. TTL-värdena kan variera beroende på:

* Plattformskategori (t.ex. dator, mobil, tv-anslutna enheter)
* Specifik plattform (t.ex. iOS, Android, tvOS, Roku, FireTV)

TTL-autentiseringen (authN) kan visas och ändras via Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) av en av dina företagsadministratörer eller av en Adobe Pass-autentiseringsrepresentant som agerar för din räkning.

Mer information finns i dokumentationen för [TVE Dashboard Integrations User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) .

## REST API V2 {#rest-api-v2}

Profilerna kan hämtas med följande API:er:

* [Hämta profiler](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Hämta profil för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Hämta profil för specifik kod](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Mer information om profilernas struktur finns i avsnitten **Svar** och **Exempel** i ovanstående API:er.

Mer information om hur och när ovanstående API:er ska integreras finns i följande dokument:

* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [Vanliga frågor om autentiseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
