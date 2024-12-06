---
title: Amazon fireTV SSO - Programmerarens startguide
description: Amazon fireTV SSO - Programmerarens startguide
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Amazon fireTV SSO - Programmerarens startguide {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Introduktion {#intro}

I det här dokumentet beskrivs den information som behövs för att integrera den nya **Adobe Pass Authentication&#39;s fireTV SDK** i ditt FireTV-program. Denna nya SDK utnyttjar integreringen på operativsystemnivå på Amazon FireTV-plattformen och erbjuder därför stöd för **enkel inloggning**. För att du ska kunna dra nytta av enkel inloggning krävs en liten insats från din sida för att migrera ditt program från klientlöst API till nya FireTV SDK. Det finns några förändringar i autentiseringsflödena som beskrivs nedan.

## Arkitektur på hög nivå och integration på operativsystemnivå {#high}

För att uppnå Single Sign On mellan TV Everywhere-program på Amazon FireTV-plattformen och för att förbättra den övergripande upplevelsen på den här plattformen beslutade vi oss för att integrera vår SDK på FireTV OS-nivå. Programmerarna måste kompilera mot ett stub-bibliotek från Adobe. Den faktiska funktionaliteten tillhandahålls av Adobe bibliotek som finns i Amazon FireTV OS.

Tills Amazon har en FireTV-simulator som innehåller vårt bibliotek på operativsystemsnivå är utvecklingen bara möjlig med riktiga FireTV-enheter.

## Fördelar {#bene}

* Single Sign On mellan alla Adobe-baserade TV Everywhere-program på Amazon FireTV-plattformen med alla integrerade MVPD-program.
* Möjlighet att utnyttja HBA (med stöd för MVPD).
* Möjlighet att använda den senaste FireTV SDK utan att behöva uppdatera programmen varje gång en ny SDK-version släpps.
* Alla TVE-appar kan utnyttja det delade systembiblioteket genom att eliminera behovet av en lokal kopia av AccessEnabler-biblioteket. Detta säkerställer också att alla program använder samma SDK-version.
* Autentisering med en skärm - inget behov av registreringskod och arbetsflöden för andra skärmar.

## Migrering från klientlös API-baserad app till FireTV SDK-baserad app {#migra1}

Om du vill migrera från klientlöst API till fireTV SDK måste du ta bort kodbasen som är relaterad till klientlöst API och integrera den nya FireTV SDK.

Jämfört med den klientlösa API-baserade appen, med den nya FireTV SDK, så är autentiseringen att gå till första skärmen, behövs ingen andra skärmautentisering längre.

Detta kräver att programmerare lägger till en MVPD-väljare i sina appar så att användarna kan välja sin TV-leverantör direkt på FireTV-enheten. När användaren har valt MVPD visas inloggningssidan för MVPD på FireTV-enheten.

Trådramar för användarflödena som beskriver vanliga scenarier, HBA och SSO på fireTV finns på [Amazon Fire TV - användarflöde för MVPD-inloggning](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migrering från Android SDK-baserad app till FireTV SDK-baserad app {#migra2}

Denna nya FireTV SDK liknar vår befintliga Android SDK och den aktuella dokumentationen som vi har för **integreringen av Android SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->kan användas tills vi har FireTV SDK-dokumenten klara. Om du redan har Android-program som använder Android SDK bör integreringen av fireTV SDK i din FireTV-applikation vara enkel.

Jämfört med befintlig Android SDK är autentiseringsprocessen på fireTV SDK enklare att utveckla eftersom uppgifterna att hantera/presentera inloggningssidan för MVPD och hämta AuthN-token kommer att utföras internt av AccessEnabler-biblioteket.

## Vanliga frågor {#faq}

1. Hur fungerar **enkel inloggning**?

   * SSO fungerar i alla programmerarprogram med Adobe Pass Authentication som använder den nya FireTV SDK på samma Amazon FireTV-enhet
   * SSO mellan programmeringsappar som implementeras på klientlöst REST API och appar som implementeras på FireTV SDK **stöds INTE**

1. Vad är MVPD-täckningen för FireTV SSO?

   * **Alla MVPD-filer** som är integrerade med Adobe Pass Authentication stöds tekniskt av SSO på FireTV SDK.

1. Vilka andra **arbetsflödesändringar** ska programmerare känna till förutom att använda den nya SDK:n?

   * Programmerarna måste implementera en MVPD-väljare för FireTV-plattformen.

1. Kommer autentiseringen **TTL:er** att ändras?

   * Beteendet för autentiserings-TTL:er ändras inte.
   * Den första giltiga autentiseringstoken används för att utföra enkel inloggning och i detta fall kommer alla andra program som autentiseras via enkel inloggning att använda samma TTL tills den upphör att gälla. När du navigerar från ett program till ett annat kommer det andra programmet att dela TTL-värdet för det första programmet som autentiseras.

1. Hur fungerar **API:t för nedbrytning**?

   * Inga ändringar behövs för API:t för nedgradering. Användarupplevelsen blir densamma som på Android-enheter.

1. Hur **TempPass**-flöden påverkas?

   * TempPass-flöden är en enda skärm och fungerar som andra inbyggda enheter.

1. Fungerar andra Adobe-funktioner som tidigare?

   * Alla Adobe Pass-autentiseringsfunktioner fungerar på FireTV som på Android-enheter.
