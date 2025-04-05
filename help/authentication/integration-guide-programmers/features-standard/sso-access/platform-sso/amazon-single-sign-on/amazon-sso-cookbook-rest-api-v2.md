---
title: Amazon SSO Cookbook (REST API V2)
description: Amazon SSO Cookbook (REST API V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Amazon SSO Cookbook (REST API V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Adobe Pass Authentication REST API V2 har stöd för Platform Single Sign-On (SSO) för slutanvändare av klientprogram som körs på FireOS.

Det här dokumentet fungerar som ett tillägg till den befintliga [REST API V2-översikten](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) som ger en högnivåvy och det dokument som beskriver hur du implementerar [enkel inloggning med plattformsidentitetsflöden](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Amazon samlad inloggning med plattformsidentitetsflöden {#cookbook}

Adobe Pass Authentication samarbetar med Amazon för att förbättra användarupplevelsen vid inloggning och för att underlätta enkel inloggning (SSO) i TV Everywhere-program för TV-prenumeranter.

### Förutsättningar {#prerequisites}

Innan du fortsätter med Amazon Single Sign-on med plattformsidentitetsflöden måste du kontrollera att följande krav uppfylls.

#### Integrera Amazon SSO SDK {#integrate-amazon-sso-sdk}

Direktuppspelningsprogrammet måste integrera [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar)-biblioteket för enkel inloggning (SSO) i sitt bygge.

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

Strömningsprogrammet måste använda Amazon SSO SDK för att erhålla SSO-tokennyttolasten (platform identity).

Amazon SSO SDK tillhandahåller både synkrona och asynkrona API:er för att hämta SSO-tokennyttolasten (platform identity).

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
   * Direktuppspelningsapplikationen kan kontakta Amazon och Adobe representanter för att undersöka saken.

### Arbetsflöde {#workflow}

Amazon SSO-tokennyttolasten (platform identity) måste finnas på alla HTTP-begäranden som görs mot Adobe Pass Authentication REST API V2-slutpunkter:

```
/api/v2/*
```

Adobe Pass Authentication REST API V2 stöder följande metoder för att ta emot SSO-tokennyttolasten (platform identity), som är en enhetsomfattning eller plattformsomfångad identifierare:

* Som en rubrik med namnet: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Mer information om rubriken `Adobe-Subject-Token` finns i dokumentationen för [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

#### Exempel

**Skicka som rubrik**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> Om rubrikvärdet `Adobe-Subject-Token` saknas eller är ogiltigt hanterar Adobe Pass Authentication förfrågningarna utan att ta enkel inloggning i beaktande.
