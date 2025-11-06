---
title: Konfigurera din miljö och testning i Pre-Qual
description: Konfigurera din miljö och testning i Pre-Qual
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# Konfigurera din miljö och testning i Pre-Qual{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Syftet med detta tekniska meddelande är att hjälpa våra partners att konfigurera sin miljö och testa en ny version som körs i Adobe förkvalificeringsmiljö.

Eftersom det finns två build-versioner: ***production*** och ***staging*** kommer vi i det här dokumentet att fokusera på produktionsinställningarna och meddela att alla steg är desamma för mellanlagring. Det är bara URL:erna som är olika.

Steg 1 och 2 konfigurerar testmiljön på en av testdatorerna, steg 3 är en verifiering av basflödet och steg 4 och 5 presenterar några riktlinjer för testningen.

>[!IMPORTANT]
>
> Det är mycket viktigt att du kör steg 1 och 2 varje gång du vill ändra testmiljön (växla från mellanlagring till produktionsprofil eller på andra sätt)


## STEG 1. Matcha lösenordsdomän till en IP {#resolving-pass-domain-to-an-ip}

* Om du vill hitta en IP-adress för belastningsutjämnare som kan användas för förfalskning kör du följande kommando:

* **I Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* **I Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>Domäner som utesluts från svar eftersom de inte är relevanta och kan skilja sig från användare till användare.

>[!IMPORTANT]
>
> Dessa IP-adresser kan komma att ändras i framtiden och de är kanske inte desamma för användare i olika geografiska regioner.


## STEG 2.  Spofing the pre-eligibility environment to be production {#spoofing-the-prequalification-environment}

* Redigera filen *c:\\windows\\System32\\drivers\\etc\\hosts* (i Windows) eller filen */etc/hosts* (i Macintosh/Linux/Android) och lägg till följande:

* Profil för dekorproduktion
   * 52.13.71.11 api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**Spofing på Android:** Om du vill göra en buffring på Android måste du använda en Android-emulator.

* När denna funktion är på plats kan du helt enkelt använda de vanliga URL:erna för produktions- och staging-profilerna: (d.v.s. `http://sp.auth-staging.adobe.com` och `http://entitlement.auth-staging.adobe.com`, och du kommer att träffa *förkvalificeringsmiljön/ produktionen* i den nya*-versionen.


## STEG 3.  Kontrollera att du pekar mot rätt miljö {#Verify-you-are-pointing-to-the-right-environment}

**Detta är ett enkelt steg:**

* läsa in [berättigandeförhandsmiljö](https://entitlement-prequal.auth.adobe.com/environment.html) och [berättigande](https://entitlement.auth.adobe.com/environment.html). De bör returnera samma svar.


## STEG 4.  Utför ett enkelt autentiserings-/auktoriseringsflöde via programmerarens webbplats {#peform-a-simple-auth-flow}

* Det här steget kräver programmerarens webbplatsadress och vissa giltiga MVPD-autentiseringsuppgifter (en användare som är autentiserad och behörig).

## STEG 5.  Testa scenarier med programmerarens webbplatser {#perform-scenario-testing-using-programmer-website}

* När du är klar med miljökonfigurationen och ser till att det grundläggande autentiserings-/auktoriseringsflödet fungerar kan du fortsätta med testningen av mer komplexa scenarier.


## STEG 6.  Testa med API-testwebbplatsen {#perform-testing-using-api-testing-site}

* Om du vill gå djupare i testningen av Adobe Pass-autentisering rekommenderar vi att du använder [API-testwebbplatsen](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Mer information om API-testwebbplatsen finns på [Så här testar du autentiserings- och auktoriseringsflöden med hjälp av Adobe API-testwebbplats](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).
