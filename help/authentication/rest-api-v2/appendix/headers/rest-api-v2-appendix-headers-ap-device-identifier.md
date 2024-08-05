---
title: Header - AP-Device-Identifier
description: REST API V2 - huvud - AP-enhets-ID
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---


# Header - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Huvudet för begäran <b>AP-Device-Identifier</b> innehåller identifieraren för direktuppspelningsenheten som den skapades av klientprogrammet.

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;typ&gt; &lt;identifierare&gt;</td>
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

<b>&lt;typ></b>

Enhetsidentifierartypen.

Det finns bara en typ som stöds enligt nedan.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Typ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>fingeravtryck</td>
      <td>Enhetsidentifieraren består av en ogenomskinlig identifierare som skapas av klientprogrammet.</td>
   </tr>
</table>


<b>&lt;identifierare></b>

Värdet `Base64-encoded` för enhets-ID.

## Exempel {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
