---
title: Undvik att använda '&'reg_code i /authenticate Request
description: Undvik att använda '&'reg_code i /authenticate Request
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# (Äldre) Undvik att använda &#39;&amp;&#39;reg_code i /authenticate Request {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

</br>



## Problem

Webbläsaren IE 9 tolkar &#39;\&amp;reg&#39; som ett specialkommando och konverterar det till ®.

## Förklaring

Om `/authenticate`-begäran består av följande..


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...den tolkas av IE:s webbläsare som nedan och skickas till Adobe i detta format:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


Begärande\_id tolkas som univision®\_code=EKAFMFI, eftersom det inte finns något &#39;&amp;&#39;, och Adobe kommer inte att hitta någon `regCode`-param att associera token med.  Det finns en risk för att AuthN-token inte skapas alls. I så fall kan `/checkauthn` anrop inte hitta några token.



## Lösning

Ett av följande alternativ bör lösa problemet:

1. Undvik att använda parametern `&reg_code` mellan de andra frågesträngsparametrarna.  Flytta den i stället till den första frågesträngsparametern i begärande-URL:en så här:


       &lt;FQDN>authenticate?reg_code =EKAFMFI&amp;requested_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   På så sätt kommer parametern `&reg` inte att tolkas felaktigt.

1. Normalisera `&reg_code` som om `&amp;reg_code` används.

1. Adobe kan införa en ny funktion för att skicka tillbaka en felkod till den andra skärmen som svar på ett autentiseringsanrop, om det inte gick att skapa AuthN-token.
