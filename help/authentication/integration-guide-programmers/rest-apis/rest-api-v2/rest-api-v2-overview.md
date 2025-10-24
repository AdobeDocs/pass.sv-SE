---
title: REST API V2 - översikt
description: REST API V2 - översikt
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: 63dc9636f74f8eee1af6205c4d31a01df4503050
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# REST API V2 - översikt {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

Vill du förbättra kostnadseffektiviteten i TVE-programmen?

Vill du minska utvecklingstiden och de resurser som krävs för att stödja TVE-program på flera plattformar?

Vill ni ha en konsekvent användarupplevelse på alla plattformar?

Vill du minska underhållsarbetet och förenkla tillhandahållandet av uppdateringar, felkorrigeringar eller förbättringar?

## Introduktion till REST API V2 {#rest-api-v2-introduction}

Adobe Pass Authentication är spännande att lansera REST API V2, som är utformat för att förbättra användarupplevelsen och förenkla integreringen med Pass services.

Vi är glada över möjligheterna med vårt nya REST API, som utgör ett stort steg framåt för vår plattform och öppnar dörren för nya funktioner och applikationsflöden.

## Nyheter {#rest-api-v2-whats-new}

Unik implementering på alla plattformar

Kundernas applikationer kan nu använda samma implementering på olika plattformar, vilket gör det enklare att starta nya funktioner eller underhålla appar live.

### Enhetsövergripande enkel inloggning {#rest-api-v2-cross-device-sso}

Med REST API V2 kan en autentiseringssession skickas säkert mellan olika enheter. Genom att bara skicka sessionen mellan enheterna kan användarna autentisera på sin mobila enhet och direktuppspela video på en tv-ansluten enhet utan omautentisering.

### Flera aktiva autentiseringssessioner {#rest-api-v2-multiple-active-authentication-sessions}

Olika aktiva MVPD-sessioner är nu möjliga, och kunderna kan välja att växla mellan TempPass och vanlig MVPD-integrering vid behov.

### Förbättrad säkerhetsmekanism {#rest-api-v2-enhanced-security-mechanism}

Dynamisk klientregistrering är den säkerhetsmekanism som används i alla flöden och funktioner. Det ger säkrare och mer detaljerad kontroll över kundernas applikationer och kan registrera applikationer på alla plattformar.

### Förbättrade prestanda för snabbare svarstider {#rest-api-v2-improved-performance}

Förbättrade cachningsmekanismer möjliggör mindre trafik till MVPD-program, vilket förbättrar svarstiderna och minskar latenstiden. Generellt sett är antalet API-anrop reducerat tills videon startar.

### Förbättrade felkoder för alla flöden {#rest-api-v2-enhanced-error-codes}

Avancerade felkoder är nu tillgängliga för alla lösenordsflöden i samma format, med ytterligare information som vägleder programmen för att förbättra den övergripande användarupplevelsen.

### Förbättrad kontroll över alla autentiseringssessioner {#rest-api-v2-improved-control}

Den nya REST API V2 tillåter åtgärder på flera autentiserade sessioner samtidigt.

### Minska underhållskostnaderna {#rest-api-v2-reduce-maintenance-costs}

Alla svar och felinformation har nu normaliserats.

## Vad händer nu? {#rest-api-v2-whats-next}

Alla kunder som för närvarande använder våra API:er via SDK:er eller REST-anrop kan fortsätta att göra det, eftersom vi planerar att fortsätta tillhandahålla support till och med slutet av 2025.

All framtida utveckling kommer dock att bygga på REST API V2. Vi rekommenderar starkt att du startar migreringsprocessen för att dra nytta av de senaste Adobe Pass-funktionerna.

## Vill du veta mer? {#rest-api-v2-want-to-learn-more}

Gå till vår offentliga dokumentation för att komma igång:

- [Webbinarium](#rest-api-v2-webinar)
- [Ordlista](rest-api-v2-glossary.md)
- [Checklista](rest-api-v2-checklist.md)
- [AI-regler](rest-api-v2-ai-rules.md)
- [Vanliga frågor](rest-api-v2-faqs.md)
- [API:er](apis/rest-api-v2-apis-overview.md)
- [Flöden](flows/rest-api-v2-flows-overview.md)
- Cookbooks
- Bilaga
- [Lägsta systemkrav](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Vårt dedikerade supportteam finns också tillgängligt för att hjälpa dig med frågor och teknisk hjälp som du kan behöva.

### Webbinarium {#rest-api-v2-webinar}

Titta på webbinariet om det nya REST API V2, där vi gav en översikt över de nya funktionerna och en live-demo av REST API V2 i aktion.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## Vill du prova REST API V2? {#rest-api-v2-want-to-try}

Nu kan du utforska REST API V2 via vår produktdedikerade sida på webbplatsen [Adobe Developer](https://developer.adobe.com/adobe-pass/).
