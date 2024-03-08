---
title: enkel inloggning på iOS när Adobe Pass Authentication Access Enabler används
description: enkel inloggning på iOS när Adobe Pass Authentication Access Enabler används
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# enkel inloggning på iOS när Adobe Pass Authentication Access Enabler används {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

## Ökning

Single Sign-On (SSO) mellan program som använder Adobe Pass Authentication fungerar på olika sätt beroende på det underliggande operativsystemet.

Dokumentadresserna **SSO på iOS** när du använder Adobe Pass-autentisering **Åtkomstaktivering**.

**Åtkomstaktivering** **1.10** är den senaste versionen av Adobe Pass Authentication iOS Native SDK. Adobe rekommenderar att du går över till den här versionen i stället för att stanna kvar med en äldre version. Om du använder en äldre version av Access Enabler kan du hämta den senaste versionen [här](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

SSO på iOS regleras av följande villkor:

- Apparna måste använda samma **tokenlagring** (i form av ett anpassat monteringsbord som skapas av Access Enabler).
- Apparna måste generera samma **Enhets-ID** (Access Enabler beräknar enhets-ID baserat på MAC-adressen eller IDFV, beroende på OS-versionen).

## Beteende

SSO-beteendet är följande:

- **iOS 6 och tidigare**: SSO fungerar automatiskt mellan program som utvecklas av samma team eller olika team. Enhets-ID beräknas baserat på MAC-adressen (samma värde produceras i alla program) och lagringsområdet är gemensamt för alla program (anpassat monteringsbord kan delas mellan program i iOS 6 och tidigare).
   - **Viktigt:** Observera att iOS SDK 1.9.4 har [har ökat iOS lägsta driftsättningsmål till iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 och senare**: SSO fungerar under följande förhållanden:

1. Program publiceras med samma Apple-distributionsprofil, eller profiler som tillhör samma team. Detta är det enda sättet för appar att dela anpassade monteringsbord på iOS 7 och senare. I alla andra scenarier är monteringsbordet i sandlåda per program. Från [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] och +\[`UIPasteboard pasteboardWithUniqueName`\] är nu unikt med det angivna namnet så att bara de appar som finns i samma programgrupp kan komma åt monteringsbordet. Om utvecklaren försöker skapa ett monteringsbord med ett namn som redan finns och som inte ingår i samma programsvit får han eller hon ett eget unikt och privat monteringsbord. Observera att detta inte påverkar systemets monteringsbord, allmänt och sök efter.

1. Appar har samma paket-ID-prefix (alla komponenter utom den sista). Endast program som delar samma paket-ID-prefix kommer att beräkna samma IDFV. Från [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): På IOS 7 används alla komponenter i paketet utom den sista komponenten för att generera leverantörs-ID. Om paket-ID:t bara har en enda komponent används hela paket-ID:t.

Nu fokuserar vi på **iOS 7 och senare** eftersom det är det vanligaste för verkliga användare:

Båda villkoren (som delar en profil från samma utvecklingsteam och har ett gemensamt paket-ID-prefix) är obligatoriska villkor för enkel inloggning.

Här är möjliga kombinationer och deras resultat:

- **Profiler från samma team och samma paket-ID-prefix**: appar delar samma monteringsbord och har samma enhets-ID (IDFV). En användare behöver bara autentisera en gång (i den första appen som används) och autentiseringsstatusen delas i alla andra appar. Exempel:

1. Användaren öppnar app A (med paket-ID) *com.x.y.AppA*) och är inte autentiserad
1. Användaren utför autentisering i app A
1. Användaren öppnar app B (med paket-ID *com.x.y.AppB*) och autentiseras automatiskt genom delning av berättigandedata från program A (från steg 2)
1. Användaren öppnar app A och är fortfarande autentiserad (från steg 2)



- **Profiler från samma team men olika paket-ID-prefix**: appar delar samma lagringsutrymme på monteringsbordet men har olika enhets-ID:n (IDFV). Användaren måste autentisera en gång per app. Exempel:

1. Användaren öppnar app A (med paket-ID) *com.x.y.AppA*) och är inte autentiserad
1. Användaren utför autentisering i app A
1. Användaren öppnar app B (med paket-ID *com.z.AppB*) och Access Enabler identifierar token som erhållits av det första programmet (eftersom lagringsutrymmet delas), men det kommer inte att försöka använda det via enkel inloggning på grund av de olika enhets-ID:n
1. Användaren utför autentisering i app B
1. Användaren öppnar app A och är fortfarande autentiserad (från steg 2)



- **Profiler från olika team (paket-ID är irrelevant i det här scenariot)**: appar har olika lager på monteringsbordet och enkel inloggning inaktiveras mellan dem. Användaren måste autentisera en gång per app och autentiseringssessionerna förblir permanenta när användaren växlar mellan appar. Exempel:


1. Användaren öppnar app A och är inte autentiserad
1. Användaren utför autentisering i app A
1. Användaren öppnar app B och är inte autentiserad
1. Användaren utför autentisering i app B
1. Användaren öppnar app A och autentiseras (från steg 2)
1. Användaren öppnar app B och autentiseras (från steg 4)

**Obs!** Observera att ovanstående villkor för enkel inloggning gäller vid installation av program via **Apple App Store**. Om apparna distribueras i en simulator (där appsignering inte gäller), installeras med Xcode eller distribueras via en Ad hoc-profil kan du få olika resultat.

**Viktigt:** Anteckning (**gällande AccessEnabler v1.8**): Det andra scenariot som beskrivs ovan (profiler från samma team men olika paket-ID-prefix) skapar en mycket obehaglig användarupplevelse för användare av **AccessEnabler v1.8** mellan program som har utvecklats av samma team (medieföretag). Användaren loggas automatiskt ut när appar överförs mellan olika medieföretag, och programutvecklare måste därför vara försiktiga när de bestämmer sig för paket-ID och distributionsprofil. Det exakta scenariot i detta fall presenteras nedan:

Apparna delar samma monteringsbord men kommer att ha olika enhets-ID:n (IDFV). En användare måste autentisera en gång per app, **men autentiseringssessionerna kommer att tas bort när du växlar mellan appar**. Exempel:

1. Användaren öppnar app A (med paket-ID) *com.x.y.AppA*) och är inte autentiserad
1. Användaren utför autentisering i app A
1. Användaren öppnar app B (med paket-ID *com.z.AppB*) och de berättigandedata som skapades av program A raderas automatiskt av Access Enabler (säkerhetsmekanism som upptäcker en konflikt mellan det aktuella beräknade enhets-ID:t i program B och det som lagras i berättigandetokenerna - skapade av program A)
1. Användaren utför autentisering i app B
1. Användare öppnar app A och berättigandedata som skapades av app B raderas automatiskt av åtkomstaktiveraren (säkerhetsmekanism som upptäcker en konflikt mellan det för närvarande beräknade enhets-ID:t i program A och det som lagras i berättigandetokenerna - skapade av app B)
