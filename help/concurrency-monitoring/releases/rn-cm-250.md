---
title: Versionsinformation om Adobe Pass Concurrency Monitoring 2.5.0
description: Versionsinformation om Adobe Pass Concurrency Monitoring 2.5.0
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Concurrency Monitoring 2.5.0 {#cm-250}

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

Releasedatum: 2016-04-21

## Nya funktioner {#new-features}

API:t för övervakning av samtidig användning (Concurrency Monitoring) v2.0 är nu tillgängligt i produktionen.

V2-versionen förenar hjärtslag och frågeanrop och förenklar implementeringen avsevärt när båda dessa API:er används samtidigt.



### Sessionens livscykel {#session-lifecycle}

* Skrivskyddade API:er för att skapa, hålla liv och avsluta en session - d.v.s. inga fler frågor, beslutet returneras både för anrop till sessionsinit och pulsslag
* Hjärttaktsintervallet styrs nu av servern via Expires-huvuden som angetts för både session init och keep-alive-anrop. På så sätt kan serversidan konfigurera hjärttaktintervall och ett mindre hårdkodat värde i programmen.
* Den lyckliga sökvägen (kompatibelt beteende för både användaren och appen) är&quot;sparad&quot; med 2XX-statuskoder

### Felhantering {#error-handling}

* Neka beslut, saknade metadata eller programfel flaggas som 4XX-svar (Konflikt, Felaktig begäran, Obehörig, Gone osv.)

* När det passar dig omfattar svaret följande:

   * tillhörande råd - detaljerade förklaringar av felet, som ska tillfrågas användaren.

   * skyldigheter - obligatoriska åtgärder som programmet måste vidta (t.ex. uppdatera metadata, logga ut från Adobe Pass).

### Metadata {#metadata}

* Standard i stället för anpassade metadata - detta gör att API kan identifiera om felaktiga metadata skickas eller om metadata som krävs av regler saknas.
* Attribut som krävs för varje program tillhandahålls nu av ett API-anrop och bör cachas av klienter.
Anteckningar

>[!NOTE]
>
>V1-versionen är fortfarande tillgänglig. Om kunderna behöver använda frågor utanför hjärtslagen kan du anropa V1-versionen.




### Felkorrigeringar {#bug-fixes}

Ej tillämpligt

### Kända fel {#known-issues}

Ej tillämpligt
