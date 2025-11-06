---
title: Amazon FireOS - programregistrering
description: Amazon FireOS - programregistrering
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# (Äldre) Amazon FireOS-programregistrering {#amazon-fireos-application-registration}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

</br>

## Introduktion {#intro}

Från och med version 3.0 av FireOS AccessEnabler SDK ändrar vi autentiseringsmekanismen med Adobe servrar. Istället för att använda en offentlig nyckel och ett hemligt system för att signera beställar-ID introducerar vi konceptet med en programsatssträng som kan användas för att få en åtkomsttoken som senare används för alla anrop som SDK gör till våra servrar. Förutom en programsats måste du också skapa en djup länk till programmet.

Mer information finns i [Översikt över registrering av dynamisk klient](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Vad är en programsats? {#what}

En programsats är en JWT-token som innehåller information om programmet. Alla program ska ha en unik programvarubeskrivning som används av våra servrar för att identifiera programmet i Adobe system. Programsatsen måste skickas när du initierar AccessEnabler SDK och den kommer att användas för att registrera programmet hos Adobe. Vid registreringen får SDK ett klient-ID och en klienthemlighet som används för att erhålla en åtkomsttoken. Alla samtal som SDK gör till våra servrar kräver en giltig åtkomsttoken. SDK ansvarar för att registrera programmet, hämta och uppdatera åtkomsttoken.

**Obs!** Programsatser är programspecifika och en enskild programsats kan inte användas för mer än ett program. Observera att detta även gäller program som erbjuder åtkomst till flera kanaler.

## Hur skaffar man en programvaruöversikt? {#how-to}

### Om du har tillgång till Adobe TV E Dashboard:

1. Öppna webbläsaren och gå till `https://experience.adobe.com/#/pass/authentication`.

1. Navigera till avsnittet **[!UICONTROL Channels]** och markera sedan kanalen.

1. Navigera till fliken **[!UICONTROL Registered Applications]**.

1. Klicka på **[!UICONTROL Add new application]**.

1. Ange ett namn och en version för programmet och välj de plattformar som det ska vara tillgängligt på (till exempel Android).

1. Ange en **[!UICONTROL Domain Name]** genom att välja i en lista över domäner som redan har konfigurerats för din programmerare.

1. Skicka ändringarna till servern och gå sedan tillbaka till kanalens **[!UICONTROL Registered Applications]**-flik.

   Du bör se en lista med alla registrerade program.

1. Klicka på **[!UICONTROL Download]** i programmet som du just har skapat.

   Du kan behöva vänta några minuter innan programsatsen är klar för nedladdning.

   En textfil hämtas. Använd innehållet som programsats.

Mer information finns i [Registreringshantering för dynamisk klient](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Om du inte har tillgång till Adobe TVE Dashboard:

Skicka en biljett till [tve-support@adobe.com](mailto:tve-support@adobe.com). Inkludera all nödvändig information, inklusive kanal, programnamn, version och plattformar, och någon i vårt supportteam skapar en programsats åt dig.

## Så här använder du programsatsen {#use}

När du har fått programsatsen måste du skicka den som en parameter i konstruktorn Access Enabler. Adobe rekommenderar att programvarusatsen finns på en fjärrplats. På så sätt kan du enkelt återkalla och ändra programsatsen utan att släppa en ny version av programmet.

## Så här använder du programsatsen {#use-both}

Lägg till följande kod i programmets resursfil `strings.xml`:

```XML
<string name="software_statement">softwarestatement value</string>
```
