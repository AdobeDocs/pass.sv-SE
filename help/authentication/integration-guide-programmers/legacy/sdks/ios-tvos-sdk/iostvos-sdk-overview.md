---
title: iOS/tvOS SDK - översikt
description: iOS/tvOS SDK - översikt
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# (Äldre) iOS/tvOS SDK - översikt {#iostvos-sdk-overview}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

</br>


## Introduktion {#intro}

iOS AccessEnabler är ett målinriktat iOS/tvOS-bibliotek som gör att mobilappar kan använda Adobe Pass Authentication for TV Everywhere-berättigandetjänster. Implementeringen består av gränssnittet *AccessEnabler* som definierar berättigande-API:t samt protokollen *EntitlementDelegate* och *[EntitlementStatus](#ios%20entitlement%20status)* som beskriver återanropen som biblioteket utlöser. Gränssnittet och protokollet kallas under ett gemensamt namn: AccessEnabler-biblioteket.

## Krav för iOS och tvOS {#reqs}

Aktuella tekniska krav för iOS och tvOS-plattformen och Adobe Pass-autentisering finns i [Plattform/Enhet/Verktygskrav](#ios) och i versionsinformationen som medföljer vid hämtningen av SDK. Under resten av den här sidan ser du avsnitt som innehåller ändringar som gäller vissa versioner av SDK och senare. Följande är till exempel en giltig anmärkning om SDK 1.7.5:

## Understanding Native Client Workflows {#flows}

Inbyggda klientarbetsflöden är vanligtvis desamma som eller liknar dem för webbläsarbaserade Adobe Pass Authentication-klienter. Det finns dock några undantag, som beskrivs nedan.

- [Arbetsflöde efter initiering](#post-init)
- [Allmänt inledande autentiseringsarbetsflöde](#generic)
- [Utloggningsarbetsflöde](#logout)


### Arbetsflöde efter initiering {#post-init}

Alla tillståndsarbetsflöden som stöds av AccessEnabler förutsätter att du tidigare har anropat [`setRequestor()`](#setReq) för att upprätta din identitet. Du gör det här anropet för att bara ange ditt begärande-ID en gång, vanligtvis under programmets initierings-/installationsfas.


När du har ett iOS-klientprogram kan du efter ditt första anrop till [`setRequestor()`](#setReq) välja hur du vill fortsätta:

- Du kan börja ringa berättigandesamtal direkt och låta dem stå i kö om det behövs.

- Du kan få en bekräftelse på att [`setRequestor()`](#setReq) lyckades/misslyckades genom att implementera återanropet [`setRequestorComplete()`](#setReqComplete).

- Du kan göra båda av det ovanstående.

Du kan antingen låta din app vänta på att meddelandet om att [`setRequestor()`](#setReq) lyckades visas eller låta den förlita sig på anropskömekanismen i AccessEnabler. Eftersom alla efterföljande autentiserings- och autentiseringsbegäranden behöver begärande-ID och den associerade konfigurationsinformationen blockerar metoden [`setRequestor()`](#setReq) alla autentiserings- och auktoriserings-API-anrop tills initieringen är slutförd.



### Allmänt inledande autentiseringsarbetsflöde {#generic}

Syftet med det här arbetsflödet är att logga in en användare med sin MVPD. När inloggningen är klar utfärdar backend-servern en autentiseringstoken till användaren. Autentisering sker vanligtvis som en del av auktoriseringsprocessen, men följande beskriver hur autentisering kan fungera fristående och inkluderar inga auktoriseringssteg.

Observera att även om det här arbetsflödet skiljer sig åt för inbyggda klienter från det vanliga webbläsarbaserade autentiseringsarbetsflödet, är steg 1-5 samma för både inbyggda klienter och webbläsarbaserade klienter.

1. Ditt program initierar autentiseringsarbetsflödet med ett anrop till AccessEnablers `getAuthentication() `API-metod, som söker efter en giltig cachelagrad autentiseringstoken.
1. Om användaren är autentiserad, anropar AccessEnabler din [`setAuthenticationStatus()`](#setAuthNStatus)-callback-funktion och skickar en autentiseringsstatus som anger att processen lyckades och att flödet avslutas.
1. Om användaren inte är autentiserad för tillfället fortsätter AccessEnabler autentiseringsflödet genom att fastställa om användarens senaste autentiseringsförsök lyckades med en viss MVPD. Om ett MVPD-ID cachelagras OCH flaggan `canAuthenticate` är true ELLER om en MVPD valdes med [`setSelectedProvider()`](#setSelProv) visas ingen dialogruta för val av MVPD. Autentiseringsflödet fortsätter med det cachelagrade värdet för MVPD (det vill säga samma MVPD som användes vid den senaste autentiseringen). Ett nätverksanrop görs till backend-servern och användaren omdirigeras till inloggningssidan för MVPD (steg 6 nedan).
1. Om inget MVPD-ID cachelagras OCH ingen MVPD har valts med [`setSelectedProvider()`](#setSelProv) ELLER om flaggan `canAuthenticate` har värdet false anropas [`displayProviderDialog()`](#dispProvDialog)-återanropet. Det här återanropet instruerar programmet att skapa användargränssnittet som visar användaren en lista över MVPD som du kan välja mellan. En array med MVPD-objekt som innehåller den information som krävs för att du ska kunna skapa MVPD-väljaren. Varje MVPD-objekt beskriver en MVPD-enhet och innehåller information som t.ex. MVPD-ID (t.ex. XFINITY, AT\&amp;T) och den URL där MVPD-logotypen finns.
1. När du har valt en viss MVPD måste programmet informera AccessEnabler om vad användaren har valt. När användaren har valt önskad MVPD informerar du AccessEnabler om användarvalet via ett anrop till metoden [`setSelectedProvider()`](#setSelProv).
1. IOS AccessEnabler anropar ditt `navigateToUrl:`-återanrop eller `navigateToUrl:useSVC:`-återanrop för att dirigera om användaren till inloggningssidan för MVPD. Genom att aktivera någon av dem, skickar AccessEnabler en begäran till ditt program om att skapa en `UIWebView/WKWebView or SFSafariViewController`-kontrollant och att läsa in URL:en som anges i callback-objektets `url`-parameter. Detta är URL:en för autentiseringsslutpunkten på backend-servern. För tvOS AccessEnabler anropas callback-funktionen [status()](#status_callback_implementation) med en `statusDictionary` -parameter och avsökningen för den andra skärmautentiseringen påbörjas omedelbart. `statusDictionary` innehåller `registration code` som behöver användas för den andra skärmautentiseringen.
1. Om iOS AccessEnabler används, kommer användaren till inloggningssidan för MVPD att ange sina inloggningsuppgifter via programstyrenheten `UIWebView/WKWebView or SFSafariViewController `. Observera att flera omdirigeringsåtgärder utförs under den här överföringen och att programmet måste övervaka de URL:er som läses in av kontrollenheten under de flera omdirigeringsåtgärderna.
1. Om iOS AccessEnabler används måste programmet stänga kontrollenheten när `UIWebView/WKWebView or SFSafariViewController`-kontrollenheten läser in en anpassad URL och anropa AccessEnablers `handleExternalURL:url ` -API-metod. Observera att den här anpassade URL:en är ogiltig och inte avsedd för att styrenheten ska läsa in den. Det får endast tolkas av ditt program som en signal om att autentiseringsflödet har slutförts och att det är säkert att stänga `UIWebView/WKWebView or SFSafariViewController`-styrenheten. Om ditt program måste använda en `SFSafariViewController `kontrollant definieras den anpassade URL:en av `application's custom scheme` (t.ex.: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), annars definieras den här anpassade URL:en av konstanten `ADOBEPASS_REDIRECT_URL` (t.ex. `adobepass://ios.app`).
1. När programmet stänger `UIWebView/WKWebView or SFSafariViewController`-kontrollanten och anropar AccessEnabler-API-metoden `handleExternalURL:url ` hämtar AccessEnabler autentiseringstoken från backend-servern och informerar programmet om att autentiseringsflödet är slutfört. AccessEnabler anropar återanropet [`setAuthenticationStatus()`](#setAuthNStatus) med statuskoden 1, vilket anger att åtgärden lyckades. Om det uppstår ett fel under körningen av de här stegen aktiveras [`setAuthenticationStatus()`](#setAuthNStatus)-återanropet med statuskoden 0, vilket anger autentiseringsfel och en motsvarande felkod.


>[!WARNING]
>
> Under de steg där AccessEnabler släpper kontrollen till ditt program (t.ex. när dialogrutan för val av leverantör visas eller när UIWebView/WKWebView eller SFSafariViewController visas) har användaren möjlighet att avbryta autentiseringsflödet. I dessa situationer ansvarar din app för att informera AccessEnabler om den här händelsen och anropa API-metoden [`setSelectedProvider()`](#setSelProv) och skicka null som en parameter. Detta ger AccessEnabler en möjlighet att rensa upp det interna tillståndet och återställa autentiseringsflödet. På tvOS kan du använda samma metod för att avbryta autentiseringsavsökningen.


### Utloggningsarbetsflöde {#logout}

För inbyggda klienter hanteras utloggningen på liknande sätt som autentiseringsprocessen som beskrivs ovan.

1. Ditt program initierar utloggningsarbetsflödet med ett anrop till AccessEnablers `logout() `API-metod. Utloggningen är resultatet av en serie HTTP-omdirigeringsåtgärder på grund av att användaren måste loggas ut både från Adobe Pass autentiseringsservrar och från MVPD servrar. Eftersom det här flödet inte kan slutföras med en enkel HTTP-begäran som utfärdas av AccessEnabler-biblioteket, måste en `UIWebView/WKWebView or SFSafariViewController`-styrenhet instansieras för att kunna följa HTTP-omdirigeringsåtgärderna.

1. Ett mönster som liknar autentiseringsflödet används. IOS AccessEnabler utlöser återanropet `navigateToUrl:` eller `navigateToUrl:useSVC:` för att skapa en `UIWebView/WKWebView or SFSafariViewController`-kontrollant och för att läsa in URL:en som anges i återanropets `url`-parameter. Det här är URL:en för utloggningsslutpunkten på backend-servern. För tvOS AccessEnabler anropas varken callback-funktionen `navigateToUrl:` eller callback-funktionen `navigateToUrl:useSVC:`.

1. När programmet går igenom flera omdirigeringar måste du övervaka aktiviteten för `UIWebView/WKWebView or SFSafariViewController `kontrollanten och identifiera tidpunkten när den läser in en specifik anpassad URL. Observera att den här anpassade URL:en är ogiltig och inte avsedd för att styrenheten ska läsa in den. Det får endast tolkas av programmet som en signal på att utloggningsflödet har slutförts och att det är säkert att stänga kontrollenheten. När kontrollenheten läser in den här anpassade URL:en måste programmet stänga kontrollenheten och anropa AccessEnablers `handleExternalURL:url `API-metod. Om ditt program måste använda en `SFSafariViewController `kontrollant definieras den anpassade URL:en av `application's custom scheme` (t.ex. `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), annars definieras den här anpassade URL:en av konstanten ` ADOBEPASS_REDIRECT_URL  ` (t.ex. `adobepass://ios.app`).

1. I slutet anropar AccessEnabler återanropet [`setAuthenticationStatus()`](#setAuthNStatus) med statuskoden 0, vilket anger att utloggningsflödet lyckades.

Utloggningsflödet skiljer sig från autentiseringsflödet på så sätt att användaren inte behöver interagera med `UIWebView/WKWebView or SFSafariViewController`-styrenheten på något sätt. Därför rekommenderar Adobe att du gör kontrollen osynlig (dvs. dold) under utloggningsprocessen.

## Tokens {#tokens}

- [Definitioner och användning](#definitions)
- [Riktlinjer för cachelagring](#caching)
- [Persistence](#persistence)
- [Format](#format)
- [Enhetsbindning](#device_binding)


### Definitioner och användning {#definitions}

Adobe Pass Authentication-berättigandelösningen kretsar kring genereringen av specifika datadelar (tokens) som genereras av Adobe Pass Authentication när autentiserings- och auktoriseringsarbetsflödena har slutförts. Dessa token lagras lokalt på klientens iOS-enhet.



Tokens har begränsad livslängd. När den upphör att gälla måste tokens utfärdas på nytt genom att autentiserings- och/eller auktoriseringsarbetsflödena återupptas.



Det finns tre typer av tokens som utfärdas under tillståndsarbetsflödena:

- **Autentiseringstoken:** Slutresultatet av användarautentiseringsarbetsflödet blir ett autentiserings-GUID som AccessEnabler kan använda för att skapa auktoriseringsfrågor för användarens räkning. Detta autentiserings-GUID har ett associerat TTL-värde (time-to-live) som kan skilja sig från användarens autentiseringssession. En autentiseringstoken genereras genom att autentiserings-GUID binds till den enhet som initierar autentiseringsbegäranden.
- **Auktoriseringstoken:** Bevilja åtkomst till en specifik skyddad resurs som identifieras av ett unikt resurs-ID. Det består av ett auktoriseringsbidrag som utfärdats av den auktoriserande parten tillsammans med det ursprungliga resurs-ID:t. Den här informationen är bunden till den enhet som initierar begäran.
- **Kortlivad medietoken:** AccessEnabler beviljar åtkomst till värdprogrammet för en given resurs genom att returnera en kortlivad medietoken. Denna token genereras baserat på den auktoriseringstoken som tidigare förvärvats för just den aktuella resursen. Den här token är inte bunden till enheten och den associerade livstiden är betydligt kortare (standard: 5 minuter).

När autentiseringen och auktoriseringen är klar kommer Adobe Pass Authentication att utfärda autentiserings-, auktoriserings- och kortlivade medietoken. Dessa token bör cachelagras på användarens enhet och användas under hela den tid som de är kopplade till sin livstid.



### Riktlinjer för cachelagring {#caching}

- Autentiseringstoken
- Auktoriseringstoken
- Kortlivad medietoken


#### Autentiseringstoken

- **AccessEnabler 1.7:** SDK introducerar en ny metod för tokenlagring, vilket aktiverar flera programmerings-MVPD-bucket och därmed flera autentiseringstoken. Nu används samma lagringslayout både för scenariot Autentisering per begärande och för det normala autentiseringsflödet. Den enda skillnaden mellan de två är hur autentiseringen utförs: &quot;Authentication per Requestor&quot; innehåller en ny förbättring (Passiv Authentication) som gör det möjligt för AccessEnabler att utföra autentisering i bakkanalen baserat på att det finns en autentiseringstoken i lagringen (för en annan programmerare). Användaren behöver bara autentisera en gång, och den här sessionen kommer att användas för att hämta autentiseringstoken i ytterligare appar. Det här bakkanalsflödet äger rum under [`setRequestor()`](#setReq)-anropet och är för det mesta genomskinligt för programmeraren. **Det finns dock ett viktigt krav här: Programmeraren MÅSTE anropa setRequestor() från huvudgränssnittstråden.**
- **AccessEnabler 1.6 och äldre:** Hur autentiseringstoken cachas på enheten beror på flaggan **Authentication per Requestor** som är associerad med den aktuella MVPD:

<!-- end list -->

1. Om funktionen &quot;Autentisering per begärande&quot; är inaktiverad, kommer en enda autentiseringstoken att lagras lokalt på det globala monteringsbordet. Denna token delas mellan alla program som är integrerade med det aktuella MVPD.
1. Om funktionen &quot;Autentisering per begärande&quot; är aktiverad, kommer en token att uttryckligen kopplas till den programmerare som utförde autentiseringsflödet (denna token kommer inte att lagras på det globala monteringsbordet, utan i en privat fil som bara är synlig för programmerarens program). Mer specifikt kommer enkel inloggning (SSO) mellan olika program att inaktiveras. Användaren måste utföra autentiseringsflödet explicit när han/hon byter till en ny app (förutsatt att programmeraren för den andra appen är integrerad med den aktuella MVPD och att det inte finns någon autentiseringstoken för den programmeraren i det lokala cacheminnet).



#### Auktoriseringstoken

Vid en given tidpunkt cachelagras endast EN auktoriseringstoken PER RESOURCE av AccessEnabler. Det kan finnas flera autentiseringstoken cachelagrade, men de är associerade med olika resurser. När en ny auktoriseringstoken utfärdas och en gammal redan finns för *samma resurs*, skriver den nya token över det befintliga cachelagrade värdet.



#### Medietoken

Den kortlivade medietoken får INTE cachelagras alls. Medietoken bör hämtas från servern varje gång ett auktoriserings-API anropas, eftersom det är begränsat till engångsanvändning.



### Persistence {#persistence}

Token måste vara beständig i flera på varandra följande körningar av samma program. Detta innebär att när autentiserings- och auktoriseringstoken har hämtats och användaren stänger programmet, är samma token tillgängliga för programmet när användaren öppnar programmet igen. Dessutom är det önskvärt att dessa variabler är beständiga i flera program. När en användare har använt ett program för att logga in med en viss identitetsleverantör (har hämtat autentiserings- och auktoriseringstoken) kan samma token användas via ett annat program, och användaren uppmanas inte längre att ange autentiseringsuppgifter när han eller hon loggar in via samma identitetsleverantör. Den här typen av smidigt arbetsflöde för autentisering/auktorisering gör Adobe Pass Authentication-lösningen till en riktig TV-Everywhere
implementering.



## iOS

IOS AccessEnabler-biblioteket kan användas för att kringgå problem med datadelning mellan program genom att tokendata lagras i en urklippsliknande datastruktur som kallas *paste board*. Den här delade resursen på systemnivå innehåller nyckelkomponenter som gör att det går att implementera de önskade permanenta token som ska användas:

- **Stöd för strukturerad lagring** - Klippbordet är inte bara en enkel, linjär buffertliknande minnesstruktur. Den innehåller en ordlisteliknande lagringsmekanism som gör det möjligt att indexera data baserat på användarspecificerade nyckelvärden.
- **Stöd för databeständighet med det underliggande filsystemet** - Innehållet i inklistringskortets struktur kan markeras som beständigt. I så fall sparas data på enhetens interna minne.



När en viss token har placerats i token-cachen kontrolleras dess giltighet vid olika tillfällen av AccessEnabler-biblioteket.  En giltig token definieras som:

- Token har inte gått ut.
- Utfärdaren av token ingår i listan över tillåtna identitetsleverantörer.



## tvOS

Eftersom monteringsbordet inte är tillgängligt på tvOS använder biblioteket tvOS AccessEnabler NsUserDefaults som lagringsalternativ. Detta löser problemet med beständiga autentiserings- och behörighetstoken, men den lagrade informationen kan inte delas mellan olika program.



**Ändringar av monteringsbordet i iOS 10 -** När du uppgraderar från en tidigare version av iOS raderas monteringsbordet. Det innebär att alla program måste autentiseras på nytt.



**Ändringar av monteringsbordet i iOS 7 -** På grund av ändringar i hur monteringsbord fungerar i iOS 7 kommer det att finnas ett begränsat genomflöde mellan program som körs i iOS 7. Program som har samma `<Bundle Seed ID>` (kallas även `<Team ID>`) delar tokens, vilket innebär att program A1 och A2 från samma programmerare X delar tokens, medan program A1 (Programmer X) och program A3 (Programmer Y) inte delar tokens.

- Källpaket-ID/Team-ID är samma mellan två program om de genereras av samma provisioneringsprofil. Här hittar du mer information:
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Den här begränsningen för enkel inloggning (Cross SSO) finns i iOS 7 oavsett vilken Adobe Pass Authentication SDK som används.

Läs den här TechNote-artikeln om du vill ha mer information om hur du konfigurerar enkel inloggning på iOS 7 och senare (TechNote gäller för Access Enabler v1.8 och senare): <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Tokenlagring (AccessEnabler 1.7)

Från och med AccessEnabler 1.7 kan tokenlagringen ha stöd för flera programmerare-MVPD-kombinationer, beroende på en kapslad mappningsstruktur på flera nivåer som kan innehålla flera autentiseringstoken. Det nya lagringsutrymmet påverkar inte det offentliga API:t för AccessEnabler på något sätt och kräver inga ändringar från programmerarens sida. Här är ett exempel på att
visar den här nya funktionen:

1. Öppna App1 (utvecklad av Programmer1).
1. Autentisera med MVPD1 (som är integrerat med Programmer1).
1. Skjut upp/stäng det aktuella programmet och öppna App2 (utvecklad av Programmer2).
1. Låt oss anta att Programmer2 inte är integrerat med MVPD2. Därför kommer användaren INTE att autentiseras i App2.
1. Autentisera med MVPD2 (som är integrerat med Programmer2) i App2.
1. Växla tillbaka till App1. Användaren autentiseras fortfarande med Programmer1.

I äldre versioner av AccessEnabler återges användaren som icke-autentiserad i steg 6, eftersom tokenlagringen tidigare bara hade stöd för en autentiseringstoken.



Om du loggar ut från en Programmer-/MVPD-session rensas hela det underliggande lagringsutrymmet, inklusive alla andra autentiseringstoken för Programmer/MVPD på enheten. Om du däremot avbryter autentiseringsflödet (anropar [`setSelectedProvider(null)`](#setSelProv)) rensas inte det underliggande lagringsutrymmet, men det påverkar bara det aktuella autentiseringsförsöket för Programmer/MVPD (genom att MVPD raderas för den aktuella Programmeraren).



### Tokenimporterare (AccessEnabler 1.7)

En annan lagringsrelaterad funktion som ingår i AccessEnabler 1.7 gör det möjligt att importera autentiseringstoken från äldre lagringsområden. Den här&quot;tokenimporteraren&quot; hjälper till att uppnå kompatibilitet mellan efterföljande AccessEnabler-versioner och upprätthålla SSO-läget även när lagringsversionen uppgraderas. Importeraren körs under [`setRequestor()`](#setReq)-flödet och körs i följande två scenarier (förutsatt att det inte finns någon giltig autentiseringstoken för den aktuella programmeraren i det aktuella lagringsutrymmet):

- Den första installationen av en 1.7-app som har utvecklats av en specifik programmerare
- Uppgradera till en framtida AccessEnabler som använder en ny lagringsplats

Importåtgärden är genomskinlig för programmeraren och kräver ingen kodändring i klientprogrammet.



### Token Sanitizer (AccessEnabler 1.7.5)

Från AccessEnabler 1.7.5 och framåt kan den här tjänsten köras på [`setRequestor()`](#setReq)`. `Den har utvecklats som ett resultat av iOS 7-bytet från WiFi MAC-adressen till IDFA för spårning. Sanitizer ser till att den aktuella lagringen bara innehåller giltiga autentiseringstoken (giltiga för enhets-ID, som tidigare beräknats med MAC-adressen, före iOS7). Token Sanitizer tar bort alla ogiltiga tokens.



Token Sanitizer-tjänsten är mest användbar när ett AccessEnabler 1.7.5-program används i iOS 6 och sedan uppdaterar användaren till iOS 7. När detta inträffar blir alla autentiseringstoken som hämtades på iOS 6 ogiltiga (på grund av att algoritmen för enhets-ID ändras från att använda MAC-adressen till IDFA). Sanitizer rensar alla ogiltiga tokens och användaren blir då oautentiserad.



Om inte Token Sanitizer tar bort ogiltiga tokens kan AccessEnabler inte erhålla auktoriseringar på grund av ogiltiga autentiseringstoken. Detta skulle visa sig vara svårt för slutanvändaren att felsöka, eftersom verifieringsfelmeddelanden kan vara kryptiska och inte alltför användbara för att avgöra vad som orsakade problemet.



### Format {#format}

- [AuthN-token](#authn_token)
- [AuthZ-token](#authz_token)
- [Kort medietoken](#short_token)
- [Enhetsbindning](#device_binding)


Observera att formatet för AuthN- och AuthZ-tokens inkluderas här endast för bakgrundsinformation. Strukturen för dessa tokens kan när som helst ändras av Adobe Pass Authentication. Programmerare behöver inte känna till den exakta strukturen för AuthN- och AuthZ-tokens för att implementera sina appar, eftersom AuthN- och AuthZ-tokens inte visas på den lokala enheten. Kort medietoken *är* exponerad för programmerarens program.



#### Autentiseringstoken {#authn_token}

I listan nedan visas formatet för autentiseringstoken:

```
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

```
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


#### Kort medietoken {#short_token}

I listan nedan visas formatet för den korta medietoken. Denna token visas för programmerarens program. Det skickas till Programmerarens program när en tillståndsprocess är slutförd:

```
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


### Enhetsbindning {#device_binding}

Observera taggen `simpleTokenFingerprint` i XML-listan ovan. Syftet med den här taggen är att lagra information om anpassad enhets-ID. AccessEnabler-biblioteket kan hämta sådan individualiseringsinformation och göra den tillgänglig för Adobe Pass Authentication Services under berättigandeanropen. Tjänsten kommer att använda den här informationen och bädda in den i de faktiska tokenerna, vilket effektivt binder tokenerna till en viss enhet. Slutmålet för detta är att göra tokens icke-överförbara över olika enheter.



Eftersom detta är en uppenbart säkerhetsrelaterad funktion är denna information i sig&quot;känslig&quot; ur säkerhetssynpunkt. Därför måste denna information skyddas mot både manipulering och tjuvlyssning. Eavesdropping-problemet löses genom att autentiserings-/auktoriseringsbegäranden skickas via HTTPS-protokollet. Manipuleringsskyddet hanteras genom att enhetsidentifieringsinformationen signeras digitalt. AccessEnabler-biblioteket beräknar ett enhets-ID från information som enheten anger och skickar sedan enhets-ID:t &quot;i klartext&quot; till Adobe Pass autentiseringsservrar som en begärandeparameter. Adobe Pass autentiseringsservrar signerar digitalt enhets-ID med Adobe privata nyckel och lägger till det i den autentiseringstoken som returneras till AccessEnabler. Detta innebär att enhets-ID är bundet till autentiseringstoken. Under auktoriseringsflödet skickar AccessEnabler igen enhets-ID:t i rensningen tillsammans med autentiseringstoken. Om valideringsprocessen misslyckas kommer autentiserings-/auktoriseringsarbetsflödena automatiskt att misslyckas. Adobe Pass autentiseringsservrar använder den privata nyckeln för enhets-ID och jämför den med värdet i autentiseringstoken. Om de inte matchar misslyckas tillståndsflödet.



**Kommentar om enhetsbindning i AccessEnabler 1.7.5:** Från och med AccessEnabler 1.7.5 ändras hur enhets-ID beräknas för att ange personaliseringsinformation för en iOS-enhet. Ändringen återspeglar en förändring i iOS 7: Från och med iOS 7 tillhandahåller Apple inte längre WiFi MAC-adressen som ett spårningsalternativ, till förmån för IDFA (Identifier for Advertisers). Eftersom personaliseringsinformation för en app som körs på iOS 7 baseras på IDFA och den informationen är inbäddad i tillståndsflödestoken innebär detta att det finns ett antal olika möjliga effekter på användarupplevelsen som följer av den här ändringen. Olika effekter baseras på vilken version av iOS som användaren uppgraderar från och vilken version av AccessEnabler som programmeraren uppgraderar från. Mer information om den här ändringen finns i versionsinformationen för AccessEnabler SDK 1.7.5.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
