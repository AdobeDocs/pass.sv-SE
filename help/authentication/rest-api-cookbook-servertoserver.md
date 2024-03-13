---
title: REST API Cookbook (Server-to-Server)
description: Återställ API-cookbook-servern till servern.
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '1847'
ht-degree: 0%

---

# REST API Cookbook (Server-to-Server) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.


## Ökning {#overview}

Syftet med detta kokboksdokument är att beskriva bästa praxis för implementering av Adobe Pass-autentisering med Server-till-Server-arkitekturer.  Den innehåller grundläggande krav, stegvis implementering av flöde och allmänna överväganden för produktionsmiljöer och drift.

### Begränsningsmekanism

Adobe Pass Authentication REST API styrs av en [Begränsningsmekanism](/help/authentication/throttling-mechanism.md).


## Komponenter {#components}

I en fungerande Server-till-Server-lösning ingår följande komponenter:


| Typ | Komponent | Beskrivning |
| --- | --- | --- |
| Strömmande enhet | Strömmande app | Programmeringsprogrammet som finns på användarens direktuppspelningsenhet och spelar upp autentiserad video. |
| | \[Valfritt\] AuthN-modul | Om direktuppspelningsenheten har en användaragent (t.ex. en webbläsare), är AuthN-modulen ansvarig för att autentisera användaren på MVPD IdP. |
| \[Valfritt\] AuthN-enhet | AuthN-app | Om direktuppspelningsenheten inte har någon användaragent (t.ex. webbläsare) är AuthN-programmet ett webbprogram för programmerare som nås från en separat användares enhet via en webbläsare. |
| Programmeringsinfrastruktur | Programmerartjänst | En tjänst som länkar direktuppspelningsenheten till Adobe Pass-tjänsten för att få autentiserings- och auktoriseringsbeslut. |
| Adobe infrastruktur | Adobe Pass Service | En tjänst som integreras med MVPD IdP- och AuthZ-tjänsten och som ger autentiserings- och auktoriseringsbeslut. |
| MVPD-infrastruktur | MVPD IdP | En MVPD-slutpunkt som tillhandahåller autentiseringsbaserad autentisering för att validera användarens identitet. |
| | MVPD AuthZ-tjänst | En MVPD-slutpunkt som ger auktoriseringsbeslut baserat på användarens prenumerationer, föräldrakontroll osv. |


Ytterligare termer som används i flödet definieras i
[Ordlista](/help/authentication/glossary.md).

## Flöden {#flows}

### Dynamic Client Registration (DCR)


Adobe Pass använder DCR för att säkra klientkommunikationen mellan ett programmeringsprogram eller en server och Adobe Pass tjänster. DCR-flödet är separat, beroende och nödvändigt och finns i [Dynamisk klientregistrering](/help/authentication/dynamic-client-registration.md).


### Autentisering (authN)

Autentiseringsflödet används för att låta en användare identifiera sig för sitt MVPD för att avgöra om användaren har ett giltigt konto.

1. Användaren startar appen Streaming Device och försöker logga in eller visa skyddat innehåll.
2. Strömningsenhetens app begär att programmeringstjänsten ska avgöra om enheten redan är autentiserad.
3. Programmerartjänsten registrerar programmet med DCR.
4. Programmerartjänsten kontrollerar direktuppspelningsenhetens autentiseringsstatus genom att anropa Adobe Pass-tjänsten **checkauthn** API.
5. I det fall **checkauthn** anrop returnerar statusen som användarenheten är autentiserad och sedan kan programmet fortsätta till auktoriseringsflödet.
6. I det fall **checkauthn** anropet returnerar statusen att användarenheten INTE är autentiserad, och programmet bör vänta på att en användarbegäran ska logga in.
7. När användaren begär direkt inloggning (t.ex. väljer inloggningsknapp) eller indirekt inloggning (t.ex. väljer skyddat innehåll när det inte redan är autentiserat), skickar strömningsenhetens app en begäran till programmeringstjänsten om att initiera användarautentisering. Programmerartjänsten begär och får en unik registreringskod (regcode) genom att ringa Adobe Pass-tjänsten **regcode** API.
8. Programmeringstjänsten hämtar även listan över aktuella MVPD och attribut genom att ringa Adobe Pass-tjänsten **config** API. Obs! Detta API kan också anropas tidigare i flödet och cachelagras.
9. Programmerartjänsten returnerar regcode till Streaming Device-appen och den bearbetade MVPD-listan som begärdes i steg \#7. Obs! Det bearbetade formatet för MVPD-listan anges av Programmer och kan filtreras så att specifika MVPD-filer tillåts eller blockeras (dvs. allow- eller block-lists).
10. Om enheten skiljer sig från AuthN-enheten (d.v.s.&quot;andra skärmen&quot;), antingen efter val eller behov (d.v.s. direktuppspelningsenheten saknar stöd för en användaragent), ska direktuppspelningsenheten visa regcode och en URI som användaren kan använda för att komma åt AuthN-programmet. Användaren skriver URI:n i användaragenten på AuthN-enheten för att starta AuthN-programmet och skriver sedan in koden i det programmet. Om direktuppspelningsenheten är samma som AuthN-enheten kan regcode skickas programmatiskt till AuthN-modulen.
11. AuthN-modulen initierar användarautentiseringen med MVPD genom att visa en MVPD-väljare. När användaren har valt MVPD anropas AuthN-modulen **autentisera** med regcode, som dirigerar om användaragenten till MVPD IdP. När användaren autentiserar med MVPD omdirigeras användaragenten tillbaka via Adobe Pass-tjänsten, där den lyckade autentiseringen registreras med regcode och sedan omdirigeras tillbaka till AuthN-modulen.
12. Om direktuppspelningsenheten inte är samma som AuthN-enheten ska AuthN-enheten visa ett meddelande om lyckad autentisering för användaren och steg för att fortsätta (t.ex. &quot;Success\! Du kan nu gå tillbaka till spelkonsolen och fortsätta med \[..\]&quot;). Om direktuppspelningsenheten är densamma som AuthN-enheten kan direktuppspelningsenheten programmässigt upptäcka att autentiseringen har slutförts.



