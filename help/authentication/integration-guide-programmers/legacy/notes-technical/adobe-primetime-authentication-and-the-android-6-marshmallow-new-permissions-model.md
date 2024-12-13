---
title: Adobe Pass Authentication and the Android 6 "Marshmallow" New Permissions Model
description: Adobe Pass Authentication and the Android 6 "Marshmallow" New Permissions Model
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# (Äldre) Adobe Pass Authentication and the Android 6 &quot;Marshmallow&quot; New Permissions Model {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

I den nya versionen av Android 6 Marshmallow introduceras vissa uppdateringar av behörighetsmodellen, vilket kan påverka beteendet för appar som använder den befintliga versionen av Adobe Pass Authentication SDK version 1.8 och äldre.

Som en ny funktion ger nya Android OS [detaljkontroll över de behörigheter som krävs för appar vid installationen och vid körning](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>Ändringarna som beskrivs nedan **påverkar bara program som utvecklats specifikt för Android 6.0** (targetSdkVersion=23). De påverkar inte äldre program som redan är installerade på användarens enhet vid uppgradering till Android 6.0.


För program som utvecklats i Android Studio med [API-nivå 23](http://developer.android.com/sdk/api_diff/23/changes.html) och som använder Adobe Pass Authentication SDK måste utvecklaren skriva egen kod (se kodutdrag nedan) [ för att aktivera dialogrutan Tillåt/Neka behörigheter](https://developer.android.com/training/permissions/requesting.html).

Här följer det kodutdrag som används för att begära skrivåtkomst till enhetens externa lagring:

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**I användarens perspektiv** kommer användare att bli välkomna av ett fönster som uppmanar dem att bekräfta läs- och skrivbehörigheter för filer (se figur 2 nedan). Detta leder till ett av följande två resultat:

1. Om användaren **bekräftar** behörigheterna behålls det reguljära autentiseringsflödet och tokens lagras i den globala lagringen. Användare förblir autentiserade i appen och mellan appar med Adobe Pass Authentication så länge tokenerna är giltiga.
1. Om användaren **nekar** behörigheter kommer skrivåtgärder i lagringsutrymmet att misslyckas och användarna autentiseras bara tills de avslutar programmet. Observera att vissa program återinitieras när de växlar mellan förgrund och bakgrund, så att användarna loggas ut när den här åtgärden utförs. Tokens lagras INTE och användarna måste autentisera varje gång de använder appen.


>[!TIP]
>
>En funktion som introducerar lagringsflexibilitet håller på att utvecklas för Adobe Pass Authentication SDK 1.9. Den nya SDK-versionen är avsedd för **version under den sista veckan i oktober**. Programmet återgår till att skriva i programmets sandlådelagring när den allmänna lagringen inte kan användas. Detta omfattar det fall där användare, för program som utvecklats på API-nivå 23, INTE accepterar läs-/skrivbehörigheter i den globala lagringen. Token lagras individuellt per app vilket innebär att enkel inloggning mellan program som använder Adobe Pass-autentisering inaktiveras.


![](../../../assets/android-permissions-request.png)

*Figur: Dialogrutan för behörighetsbegäran för program som skrivits med API-nivå 23*

>[!IMPORTANT]
>
> Adobe rekommenderar **sina partner att utveckla program med API-nivå 22 (targetSdkVersion=22) eller äldre för att garantera bästa möjliga användarupplevelse i autentiseringsprocessen**.
