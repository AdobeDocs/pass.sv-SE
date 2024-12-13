---
title: Amazon FireOS Technical Overview
description: Amazon FireOS Technical Overview
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '2167'
ht-degree: 0%

---

# (Äldre) Amazon FireOS Technical Overview {#amazon-fireos-technical-overview}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Introduktion {#intro}

Amazon FireOS AccessEnabler representeras av två komponenter: ett AccessEnabler-stub-bibliotek som används av programmet och ett Java Android-bibliotek på systemnivå som gör det möjligt för mobilappar att använda Adobe Pass Authentication for TV Everywhere&#39;s entitlement services. En Android-implementering för Amazon FireOS består av AccessEnabler-gränssnittet som definierar berättigande-API:t och ett EntitlementDelegate-protokoll som beskriver återanropen som biblioteket utlöser. Med hjälp av AccessEnabler Android-biblioteket på systemnivå kan du få tillgång till Amazon tjänster och aktivera enkel inloggning på plattformsnivå.

## Understanding Native Client Workflows {#native_client_workflows}

Inbyggda klientarbetsflöden är vanligtvis desamma som, eller mycket lika, för webbläsarbaserade Adobe Pass Authentication-klienter. Det finns dock några undantag, som beskrivs nedan.


### Arbetsflöde efter initiering {#post-init}

