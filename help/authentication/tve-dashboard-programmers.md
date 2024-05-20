---
title: Programmerare
description: Lär dig mer om programmerare och dess konfigurationer på TVE-kontrollpanelen.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programmerare {#programmers}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

The **Programmerare** i TVE Dashboard kan du visa och hantera inställningar för [programmerare](/help/authentication/glossary.md#programmer) länkade till dina kontoberättiganden. Du kan också [lägg till en ny programmerare](#add-new-programmer) efter dina behov.

The **Programmerare** i den vänstra panelen visas en lista med befintliga programmerare med följande information:

* **Programmer-ID**: En identifierare för medieföretag i systemet.
* **Kanaler**: Antalet associerade kanaler som är länkade till en programmerare.

![Lista över befintliga programmerare](assets/programmers-list.png)

*Lista över befintliga programmerare*

Skriv namnet på programmeraren i **Sök** om du vill veta mer om en programmerare.

## Hantera programmerarkonfigurationer {#manage-programmer-conf}

Följ de här stegen för att hantera olika inställningar för en viss programmerare.

1. Välj **Programmerare** i den vänstra panelen.
1. Välj en programmerare i listan.
1. Välj någon av följande flikar om du vill visa och redigera motsvarande inställningar för den valda programmeraren:

   * [Kanaler](#channels)
   * [Certifikat](#certificates)
   * [Registrerade program](#registered-applications)
   * [Anpassade scheman](#custom-schemes)

   ![Programmeringsinställningar](assets/programmer-settings.png)

   *Programmeringsinställningar*

>[!IMPORTANT]
>
> Visa [Granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md) om du vill ha mer information om hur du aktiverar konfigurationsändringarna.

### Kanaler {#channels}

På den här fliken visas en lista med kanaler som är länkade till en aktuell programmerare. Välj en specifik kanal i den här listan för att få tillgång till detaljerad information i [Kanaler](/help/authentication/tve-dashboard-channels.md) -avsnitt.

Om du vill lägga till en ny kanal för den valda programmeraren väljer du **Lägg till ny kanal** från det övre högra hörnet av **Tillgängliga kanaler** -avsnitt. Läs [lägga till en ny kanal](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Lägg till en ny kanal](assets/programmers-channels.png)

*Lägg till en ny kanal*

### Certifikat {#certificates}

På den här fliken visas en lista med [tillgängliga certifikat](#available-certificates) används i krypteringsflödena för användarmetadata. Den visar information om varje certifikat som innehåller:

* Status (om aktiverad för **kryptering av användarmetadata** eller inte)
* Serienummer
* Emittentorganisationens namn
* Ämnesorganisationens namn
* Utfärdat den
* Utgångsdatum
* En nedrullningsbar meny för kryptering av användarmetadata (om du väljer **Ja** krypterar certifikatet känslig användarinformation, t.ex. postkodsvärden).

#### Tillgängliga certifikat {#available-certificates}

Dessa certifikat fungerar som privata eller offentliga nycklar och används för kryptering av användarmetadata. Alla kanaler som är associerade med samma medieföretag kan använda dessa certifikat.

Du kan göra följande ändringar i tillgängliga certifikat:

* [Lägg till nytt certifikat](#add-new-certificate)
* [Ta bort certifikat](#delete-certificate)

##### Lägg till nytt certifikat {#add-new-certificate}

Följ de här stegen för att lägga till ett nytt certifikat.

1. Välj **Lägg till nytt certifikat** längst upp till höger i **Tillgängliga certifikat** -avsnitt.

   ![Lägg till ett nytt certifikat](assets/programmer-add-new-certificate.png)

   *Lägg till ett nytt certifikat*

1. Klistra in den offentliga nyckeln för certifikatet i **Nytt certifikat** -dialogrutan.
1. Välj **Lägg till certifikat**.

   En ny konfigurationsändring har skapats och är klar för serveruppdatering. Så här använder du det nya certifikatet som finns i **Tillgängliga certifikat** fortsätter du med [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md) flöde.

1. Leta reda på det nya certifikatet i listan med **Tillgängliga certifikat**.

   >[!IMPORTANT]
   >
   > Kontrollera att dina system är uppdaterade och klara att använda det nya certifikatet.

1. Välj **Ja** från **Används för att kryptera användarmetadata** för att aktivera ett nytt certifikat.

##### Ta bort certifikat {#delete-certificate}

Så här tar du bort ett certifikat.

1. Hovra över det certifikat som du vill ta bort från listan med **Tillgängliga certifikat**.
1. Välj **Ta bort**.

   ![Ta bort det markerade certifikatet](assets/programmer-remove-certificate.png)

   *Ta bort det markerade certifikatet*

1. Välj **Ta bort** på **Ta bort certifikat** -dialogrutan.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Certifikatet tas bort från **Tillgängliga certifikat** endast efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

### Registrerade program {#registered-applications}

På den här fliken finns en lista med programregistreringar. Visa [Dynamisk hantering av klientregistrering](/help/authentication/dynamic-client-registration-management.md), för mer information.

### Anpassade scheman {#custom-schemes}

På den här fliken visas en lista med anpassade scheman. Visa [Registrering av iOS/tvOS](/help/authentication/iostvos-application-registration.md) och [Dynamisk hantering av klientregistrering](/help/authentication/dynamic-client-registration-management.md), för mer information.

## Lägg till ny programmerare {#add-new-programmer}

Följ de här stegen för att lägga till en ny programmeringsenhet.

1. Välj **Programmerare** i den vänstra panelen.
1. Välj **Lägg till ny programmerare** längst upp till höger i **Programmerare** -avsnitt.

   ![Lägg till en ny programmerare](assets/add-new-programmer.png)

   *Lägg till en ny programmerare*

1. Ange medieföretagsidentifierare i **Programmer-ID** i **Ny programmerare** -dialogrutan.
1. Ange ett varumärke som du vill ska visas i konsolen under **Visningsnamn**.
1. Välj **Lägg till programmerare**.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Använda den nya programmeraren som listas i **Programmerare** fortsätter du med [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md) flöde.

