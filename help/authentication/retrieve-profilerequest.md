---
title: Hämta plattformens SSO-profilbegäran
description: Hämta plattformens SSO-profilbegäran
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 0%

---

# Hämta plattformens SSO-profilbegäran {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!NOTE]
>
> REST API-implementeringen begränsas av [Begränsningsmekanismen](/help/authentication/throttling-mechanism.md)

## REST API-slutpunkter {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Den här resursen skapar profilförfrågningar för begärande-ID och MVPD-tuppel.


| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. beställare (path param)</br>2. mvpd (path param)</br>3. deviceType (obligatoriskt) | GET | Svarets Content-Type blir application/octet-stream eftersom den faktiska nyttolasten är ogenomskinlig för klientprogrammet.</br></br>Svaret bör vidarebefordras av programmet till SSO-motorn för plattformen</br></br>för att erhålla en enkel inloggning för profiler. | 200 - lyckades   </br>400 - Ogiltig begäran |


| Indataparameter | Beskrivning |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| mvpd | Det MVPD-ID som den här åtgärden är giltig för. |
| deviceType | Den Apple-plattform som vi försöker få en profilbegäran för.  Antingen **iOS** eller **tvOS**. |
