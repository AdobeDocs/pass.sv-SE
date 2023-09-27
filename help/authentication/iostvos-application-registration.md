---
title: iOS/tvOS - programregistrering
description: iOS/tvOS - programregistrering
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# iOS/tvOS - programregistrering {#iostvos-application-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#Intro}

Från och med version 3.0 av iOS/tvOS AccessEnabler SDK ändrar vi autentiseringsmekanismen med Adobe-servrar. I stället för att använda en offentlig nyckel och ett hemligt system för att signera beställar-ID introducerar vi konceptet med en programsatssträng som kan användas för att få en åtkomsttoken som senare används för alla anrop som SDK gör till våra servrar. Förutom en programsats behöver du också ett anpassat URL-schema för programmet.

Mer information finns i [Dynamisk klientregistrering](/help/authentication/dynamic-client-registration.md)

## Vad är en programsats? {#Soft_state}

En programsats är en JWT-token som innehåller information om programmet. Alla program ska ha en unik programsats som används av våra servrar för att identifiera programmet i Adobe system. Programsatsen måste skickas när du initierar AccessEnabler SDK och den används för att registrera programmet hos Adobe. Vid registreringen får SDK ett klient-ID och en klienthemlighet som används för att hämta en åtkomsttoken. Alla anrop som SDK gör till våra servrar kräver en giltig åtkomsttoken. SDK:n ansvarar för att registrera programmet, hämta och uppdatera åtkomsttoken.

**Obs!** En programsats är programspecifik och samma programsats kan inte användas på mer än ett program. Observera att programsatser på programmeringsnivå även följer samma regler, dvs. de kan bara användas för enstaka program - vare sig det är en kanal eller en flerkanal. Denna begränsning gäller även för anpassade scheman.

## Hur skaffar man en programvaruöversikt? {#obtain}

### Om du har tillgång till Adobe TVE Dashboard:

- Öppna webbläsaren och navigera till <https://console.auth.adobe.com>
- Navigera till `Channels` och välj kanal.
- Navigera till `Registered Applications` Tabb.
- Klicka på `Add new application`.
- Ange ett namn och en version för programmet och välj de plattformar som det ska vara tillgängligt på. iOS/tvOS i vårt fall.
- Skicka ändringarna till servern och gå sedan tillbaka till fliken Registrerade program i din kanal.
- Du bör se en lista med alla registrerade program. Klicka på   `Download` på programmet som du just har skapat. Du kan behöva vänta några minuter innan programsatsen är klar för nedladdning.
- En textfil hämtas. Använd innehållet som programsats.

Mer information finns i [Dynamisk hantering av klientregistrering](/help/authentication/dynamic-client-registration-management.md).

### Om du inte har tillgång till Adobe TV Dashboard:

Skicka en biljett till <tve-support@adobe.com>. Inkludera all nödvändig information, t.ex. kanal, programnamn, version och plattformar, och någon i vårt supportteam skapar en programsats åt dig.

## Hur använder man programsatsen? {#use}

När du har fått programsatsen måste du skicka den som en parameter i konstruktorn Access Enabler. Vi rekommenderar att programvarusatsen finns på en fjärrplats. På så sätt kan du enkelt återkalla och ändra programsatsen utan att behöva släppa en ny version av programmet.

## Skapa ett anpassat URL-schema för programmet {#generating}

### Om du har tillgång till Adobe TVE Dashboard:

- Öppna webbläsaren och navigera till <https://console.auth.adobe.com>
- Navigera till `Channels` och välj kanal.
- Navigera till `Custom Schemes` Tabb.
- Klicka på `Generate a new custom scheme`.
- Ett nytt anpassat schema skapas för programmet. Exempel: `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Skicka ändringarna till servern.

### Om du inte har tillgång till Adobe TV Dashboard:

Skicka en biljett till <tve-support@adobe.com>. Ange kanal-ID så skapar någon från vårt supportteam ett anpassat schema åt dig.

## Använda det anpassade schemat {#use_custom}

I programmets `info.plist` läggs följande kod till i filen:

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
