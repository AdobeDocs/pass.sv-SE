---
title: AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg
description: AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

Förändringar av nyckelhanteraren från [new AccessEnabler version 3.7.0](/help/authentication/authn-rn-ios-tvos-370.md) är inkompatibla med Keychain-lagringsimplementering från AccessEnabler-versionen är lägre än 3.7.0.

Uppgraderingssökvägen för ett program som använder den nya AccessEnabler-versionen 3.7.0 migrerar alla token från tidigare versioner av Keychain-lagring. Därför bör slutanvändare **får inte uppleva någon förlust av autentiserings-/auktoriseringssessioner** under uppdateringsprocessen för AccessEnabler-ramverket.

## Kända begränsningar

Vissa begränsningar, som beskrivs nedan, kan förekomma av implementerare.


1. Vanlig enkel inloggning (Adobe) fungerar inte mellan ett program som använder AccessEnabler version 3.7.0 och ett program som använder AccessEnabler version/er som är lägre än 3.7.0, inte ens för program som utvecklats av samma leverantör.

   - **Viktigt:**
      - SSO (Apple) på systemnivå påverkas inte.
      - Vanlig enkel inloggning (Adobe) fungerar även fortsättningsvis om båda programmen har utvecklats av samma leverantör och använder versioner av AccessEnabler som är lägre än 3.7.0!
      - Vanlig enkel inloggning (Adobe) fungerar om båda programmen har utvecklats av samma leverantör och använder AccessEnabler version 3.7.0!

1. Om ett program som använder AccessEnabler version 3.7.0 nedgraderas till en lägre version av AccessEnabler migreras inte nya genererade token. Därför kan slutanvändare drabbas av förlust av autentiserings-/auktoriseringssessioner, utan att det förväntas.

   - **Viktigt:**
      - Slutanvändare som autentiseras via Apple SSO (System Level) påverkas inte.
      - Slutanvändare som redan har autentiserats innan de uppdaterades till det nya programmet med AccessEnabler version 3.7.0 påverkas inte!

1. Om ett program som använder AccessEnabler version 3.7.0 nedgraderas till en lägre version av AccessEnabler, bekräftas inte borttagna token. Därför kan slutanvändare uppleva att det finns autentiserings-/auktoriseringssessioner, utan att det förväntas.
