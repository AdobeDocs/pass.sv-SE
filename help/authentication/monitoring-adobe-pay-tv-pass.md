---
title: Övervaka Adobe Pass-autentisering
description: Övervaka Adobe Pass-autentisering
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Övervaka Adobe Pass-autentisering {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

Kunder kan använda [Nagios](http://www.nagios.org) eller andra verktyg för att kontrollera om Adobe Pass-autentisering är aktiverad eller inte.

## Övervaka slutpunkter {#monitoring-endpoints}

### Slutpunkter som du kan övervaka {#endpoints-to-monitor}

* Konfigurationsslutpunkten för alla plattformar: `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]`- Den är tillgänglig via antingen HTTP eller HTTPS (beroende på vilket val som görs av innehållsleverantörens utvecklare). Om den här slutpunkten saknas betyder det att innehållet inte är tillgängligt på alla plattformar och för alla programmeringsgränssnitten. För klientlöst REST API har vi även följande slutpunkt: `https://api.auth.adobe.com/adobe-services/config your-config-ID]`.

* Följande slutpunkter ingår i Adobe Pass Authentication web SDK.  Om betal-TV-pass saknas betyder det att betal-TV-pass inte används för alla programmerare och alla webbegenskaper:

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### Slutpunkter som du inte bör övervaka {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  Du får alltid ett 503-fel eftersom den här slutpunkten behöver ett MVPD SAML-svar på den.

* Andra berättigandeslutpunkter - `adobe-services/1.0/authenticate/`, `adobe-services/1.0/deviceShortAuthorize`, `adobe-services/1.0/authorize`

Du kan inte övervaka de här slutpunkterna eftersom de behöver en nyttolast för ett relevant svar.
