---
title: Header - AP-Partner-Framework-Status
description: REST API V2 - Header - AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# Header - AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Rubriken <b>AP-Partner-Framework-Status</b> innehåller statusinformation som hämtats från ett partnerramverk för att uppnå enkel inloggning (SSO).

## Syntax {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;partner_framework_status_information></b>

Värdet `Base64-encoded` för JSON-elementet som innehåller följande attribut:

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Det här är ett obligatoriskt attribut.
         <br/><br/>
         Statusinformation för användarbehörigheter som returneras av partnerramverket och bearbetas av programmet.
         <br/><br/>
         Detta är ett JSON-element med följande attribut:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  Det här är ett obligatoriskt attribut.
                  <br/><br/>
                  Detta är en uppräkning med följande möjliga värden:
                  <br/>
                  <ul>
                     <li><b>beviljad</b><br/>Användaren tillät programmet att komma åt prenumerationsinformation.</li>
                     <li><b>nekades</b><br/>Användaren nekade programmet åtkomst till prenumerationsinformation.</li>
                     <li><b>Väntande</b><br/>Användaren har ännu inte valt att ge programmet åtkomst till prenumerationsinformation.</li>
                     <li><b>notDetermined</b><br/>Programmet har inte åtkomst till prenumerationsinformation.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>fel</td>
               <td>
                  Detta är ett valfritt attribut.
                  <br/><br/>
                  Detta kan användas för att skicka fel i partnerramverket om ett fel utlöses när användaren tillfrågas om statusinformation om användarbehörigheter.
                  <br/><br/>
                  Detta är ett JSON-element med följande attribut:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>kod</td>
                        <td>En sträng som unikt identifierar felet enligt partnerramverket.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>En sträng som innehåller beskrivningen av felet enligt partnerramverket.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         Det här är ett obligatoriskt attribut.
         <br/><br/>
         Providerinloggningsstatusinformationen som returneras av partnerramverket och bearbetas av programmet.
         <br/><br/>
         Detta är ett JSON-element med följande attribut:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  Det här är ett obligatoriskt attribut.
                  <br/><br/>
                  Detta är det mappingId som identifierar den MVPD som används under autentiseringsflödet på partnerramverksnivå.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Det här är ett obligatoriskt attribut.
                  <br/><br/>
                  Detta är förfallodatumet för den autentiserade användarprofilen om användaren har loggat med en MVPD som stöds på partnerramverksnivå.
               </td>
            </tr>
            <tr>
               <td>fel</td>
               <td>
                  Detta är ett valfritt attribut.
                  <br/><br/>
                  Detta kan användas för att skicka fel i partnerramverket om ett fel utlöses vid sökning efter information om leverantörens inloggningsstatus.
                  <br/><br/>
                  Detta är ett JSON-element med följande attribut:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>kod</td>
                        <td>En sträng som unikt identifierar felet enligt partnerramverket.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>En sträng som innehåller beskrivningen av felet enligt partnerramverket.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## Exempel {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