I följande diagram visas autentiseringsflödet:

![](assets/authn-flow.png)

### Behörighet (authZ)

Auktoriseringsflödet används för att avgöra om en användare har behörighet till begärt innehåll.

1. Varje gång användaren försöker visa skyddat innehåll i appen för direktuppspelningsenheten anropar appen för direktuppspelningsenheten programmeringstjänsten som identifierar innehållet och begär tillstånd och information som behövs för att starta strömmen.
1. Programmerartjänsten anropar Adobe Pass **auktorisera** API skickar resurs-ID tillsammans med andra obligatoriska parametrar. Adobe-tjänsten anropar MVPD AuthZ-tjänsten med resurs-ID:t och tar emot ett auktoriseringsbeslut som sedan skickas tillbaka till programmeringstjänsten. Detta auktoriseringsbeslut cachas av Adobe Pass-tjänsten under en konfigurerbar period. Vid efterföljande **auktorisera** anrop från Programmer Service till Adobe Pass Service returnerar det cachelagrade värdet så länge det är giltigt.
1. Om tillstånd beviljas bör Programmer Service ringa Adobe Pass **/tokens/media** API, som returnerar en signerad medietoken. Programmerartjänsten bör validera medietoken med hjälp av medietokentverifieringsbiblioteket (JAR). Om det är giltigt bör programmeringstjänsten returnera behörighet och den som behövs för att starta strömmen (t.ex. strömmens URL) som begärdes i steg \#1.
1. Om auktorisering nekas **auktorisera** anrop returnerar felkod och beskrivning till programmeringstjänsten. Programmerartjänsten ska returnera felkoden och beskrivningen (eller ett meddelande som ändrats av programmeraren) till begäran i steg \#1.

I följande diagram visas auktoriseringsflödet:

![](assets/authz-flow.png)

### Utloggning

Med utloggningsflödet kan en användare ta bort den identitet som är kopplad till programmet.

1. När användaren begär utloggning (d.v.s. tar bort det aktuella MVPD-kontot som är kopplat till programmet från enheten), anropar direktuppspelningsenhetens app programmeringstjänsten som talar om att enheten ska loggas ut.
1. Programmerartjänsten bör ringa Adobe Pass **utloggning** API.

I följande diagram visas utloggningsflödet:

![](assets/logout-flow.png)

### \[Valfritt\] Förhandsauktorisering (alias Preflight)

Förhandsauktorisering kan användas för att snabbt utifrån en uppsättning resurser avgöra vilka som en användare har åtkomst till.  Resultatet av det här anropet används vanligtvis för att anpassa användargränssnittet för en enskild användare.

1. När användaren har autentiserats kan styrenheten anropa programmeringstjänsten för att begära det innehåll som användaren har rätt att strömma till.

1. Programmerartjänsten bör ringa Adobe Pass **förauktorisera** API med en lista över resurs-ID:n, som är en enkel sträng som vanligtvis representerar en kanal som en användare kan vara berättigad till för direktuppspelning. *Obs! För närvarande är* ***förauktorisera*** *anropet är konfigurerat för att begränsa listan till fem (5) resurs-ID:n. När mer än fem resurser behövs kan du* ***förauktorisera*** *anrop kan göras, eller så kan anropet konfigureras att acceptera mer än fem resurser med ett avtal från sidopanelen. Implementerare bör tänka på kostnaden för en* ***förauktorisera*** *anropa både MVPD-resurser och svarstiden till programmeraren och strukturera deras användning av samtalet effektivt.*

