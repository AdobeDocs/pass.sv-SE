---
title: AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg
description: AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# AccessEnabler iOS/tvOS 3.7.0 - uppgraderingssökväg {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

Ändringar av nyckelring från [nya AccessEnabler version 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md) är inte kompatibla med Keychain-lagringsimplementeringen från AccessEnabler-versionen är lägre än 3.7.0.

Uppgraderingssökvägen för ett program som använder den nya AccessEnabler-versionen 3.7.0 migrerar alla token från tidigare versioner av Keychain-lagring. Därför bör slutanvändare **inte drabbas av förlust av autentiserings-/auktoriseringssessioner** under uppdateringsprocessen för AccessEnabler-ramverket.

## Kända begränsningar

Vissa begränsningar, som beskrivs nedan, kan förekomma av implementerare.


1. Vanlig enkel inloggning (Adobe) fungerar inte mellan ett program som använder AccessEnabler version 3.7.0 och ett program som använder AccessEnabler version/er som är lägre än 3.7.0, inte ens för program som utvecklats av samma leverantör.

   >[!IMPORTANT]
   >
   >* SSO (Apple) på systemnivå påverkas inte.
   >
   >* Vanlig enkel inloggning (Adobe) fungerar även fortsättningsvis om båda programmen har utvecklats av samma leverantör och använder versioner av AccessEnabler som är lägre än 3.7.0!
   >
   >* Vanlig enkel inloggning (Adobe) fungerar om båda programmen har utvecklats av samma leverantör och använder AccessEnabler version 3.7.0!


1. Om ett program som använder AccessEnabler version 3.7.0 nedgraderas till en lägre version av AccessEnabler migreras inte nya genererade token. Därför kan slutanvändare drabbas av förlust av autentiserings-/auktoriseringssessioner, utan att det förväntas.

   >[!IMPORTANT]
   >
   >* Slutanvändare som autentiseras via Apple SSO (System Level) påverkas inte.
   >* Slutanvändare som redan har autentiserats innan de uppdaterades till det nya programmet med AccessEnabler version 3.7.0 påverkas inte!

1. Om ett program som använder AccessEnabler version 3.7.0 nedgraderas till en lägre version av AccessEnabler, bekräftas inte borttagna token. Därför kan slutanvändare uppleva att det finns autentiserings-/auktoriseringssessioner, utan att det förväntas.
