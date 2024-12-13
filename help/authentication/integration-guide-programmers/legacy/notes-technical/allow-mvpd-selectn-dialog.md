---
title: Tillåt PDF-filer i dialogrutan Markering
description: Tillåt PDF-filer i dialogrutan Markering
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# (Äldre) Tillåt PDF-filer i dialogrutan Markering {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Problem {#issue}

Programmeraren kanske vill testa eller kontrollera användarupplevelsen av nya MVPD-integreringar innan den blir offentlig för slutanvändarna.

## Lösning {#solution}

I motringningen `displayProviderDialog()` returnerar Adobe Pass-autentisering alla MVPD-filer som är integrerade med den valda programmeraren (begärande-ID). Men Programmeraren kan använda ett filter på MVPD-filens returmatris och bara visa de som finns i båda listorna.

## Exempel {#example}

I det här exemplet visas hur du endast visar CableCompany_1 och CableCompany_2 i MVPD-dialogrutan för väljare, och inte CableCompany_NewIntegration.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
