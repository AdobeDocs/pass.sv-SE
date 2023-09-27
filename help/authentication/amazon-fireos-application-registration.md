---
title: Amazon FireOS - programregistrering
description: Amazon FireOS - programregistrering
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Amazon FireOS - programregistrering {#amazon-fireos-application-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Introduktion {#intro}

Från och med version 3.0 av FireOS AccessEnabler SDK ändrar vi autentiseringsmekanismen med Adobe-servrar. I stället för att använda en offentlig nyckel och ett hemligt system för att signera begärande-ID introducerar vi begreppet programsatssträng som kan användas för att få en åtkomsttoken som senare används för alla anrop som SDK gör till våra servrar. Förutom en programsats måste du också skapa en djup länk till programmet.

Mer information finns i [Dynamisk klientregistrering](/help/authentication/dynamic-client-registration.md)

## Vad är en programsats? {#what}

En programsats är en JWT-token som innehåller information om programmet. Alla program ska ha en unik programvarubeskrivning som används av våra servrar för att identifiera programmet i Adobe system. Programsatsen måste skickas när du initierar AccessEnabler SDK och den används för att registrera programmet hos Adobe. Vid registreringen får SDK ett klient-ID och en klienthemlighet som används för att hämta en åtkomsttoken. Alla anrop som SDK gör till våra servrar kräver en giltig åtkomsttoken. SDK:n ansvarar för att registrera programmet, hämta och uppdatera åtkomsttoken.

**Obs!** Programsatser är programspecifika och en enskild programsats kan inte användas för mer än en applikation. Observera att detta även gäller program som erbjuder åtkomst till flera kanaler.

## Hur skaffar man en programvaruöversikt? {#how-to}

### Om du har tillgång till Adobe TVE Dashboard:

- Öppna webbläsaren och navigera till <https://console.auth.adobe.com>
- Navigera till `Channels` och välj kanal.
- Navigera till `Registered Applications` Tabb.
- Klicka på `Add new application`.
- Ange ett namn och en version för programmet och välj de plattformar som det ska vara tillgängligt på (t.ex. Android).
- Ange ett domännamn genom att välja i en lista över domäner som redan har konfigurerats för din programmerare.
- Skicka ändringarna till servern och gå sedan tillbaka till fliken Registrerade program i Kanalen.
- Du bör se en lista med alla registrerade program. Klicka på `Download` på programmet som du just har skapat. Obs! Du kan behöva vänta några minuter innan programsatsen är klar för nedladdning.
- En textfil hämtas. Använd innehållet som programsats.

Mer information finns i [Registreringshantering för dynamisk klient](/help/authentication/dynamic-client-registration-management.md)

### Om du inte har tillgång till Adobe TV Dashboard:

Skicka en biljett till <tve-support@adobe.com>. Inkludera all nödvändig information, inklusive kanal, programnamn, version och plattformar, och någon i vårt supportteam skapar en programsats åt dig.

## Hur använder man programsatsen? {#use}

När du har fått programsatsen måste du skicka den som en parameter i konstruktorn Access Enabler. Vi rekommenderar att programvarusatsen finns på en fjärrplats. På så sätt kan du enkelt återkalla och ändra programsatsen utan att släppa en ny version av programmet.

## Så här använder du programsatsen {#use-both}

I programmets resursfil `strings.xml` lägg till följande kod:

```XML
<string name="software_statement">softwarestatement value</string>
```
