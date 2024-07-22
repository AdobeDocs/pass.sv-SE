---
title: Klientlöst API-flöde i frånvaro av enhets-ID
description: Klientlöst API-flöde i frånvaro av enhets-ID
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# Klientlöst API-flöde i frånvaro av enhets-ID {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>


## Problem

Alla smarta enhetsappar kan inte tillhandahålla ett unikt enhets-ID.  Eftersom deviceId är en obligatorisk parameter returnerar tjänsten ett 400-fel om den inte skickas.


## Tillfällig lösning/Tillfällig lösning

För klienter utan enhets-ID:

1. Ring registreringskodstjänsten första gången med `deviceId=dummy`
1. Extrahera UUID från svaret. UUID är tillgängligt i elementet&quot;id&quot; i registreringskodens svar (XML- och JSON-svarsformat).
1. Ring registreringstjänsten en andra gång. Den här gången, skicka `deviceId=<uuid obtained in step #2>`
1. Visa registreringskoden som fås i steg 3 i konsolens användargränssnitt


När dessa steg är klara använder Adobe Pass Authentication UUID som enhets-ID. Lagra detta enhets-ID (UUID) i enhetens lokala lagring. Om användaren skapar en ny registreringskod ska du köra steg 1 till 4 igen och sedan ersätta det tidigare lagrade enhets-ID:t (UUID) med det nya.



## Permanent lösning

Adobe ändrar detta i en framtida version genom att göra `deviceId` till en valfri nyttolast när reg.koden skapas och använda UUID som tokennyckel i stället för `deviceId` när `deviceId` inte finns.

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
