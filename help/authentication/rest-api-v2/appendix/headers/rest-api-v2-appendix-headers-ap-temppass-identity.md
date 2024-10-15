---
title: Header - AP-TempPass-Identity
description: REST API V2 - Header - AP-TempPass-Identity
exl-id: a6238a58-a3f1-495d-a9d1-82475f5ffc60
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 0%

---

# Header - AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

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

X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
