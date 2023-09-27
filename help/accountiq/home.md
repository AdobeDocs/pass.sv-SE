---
title: Översikt över konto-IQ
description: Med hjälp av IQ kan programmerare och distributörer förstå riskerna för sina intäkter och sin verksamhet och fastställa de mest effektiva åtgärder som ska vidtas för att minska verkningarna av kreditbedrägeri.
exl-id: c0d85fc8-b5ab-4284-802e-82f52eff401f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---

# [!UICONTROL Account IQ] översikt {#account-iq-overview}

Delning av autentiseringsuppgifter av prenumeranter på direktuppspelningstjänster är ett stort och växande problem för branschen. Att förstå, identifiera och hantera delning av autentiseringsuppgifter är dessutom en komplex process. Det är komplicerat att förstå prenumerantens användningsbeteende och att få en helhetssyn på deras verksamhet - till exempel genom att skilja på medlemmarna inom samma hushåll och utanför. På grund av detta kan leverantörer av direktuppspelningstjänster ha hinder att agera mot delning av autentiseringsuppgifter.


<div class "preview">
Leverantörer av direktuppspelningstjänster för video är i allmänhet medvetna om riskerna och kostnaderna med att dela till sin verksamhet, men har begränsade möjligheter att vidta åtgärder, som att blockera aktieägarna eller erbjuda dem. Vi rekommenderar dock en välinformerad och målinriktad strategi, som gör det möjligt för tjänster att förstå hur materialet delas på rätt sätt och att anta strategier som belönar ett bra tittarbeteende och samtidigt inriktas på affärstillväxt. </span>

![IQ-flödesdiagram för konto](assets/aiq-intro.png)

*Bild: Informationsflöde för IQ-konto*

Adobe Pass [!UICONTROL Account IQ] gör det möjligt för direktuppspelningstjänster för video att förstå abonnenternas användningsmönster och identifiera delning av autentiseringsuppgifter. Genom att grundligt analysera det långa spåret av data som lämnas kvar av varje abonnent med hjälp av en maskininlärningsmodell som är tillverkad i flera lager från Adobe, kan direktuppspelningstjänster förstå användarbeteendet och identifiera delning av autentiseringsuppgifter med större säkerhet. Dessutom kan åtgärder vidtas genom integreringar med andra system, som att begränsa samtidiga strömmar eller anpassa erbjudanden, och validerar effekten av dessa åtgärder, oavsett om det gäller att uppmuntra till ett lagligt tittande eller öka antalet abonnenter och intäkterna.

[!UICONTROL Account IQ] innehåller verktyg och funktioner för att mäta, hantera och tjäna pengar på delning av autentiseringsuppgifter. Rapporter, analyser och kontrollpaneler gör det möjligt att utforska data för att identifiera mönster. Direktåtgärder stöds genom export och integrering med Adobe och tredjepartssystem som kampanjhantering, valutagränsbegränsning eller prenumerationsregistrering. Och dedikerade spårningsverktyg mäter framgången för dessa åtgärder så att de kan uppdateras eller utökas.

[!UICONTROL Account IQ] Programverktyg och -funktioner beskrivs i följande avsnitt:

* [Kontrollpanel](/help/accountiq/dashboard.md)
* [Allmänna användningsrapporter](/help/accountiq/general-usage-reports.md)
* [Rapporter om delade konton](/help/accountiq/shared-acc-reports.md)
* [Användningsmönster](/help/accountiq/usage-patterns.md)
* [Operationer](/help/accountiq/operations.md)

Låt oss göra en djupdykning i diagrammen och rapporterna i vart och ett av dessa avsnitt.

