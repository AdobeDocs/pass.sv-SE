---
title: Ange MVPD-lista
description: Ange MVPD-lista
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
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

* **enablePlatformServices (boolesk):**-flagga som anger om detta MVPD är integrerat via plattformens SSO
* **boardingStatus (sträng):** flagga som anger om MVPD har fullständigt stöd för Platform SSO (SUPPORTED) eller om MVPD bara visas i plattformsväljaren (PICKER)
* **displayInPlatformPicker (booleskt):** om detta MVPD ska visas i plattformsväljaren
* **platformMappingId (sträng):** identifieraren för det här MVPD som är känd av plattformen
* **requiredMetadataFields (strängmatris):** användarmetadatafälten som förväntades vara tillgängliga vid en lyckad inloggning
