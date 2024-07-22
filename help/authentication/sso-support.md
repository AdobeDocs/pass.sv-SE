---
title: Stöd för enkel inloggning
description: Stöd för enkel inloggning
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%

---

# Stöd för enkel inloggning

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview-sso-support}

Det här dokumentet beskriver de typer av enkel inloggning som stöds och drivs av Adobe Pass-autentisering på olika plattformar. Det här dokumentets omfattning är att förklara vad som stöds och vad som inte stöds, vad som omfattas av MVPD för varje SSO-metod och vad som krävs av programmerarna för att kunna utnyttja enkel inloggning på varje plattform.

När en användare har loggat in med sina MVPD-autentiseringsuppgifter, genererar Adobe Pass Authentication en säker token som representerar MVPD:s autentiseringssession och binder denna token till användarens enhet med ett enhets-ID. Adobe Pass Authentication lagrar token/enhets-ID på en server eller på enheten. Detta gör att användare kan ange sina inloggningsuppgifter mindre ofta samtidigt som transaktionerna är säkra.

>[!NOTE]
>
>Arbetsflöden för enkel inloggning ingår i Premium Workflow-paketet. Kontakta din Adobe Pass-säljare om du är intresserad av att använda den här funktionen.

## Aktuell status för enkel inloggning på olika plattformar {#current-sso-status-platforms}

| Plattform/enhet | SSO-stöd | SSO-typ | MVPD-täckning | Anteckningar |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Webb (JavaScript) | Ja | Token för delad autentisering (Adobe SSO) | Alla | Ingen SSO för olika webbläsare Följ instruktionerna i Programmer Integration Guide for JavaScript. När du följer instruktionerna aktiveras enkel inloggning som standard.  Aktivering av autentisering per begärande avbryter enkel inloggning |
| iOS | Ja | Plattforms-SSO - tokenutbyte | Beroende på Apple support - listan är här | Från och med iOS 10 har Apple och Adobe infört SSO-funktioner för deltagande programmerare och programmerare. Genom att använda den senaste Adobe iOS SDK eller genom att använda Adobe klientless REST API och implementera Apple SSO-funktionen kan du dra nytta av enkel inloggning på iOS-enheter. Mer information om SDK-implementering här och mer information om klientlös implementering här. Extra noteringar: - Om du inte vill använda Apple SSO kan du fortfarande ha en begränsad enkel inloggning mellan appar från samma leverantör (samma paket-ID) som kan dela lagringsutrymme och ett ID (IDFV) - så enkel inloggning är begränsad till appar från samma leverantör. |
| Android | Ja | Token för delad autentisering (Adobe SSO) | Alla | Om användaren inte accepterar behörighetsförfrågan WRITE_EXTERNAL_STORAGE kommer biblioteket att använda en lokal sandlådelagring. I det här fallet beror det på att det inte kommer att finnas någon enkel inloggning mellan olika program när du använder den lokala lagringen. |
| tvOS - ny Apple TV | Ja | Plattforms-SSO - tokenutbyte | Beroende på Apple support - listan är här | Från och med tvOS 10 introducerade Apple och Adobe SSO-funktioner för programmerare och programmerare som deltar. Genom att använda den senaste Adobe tvOS SDK eller genom att använda Adobe klientless REST API och implementera Apple SSO-funktionen kan du dra nytta av enkel inloggning på tvOS-enheter. Mer information om tvOS SDK: här och här och mer information om klientlös implementering här. |
| Roku | Ja | Token för delad autentisering (Adobe SSO) | Avsevärd fullständig lista över täckning kommer snart. | Roku SSO fungerar direkt med det klientlösa API:t för alla kunder som respekterar Roku-riktlinjerna, utan någon särskild implementering. SSO baseras på enhetsidentifieringsinformation som Roku skickar säkert till Adobe. |
| Amazon FireTV | Ja | Token för delad autentisering (Adobe SSO) | Avsevärd fullständig lista över täckning kommer snart. | FireTV SDK har stöd för enkel inloggning baserat på Android-funktioner. SSO på den här plattformen är bara möjligt mellan appar som använder Adobe FireTV SDK för tillfället. Mer information om nya FireTV SDK här. FireTV-appar som implementeras ovanpå klientlöst API kan dra nytta av enkel inloggning senast vid EOY 2018. |
| Xbox 360 | Nej |                                         |                                                     | Det finns inget enhets-ID som kan utnyttjas. Det finns ett program-ID, så användarna behöver inte autentisera varje gång. |
| Xbox One | Nej |                                         |                                                     | Det finns inget enhets-ID som kan utnyttjas. Det finns ett program-ID, så användarna behöver inte autentisera varje gång. |
| Windows 8/10 | Nej |                                         |                                                     | Det finns inget enhets-ID som kan utnyttjas. Det finns ett program-ID, så användarna behöver inte autentisera varje gång. |
| Samsung TV | Nej |                                         |                                                     | Det finns inget enhets-ID som kan utnyttjas. Det finns ett program-ID, så användarna behöver inte autentisera varje gång. |

