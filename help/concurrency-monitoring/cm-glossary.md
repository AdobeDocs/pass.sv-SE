---
title: Ordlista
description: Ordlista för termer i övervakning av samtidig användning
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# Ordlista {#glossary}

## Konto-ID {#accid-defn}

* MVPD-konto för en prenumerant, som vanligtvis motsvarar det faktiska faktureringskontot. Det här kontot måste kunna identifieras av MVPD i sitt eget system.

## Åtgärd {#action-defn}

* Den typ av åtkomst som ämnet begär; möjliga värden för CM är ***starta*** eller ***continue*** en strömningssession.

## Aktiv ström {#active-stream-defn}

* En ström som har fått minst en händelse (pulsslag) under de senaste 90 sekunderna.

* ***Obs!*** Om den sista händelsen i strömmen är av typen stop (`?event=stop`) kommer den inte att räknas. Detta är en optimering som gör att en spelare uttryckligen kan stänga en ström så att den inte längre betraktas som aktiv.

## Program {#application-defn}

* Utvecklad av klienten för åtkomst till videoinnehåll
* Fatta och verkställa beslut om innehållsåtkomst baserat på information som tillhandahålls av Concurrency Monitoring Service (detta gäller i [Policyinformationspunkt](/help/concurrency-monitoring/policy-info-pt-versionone.md) case)
* Kommer att ha en unik **program-ID** tillhandahålls av Adobe.

## Övervakningstjänst för samtidig valuta {#cm-service-defn}

* Fungerar som ett övervakningssystem för abonnenterna som stöder de sidoskydd och programmerare som arbetar med att genomföra policyer för olika tillämpningar.
* Tar emot pulsslag som indikerar strömaktivitet.
* Fungerar som en _Policybeslutspunkt_ genom att utvärdera begäranden om auktorisering baserat på användaraktivitet och ge ett svar om att tillåta/neka.
* Fungerar som en _Policyinformationspunkt_ genom att rapportera antalet aktiva strömmar (och ytterligare strömmetadata) för en prenumerant.

## Miljö {#env-defn}

* Ytterligare information som är relevant för begäran, som konfigurationsinställningar eller systemtid.

## MVPD {#mvpd-defn}

* Distributör av flerkanalsvideo-programmering.
* Fungerar som autentiserings- och auktoriseringsleverantör, men kan även vara tjänste- eller innehållsleverantör.

## Policy {#policy-defn}

* Det centrala begreppet för åtkomstkontroll i CM definieras som ett mål och en eller flera regler grupperade under ett unikt namn.

## Policyadministrationspunkt (PAP) {#policy-admin-pt-defn}

* Punkt som hanterar åtkomstauktoriseringsprinciper. Detta kommer inte att dokumenteras här, men så småningom kommer vi att tillhandahålla en självbetjäningskonsol där kunderna kan hantera sina åtkomstregler.

## Policybeslutsplats (PDP) {#policy-decn-pt-defn}

* Punkt som utvärderar åtkomstbegäranden mot auktoriseringsprinciper innan åtkomstbeslut utfärdas.

## PEP (Policy Enforcement Point) {#policy-ef-pt-defn}

* Den punkt som avlyssnar användarens begäran om åtkomst till en resurs gör en beslutsbegäran till PDP och verkställer det beslutet på begäran. Det här är för närvarande klientprogrammet och det finns ingen plan för att överföra rollen till övervakning av samtidig användning.

## Policyinformationspunkt (PIP) {#policy-info-pt-defn}

* En källa med attributvärden. Övervakning av samtidig användning fungerar som en informationspunkt genom att tillhandahålla:
   * direktuppspelningsmetadata.
   * aktivitetsmått för samtidiga strömmar.

## Programmer {#programmer-defn}

* Fungerar som en tjänst- och innehållsleverantör.
* Förlitar sig på det distribuerade klientprogrammet som integreras med tjänsten för övervakning av samtidig användning (Concurrency Monitoring) för att tillämpa de definierade säkerhetsprinciperna baserat på ovannämnda tjänstdata.
* Behovet av stöd för MVPD vid insamling av abonnentaktivitet och genomförande av begränsningsreglerna när det gäller deras egenskaper.
* Som en separat regel kan du även vara intresserad av att begränsa den samtidiga åtkomsten till innehållet i alla målportaler.

  *F: Varför programmera och inte beställar-ID som i resten av Adobe Pass-autentiseringen?*

  *S: Anledningen är att programmerare kan använda den här parametern flexibelt för att skicka eller isolera data mellan sina egenskaper beroende på hur de används.*

## Resurs {#resource-defn}

* Det faktiska innehåll som ett ämne vill konsumera. Resursen innehåller vanligtvis attribut som är relaterade till ägaren (utgivaren) och kan även innehålla extra information, som genre eller något annat.

## Regel {#rule-defn}

* En boolesk funktion som utvärderas mot en viss ström och den relevanta användaraktiviteten för att avgöra om åtkomst ska tillåtas eller nekas för den strömmen.

## Direktuppspelningssession {#streaming-session-defn}

* En direktuppspelad videosession som initierats av ett ämne för att uppta en viss resurs.

## Ämne {#subj-defn}

* Konsumenten av (video)-innehållet via internet. Vi undviker avsiktligt termen _**användare**_, eftersom Concurrency Monitoring vanligtvis hanterar konton-ID:n för distributörer av videoprogrammeringstjänster (vilket inbegriper flera faktiska användare som delar samma kontrakt, till exempel familjemedlemmar för ett hushåll).

* För varje ström kan motivet förbättras med attribut som är kopplade till den person som använder tjänsten, den nätverksanslutna enheten och så vidare.

## Prenumerant {#subscriber-defn}

* Den betalande kunden till ett sidoskydd eller en person som delar den betalande kundens uppgifter
* Kan stoppas från att titta på innehåll av Concurrency Monitoring Service, av klientprogrammet som använder tjänsten ovan.
* I det bästa fallet kommer han eller hon aldrig att märka att Concurrency Monitoring Service finns

## Mål {#target-defn}

* Ett dataströmpredikat som returnerar om regeln gäller för en viss dataström. Det implicita målet i CM är en dataström som skapas av ett program som refererar till den aktuella principen. Dessutom kan attributvärdesvillkor läggas till för att finjustera aktivitetsfiltreringen innan reglerna tillämpas.

## Klientorganisation {#tenant-defn}

* En samtidighetsövervakningskundens organisation.
