---
title: Android-programregistrering
description: Android-programregistrering
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Android-programregistrering {#android-application-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#intro}

Från och med version 3.0 av Android AccessEnabler SDK ändrar vi autentiseringsmekanismen med Adobe-servrar. I stället för att använda en offentlig nyckel och ett hemligt system för att signera beställar-ID introducerar vi konceptet med en programsatssträng som kan användas för att få en åtkomsttoken som senare används för alla anrop som SDK gör till våra servrar. Förutom en programsats måste du också skapa en länk till programmet.

Mer information finns i [Dynamisk klientregistrering](/help/authentication/dynamic-client-registration.md)

## Vad är en programsats? {#what}

En programsats är en JWT-token som innehåller information om programmet. Alla program ska ha en unik programsats som används av våra servrar för att identifiera programmet i Adobe system. Programsatsen måste skickas när du initierar AccessEnabler SDK och den används för att registrera programmet hos Adobe. Vid registreringen får SDK ett klient-ID och en klienthemlighet som används för att hämta en åtkomsttoken. Alla anrop som SDK gör till våra servrar kräver en giltig åtkomsttoken. SDK:n ansvarar för att registrera programmet, hämta och uppdatera åtkomsttoken.

>[!NOTE]
>
>Programsatser är programspecifika och en enskild programsats kan inte användas för mer än en applikation. Observera att programsatser på programmeringsnivå har samma begränsning. De kan bara användas för enstaka program, vare sig det är en kanal eller en flerkanal.

## Hur skaffar man en programvaruöversikt? {#how-to-get-ss}

### Om du har tillgång till Adobe TVE Dashboard:

* Öppna webbläsaren och navigera till [Adobe Pass TVE Dashboard](https://console.auth.adobe.com).
* Navigera till `Channels` och välj kanal.
* Navigera till `Registered Applications` Tabb.
* Klicka på `Add new application`.
* Ange ett namn och en version för programmet och välj de plattformar som det ska vara tillgängligt på. Android, i vårt fall.
* Ange ett domännamn genom att välja i en lista över domäner som redan har konfigurerats för din programmerare.
* Skicka ändringarna till servern och gå sedan tillbaka till fliken Registrerade program i Kanalen.
* Du bör se en lista med alla registrerade program. Välj **Ladda ned** på programmet som du just har skapat. Du kan behöva vänta några minuter innan programsatsen är klar för nedladdning.
* En textfil hämtas. Använd innehållet som programsats.

Mer information finns i [Dynamisk hantering av klientregistrering](/help/authentication/dynamic-client-registration-management.md)

### Om du inte har tillgång till Adobe TV Dashboard:

Skicka en biljett till `tve-support@adobe.com`. Inkludera all nödvändig information, t.ex. kanal, programnamn, version och plattformar, så skapar någon i vårt supportteam en programvarubeskrivning åt dig.

## Hur använder man programsatsen? {#how-to-use-ss}

När du har fått programsatsen måste du skicka den som en parameter i konstruktorn Access Enabler. Vi rekommenderar att programvarusatsen finns på en fjärrplats. På så sätt kan du enkelt återkalla och ändra programsatsen utan att behöva släppa en ny version av programmet.

## Skapa och använd en länk för programmet {#create}

På Android ska du som värde för djuplänken använda det omvända domännamnet som du valde när du skapade programsatsen

Skapad djuplänk ska ha ett unikt värde på Android-enheten. När flera program använder samma värde för djuplänk kommer autentiserings- och utloggningsflödena att störa.

## Så här använder du programsatsen och länken {#use-both}

I programmets resursfil `strings.xml` lägg till följande kod:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
