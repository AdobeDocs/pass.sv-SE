---
title: Ta bort registreringspost
description: Ta bort registreringspost
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---

# (Äldre) Ta bort registreringspost {#delete-registration-record}

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


## Beskrivning {#delete-record}

Tar bort reg.kodposten och släpper reg.koden för återanvändning.

| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Till exempel:</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. Begärande-ID </br>    (Bankomponent)</br>2.  Registreringskod </br>    (Bankomponent) | DELETE | Ingen | 204 |

{style="table-layout:auto"}

</br>

| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| registreringskod | Registreringskodvärdet som skulle visas på strömningsenheten (anges i autentiseringsflödet). |

{style="table-layout:auto"}

</br>

### [Tillbaka till REST API-referens](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
