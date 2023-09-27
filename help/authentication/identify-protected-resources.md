---
title: Identifiera skyddade resurser
description: Identifiera skyddade resurser
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Identifiera skyddade resurser {#identifying-protected-resources}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#overview}

Varje auktoriseringsbegäran (eller begäran om kontroll av auktorisering) måste innehålla en unik identifierare för den skyddade resurs som användaren begär åtkomst till. En skyddad resurs kan vara vilken nivå som helst av auktoriserat innehåll, enligt överenskommelse mellan en distributör och medverkande programmerare. Potentiella skyddade resurser måste passa in i denna trädstruktur av allt mer specifik granularitet:

- Nätverk
   - Kanal
      - Visa
         - Episod
            - Tillgång

</br>

## Media RSS-format {#media_rss}

Resurser kan identifieras med en enkel sträng (en unik identifierare för en kanal) eller representeras i Media RSS-format (MRSS) enligt överenskommelse mellan Adobe (eller en auktoriserad partner för Adobe Pass-autentisering) och deltagande programmerare och programmerare. Den RSS-sträng som används som resursspecificerare kan innehålla ytterligare information, till exempel klassificeringar och metadata för föräldrakontroll.


Om du använder en enkel resursidentifierare, t.ex. &quot;TNT&quot;, antas den representera en kanal och översätts till den här RSS-resursspecificeraren:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


En mer komplex specificerare kan t.ex. innehålla ytterligare graderingsinformation. Du kan skicka hela RSS-strängen till Access Enabler-funktioner som kräver ett resurs-ID, som [`getAuthorization()`](/help/authentication/rest-api-reference.md):

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

Resursspecificerare är ogenomskinliga för Adobe Pass-autentisering. De skickas bara till MVPD. Om MVPD inte känner igen eller inte kan tolka din resursspecificerare returneras ett fel till Adobe Pass Authentication, som skickar felet tillbaka till din `tokenRequestFailed()` återanrop.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
