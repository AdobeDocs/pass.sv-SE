---
title: Ange MVPD-lista
description: Ange MVPD-lista
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 1%

---

# Ange MVPD-lista {#provide-mvpd-list}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!NOTE]
>
> REST API-implementering begränsas av [Begränsningsmekanism](/help/authentication/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Returnerar en lista över konfigurerade MVPD-filer för den som gjorde begäran.

| Slutpunkt | Anropat  </br>Av | Indata   </br>Parametrar | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/config/{requestorId}</br></br>Till exempel:</br></br>&lt;sp_fqdn>/api/v1/config/sampleRequestorId | Adobe Pass-autentisering | 1. Begärande</br>    (Bankomponent)</br>_2.  deviceType (utgått)_ | GET | XML eller JSON som innehåller en lista över PDF-filer. | 200 |

{style="table-layout:auto"}


| Indataparameter | Beskrivning |
| --------------- | ------------------------------------------------------------- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| *deviceType* | Enhetstyp. |

{style="table-layout:auto"}

### Exempelsvar {#sample-response}

Samma som det befintliga MVPD XML-svaret till /config-servern

Obs! Alla MVPD-filer som konfigurerats för att använda enkel inloggning för plattformar har följande extra egenskaper i motsvarande nod (JSON/XML):

* **enablePlatformServices (boolean):** flagga som anger om denna MVPD är integrerad via plattformens SSO
* **boardingStatus (sträng):** flagga som anger om MVPD har fullt stöd för Platform SSO (SUPPORTED) eller om MVPD bara visas i plattformsväljaren (PICKER)
* **displayInPlatformPicker (boolean):** om det här videofilmsprogrammet ska visas i plattformsväljaren
* **platformMappingId (sträng):** identifieraren för detta MVPD så som den är känd av plattformen
* **requiredMetadataFields (strängmatris):** de metadatafält som förväntas vara tillgängliga vid en lyckad inloggning