Alla tillståndsarbetsflöden som stöds av AccessEnabler förutsätter att du tidigare har anropat [`setRequestor()`](#setRequestor) för att upprätta din identitet. Du gör det här anropet för att bara ange ditt begärande-ID en gång, vanligtvis under programmets initierings-/installationsfas.

Med de inbyggda klienterna (t.ex. AmazonFireOS) kan du efter ditt första anrop till [`setRequestor()`](#setRequestor) välja hur du vill fortsätta:

- Du kan börja ringa berättigandesamtal direkt och låta dem stå i kö om det behövs.
- Du kan också få en bekräftelse på att [`setRequestor()`](#setRequestor) lyckades/misslyckades genom att implementera callback-funktionen setRequestorComplete().
- Eller gör båda.

Det är upp till dig om du ska vänta på att meddelandet om att [`setRequestor()`](#setRequestor) lyckades ska visas eller om du ska förlita dig på åtkomstaktiverarens anropskömekanism. Eftersom alla efterföljande autentiserings- och autentiseringsbegäranden behöver begärande-ID och den associerade konfigurationsinformationen blockerar metoden [`setRequestor()`](#setRequestor) alla autentiserings- och auktoriserings-API-anrop tills initieringen är slutförd.

### Allmänt inledande autentiseringsarbetsflöde {#generic}

Syftet med det här arbetsflödet är att logga in en användare med sin MVPD.  När inloggningen är klar utfärdar backend-servern en autentiseringstoken till användaren. Autentisering sker vanligtvis som en del av auktoriseringsprocessen, men följande beskriver hur autentisering kan fungera fristående och inkluderar inga auktoriseringssteg.

Observera att även om följande inbyggda klientarbetsflöde skiljer sig från det vanliga webbläsarbaserade autentiseringsarbetsflödet är steg 1-5 samma för både inbyggda klienter och webbläsarbaserade klienter:

1. Sidan eller spelaren initierar autentiseringsarbetsflödet med ett anrop till [getAuthentication()](#getAuthN) som söker efter en giltig cachelagrad autentiseringstoken. Den här metoden har en valfri `redirectURL`-parameter. Om du inte anger ett värde för `redirectURL` returneras användaren till den URL som autentiseringen initierades från när autentiseringen lyckades.
1. AccessEnabler avgör aktuell autentiseringsstatus. Om användaren är autentiserad anropar AccessEnabler din `setAuthenticationStatus()`-callback-funktion och skickar en autentiseringsstatus som anger att åtgärden lyckades (steg 7 nedan).
1. Om användaren inte är autentiserad fortsätter AccessEnabler autentiseringsflödet genom att fastställa om användarens senaste autentiseringsförsök lyckades med en viss MVPD. Om ett MVPD-ID cachelagras OCH flaggan `canAuthenticate` är true ELLER om en MVPD valdes med [`setSelectedProvider()`](#setSelectedProvider) visas ingen dialogruta för val av MVPD. Autentiseringsflödet fortsätter med det cachelagrade värdet för MVPD (det vill säga samma MVPD som användes vid den senaste autentiseringen). Ett nätverksanrop görs till backend-servern och användaren omdirigeras till inloggningssidan för MVPD (steg 6 nedan).
1. Om inget MVPD-ID cachelagras OCH ingen MVPD har valts med [`setSelectedProvider()`](#setSelectedProvider) ELLER om flaggan `canAuthenticate` har värdet false anropas [`displayProviderDialog()`](#displayProviderDialog)-återanropet. I det här återanropet dirigeras sidan eller spelaren till det användargränssnitt som visar en lista över PDF-filer som användaren kan välja mellan. En array med MVPD-objekt som innehåller den information som krävs för att du ska kunna skapa MVPD-väljaren. Varje MVPD-objekt beskriver en MVPD-enhet och innehåller information som t.ex. MVPD-ID (t.ex. XFINITY, AT\&amp;T) och den URL där MVPD-logotypen finns.
1. När en viss MVPD har valts måste sidan eller spelaren informera AccessEnabler om vad användaren väljer. För klienter som inte är Flashar informerar du AccessEnabler om användarvalet via ett anrop till metoden [`setSelectedProvider()`](#setSelectedProvider) när användaren har valt önskad MVPD. Flash-klienter skickar i stället en delad `MVPDEvent` av typen `mvpdSelection` och skickar den markerade providern.
1. Återanropet [`navigateToUrl()`](#navigagteToUrl) ignoreras för Amazon-program. Biblioteket Access Enabler ger åtkomst till en gemensam WebView-kontroll som autentiserar användare.
1. Via `WebView` kommer användaren till MVPD inloggningssida och anger sina inloggningsuppgifter. Observera att flera omdirigeringsåtgärder utförs under den här överföringen.
1. När WebView-kontrollen har slutfört autentiseringen stängs den och informerar AccessEnabler om att användaren har loggat in, så hämtar AccessEnabler den faktiska autentiseringstoken från backend-servern. AccessEnabler anropar återanropet [`setAuthenticationStatus()`](#setAuthNStatus) med statuskoden 1, vilket anger att åtgärden lyckades. Om det uppstår ett fel under körningen av dessa steg utlöses [`setAuthenticationStatus()`](#setAuthNStatus)-återanropet med statuskoden 0, tillsammans med motsvarande felkod, som anger att användaren inte är autentiserad.

### Utloggningsarbetsflöde {#logout}

För inbyggda klienter hanteras inloggningar på liknande sätt som autentiseringsprocessen som beskrivs ovan. I det här mönstret skapar AccessEnabler en `WebView`-kontroll och läser in kontrollen med URL:en för utloggningsslutpunkten på backend-servern. När utloggningsprocessen är slutförd rensas token.

Observera att utloggningsflödet skiljer sig från autentiseringsflödet på så sätt att användaren inte behöver interagera med `WebView` på något sätt. När utloggningen är klar anropar AccessEnabler återanropet `setAuthenticationStatus()` med statuskoden 0, vilket anger att användaren inte är autentiserad.

## Tokens {#tokens}

### Definitioner och användning {#definitions}

Adobe Pass Authentication-berättigandelösningen kretsar kring genereringen av specifika datadelar (tokens) som genereras av Adobe Pass Authentication när autentiserings- och auktoriseringsarbetsflödena har slutförts. Dessa token lagras lokalt på klientens Amazon FireOS-enhet.

Tokens har begränsad livslängd. När den upphör att gälla måste tokens utfärdas på nytt genom att autentiserings- och/eller auktoriseringsarbetsflödena återupptas.

Det finns tre typer av tokens som utfärdas under tillståndsarbetsflödena:

- **Autentiseringstoken** - Slutresultatet av användarautentiseringsarbetsflödet blir ett autentiserings-GUID som AccessEnabler kan använda för att skapa auktoriseringsfrågor för användarens räkning. Detta autentiserings-GUID har ett associerat TTL-värde (time-to-live) som kan skilja sig från användarens autentiseringssession. Adobe Pass Authentication genererar en autentiseringstoken genom att binda autentiserings-GUID till den enhet som initierar autentiseringsbegäranden.
- **Auktoriseringstoken** - Ger åtkomst till en specifik skyddad resurs som identifieras av en unik `resourceID`. Det består av ett auktoriseringsbidrag som utfärdats av den auktoriserande parten tillsammans med originalet `resourceID`. Den här informationen är bunden till den enhet som initierar begäran.
- **Kortlivad medietoken** - AccessEnabler beviljar åtkomst till värdprogrammet för en given resurs genom att returnera en kortlivad medietoken. Denna token genereras baserat på den auktoriseringstoken som tidigare förvärvats för just den aktuella resursen. Den här token är inte bunden till enheten och den associerade livstiden är betydligt kortare (standard: 5 minuter).

När autentiseringen och auktoriseringen är klar kommer Adobe Pass Authentication att utfärda autentiserings-, auktoriserings- och kortlivade medietoken. Dessa token bör cachelagras på användarens enhet och användas under hela den tid som de är kopplade till sin livstid.

### Riktlinjer för cachelagring {#caching}


#### Autentiseringstoken

- **AccessEnabler 1.10.1 för FireOS** är baserad på AccessEnabler för Android 1.9.1 - Denna SDK introducerar en ny metod för tokenlagring som aktiverar flera programmerings-MVPD-bucket och därmed flera autentiseringstoken.

#### Auktoriseringstoken

Vid ett givet tillfälle cachelagras bara EN auktoriseringstoken per resurs av AccessEnabler. Det kan finnas flera autentiseringstoken cachelagrade, men de är associerade med olika resurser. När en ny auktoriseringstoken utfärdas och en gammal redan finns för samma resurs, skriver den nya token över det befintliga cachelagrade värdet.

#### Medietoken

Den kortlivade medietoken får INTE cachelagras alls. Medietoken bör hämtas från servern varje gång ett auktoriserings-API anropas, eftersom det är begränsat till engångsanvändning.

### Persistence {#persistence}

Token måste vara beständig i flera på varandra följande körningar av samma program. Detta innebär att när autentiserings- och auktoriseringstoken har hämtats och användaren stänger programmet, är samma token tillgängliga för programmet när användaren öppnar programmet igen. Dessutom är det önskvärt att dessa variabler är beständiga i flera program. När en användare har använt ett program för att logga in med en viss identitetsleverantör (har hämtat autentiserings- och auktoriseringstoken) kan samma token användas via ett annat program, och användaren uppmanas inte längre att ange autentiseringsuppgifter när han eller hon loggar in via samma identitetsleverantör.

Den här typen av smidigt autentiserings-/auktoriseringsarbetsflöde gör Adobe Pass Authentication-lösningen till en äkta TV-Everywhere-implementering. Ur en rent teknisk synvinkel kan Android AccessEnabler-biblioteket kringgå problemen med datadelning mellan program genom att lagra tokendata i en databasfil som finns på en extern lagringsplats. Dessa delade resurser på systemnivå innehåller de viktigaste komponenterna som gör det möjligt att implementera de önskade permanenta token-användningsexemplen:

- Stöd för strukturerad lagring - Adobe Pass Authentication-tokenarkivet är inte bara en enkel linjär buffertliknande minnesstruktur. Den innehåller en ordlisteliknande lagringsmekanism som gör det möjligt att indexera data baserat på användarspecificerade nyckelvärden.
- Stöd för databeständighet med det underliggande filsystemet - innehållet i databasfilen bevaras som standard och data sparas på enhetens externa minne.

När en viss token har placerats i token-cachen kontrolleras dess giltighet vid olika tillfällen av AccessEnabler-biblioteket.  En giltig token definieras som:

- Token har inte gått ut
- Utfärdaren av token ingår i listan över tillåtna identitetsleverantörer

Tokenlagringen kan stödja flera programmerare-MVPD-kombinationer, beroende på en kapslad mappningsstruktur på flera nivåer som kan innehålla flera autentiseringstoken. Det nya lagringsutrymmet påverkar inte det offentliga API:t för AccessEnabler på något sätt och kräver inga ändringar från programmerarens sida. Här följer ett exempel som visar den här nyare funktionen:

1. Öppna App1 (utvecklad av Programmer1).
1. Autentisera med MVPD1 (som är integrerat med Programmer1).
1. Skjut upp/stäng det aktuella programmet och öppna App2 (utvecklad av Programmer2).
1. Låt oss anta att Programmer2 inte är integrerat med MVPD2. Därför kommer användaren INTE att autentiseras i App2.
1. Autentisera med MVPD2 (som är integrerat med Programmer2) i App2.
1. Växla tillbaka till App1. Användaren autentiseras fortfarande med Programmer1.

### Format {#format}

#### Autentiseringstoken {#authn_token}

I listan nedan visas formatet för autentiseringstoken:

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### Auktoriseringstoken {#authz_token}

I listan nedan visas auktoriseringstokens format:

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### Kort medietoken {#short_media_token}

I listan nedan visas formatet för den korta medietoken.  Denna token visas för programmerarens program.  Det skickas till Programmerarens program när en tillståndsprocess är slutförd:

```JSON
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### Enhetsbindning {#device_binding}

Observera taggen `simpleTokenFingerprint` i XML-listan ovan. Syftet med den här taggen är att lagra information om anpassad enhets-ID. AccessEnabler-biblioteket kan hämta sådan individualiseringsinformation och göra den tillgänglig för Adobe Pass Authentication Services under berättigandeanropen. Tjänsten kommer att använda den här informationen och bädda in den i de faktiska tokenerna, vilket effektivt binder tokenerna till en viss enhet. Slutmålet för detta är att göra tokens icke-överförbara över olika enheter.

Observera taggen simpleTokenFingerprint i XML-listan ovan. Syftet med den här taggen är att lagra information om anpassad enhets-ID. AccessEnabler-biblioteket kan hämta sådan individualiseringsinformation och göra den tillgänglig för Adobe Pass Authentication Services under berättigandeanropen. Tjänsten kommer att använda den här informationen och bädda in den i de faktiska tokenerna, vilket effektivt binder tokenerna till en viss enhet. Slutmålet för detta är att göra tokens icke-överförbara över olika enheter.

Eftersom detta är en uppenbart säkerhetsrelaterad funktion är denna information i sig&quot;känslig&quot; ur säkerhetssynpunkt. Därför måste denna information skyddas mot både manipulering och tjuvlyssning. Eavesdropping-problemet löses genom att autentiserings-/auktoriseringsbegäranden skickas via HTTPS-protokollet. Manipuleringsskyddet hanteras genom att enhetsidentifieringsinformationen signeras digitalt. AccessEnabler-biblioteket beräknar ett enhets-ID från information som enheten anger och skickar sedan enhets-ID:t &quot;i klartext&quot; till Adobe Pass autentiseringsservrar som en begärandeparameter.  Adobe Pass autentiseringsservrar signerar digitalt enhets-ID med Adobe privata nyckel och lägger till det i den autentiseringstoken som returneras till AccessEnabler. Detta innebär att enhets-ID är bundet till autentiseringstoken.  Under auktoriseringsflödet skickar AccessEnabler igen enhets-ID:t i rensningen tillsammans med autentiseringstoken.  Om valideringsprocessen misslyckas kommer det automatiskt att leda till att arbetsflödena för autentisering/auktorisering misslyckas.  Adobe Pass autentiseringsservrar använder den privata nyckeln för enhets-ID och jämför den med värdet i autentiseringstoken.  Om de inte matchar misslyckas tillståndsflödet.
