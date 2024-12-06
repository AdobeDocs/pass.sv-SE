---
title: Använda Experience Cloud-ID i Adobe Pass-autentisering
description: Använda Experience Cloud-ID i Adobe Pass-autentisering
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Använda Experience Cloud-ID i Adobe Pass-autentisering

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Vad är Experience Cloud ID och hur skaffar man det? {#what-exp-cloud-id-obtain}

Experience Cloud ID (ECID för kort) är ett unikt ID som skapas av Adobe Experience Cloud för varje enskild användare i ditt program/på din webbplats. ECID används i stor utsträckning i alla Experience Cloud-rapporter som används för att länka samman information om en viss användare över flera program/webbplatser.

Om du redan har ett system som tillhandahåller ett besökar-ID bör du använda samma ID för dokumentets omfång.

Ett sätt att få ett ECID är att använda Experience Cloud ID-tjänsten. Du kan använda den implementeringstyp som du föredrar, antingen baserat på TDM, JS-bibliotek, serversidan, direktintegrering eller inbyggda bibliotek för mobila plattformar. En utförlig översikt över tillgängliga tjänster, bibliotek, SDK:er och implementeringsguider finns på: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## Vilken är fördelen med att använda Experience Cloud-ID vid Adobe Pass-autentisering? {#benefit-ex-cloud-id}

Om du konfigurerar våra SDK:er och klientlösa REST API:er att använda ditt ECID kan du senare länka data som samlats in av Adobe Pass Authentication till dina befintliga Experience Cloud-lösningar. På så sätt kan ni bättre förstå era kunders resa och upplevelse i alla lösningar som Adobe tillhandahåller.

## Hur använder man Experience Cloud ID i Adobe Pass Authentication? {#how-to-ex-cloud-id-authn}

När du har fått ett ECID (förklaras ovan) måste du skicka informationen till våra SDK:er och vårt klientlösa REST API. Den här informationen skickas senare till våra servrar vid varje nätverksanrop som SDK gör. Konfigurationsprocessen är annorlunda för varje SDK enligt följande:

### JS SDK {#js-sdk}

För JavaScript måste du skicka ECID i en karta som den tredje parametern till setRequestor-anropet.

**Användningsexempel:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

För iOS/tvOS SDK finns det en dedikerad metod som kallas setOptions.

**Användningsexempel:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/FireTV SDK {#android-sdk}

För Android/FireTV SDK liknar mekanismen iOS. Det är bara parameternamnet som är annorlunda. API:t beskrivs här.

**Användningsexempel:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### Klientlöst API {#clientless-api}

När du använder Adobe Pass via dess REST API, ska värdet **ECID** skickas **för alla API:er** som en parameter med namnet **&#39;ap_vi&#39;**.

**Användningsexempel:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`
