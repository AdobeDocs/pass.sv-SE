---
title: Rapporter
description: Lär dig hur data aggregeras i TVE Dashboard-rapporter.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Rapporter {#Reports}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användningen av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

The **Rapporter** på TVE Dashboard ger åtkomst till aggregerade data för AuthN TTL-, AuthZ TTL- och SSO-rapporter. Rapporterna innehåller kanalintegreringar med olika programmeringsvideoprogrammeringsgränssnitt för alla [plattformar](#platforms).

Med rapporter kan ni filtrera data och samla in insikter över [specifika kanaler eller videofilmsprogram](#selecting-specific-channels-mvpds). Du kan också exportera rapporter i en CSV-fil för ytterligare analys.

## Visa rapporter {#view-reports}

Följ de här stegen för att visa en specifik rapport.

1. Välj **Rapporter** i den vänstra panelen.
1. Välj någon av följande flikar för att visa och exportera aggregerade data för de inkluderade kanalerna och videoprogrammeringsgränssnitten:
   * [AuthN TTL-rapporter](#authn-ttl-reports)
   * [AuthZ TTL-rapporter](#authz-ttl-reports)
   * [SSO-rapporter](#sso-reports)

   ![Typ av rapporter](assets/type-of-reports.png)

   *Typ av rapporter*

### AuthN TTL-rapporter {#authn-ttl-reports}

I AuthN TTL-rapporterna, som även kallas TTL (Authentication Time-To-Live), visas hur länge autentiseringstokens konfigureras för kanalintegreringar med olika MVPD-värden i alla [plattformar](#platforms). Med hjälp av dessa rapporter kan du kontrollera hur lång tid en användare förblir autentiserad för en viss plattform och plattform. Varaktighetsvärdena visas i användarvänliga format som **dagar**, **timmar**, **minuter** och **sekunder**. Tabellen AuthN TTL Reports innehåller vågrät och lodrät rullning för att passa olika skärmstorlekar.

Du kan även visa och hämta data för [specifika kanaler eller videoprogrammeringsprogram](#selecting-specific-channels-mvpds).

![Exportera AuthN TTL-rapporter](assets/authn-ttl-reports.png)

*Exportera AuthN TTL-rapporter*

>[!IMPORTANT]
>
> The **Inställd av MVPD** platshållare används när MVPD använder AuthN TTL-värdet i stället för Adobe Pass Authentication-konfigurationen.

Välj **Exportera rapporter** för att spara data som en CSV-fil på den lokala datorn.

### AuthZ TTL-rapporter {#authz-ttl-reports}

I AuthZ TTL-rapporterna, som även kallas TTL (Authorization Time-To-Live), visas varaktigheten för den auktoriseringstoken som konfigurerats för kanalintegreringar med olika MVPD i alla [plattformar](#platforms). Med hjälp av dessa rapporter kan du kontrollera hur lång tid en användare har på sig att bevaka innehållet för en viss plattform och plattform. Varaktighetsvärdena visas i användarvänliga format som **dagar**, **timmar**, **minuter** och **sekunder**. Tabellen AuthZ TTL Reports innehåller vågrät och lodrät rullning för olika skärmstorlekar.

Du kan även visa och hämta data för [specifika kanaler eller videoprogrammeringsprogram](#selecting-specific-channels-mvpds).

![Exportera AuthZ TTL-rapporter](assets/authz-ttl-reports.png)

*Exportera AuthZ TTL-rapporter*

>[!IMPORTANT]
>
> The **Inställd av MVPD** platshållare används när MVPD använder AuthZ TTL-värdet i stället för Adobe Pass Authentication-konfigurationen.

Välj **Exportera rapporter** för att spara data som en CSV-fil på den lokala datorn.

### SSO-rapporter {#sso-reports}

SSO-rapporterna, som även kallas för enkel inloggning, visar statusen för enkel inloggning konfigurerad för dina kanalintegreringar med olika MVPD-program för alla [plattformar](#platforms). Med hjälp av dessa rapporter kan du inspektera den förväntade användarautentiseringen för enkel inloggning för en specifik MVPD och plattform. Värdena visas i användarvänliga format som **Inaktiverad enkel inloggning**, **SSO aktiverat** och **Osäker enkel inloggning**. Tabellen SSO-rapporter innehåller vågrät och lodrät rullning för olika skärmstorlekar.

Du kan även visa och hämta data för [specifika kanaler eller videoprogrammeringsprogram](#selecting-specific-channels-mvpds).

![Exportera SSO-rapporter](assets/sso-reports.png)

*Exportera SSO-rapporter*

>[!IMPORTANT]
>
> The **Osäker enkel inloggning** platshållaren anger att enkel inloggning (SSO) är aktiverad och kan fungera. Inställningarna nedan kan dock hämma SSO-autentisering, vilket förklaras i följande exempel:
>
> * Inställningar för användarplattform: Alternativet att blockera cookies från tredje part.
> * Användarbeslut: Användarna nekar plattformsåtkomst till sin TV-leverantörsprenumeration.
> * MVPD-inställningar: MVPD begär autentisering för varje kanal.

Välj **Exportera rapporter** för att spara data som en CSV-fil på den lokala datorn.

## Plattformar {#platforms}

The [AuthN TTL-rapporter](#authn-ttl-reports), [AuthZ TTL-rapporter](#authz-ttl-reports)och [SSO-rapporter](#sso-reports) Presentera data på olika plattformar, som

* **Skrivbord**: Visar värden som tillämpas på programmeringsimplementeringarna via Adobe Pass Authentication JavaScript SDK.

* **Mobil**

  **iOS**: Visar värden som används med Adobe Pass Authentication iOS SDK.

  **Android**: Visar värden som används via Adobe Pass Authentication Android SDK.

  **Övriga**: Visar värden som används med Adobe Pass Authentication REST API som utvecklats för mobila enheter.

* **TVCD**

  **Roku**: Visar värden som används via Adobe Pass Authentication REST API, vilket identifierar Roku som en enhetstyp.

  **FireTV**: Visar värden som används via Adobe Pass Authentication FireTV SDK.

  **AppleTV**: Visar värden som används via Adobe Pass Authentication TVOS SDK.

  **Övriga**: Visar värden som används med Adobe Pass Authentication REST API för tv-anslutna enheter.

* **Oidentifierad plattform**: Visar värden som används för programmeringsimplementeringar när Adobe Pass Authentication Services identifierar en okänd enhetstyp.

Mer information om hur du delar önskad enhetstyp, till exempel **Roku** med Adobe Pass Authentication REST API:er eller SDK:er, visa mekanismen för [skicka klientinformation](/help/authentication/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> De aggregerade uppgifterna baseras på den specifika konfigurationen för varje Adobe Pass-autentiseringsmiljö. När du växlar mellan olika TVE Dashboard-miljöer kan du förvänta dig variationer i data mellan olika rapporter. Se [Adobe Pass autentiseringsmiljöer](/help/authentication/tve-dashboard-environments.md) för att veta mer.

## Välja ut specifika kanaler och videoprogrammeringsfönster {#selecting-specific-channels-mvpds}

The [AuthN TTL-rapporter](#authn-ttl-reports), [AuthZ TTL-rapporter](#authz-ttl-reports)och [SSO-rapporter](#sso-reports) presentera data för **Alla kanaler** integreringar med **Alla MVPD** som standard.

>[!NOTE]
>
> Om du avmarkerar **Alla kanaler** eller **Alla MVPD** i respektive listruta visas ett meddelande som gör ett val för att visa meningsfulla rapporter.

Så här skapar du en rapport för specifika kanaler:

1. Välj **Inkluderade kanaler** listrutemeny högst upp i den valda rapporten.

   ![Listrutan Inkluderade kanaler](assets/include-channels.png)

   *Listrutan Inkluderade kanaler*

1. Avmarkera **Alla kanaler**.
1. Välj önskade kanaler på menyn **Inkluderade kanaler** listrutemeny som du vill generera data för.

>[!NOTE]
>
> Om du vill ha alternativ tillgängliga i **MVPD ingår** nedrullningsbar meny måste du välja minst en kanal i **Inkluderade kanaler** listrutemeny.

Så här skapar du en rapport för specifika programmeringsdokumentskydd:

1. Välj **MVPD ingår** listrutemeny högst upp i den valda rapporten.

   ![Inkluderad listruta för MVPD](assets/include-mvpds.png)

   *Inkluderad listruta för MVPD*

1. Avmarkera **Alla MVPD**.
1. Välj önskad MVPD i listrutan **Inkluderade MVPD-filer** listrutemeny som du vill generera data för.
