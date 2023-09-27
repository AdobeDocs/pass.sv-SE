---
title: Policybeslutspunkt
description: Policybeslutspunkt
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---


# Policybeslutspunkt {#policy-desc-pt}

## Domänmodell {#domain-model}

Den här sidan är avsedd att fungera som referens för olika användningar och implementeringar av policyer. Vi rekommenderar att du också läser [Ordlista](/help/concurrency-monitoring/cm-glossary.md) del av dokumentationen för termdefinitioner.

A **tenant** ägare **program** för vilka den vill verkställa **policyer**. **Klientapplikationer** måste konfigureras med **program-ID** (tillhandahålls av Adobe).

Klienten associerar sedan varje program med en eller flera profiler, antingen skapade av honom eller skapade och delade av andra. Profiler kan länkas mellan flera innehavare.

The **ämnesaktivitet** består av alla strömmar (oavsett program) som rapporteras till övervakning av samtidig användning för ett visst ämne.

När en ström ska auktoriseras för ett visst ämne, kontrollerar systemet först alla principer som definierats för programmet som skapade strömmen.

För varje tillämplig policy måste vi sedan samla in alla **relevant verksamhet** som skickas till regeln. The **relevant verksamhet** för en princip P kommer endast att innehålla strömmen S om den uppfyller följande villkor:

**Strömmen S startas av ett program som innehåller principen P bland sina principer.**

![Strömmen S startas av ett program som innehåller principen P bland sina principer.](assets/pdp-domain-model.png)

## Användningsexempel för torr körning {#dry-run-use-cases}

Genomgången nedan avser att validera modellen mot vissa användningsfall. Vi gör det gradvis genom att börja med en grundläggande konfiguration och lägga till komplexitet på olika sätt.

### 1. En klient. Ett program. En policy. En ström {#onetenant-oneapp-onepolicy-onestream}

Vi börjar med en enda klient, med ett enda program och en enda policy. Låt oss anta att principen anger att det kan finnas högst en aktiv ström för alla användare (den senaste strömmen får spelas upp).

När en direktuppspelning har startats består aktiviteten endast av den strömmen och den får spelas upp.

![En hyresgäst. Ett program. En policy. En ström](assets/onetenant-app-policy-stream.png)


### 2. En klient. Ett program. En policy. Två strömmar. {#onetenant-oneapp-onepolicy-twostreams}

När en andra ström startas (av samma ämne som använder samma program), består den aktivitet som används för valideringen av båda **s1** och **s2**.

Gränsen har överskridits eftersom principen anger att endast en ström får spelas upp, så att vi endast tillåter den senaste strömmen (**s2**) att spela.

![En hyresgäst. Ett program. En policy. Två strömmar.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>Diagrammen representerar systemvyn för användaraktiviteten. Vid försök till initiering av strömmar inkluderas åtkomstbeslutet i svaret. För aktiva strömmar kommer beslutet att returneras vid pulsslagssvar.

### 3. Två hyresgäster. Två program. En policy. Två strömmar. {#twotenant-twoapp-onepolicy-twostreams}

Låt oss nu anta att en ny hyresgäst vill tillämpa samma policy i sina program:

![Två hyresgäster. Två program. En policy. Två strömmar.](assets/onepolicy-twotenant-app-stream.png)

På grund av att de två hyresgästerna är kopplade till samma policy är den situation som beskrivs i punkt 2 tillämplig här och **s3** kan spelas upp som den senaste strömmen.

### 4. Två hyresgäster. Tre program. Två policyer. Två strömmar. {#twotenants-threeapps-twopolicies-twostreams}

Låt oss nu anta att den andra klienten distribuerar ett nytt program och vill definiera en ny princip som ska delas mellan **app2** och **app3**.

![Två hyresgäster. Tre program. Två policyer. Två strömmar.](assets/twotenant-policies-streams-threeapps.png)

För närvarande strömmar de aktiva **s3** och **s4** båda är tillåtna. För **s3**, när princip **P1** utvärderas, systemet kommer endast att räkna **s3** as **relevant verksamhet** (**s4** är inte på något sätt relaterat till politiken **P1**) så att det inte blir något brott.

Policy **P2** används för båda strömmarna och omfattar båda **s3** och **s4** som relevant verksamhet. Eftersom den här aktiviteten ligger inom gränserna för två strömmar tillåts båda strömmarna.

### 5. Två hyresgäster. Tre program. Två policyer. Tre strömmar. {#twotenants-threeapps-twopolicies-threestreams}

Anta nu att ett nytt försök att initiera strömmen utförs med **app2**:

![Två hyresgäster. Tre program. Två policyer. Tre strömmar.](assets/twotenants-policies-threeapps-streams.png)

**s5** får starta med **P1** (som tillåter att nyare strömmar tar över) men det nekas av **P2** så det inte börjar.

Samma sak händer om en direktuppspelningsinit görs med app3: samma P2-princip nekar åtkomst för den.

![](assets/stream-init-attempted-app3.png)

Nu ska vi se vad som händer om användaren försöker skapa en ny ström med app1:

![](assets/new-stream-with-app1.png)

Programprogrammet app1 är inte på något sätt relaterat till principen **P2** så att profilen endast tillämpas **P1**: som tillåter att den nya strömmen startar och nekar den äldre strömmen (**s3** i detta fall).

