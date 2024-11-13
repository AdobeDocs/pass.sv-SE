---
title: Header - Adobe-Subject-Token
description: REST API V2 - rubrik - Adobe-Subject-token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: e5ef8c0cba636ac4d2bda1abe0e121d0ecc1b795
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---

# Header - Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Begärandehuvudet <b>Adobe-Subject-Token</b> innehåller den unika plattformsidentifieraren som `JWS` eller `JWE` som hämtats från en identitetstjänst eller ett bibliotek som körs utanför Adobe Pass autentiseringssystem.

Det här huvudet är utformat för att användas i enkla inloggningsflöden (SSO) som utnyttjar plattformsidentitetsmetoden.

Mer information om enkel inloggning (SSO)-aktiverade flöden som utnyttjar metoden för plattformsidentitet finns i dokumentationen för [enkel inloggning med plattformsidentitetsflöden](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

* [Amazon SSO Cookbook (REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Exempel {#examples}

Se exemplen som beskrivs för följande plattformar:

* [Amazon SSO Cookbook (REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
