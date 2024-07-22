---
title: enkel inloggning via passiv autentisering
description: enkel inloggning via passiv autentisering
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# enkel inloggning via passiv autentisering

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.


## Introduktion

Det här dokumentets omfattning är att beskriva implementeringen av det passiva autentiseringsflödet och hur det fungerar med vår vanliga inloggningsmetod.

## Användare

Adobe Pass-autentisering möjliggör enkel inloggning (SSO) mellan appar/platser. När en användare har loggat in med sina MVPD-autentiseringsuppgifter, genererar Adobe Pass Authentication en säker token som representerar MVPD:s autentiseringssession och binder denna token till användarens enhet med ett enhets-ID. Adobe Pass Authentication lagrar token/enhets-ID på en server eller på enheten.

Så länge token fortfarande är giltig visas användarna direkt som autentiserade. Detta gör att användare kan ange sina inloggningsuppgifter mindre ofta samtidigt som transaktionerna är säkra.



Affärsanvändningsexemplet som beskrivs här är ett mycket specifikt krav: att användaren MÅSTE autentiseras minst en gång för varje besökt plats. Detta gör att MVPD kan tillämpa affärsregler som är kopplade till authN-sessionen som kan variera mellan olika nätverk. Det står i konflikt med det aktuella TVE-löftet att en användare bara behöver logga in en gång och autentiseras på alla webbplatser som ingår i Adobe Pass Authentication-ekosystemet.



För att behålla affärsregeln men även behålla en bra användarupplevelse, kräver MVPD INTE att en användare anger autentiseringsuppgifter manuellt. Vi kan dra nytta av den tidigare angivna sessionscookie-filen och försöka att utföra en automatisk omautentisering med det passiva flödet. Ur användarens perspektiv kommer han att se ut som inloggad automatiskt.



För att lösa dessa implementerade vi två distinkta funktioner: autentisering per nätverk och stöd för passiv autentisering. MVPD-programmen stöder SAML passiv authN där användaren helt enkelt autentiseras igen om det finns en authN-session på IdP, oavsett på vilken plats sessionen skapades.



## Autentisering per nätverk

Den här funktionen ser till att MVPD tar emot en autentiseringsbegäran en gång för varje besökt webbplats. Den här funktionen innebär att en autentiseringstoken för Adobe Pass-autentisering kommer att begränsas till beställar-ID:t, och därmed endast gälla för det nätverk som begärde det. När användaren autentiserar på plats A och sedan besöker webbplatsen B måste han/hon autentisera.



Observera att eftersom användaren redan är autentiserad på MVPD IdP behöver han/hon inte ange inloggningsinformation, utan webbläsaren kommer i stället att omdirigeras från webbplatsen&quot;B&quot; till MVPD IdP och sedan tillbaka. Samma användare autentiseras fortfarande på plats&quot;A&quot; om han/hon återkommer.



I följande flöde visas grundläggande autentisering per nätverksfunktion:





## Passiv autentisering

Målet med detta är att få processen för omautentisering att ske i bakgrunden utan att en fullständig omdirigering till webbläsaren behövs och väljaren visas. Därför kommer en användare som går från plats A till plats B automatiskt att autentiseras.



Från ett UX-perspektiv kommer det inte att vara någon skillnad mellan det här flödet och ett flöde som körs med ett regelbundet MVPD-värde. Vad användaren ser är att när inloggningsuppgifterna har angetts som ett resultat av besök på webbplats A autentiseras de automatiskt på plats B.



För att göra det här flödet lönsamt måste MVPD ha stöd för passiv autentisering så att den dolda iframe-instansen inte &quot;fastnar&quot; på inloggningssidan om det inte längre finns någon session. Detta görs via standardattributet&quot;isPassive&quot;.



I följande diagram visas det förbättrade flödet och den passiva autentiseringen bakom kulisserna:





Exempel på SAML-begäran
Här följer ett SAML-begärandeexempel för det passiva authN-flödet:


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## Affärsregler

MVPD-program har specifika begränsningar för SSO-scoping-domäner. Exempelvis kan bara vissa domäner tillåtas av vissa distributörer av videoprogrammeringstjänster (SSO för samma medieföretag men inte för flera företag).
Vissa MVPD-program kan kräva att olika autentiseringsregler används. MVPD kan till exempel ha olika autentiserings-TTL:er per olika nätverk. Dessutom kan programmeringsgränssnitten möjliggöra hembaserad autentisering för vissa nätverk men inte för andra (fall där föräldrakontroll används representeras starkt här).


Dessa affärskrav bör tänka på att det viktigaste användningsfallet är att användaren inte behöver logga in igen efter att ha loggat in med sitt MVPD-program.

Detta kan uppnås genom att använda autentisering per nätverk med passiv authN-flagga.



## Kända begränsningar

iOS - På grund av iOS lokala lagring fungerar inte SSO-flöden i iOS för program som utvecklats av olika leverantörer. Mer information om enkel inloggning i iOS 8 och senare finns i denna Tech Note.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
