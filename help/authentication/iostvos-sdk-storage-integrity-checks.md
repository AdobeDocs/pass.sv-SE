---
title: Kontrollmekanism för lagringsintegritet för iOS/tvOS
description: Kontrollmekanism för integritetskontroll för iOS/tvOS
source-git-commit: 444a81ad18afcb26dcf3ae8b41f4d02c465f4655
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Kontrollmekanism för integritetskontroll för iOS/tvOS {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Introduktion {#Intro}

Från och med version 3.8.3 av iOS/tvOS AccessEnabler SDK kan du utföra kontroller av lagringsintegritet vid initiering av AccessEnabler.

Om du vill använda den här funktionen utökades API:t med en extra initieringsmetod för klassen AccessEnabler.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Integritetskontroller {#Checks}

Lagringsintegritetskontrollerna är användbara när man misstänker att AccessEnabler-lagringen är skadad (till exempel när ett konkurrenstillstånd inträffar under en read/write-lagringsåtgärd).

Följande kontroller kan utföras vid AccessEnabler-initiering:
- Lagringsoperabilitet: söker efter lyckade läs- och skrivåtgärder
- Sparad värdeintegritet: kontrollerar att alla värden är giltiga och i det förväntade formatet

>[!IMPORTANT]
> 
>Om en av kontrollerna misslyckas rensas alla värden i lagringen och användaren loggas ut, vilket kan leda till en dålig användarupplevelse. Använd endast kontroller av lagringsintegritet när det anses nödvändigt.


## Standardbeteende {#Default}

Lagringsintegritetskontrollerna inaktiveras som standard när AccessEnabler initieras med standardinitieringsmetoden:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Använd följande initieringsmetod för att explicit ange vilken lagringsintegritet som ska utföras vid AccessEnabler-initiering:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

Uppräkningen IntegrityCheckType visas för klientprogrammet och har följande värden:

| Värde | Utförda kontroller | Lagring rensad | Beskrivning | Rekommenderat användningsfall |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Ingen | Aldrig | Inga integritetskontroller utförs vid lagringsinitiering | När SDK-flödena fungerar som förväntat |
| INTEGRITY_CHECK_ALL | Driftsäkerhet för lagring <br/> Giltighet för lagrade värden | Vid check misslyckas | Alla tillgängliga integritetskontroller utförs vid lagringsinitiering | När SDK-lagring misstänks vara skadad. <br/> Om integritetskontrollerna misslyckas loggas användaren ut |
| INTEGRITY_CHECK_CLEAR | Ingen | Alltid | Lagringsutrymmet rensas vid lagringsinitiering | När SDK-flödena inte kan slutföras som förväntat |
