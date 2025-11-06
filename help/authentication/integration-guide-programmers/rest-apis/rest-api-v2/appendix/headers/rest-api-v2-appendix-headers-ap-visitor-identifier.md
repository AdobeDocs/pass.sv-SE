---
title: Header - AP-Visitor-Identifier
description: REST API V2 - rubrik - AP-Visitor-ID
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# Header - AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Huvudet <b>AP-Visitor-Identifier</b> för begäran innehåller det `ECID` som krävs för att klientprogrammet ska kunna identifiera en besökare på ett unikt sätt i olika Adobe Experience Cloud-lösningar.

Mer information om hur du använder ECID vid Adobe Pass-autentisering finns i dokumentationen för [Använda Experience Cloud ID i Adobe Pass-autentisering](../../../../features-premium/analytics/exp-cloud-id-authn.md).

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>: &lt;visitor_identifier&gt;</td>
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

## Exempel {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
