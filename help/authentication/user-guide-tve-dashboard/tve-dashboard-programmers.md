---
title: Programmerare
description: Lär dig mer om programmerare och dess konfigurationer på TVE-kontrollpanelen.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Programmerare {#programmers}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

Med avsnittet **Programmerare** i TVE Dashboard kan du visa och hantera inställningar för de [programmerare](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) som är länkade till dina kontoberättiganden. Du kan även [lägga till en ny programmerare](#add-new-programmer) efter dina behov.

Fliken **Programmerare** i den vänstra panelen visar en lista med befintliga programmerare med följande information:

* **Program-ID**: En medieföretags-ID i systemet.
* **Kanaler**: Antalet associerade kanaler som är länkade till en programmerare.

![Lista över befintliga programmerare](../assets/tve-dashboard/new-tve-dashboard/programmers/programmers-list-view.png)

*Lista över befintliga programmerare*

Skriv namnet på programmeraren i fältet **Sök** ovanför listan om du vill veta mer om en programmerare.

## Hantera programmerarkonfigurationer {#manage-programmer-conf}

Följ de här stegen för att hantera olika inställningar för en viss programmerare.

1. Välj fliken **Programmerare** i den vänstra panelen.
1. Välj en programmerare i listan.
1. Välj någon av följande flikar om du vill visa och redigera motsvarande inställningar för den valda programmeraren:

   * [Kanaler](#channels)
   * [Certifikat](#certificates)
   * [Registrerade program](#registered-applications)
   * [Anpassade scheman](#custom-schemes)

   ![Programmeringsinställningar](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-tabs-view.png)

   *Programmeringsinställningar*

>[!IMPORTANT]
>
> Visa [Granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) om du vill ha mer information om hur du aktiverar konfigurationsändringarna.

### Kanaler {#channels}

På den här fliken visas en lista med kanaler som är länkade till en aktuell programmerare. Välj en specifik kanal i den här listan för att få tillgång till detaljerad information i avsnittet [Kanaler](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md).

Om du vill lägga till en ny kanal för den valda programmeraren väljer du **Lägg till ny kanal** i det övre högra hörnet av avsnittet **Tillgängliga kanaler**. Lär dig [hur du lägger till en ny kanal](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#add-new-channel).

![Lägg till en ny kanal](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-channel-button.png)

*Lägg till en ny kanal*

### Certifikat {#certificates}

På den här fliken visas en lista med [tillgängliga certifikat](#available-certificates) som används i krypteringsflödena för användarmetadata. Den visar information om varje certifikat som innehåller:

* Status (om aktiverad för **användarens metadatakryptering** eller inte)
* Serienummer
* Emittentorganisationens namn
* Ämnesorganisationens namn
* Utfärdat den
* Utgångsdatum
* En nedrullningsbar meny för kryptering av användarmetadata (om du väljer **Ja** krypteras känslig användarinformation, till exempel postkodsvärden).

#### Tillgängliga certifikat {#available-certificates}

Dessa certifikat fungerar som privata eller offentliga nycklar och används för kryptering av användarmetadata. Alla kanaler som är associerade med samma medieföretag kan använda dessa certifikat.

Du kan göra följande ändringar i tillgängliga certifikat:

* [Lägg till nytt certifikat](#add-new-certificate)
* [Ta bort certifikat](#delete-certificate)

##### Lägg till nytt certifikat {#add-new-certificate}

Följ de här stegen för att lägga till ett nytt certifikat.

1. Välj **Lägg till nytt certifikat** längst upp till höger i avsnittet **Tillgängliga certifikat**.

   ![Lägg till ett nytt certifikat](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-certificate-button.png)

   *Lägg till ett nytt certifikat*

1. Klistra in den offentliga nyckeln för ditt certifikat i dialogrutan **Nytt certifikat**.

1. Välj **Lägg till certifikat**.

1. Leta reda på det nya certifikatet i listan med **tillgängliga certifikat**.

   >[!IMPORTANT]
   >
   > Kontrollera att dina system är uppdaterade och klara att använda det nya certifikatet.

1. Välj **Ja** i listrutan **Används för krypterade användarmetadata** för att aktivera ett nytt certifikat.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda det nya certifikatet som visas i avsnittet **Tillgängliga certifikat** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

##### Ta bort certifikat {#delete-certificate}

Så här tar du bort ett certifikat.

1. Hovra över det certifikat som du vill ta bort från listan med **tillgängliga certifikat**.

1. Välj **Ta bort**.

   ![Ta bort det markerade certifikatet](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-remove-certificate-button.png)

   *Ta bort det markerade certifikatet*

1. Välj **Ta bort** i dialogrutan **Ta bort certifikat**.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Certifikatet tas endast bort från avsnittet **Tillgängliga certifikat** efter [gransknings- och push-ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

### Registrerade program {#registered-applications}

På den här fliken visas en lista med registrerade program. Mer information om användning av registrerade program finns i dokumentationen om [dynamisk klientregistrering](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Du kan göra följande med registrerade program:

* [Lägg till ett nytt registrerat program](#add-registered-applications)
* [Ladda ned en programsats](#download-software-statement)

#### Lägg till nytt registrerat program {#add-registered-applications}

Följ de här stegen för att lägga till ett nytt registrerat program.

1. Välj **Lägg till nytt program** längst upp till höger i avsnittet **Registrerade program**.

   ![Lägg till ett nytt program](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-application-button.png)

   *Lägg till ett nytt program*

1. Välj **Tilldelad till kanal** i listrutan i dialogrutan **Nytt program** .

   >[!IMPORTANT]
   >
   > Vi rekommenderar att du skapar registrerade program med mer specifika och begränsade behörigheter för att förbättra säkerheten och förhindra obehörig åtkomst. När du skapar registrerade program bör du därför överväga att använda smalare alternativ för den tilldelade `channel`.

1. Välj **Plattformar** i listrutan.

   >[!IMPORTANT]
   >
   > Vi rekommenderar att du skapar registrerade program med mer specifika och begränsade behörigheter för att förbättra säkerheten och förhindra obehörig åtkomst. När du skapar registrerade program bör du därför överväga att använda smalare alternativ för den tilldelade `platforms`.

1. Välj **Domäner** i listrutan.

   >[!IMPORTANT]
   >
   > I klientregistreringsprocessen kan klientprogrammet begära att få använda en omdirigerings-URL för att slutföra autentiseringsflödet. När ett klientprogram använder en specifik omdirigerings-URL valideras den mot de `domains` som valts i det här urvalet.

1. Skriv programmets **namn**.

1. Skriv programmets **version**.

   >[!IMPORTANT]
   >
   > Vi rekommenderar att du skapar ett nytt registrerat program för varje större uppdatering av klientprogrammet för att hantera dess livscykel och användning. Om det behövs skapar du en biljett via vår [Zendesk](https://adobeprimetime.zendesk.com) och ber din tekniska kontohanterare (TAM) att återkalla ett registrerat program för att blockera funktionaliteten i en viss klientprogramversion.

1. Välj **Type**-värdet DIRECT i listrutan.

1. Välj **Lägg till program**.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda det nya registrerade programmet som listas i avsnittet **Registrerade program** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Ladda ned programsats {#download-software-statement}

Följ de här stegen för att hämta en programsats.

1. Håll muspekaren över det registrerade programmet som du vill hämta programsatsen från listan över **registrerade program**.

1. Välj **Hämta**.

   ![Hämta en programsats](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-download-software-statement-button.png)

   *Hämta en programsats*


### Anpassade scheman {#custom-schemes}

På den här fliken visas en lista med anpassade scheman. Mer information om användning av anpassade scheman finns i [iOS/tvOS-programregistreringen](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

Du kan göra följande ändringar i anpassade scheman:

* [Generera ett nytt anpassat schema](#generate-custom-schemes)

#### Generera nytt anpassat schema {#generate-custom-schemes}

Följ de här stegen för att skapa ett nytt anpassat schema.

1. Välj **Skapa nytt anpassat schema**.

   ![Skapa ett nytt anpassat schema](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-custom-scheme-button.png)

   *Skapa ett nytt anpassat schema*

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda det nya anpassade schemat som listas i avsnittet **Anpassade scheman** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

## Lägg till ny programmerare {#add-new-programmer}

Följ de här stegen för att lägga till en ny programmeringsenhet.

1. Välj fliken **Programmerare** i den vänstra panelen.

1. Välj **Lägg till ny programmerare** längst upp till höger i avsnittet **Programmerare**.

   ![Lägg till en ny programmerare](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-programmer-button.png)

   *Lägg till en ny programmerare*

1. Ange medieföretagsidentifierare i **Program-ID** i dialogrutan **Ny programmerare**.

1. Skriv ett varumärke som du vill ska visas i konsolen under **Visningsnamn**.

1. Välj **Lägg till programmerare**.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda den nya programmeraren som listas i avsnittet **Programmerare** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).
