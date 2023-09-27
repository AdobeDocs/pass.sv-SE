---
title: Hämta plattformens SSO-profilbegäran
description: Hämta plattformens SSO-profilbegäran
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Hämta plattformens SSO-profilbegäran {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## REST API-slutpunkter {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produktion - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Mellanlagring - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Beskrivning {#description}

Den här resursen skapar profilförfrågningar för begärande-ID och MVPD-tuppel.


| Slutpunkt | Anropat  </br>Av | Indata   </br>Parametrar | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/{requestor}/profile-requests/{mvpd} | Strömmande app</br></br>eller</br></br>Programmerartjänst | 1. beställare (path param)</br>2. mvpd (path param)</br>3. deviceType (obligatoriskt) | GET | Svarets Content-Type blir application/octet-stream eftersom den faktiska nyttolasten är ogenomskinlig för klientprogrammet.</br></br>Svaret bör vidarebefordras av programmet till plattformen</br></br>SSO-motor för att erhålla en profil-SSO. | 200 - lyckades   </br>400 - Ogiltig begäran |


| Indataparameter | Beskrivning |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| mvpd | Det MVPD-ID som den här åtgärden är giltig för. |
| deviceType | Den Apple-plattform som vi försöker få en profilbegäran för.  Antingen **iOS** eller **tvOS**. |
