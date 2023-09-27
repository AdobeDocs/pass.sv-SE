---
title: Användningsexempel
description: Användningsfall i övervakning av samtidig användning.
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---


# Användningsexempel {#use-cases}

Det huvudsakliga användningsfallet för tjänsten Stream Counting Service är att räkna antalet samtidiga videoströmmar som en användare bevakar och fatta ett beslut om dess samtidiga användning för samma konto-ID.

För att kunna övervaka abonnentens användning finns det ett behov av en centraliserad tjänst som kan samla användaraktiviteten oavsett om den sker på programmerarens webbplats eller i applikationen, på MVPD:s innehållsportal eller på en syndikerad egenskap.

De huvudsakliga användningsområdena som stöds av denna centraliserade tjänst bör vara:

1. Så snart en prenumerant börjar titta på en video kan programmet **initiera en direktuppspelningssession** och börja **rapporteringsaktivitet** data.
1. I samma centrala tjänst får en annan instans ***CM-beslut*** - om programmet har en eller flera registrerade policyer i CM-tjänsten, kommer tjänsten att svara med åtkomstbeslut baserat på den aktuella aktiviteten.


## Skapa en session {#create-session}

Detta API-anrop gör att klienten kan skapa en ny CM-session när användaren trycker på uppspelningsknappen för att se visst innehåll. Serversvaret innehåller den nya strömmens URL (som innehåller strömmens ID) för att hålla den aktiv och den tid då strömmen kommer att timeout. Klientprogrammet förväntas rapportera aktivitet via hjärtslag. Initieringsanropet för sessionen måste innehålla metadata i form av nyckel-/värdepar som skickas som formulärdata (eller frågesträngsparametrar). Svaret kommer dessutom att innehålla en flagga som anger om uppspelningen är&quot;policykompatibel&quot;. Om så inte är fallet tillåts inte uppspelningen.

## Rapporteringsaktivitet {#reporting-activity}

När en session har skapats måste programmet skicka hjärtslag regelbundet för att den strömmen ska förbli aktiv. Vi rekommenderar också att klientprogrammet stoppar strömmen när användaren avbryter uppspelningen, så att strömmen inte räknas som aktiv förrän timeout inträffar.

Hjärtslagsanropets svar kan tillåta klientprogrammet att fortsätta videouppspelningen (när det är principkompatibelt) eller instruera det att stoppa videouppspelningen. Om videoströmmen inte är kompatibel måste klientprogrammet stoppa den. Svaret ger information för att klientprogrammet ska kunna visa ett felmeddelande och/eller tillgängliga åtgärder för att användaren ska kunna fortsätta uppspelningen.
