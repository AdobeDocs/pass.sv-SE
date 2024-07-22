---
title: MVPD-autentisering
description: MVPD-autentisering
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# MVPD-autentisering {#mvpd-authn}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Ökning {#mvpd-authn-overview}

Den faktiska rollen som tjänsteleverantör (SP) innehas av en programmerare, men Adobe Pass Authentication fungerar som SP-proxy för den programmeraren. Genom att använda Adobe Pass Authentication som mellanhand slipper både programmerare och distributörer anpassa sina tillståndsprocesser från fall till fall.

Stegen nedan visar händelseföljden, med hjälp av Adobe Pass-autentisering, när en programmerare begär autentisering från ett MVPD som stöder SAML. Observera att komponenten Adobe Pass Authentication Access Enabler är aktiv på användarens/prenumerantens klient. Därifrån underlättas alla steg i autentiseringsflödet av Access Enabler.

1. När användaren begär åtkomst till skyddat innehåll initierar Access Enabler autentisering (AuthN) för programmeraren (SP).
1. SP:s app presenterar en&quot;MVPD Picker&quot; för användaren för att erhålla en Pay TV-leverantör (MVPD). SP:n dirigerar sedan om användarens webbläsare till den valda IdP-tjänsten (MVPD).  Detta är **Programmerarinitierad inloggning**.  MVPD skickar svaret från IdP till Adobe SAML-bekräftelsens konsumenttjänst, där den behandlas.
1. Slutligen dirigerar Access Enabler om webbläsaren till SP-webbplatsen och informerar SP-leverantören om status (lyckad/misslyckad) för AuthN-begäran.

## Autentiseringsbegäran {#authn-req}

Som framgår av stegen ovan måste ett MVPD under AuthN-flödet både acceptera en SAML-baserad AuthN-begäran och skicka ett SAML AuthN-svar.

[OLCA-autentisering (Online Content Access) och gränssnittsspecifikationen ](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} visar en standard-AuthN-begäran och -svar. Även om Adobe Pass Authentication inte kräver att PDF-dokument baserar sina tillståndsmeddelanden på den här standarden, kan en titt på specifikationen ge insikt i de nyckelattribut som krävs för en AuthN-transaktion.

