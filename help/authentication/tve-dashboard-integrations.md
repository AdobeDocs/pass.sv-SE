---
title: Integrering med TVE Dashboard
description: Lär dig mer om integreringarna mellan era kanaler och distributörer av videoprogrammeringstjänster och hur ni hanterar integreringar.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# Integreringar

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

The **Integreringar** på TVE Dashboard kan du visa och hantera inställningar för integreringar mellan dina kanaler och distributörer av videoprogrammeringstjänster. Du kan också [skapa en ny integrering](#create-new-integration) efter dina behov.

The **Integreringar** på den vänstra panelen visas en lista med befintliga integreringar med följande information:

* Status som anger om integreringen är aktiv eller inaktiv
* Integrering som länkar specifika kanaler till respektive videoprogrammeringsprogram
* Kanalnamn med kanal-ID
* MVPD-visningsnamn och MVPD ID

![Lista över befintliga integreringar](assets/integrations-list.png)

*Lista över befintliga integreringar*

Skriv namnet på kanalen eller den virtuella dokumentationsfilen i **Sök** om du vill veta mer om integreringen.

## Hantera integrationskonfigurationer {#manage-integration-conf}

Följ de här stegen för att hantera en specifik integrering.

1. Välj **Integreringar** i den vänstra panelen.
1. Välj en integrering i listan om du vill visa och redigera olika inställningar i följande avsnitt:

   * [Val av slutpunkt](#endpoint-selection)
   * [Plattformsinställningar](#platform-settings)
   * [Användarmetadata](#user-metadata)

>[!IMPORTANT]
>
> Visa [Granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md) om du vill ha mer information om hur du aktiverar konfigurationsändringarna.

### Val av slutpunkt {#endpoint-selection}

I det här avsnittet kan du välja slutpunkter för det MVPD som används för autentiserings-, auktoriserings- och utloggningsflöden från respektive listruta.

![Slutpunkter för autentisering, auktorisering och utloggningsflöden](assets/endpoint-selection.png)

*Slutpunkter för autentisering, auktorisering och utloggningsflöden*

>[!NOTE]
>
>MVPD-program kan tillhandahålla en eller flera slutpunkter för varje flöde. När du integrerar en ny kanal måste MVPD ange vilken slutpunkt de föredrar för varje flöde.

>[!IMPORTANT]
>
>Alla ändringar av slutpunkter påverkar det övergripande beteendet för en integrering. Dessa ändringar ska endast implementeras efter att ha fått bekräftelse från det mobila dokumentationsdokumentet.

### Plattformsinställningar {#platform-settings}

I det här avsnittet kan du visa och redigera integreringsinställningar för alla [plattformar](/help/authentication/tve-dashboard-reports.md#platforms). Du kan ändra de här inställningarna baserat på enskilda plattformar. Du kan till exempel justera TTL-längden för auktorisering på Android samtidigt som du behåller standardvärdet för en annan plattform.

Varje egenskap i plattformsinställningarna ärver ett standardvärde som angetts av MVPD, men kan justeras om det behövs.

>[!IMPORTANT]
>
>Ett avtal med MVPD krävs för att fastställa de värden som anges för varje egenskap i plattformsinställningarna.

>[!IMPORTANT]
>
> Inställningsarvet följer en kedja som börjar med MVPD-inställningarna (som är de mest allmänna) och sedan MVPD-slutpunkten, integrering, plattformskategori och plattform (som innehåller det mest specifika värdet).

**Plattformsinställningar** används för att åsidosätta inställningar för varje nivå i arvskedjan. De tillgängliga nivåerna i kedjan är grupperade enligt följande:

* **Standard för alla**: Ange värden för egenskaper som ska gälla överallt på alla plattformar om specifika plattformsvärden inte definieras, oavsett programmerarens implementeringar.

* **Skrivbordsenheter**: Ange värden för egenskaper som gäller för alla stationära och bärbara datorer, oavsett programmeringsmetod (JS SDK eller REST API).

* **Mobila enheter**: Ange värden för egenskaper som gäller för alla mobila enheter, inklusive **iOS**, **Android** och andra, oavsett programmeringsmetod (SDK eller REST API).

* **TV-anslutna enheter**: Ange värden för egenskaper som gäller för alla tv-anslutna enheter, inklusive **tvOS**, **Roku**, **FireTV** och andra, oavsett programmeringsmetod (SDK eller REST API).

* **Oidentifierade enheter**: Ange värden för egenskaper som gäller för alla enheter där den aktuella mekanismen inte kan identifiera plattformen korrekt. I sådana fall tillämpar du de mest restriktiva regler som definieras av det enhetliga upphandlingsdokumentet.

  ![Plattformskategorier och deras enheter](assets/platform-settings.png)

  *Plattformskategorier och deras enheter*

Välj <img alt= "arvskedjeikon" src="./assets/multiple-icon.svg" width="25"> ikonen till höger om varje egenskap för att utforska de egenskaper som används för varje arvsnivå som beskrivs ovan.

#### De mest använda affärsflödena {#most-used-flows}

The **Plattformsinställningar** -avsnittet erbjuder en rad egenskaper som används i olika affärsflöden. De faktiska egenskaperna kan variera beroende på vilka MVPD-program som har valts i den specifika integreringen. Nedan visas de mest använda flödena:

**AuthN TTL och AuthZ TTL över alla plattformar**

>[!IMPORTANT]
>
>TTL-värden för autentisering (AuthN) och auktorisering (AuthZ) måste vara konsekvent justerade mot MVPD-inställningarna.

Följ de här stegen för att ändra autentiserings- och auktoriserings-TTL på alla plattformar för en viss integrering.

1. Välj **Integreringar** i den vänstra panelen.
1. Välj den integrering som du vill ändra AuthN TTL- och AuthZ TTL-värden för.
1. Navigera till **Plattformsinställningar** -avsnitt.

1. Välj **Standard för alla** flik under **Plattformsinställningar**.

   >[!NOTE]
   >
   >Om du vill ändra längden på **AuthN TTL** och **AuthZ TTL** för en plattformskategori eller en viss plattform väljer du plattform.

   ![Ändra AuthN TTL AuthZ TTL-varaktighet på alla plattformar](assets/authn-ttl-authz-ttl-for-all-platform.png)

   *Ändra AuthN TTL AuthZ TTL-varaktighet på alla plattformar*

   **S.** AuthN TTL-egenskap **B.** AuthZ TTL-egenskap

1. Markera uppåt- och nedåtpilarna för att justera varaktigheten för antalet dagar, timmar, minuter och sekunder i **AuthN TTL** och **AuthZ TTL** egenskaper.

Varaktigheten för **AuthN TTL** och **AuthZ TTL** på alla plattformar uppdateras först efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

**Aktivera plattformens SSO**

>[!IMPORTANT]
>
>**Aktivera enkel inloggning** egenskapen stöds endast på *iOS, tvOS, Roku och FireTV* -plattformar. Det gäller endast för integreringar med MVPD-program som stöder enkel inloggning för dessa plattformar.

Följ de här stegen för att aktivera eller inaktivera enkel inloggning för en specifik integrering och plattform.

1. Välj **Integreringar** i den vänstra panelen.
1. Välj den integrering som du vill aktivera eller inaktivera enkel inloggning för.

1. Navigera till **Plattformsinställningar** -avsnitt.

1. Välj en specifik plattform eller plattformskategori för vilken du vill aktivera enkel inloggning under **Plattformsinställningar**.

   ![Aktivera enkel inloggning för en viss plattform](assets/single-sign-on.png)

   *Aktivera enkel inloggning för en viss plattform*

   **S.** Enkel inloggning, egenskap **B.** Använd plattformsbehörigheter, egenskap

1. Välj **Ja** för att aktivera eller **Nej** för att inaktivera från **Aktivera enkel inloggning** listrutemeny.

1. Välj **Ja** för att aktivera eller **Nej** för att inaktivera från **Tvinga plattformsbehörighet** listrutemeny.

   **Tvinga plattformsbehörighet** egenskapen kontrollerar om användaren väljer att **Tillåt** eller **Neka** plattformsåtkomst till deras TV-leverantörsprenumeration respekteras.

   Om till exempel både **Aktivera enkel inloggning** och **Tvinga plattformsbehörighet** är aktiverade och användaren väljer att neka plattformsåtkomst till sin TV-leverantörsprenumeration, så att respektive program (kanal) inte kan använda den Adobe Pass-autentiseringstoken som erhålls av ett annat program (kanal).

The **Enkel inloggning** egenskapen för en vald plattform aktiveras eller inaktiveras först efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

**Aktivera hembaserad autentisering**

Följ de här stegen för att aktivera eller inaktivera hembaserad autentisering för OAuth2-baserade MVPD-filer.

1. Välj **Integreringar** i den vänstra panelen.
1. Välj den integrering som du vill aktivera eller inaktivera hembaserad autentisering för.
1. Navigera till **Plattformsinställningar** -avsnitt.
1. Välj en specifik plattform eller plattformskategori för vilken du vill aktivera hembaserad autentisering under **Plattformsinställningar**.

   ![Aktivera hembaserad autentisering för en viss plattform](assets/attempt-hba.png)

   *Aktivera hembaserad autentisering för en viss plattform*

   **S.** Attempt HBA property **B.** HBA AuthN TTL-egenskap

1. Välj **Ja** aktivera och **Nej** för att inaktivera från **Attempt HBA** listrutemeny.

>[!IMPORTANT]
>
>Ändra längden på **HBA AuthN TTL** Egendom bör undvikas. Det kan potentiellt leda till oväntade fel i auktoriseringsprocessen.

The **Attempt HBA** egenskapen för en viss MVPD aktiveras eller inaktiveras först efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

#### Lägg till fler egenskaper {#add-more-properties}

The **Lägg till fler egenskaper** ger flexibilitet att inkludera ytterligare specifika egenskaper för integreringar, särskilt för mindre vanliga flöden.

Du kan lägga till följande egenskaper:

* Välj för alla plattformar **Standard för alla** till vänster.
* Välj en plattformskategori **Skrivbordsenheter**, **Mobila enheter**, eller **TV-anslutna enheter** till vänster.
* För en viss enhet väljer du **iOS**, **Android**, **tvOS**, **Roku**, eller **FireTV** till vänster.

Här är några exempel på olika flöden som kan aktiveras genom att du lägger till dessa egenskaper:

**Ändra antalet förauktoriserade resurser**

De flesta MVPD-program har stöd för ett preflight-authZ-anrop med upp till 5 resurs-ID:n som standard.
I de fall då distributörer av videoprogrammeringstjänster accepterar att höja denna gräns kan du navigera till **Lägg till fler egenskaper** och markera **Max preflight-resurser** på Alternativ-menyn.

**Max preflight-resurser** lägger till ett nytt attribut där den avtalade gränsen med MVPD kan anges.

![Lägg till egenskapen Preflight Max Resources](assets/preflight-max-resources.png)

*Lägg till egenskapen Preflight Max Resources*

The **Max preflight-resurser** egenskapen läggs bara till efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

**Ändra visningsnamn eller logotyp för MVPD**

För programmerarprogram som inte vill bygga sin MVPD-väljare och i stället förlitar sig på de konfigurationer som tillhandahålls kan du navigera till **Lägg till fler egenskaper** och markera **Visningsnamn** eller **URL för logotyp** om du vill lägga till det visningsnamn eller den logotyp som krävs för varje MVPD på Alternativ-menyn.

Olika värden för dessa egenskaper kan användas för samma MVPD beroende på enhetsplattform och önskad användarupplevelse.

![Lägg till visningsnamn eller URL-egenskap för logotyp](assets/displayname-logourl.png)

*Lägg till visningsnamn eller URL-egenskap för logotyp*

The **Visningsnamn** eller **URL för logotyp** egenskapen läggs bara till efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

**Begär ett nytt autentiseringsflöde vid växling av program (kanal)**

Om du vill framtvinga en ny autentisering när användare växlar mellan appar. I så fall kan du navigera till **Lägg till fler egenskaper** väljer du **Autentisering per aggregator** -egenskap.

Lägger till **Autentisering per aggregator** avbryter enkel inloggning för respektive kanal.

![Lägg till autentisering per aggregator, egenskap](assets/auth-per-aggregator.png)

*Lägg till autentisering per aggregator, egenskap*

The **Autentisering per aggregator** egenskapen läggs bara till efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

Välj **Ja** aktivera **Autentisering per aggregator** -egenskap för en vald integrering.

#### Ta bort egenskaper {#delete-properties}

Välj <img alt= "delete, egenskapsknapp" src="./assets/delete-icon.svg" width="25"> -ikonen till höger om varje egenskap för att ta bort de egenskaper som inte längre behövs.

>[!NOTE]
>
>Vissa egenskaper kan inte tas bort eftersom de är obligatoriska krav för det valda MVPD-programmet.

Egenskapen tas bort från **Plattformsinställningar** endast efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

### Användarmetadata {#user-metadata}

I det här avsnittet kan du uppdatera inställningarna för varje användarmetadataparameter som delas av MVPD.

>[!NOTE]
>
>Varje MVPD kan ha olika parametrar. Mer information om parametrarna som ett visst MVPD kan dela får du av din Adobe-representant.

I avsnittet med användarens metadata visas följande kolumner:

**Nyckel**: Representerar de faktiska parametrarna för användarmetadata som ska användas i API:t för att extrahera värden.

**Beskrivning**: Ger en kort beskrivning av varje användarmetadataparameter.

**Krypterad**: Med den här kolumnen kan du aktivera eller inaktivera parametrar i API:t genom att välja **Ja** eller **Nej** i listrutan. Välj för **Ja** anger att parametervärdet kommer att krypteras i API:t. Krypteringen utförs med ett certifikat som definieras av en **Användarmetadata** omfång.

>[!TIP]
>
>
> Se alltid till att **ZIP** parametern är krypterad.

Läs mer om tillgängliga certifikat i [Programmerare](/help/authentication/tve-dashboard-programmers.md#available-certificates) och [Kanaler](/help/authentication/tve-dashboard-channels.md#available-certificates) -avsnitt.

**Aktiverad**: I den här kolumnen kan du aktivera eller inaktivera parametrar i API genom att välja **Ja** eller **Nej** i listrutan.

![Tillgängliga parametrar för användarmetadata](assets/user-metadata.png)

*Tillgängliga parametrar för användarmetadata*

## Skapa ny integrering {#create-new-integration}

Så här skapar du en ny integrering med ett nytt MVPD-program i den aktuella konfigurationen:

1. Välj **Integreringar** i den vänstra panelen.
1. Välj **Skapa ny integrering** längst upp till höger på sidan **Integreringar** -avsnitt.

   ![Skapa en ny integrering](assets/create-new-integration.png)

   *Skapa en ny integrering*

   Följande avsnitt visas:

   **Välj kanal och MVPD**

   Välj en **Kanal** från **Välj kanal** listrutemeny för att lägga till en ny integrering. När du har markerat kanalen väljer du önskat **MVPD** från **Välj MVPD** listrutemeny som ska integreras med den valda kanalen.

   ![Välj kanal och MVPD](assets/select-channel-mvpd.png)

   *Välj kanal och MVPD*

   **Markera slutpunkter**

   När du har valt önskat MVPD, **Markera slutpunkt**  -avsnittet fylls i automatiskt med standardslutpunkterna som är konfigurerade för det aktuella MVPD-programmet.

   >[!IMPORTANT]
   >
   >Ändra inte standardslutpunkterna i något flöde om det inte uttryckligen anges av MVPD.

   ![Markera slutpunkter ](assets/select-endpoints.png)

   *Markera slutpunkter*

   **Ytterligare information**

   Det här avsnittet innehåller olika egenskaper som måste konfigureras för det valda MVPD-programmet i **Välj kanal och MVPD** -avsnitt.

   >[!NOTE]
   >
   > De faktiska egenskaperna kan skilja sig åt beroende på vilka MVPD-program som har valts i dialogrutan **Välj kanal och MVPD** -avsnitt.

   Du kan till exempel redigera **AuthN TTL** eller **Partner-ID** (Channel ID) för sammärkning på MVPD-inloggningssidan i följande bild.

   ![Redigera ytterligare information](assets/additional-information.png)

   *Redigera ytterligare information*

   Välj **Spara integration** längst upp till höger på sidan **Skapa ny integrering** -avsnitt.

En ny integrering skapas först efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).


## Inaktivera integrering {#disable-integratgion}

Så här inaktiverar du en integrering:

1. Välj **Integreringar** i den vänstra panelen.
1. Välj den integrering som du vill inaktivera.
1. Inaktivera alternativet längst upp till höger i den valda integreringen.

   ![Inaktivera integrering](assets/disable-integration.png)

   *Inaktivera integrering*

Integreringen inaktiveras först efter [granska och skicka ändringar](/help/authentication/tve-dashboard-review-push-changes.md).

När integreringen har inaktiverats förlorar slutanvändarna möjligheten att autentisera eller auktorisera med det specifika MVPD-programmet.


