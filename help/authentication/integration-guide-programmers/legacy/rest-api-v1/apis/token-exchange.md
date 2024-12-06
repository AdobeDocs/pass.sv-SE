---
title: Exchange a Platform SSO token for an Adobe token
description: Exchange a Platform SSO token for an Adobe token
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Exchange a Platform SSO token for an Adobe token {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Tillåter att en plattforms-SSO-profil&quot;utbyts&quot; för en Adobe-token.

| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/token/authn | Direktuppspelande app</br></br>eller</br></br>Programmeringtjänst | 1. beställare (obligatoriskt)</br>    </br>2.  deviceId (obligatoriskt)</br>    </br>3.  mvpd (obligatoriskt)</br>    </br>4.  deviceType (obligatoriskt)</br>    </br>5.  SAMLResponse (obligatoriskt)</br>    </br>6.  deviceUser (utgått)</br>    </br>7.  appId (utgått) | POST | Svaret blir 2004 No Content, vilket anger att token har skapats och är klar att användas för redigeringsflödena. | 204 - Inget innehåll   </br>400 - Ogiltig begäran |


| Indataparameter | Beskrivning |
| --- | --- |
| begärande | Programmerarens requestId som den här åtgärden är giltig för. |
| deviceId | Byte för enhets-ID. |
| mvpd | Det MVPD-ID som den här åtgärden är giltig för. |
| deviceType | Den Apple-plattform som vi försöker få en profilbegäran för.  Antingen **iOS** eller **tvOS**. |
| SAMLResponse | Den faktiska profilen som returneras av enkel inloggning för plattformen. |
| _deviceUser_ | Enhetens användaridentifierare. |
| _appId_ | Program-ID/namn. |
