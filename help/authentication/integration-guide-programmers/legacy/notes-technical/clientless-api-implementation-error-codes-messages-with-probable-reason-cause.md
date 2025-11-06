---
title: Kundfri API-implementering - felkoder/meddelanden med trolig orsak/orsak
description: Kundfri API-implementering - felkoder/meddelanden med trolig orsak/orsak
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (Äldre) API-implementering utan klient - Felkoder/meddelanden med trolig orsak/orsak {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

</br>


## Fel: Inte auktoriserad

### Orsaker:

1. Auktoriseringshuvud saknas i POST
1. Problem med auktoriseringshuvud - kontrollera om begärandetiden är i millisekunder.

## Fel: SC 400 vid autentisering

### Orsaker:

1. Servern hittade inte registreringskoden som skapades för en specifik begärare och miljö.
1. Du kan stöta på cross domain-skriptproblem
1. Korrekt förfalskning bör läggas till i filen /etc/hosts

## Fel: 400 Ogiltig begäran

### Orsaker:

1. Felaktig URL för POST/GET
1. SAMLAsertionParserException - Den krypterade SAML-kontrollen kunde inte dekrypteras i Adobe slut

## Fel: 403 Ej tillåtet

### Orsaker:

1. För många snabba förfrågningar - en funktion i API-hanteringen för att förhindra DoS-attacker.
2. Om du använder en fördefinierad miljö lägger du till förfalskning, annars kontrollerar du att förfalskning har tagits bort från filen /etc/hosts

## Fel: Det gick inte att logga in på MVPD-sidan

### Orsaker:

1. Användarnamnet och lösenordet matchar inte
2. Inloggning kan ha inaktiverats
3. Kontrollera om inloggningen är avsedd för produktion eller mellanlagring


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