1. The **förauktorisera** anrop kommer att svara programmerartjänsten med ett JSON-objekt som innehåller värdet TRUE eller FALSE för varje resurs-ID i begäran som anger om användaren är berättigad till den associerade kanalen eller inte. *Obs! Om en MVPD inte ger ett svar för ett visst resurs-ID (t.ex. på grund av nätverksfel eller timeout), används standardvärdet FALSE.*

1. Programmerartjänsten bör använda **förauktorisera** anropa svar för att skapa ett programmeringsdefinierat anpassat svar på direktuppspelningsenheten, vanligtvis för att anpassa presentationen till användaren utifrån deras rättigheter.

I följande diagram visas förauktoriseringsflödet:

![](assets/preauthz-flow.png)


### \[Valfritt\]-metadata

Metadata kan användas för att hämta användarinformation som delas av MVPD.
Exempel på detta kan vara användar-ID, postnummer osv.

1. När användaren har autentiserats kan Programmer Service ringa Adobe Pass **användaremetadata** API för att begära information om den autentiserade användaren.

1. Svaret innehåller alla metadata som är tillgängliga för den angivna användaren. De specifika fälten konfigureras separat för varje programmerare/MVPD-integrering.

I följande diagram visas förauktoriseringsflödet:



![](assets/user-metadata-api-preauthz.png)



## Miljö och funktionskrav{#environments}



En programmerare bör skapa minst två miljöer: en för produktion och en eller flera för mellanlagring.


### Produktion

Produktionsmiljön bör vara mycket tillgänglig och skalas på lämpligt sätt för stora eller oväntade toppar (t.ex. live-sporter, nyheter).



Adobe Pass-tjänsten körs på flera datacenter som är geografiskt spridda i hela USA.  För att få bästa svarstid (dvs. lägsta svarstid) från Adobe Pass-tjänsten bör Programmeraren även skapa en liknande geografiskt spridd infrastruktur.


Programmerartjänsten bör begränsa DNS-cachen till högst 30 sekunder om Adobe behöver dirigera om trafiken. Detta kan inträffa om ett datacenter blir otillgängligt.


Programmeraren ska tillhandahålla produktionsmiljöns offentliga IP-intervall. Dessa kommer att ingå i en tillåten lista över IP-adresser i Adobe Pass-infrastruktur för åtkomst och hanteras av Adobe användningspolicyer för rättvisa API.

### Mellanlagring

Mellanlagringsmiljön kan vara minimal, men bör innehålla alla systemkomponenter och affärslogik. Den bör fungera på liknande sätt som produktionen och göra det möjligt att testa releaser utanför produktionen. I idealfallet kan mellanlagringsmiljön anslutas till Adobe Pass testmiljöer som kan användas av Programmer och av Adobe vid behov så att vi kan hjälpa till med testning och felsökning.

### Funktionskrav

Programmerartjänsten måste skicka korrekt enhetsidentifieringsinformation för den enhet som de kör flödena för. Programmerartjänsten måste dessutom skicka IP-adressen för den enhet som de kör flödena för (i ett x-vidarebefordrat-for-huvud) tillsammans med anslutningskällporten (i enhetsinformationsfältet):

    **x-Forwarded-for : \&lt;client _ip=&quot;&quot;>**
    
    där \&lt;client _ip=&quot;&quot;> är klientens offentliga IP-adress
    
    
    
    Rubriken måste läggas till på **regcode**- och **authorized**-anrop
    
    Exempel:
    
    POST /reggie/v1/{req\_id}/regcode HTTP/1.1
    
    x-Forwarded-for:203.45.101.20
    
    
    
    GET /api/v1/authorized HTTP/1.1
    
    x-Forwarded-for:203.45.101.20



Programmerartjänsten ska skicka data och formatering som krävs av enskilda programmeringsskyltar eller integrerade program (t.ex. enhets-IP, källport, enhetsinformation, MRSS, valfria data som ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.


Programmeringstjänsten måste respektera åtkomstkontrollistan authN och authZ vid cachelagring och ogiltigförklara authN- eller authZ-sessionerna när de meddelas.

Programmeraren måste behålla certifikat som delas med Adobe.

<!--
## Related Information {#related}

* [REST API Reference](/help/authentication/rest-api-reference.md)
* [Glossary of Terms](/help/authentication/adobe-pass-glossary.md)
-->
