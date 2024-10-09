---
title: Användarmetadatacertifikat för kryptering
description: Användarmetadatacertifikat för kryptering
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a2
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# Användarmetadatacertifikat för kryptering

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

För krypterade användarmetadata måste Adobe Pass Authentication-integrering ha ett privat/offentligt nyckelpar.

I det här dokumentet beskrivs en process för att generera certifikat för offentlig nyckel för användning i Adobe Pass-autentisering. Processen som beskrivs här använder OpenSSL-verktygen.

## Processen för certifikatgenerering (#generation)

1. Hämta och installera OpenSSL-verktygen (http://www.openssl.org).

1. Generera en CSR-begäran (Certificate Signing Request):

   * Skapa ett nyckelpar.  Öppna ett kommando-/terminalfönster och kör följande kommando:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Generera CSR. Kör följande på kommandoraden:

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

1. Certifikatutfärdaren skickar certifikatet i .p7b-format (PKCS#7, Cryptographic Message Syntax Standard)

1. Distribuera P7b-certifikatet. Konvertera PKCS#7-filen (.p7b) till en PKCS#12-fil (PFX-fil, Personal Information Exchange Syntax Standard) med hjälp av din privata nyckel och generera PEM-filen (sammanfogad certifikatbehållarfil):

   * Konvertera PKCS#7-filen till en tillfällig PEM-fil. Kör följande på kommandoraden:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Konvertera den tillfälliga PEM-filen till en PFX-fil.  Kör följande på kommandoraden:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Konvertera den tillfälliga PEM-filen till en slutgiltig PEM-fil. Kör följande på kommandoraden:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Skicka den slutliga PEM-filen till Adobe för att få den konfigurerad.

   * Den person som slutligen behöver få PEM-filen är den Adobe-tekniker som är tilldelad din integrering/validering. Om du inte arbetar direkt med den personen kan du ta reda på vem du ska skicka filen till från Adobe.
   * Adobe stöder både ett primärt certifikat och ett säkerhetskopieringscertifikat. Om ditt primära certifikat äventyras på något sätt kan du återkalla det och växla till det sekundära certifikatet för att signera det begärande-ID:t i din app. Detta kommer att säkerställa en smidig övergång mellan certifikat i produktionen, med minimal inverkan på kunderna.
   * När Adobe har tagit emot PEM-filen kommer autentiseringsteknikerna att lägga till den i din konfiguration på serversidan och bekräfta att filen har tagits emot.
