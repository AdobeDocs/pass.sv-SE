---
title: Header - X-Roku-Reserved-Roku-Connect-Token
description: REST API V2 - Header - X-Roku-Reserved-Roku-Connect-Token
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Header - X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Huvudet <b>X-Roku-Reserved-Roku-Connect-Token</b> innehåller den unika plattforms-ID:t `JWS` eller `JWE` som hämtats från en identitetstjänst eller ett bibliotek som körs utanför Adobe Pass autentiseringssystem.

Det här huvudet är utformat för att användas i enkla inloggningsflöden (SSO) som utnyttjar plattformsidentitetsmetoden.

Mer information om enkel inloggning (SSO)-aktiverade flöden som utnyttjar metoden för plattformsidentitet finns i dokumentationen för [enkel inloggning med plattformsidentitetsflöden](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

<b>unique_platform_identifier</b>

JSON-webbsignaturen (`JWS`) eller JSON-webbkrypteringen (`JWE`) som är en signerad eller krypterad JSON-webbtoken (`JWT`) som innehåller unik plattformsidentifierarinformation.

Detta är tillgängligt för följande plattformar:

* [Roku SSO Cookbook (REST API V2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
