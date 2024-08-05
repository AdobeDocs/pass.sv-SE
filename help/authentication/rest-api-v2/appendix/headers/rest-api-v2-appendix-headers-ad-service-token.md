---
title: Header - AD-Service-Token
description: REST API V2 - huvud - AD-Service-Token
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---


# Header - AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

<b>AD-Service-Token</b>-begärandehuvudet innehåller den unika användaridentifieraren som `JWS` hämtats från en identitetstjänst som körs utanför Adobe Pass autentiseringssystem.

Det här huvudet är utformat för att användas i enkla inloggningsflöden (SSO) som utnyttjar Service Token-metoden.

Mer information om SSO-aktiverade flöden (Single Sign-on) som använder Service Token-metoden finns i dokumentationen för [enkel inloggning med tjänsttokenflöden](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

## Syntax {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b>: &lt;unique_user_identifier&gt;</td>
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

<b>unique_user_identifier</b>

JSON-webbsignaturen (`JWS`) som är en signerad JSON-webbtoken (`JWT`) som innehåller unik användaridentifierarinformation.

`JWT` har följande attribut:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Attribut</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>is</td>
      <td>Den unika identifierare som är associerad med den entitet som erbjuder programmet en extern identitetstjänst för enkel inloggning (SSO).</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>Den unika identifieraren för användaren som returneras av den externa identitetstjänsten.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>Publiken, som borde vara "Adobe".</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>Den utfärdad vid tidsstämpel för den aktuella JWT-filen.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>Tidsstämpeln för förfallodatum för aktuell JWT.</td>
   </tr>
</table>

`JWT` måste signeras med algoritmen `SHA256withRSA`.

`JWT` måste signeras med en privat nyckel, en del av ett par privata RSA-nycklar - offentlig nyckel som hanteras av den externa identitetstjänsten.

Den offentliga nyckeln för det paret måste skickas till Adobe Pass-autentiseringen för att `JWT`-token som signerats med den tidigare privata nyckeln ska kunna identifieras.

## Exempel {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
