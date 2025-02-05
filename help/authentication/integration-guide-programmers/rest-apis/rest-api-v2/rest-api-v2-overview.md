---
title: REST API V2 - översikt
description: REST API V2 - översikt
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: 9dcc649b4216cccc9be35cd6553308bfc345b5f4
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# REST API V2 - översikt {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

<a href="https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements">![Live Webinar Series](/help/authentication/assets/rest-api-v2/live-webinar-series-banner.png)</a>

Vill du förbättra kostnadseffektiviteten i TVE-programmen?

Vill du minska utvecklingstiden och de resurser som krävs för att stödja TVE-program på flera plattformar?

Vill ni ha en konsekvent användarupplevelse på alla plattformar?

Vill du minska underhållsarbetet och förenkla tillhandahållandet av uppdateringar, felkorrigeringar eller förbättringar?

## Introduktion till REST API V2

Adobe Pass Authentication är spännande att lansera REST API V2, som är utformat för att förbättra användarupplevelsen och förenkla integreringen med Pass services.

Vi är glada över möjligheterna med vårt nya REST API, som utgör ett stort steg framåt för vår plattform och öppnar dörren för nya funktioner och applikationsflöden.

## Nyheter

Unik implementering på alla plattformar

Kundernas applikationer kan nu använda samma implementering på olika plattformar, vilket gör det enklare att starta nya funktioner eller underhålla appar live.

### Enhetsövergripande enkel inloggning

Med REST API V2 kan en autentiseringssession skickas säkert mellan olika enheter. Genom att bara skicka sessionen mellan enheterna kan användarna autentisera på sin mobila enhet och direktuppspela video på en tv-ansluten enhet utan omautentisering.

### Flera aktiva autentiseringssessioner

Olika aktiva MVPD-sessioner är nu möjliga, och kunderna kan välja att växla mellan TempPass och vanlig MVPD-integrering vid behov.

### Förbättrad säkerhetsmekanism

Dynamisk klientregistrering är den säkerhetsmekanism som används i alla flöden och funktioner. Det ger säkrare och mer detaljerad kontroll över kundernas applikationer och kan registrera applikationer på alla plattformar.

### Förbättrade prestanda för snabbare svarstider

Förbättrade cachningsmekanismer möjliggör mindre trafik till MVPD-program, vilket förbättrar svarstiderna och minskar latenstiden. Generellt sett är antalet API-anrop reducerat tills videon startar.

### Förbättrade felkoder för alla flöden

Avancerade felkoder är nu tillgängliga för alla lösenordsflöden i samma format, med ytterligare information som vägleder programmen för att förbättra den övergripande användarupplevelsen.

### Förbättrad kontroll över alla autentiseringssessioner

Den nya REST API V2 tillåter åtgärder på flera autentiserade sessioner samtidigt.

### Minska underhållskostnaderna

Alla svar och felinformation har nu normaliserats.

## Vad händer nu?

Alla kunder som för närvarande använder våra API:er via SDK:er eller REST-anrop kan fortsätta att göra det, eftersom vi planerar att fortsätta tillhandahålla support till och med slutet av 2025.

All framtida utveckling kommer dock att bygga på REST API V2. Vi rekommenderar starkt att du startar migreringsprocessen för att dra nytta av de senaste Adobe Pass-funktionerna.

## Vill du veta mer?

Gå till vår offentliga dokumentation för att komma igång:

- [Ordlista](rest-api-v2-glossary.md)
- [Vanliga frågor](rest-api-v2-faqs.md)
- [API:er](apis/rest-api-v2-apis-overview.md)
- [Flöden](flows/rest-api-v2-flows-overview.md)
- Cookbooks
- Bilaga
- [Lägsta systemkrav](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Vårt dedikerade supportteam finns också tillgängligt för att hjälpa dig med frågor och teknisk hjälp som du kan behöva.

## Vill du prova REST API V2?

Nu kan du utforska REST API V2 via vår produktdedikerade sida på webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass/).
