---
title: Android SDK - översikt
description: Android SDK - översikt
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '2732'
ht-degree: 0%

---

# (Äldre) Android SDK - översikt {#android-sdk-overview}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

Android AccessEnabler är ett Java Android-bibliotek som gör att mobilappar kan använda Adobe Pass Authentication for TV Everywhere&#39;s tillståndsservice. En Android-implementering består av AccessEnabler-gränssnittet som definierar berättigande-API:t och ett EntitlementDelegate-protokoll som beskriver återanropen som biblioteket utlöser. Gränssnittet och protokollet kallas under ett gemensamt namn: AccessEnabler Android-biblioteket.

## Android-krav {#reqs}

Aktuella tekniska krav för Android-plattformen och Adobe Pass-autentisering finns i [Plattform/Enhet/Verktygskrav](#android) eller i versionsinformationen som medföljer nedladdningen av Android SDK.

## Understanding Native Client Workflows {#native_client_workflows}

Inbyggda klientarbetsflöden är vanligtvis desamma som, eller mycket lika, för webbläsarbaserade Adobe Pass Authentication-klienter. Det finns dock några undantag, som beskrivs nedan.

- [Arbetsflöde efter initiering](#post-init)
- [Allmänt inledande autentiseringsarbetsflöde](#generic)
- [Utloggningsarbetsflöde](#logout)

### Arbetsflöde efter initiering {#post-init}

Alla tillståndsarbetsflöden som stöds av AccessEnabler förutsätter att du tidigare har anropat [`setRequestor()`](#setRequestor) för att upprätta din identitet. Du gör det här anropet för att bara ange ditt begärande-ID en gång, vanligtvis under programmets initierings-/installationsfas.


Med de inbyggda klienterna (t.ex. Android) kan du efter ditt första anrop till [`setRequestor()`](#setRequestor) välja hur du vill fortsätta:

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
1. Om com.android.chrome är tillgängligt för Android-program läses autentiserings-URL:en in på anpassade Chrome-flikar.
1. Via Chrome anpassade flikar kommer användaren till MVPD inloggningssida och anger sina inloggningsuppgifter. Observera att flera omdirigeringsåtgärder utförs under den här överföringen.
1. När Chrome anpassade flikar identifierar att en URL matchar schemat (adobepass://) och djuplänken från resursen &quot;redirect\_uri&quot; (d.v.s. adobepass://com.adobepass ) hämtar AccessEnabler den faktiska autentiseringstoken från serverdelsservrarna. Observera att de slutliga omdirigerings-URL:erna är ogiltiga och de är inte avsedda för Chrome anpassade flikar för att läsa in dem. De får endast tolkas av SDK som en signal om att autentiseringsflödet har slutförts.
1. AccessEnabler informerar programmet om att autentiseringsflödet är slutfört. AccessEnabler anropar återanropet [`setAuthenticationStatus()`](#setAuthNStatus) med statuskoden 1, vilket anger att åtgärden lyckades. Om det uppstår ett fel under körningen av dessa steg utlöses [`setAuthenticationStatus()`](#setAuthNStatus)-återanropet med statuskoden 0, tillsammans med motsvarande felkod, vilket anger att autentiseringen misslyckades.

### Utloggningsarbetsflöde {#logout}

För inbyggda klienter hanteras inloggningar på liknande sätt som autentiseringsprocessen som beskrivs ovan. I det här mönstret öppnar AccessEnabler anpassade Chrome-flikar och läser in URL:en för utloggningsslutpunkten på backend-servern.



**Obs!** Loggar ut från en programmerare/MVPD-session rensas
det underliggande lagringsutrymmet för den specifika MVPD-produkten, inklusive
andra autentiseringstoken för programmerare som erhållits via enkel inloggning
den enheten. Token som erhållits för andra videoprogrammerings-programmerings-ID eller inte via enkel inloggning kommer inte att
tas bort.


## Tokens {#tokens}

- [Definitioner och användning](#definitions)
- [Riktlinjer för cachelagring](#caching)
- [Persistence](#persistence)
- [Format](#format)
- [Enhetsbindning](#device_binding)



### Definitioner och användning {#definitions}

Adobe Pass Authentication-berättigandelösningen kretsar kring genereringen av specifika datadelar (tokens) som genereras av Adobe Pass Authentication när autentiserings- och auktoriseringsarbetsflödena har slutförts. Dessa token lagras lokalt på klientens Android-enhet.

Tokens har begränsad livslängd. När den upphör att gälla måste tokens utfärdas på nytt genom att autentiserings- och/eller auktoriseringsarbetsflödena återupptas.

Det finns tre typer av tokens som utfärdas under tillståndsarbetsflödena:

- **Autentiseringstoken** - Slutresultatet av användarautentiseringsarbetsflödet blir ett autentiserings-GUID som AccessEnabler kan använda för att skapa auktoriseringsfrågor för användarens räkning. Detta autentiserings-GUID har ett associerat TTL-värde (time-to-live) som kan skilja sig från användarens autentiseringssession. Adobe Pass Authentication genererar en autentiseringstoken genom att binda autentiserings-GUID till den enhet som initierar autentiseringsbegäranden.
- **Auktoriseringstoken** - Ger åtkomst till en specifik skyddad resurs som identifieras av en unik `resourceID`. Det består av ett auktoriseringsbidrag som utfärdats av den auktoriserande parten tillsammans med originalet `resourceID`. Den här informationen är bunden till den enhet som initierar begäran.
- **Kortlivad medietoken** - AccessEnabler beviljar åtkomst till värdprogrammet för en given resurs genom att returnera en kortlivad medietoken. Denna token genereras baserat på den auktoriseringstoken som tidigare förvärvats för just den aktuella resursen. Den här token är inte bunden till enheten och den associerade livstiden är betydligt kortare (standard: 5 minuter).

När autentiseringen och auktoriseringen är klar kommer Adobe Pass Authentication att utfärda autentiserings-, auktoriserings- och kortlivade medietoken. Dessa token bör cachelagras på användarens enhet och användas under hela den tid som de är kopplade till sin livstid.



### Riktlinjer för cachelagring {#caching}

- Autentiseringstoken
- Auktoriseringstoken
- Medietoken


#### Autentiseringstoken

- **AccessEnabler 1.6 och äldre** - Hur autentiseringstoken cachas på enheten beror på flaggan **Authentication per Requestor** som är associerad med den aktuella MVPD:


1. Om funktionen &quot;Autentisering per begärande&quot; är *inaktiverad* lagras en enda autentiseringstoken lokalt på det globala monteringsbordet. Denna token delas mellan alla program som är integrerade med den aktuella MVPD.
1. Om funktionen &quot;Autentisering per begärande&quot; är *aktiverad* kopplas en token explicit till programmeraren som utförde autentiseringsflödet (token lagras inte på det globala monteringsbordet, utan i en privat fil som bara är synlig för programmerarens program). Mer specifikt kommer enkel inloggning (SSO) mellan olika program att inaktiveras. Användaren måste utföra autentiseringsflödet explicit när han/hon byter till en ny app (förutsatt att programmeraren för den andra appen är integrerad med den aktuella MVPD och att det inte finns någon autentiseringstoken för den programmeraren i det lokala cacheminnet).

   **Obs!** AE 1.6 Google GSON Tech Note: [Lösa Gson-beroenden](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** - Den här SDK introducerar en ny metod för tokenlagring, som aktiverar flera programmerings-MVPD-buketter och därmed flera autentiseringstoken. Från AE 1.7 används samma lagringslayout både för scenariot Autentisering per begärande och för det normala autentiseringsflödet. Den enda skillnaden mellan de två är hur autentiseringen utförs: &quot;Authentication per Requestor&quot; innehåller en ny förbättring (passiv autentisering) som gör det möjligt för AccessEnabler att utföra autentisering i bakkanalen baserat på att det finns en autentiseringstoken i lagringen (för en annan programmerare). Användaren behöver bara autentisera en gång, och den här sessionen kommer att användas för att hämta autentiseringstoken i efterföljande appar. Det här bakkanalsflödet äger rum under [`setRequestor()`](#setRequestor)-anropet och är för det mesta genomskinligt för programmeraren. Det finns dock ett viktigt krav här: Programmeraren MÅSTE anropa [`setRequestor()`](#setRequestor) från huvudgränssnittstråden och inifrån en aktivitet.


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



Från och med AccessEnabler 1.7 kan tokenlagringen ha stöd för flera programmerare-MVPD-kombinationer, beroende på en kapslad mappningsstruktur på flera nivåer som kan innehålla flera autentiseringstoken. Det nya lagringsutrymmet påverkar inte det offentliga API:t för AccessEnabler på något sätt och kräver inga ändringar från programmerarens sida. Här följer ett exempel som visar den här nyare funktionen:

1. Öppna App1 (utvecklad av Programmer1).
1. Autentisera med MVPD1 (som är integrerat med Programmer1).
1. Skjut upp/stäng det aktuella programmet och öppna App2 (utvecklad av Programmer2).
1. Låt oss anta att Programmer2 inte är integrerat med MVPD2. Därför kommer användaren INTE att autentiseras i App2.
1. Autentisera med MVPD2 (som är integrerat med Programmer2) i App2.
1. Växla tillbaka till App1. Användaren autentiseras fortfarande med Programmer1.

I äldre versioner av AccessEnabler återges användaren som icke-autentiserad i steg 6, eftersom tokenlagringen tidigare bara hade stöd för en autentiseringstoken.


**Obs!** Om du loggar ut från en programmerare-/MVPD-session rensas det underliggande lagringsutrymmet, inklusive alla andra autentiseringstoken för programmerare på enheten med enkel inloggning. Token som erhållits för andra MVPD eller inte via enkel inloggning tas inte bort. Om autentiseringsflödet avbryts (anropar [`setSelectedProvider(null)`](#setSelectedProvider)) rensas INTE det underliggande lagringsutrymmet, men det påverkar bara det aktuella autentiseringsförsöket för Programmer/MVPD (genom att MVPD raderas för den aktuella programmeraren).


En annan lagringsrelaterad funktion som ingår i AccessEnabler 1.7 gör det möjligt att importera autentiseringstoken från äldre lagringsområden. Den här&quot;tokenimporteraren&quot; hjälper till att uppnå kompatibilitet mellan efterföljande AccessEnabler-versioner och upprätthålla SSO-läget även när lagringsversionen uppgraderas.

Importören körs under [`setRequestor()`](#setRequestor)-flödet och körs i följande två scenarier (förutsatt att det inte finns någon giltig autentiseringstoken för den aktuella programmeraren i det aktuella lagringsutrymmet):

- Den första installationen av en 1.7-app som har utvecklats av en specifik programmerare
- Uppgraderingsväg till en framtida AccessEnabler som använder en ny lagringsplats

Importåtgärden är genomskinlig för programmeraren och kräver ingen kodändring i klientprogrammet.



### Format {#format}

- [Autentiseringstoken](#authn_token)
- [Auktoriseringstoken](#authz_token)
- [Kort medietoken](#short_media_token)
- [Enhetsbindning](#device_binding)

#### Autentiseringstoken {#authn_token}

I listan nedan visas formatet för autentiseringstoken:

```XML
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

```XML
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

```XML
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


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
