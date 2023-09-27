---
title: Versionsinformation om Adobe Pass Concurrency Monitoring 2.6.0
description: Versionsinformation om Adobe Pass Concurrency Monitoring 2.6.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Versionsinformation om Adobe Pass Concurrency Monitoring 2.6.0 {#cm-260}


Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:



## Releasedatum: 2016-11-10



## Nya funktioner

Den här versionen ger möjlighet att avsluta befintliga strömmar så att den aktuella strömmen kan starta (t.ex. avsluta strömmen).



**Fjärravslutning**

* På ett 409-svar i en konflikt kommer varje session som listas i fältet&quot;i konflikt&quot; i råden att innehålla ett terminateCode-attribut.
* Användaren kan uppmanas att ange en lista över sessioner som är i konflikt och få välja vilka sessioner som ska avslutas
* Fjärrsessioner kan bara stoppas genom att ett X-Terminate-begärandehuvud (med de valda avslutningskoderna som värden) skickas i ett sessionsinitieringsförsök.
* En ny typ av &quot;råd&quot; definierades för svaret från 410 Gone för att ange vilken session som har avbrutit den aktuella sessionen.


Mer information finns i den uppdaterade dokumentationen.



>[!NOTE]
>
>Sessionsdefinitionen som används för att lista aktiva sessioner uppdaterades så att den innehåller programnamn och enhetsnamn (i stället för program-ID).




## Felkorrigeringar {#bug-fixes}

Duplicerade rubriker har tagits bort i serversvaret (korrigeringen omfattar både CORS-rubriker och Date one).




## Kända fel {#known-issues}

Ej tillämpligt
