---
title: Tillhandahåll MVPD List
description: Tillhandahåll MVPD List
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 1%

---

# (Äldre) Ange MVPD-lista {#provide-mvpd-list}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

>[!NOTE]
>
> REST API-implementeringen begränsas av [Begränsningsmekanismen](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Returnerar en lista över konfigurerade MVPD-filer för den som gjorde begäran.

| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Till exempel:</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass-autentisering | 1. Begärande </br>    (Bankomponent)</br>_2.  deviceType (utgått)_ | GET | XML eller JSON som innehåller en lista över PDF-filer. | 200 |

{style="table-layout:auto"}


| Indataparameter | Beskrivning |
| --------------- | ------------------------------------------------------------- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| *deviceType* | Enhetstyp. |

{style="table-layout:auto"}

### Exempelsvar {#sample-response}

Samma som det befintliga MVPD XML-svaret till /config-servern

Obs! Alla MVPD-filer som konfigurerats för att använda enkel inloggning för plattformar har följande extra egenskaper i motsvarande nod (JSON/XML):

* **enablePlatformServices (boolesk):**-flagga som anger om denna MVPD är integrerad via enkel inloggning (Platform SSO)
* **boardingStatus (sträng):** flagga som anger om MVPD har fullständigt stöd för Platform SSO (SUPPORTED) eller om MVPD bara visas i plattformsväljaren (PICKER)
* **displayInPlatformPicker (booleskt):** om denna MVPD ska visas i plattformsväljaren
* **platformMappingId (sträng):** identifieraren för det här MVPD som är känd av plattformen
* **requiredMetadataFields (strängmatris):** användarmetadatafälten som förväntades vara tillgängliga vid en lyckad inloggning