### Anteckningar på Xbox 360 och Xbox One {#notes-xbox-360}

* **Xbox 360**- Xbox 360 förlitar sig på Live-tjänsten för att tillhandahålla den token som bäddar in enhets-ID:t. Live Service-lagren i appID-värdet för deviceID, vilket innebär att det endast omfattar programmet. För Xbox 360 har Microsoft försett Adobe med ett Java-bibliotek som hjälp vid tolkningen av token.

* **Xbox One** - En JSON-webbtoken utfärdas som krypteras med utgivarens certifikat/nyckel och signeras av Microsoft. Adobe extraherar deviceID från en parameter som kallas DPI (Device Pairwise ID), som skiljer sig från Xbox 360-parametern PDID (Partner Device ID). PDID finns också i Xbox One men är tänkt att ersättas med den nya parametern &quot;Device Pairwise ID&quot; (DPI).


### Inaktiverar enkel inloggning {#disable-sso}

I vissa situationer kan vissa appar eller webbplatser behöva inaktivera enkel inloggning för att uppfylla avancerade affärsärenden.

* **För JS och systemspecifika SDK:er** - supportteamet för Adobe Pass-autentisering kan inaktivera enkel inloggning för ett begärande-ID/MVPD-par. Inget arbete behövs på webbplatser eller i appar som har inbyggt stöd.  När enkel inloggning har inaktiverats av Adobe Pass Authentication-supportteamet delas inte autentiseringar som utförts med angivet RequestorId/MVPD-par med webbplatser eller appar som använder olika RequestID:n. Befintliga autentiseringar med olika begärande-ID:n är inte heller giltiga för den kombination av begärande-ID/MVPD som har inaktiverats för enkel inloggning. Tekniskt sett genomförs inaktiveringen av enkel inloggning genom att AuthN-token binds till den specifika begärande-ID/MVPD-kombinationen.
* **För klientlöst API** - Du kan inaktivera enkel inloggning i det klientlösa autentiseringsflödet genom att ange en appId-parameter som inte är tom i REST-anropen. Du kan använda valfri sträng som värde, förutsatt att strängen är unik för begärande-ID:t. Observera att programmeraren/implementeraren måste ändra webbplatsen eller appen för att lägga till den här begärarspecifika parametern för det klientlösa API:t.

>[!IMPORTANT]
>
>VIKTIG ANMÄRKNING FÖR KLIENTLESS API SSO: Vissa MVPD-program kräver att varje nätverk (begärande-ID) utför sitt eget autentiseringsflöde. För SDK-baserade flöden (iOS osv.) hanteras detta automatiskt av SDK:n. För klientlösa API:er måste dock detta hanteras av programmeraren. Vi rekommenderar varmt att programmerare inte aktiverar SSO-flöden för klientlösa API:er i det här läget och i stället använder en kombination av enhets-ID och program-ID för enhets-ID. Adobe kommer också att arbeta med att förbättra sina klientlösa API-flöden så att rätt enkel inloggning kan upprättas.

### Utloggning {#logout-sso-support}

Programmerare måste vara medvetna om att inloggningsåtgärden i samband med enkel inloggning, när den utförs i en app/på en webbplats, tar bort alla token på enheten och användaren loggas ut från alla program/platser.

Om SSO-villkoren uppfylls (oavsett om enkel inloggning är aktiverad eller inaktiverad), kommer utloggning att utföras och all autentiserings- och auktoriseringsinformation kommer att tas bort.
