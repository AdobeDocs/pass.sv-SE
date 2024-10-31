---
title: Aktivera Android SDK enkel inloggning (SSO) i Android 10-appar
description: Aktivera Android SDK enkel inloggning (SSO) i Android 10-appar
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---

# Aktivera Android SDK enkel inloggning (SSO) i Android 10-appar {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning

Single Sign-On (SSO) mellan program som använder Adobe Pass Authentication är tillgängligt på enheter som använder Android OS via Access Enabler Android SDK. För att kunna erbjuda enkel inloggning (SSO) på Android-enheter använder Access Enabler Android SDK version 3.2.1 (senaste) och tidigare versioner en delad databasfil som sparats i en Android-lagringsimplementering, som är tillgänglig för alla program som drivs med Adobe Pass Authentication.

Men Google i den senaste Android 10-versionen medförde vissa ändringar &quot;för att ge användarna bättre kontroll över sina filer och för att begränsa fillösheten, så får appar som är inriktade på Android 10 (API-nivå 29) och senare som standard tillgång till en extern lagringsenhet, eller lagringsutrymme som omfattas. Sådana appar kan bara se sin programspecifika katalog `\[...\]`. Mer information om dessa Android 10-lagringsändringar finns i [Data- och fillagringsdokumentation för Android](https://developer.android.com/training/data-storage/files/external-scoped).

Som ett resultat av dessa ändringar kan enkel inloggning (SSO) som erbjuds av Access Enabler Android version **3.2.1 SDK (senaste)** och tidigare versioner påverkas på Android 10-enheter, vilket förklaras i nästa avsnitt.

## Beteende

Beroende på appens **[!UICONTROL target SDK level]** eller användningen av manifestattributet **android:requestLegacyExternalStorage** kommer enkel inloggning (SSO) som erbjuds av Access Enabler Android version 3.2.1 SDK (senaste) och tidigare versioner att fungera enligt följande:

- Din app har **Android 9 (API-nivå 28)** eller lägre **-\>** enkel inloggning (SSO) **fungerar**
- Din app har **Android 10** **(API-nivå 29)** som mål och **anger** värdet för **requestLegacyExternalStorage till true** i appens manifestfil **-\>** enkel inloggning (SSO) **fungerar**
- Din app har **Android 10** **(API-nivå 29)** som mål och **anger inte** värdet för **requestLegacyExternalStorage till true** i appens manifestfil **-\>** enkel inloggning (SSO) **fungerar inte**

>[!TIP]
>
> Innan aktiveringen av Adobe Pass Authentication Access Enabler Android SDK är helt kompatibelt med omfångslagring kan du tillfälligt avanmäla dig baserat på programmets mål-SDK-nivå eller attributet requestLegacyExternalStorage-manifest enligt beskrivningen i den offentliga [Android-dokumentationen](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
