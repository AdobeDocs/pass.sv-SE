---
title: Cookies Updates - SameSite and Secure flags
description: Cookies Updates - SameSite and Secure flags
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 0%

---

# Cookies Updates - SameSite and Secure flags {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>


## Uppdateringar {#Updates}

I det här avsnittet beskrivs de ändringar som gjorts i webbläsaren Chrome och i Adobe Pass Authentication för hantering av cookies från tredje part.



### Chrome 80-uppdateringar {#Chrome}

Från och med Chrome version 80 (utom version 82), cookies som inte anger en *SameSite* -attribut behandlas som om de vore *SameSite=Lax*. Därför måste cookies som måste levereras i ett sammanhang som omfattar flera webbplatser uttryckligen ange *SameSite=None* och måste även markeras med *Säker* attribut och levereras över *HTTPS*. Mer information om uppdateringarna finns på den officiella kromsidan: <https://www.chromium.org/updates/same-site> och från <https://web.dev/samesite-cookies-explained/>.


### Autentiseringsuppdateringar för Adobe Pass {#Pass-Updates}

Adobe Pass autentiseringstjänst förlitar sig för närvarande på några cookies som betraktas som cookies från tredje part från webbläsarens synvinkel, inklusive Chrome, för att fungera i kombination med vissa plattformar och versioner av Adobe Pass Authentication SDK:er. För att följa de kommande ändringarna och fortsätta att leverera dessa cookies i en kontext som sträcker sig över flera webbplatser från dessa äldre SDK:er implementerar Adobe Pass Authentication-tjänsten de ändringar som krävs i *adobe-pass-2.55.1* version.

De här ändringarna från *adobe-pass-2.55.1* version innehåller tillägg av *Säker* och *SameSite=None* attribut för alla cookies som skickas tillbaka till alla Adobe Pass Authentication SDK:er när Chrome-webbläsare används från version 80 och senare (utom version 82).

I nästa avsnitt beskrivs några möjliga problem med en lista över plattformar och Adobe Pass Authentication SDKs-versioner om en användare använder Chrome-webbläsaren 80 eller senare (förutom version 82).

## Felsökning {#Troubleshooting}

När du bläddrar igenom det här avsnittet bör du tänka på att alla cookies för tjänsten Adobe Pass Authentication måste ha *Säker* attributuppsättning i *adobe-pass-2.55.1* version för alla webbläsare, medan *SameSite=None* -attributet får bara anges för Chrome-webbläsare version 80 och senare (utom version 82).


### Allmän felsökning {#General}

1. Observera att vissa användaragenter är kända för att vara inkompatibla med *SameSite=None* -attribut.

   - Versioner av Chrome från Chrome 51 till Chrome 66 (inklusive på båda sidor). Dessa Chrome-versioner avvisar en cookie med *SameSite=None*. Detta påverkar även äldre versioner av Chromium-baserade webbläsare samt Android WebView. Detta beteende var korrekt enligt den version av cookie-specifikationen som användes vid den tidpunkten, men i och med att det nya värdet &quot;Ingen&quot; lades till i specifikationen har det här beteendet uppdaterats i Chrome 67 och senare. (Före Chrome 51 ignorerades attributet SameSite fullständigt och alla cookies behandlades som om de vore det *SameSite=None*.)
   - Tidigare version 12.13.2 av UC Browser i Android. Äldre versioner avvisar en cookie med *SameSite=None*. Detta beteende var korrekt enligt den version av cookie-specifikationen som användes vid den tidpunkten, men i och med att det nya värdet &quot;Ingen&quot; lades till i specifikationen har det här beteendet uppdaterats i senare versioner av UC Browser.
   - Versioner av Safari och inbäddade webbläsare i MacOS 10.14 och alla webbläsare i iOS 12. Dessa versioner hanterar felaktigt cookies som är markerade med *SameSite=None* som om de hade markerats *SameSite=strict*. Detta fel har åtgärdats i senare versioner av iOS och MacOS.


1. Observera att cookies som har *Säker* attribut måste skickas över *HTTPS* annars kommer cookien inte att nå Adobe Pass-autentiseringstjänsten.

   - AccessEnabler JavaScript SDK:
      - Obligatoriskt att kommunikationen med *sp.auth.adobe.com* använder *HTTPS* för versioner *2.35* och *3.5.0* innan vi introducerar Dynamic Client Registration.
   - AccessEnabler iOS/tvOS SDK:
      - Obligatoriskt att kommunikationen med *sp.auth.adobe.com* använder *HTTPS* för versioner före *3.0.0* innan vi introducerar Dynamic Client Registration.
   - AccessEnabler Android SDK:
      - Obligatoriskt att kommunikationen med *sp.auth.adobe.com* använder *HTTPS* för versioner före *3.0.0* innan vi introducerar Dynamic Client Registration.
   - AccessEnabler FireOS SDK:
      - Obligatoriskt att kommunikationen med *sp.auth.adobe.com* använder *HTTPS* för version *2.0.4*.

</br>

### AccessEnabler JavaScript SDK version 2.35 - felsökning {#235-Troubleshooting}

Användarens autentiseringsflöde kan påverkas i Chrome 80 och senare (utom version 82). För att säkerställa att användaren inte har problem att autentisera på grund av uppdateringarna ovan kan man:

- Kontrollera att *JSESSIONID* cookie är inställd i webbläsaren och har *SameSite=None* och *Säker* attribut.
- Kontrollera att *JSESSIONID* cookie från *https://sp.auth.adobe.com/authenticate/saml* nätverksbegäran matchar *JSESSIONID* cookie från *https://sp.auth.adobe.com/session* nätverksbegäran.


### AccessEnabler JavaScript SDK version 3.5.0 - felsökning {#350-Troubleshooting}

Användarens autentiseringsflöde kan påverkas i Chrome 80 och senare (utom version 82). För att säkerställa att användaren inte har problem att autentisera på grund av uppdateringarna ovan kan man:

- Kontrollera att *JSESSIONID* cookie är inställd i webbläsaren och har *SameSite=None* och *Säker* attribut.
- Kontrollera att *JSESSIONID* cookie från *https://sp.auth.adobe.com/authenticate/saml* nätverksbegäran matchar *JSESSIONID* cookie från *https://sp.auth.adobe.com/session* nätverksbegäran.
- Kontrollera att *pass\_sfp* cookie är inställd i webbläsaren och har *SameSite=None* och *Säker* attribut.
- Kontrollera att *pass\_sfp* cookie är inställd i *https://sp.auth.adobe.com/session* nätverksbegäran.


Användarens auktoriseringsflöde kan påverkas i Chrome 80 och senare (utom version 82). För att vara säker på att användaren inte har problem att se på en skyddad resurs efter att ha autentiserats korrekt kan du göra följande på grund av uppdateringarna ovan:

- Kontrollera att *pass\_sfp* cookie är inställd i webbläsaren och har *SameSite=None* och *Säker* attribut.
- Kontrollera att *pass\_sfp* cookie är inställd i *https://sp.auth.adobe.com/adobe-services/authorize* nätverksbegäran.
- Kontrollera att *pass\_sfp* cookie är inställd i *https://sp.auth.adobe.com/adobe-services/shortAuthorize* nätverksbegäran.
