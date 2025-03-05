---
title: Header - Authorization
description: REST API V2 - Header - Authorization
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 0%

---


# Header - Authorization {#header-authorization}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Huvudet <b>Authorization</b> innehåller åtkomsttoken `Bearer` som krävs för att klientprogrammet ska få åtkomst till Adobe Pass-skyddade API:er.

Mer information om hur du får åtkomst till Adobe Pass-skyddade API:er finns i [Översikt över registrering av dynamiska klienter](../../../rest-api-dcr/dynamic-client-registration-overview.md) -dokumentationen.

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Auktorisering</b>: Bearer &lt;åtkomsttoken&gt;</td>
   </tr>
   <tr>
      <td>Huvudtyp</td>
      <td>Huvud för begäran</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Ja</td>
   </tr>
</table>

## Direktiv {#directives}

<b>&lt;åtkomsttoken></b>

Åtkomsttoken-värdet är ett ogenomskinligt värde med begränsad tid till livstid (t.ex. 24 timmar) som måste hämtas från Adobe Pass enligt beskrivningen i API-dokumentationen för [Hämta åtkomsttoken](../../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## Exempel {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
