---
title: Klientlös API-implementering - Felkoder / meddelanden med sannolik orsak / orsak
description: Klientlös API-implementering - Felkoder / meddelanden med sannolik orsak / orsak
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Klientlös API-implementering - Felkoder / meddelanden med sannolik orsak / orsak {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Innehållet på denna sida tillhandahålls endast i informationssyfte. Användning av detta API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>


## Fel: Inte auktoriserad

### Orsakar:

1. Auktoriseringshuvud saknas i självtest vid start
1. Problem med auktoriseringshuvudet - kontrollera om förfrågningstiden är i millisekunder.

## Fel: SC 400 under autentisering

### Orsakar:

1. Servern hittade inte registreringskoden, som skapades för en specifik beställare och miljö.
1. Du kan stöta på problem med skriptkörning över flera domäner
1. Korrekt förfalskning bör läggas till i filen /etc/hosts

## Fel: 400 felaktig begäran

### Orsaker:

1. Felaktig webbadress för POST/GET
1. SAMLAssertionParserException – Den krypterade SAML-försäkran kunde inte dekrypteras i slutet av Adobe

## Fel: 403 Förbjuden

### Orsakar:

1. För många snabba förfrågningar - en funktion i API-hanteringen för att förhindra DoS-attacker.
2. Om du använder prequal environment lägger du till förfalskning, annars ser du till att förfalskning har tagits bort från filen /etc/hosts

## Fel: Det går inte att logga in på MVPD-sidan

### Orsakar:

1. Användarnamnet och lösenordet matchar inte
2. Inloggning kan ha inaktiverats
3. Kontrollera om inloggningen är avsedd för produktion eller mellanlagring


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
