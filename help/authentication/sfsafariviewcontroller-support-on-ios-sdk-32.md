---
title: Stöd för SFSafariViewController i iOS SDK 3.2+
description: Stöd för SFSafariViewController i iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Stöd för SFSafariViewController i iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>


**På grund av säkerhetskrav MÅSTE vissa MVPD:s inloggningssidor visas i en SFSafariViewController i stället för i webbvyer.**

Vissa MVPD-program kräver att deras inloggningssidor visas i en säker webbläsarkontroll som SFSafariViewController. De blockerar aktivt webbvyer, så för att kunna autentisera med dem måste vi använda SVC.

## Kompatibilitet {#compatiblity}

Från och med iOS SDK version 3.1 visar AccessEnabler SDK automatiskt inloggningssidan för specifika MVPD:er i en SFSafariViewController, baserat på serverkonfigurationen.

Version 3.1 av SDK visar automatiskt SFSafariViewController från programmets rotvykontroll. Detta förenklar hanteringen av inloggningssidor för implementerare, men det finns fall där det inte är möjligt att presentera SFSafariViewController från rotvystyrenheten på grund av att appens specialiseringsimplementering (som en modal styrenhet som redan är synlig).

I sådana fall ger version 3.2 programmeraren möjlighet att hantera SVC manuellt.

## Manuell SVC-hantering {#manual-svc-management}

För att manuellt kunna hantera SVC måste implementeraren utföra följande steg:


1. ring **setOptions([&quot;handleSVC&quot;:true])** efter initiering av AccessEnabler (kontrollera att anropet utförs innan autentiseringen börjar). Detta aktiverar&quot;manuell&quot; SVC-hantering, SDK visar inte automatiskt SVC, utan anropar istället vid behov **navigate(toUrl:*{url}* useSVC:true)**.

1. implementera det valfria återanropet **navigateToUrl:useSVC:** I implementeringen måste du skapa en svc-instans med SFSafariViewController-instansen med den angivna url:en och visa den på skärmen:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Anteckningar:***

   - *Du kan anpassa SFSafariViewController precis som du vill. På iOS 11+ kan du till exempel ändra etiketten Klar till Avbryt.*
   - *för att kunna avfärda SVC:n behöver du en referens till den. Skapa den inte inom ramen för **navigateToUrl:useSVC***
   - *använd din egen vykontroll för &quot;myController&quot;*


1. I programmets delegatimplementering av **application(\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, lägger du till kod för att stänga svc. Du bör redan ha kod där som anropar **accessEnabler.handleExternalURL()**. Lägg till nedan:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Även här är svc en referens till den SFSafariViewController som du skapade i steg 2.


1. Implementera **safariViewControllerDoFinish(\_ kontrollenhet: SFSafariViewController)** från **SFSafariViewControllerDelegate** för att kunna fånga upp när användaren avbröt SVC med hjälp av knappen Klar. I den här funktionen måste du anropa följande för att informera SDK om att autentiseringen har avbrutits:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
