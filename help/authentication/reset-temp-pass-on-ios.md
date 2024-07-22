---
title: Återställ tillfälligt pass i iOS
description: Återställ tillfälligt pass i iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# Återställ tillfälligt pass i iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

IOS Demo App innehåller en dedikerad skärm för återställning av TTL för tillfälligt pass. Följande information krävs för återställningsåtgärden:

- **Miljö:** anger serverslutpunkten Adobe Pay-TV som tar emot återställningen av tillfälligt pass-nätverksanropet. Möjliga värden: **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Release** (*mgmt.auth.adobe.com*) eller **Custom** (reserverat för intern testning i Adobe).
- **OAuth2 Bearer-token:** OAuth2-token krävs för att auktorisera programmeraren för Adobe Pay-TV-autentisering. En sådan token kan hämtas från den dedikerade Pay-TV-autentiseringen OAuth2-slutpunkten (t.ex. *curl -u &quot;\&lt;Consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **Begärande-ID:** det unika ID:t för den aktuella programmeraren. Det här värdet läses från huvudskärmen i demoappen (fältet för begärande).
- **Temporärt ID:** är det unika ID:t för det temporära MVPD-dokumentet.
- **Enhets-ID:** hashas Device ID som beräknas av demoappen.
- **Allmän nyckel:** Vissa Temp Pass MVPD (d.v.s. nästa tänkta Temp Pass-funktion) stöder en generisk nyckel för återställning av Temp Pass (tillsammans med enhets-ID).

Alla ovanstående parametrar (förutom *Allmän nyckel*) är obligatoriska. Här är ett exempel på parametrar och det associerade nätverksanrop som kommer att utföras av demoappen (exemplet är i form av ett *curl *kommando):

- **Miljö:** Utgåva (*mgmt.auth.adobe.com*)
- **OAuth2 Bearer-token:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **Program-ID:** REF
- **Tillfälligt pass-ID:** TempPassREF
- **Enhets-ID:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa 639991
- **Allmän nyckel:** null (inget värde har angetts)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

en DELETE HTTP-begäran görs till slutpunkten **/reset** och skickar *OAuth2 Bearer Token* i auktoriseringshuvudet och *Device ID*, *Requestor ID* och *Temp Pass ID (MVPD ID)* som parametrar.

Om programmeraren anger ett värde för den *allmänna nyckeln* kommer ett annat HTTP-anrop att utföras (den här gången till **/reset/generic** -slutpunkten) och *Generic Key* skickas i *key* -begärandeparametern.

Om du till exempel anger *Allmän nyckel* som en hash för e-postadressen (till exempel
Temp-pass-MVPD-program som stöder den här typen av funktioner) ger
efter HTTP-anrop (e-postmeddelandet är `user@domain.com` dess SHA-256
hash är `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Det är viktigt att understryka att återställning av Temp Pass i Demo App kanske inte har samma effekt för en programmeringsapp på samma enhet. Detta beror på att enhets-ID (enligt beräkning i demoappen och AccessEnabler) kanske inte är samma för alla program på enheten:

- iOS 6 och lägre: enhets-ID beräknas med MAC-adressen (som är unik för alla program), så om du återställer det tillfälliga passet i demoappen återställs det i alla andra programmeringsappar på enheten.

- iOS 7 och senare: enhets-ID beräknas baserat på IDFV-värdet (ID för leverantör), som är unikt för alla program som har samma paket-ID-prefix (dvs. alla komponenter utom den sista). Eftersom demoappen och en programmerarapp har olika paket-ID:n kommer återställning av Temp Pass i demoappen inte att ha någon effekt på en programmerarapp.

Det senaste användningsexemplet (iOS 7 och senare) är det vanligaste så vi ska se hur programmerare kan återställa Temp Pass för sina appar i den här situationen. Det finns flera alternativ:

1. Skicka koden från demoappen till programmerarappen. Klasserna *TempPassResetViewController* och *DeviceIdDemoApp* innehåller kärnlogiken för återställning av Temp Pass, och de kan enkelt ändras och inkluderas i programmerarappen.

1. Kör HTTP-begäran om att återställa det tillfälliga lösenordet med *curl*. Parametern device\_Id kan erhållas genom att beräkna IDFV för programmeringsappen och använda en SHA-256-hash över den (exempelkod i klassen *DeviceIdDemoApp*).

1. Utför helt enkelt återställningen från demoappen genom att ange den streckade IDFV:en för programmerarens app som *generisk nyckel*. Detta leder till två nätverksanrop: en för att återställa det tillfälliga passet för demoappen (som inte är relevant för programmeraren) och en för att återställa det tillfälliga passet för programmerarappen.

Alla ovanstående alternativ liknar varandra, det är upp till programmeraren att välja ett beroende på hur lätt implementeringen är.
