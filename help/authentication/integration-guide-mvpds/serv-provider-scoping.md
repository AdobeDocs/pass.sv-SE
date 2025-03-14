---
title: Tjänstleverantörsomfång
description: Tjänstleverantörsomfång
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Tjänstleverantörsomfång {#service-provoider-scoping}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Standardimplementeringen av en Adobe Pass-autentiseringsintegrering med ett MVPD baseras på **OLCA-specifikationen**. I avsnittet Autentiseringskrav i OLCA-specifikationen (6.5, Ämnesidentifierare) anges att det är möjligt att ange omfattningen för tjänstleverantörens SP (Service Provider) för Ämnesidentifieraren. (Ämnesidentifieraren är det obefuffserade användar-ID som MVPD returnerar till SP:n.)  I en integrering med Adobe Pass Authentication krävs att MVPD-program aktiverar omfång för SP Authentication-begäranden.

Med Adobe Pass Authentication i rollen som SP för programmeraren är det nödvändigt att implementera en anpassning som möjliggör SP-omfång för autentiseringsbegäran.  Detta måste göras så att distributören kan identifiera det nätverksmärke som skickades in i SAML-försäkran till identitetsleverantören (IdP).  Omfång kan implementeras på ett av de två sätt som beskrivs i nästa avsnitt.

## Tjänstleverantörsomfång {#service-provider-scoping}

Adobe Pass Authentication stöder följande två sätt att aktivera SP-scoping för autentiseringsbegäranden:

* **SAML-utfärdarens metod.** I det här arbetssättet läggs ID för begärande till i SAML-utfärdarsträngen i SAML-autentiseringsbegäran.

* **Den anpassade omfångsegenskapen.** I det här arbetssättet inkluderas&quot;ID för begärande&quot; explicit som en anpassad&quot;omfångsegenskap&quot; i SAML-autentiseringsbegäran.

>[!NOTE]
>
>&quot;ID för begärande&quot; är hur Adobe Pass Authentication hänvisar till programmerarens nätverksmärke (till exempel: &quot;CNN&quot; är ett av varumärkena i Turner-nätverket).

### SAML-utfärdarmetod {#saml-issuer-approach}

I det här tillvägagångssättet används SAML `<Issuer>`-elementet i SAML-autentiseringsbegäran, vilket visas i det här kodutdraget:

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Egenskap för anpassat omfång {#custom-scoping-property-approach}

Den här metoden använder en anpassad egenskap med namnet&quot;Scoping&quot;, vilket visas i det här fragmentet i en SAML-autentiseringsbegäran:

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