>[!NOTE]
>
>AuthN-begäran som ett MVPD tar emot med Adobe Pass Authentication innehåller inte en digital signatur. I exemplet nedan visas dock ingen signatur av utrymmesskäl. Ett exempel som visar en digital signatur finns i exemplet i [autentiseringssvaret](#authn-response) i följande avsnitt.

Exempel på SAML-autentiseringsbegäran:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

Tabellen nedan förklarar de attribut och taggar som måste finnas i en autentiseringsbegäran, med de förväntade standardvärdena.

**Information om SAML-autentiseringsbegäran**

| sample:AuthnRequest | &lt;AuthnRequest> utfärdad av tjänsteleverantören till identitetsleverantören. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | Det här är slutpunkten för Adobe som ska användas i efterföljande svar. Standardvärde: **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Mål | En URI-referens som anger den adress som denna begäran har skickats till. Detta är användbart för att förhindra obehörig vidarebefordran av begäranden till oönskade mottagare, ett skydd som krävs av vissa protokollbindningar. Om den finns måste den faktiska mottagaren kontrollera att URI-referensen identifierar platsen där meddelandet togs emot. Om så inte är fallet MÅSTE begäran förkastas. Vissa protokollbindningar kan kräva att det här attributet används. |
| ForceAuthn | Om ForceAuthn-attributet finns med värdet true, tvingar det identitetsleverantören att etablera identiteten på nytt, i stället för att förlita sig på en befintlig session som det kan ha med huvudkontot. |
| ID | En identifierare för begäran. Mer information finns i [SAML Core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank}, avsnitt 1.3.4. |
| IsPassive | Ett booleskt värde. Om värdet är&quot;true&quot; FÅR identitetsleverantören och själva användaragenten INTE synligt ta kontroll över användargränssnittet från den som gjorde begäran och interagera med presentatören på ett märkbart sätt. Om inget värde anges är standardvärdet &quot;false&quot;. |
| IssueInstant | Tidpunkten då svaret ska utfärdas. Tidsvärdet kodas i UTC enligt beskrivningen i [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Section 1.3.3. |
| Protokollbindning | En URI-referens som identifierar en SAML-protokollbindning som ska användas när &lt;Response>-meddelandet returneras. Mer information om protokollbindningar och URI-referenser som definierats för dem finns i [SAMLBind]. Standardvärde: urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| Version | Versionen av den här begäran. |
| saml:Issuer | Identifierar entiteten som genererade svarsmeddelandet. (Mer information om detta element finns i SAML core 2.0-os Section 2.2.5) |
| ds:Signature | En XML-signatur som skyddar intygets integritet och autentiserar utfärdaren, vilket beskrivs i avsnitt 5 i SAML Core 2.0-os |
| sample:NameIDPolicy | Anger begränsningar för den namnidentifierare som ska användas för att representera det begärda ämnet. |
| TillåtSkapa | Ett booleskt värde som används för att ange om identitetsleverantören tillåts skapa en ny identifierare som representerar säkerhetsobjektet när begäran slutförs. Standard: true |
| Format | Anger den URI-referens som motsvarar ett namnidentifierarformat Standard: urn:oasis:names:tc:SAML:2.0:nameid-format:transient Adobe rekommenderar: urn:oasis:names:tc:SAML:2.0:nameid-format:persistent |
| SPNameQualifier | Alternativt kan du ange att ämnesidentifieraren för kontrollämnet ska returneras (eller skapas) i namnutrymmet för en annan tjänsteleverantör än den som gjorde begäran. Standard: http://saml.sp.adobe.adobe.com |

## Autentiseringssvaret {#authn-response}

Efter att ha tagit emot och hanterat autentiseringsbegäran måste MVPD nu skicka ett autentiseringssvar.

**Exempel på SAML-autentiseringssvar**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


I exemplet ovan förväntar sig Adobe SP att hämta användar-ID:t från Subject/NameId. Adobe SP kan konfigureras för att hämta användar-ID från ett anpassat definierat attribut. Svaret ska innehålla ett element som följande:

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**SAML-autentiseringssvarsinformation**
| sampling:svar               | Svar mottaget av Adobe Pass-autentisering.                                                                                                                                                                                                                                                                                                                                                                                                                       |
|—|—|
| Mål                  | En URI-referens som anger den adress som denna begäran har skickats till. Detta är användbart för att förhindra obehörig vidarebefordran av begäranden till oönskade mottagare, ett skydd som krävs av vissa protokollbindningar. Om den finns måste den faktiska mottagaren kontrollera att URI-referensen identifierar platsen där meddelandet togs emot. Om så inte är fallet MÅSTE begäran förkastas. Vissa protokollbindningar kan kräva att det här attributet används. |
| ID                           | En identifierare för begäran. Den är av typen xs:ID och MÅSTE uppfylla de krav som anges i avsnitt 1.3.4 i [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} för unika identifierare. Värdena för ID-attributet i en begäran och InResponseTo-attributet i motsvarande svar MÅSTE matcha.                                                                                                                                                                                         |
| InResponseTo                 | ID:t för ett SAML-protokollmeddelande som en attesterande enhet kan skicka försäkran till. Värdet måste vara lika med det ID-attribut som skickas i autentiseringsbegäran. Se SAML core 2.0-os                                                                                                                                                                                                                                                      |
| IssueInstant                 | Tidpunkten då begäran utfärdades.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Version                      | Versionen av begäran.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| saml:Issuer                  | Identifierar entiteten som genererade begärandemeddelandet. (Mer information om detta element finns i avsnitt 2.2.5. SAML Core 2.0-os)                                                                                                                                                                                                                                                                                                                                     |
| sample:status                 | En kod som representerar status för motsvarande begäran.                                                                                                                                                                                                                                                                                                                                                                                                               |
| sample:StatusCode             | En kod som representerar statusen för den aktivitet som utfördes som svar på motsvarande begäran.                                                                                                                                                                                                                                                                                                                                                                       |
| saml:Assertion               | Den här typen anger den grundläggande information som är gemensam för alla försäkringar.                                                                                                                                                                                                                                                                                                                                                                                                        |
| ID                           | Identifieraren för den här försäkran.                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Version                      | Versionen av det här påståendet.                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| IssueInstant                 | Tidpunkten då begäran utfärdades.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ds:Signature                 | En XML-signatur som skyddar intygets integritet och autentiserar utfärdaren, vilket beskrivs i avsnitt 5 i SAML Core 2.0-os                                                                                                                                                                                                                                                                                                                                 |
| ds:SignedInfo                | SignedInfo-strukturen innehåller kanoniseringsalgoritmen, en signaturalgoritm och en eller flera referenser. SignedInfo-elementet kan innehålla ett valfritt ID-attribut som gör att andra signaturer och objekt kan referera till det. Se XML-signatursyntax och -bearbetning                                                                                                                                                                           |
| ds:CanonicalizationMethod    | CanonicalizationMethod är ett obligatoriskt element som anger den kanoniseringsalgoritm som tillämpas på SignedInfo-elementet innan signaturberäkningar utförs. Se XML-signatursyntax och -bearbetning                                                                                                                                                                                                                                                           |
| ds:SignatureMethod           | SignatureMethod är ett obligatoriskt element som anger den algoritm som används för signaturgenerering och validering. Den här algoritmen identifierar alla kryptografiska funktioner som ingår i signeringsåtgärden (t.ex. hash, algoritmer för offentlig nyckel, MAC, utfyllnad osv.) Se XML-signatursyntax och -bearbetning                                                                                                                                                              |
| ds:Reference                 | Referens är ett element som kan förekomma en eller flera gånger. Den specificerar en sammanfattningsalgoritm och ett sammanfattningsvärde, och eventuellt en identifierare för det objekt som signeras, objekttypen och/eller en lista över transformeringar som ska tillämpas före sammandragning. Se XML-signatursyntax och -bearbetning                                                                                                                                                                         |
| ds:Transforms                | Det valfria elementet Transforms innehåller en ordnad lista med Transform-element. Dessa beskriver hur signeraren fick dataobjektet som samlades in. Utdata för varje omformning fungerar som indata till nästa omformning. Indata till den första omformningen är resultatet av att URI-attributet för Reference-elementet har tagits bort. Utdata från den sista omformningen är indata för DigestMethod-algoritmen. Se XML-signatursyntax och -bearbetning          |
| ds:DigestMethod              | DigestMethod är ett obligatoriskt element som identifierar den sammandragsalgoritm som ska användas på det signerade objektet. Se XML-signatursyntax och -bearbetning                                                                                                                                                                                                                                                                                                                        |
| ds:DigestValue               | DigestValue är ett element som innehåller det kodade värdet för sammanfattningen. Sammanfattningen kodas alltid med base64. Se XML-signatursyntax och -bearbetning                                                                                                                                                                                                                                                                                                               |
| ds:SignatureValue            | SignatureValue-elementet innehåller det faktiska värdet för den digitala signaturen. Det kodas alltid med base64. Se XML-signatursyntax och -bearbetning                                                                                                                                                                                                                                                                                                                 |
| ds:KeyInfo                   | KeyInfo är ett valfritt element som gör att mottagaren/mottagarna kan få den nyckel som behövs för att validera signaturen. Se XML-signatursyntax och -bearbetning                                                                                                                                                                                                                                                                                                                   |
| ds:X509Data                  | Ett X509Data-element i KeyInfo innehåller en eller flera identifierare för nycklar eller X509-certifikat. Se XML-signatursyntax och -bearbetning                                                                                                                                                                                                                                                                                                                                 |
| ds: X509Certificate          | X509Certificate-elementet som innehåller ett base64-kodat [X509v3]-certifikat                                                                                                                                                                                                                                                                                                                                                                                          |
| saml:Subject                 | Innehållet i påståendet.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| saml:NameID                  | Elementet &lt;NameID> är av typen NameIDType (se avsnitt 2.2.2 i SAML Core 2.0-os) och används i olika SAML-kontrollkonstruktioner, som &lt;Subject>- och &lt;SubjectConfirmation>-element, och i olika protokollmeddelanden.                                                                                                                                                                                                                                           |
| Format                       | En URI-referens som representerar klassificeringen av strängbaserad identifierarinformation.                                                                                                                                                                                                                                                                                                                                                                                    |
| SPNameQualifier              | Kvalificerar dessutom ett namn med namnet på en tjänsteleverantör eller tjänsteleverantörsanknytning. Det här attributet ger ytterligare ett sätt att federera namn baserat på den förlitande parten eller de förlitande parterna.                                                                                                                                                                                                                                                                      |
| saml:SubjectConfirmation     | Information som gör att ämnet kan bekräftas. Om mer än en försökspersoner har fått en bekräftelse räcker det att någon av dem kan bekräfta ämnet för att kunna tillämpa påståendet.                                                                                                                                                                                                                                                    |
| saml:SubjectConfirmationData | Ytterligare bekräftelseinformation som ska användas av en specifik bekräftelsemetod. Det typiska innehållet i det här elementet kan till exempel vara ett <!--<ds:KeyInfo>-->-element enligt definitionen i XML-signatursyntax och bearbetningsspecifikation                                                                                                                                                                                                                                         |
| NotOnOrAfter                 | En tidpunkt då motivet inte längre kan bekräftas.                                                                                                                                                                                                                                                                                                                                                                                                            |
| Mottagare                    | En URI som anger entiteten eller platsen som en testentitet kan presentera försäkran på. Det här attributet kan till exempel indikera att försäkran måste levereras till en viss nätverksslutpunkt för att förhindra att en mellanhand dirigerar om den någon annanstans.                                                                                                                                                                                   |
| saml:Villkor              | &lt;Condition>-elementet fungerar som en tilläggspunkt för nya villkor.                                                                                                                                                                                                                                                                                                                                                                                                   |
| NotBefore                    | NotBefore-attributet anger den tidpunkt då giltighetsintervallet börjar.                                                                                                                                                                                                                                                                                                                                                                                  |
| saml:AudienceRestriction     | &lt;AudienceRestriction>-elementet anger att försäkran är adresserad till en eller flera specifika målgrupper som identifieras av &lt;Audience>-element.                                                                                                                                                                                                                                                                                                                           |
| saml:Audience                | En URI-referens som identifierar en avsedd målgrupp.                                                                                                                                                                                                                                                                                                                                                                                                                      |
| saml:AuthnStatement          | En autentiseringssats.                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| AuthnInstant                 | Anger tidpunkten då autentiseringen ägde rum.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| SessionIndex                 | Anger indexvärdet för en viss session mellan huvudobjektet som identifieras av ämnet och autentiseringsutfärdaren.                                                                                                                                                                                                                                                                                                                                              |
| saml:AuthnContext            | Den kontext som används av den autentiserande myndigheten fram till och med den autentiseringshändelse som genererade den här satsen.                                                                                                                                                                                                                                                                                                                                                 |
| saml:AuthnContextClassRef    | En URI-referens som identifierar en kontextklass för autentisering som beskriver den efterföljande kontextdeklarationen.                                                                                                                                                                                                                                                                                                                                            |
