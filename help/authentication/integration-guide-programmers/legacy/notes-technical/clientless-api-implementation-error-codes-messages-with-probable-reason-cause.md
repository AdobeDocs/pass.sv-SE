---
title: Klientlös API-implementering - Felkoder / Meddelanden med trolig orsak / orsak
description: Klientlös API-implementering - Felkoder / Meddelanden med trolig orsak / orsak
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (Äldre) Klientlös API-implementering - Felkoder / Meddelanden med trolig orsak / orsak {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av detta API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena och tidslinjerna för inaktivering av Adobe Pass Authentication som sammanställs på [sidan Product Announcements](/help/authentication/product-announcements.md) .

</br>


## Fel: Inte auktoriserad

### Orsakar:

1. Auktoriseringshuvud saknas i POST
1. Problem med auktoriseringshuvud – kontrollera om begärandetiden är i millisekunder.

## Fel: SC 400 under autentisering

### Orsakar:

1. Servern hittade inte registreringskoden, som skapades för en specifik beställare och miljö.
1. Du kan stöta på problem med skript över flera domäner
1. Korrekt förfalskning bör läggas till i filen /etc/hosts

## Fel: 400 Ogiltig begäran

### Orsakar:

1. Felaktig url för POST/GET
1. SAMLAssertionParserException – Det gick inte att dekryptera den krypterade SAML-försäkran hos Adobe

## Fel: 403 Förbjudet

### Orsakar:

1. För många snabba begäranden – en funktion i API-hanteringen för att förhindra DoS-attacker.
2. Om du använder prequal miljö ska du lägga till spoofing, annars se till att spoofing har tagits bort från /etc/hosts-filen

## Fel: Det går inte att logga in på MVPD-sidan

### Orsakar:

1. Användarnamnet och lösenordet matchar inte
2. Inloggning kan ha inaktiverats
3. Kontrollera om inloggningen är för produktion eller mellanlagring


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
