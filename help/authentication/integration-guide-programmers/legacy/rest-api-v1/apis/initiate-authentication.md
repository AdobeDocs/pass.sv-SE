---
title: Initiera autentisering
description: Initiera autentisering
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# Initiera autentisering {#initiate-authentication}

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

Initierar autentiseringsprocessen genom att informera om en MVPD-markeringshändelse. Skapar en post i Adobe Pass-autentiseringsdatabasen, som avstäms när ett godkänt svar tas emot från MVPD.



| Slutpunkt | Anropat </br>av | Indata   </br>Parametrar | HTTP </br>Metod | Svar | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authenticate | AuthN-modul | 1. request_id (obligatoriskt)</br>2.  mso_id (obligatoriskt)</br>3.  reg_code (obligatoriskt)</br>4.  domain_name (obligatoriskt)</br>5.  noflash=true - </br>    (Obligatoriskt, Restresterande parameter)</br>6.  no_iframe=true (obligatorisk, restparameter)</br>7.  extra parametrar (valfritt)</br>8.  redirect_url (obligatoriskt) | GET | Inloggningswebbappen omdirigeras till inloggningssidan för MVPD. | 302 för fullständiga omdirigeringsimplementeringar |

{style="table-layout:auto"}


| Indataparameter | Beskrivning |
| --- | --- |
| beställare_id | Den programmerarbegäran som den här åtgärden är giltig för. |
| mso_id | Det MVPD-ID som den här åtgärden gäller för. |
| reg_code | Registreringskoden som genereras av Reggie-tjänsten. |
| domain_name | Den ursprungliga domänen. |
| redirect_url | Omdirigerings-URL för inloggningswebbprogrammet efter att autentiseringen har slutförts. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Viktigt! Obligatoriska parametrar -** Oavsett implementering på klientsidan är alla ovanstående parametrar obligatoriska.
>
>
>Exempel:
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**Viktigt: Valfria parametrar**
>
>Anropet kan även innehålla valfria parametrar som möjliggör andra funktioner som:
>
> * generisk\_data - aktiverar användning av [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Anteckningar** {#notes}

* Värdet för parametern `domain_name` måste anges till ett av de domännamn som är registrerade med Adobe Pass-autentisering. Mer information finns i [Registrering och initiering](/help/authentication/kickstart/programmer-overview.md).

* [Undvik att använda &#39;&amp;&#39;reg\_code i /authenticate request (Tech Note)](/help/authentication/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* Parametern `redirect_url` måste vara den sista i ordningen

* Värdet för parametern `redirect_url` måste vara URL-kodat
