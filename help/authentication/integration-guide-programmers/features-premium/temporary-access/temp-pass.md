---
title: Tillfälligt pass
description: Tillfälligt pass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Tillfälligt pass {#temp-pass}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

## Sammanfattning av funktioner {#tempass-featur-summary}

Med Temp Pass kan programmerare ge temporär åtkomst till sitt skyddade innehåll för användare som inte har kontoautentiseringsuppgifter med ett MVPD.  Temporärt pass innehåller följande funktioner:

* Tillfälligt pass kan konfigureras för att ge temporär åtkomst för att täcka ett antal olika scenarier, bland annat följande:
   * En programmerare kan erbjuda en daglig, kort (t.ex. en 10-minuters förhandsgranskning) av en av sina webbplatser.
   * En programmerare kan erbjuda en enda, lång presentation (t.ex. fyra timmar) av ett större idrottsevenemang som OS, eller NCAA March Madness.
   * En programmerare kan innehålla en kombination av de två föregående scenarierna, till exempel en inledande, längre visningsperiod en dag, följt av en serie korta perioder som upprepas dagligen under ett antal efterföljande dagar.
* Programmerarna anger varaktigheten (Time-To-Live eller TTL) för sitt temporära pass.
* Temporärt pass utförs per begärande.  NBC kan till exempel konfigurera ett 4-timmars tillfälligt pass för begäraren&quot;NBCOlyics&quot;.
* Programmerare kan återställa alla token som har tilldelats en viss begärare.  Det tillfälliga MVPD-program som används för att implementera det tillfälliga lösenordet måste konfigureras med alternativet Autentisering per begärande aktiverat.
* **Tillfällig genomströmningsåtkomst beviljas enskilda användare på specifika enheter**. När tidsbegränsad åtkomst för en användare har upphört att gälla, kommer den användaren inte att kunna få temporär åtkomst på samma enhet förrän användarens [auktoriseringstoken](/help/authentication/kickstart/glossary.md#authz-token) har upphört att gälla rensas från Adobe Pass autentiseringsserver.


>[!NOTE]
>
>Temporärt pass är en del av Premium Workflow-paketet. Kontakta din Adobe Pass-säljare om du är intresserad av att använda den här funktionen.

## Funktionsinformation {#tempass-featur-details}

* **Hur visningstiden beräknas** - Den tid som ett tillfälligt pass är giltigt motsvarar inte den tid som en användare tillbringar med att visa innehåll i programmerarens program.  Efter den första användarbegäran för auktorisering via tillfälligt godkännande beräknas en förfallotid genom att den inledande aktuella begärandetiden läggs till i TTL-värdet som anges av programmeraren. Den här förfallotiden är kopplad till användarens enhets-ID och programmerarens begärande-ID, och lagras i Adobe Pass autentiseringsdatabas. Varje gång användaren försöker få åtkomst till innehåll med hjälp av ett tillfälligt pass från samma enhet, kommer Adobe Pass Authentication att jämföra serverns begärandetid med den förfallotid som är kopplad till användarens enhets-ID och programmerarens begärande-ID. Om serverns begärandetid är kortare än förfallotiden beviljas auktoriseringen. I annat fall nekas auktoriseringen.
* **Konfigurationsparametrar** - Följande parametrar för tillfälligt pass kan anges av en programmerare för att skapa en regel för tillfälligt pass:
   * **Token TTL** - Den tid som en användare kan bevaka utan att logga in på ett MVPD. Den här tiden är tidsbaserad och upphör att gälla oavsett om användaren tittar på innehåll eller inte.
  >[!NOTE]
  >Ett begärande-ID kan inte ha mer än en temporär lösenordsregel kopplad till sig.
* **Autentisering/auktorisering** - I det tillfälliga genomströmningsflödet anger du MVPD som &quot;Temp Pass&quot;.  Adobe Pass Authentication kommunicerar inte med ett faktiskt MVPD i Temp Pass-flödet, så MVPD för Temp Pass godkänner alla resurser. Programmerare kan ange en resurs som är tillgänglig med Temp-pass på samma sätt som de gör för resten av resurserna på deras plats. Mediekontrollantbiblioteket kan användas som vanligt för att verifiera den korta medietoken som används för temporärt pass och framtvinga resurskontroll före uppspelning.
* **Spårningsdata i tillfälligt genomströmningsflöde** - Två punkter gällande spårningsdata under ett tillfälligt genomströmningsberättigandeflöde:
   * Spårnings-ID:t som skickas från Adobe Pass-autentisering till ditt **sendTrackingData()**-återanrop är en hash-tagg för enhets-ID.
   * Eftersom det MVPD-ID som används i det temporära genomströmningsflödet är &quot;Temp Pass&quot;, skickas samma MVPD-ID tillbaka till **sendTrackingData()**. De flesta programmerare vill troligtvis behandla Temp Pass-värden annorlunda än faktiska MVPD-värden. Detta kräver ytterligare arbete i er analysimplementering.

Följande bild visar flödet för tillfälligt pass:

![Det tillfälliga genomströmningsflödet](../../../assets/temp-pass-flow.png)

*Bild: Det tillfälliga genomströmningsflödet*

## Implementera tillfälligt pass {#implement-tempass}

Temp Pass implementeras på sidan Adobe Pass Authentication med tillägget pseudo-MVPD med namnet&quot;TempPass&quot; i den deltagande programmerarens serverkonfiguration.  Detta pseudo-MVPD fungerar som ett faktiskt MVPD som tillfälligt ger åtkomst till programmerarens skyddade innehåll.

På programmerarsidan implementeras Temp Pass enligt följande för de två scenarier som MVPD använder för autentisering:

* **iFrame på programmerarens sida**. Tillfälligt pass fungerar oavsett vilken autentiseringstyp en MVPD har, men för iFrame-scenariot krävs ytterligare steg för att avbryta det aktuella autentiseringsflödet och autentisera med Temp Pass. De här stegen visas i [iFrame-inloggningen](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) nedan.
* **Omdirigering till MVPD-inloggningssidan**. I det mer traditionella fallet, där användargränssnittet för att aktivera det temporära passet presenteras innan autentiseringen med ett MVPD-dokument påbörjas, finns det inga särskilda åtgärder att vidta. Temporärt genomströmningstillstånd ska behandlas på samma sätt som ett vanligt MVPD.

Följande punkter gäller båda implementeringsscenarierna:

* &quot;Temporärt pass&quot; ska endast visas i MVPD-väljaren för användare som ännu inte begärt en tillfällig godkännande. Du kan blockera visningen för efterföljande förfrågningar genom att ha en flagga på cookies. Detta fungerar så länge användaren inte rensar webbläsarens cache. Om användaren rensar sina webbläsarcacheminnen visas&quot;Temporärt pass&quot; igen i väljaren och användaren kan begära det igen. Åtkomst beviljas endast om tiden för tillfälligt pass inte har löpt ut ännu.
* När en användare begär åtkomst via ett tillfälligt pass kommer Adobe Pass Authentication Server inte att utföra sin vanliga SAML-begäran (Security Assertion Markup Language) under autentiseringsprocessen. I stället returneras autentiseringsslutpunkten varje gång den anropas medan tokens är giltiga för enheten.
* När ett tillfälligt pass förfaller autentiseras inte användaren längre, eftersom autentiseringstoken och auktoriseringstoken har samma förfallodatum i Temp Pass-flödet. För att förklara för användarna att deras Temp-pass har upphört att gälla måste programmerare hämta det valda MVPD-programmet direkt efter att ha anropat `setRequestor()` och sedan ringa `checkAuthentication()` som vanligt. I återanropet `setAuthenticationStatus()` kan en kontroll göras för att avgöra om autentiseringsstatusen är 0, så om det valda MVPD var TempPass kan ett meddelande visas för användarna att deras Temp Pass-session har gått ut.
* Om en användare tar bort den temporära passtoken innan den upphör att gälla, kommer efterföljande tillståndskontroller att generera en token som har en TTL som motsvarar den återstående tiden.
* Om användaren tar bort den temporära passtoken efter förfallodatum returnerar de efterföljande behörighetskontrollerna &quot;användare ej auktoriserad&quot;.

I exemplen i [Exempelkod](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) nedan finns exempel på hur du kodar implementeringsinformationen som beskrivs i det här avsnittet.

## Exempelkod {#tempass-sample-code}

Avsnitten nedan visar hur du anropar Adobe Pass Authentication API för att implementera det tillfälliga genomströmningsflödet:

* [Exempel på iFrame-inloggning](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [Exempel på automatisk inloggning](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### Exempel på iFrame-inloggning {#iframe-login-sample}

I det här exemplet visas hur du implementerar Temp Pass för de fall där MVPD-program stöder iFrame-integrering:

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### Användningsexempel vid iFrame-inloggning {#iframe-login-use-cases}

**Om du vill begära ett tillfälligt pass för första gången:**

1. En användare öppnar programmerarens sida och klickar på inloggningslänken.
1. MVPD-väljaren öppnas och användaren väljer ett MVPD-dokument i listan.
1. Autentiseringsramen iFrame visas. Den här iFrame innehåller länken Temp Pass.
1. Användaren klickar på&quot;Tillfälligt pass&quot;, så programmeraren lägger till en flagga i en cookie för att hindra användaren från att se länken&quot;Tillfälligt pass&quot; vid efterföljande besök på sidan.
1. Autentiseringsbegäran för tillfälligt pass når Adobe Pass autentiseringsservrar och genererar en autentiseringstoken. TTL är lika med den tidsperiod som anges av programmeraren för det tillfälliga passet.
1. Begäran om auktorisering för tillfälligt pass når Adobe Pass autentiseringsservrar.
1. Adobe Pass autentiseringsservrar extraherar enhets- och begärande-ID från begäran och lagrar dem i databasen tillsammans med förfallotiden. Förfallotiden beräknas som: inledande tid för begäran om tillfälligt godkännande plus TTL (anges av Programmer).
1. Adobe Pass autentiseringsservrar genererar en auktoriseringstoken.
1. Användaren kommer åt det skyddade innehållet.

**Om du vill begära ett tillfälligt pass igen efter att en återkommande användare för tillfälligt pass har tagit bort webbläsarens cookies:**

1. Användaren öppnar programmerarens sida och klickar på inloggningslänken.
1. MVPD-väljaren öppnas och användaren väljer ett MVPD-dokument i listan.
1. Autentiseringsramen iFrame visas. Den här iFrame innehåller länken Temp Pass (användaren tog bort den ursprungliga cookien, så programmeraren vet inte om användaren har klickat på länken Temp Pass (Tillfälligt pass) tidigare.
1. Användaren klickar på&quot;Tillfälligt pass&quot; igen, så programmeraren lägger till en flagga i en cookie igen för att förhindra att användaren ser länken&quot;Tillfälligt pass&quot; vid efterföljande besök på sidan.
1. Begäran om temporär autentisering når Adobe Pass autentiseringsservrar, som genererar en autentiseringstoken. TTL är nu den återstående tiden för det tillfälliga passet (skillnaden mellan den aktuella tiden och den förfallotid som är kopplad till enhets-ID).
1. Begäran om auktorisering för tillfälligt pass når Adobe Pass autentiseringsservrar.
1. Adobe Pass autentiseringsservrar extraherar enhets- och begärande-ID från begäran och använder dem för att hämta förfallotiden från Adobe Pass autentiseringsdatabas. Den aktuella tiden jämförs med förfallotiden.
1. Om användarens temporära pass inte har gått ut genererar Adobe Pass-autentiseringsservrarna en auktoriseringstoken.
1. Om användarens temporära pass inte har gått ut kan användaren komma åt det skyddade innehållet.

### Exempel på automatisk inloggning {#auto-login-sample}

Följande exempel visar ett fall där en användare automatiskt loggas in med TempPass när han/hon besöker en webbplats. Användaren kan när som helst välja att logga in med ett vanligt MVPD och får ett varningsmeddelande om TempPass har gått ut:

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## Använd flera tillfälliga inmatningar {#use-mult-tempass}

För vissa händelser krävs kostnadsfri tillgång till innehåll i faser, t.ex. ett initialt intervall med kostnadsfri åtkomst (t.ex. 4 timmar), följt av kostnadsfri åtkomst varje dag (t.ex. 10 minuter på varje efterföljande dag).  För att en programmerare ska kunna implementera det här scenariot måste de ordna det med sin Adobe-kontakt för att konfigurera två tillfälliga programmeringsskyltar för programmeraren.

I det här scenariot (en inledande 4 timmars kostnadsfri session, följt av dagliga 10 minuters kostnadsfria sessioner) konfigurerar Adobe ett MVPD som kallas TempPass1 med en TTL på 4 timmar och ett TempPass2 med en TTL på 10 minuter för efterföljande period.  Båda är kopplade till programmerarens begärande-ID.

### Programmeringsimplementering {#mult-tempass-prog-impl}

När Adobe har konfigurerat de två TempPass-instanserna visas de två ytterligare MVPD-filerna (TempPass1 och TempPass2) i programmerarens MVPD-lista.  Programmeraren måste utföra följande steg för att implementera de flera tillfälliga passeren:

1. Vid användarens första besök på webbplatsen loggar du in dem automatiskt med TempPass1. Du kan använda exemplet för automatisk inloggning ovan som utgångspunkt för den här uppgiften.
1. När du upptäcker att TempPass1 har upphört att gälla, ska du lagra uppgiften (i en cookie/lokal lagring) och visa användaren med standardväljaren för MVPD. **Se till att filtrera bort TempPass1 och TempPass2 från den listan**.
1. Om TempPass1 har gått ut följande dag loggar du automatiskt in den användaren med TempPass2.
1. När TempPass2 har upphört att gälla, lagra detta (i en cookie/lokal lagring) för dagen och ge användaren en MVPD-standardväljare. Se även till att filtrera bort TempPass1 och TempPass2 från den listan.
1. För varje ny dag, kl. 00:00 timmar, återställer du alla tillfälliga pass för TempPass2 med [Reset TempPass web API](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass).

>[!NOTE]
>**Programmeringsmeddelande:** Adobe Pass Authentication har ingen inbyggd mekanism för att stoppa den kostnadsfria strömningen efter att 10 minuter har gått.  Det är programmerare som ska begränsa åtkomsten när TempPass2 har upphört att gälla. För att uppnå detta kan programmerare implementera ett checkAuthorization-anrop var X:e minut i sina webbplatser/appar, där X är den tidsperiod som programmeraren fastställer är lämplig för deras appar.

## Återställ alla temporära överföringar {#reset-all-tempass}

Vissa affärsregler kräver regelbunden rensning av temporärt pass eller en tillfällig återställning av alla temporära passeringar som utfärdats för ett visst begärande-ID och MVPD-ID. Den här funktionen stöder exempelvis följande användningsområden:

* Ett 10-minuters tillfälligt pass (det tillfälliga passet måste återställas i början av dagen)
* Ett tillfälligt pass som är tillgängligt för alla användare när nyheter bryts. (Temp-omgången måste återställas för alla enheter så snart de senaste nyheterna börjar.)
* Scenariot med flera tillfälliga pass som ger en kombination av en inledande visningsperiod med en viss längd, följt av efterföljande dagliga perioder med en annan längd.

Adobe Pass Authentication förser programmerare med ett *public*-webb-API för att återställa alla temporära passeringar:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>URL:en ovan ersätter föregående återställnings-API. Det gamla återställnings-API:t (v1) stöds inte längre.

* **Protokoll:** HTTPS
* **Värd:**
   * Version - mgmt.auth.adobe.com
   * Föregående - mgmt-prequal.auth.adobe.com
* **Sökväg:** /reset-tempass/v2/reset
* **Frågeparametrar:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Huvuden:** ApiKey - 1232293681726481
* **Svar:**
   * Lyckades - HTTP 204
   * Fel:
      * HTTP 400 för en felaktig begäran
      * HTTP 401 om ApiKey inte har angetts
      * HTTP 403 om ApiKey är ogiltig

Exempel:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Klienter som stöds {#supp-clients}

Stöd för tillfälligt pass- och återställningsverktyg per plattform:

| Adobe Pass-autentiseringsklienter | Temporärt pass | Återställ verktyg |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | JA | JA |
| IOS för inbyggd klient | JA | JA |
| Inbyggt klienttvOS | JA | JA |
| Android för inbyggd klient | JA | JA |
| Native Client fireTV | JA | JA |
| Klientlöst API | JA | JA |

## Begränsningar och kända fel {#limitations}

I det här avsnittet beskrivs de begränsningar som gäller för den aktuella implementeringen av Temp Pass.

**JavaScript SDK**: har stöd för att återställa funktioner för temporär överföring från version **3.X och senare**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
