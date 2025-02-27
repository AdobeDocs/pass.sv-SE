---
title: Frågor och svar om certifikat
description: Frågor och svar om certifikat
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# (Äldre) certifikatfrågor och svar {#certificates-q}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

>[!IMPORTANT]
>
> Se till att du håller dig informerad om de senaste produktmeddelandena för Adobe Pass-autentisering och tidslinjer för avveckling som sammanställts på sidan [Produktmeddelanden](/help/authentication/product-announcements.md).

</br>

**Q1:** Går det att registrera certifikat i iOS och Android?

**A:** Certifikatet för iOS och Android är detsamma i den aktuella konfigurationen. Det systemspecifika certifikatet används för båda plattformarna.

</br>

**Q2:** Kan samma iOS-certifikat användas som primära certifikat och säkerhetskopieringscertifikat i produktions- och mellanlagringsmiljöerna? Om det inte rekommenderas, kan du förklara detta?

**A:** Det är ingen idé att konfigurera samma certifikat som både primärt certifikat och säkerhetskopieringscertifikat. Vi har begreppet primära certifikat och säkerhetskopieringscertifikat så att vi kan konfigurera flera certifikat för en programmerare om det primära certifikatet upphör att gälla eller återkallas. Med ett säkerhetskopieringscertifikat får programmerarna tid att ändra det primära utan att det påverkar versionsmiljön. Men du kan använda samma uppsättning primära certifikat och säkerhetskopieringscertifikat för både produktions- och mellanlagringsprofilerna.

</br>

**Q3:** Krävs ett nytt certifikat för webbsidor som ska använda det nya flexibla TempPass?

**A:** Certifikatet (och vilket certifikat som helst) har konfigurerats på mediaföretags- och programmerarnivå. FlexibleTempPass är ett MVPD-program och du behöver inte konfigurera något certifikat för det, så om du integrerar en befintlig programmerare med flexibelt TempPass kommer det certifikat som redan har konfigurerats på programmerings-/medieföretagsnivå att användas.
