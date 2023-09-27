---
title: Åtkomstaktivera Android SDK enkel inloggning (SSO) i Android 10-program
description: Åtkomstaktivera Android SDK enkel inloggning (SSO) i Android 10-program
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Åtkomstaktivera Android SDK enkel inloggning (SSO) i Android 10-program {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning

Single Sign-On (SSO) mellan program som använder Adobe Pass Authentication är tillgängligt på enheter som använder Android OS via Access Enabler Android SDK. För att kunna erbjuda enkel inloggning (SSO) på Android-enheter använder Access Enabler Android SDK version 3.2.1 (senaste) och tidigare versioner en delad databasfil som sparats i en Android-lagringsimplementering, som är tillgänglig för alla program som använder Adobe Pass Authentication.

Men Google i den senaste Android 10-utgåvan medförde vissa ändringar &quot;för att ge användarna bättre kontroll över sina filer och för att begränsa filtrassel, så får appar med Android 10 (API-nivå 29) och senare som standard tillgång till en extern lagringsenhet, eller lagringsutrymme med omfång. Sådana program kan bara se sin programspecifika katalog `\[...\]`&quot;. Mer information om de här lagringsändringarna för Android 10 finns i [Dokumentation för data och fillagring för Android](https://developer.android.com/training/data-storage/files/external-scoped).

Som ett resultat av dessa ändringar är den enkel inloggning (SSO) som finns i Android-versionen med Access Enabler **3.2.1 SDK (senaste)** och tidigare versioner kan påverkas på Android 10-enheter, vilket förklaras i nästa avsnitt.

Se [Roku SSO-översikt](/help/authentication/roku-sso-overview.md).

## Beteende

Beroende på appens **mål-SDK-nivå** eller användningen av **android:requestLegacyExternalStorage** manifest-attributet för enkel inloggning (SSO) som erbjuds av Access Enabler Android version 3.2.1 SDK (senaste) och tidigare versioner fungerar för närvarande enligt följande:

- Dina appmål **Android 9 (API-nivå 28)** eller lägre **-\>** enkel inloggning (SSO) **kommer att arbeta**
- Dina appmål **Android 10** **(API-nivå 29)** och gör **set** värdet av **requestLegacyExternalStorage till true** i appens manifestfil **-\>** enkel inloggning (SSO) **kommer att arbeta**
- Dina appmål **Android 10** **(API-nivå 29)** och gör **inte inställd** värdet av **requestLegacyExternalStorage till true** i appens manifestfil **-\>** enkel inloggning (SSO) **fungerar inte**


>[!TIP]
>
> Innan aktivering av Android SDK för Adobe Pass-autentiseringsåtkomst är helt kompatibelt med omfångslagring kan du tillfälligt avanmäla dig baserat på programmets mål-SDK-nivå eller attributet requestLegacyExternalStorage-manifest enligt beskrivningen i det offentliga [Android-dokumentation](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
