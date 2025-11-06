---
title: Användarmetadata
description: Användarmetadata
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '1902'
ht-degree: 0%

---

# Användarmetadata {#user-metadata}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Användarmetadata refererar till användarspecifika [attribut](#attributes) (t.ex. postnummer, klassificeringar av föräldrar, användar-ID:n) som underhålls av programmeringsprogram och tillhandahålls programmerare via Adobe Pass Authentication [REST API V2](#apis).

Användarmetadata blir tillgängliga när autentiseringsflödet har slutförts, men vissa metadataattribut kan uppdateras under auktoriseringsflödet, beroende på MVPD och det specifika metadataattributet i fråga.

Användarmetadata kan användas för att förbättra personaliseringen för användare, men kan också användas för analys. En programmerare kan till exempel använda användarens postnummer för att leverera lokaliserade nyheter eller väderuppdateringar, eller för att framtvinga föräldrakontroll.

Adobe Pass Authentication normaliserar användarens metadatavärden när MVPD tillhandahåller data i olika format. För vissa attribut (t.ex. zip-kod) kan värdena dessutom vara [krypterade](#encryption) med ett programmerarcertifikat.

Med Adobe Pass Authentication kan programmerare granska de användarmetadata som är tillgängliga i deras MVPD-integreringar och [hantera](#management) dem via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

## Attribut för användarmetadata {#attributes}

I följande tabell visas några av de attribut för användarmetadata som är tillgängliga för programmerare:

| Nyckel | Typ | Exempel | Kräver kryptering | Beskrivning | Information |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | Sträng | &quot;1o7241p&quot; | Nej | Kontoidentifierare. | Attributvärdet kan vara en hushållsidentifierare eller en underkontoidentifierare. Värdet `userID` skiljer sig från värdet `householdID` om MVPD stöder underkonton och den aktuella användaren inte är den primära kontoinnehavaren. |
| `upstreamUserID` | Sträng | &quot;1o7241p&quot; | Nej | Kontoidentifierare för övervakning av samtidig användning. | Attributvärdet kan användas för att tillämpa samtidighetsgränser på webbplatser och i appar från MVPD och Programmer. Värdet `upstreamUserID` är samma som värdet `userID` för de flesta PDF-filer. |
| `householdID` | Sträng | &quot;1o7241p&quot; | Nej | Kontoidentifierare för föräldrakontroll. | Attributvärdet kan användas för att skilja mellan hushåll och underkonton. Ibland kan det användas som ett alternativ för föräldrakontroll om det inte finns någon äkta klassificering, och om användaren var inloggad med hushållskontot kan han eller hon titta på, annars visas inte graderat innehåll. Det finns många variationer i de olika programmeringsdokumenten för hur detta ska visas (t.ex. användar-ID för hushåll, huvud för hushåll-ID, huvud för hushållsflagga, osv.). Om MVPD inte stöder underkonton är detta identiskt med `userID`. |
| `primaryOID` | Sträng | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Nej | Kontoidentifierare. | Attributet är specifikt för AT&amp;T. Värdet `primaryOID` är samma som värdet `userID` när värdet `typeID` är Primär. |
| `typeID` | Sträng | &quot;Primär&quot; | Nej | Attribut som anger om den aktuella användaren är primär eller sekundär kontoinnehavare. | Attributet är specifikt för AT&amp;T. Värdet `primaryOID` är samma som värdet `userID` när värdet `typeID` är Primär. |
| `is_hoh` | Sträng | &quot;1&quot; | Nej | Attribut som anger om den aktuella användaren är chef för hushållet eller inte. | Attributet är specifikt för Synacor. |
| `hba_status` | Boolean | &quot;true&quot; | Nej | Attribut som anger om den aktuella användaren autentiserats via HBA eller inte. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | Boolean | &quot;true&quot; | Nej | Attribut som anger om den aktuella enheten kan spegla skärmen eller inte. | Attributet är specifikt för Spectrum. |
| `zip` | Array | \[&quot;77754&quot;, &quot;12345&quot;\] | Ja | Användarens postnummer. | Attributvärdet kan användas för att leverera lokaliserade nyheter, väderuppdateringar och sportevenemang. Värdet `zip` representerar känsliga data som behöver juridiska avtal med MVPD. När nyckeln `zip` är krypterad blir den `String` i stället för `Array`. |
| `encryptedZip` | Sträng | &quot;&quot; | Ja | Användarens krypterade postnummer. | Attributet är specifikt för Comcast. |
| `channelID` | Array | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | Nej | Lista över kanaler som användaren har rätt att visa. | Attributvärdet kan användas för att filtrera olika kanaler från portaler som samlar flera nätverk. Vi rekommenderar att du använder [API:t för förhandsauktorisering](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) i stället för det här attributet för användarmetadata för att filtrera bort kanaler som inte är tillgängliga för användaren. |
| `maxRating` | Objekt | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | Nej | Högsta föräldraklassificering för den aktuella användaren. | Attributvärdet kan användas för att filtrera innehåll som inte är lämpligt för den aktuella användaren baserat på klassificeringarna &quot;MPAA&quot; eller &quot;VCHIP&quot;. |
| `language` | Sträng | &quot;Engelska&quot; | Nej | Språkinställningar. | Attributvärdet kan användas för att visa meddelanden i enlighet med användarens språkinställningar. |

Vilka metadataattribut som är tillgängliga för en programmerare beror på vad en MVPD tillhandahåller. I följande tabell visas de attribut som finns tillgängliga för olika dokumentskyddskomponenter:

|                         | **Juridiskt avtal signerat (endast ZIP)** | **Användar-ID på AuthN** | **Upstream User ID on AuthN** | **Hushålls-ID för AuthN/Z** | **Primärt OID på AuthN** | **Typ-ID på AuthN** | **Hushållschef på AuthN** | **HBA-status** | **Tillåt spegling i AuthZ** | **Postnummer i AuthN/Z** | **Kanal-ID på AuthN** | **Klassificering på AuthN/Z** | **Språk** | **onNet** | **inHome** | **Anteckningar** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Formalt namn** | n/a | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **Kräver kryptering** | n/a | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| **Känslig** | n/a | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Adobe IdP | **Ja** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja** | **Ja** | **Ja** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | Rättsligt avtal behövs inte. |
| Synacor | **Ja** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | Rättsligt avtal som inte täcker alla proxiderade MVPD. Det här är ett generiskt stöd för Synacor och kanske inte sammanfogat till alla deras MVPD-program. |
| Danska | **Nej** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | Den delar samma lista som alla MVPD-filer för synkronisering, plus `upstreamUserID`. |
| Comcast | **Nej** | **Ja** | **Ja** | **Ja (endast AuthZ)** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthZ)** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| AT&amp;T | **Ja** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| DTV | **Ja** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| COX | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Cablevision | **Ja** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| Spektrum | **Ja** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Stadga | **Ja** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Verizon | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| HTC | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Rogers | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| RCN | **Ja** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Eastlink | **Nej** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Cogeco | **Nej** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Videotron | **Nej** | **Ja** | **Ja** | **Ja*** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | Den visar `householdID` med samma värde som `userID`. |
| Proxy Massilon | **Ja** | **Ja** | **Ja** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| Rensning av proxy | **Ja** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Ja (endast AuthZ)** | **Ja** | **Nej** | **Nej** | Rättsligt avtal undertecknat. |
| Proxy GLDS | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Ja (endast AuthN)** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** |                                                                                                                                           |
| Andra videoprogrammeringsprogram | **Nej** | **Ja** | **Ja** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | **Nej** | Inget juridiskt avtal ännu, känsliga metadata är inte tillgängliga för produktion. För alla MVPD-filer är `userID` tillgänglig utan extra arbete. |

>[!IMPORTANT]
>
> Juridiska avtal måste undertecknas med distributörerna innan känsliga användarmetadata (t.ex. postnummer) blir tillgängliga.

## Kryptering av användarmetadata {#encryption}

Om du vill kryptera och dekryptera användarmetadataattribut måste programmeraren generera ett certifikat (offentlig/privat nyckelpar) och [konfigurera &#x200B;](#management) certifikatet automatiskt via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) eller dela den offentliga nyckeln med Adobe Pass autentiseringsrepresentanter.

Följ stegen nedan för att se till att certifikatet skapas och konfigureras korrekt:

1. Hämta och installera OpenSSL-verktygen (http://www.openssl.org).

1. Generera en CSR-begäran (Certificate Signing Request):

   * Skapa ett nyckelpar. Kör följande på kommandoterminalen:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Generera CSR. Kör följande på kommandoterminalen:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Du uppmanas att ange lösenordet för den privata nyckeln.

   * Skapa en säkerhetskopia av din privata nyckel och lösenord. Exempel på CSR:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Skicka CSR till en certifikatutfärdare (CA) (till exempel Verisign).

1. Certifikatutfärdaren skickar certifikatet i .p7b-format (PKCS#7, Cryptographic Message Syntax Standard).

1. Distribuera P7b-certifikatet. Konvertera PKCS#7-filen (.p7b) till en PKCS#12-fil (PFX-fil, Personal Information Exchange Syntax Standard) med hjälp av din privata nyckel och generera PEM-filen (sammanfogad certifikatbehållarfil):

   * Konvertera PKCS#7-filen till en tillfällig PEM-fil. Kör följande på kommandoraden:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Konvertera den tillfälliga PEM-filen till en PFX-fil. Kör följande på kommandoraden:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Konvertera den tillfälliga PEM-filen till en slutgiltig PEM-fil. Kör följande på kommandoraden:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Använd PEM-filen för att [konfigurera](#management) certifikatet via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) eller skicka PEM-filen till Adobe Pass autentiseringsrepresentanter.

   * Mer information om hur du hanterar certifikat via [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) finns i nästa avsnitt.

   * Adobe Pass Authentication stöder både ett primärt certifikat och ett säkerhetskopieringscertifikat. Om ditt primära certifikat äventyras på något sätt kan du återkalla det och växla till det sekundära certifikatet. Detta säkerställer en smidig övergång mellan certifikat med minimal inverkan på kunderna.

## Hantering av användarmetadata {#management}

>[!IMPORTANT]
>
> Om du inte har tillgång till Adobe Pass TV-instrumentpanelen skapar du en biljett via [Zendesk](https://adobeprimetime.zendesk.com) och ber din tekniska kontoansvarige (TAM) att göra lämpliga ändringar åt dig.

Adobe Pass TVE Dashboard är ett verktyg som används av Adobe Pass Authentication-kunder (programmerare) för att hantera konfiguration och data. Den här självbetjäningsinstrumentpanelen möjliggör en rad funktioner som beskrivs i [Adobe Pass TVE Dashboard User Guide](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) -dokumentationen.

Om du vill granska och hantera attribut för användarmetadata som gjorts tillgängliga av en MVPD följer du stegen i [användarhandboken för TVE Dashboard för integreringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata) .

Om du vill granska och hantera certifikat som används för att kryptera attribut för användarmetadata följer du stegen i [TVE Dashboard User Guide for Programmers](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) eller [TVE Dashboard User Guide for Channels](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates) -dokument.

## REST API V2 {#rest-api-v2}

Attribut för användarmetadata kan hämtas med följande API:er:

* [Hämta profiler](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Hämta profil för specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Hämta profil för specifik kod](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Se avsnitten **Svar** och **Exempel** i API:erna ovan för att förstå strukturen för attributen för användarmetadata.

>[!IMPORTANT]
>
> Användarmetadata blir tillgängliga när autentiseringsflödet har slutförts och klientprogrammet behöver därför inte fråga en separat slutpunkt för att hämta informationen för [användarens metadata](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) eftersom den redan ingår i profilinformationen.

Mer information om hur och när ovanstående API:er ska integreras finns i följande dokument:

* [Grundläggande profilflöden som utförs i det primära programmet](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Grundläggande profiler som körs i sekundärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Vissa metadataattribut kan uppdateras under auktoriseringsflödet beroende på MVPD och det specifika metadataattributet. Därför kan klientprogrammet behöva fråga API:erna ovan igen för att hämta de senaste användarmetadata.

>[!MORELIKETHIS]
>
> [Vanliga frågor om autentiseringsfasen](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
