---
title: Begränsningsmekanism
description: Ta reda på vilken begränsningsmekanism som används vid Adobe Pass-autentisering. Utforska en översikt över den här funktionen på den här sidan.
source-git-commit: 4f81f39427d87e4274c27d8f1b4bd1eb366d9abb
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---


# Begränsningsmekanism {#throttling-mechanism}

Alla lösenordsautentiseringskunder måste kunna komma åt API:t för lösenordsautentisering för var och en av sina användare, enligt instruktionerna och deras affärsmodell.

Godkänn autentisering introducerar en begränsningsmekanism som säkerställer en rättvis fördelning av resurser mellan våra kunders användare.

Den här mekanismen är viktig av några skäl:

- En av de gyllene reglerna i multi-tenant-arkitekturen är att en användares beteende inte ska påverka någon annan.
- Hastighetsbegränsning är viktigt för API:er eftersom det är enkelt att göra misstag vid integrering med ett API. Eftersom API:er programmatiskt är det relativt enkelt att av misstag skicka fler förfrågningar än vad som är tänkt.

## Mekanismöversikt {#mechanism-overview}

### Kundimplementeringar

Pass Authentication ger riktlinjer och SDK för interaktion med API:t, men styr inte hur kunden använder det. Vissa implementeringar kan ha en grundläggande implementering och kan översvämma tjänsten med onödiga API-begäranden, oavsett om det sker oavsiktligt eller inte, vilket kan innebära att andra användare drabbas av flaskhalsar eller kapacitetsproblem.

Tjänsten i sig bör kunna hantera alla rimliga möjligheter. Men oavsett hur skalbar eller prestanda en tjänst är finns det alltid begränsningar. Tjänsten måste därför ha konfigurerade gränser för antalet godkända samtal inom ett visst tidsintervall.

### Introduktion till begränsning

Lösenautentisering baseras på användaridentifiering och en algoritm för begränsning av tokenhastighet med fördefinierade värden för att styra varje användares enhetsåtkomst till vårt API.

### Enhetsidentifieringsmekanism

Den föreslagna begränsningsmekanismen använder de identifierade enheterna separat, med hjälp av rubriken&quot;X-Forwarded-For&quot;. Gränserna tillämpas på samma sätt för varje enhet.

### Nödvändiga uppdateringar

Implementeringar från server till server måste vidarebefordra klientens IP-adresser med hjälp av rubrikmekanismen X-Forwarded-For.

Mer information om hur du skickar rubriken X-Forwarded-For finns [här](rest-api-cookbook-servertoserver.md).

### Faktiska gränser och slutpunkter

För närvarande tillåter standardgränsen högst 1 begäran per sekund, med en inledande sekvens på 3 begäranden (engångsavdrag vid den första interaktionen med den identifierade klienten, som bör tillåta initieringen att slutföras). Detta bör inte påverka alla vanliga affärstillfällen för alla våra kunder.

Begränsningsmekanismen aktiveras för följande slutpunkter:

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/utloggning
- /api/v1/authorized
- /api/v1/preauthorized
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDK-implementeringssätt

Eftersom klienter som använder den angivna SDK:n för Adobe Pass-autentisering inte uttryckligen interagerar med några slutpunkter, kommer det här avsnittet att visa de kända funktionerna, hur de beter sig när de stöter på ett strypningssvar och vilka åtgärder som bör vidtas.

#### setRequestor

Vid uppnående av begränsningsgränsen med `setRequestor` SDK-funktionen från SDK returnerar en CFG429-felkod via `errorHandler` återanrop.

#### getAuthorization

Vid uppnående av begränsningsgränsen med `getAuthorization` SDK-funktionen från SDK returnerar en Z100-felkod via `errorHandler` återanrop.

#### checkPreauthorizedResources

Vid uppnående av begränsningsgränsen med `checkPreauthorizedResources` SDK-funktionen från SDK returnerar en P100-felkod via `errorHandler` återanrop.

#### getMetadata

Vid uppnående av begränsningsgränsen med `getMetadata` SDK-funktionen returnerar ett tomt svar via `setMetadataStatus` återanrop.

För varje specifik implementeringsinformation, se den specifika SDK-dokumentationen.

- [API-referens för JavaScript SDK](javascript-sdk-api-reference.md)
- [Android SDK API-referens](android-sdk-api-reference.md)
- [API-referens för iOS/tvOS](iostvos-sdk-api-reference.md)

### Ändringar av API-svar och svar

När vi identifierar att gränsen överskrids, markerar vi den här begäran med en specifik svarsstatus (HTTP 429 för många begäranden), vilket anger att du har förbrukat alla token som tilldelats användarenheten (IP-adressen) för tidsintervallet.

Begränsningen upphör att gälla efter en sekund av de första 429 svaren. Varje program som får ett 429-svar måste vänta i minst en sekund innan en ny begäran genereras.

Alla kundprogram bör hantera svaret&quot;429 För många förfrågningar&quot; på lämpligt sätt.

Här är ett exempel på ett 429-svarsmeddelande:

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## Påverkan och nödvändiga ändringar

### Skicka sidhuvudet X-Forwarded-For

Kunder som använder en anpassad implementering (inklusive server-till-server-en) för att interagera med API:t för lösenordsautentisering bör se till att de kan hämta användarens IP-adress och vidarebefordra den korrekt med hjälp av rubriken X-Forwarded-For vidare till API:t för autentisering.

Se [här](rest-api-cookbook-servertoserver.md) för mer information.

### Reagera på ny svarskod

Kunder som använder en anpassad implementering (inklusive server-till-server-sådana) för att interagera med API:t för autentisering av pass bör se till att alla anrop som görs efter att ha tagit emot 429 för många begäranden omfattar en vänteperiod på minst en sekund. Denna vänteperiod ger möjlighet att ändra denna mekanism och få ett giltigt svar från verksamheten.

## Scenarioexempel för begränsning

| Tid sedan första begäran | Mottaget svar | Förklaring |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| Andra 0 | Samtalet tar emot statuskod för lyckat resultat | 1 samtal förbrukas från gränsen |
| Andra 0.3 | Samtalet tar emot statuskod för lyckat resultat | 1 samtal förbrukade från gränsen och 1 samtal markerade som burna |
| Andra 0,6 | Samtalet tar emot statuskod för lyckat resultat | 1 samtal förbrukade från gränsen och 2 samtal markerade som burna |
| Andra 0,9 | Samtalet tar emot statuskod för lyckat resultat | 1 samtal förbrukade från gränsen och 3 samtal markerade som burna |
| Andra 1.2 | Samtalet tar emot statuskod för lyckat resultat | 2 samtal förbrukade från gränsen och 3 samtal markerade som burna |
| Andra 1.4 | Samtalet tar emot 429 statuskod | 2 samtal tas bort från gränsen och 3 samtal markeras som burst och 1 samtal tar emot&quot;429 För många begäranden&quot; |
| Andra 1.6 | Samtalet tar emot 429 statuskod | 2 samtal tas bort från gränsen och 3 samtal markeras som burst och 2 samtal tar emot&quot;429 För många begäranden&quot; |
| Andra 1.8 | Samtalet tar emot 429 statuskod | 2 samtal tas bort från gränsen och 3 samtal markeras som burst och 3 samtal tar emot&quot;429 För många begäranden&quot; |
| Andra 2.1 | Samtalet tar emot statuskod för lyckat resultat | 3 samtal tas bort från gränsen och 3 samtal markeras som burst och 3 samtal tar emot&quot;429 För många begäranden&quot; |
