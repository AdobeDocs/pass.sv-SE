---
title: Exchange a Platform SSO token for an Adobe token
description: Exchange a Platform SSO token for an Adobe token
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 2%

---

# Exchange a Platform SSO token for an Adobe token {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Tillåter att en plattforms-SSO-profil&quot;utbyts&quot; för en Adobe-token.

| Slutpunkt | Anropat  </br>Av | Indata   </br>Parametrar | HTTP  </br>Metod | Svar | HTTP  </br>Svar |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn | Strömmande app</br></br>eller</br></br>Programmerartjänst | 1. beställare (obligatoriskt)</br>    </br>2.  deviceId (obligatoriskt)</br>    </br>3.  mvpd (obligatoriskt)</br>    </br>4.  deviceType (obligatoriskt)</br>    </br>5.  SAMLResponse (obligatoriskt)</br>    </br>6.  deviceUser (utgått)</br>    </br>7.  appId (utgått) | POST | Svaret blir 2004 No Content, vilket anger att token har skapats och är klar att användas för redigeringsflödena. | 204 - Inget innehåll   </br>400 - Ogiltig begäran |


| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| mvpd | Det MVPD-ID som den här åtgärden är giltig för. |
| deviceType | Den Apple-plattform som vi försöker få en profilbegäran för.  Antingen **iOS** eller **tvOS**. |
| SAMLResponse | Den faktiska profilen som returneras av enkel inloggning för plattformen. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. |
