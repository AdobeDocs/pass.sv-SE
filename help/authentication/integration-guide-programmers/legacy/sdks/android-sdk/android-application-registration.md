---
title: Registrering av Android
description: Registrering av Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Registrering av Android {#android-application-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

Från och med version 3.0 av Android AccessEnabler SDK ändrar vi autentiseringsmekanismen med Adobe-servrar. I stället för att använda en offentlig nyckel och ett hemligt system för att signera begärande-ID introducerar vi begreppet programsatssträng som kan användas för att få en åtkomsttoken som senare används för alla anrop som SDK gör till våra servrar. Förutom en programsats måste du också skapa en djup länk till programmet.

Mer information finns i [Översikt över registrering av dynamisk klient](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Vad är en programsats? {#what}

En programsats är en JWT-token som innehåller information om programmet. Alla program ska ha en unik programvarusats som används av våra servrar för att identifiera programmet i Adobe-systemet.

Programsatsen måste skickas när du initierar SDK:n `AccessEnabler`. Den används för att registrera programmet hos Adobe. Vid registrering får SDK ett klient-ID och en klienthemlighet, som används för att hämta en åtkomsttoken. Alla anrop som SDK gör till Adobe-servrar kräver en giltig åtkomsttoken. SDK:n ansvarar för att registrera programmet, hämta och uppdatera åtkomsttoken.

>[!NOTE]
>
>Programsatser är programspecifika och en enskild programsats kan inte användas för mer än en applikation. Observera att programsatser på programmeringsnivå har samma begränsning. De kan bara användas för enstaka program, vare sig det är en kanal eller en flerkanal.

## Så här får du en programvarubeskrivning {#how-to-get-ss}

Här beskrivs hur du kan få en programsats.

### Om du har tillgång till Adobe TVE Dashboard

1. Öppna webbläsaren och gå till [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication).

1. Navigera till avsnittet **[!UICONTROL Channels]** och markera sedan kanalen.

1. Navigera till fliken **[!UICONTROL Registered Applications]**.

1. Klicka på **[!UICONTROL Add new application]**.

1. Ge programmet ett namn och ange en version.

1. Välj de plattformar som programmet ska vara tillgängligt på (Android i det här fallet).

1. Ange en **[!UICONTROL Domain Name]** genom att välja i en lista över domäner som redan har konfigurerats för din programmerare.

1. Skicka dina ändringar till servern och gå sedan tillbaka till fliken **[!UICONTROL Registered Applications]** i din kanal.

   Du bör se en lista med alla registrerade program. Välj **[!UICONTROL Download]** i programmet som du skapade. Du kan behöva vänta några minuter innan programsatsen är klar för nedladdning.

   En textfil hämtas. Använd innehållet som programsats.

Mer information finns i [Registreringshantering för dynamisk klient](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Om du inte har tillgång till Adobe TV Dashboard

Skicka en biljett till `tve-support@adobe.com`. Inkludera nödvändig information som kanal, programnamn, version och plattformar. Någon från vårt supportteam kommer att skapa en programsats åt dig.

## Så här använder du programsatsen {#how-to-use-ss}

När du har fått programsatsen måste du skicka den som en parameter i konstruktorn Access Enabler. Vi rekommenderar att programvarusatsen finns på en fjärrplats. På så sätt kan du enkelt återkalla och ändra programsatsen utan att släppa en ny version av programmet.

## Skapa och använd en länk för programmet {#create}

I Android ska du som värde för djuplänken använda det omvända domännamnet som du valde när du skapade programsatsen

Skapad djuplänk ska ha ett unikt värde på Android-enheten. När flera program använder samma värde för djuplänk kommer autentiserings- och utloggningsflödena att störa.

## Så här använder du programsatsen och länken {#use-both}

Lägg till följande kod i programmets resursfil `strings.xml`:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
