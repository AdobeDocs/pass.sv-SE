---
title: iOS-autentiseringsfel - adobepass.ios.app hittades inte
description: iOS-autentiseringsfel - adobepass.ios.app hittades inte
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# iOS-autentiseringsfel - adobepass.ios.app hittades inte {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Problem {#issue}

Användaren går igenom autentiseringsflödet och när de har angett sina autentiseringsuppgifter hos providern dirigeras de tillbaka till antingen en felsida, en söksida eller någon annan anpassad sida som informerar användaren om att `adobepass.ios.app` kunde inte hittas/lösas.

## Förklaring {#explanation}

På iOS `adobepass.ios.app` används som den slutliga omdirigerings-URL:en för att ange att AuthN-flödet är färdigt. Nu måste appen göra en begäran till AccessEnabler för att hämta AuthN-token och slutföra AuthN-flödet.

Problemet är att `adobepass.ios.app` finns inte och utlöser ett felmeddelande i `webView`. Äldre versioner av iOS DemoApp antog att det här felet alltid skulle utlösas i slutet av AuthN-flödet och konfigurerades för att hantera det därefter (`indidFailLoadWithError`).

**Obs!** Problemet har åtgärdats i senare versioner av DemoApp (ingår i iOS SDK-nedladdningen).

Tyvärr är detta antagande INTE korrekt. Det finns några så kallade smarta DNS- eller proxyservrar som inte bara överför det uppkomna felet, utan som i stället gör något av följande:

- Skapa en anpassad felsida
- Vidarebefordra till en söksida eller någon annan typ av kundsida eller portal.

I dessa fall kommer det svar som kommer tillbaka till iOS webView att vara ett helt giltigt svar vad gäller webView, och det utlöser INTE det fel som den gamla DemoApp-appen var beroende av.

## Lösning {#solution}

Gör INTE samma antagande som DemoApp gör. Avlyssna begäran innan den körs (i `shouldStartLoadWithRequest`) och hantera det på rätt sätt.

Exempel på hur begäran ska fångas upp innan den körs:

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

Några saker att notera:

- ANVÄND ALDRIG `adobepass.ios.app` direkt var som helst i koden. Använd i stället konstanten `ADOBEPASS_REDIRECT_URL`
- The `return NO;` programsatsen förhindrar att sidan läses in
- Se till att `getAuthenticationToken` anropet anropas en gång och endast en gång i koden. Flera anrop till `getAuthenticationToken` resulterar i odefinierade resultat.
