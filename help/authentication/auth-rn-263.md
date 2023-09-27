---
title: Versionsinformation om Adobe Pass Authentication 2.63
description: Versionsinformation om Adobe Pass Authentication 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Versionsinformation om Adobe Pass Authentication 2.63 {#pt-authn-263-rn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Den här sidan beskriver nya funktioner, ändringar och kända fel i den här versionen:

## Klienter på serversidan och webben {#server-side-web-clients-263}

* [Byggnummer](#build-number)
* [Nya funktioner](#new-features)

### Byggnummer {#build-number-263}

Adobe Pass-autentisering: adobe-pass-**2.63**
Utgivningsdatum: **09/20/2022 - 09/22/2022**

### Nya funktioner {#new-features-263}

#### Förbättra plattformsidentifieringsmekanismen {#pf-identification-mech}

* Från och med den här versionen har vi förbättrat den mekanism som används för att identifiera en enhet och den kommer inte längre att vara beroende av implementeringen på klientsidan. Detta ger en exaktare detaljrikedom vid tillämpning av affärsregler på plattformsnivå och en bättre förståelse för trafikvärden i ESM-rapporter.

* En ny ESM-release kommer snart, med nya och förbättrade rapporter som visar de plattformsrelaterade fälten.

* Mer information om de planerade ändringarna får du av din Adobe TAM.

#### Självnedbrytning av MVPD {#mvpd-self-degradation}

Den här funktionen ger distributörer av videoprogrammeringstjänster möjlighet att tillfälligt kringgå sina egna autentiserings- och auktoriseringsslutpunkter för högtrafikscenarier när belastningen på dessa respektive slutpunkter blir för hög.


#### Lägg till proxyID i huvudet för auktoriseringsanrop {#add-proxied-id}

Den här funktionen lägger till ID:t för en MVPD som proxyeras av Synacor i auktoriseringssamtalets huvud. Det gör att Synacor kan ställa in affärsregler för varje enskild proxiderad (t.ex. dirigering till olika domäner per proxiderat MVPD).


#### TVE Dashboard {#tve-dashboard}

I den här versionen har ett problem korrigerats där authN eller authZ TTL inställt på MVPD-nivå inte beräknades korrekt i konfigurationsrapporterna.


#### JavaScript SDK 4.6.0 {#js-sdk}

* Borttagen användning av `eval` så att SDK blir kompatibelt med Content Security Policy.
* Korrigerade ett problem som förhindrade autentiseringsflödet från att slutföras när webbläsarens lokala lagring rensades explicit av ett partnerprogram.
