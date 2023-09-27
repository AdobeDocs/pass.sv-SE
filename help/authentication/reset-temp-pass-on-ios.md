---
title: Återställ tillfälligt pass i iOS
description: Återställ tillfälligt pass i iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Återställ tillfälligt pass i iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

IOS Demo App innehåller en dedikerad skärm för återställning av TTL för tillfälligt pass. Följande information krävs för återställningsåtgärden:

- **Environment:** Anger serverslutpunkten Adobe Pay-TV-pass som tar emot återställningen av tillfälligt Pass-nätverksanropet. Möjliga värden: **Föregående** (*mgmt-prequal.auth-staging.adobe.com*), **Frigör** (*mgmt.auth.adobe.com*) eller **Egen** (reserverat för intern testning i Adobe).
- **OAuth2 Bearer-token:** OAuth2-token krävs för att godkänna programmeraren för autentisering med Adobe Pay-TV. En sådan token kan hämtas från den dedikerade Pay-TV-autentiseringen OAuth2-slutpunkten (t.ex. *curl -u &quot;\&lt;consumer _key=&quot;&quot;>:\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **Begärande-ID:** Unikt ID för den aktuella programmeraren. Det här värdet läses från huvudskärmen i demoappen (fältet för begärande).
- **ID för tillfälligt pass:** Unikt ID för det temporära pass-MVPD.
- **Enhets-ID:** hash-kodade enhets-ID som beräknas av demoappen.
- **Allmän nyckel:** Vissa Temp Pass MVPD-program (dvs. nästa utökningsbara Temp Pass-funktion) har stöd för en generisk nyckel för återställning av Temp Pass (tillsammans med enhets-ID).

Alla ovanstående parametrar (utom *Allmän nyckel*) är obligatoriska. Här är ett exempel på parametrar och det associerade nätverksanrop som kommer att utföras av demoappen (exemplet är i form av ett *curl *kommando):

- **Environment:** Frigör (*mgmt.auth.adobe.com*)
- **OAuth2 Bearer-token:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **Program-ID:** REF
- **ID för tillfälligt pass:** TempPassREF
- **Enhets-ID:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa6399 91
- **Allmän nyckel:** null (inget värde har angetts)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

en DELETE HTTP-begäran kommer att göras till **/reset** slutpunkt, skicka *OAuth2 Bearer-token* i auktoriseringsrubriken och *Enhets-ID*, *Begärande-ID* och *Tillfälligt pass-ID (MVPD ID)* som parametrar.

Om Programmeraren anger ett värde för *Allmän nyckel*, kommer ytterligare ett HTTP-anrop att utföras (den här gången till **/reset/generic** slutpunkt), skicka *Allmän nyckel* innanför *key* request-parameter.

Du kan till exempel ange *Allmän nyckel* till en hash-kodad e-postadress (för Temp Pass MVPD-program som stöder den här typen av funktioner) ger följande HTTP-anrop (e-postmeddelandet är `user@domain.com` dess SHA-256-hash är `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Det är viktigt att understryka att återställning av Temp Pass i Demo App kanske inte har samma effekt för en programmeringsapp på samma enhet. Detta beror på att enhets-ID (enligt beräkning i demoappen och AccessEnabler) kanske inte är samma för alla program på enheten:

- iOS 6 och lägre: enhets-ID beräknas med MAC-adressen (som är unik för alla program), så om du återställer det tillfälliga passet i demoappen återställs det i alla andra programmeringsappar på enheten.

- iOS 7 och senare: enhets-ID beräknas baserat på IDFV-värdet (ID för leverantör), som är unikt för alla program som har samma paket-ID-prefix (dvs. alla komponenter utom den sista). Eftersom demoappen och en programmerarapp har olika paket-ID:n kommer återställning av Temp Pass i demoappen inte att ha någon effekt på en programmerarapp.

Det senaste användningsexemplet (iOS 7 och senare) är det vanligaste så vi ska se hur programmerare kan återställa Temp Pass för sina appar i den här situationen. Det finns flera alternativ:

1. Skicka koden från demoappen till programmerarappen. The *TempPassResetViewController* och *DeviceIdDemoApp* klasser innehåller kärnlogiken för återställning av Temp Pass, och de kan enkelt ändras och inkluderas i Programmer-appen.

1. Kör HTTP-begäran för återställning av tillfälligt pass med *kurva*. Parametern device\_Id kan erhållas genom att beräkna IDFV för programmerarappen och lägga en SHA-256-hash över den (exempelkod i *DeviceIdDemoApp* klass).

1. Utför helt enkelt återställningen från demoappen genom att ange den streckade IDFV:en för programmerarens app som *Allmän nyckel*. Detta leder till två nätverksanrop: en för att återställa det tillfälliga passet för demoappen (som inte är relevant för programmeraren) och en för att återställa det tillfälliga passet för programmerarappen.

Alla ovanstående alternativ liknar varandra, det är upp till programmeraren att välja ett beroende på hur lätt implementeringen är.
