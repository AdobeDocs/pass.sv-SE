---
title: Amazon SSO Cookbook (REST API V1)
description: Amazon SSO Cookbook (REST API V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Amazon SSO Cookbook (REST API V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Adobe Pass Authentication REST API V1 har stöd för Platform Single Sign-On (SSO) för slutanvändare av klientprogram som körs på FireOS.

Det här dokumentet fungerar som ett tillägg till den befintliga [REST API V1 Overview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md) som ger en vy på hög nivå.

## Amazon samlad inloggning med plattformsidentitetsflöden {#cookbook}

### Förutsättningar {#prerequisites}

Innan du fortsätter med Amazon Single Sign-on med plattformsidentitetsflöden måste du kontrollera att följande krav uppfylls.

#### Integrera Amazon SSO SDK {#integrate-amazon-sso-sdk}

Direktuppspelningsprogrammet måste integrera biblioteket [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) för enkel inloggning (SSO) i sitt bygge.

* Hämta och kopiera det senaste Amazon SSO SDK-biblioteket till en `/SSOEnabler`-mapp parallellt med programmets katalog.

* Uppdatera manifest- och Gradle-filerna för att använda Amazon SSO SDK-biblioteket.

  **Manifest:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  Under databaser:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  Under beroenden:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Använd Amazon SSO SDK {#use-amazon-sso-sdk}

Strömningsprogrammet måste använda Amazon SSO SDK för att hämta SSO-tokennyttolasten (platform identity).

Amazon SSO SDK innehåller både synkrona och asynkrona API:er för att hämta SSO-tokennyttolasten (platform identity).

Strömningsprogrammet kan välja ett av de två alternativen baserat på dess arkitektur.

##### Asynkrona API:er

* Hämta instansen `SSOEnabler` och ange `SSOEnablerCallback`:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  Detta kan göras under initieringen av direktuppspelningsprogrammet.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  Svarspaketet för lyckade SSO-token innehåller:
   * En SSO-token som en `string` med nyckeln SSOToken.

  <br/>

  Svarspaketet för SSO-tokenfel innehåller:
   * En felkod som `int` med nyckeln ErrorCode.
   * En felbeskrivning som `string` med nyckeln ErrorDescription.

  <br/>

* Hämta SSO-token:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Detta API tillhandahåller svaret via återanropsuppsättningen under initieringen.

##### Synkrona API:er

* Hämta instansen `SSOEnabler`:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Hämta SSO-token:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Detta API blockerar anropartråden och svarar med resultatpaketet. Eftersom detta är ett synkront anrop bör du inte använda det i huvudtråden.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Detta API anger timeout-värdet för det synkrona anropet. Standardvärdet för timeout är 1 minut.

#### Reservation för Amazon SSO {#fallback-amazon-sso}

Strömningsprogrammet måste hantera reservscenarier från Amazon SSO-flödet till det reguljära autentiseringsflödet.

Kontrollera att direktuppspelningsprogrammet hanterar:

* Frånvaro av Amazon-tilläggsprogram som ska köras på Amazon-enheten.
   * Strömningsprogrammet kan stöta på `ClassNotFoundException` vid körning i följande klass `com.amazon.ottssotokenlib.SSOEnabler`.

* Frånvaro av SSO-token (platform identity)-nyttolast som ska returneras av ovanstående API:er.
   * Direktuppspelningsapplikationen kan kontakta Amazon och Adobe för att undersöka saken.

### Arbetsflöde {#workflow}

Amazon SSO-token (platform identity)-nyttolasten måste finnas på alla HTTP-begäranden som görs mot Adobe Pass Authentication-slutpunkter:

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> Strömningsprogrammet kan hoppa över att skicka Amazon SSO-token (platform identity)-nyttolast för anropet `/authenticate`, vilket angavs för anropet `/regcode`.

Adobe Pass Authentication stöder följande metoder för att ta emot SSO-tokennyttolasten (platform identity), som är en enhetsomfattning eller plattformsomfångad identifierare:

* Som en rubrik med namnet: `Adobe-Subject-Token`
* Som en frågeparameter med namnet: `ast`
* Som en postparameter med namnet: `ast`

>[!IMPORTANT]
>
> Om den skickas som en frågeparameter kan hela URL:en bli mycket lång och avvisas.
>
> Om det skickas som en fråga/post-parameter måste det inkluderas när signaturen för begäran genereras.

#### Exempel

**Skicka som rubrik**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Skickar som frågeparameter**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**Skickar som en postparameter**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> Om parametervärdet `Adobe-Subject-Token` eller `ast` saknas eller är ogiltigt kommer Adobe Pass Authentication att hantera förfrågningarna utan att ta enkel inloggning i beaktande.
