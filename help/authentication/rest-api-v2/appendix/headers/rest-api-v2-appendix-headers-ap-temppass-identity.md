---
title: Header - AP-TempPass-Identity
description: REST API V2 - Header - AP-TempPass-Identity
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# Header - AP-TempPass-Identity {#header-ap-temppass-identity}

## Ökning {#overview}

Huvudet för begäran <b>AP-TempPass-Identity</b> innehåller den användaridentitetsinformation som används för att få fram erbjudandet TempPass.

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>: &lt;user_identity_information&gt;</td>
   </tr>
   <tr>
      <td>Huvudtyp</td>
      <td>Huvud för begäran</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Nej</td>
   </tr>
</table>

## Direktiv {#directives}

<b>&lt;user_identity_information></b>

Värdet `Base64-encoded` över användaridentitetsinformationen som är associerad med slutanvändaren som en tillfällig kampanjåtkomst måste beviljas till.

## Exempel {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