>[!MORELIKETHIS]
>
>* [Så här kommer du igång med konto-IQ](/help/accountiq/get-started.md)
>* [Kontrollpanel](/help/accountiq/dashboard.md)
>* [Allmänna användningsrapporter](/help/accountiq/general-usage-reports.md)
>* [Rapporter om delade konton](/help/accountiq/shared-acc-reports.md)
>* [Användningsmönster](/help/accountiq/usage-patterns.md)
>* [Ordlista över produkttermer](/help/accountiq/product-concepts.md)
>* [IQ-rapport för konto](https://www.adobe.com/content/dam/dx/us/en/products/primetime/resources/primetime-account-iq-whitepaper.pdf)

<!-- Credential sharing is rampant and prevalent among subscribers in the video streaming industry. To add to it, understanding, identifying, and acting on password sharing is a complex process. There is complexity involved in understanding the subscriber usage behavior and developing a holistic view of viewer activity—for example, distinguishing sharing among members within the same household and outside. Due to this challenge, streaming service providers have inhibitions in acting against password sharing.

Generally, video streaming service providers consider password sharing as fatal for business and act strongly against it, by blocking the sharers. However, it is advised to follow a holistic approach that enables them to understand sharing accurately and adopt strategies to reward good viewing behavior and target business growth simultaneously.

![Account IQ flow diagram](assets/aiq-intro.png)

*Figure: Account IQ information flow*

Adobe Pass Account IQ enables video streaming services understand the subscriber usage patterns and identify password sharing by analyzing usage behavior. Moreover, it validates the impact of applying actions to encourage legitimate viewing behavior while maximizing business ROI, eventually growing subscribers and revenue.

By deeply analyzing the long, winding trail of data left behind by each subscriber using Adobe's proprietary multi-layer machine learning model, customers can understand usage behavior and identify password sharing with a greater degree of certainty, use the insights to validate the impact of applying actions to encourage legitimate viewing behavior while maximizing business growth, eventually act on password sharing using validated tactics to improve viewer experience, growing subscribers and revenue (for e.g. converting sharers to paid subscribers, managing ad loads based on sharing behavior, rewarding good behavior with better viewer experience).

Account IQ is helps you understand usage patterns and identify password sharing by leveraging the Adobe Pass Authentication  solution that processes a huge volume of TV Everywhere transactions. A proprietary multi-layer machine learning model trained by this real-world TVE data accurately characterizes usage patterns and helps video streaming services understand usage patterns and identify password sharing at an individual account level. Based on Adobe's customer experience management solutions, Account IQ enables video streaming services to effectively use their audience data to create actionable sharing profiles as well powers integrations with other Adobe Digital Experience and 3rd party solutions—for example, Adobe Pass Concurrency Monitoring or Adobe Analytics—to enable understanding usage patterns, identify and act upon password sharing.


<!-- The widespread availability of video content and streaming services bring with it problem of account sharing; eventually leading to the loss of revenue by content providers. Account IQ helps TV Everywhere and VOD (video on demand) providers understand the risks to their revenue and business operations, and determine the most effective actions to take to mitigate the impacts of credential fraud. It helps these media companies (MVPDs, Programmers, and VOD providers) manage and uncover the instances of password sharing with a high level of confidence, enabling them deliver better business outcomes and provide better viewing experiences for subscribers.

To help media companies better understand the password sharing within their businesses, Adobe Pass Account IQ determines **Password Sharing Risk Index** that rates every subscriber on their likelihood of sharing account credentials for subscription passwords, from very low to very high. Based on these calculations and the resulting indices, analytics are performed and visuals are generated for better understanding and interpretation of the account sharing behavior. Account IQ is a hosted web application, which you can access using your browser.

Account IQ assigns sharing scores to different subscriber accounts, so that the content providers (media companies, programmers, MVPDs, and VOD providers) can take informed decisions about subscriber accounts and check the illicit sharing.

Passwords are the main methods for viewers to authenticate, and there is a misconception that credential sharing is allowed. This idea makes illicit password sharing a common practice; necessitating the need for media companies to educate their viewers about permissible sharing and prevent illicit sharing.-->
