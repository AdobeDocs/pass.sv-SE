---
title: Kanaler
description: Lär dig mer om kanaler och deras olika konfigurationer i TVE Dashboard.
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# Kanaler {#channels}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

I avsnittet **Kanaler** i TVE Dashboard kan du visa och hantera inställningar för de kanaler som är associerade med en viss programmerare. Du kan även [lägga till en ny kanal](#add-new-channel) efter dina behov.

På fliken **Kanaler** i den vänstra panelen visas en lista med länkade kanaler med följande information:

* **Visningsnamn**: Namnet på den kanal som används för kommersiella ändamål.
* **Kanal-ID**: En unik identifierare som även kallas begärande-ID.
* **Integrationer**: Antalet anslutningar som upprättats med [MVPD-filer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd).

![Lista över befintliga kanaler](../assets/tve-dashboard/new-tve-dashboard/channels/channels-list-view.png)

*Lista över befintliga kanaler*

Skriv namnet på kanalen i **sökfältet** ovanför listan om du vill veta mer om kanalen.

## Hantera kanalkonfigurationer {#manage-channel-conf}

Följ stegen för att hantera olika inställningar för en viss kanal.

1. Välj fliken **Kanaler** i den vänstra panelen.

1. Välj kanalen i listan över tillgängliga kanaler.

1. Välj någon av följande flikar om du vill visa och redigera motsvarande inställningar för den valda kanalen:

   * [Allmänna inställningar](#general-settings)
   * [Integreringar](#integrations)
   * [Certifikat](#certificates)
   * [Domäner](#domains)
   * [Registrerade program](#registered-applications)
   * [Anpassade scheman](#custom-schemes)

   ![Kanalinställningar](../assets/tve-dashboard/new-tve-dashboard/channels/channel-tabs-view.png)

   *Kanalinställningar*

>[!IMPORTANT]
>
> Visa [Granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) om du vill ha mer information om hur du aktiverar konfigurationsändringarna.

### Allmänna inställningar {#general-settings}

På den här fliken visas **Kanalinformation** och **Analyskonfiguration**.

#### Kanalinformation {#channel-information}

I det här avsnittet kan du redigera följande information:

* **Visningsnamn**: Namnet på den kanal som används för kommersiella ändamål.

* **Standardomdirigerings-URL**: Omdirigerings-URL för säkerhetskopiering för autentisering och utloggning.

* **Felrapportering**: När du väljer **Ja** skickar Adobe Pass SDK:er felrapporter till Adobe Pass backend för analys.

![Redigera kanalinformation](../assets/tve-dashboard/new-tve-dashboard/channels/channel-general-settings-tab-view.png)

*Redigera kanalinformation*

#### Analyskonfiguration {#analytics-configuration}

I det här avsnittet kan du konfigurera vidarebefordran av Adobe Pass Authentication-händelser till Adobe Analytics.

Om du vill aktivera **Analyskonfiguration** kontaktar du den tekniska kontohanteraren (TAM) för mer information om hur du konfigurerar Report Suite-ID (RSID).

![Aktivera analyskonfigurationer](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-analytics-configuration-button.png)

*Aktivera analyskonfigurationer*

Välj **Lägg till ny analyskonfiguration** om du vill lägga till flera konfigurationer.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda den nya analyskonfigurationen från avsnittet **Analyskonfiguration** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

### Integreringar {#integrations}

På den här fliken visas en lista med tillgängliga integreringar mellan den valda kanalen och de virtuella dokumentdokumenten. I listan visas varje integrering tillsammans med dess status, vilket anger om den är aktiverad eller inte. Välj en specifik integrering i den här listan för att få tillgång till detaljerad information i avsnittet [Integrationer](tve-dashboard-integrations.md).

![Lista över tillgängliga integreringar](../assets/tve-dashboard/new-tve-dashboard/channels/channel-integrations-tab-view.png)

*Lista över tillgängliga integreringar*

### Certifikat {#certificates}

På den här fliken visas en lista med [tillgängliga certifikat](#available-certificates) och [ärvda tillgängliga certifikat](#inherited-avail-certificates) som används i krypteringsflödena för användarmetadata. Den visar information om varje certifikat som innehåller:

* Status (om aktiverad för **användarens metadatakryptering** eller inte)
* Serienummer
* Emittentorganisationens namn
* Ämnesorganisationens namn
* Utfärdat den
* Utgångsdatum
* En listruta där användarmetadata krypteras (om du väljer **Ja** krypteras känslig användarinformation, t.ex. postkodsvärden).

#### Tillgängliga certifikat {#available-certificates}

Dessa certifikat fungerar som privata eller offentliga nycklar och används för kryptering av användarmetadata.
Du kan göra följande ändringar under det tillgängliga certifikatavsnittet:

* [Lägg till nytt certifikat](#add-new-certificate)
* [Ta bort certifikat](#delete-certificate)

##### Lägg till nytt certifikat {#add-new-certificate}

Så här lägger du till ett nytt certifikat:

1. Välj **Lägg till nytt certifikat** högst upp i avsnittet **Tillgängliga certifikat**.

   ![Lägg till ett nytt certifikat](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-certificate-button.png)

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

1. Hovra över det certifikat som du vill ta bort från listan över **tillgängliga certifikat**.

1. Välj **Ta bort**.

   ![Ta bort det markerade certifikatet](../assets/tve-dashboard/new-tve-dashboard/channels/channel-delete-certificate-button.png)

   *Ta bort det markerade certifikatet*

1. Välj **Ta bort** i dialogrutan **Ta bort aktivt certifikat**.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Certifikatet tas endast bort från avsnittet **Tillgängliga certifikat** efter [gransknings- och push-ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Ärvda tillgängliga certifikat {#inherited-avail-certificates}

Medieföretag definierar dessa certifikat på sin egen nivå. Alla kanaler som är associerade med samma medieföretag kan använda dessa certifikat.

![Ärvda tillgängliga certifikat](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-available-certificates-panel-view.png)

*Ärvda tillgängliga certifikat*

### Domäner {#domains}

På den här fliken visas en lista över tillgängliga domäner som respektive kanal kommunicerar med Adobe Pass-autentisering genom.

Du kan göra följande ändringar i domäner:

* [Lägg till en ny domän](#add-domains)
* [Ta bort domän](#delete-domain)

>[!TIP]
>
> Undvik att lägga till en ny underdomän om det finns en mer allmän domän i listan.

#### Lägg till ny domän {#add-domains}

Följ de här stegen för att lägga till en domän.

1. Välj **Lägg till ny domän** längst upp till höger i avsnittet **Tillgängliga domäner**.

   ![Lägg till en ny domän](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-domain-button.png)

   *Lägg till en ny domän*

1. Skriv namnet på din domän i dialogrutan **Ny domän**.

1. Välj **Lägg till domän** om du vill lägga till en ny domän för den valda kanalen.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda den nya domänen som visas i avsnittet **Tillgängliga domäner** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Ta bort domän {#delete-domain}

Så här tar du bort en domän.

1. Hovra över den domän som du vill ta bort från listan över **tillgängliga domäner**.

1. Välj **Ta bort**.

   ![Ta bort den markerade domänen](../assets/tve-dashboard/new-tve-dashboard/channels/channel-remove-domain-button.png)

   *Ta bort den markerade domänen*

1. Välj **Ta bort** i dialogrutan **Ta bort domän**.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Domänen tas endast bort från avsnittet **Tillgängliga domäner** efter [granskning och push-ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

Den valda domänen är inte längre tillgänglig för användning. Därför förlorar programmet som är kopplat till den här domänen åtkomsten till Adobe Pass autentiseringstjänster.

### Registrerade program {#registered-applications}

På den här fliken visas en lista med registrerade program. Mer information om användning av registrerade program finns i dokumentationen om [dynamisk klientregistrering](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Du kan göra följande med registrerade program:

* [Lägg till ett nytt registrerat program](#add-registered-applications)
* [Ladda ned en programsats](#download-software-statement)

#### Lägg till nytt registrerat program {#add-registered-applications}

Följ de här stegen för att lägga till ett nytt registrerat program.

1. Välj **Lägg till nytt program** längst upp till höger i avsnittet **Registrerade program**.

   ![Lägg till ett nytt program](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-application-button.png)

   *Lägg till ett nytt program*

1. Välj **Plattformar** i listrutan i dialogrutan **Nytt program** .

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

1. Hovra över det registrerade programmet som du vill hämta programsatsen från listan med **registrerade program**.

1. Välj **Hämta**.

   ![Hämta en programsats](../assets/tve-dashboard/new-tve-dashboard/channels/channel-download-software-statement-button.png)

   *Hämta en programsats*

### Anpassade scheman {#custom-schemes}

På den här fliken visas en lista med anpassade scheman. Mer information om användning av anpassade scheman finns i [iOS/tvOS-programregistreringen](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

Du kan göra följande ändringar i anpassade scheman:

* [Generera ett nytt anpassat schema](#generate-custom-schemes)

#### Generera nytt anpassat schema {#generate-custom-schemes}

Följ de här stegen för att skapa ett nytt anpassat schema.

1. Välj **Skapa nytt anpassat schema**.

   ![Skapa ett nytt anpassat schema](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-custom-scheme-button.png)

   *Skapa ett nytt anpassat schema*

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda det nya anpassade schemat som listas i avsnittet **Anpassade scheman** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Ärvda anpassade scheman {#inherited-custom-schemes}

Medieföretag definierar dessa anpassade scheman på sin egen nivå. Alla kanaler som är associerade med samma medieföretag kan använda dessa anpassade scheman.

![Ärvda anpassade scheman](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-custom-schemes-panel-view.png)

*Ärvda anpassade scheman*

## Lägg till ny kanal {#add-new-channel}

Följ de här stegen för att lägga till en ny kanal.

1. Välj fliken **Kanaler** i den vänstra panelen.

1. Välj **Lägg till ny kanal** längst upp till höger i avsnittet **Kanaler**.

   ![Lägg till en ny kanal](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-channel-button.png)

   *Lägg till en ny kanal*

1. Välj **Program-ID** i listrutan i dialogrutan **Ny kanal**.

1. Ange en unik identifierare i **Channel ID**.

1. Skriv kanalens varumärkesnamn för kommersiellt bruk i **visningsnamnet**.

1. Välj **Lägg till kanal**.

En ny konfigurationsändring har skapats och är klar för serveruppdatering. Om du vill använda den nya kanalen som visas i avsnittet **Kanaler** fortsätter du med flödet [granska och skicka ändringar](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).
