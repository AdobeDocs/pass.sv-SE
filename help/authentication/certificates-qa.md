---
title: Frågor och svar om certifikat
description: Frågor och svar om certifikat
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Frågor och svar om certifikat {#certificates-q}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>

**Fråga 1:** Går det att registrera certifikat i iOS och Android?

**S:** Certifikatet för iOS och Android är detsamma i den aktuella konfigurationen. Det systemspecifika certifikatet används för båda plattformarna.

</br>

**Fråga 2:** Kan samma iOS-certifikat användas som primära certifikat och säkerhetskopieringscertifikat i produktions- och mellanlagringsmiljöer? Om det inte rekommenderas, kan du förklara detta?

**S:** Det är ingen mening med att konfigurera samma certifikat som både primärt certifikat och säkerhetskopieringscertifikat. Vi har begreppet primära certifikat och säkerhetskopieringscertifikat så att vi kan konfigurera flera certifikat för en programmerare om det primära certifikatet upphör att gälla eller återkallas. Med ett säkerhetskopieringscertifikat får programmerarna tid att ändra det primära utan att det påverkar versionsmiljön. Men du kan använda samma uppsättning primära certifikat och säkerhetskopieringscertifikat för både produktions- och mellanlagringsprofilerna.

</br>

**Fråga 3:** Krävs ett nytt certifikat för webbsidor som ska använda det nya flexibla TempPass?

**S:** Certifikatet (och vilket certifikat som helst) har konfigurerats på Media Company- och Programmer-nivå. FlexibleTempPass är ett MVPD-program, du behöver inte konfigurera något certifikat för det, så om du integrerar en befintlig programmerare med flexibelt TempPass kommer det certifikat som redan har konfigurerats på programmerings-/medieföretagsnivå att användas.
