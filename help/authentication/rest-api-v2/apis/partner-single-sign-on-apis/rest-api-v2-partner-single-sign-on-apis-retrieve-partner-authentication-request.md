---
title: Hämta partnerautentiseringsbegäran
description: REST API V2 - Hämta begäran om partnerautentisering
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '1093'
ht-degree: 0%

---


# Hämta partnerautentiseringsbegäran {#retrieve-partner-authentication-request}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Begäran {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">bana</td>
      <td>/api/v2/{serviceProvider}/sessions/sso/{partner}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">method</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Sökvägsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Den interna unika identifierare som är associerad med tjänsteleverantören under introduktionsprocessen.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">partner</td>
      <td>Namnet på den partner (t.ex. Apple) som tillhandahåller det single sign-on-ramverk som är integrerat med Adobe Pass autentiseringsflöden.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Kroppsparametrar</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Den ursprungliga domänen för det program som utför MVPD-inloggning.
        <br/><br/>
        Om plattformen för direktuppspelningsenheten har begränsningar för att tillhandahålla ett värde, måste ett program återuppta autentiseringssessionen och ange ett giltigt värde.
        <br/><br/>
        Detta används i händelse av reservscenarier där svaret anger att direktuppspelningsprogrammet ska fortsätta med det grundläggande autentiseringsflödet.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Den slutliga omdirigerings-URL som användaragenten navigerar till när autentiseringsflödet för det virtuella dokumentfönstret är slutfört.
        <br/><br/>
        Värdet måste vara URL-kodat.
        <br/><br/>
        Om plattformen för direktuppspelningsenheten har begränsningar för att tillhandahålla ett värde, måste ett program återuppta autentiseringssessionen och ange ett giltigt värde.
        <br/><br/>
        Detta används i händelse av reservscenarier där svaret anger att direktuppspelningsprogrammet ska fortsätta med det grundläggande autentiseringsflödet.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Behörighet</td>
      <td>Genereringen av mottagarens tokennyttolast beskrivs i dokumentationen för <a href="../../../dynamic-client-registration-api.md">Dynamisk klientregistrering</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         Godkänd medietyp för resurserna som skickas.
         <br/><br/>
         Det måste vara application/x-www-form-urlencoded.
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>Genereringen av nyttolasten för enhetsidentifieraren beskrivs i dokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         Genereringen av nyttolasten för enhetsinformation beskrivs i <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> -dokumentationen.
         <br/><br/>
         Vi rekommenderar att du alltid använder den när programmets enhetsplattform tillåter explicit tillhandahållande av giltiga värden.
         <br/><br/>
         När detta anges sammanfogas Adobe Pass Authentication-backend explicit med extraherade värden implicit (som standard).
         <br/><br/>
         Om det inte anges kommer Adobe Pass Authentication-serverdelen att använda extraherade värden implicit (som standard).
      </td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-status</td>
      <td>
        Genereringen av nyttolasten för enkel inloggning för partnermetoden beskrivs i dokumentationen för <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> .
        <br/><br/>
        Mer information om aktiverade flöden för enkel inloggning med en partner finns i dokumentationen för <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md"> enkel inloggning med partnerflöden </a> .</td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         Direktuppspelningsenhetens IP-adress.
         <br/><br/>
         Vi rekommenderar starkt att du alltid använder det för server-till-server-implementeringar, särskilt när anropet görs av programmeringstjänsten i stället för av direktuppspelningsenheten.
         <br/><br/>
         För implementeringar från klient till server skickas direktuppspelningsenhetens IP-adress implicit.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Acceptera</td>
      <td>
         Medietypen som accepteras av klientprogrammet.
         <br/><br/>
         Om det anges måste det vara application/json.
      </td>
      <td>valfri</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Användaragent</td>
      <td>Användaragenten för klientprogrammet.</td>
      <td>valfri</td>
   </tr>
</table>

