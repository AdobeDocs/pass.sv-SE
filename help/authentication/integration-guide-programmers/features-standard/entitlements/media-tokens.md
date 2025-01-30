---
title: Medietoken
description: Medietoken
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# Medietoken {#media-tokens}

>[!IMPORTANT]
>
> Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Medietoken är en token som genereras av Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) som ett resultat av ett auktoriseringsbeslut som ska ge visningsåtkomst till skyddat innehåll (resurs). Medietoken är giltig under en begränsad och kort tidsperiod (några minuter) som anges vid utfärdandetillfället, vilket anger hur lång tid det måste verifieras och användas av klientprogrammet.

Medietoken består av en signerad sträng baserad på PKI (Public Key Infrastructure) som skickas i klartext. Med det PKI-baserade skyddet signeras token med en asymmetrisk nyckel som utfärdats till Adobe av en certifikatutfärdare (CA).

Medietoken skickas till programmeraren, som sedan kan validera den med Media Token Verifier innan videoströmmen startas för att säkerställa åtkomstsäkerheten för den resursen.

Verifieraren för medietoken är ett bibliotek som distribueras av Adobe Pass Authentication och som ansvarar för att verifiera äktheten för en medietoken.

## Media Token Verifier {#media-token-verifier}

Adobe Pass Authentication rekommenderar att programmerare skickar medietoken till en backend-tjänst som integrerar biblioteket Media Token Verifier för att säkerställa säker åtkomst innan videoströmmen initieras. Medietokenens TTL (time-to-live) är utformad för att ta hänsyn till potentiella klocksynkroniseringsproblem mellan den tokengenererande servern och den validerande servern.

Adobe Pass Authentication ger starkt stöd för att parsa medietoken och extrahera data direkt eftersom tokenformatet inte är garanterat och kan ändras i framtiden. Media Token Verifier-biblioteket bör vara det enda verktyget som används för att analysera tokeninnehållet.

Du kan ladda ned Media Token Verifier-biblioteket via följande länk:

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

Media Token Verifier-biblioteket kräver JDK version 1.5 eller senare och stöder användning av en önskad Java Cryptography Extension-provider (JCE) för signaturalgoritmen (`SHA256WithRSA`).

Biblioteket för verifiering av medietoken som representeras av Java-arkivet `mediatoken-verifier-VERSION.jar` innehåller:

* Adobe offentlig nyckel.
* API för tokenverifiering (`ITokenVerifier.java`).
* Referensimplementering (`com.adobe.entitlement.test.EntitlementVerifierTest.java`).
* Beroenden och certifikatnyckelbehållare.

>[!IMPORTANT]
> 
> Standardlösenordet för den inkluderade certifikatnyckelbehållaren är `123456`.

### Metoder {#methods}

Klassen `ITokenVerifier` definierar följande metoder:

* Metoden `isValid()` som används för att validera medietoken. Det accepterar ett enskilt argument, [resurs-ID](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier). Om den angivna resursidentifieraren är `null` validerar metoden endast medietokenens autentiserings- och giltighetsperiod.

  Metoden `isValid()` returnerar ett av följande statusvärden:

  | VALID_TOKEN | Tokenvalideringar har slutförts |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | Tokenformatet är ogiltigt |
  | INVALID_SIGNATURE | Det gick inte att verifiera tokenautentiseringen |
  | TOKEN_EXPIRED | Token TTL är inte giltig |
  | INVALID_RESOURCE_ID | Token är inte giltig för angiven resurs |
  | ERROR_UNKNOWN | Token har inte verifierats än |

* Metoden `getResourceID()` som används för att hämta den resursidentifierare som är associerad med medietoken och jämföra den med identifieraren som returnerades från auktoriseringsbeslutets svar.

* Metoden `getTimeIssued()` som används för att hämta tiden när medietoken utfärdades.

* Metoden `getTimeToLive()` som används för att hämta medietoken.

* Metoden `getUserSessionGUID()` som används för att hämta en anonym GUID som angetts av MVPD.

* Metoden `getMvpdId()` som används för att hämta identifieraren för den MVPD som autentiserade användaren.

* Metoden `getProxyMvpdId()` som används för att hämta identifieraren för Proxy MVPD som autentiserade användaren.

### Exempel {#sample}

Verifieringsarkivet för medietoken innehåller en referensimplementering (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) och ett exempel på hur API anropas med testklassen. Det här exemplet (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) illustrerar integrationen av Media Token Verifier-biblioteket i en medieserver.

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST API V2 {#rest-api-v2}

Medietoken kan hämtas med följande API:

* [Hämta auktoriseringsbeslut med hjälp av specifik mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Se avsnitten **Svar** och **Exempel** i API:t ovan för att förstå strukturen för auktoriseringsbeslut och medietokens.

Mer information om hur och när ovanstående API ska integreras finns i följande dokument:

* [Grundläggande auktoriseringsflöde som utförs i primärt program](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> Klientprogrammet måste skicka värdet `serializedToken` från returnerad `token` till [Media Token Verifier](#media-token-verifier) för validering.
