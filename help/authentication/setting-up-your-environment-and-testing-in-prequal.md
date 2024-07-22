---
title: Konfigurera din miljö och testning i Pre-Qual
description: Konfigurera din miljö och testning i Pre-Qual
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Konfigurera din miljö och testa i Pre-Qual{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av detta API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Syftet med den här tekniska anteckningen är att hjälpa våra partners att konfigurera sin miljö och börja testa en ny version som distribuerats i Adobes förkvalificeringsmiljö.

Eftersom det finns två byggvarianter: ***produktion*** och ***mellanlagring***, kommer vi i det här dokumentet att fokusera på produktionsinställningen med omnämnandet att alla steg är desamma för mellanlagring, bara URL:erna är olika.

Steg 1 och 2 är att sätta upp testmiljön på en av testmaskinerna, steg 3 är en verifiering av det grundläggande flödet och steg 4 och 5 presenterar några testriktlinjer.

>[!IMPORTANT]
>
> Det är mycket viktigt att utföra steg 1 och 2 varje gång du vill ändra din testmiljö (byta från staging till produktionsprofil eller tvärtom)


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

* **På Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>Domäner utesluts från svar eftersom de inte är relevanta och kan skilja sig från användare till användare.

>[!IMPORTANT]
>
> Dessa IP-adresser kan ändras i framtiden och de kanske inte är desamma för användare i olika geografiska regioner.


## STEG 2.  Förfalskning av förkvalificeringsmiljön som ska vara produktion {#spoofing-the-prequalification-environment}

* *Redigera filen c:\\windows\\System32\\drivers\\etc\\hosts* (i Windows) eller */etc/hosts* filen (på Macintosh/Linux/Android) och lägg till följande:

* Profil för förfalskning av produktion
   * 52.13.71.11 http://entitlement.auth.adobe.com, http://sp.auth.adobe.com, http://api.auth.adobe.com

**Spofing på Android:** Om du vill göra en buffring på Android måste du använda en Android-emulator.

* När denna funktion är på plats kan du helt enkelt använda de vanliga URL:erna för produktions- och staging-profilerna: (d.v.s. `http://sp.auth-staging.adobe.com` och `http://entitlement.auth-staging.adobe.com`, och du kommer att träffa *förkvalificeringsmiljön/ produktionen* i den nya*-versionen.


## STEG 3.  Kontrollera att du pekar mot rätt miljö {#Verify-you-are-pointing-to-the-right-environment}

**Det här är ett enkelt steg:**

* Lastberättigande [prequal miljö](https://entitlement-prequal.auth.adobe.com/environment.html) och [berättigande](https://entitlement.auth.adobe.com/environment.html). De bör returnera samma svar.


## STEG 4.  Utför ett enkelt autentiserings-/auktoriseringsflöde med hjälp av programmerarens webbplats {#peform-a-simple-auth-flow}

* Det här steget kräver programmerarens webbadress och några giltiga MVPD-autentiseringsuppgifter (en användare som den är autentiserad och auktoriserad).

## STEG 5.  Utför scenariotestning med hjälp av programmerarens webbplatser {#perform-scenario-testing-using-programmer-website}

* När du har slutfört miljökonfigurationen och sett till att det grundläggande autentiseringsauktoriseringsflödet fungerar kan du fortsätta med testningen av mer komplexa scenarier.


## STEG 6.  Testa med API-testwebbplatsen {#perform-testing-using-api-testing-site}

* Om du vill gå djupare i testningen av Adobe Pass-autentisering rekommenderar vi att du använder [API-testwebbplatsen](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Mer information om API-testwebbplatsen finns på [Så här testar du autentiserings- och auktoriseringsflöden med Adobe API-testplats](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).