## Svar {#response}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">Code</th>
      <th style="background-color: #EFF2F7; width: 20%;">Text</th>
      <th style="background-color: #EFF2F7;">Beskrivning</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Svarstexten innehåller information om nästa åtgärd som krävs för att utföra autentisering.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Felaktig begäran</td>
      <td>
        Begäran är ogiltig. Klienten måste åtgärda begäran och försöka igen. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="../../../enhanced-error-codes.md">Förbättrade felkoder</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Obehörig</td>
      <td>
        Åtkomsttoken är ogiltig. Klienten måste hämta en ny åtkomsttoken och försöka igen. Mer information finns i dokumentationen för <a href="../../../dynamic-client-registration-api.md">registrering av dynamisk klient</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Metoden tillåts inte</td>
      <td>
        HTTP-metoden är ogiltig. Klienten måste använda en HTTP-metod som är tillåten för den begärda resursen och försök igen. Mer information finns i avsnittet <a href="#request">Begäran</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Internt serverfel</td>
      <td>
        Ett fel uppstod på serversidan. Svarstexten kan innehålla felinformation som följer dokumentationen för <a href="../../../enhanced-error-codes.md">Förbättrade felkoder</a>.
      </td>
   </tr>
</table>

### Lyckades {#success}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Brödtext</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         JSON-objekt med följande attribut:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Attribut</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Den åtgärd som strömningsenheten måste utföra för att autentiseringsflödet ska kunna slutföras.
                  <br/><br/>
                  Möjliga värden är:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Värde</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">partner_profile</td>
                        <td>Direktuppspelningsenheten kan använda den angivna partnerautentiseringsbegäran för att erhålla ett partnerautentiseringssvar som kan utnyttjas för att hämta en profil.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">autentisera</td>
                        <td>
                            När partnerns enkla inloggningsflöde inte kan fortsätta kan direktuppspelningsenheten återgå till det grundläggande autentiseringsflödet.
                            <br/><br/>
                            Direktuppspelningsenheten eller någon annan enhet måste öppna den angivna URL:en i en användaragent.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">meritförteckning</td>
                        <td>
                            När partnerns enkla inloggningsflöde inte kan fortsätta kan direktuppspelningsenheten återgå till det grundläggande autentiseringsflödet.
                            <br/><br/>
                            Direktuppspelningsenheten eller någon annan enhet måste tillhandahålla de saknade parametrarna och återuppta autentiseringssessionen med hjälp av koden.
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">auktorisera</td>
                        <td>Direktuppspelningsenheten kan fortsätta direkt med beslutsflöden.</td>
                     </tr>
                  </table>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  Den typ av interaktion som direktuppspelningsenheten måste utföra för att fortsätta flödet med den åtgärd som anges av attributet actionName.
                  <br/><br/>
                  Möjliga värden är:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Värde</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">direkt</td>
                        <td>Flödet fortsätter med ett direkt anrop till den angivna URL:en med en HTTP-klient som är tillgänglig för klientimplementeringen.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">interaktiv</td>
                        <td>Flödet fortsätter med en navigering till den angivna URL:en med en användaragent.</td>
                     </tr>
                  </table>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>
                    De saknade parametrar som måste anges för att det grundläggande autentiseringsflödet ska kunna slutföras.
                    <br/><br/>
                    Det här fältet visas när partnerns enkla inloggningsflöde inte kan fortsätta.
               </td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>Den URL som klientprogrammet behöver för att navigera i.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">kod</td>
               <td>
                    Autentiseringskoden som kan användas i ett sekundärt program för att återuppta autentiseringssessionen.
                    <br/><br/>
                    Det här fältet visas när partnerns enkla inloggningsflöde inte kan fortsätta.
               </td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    Partnerautentiseringsbegäran som ska användas i autentiseringsflödet med partnern utanför Adobe Pass autentiseringssystem.
                    <br/><br/>
                    Det här fältet visas när partnerflödet för enkel inloggning kan fortsätta.
                    <br/><br/>
                    JSON-objekt med följande attribut:
                    <table>
                        <tr>
                            <th style="background-color: #EFF2F7; width: 30%;">Attribut</th>
                            <th style="background-color: #EFF2F7;"></th>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">type</td>
                            <td>
                                Anger vilken typ av protokoll som stöds av MVPD.
                                <br/><br/>
                                Möjliga värden är:
                                <table>
                                    <tr>
                                        <th style="background-color: #EFF2F7; width: 30%;">Värde</th>
                                        <th style="background-color: #EFF2F7;"></th>
                                    </tr>
                                    <tr>
                                        <td style="background-color: #DEEBFF;">saml</td>
                                        <td>MVPD stöder SAML-protokoll.</td>
                                    </tr>
                                </table>
                            </td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">förfrågan</td>
                            <td>SAML-begäran.</td>
                        </tr>
                        <tr>
                            <td style="background-color: #DEEBFF;">attributes</td>
                            <td>Attributen för SAML-begäran.</td>
                        </tr>
                    </table>
               </td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Den ogenomskinliga identifierare som kan användas för att spåra användaraktivitet.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>Den interna unika identifierare som är associerad med identitetsleverantören under introduktionsprocessen.</td>
               <td>valfri</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>Den interna unika identifierare som är associerad med tjänsteleverantören under introduktionsprocessen.</td>
               <td><i>obligatoriskt</i></td>
            </tr>
         </table>
      </td>
