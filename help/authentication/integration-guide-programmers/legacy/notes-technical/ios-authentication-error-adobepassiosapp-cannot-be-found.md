---
title: iOS-autentiseringsfel - adobepass.ios.app hittades inte
description: iOS-autentiseringsfel - adobepass.ios.app hittades inte
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# (Äldre) iOS-autentiseringsfel - adobepass.ios.app kan inte hittas {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

## Problem {#issue}

Användaren går igenom autentiseringsflödet och när de har angett sina autentiseringsuppgifter hos providern omdirigeras de antingen tillbaka till en felsida, en söksida eller någon annan anpassad sida som informerar dem om att `adobepass.ios.app` inte kunde hittas/lösas.

## Förklaring {#explanation}

I iOS används `adobepass.ios.app` som den slutliga omdirigerings-URL:en för att ange att AuthN-flödet är färdigt. Nu måste appen göra en begäran till AccessEnabler för att hämta AuthN-token och slutföra AuthN-flödet.

Problemet är att `adobepass.ios.app` inte finns och kommer att utlösa ett felmeddelande i `webView`. Äldre versioner av iOS DemoApp antog att det här felet alltid skulle utlösas i slutet av AuthN-flödet och konfigurerades för att hantera det därefter (`indidFailLoadWithError`).

**Obs!** Problemet har åtgärdats i senare versioner av DemoApp (ingår i iOS SDK-nedladdningen).

Tyvärr är detta antagande INTE korrekt. Det finns några så kallade smarta DNS- eller proxyservrar som inte bara överför det uppkomna felet, utan som i stället gör något av följande:

- Skapa en anpassad felsida
- Vidarebefordra till en söksida eller någon annan typ av kundsida eller portal.

I dessa fall kommer det svar som kommer tillbaka till iOS webView att vara ett helt giltigt svar vad gäller webView, och det utlöser INTE det fel som den gamla DemoApp-appen var beroende av.

## Lösning {#solution}

Gör INTE samma antagande som DemoApp gör. Avlyssna i stället begäran innan den körs (i `shouldStartLoadWithRequest`) och hantera den korrekt.

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

- Använd ALDRIG `adobepass.ios.app` direkt i koden. Använd i stället konstanten `ADOBEPASS_REDIRECT_URL`
- Programsatsen `return NO;` förhindrar att sidan läses in
- Se till att anropet `getAuthenticationToken` anropas en gång och bara en gång i koden. Flera anrop till `getAuthenticationToken` resulterar i odefinierade resultat.