</table>

### Fel {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Sidhuvuden</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatoriskt</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Brödtext</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">fel</td>
      <td>Felet ger ytterligare information som följer dokumentationen för <a href="../../../enhanced-error-codes.md">Förbättrade felkoder</a>.</td>
      <td><i>obligatoriskt</i></td>
   </tr>
</table>

## Exempel {#samples}

### 1. Giltig enkel inloggning för partner aktiverad

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"partner_profile",
   "actionType":"direct",
   "url":"/v2/REF30/profiles/sso/Apple/Cablevision",
   "sessionId":"83c046be-ea4b-4581-b5f2-13e56e69dee9",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30",
   "authenticationRequest":{
      "type":"saml",
      "request":"PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG...."
   }
}    
```

>[!ENDTABS]

### 2. Försämrad MVPD

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK
 
{          
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/api/v2/REF30/decisions",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30",
   "sessionId":"14d4f239-e3b1-4a4a-b8b3-6395b968a260"
}
```

>[!ENDTABS]

### 3. Inaktiverad integrering

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 403
Content-Type: application/json; charset=utf-8
 
{
    "errors" : [
        {
            "code": "unknown_integration",
            "message": "The integration between the specified programmer and identity provider doesn't exist or it's disabled. Use the TVE Dashboard to register or enable the required integration.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}
```

>[!ENDTABS]

### 4. Partnerenkel inloggning är inte aktiverad (i lösenordskonfiguration eller VSA) - återgår till vanlig autentisering, alla nödvändiga parametrar finns

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status : ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"authenticate",
   "actionType":"interactive",
   "url":"/v2/authenticate/REF30/OKTWW2W",
   "code":"OKTWW2W",
   "sessionId":"748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}  
```

>[!ENDTABS]

### 5. Enkel inloggning för partner är inte aktiverad (i lösenordskonfiguration eller på VSA) - återgång till vanlig autentisering, nödvändiga parametrar saknas, autentiseringssessionen måste återupptas

>[!BEGINTABS]

>[!TAB Begäran]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB Svar]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"resume",
   "actionType":"direct",
   "missingParameters":[
      "redirectUrl"
   ],
   "url":"/v2/REF30/sessions/SB7ZRIO",
   "code":"SB7ZRIO",
   "sessionId":"1476173f-5088-43b8-b7c3-8cf3a185de0a",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}
```

>[!ENDTABS]
