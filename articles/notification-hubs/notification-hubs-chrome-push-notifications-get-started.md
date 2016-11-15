---
title: Senden von Pushbenachrichtigungen an Chrome-Apps mit Azure Notification Hubs | Microsoft Docs
description: Erfahren Sie, wie Sie Azure Notification Hubs zum Senden von Pushbenachrichtigungen an eine Chrome-App verwenden.
services: notification-hubs
keywords: Mobile Pushbenachrichtigungen,Pushbenachrichtigungen,Pushbenachrichtigung,Chrome-Pushbenachrichtigungen
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 600b1b7e5f3987c9a0acc33b7049f7118442b931


---
# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Senden von Pushbenachrichtigungen an Chrome-Apps mit Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In diesem Thema wird gezeigt, wie Sie Azure Notification Hubs zum Senden von Pushbenachrichtigungen an eine Chrome-App verwenden, die im Kontext des Google Chrome-Browsers angezeigt wird. In diesem Tutorial erstellen Sie eine Chrome-App, die Pushbenachrichtigungen per [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/)empfängt. 

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).
> 
> 

In diesem Lernprogramm werden die folgenden grundlegenden Schritte zur Aktivierung von Pushbenachrichtigungen behandelt:

* [Aktivieren von Google Cloud Messaging](#register)
* [Konfigurieren Ihres Notification Hub](#configure-hub)
* [Verbinden Ihrer Chrome-App mit dem Notification Hub](#connect-app)
* [Senden einer Pushbenachrichtigung an Ihre Chrome-App](#send)
* [Zusätzliche Funktionen](#next-steps)

> [!NOTE]
> Pushbenachrichtigungen von Chrome-Apps sind keine generischen browserinternen Benachrichtigungen. Sie sind speziell für das Browser-Erweiterbarkeitsmodell bestimmt. (Details finden Sie in der Übersicht zu Chrome-Apps unter [Chrome Apps Overview].) Zusätzlich zum Desktopbrowser können Chrome-Apps über Apache Cordova auch auf Mobilgeräten (Android und iOS) ausgeführt werden. Weitere Informationen finden Sie unter [Chrome-Apps auf Mobile].
> 
> 

GCM und Azure Notification Hubs werden genau wie unter Android konfiguriert, da [Google Cloud Messaging für Chrome] veraltet ist und dieselbe GCM jetzt sowohl Android-Geräte als auch Chrome-Instanzen unterstützt.

## <a name="a-idregisteraenable-google-cloud-messaging"></a><a id="register"></a>Aktivieren von Google Cloud Messaging
1. Navigieren Sie zur Website [Google Cloud Console] , melden Sie sich mit den Anmeldeinformationen für Ihr Google-Konto an, und klicken Sie dann auf **Create Project** . Geben Sie unter **Project Name** einen geeigneten Projektnamen an, und klicken Sie dann auf die Schaltfläche **Create**.
   
       ![Google Cloud Console - Create Project][1]
2. Notieren Sie sich die Projektnummer unter **Project Number** auf der Seite **Projects** für das Projekt, das Sie gerade erstellt haben. Sie verwenden diese Nummer in der Chrome-App als **GCM Sender ID** für die Registrierung bei GCM.
   
       ![Google Cloud Console - Project Number][2]
3. Klicken Sie im linken Bereich auf **APIs & auth**, scrollen Sie dann nach unten, und klicken Sie auf die Umschaltfläche, um **Google Cloud Messaging for Android** zu aktivieren. Sie müssen **Google Cloud Messaging for Chrome**nicht aktivieren.
   
       ![Google Cloud Console - Server Key][3]
4. Klicken Sie im linken Bereich auf **Anmeldeinformationen** > **Neuen Schlüssel erstellen** > **Serverschlüssel** > **Erstellen**.
   
       ![Google Cloud Console - Credentials][4]
5. Notieren Sie sich den API-Schlüssel des Servers (unter **API Key**). Sie konfigurieren diesen anschließend in Ihrem Notification Hub, um ihn zum Senden von Pushbenachrichtigungen an GCM zu aktivieren.
   
       ![Google Cloud Console - API Key][5]

## <a name="a-idconfigurehubaconfigure-your-notification-hub"></a><a id="configure-hub"></a>Konfigurieren Ihres Notification Hub
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   Wählen Sie im Blatt **Einstellungen** die Option **Notification Services** und dann **Google (GCM)**. Geben Sie den API-Schlüssel ein, und speichern Sie.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <a name="a-idconnectappaconnect-your-chrome-app-to-the-notification-hub"></a><a id="connect-app"></a>Verbinden Ihrer Chrome-App mit dem Notification Hub
Der Notification Hub ist jetzt für die Arbeit mit GCM konfiguriert, und Sie besitzen die Verbindungszeichenfolge, um die App sowohl für den Empfang als auch das Senden von Pushbenachrichtigungen zu registrieren. LK

### <a name="create-a-new-chrome-app"></a>Erstellen einer neuen Chrome-App
Das folgende Beispiel basiert auf dem [GCM-Beispiel für Chrome-Apps] und orientiert sich an der empfohlenen Methode zum Erstellen einer Chrome-App. Wir heben die Schritte hervor, die sich speziell auf Azure Notification Hubs beziehen. 

> [!NOTE]
> Es wird empfohlen, den Quellcode für diese Chrome-App aus dem [Notification Hub-Beispiel für Chrome-Apps]herunterzuladen.
> 
> 

Die Chrome-App wird mit JavaScript erstellt, und Sie können dazu einen beliebigen Text-Editor verwenden. Im folgenden wird dargestellt, wie die Chrome-App aussieht.

![Google Chrome-App][15]

1. Erstellen Sie einen Ordner, und geben Sie ihm den Namen `ChromePushApp`. Sie können den Namen natürlich frei wählen. Wenn Sie einen eigenen Namen verwenden, sollten Sie sicherstellen, dass Sie den Pfad in den erforderlichen Codesegmenten entsprechend anpassen.
2. Laden Sie die [crypto-js-Bibliothek] in den Ordner herunter, den Sie im zweiten Schritt erstellt haben. Dieser Bibliotheksordner enthält zwei Unterordner: `components` und `rollups`.
3. Erstellen Sie die Datei `manifest.json`. Alle Chrome-Apps verfügen über eine Manifestdatei. Sie enthält die App-Metadaten und, was noch wichtiger ist, alle Berechtigungen, die der App gewährt werden, wenn sie vom Benutzer installiert wird.
   
        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }
   
    Beachten Sie das `permissions`-Element, mit dem angegeben wird, dass diese Chrome-App Pushbenachrichtigungen von GCM empfangen kann. Es muss auch den Azure Notification Hubs-URI zum Senden eines REST-Aufrufs zur Registrierung durch die Chrome-App angeben.
    In unserer Beispiel-App wird auch eine Symboldatei ( `gcm_128.png`) verwendet, die sich in der Quelle befindet und aus dem ursprünglichen GCM-Beispiel wiederverwendet wird. Sie können sie durch ein beliebiges Bild ersetzen, das die [Kriterien für Symbole](https://developer.chrome.com/apps/manifest/icons)erfüllt.
4. Erstellen Sie eine Datei namens `background.js` mit dem folgenden Code:
   
        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }
   
        var registerWindowCreated = false;
   
        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {
   
            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }
   
        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);
   
        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    Dies ist die Datei, die die HTML-Datei des Chrome-App-Fensters einblendet (**register.html**) und den Handler **messageReceived** definiert, der die eingehende Pushbenachrichtigung verarbeitet.
5. Erstellen Sie eine Datei mit dem Namen `register.html`. Hiermit wird die Benutzeroberfläche der Chrome-App definiert. 
   
   > [!NOTE]
   > In diesem Beispiel wird **CryptoJS v3.1.2**verwendet. Wenn Sie eine andere Version der Bibliothek heruntergeladen haben, sollten Sie sicherstellen, dass Sie die Version unter dem `src` -Pfad richtig ersetzen.
   > 
   > 
   
        <html>
   
        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
   
        <body>
   
        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>
   
        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>
   
        <br/>
   
        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
   
        <br/>
        <br/>
   
        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
   
        </body>
   
        </html>
6. Erstellen Sie die Datei `register.js` mit dem folgenden Code. Diese Datei legt das Skript hinter `register.html`fest. Chrome-Apps erlauben keine Inline-Ausführung, daher müssen Sie ein separates Sicherungsskript für Ihre Benutzeroberfläche erstellen.
   
        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";
   
        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }
   
        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }
   
          document.getElementById("console").innerHTML = currentStatus  + status;
        }
   
        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);
   
          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }
   
        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";
   
          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });
   
          originalUri = endpoint + hubName;
        }
   
        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration
   
          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;
   
          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);
   
          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }
   
        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";
   
          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);
   
          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();
   
          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };
   
          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }
   
          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");
   
          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }
   
    Das obige Skript verfügt über die folgenden Schlüsselparameter:
   
   * **window.onload** definiert die Klickereignisse für die zwei Schaltflächen auf der Benutzeroberfläche. Eine führt die Registrierung bei GCM durch, und die andere verwendet die nach der Registrierung bei GCM zurückgegebene Registrierungs-ID für die Registrierung bei Azure Notification Hubs.
   * **updateLog** ist die Funktion, mit der wir einfache Protokollierungsfunktionen behandeln können.
   * **registerWithGCM** ist der Klick-Handler für die erste Schaltfläche, der den `chrome.gcm.register`-Aufruf zum Registrieren der aktuellen Chrome-App-Instanz an GCM sendet.
   * **registerCallback** ist die Rückruffunktion, die bei der Rückgabe des GCM-Registrierungsaufrufs aufgerufen wird.
   * **registerWithNH** ist der Klick-Handler für die zweite Schaltfläche, der die Registrierung bei Notification Hubs durchführt. Er ruft die vom Benutzer angegebenen Werte für `hubName` und `connectionString` ab und erstellt den REST-API-Aufruf für die Notification Hubs-Registrierung.
   * **splitConnectionString** und **generateSaSToken** sind Hilfselemente, die die JavaScript-Implementierung eines Prozesses zur SaS-Tokenerstellung darstellen, der für alle REST-API-Aufrufe verwendet werden muss. Weitere Informationen finden Sie unter [Allgemeine Konzepte](http://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest** ist die Funktion, mit der ein HTTP-REST-Aufruf für Azure Notification Hubs durchgeführt wird.
   * **registrationPayload** definiert die Nutzlast der XML-Datei der Registrierung. Weitere Informationen finden Sie unter [Erstellen der Registrierungs-NH-REST-API]. Die darin enthaltene Registrierungs-ID wird mit den von GCM empfangenen Daten aktualisiert.
   * **client** ist eine Instanz von **XMLHttpRequest**, die zum Senden der HTTP-POST-Anforderung verwendet wird. Beachten Sie, dass der `Authorization`-Header mit `sasToken` aktualisiert wird. Beim erfolgreichen Abschluss dieses Aufrufs wird diese Chrome-App-Instanz bei Azure Notification Hubs registriert.

Die gesamte Ordnerstruktur für dieses Projekt sollte dieser Struktur ähneln:        ![Google Chrome-App – Ordnerstruktur][21]

### <a name="set-up-and-test-your-chrome-app"></a>Einrichten und Testen Ihrer Chrome-App
1. Öffnen Sie den Chrome-Browser. Öffnen Sie **Chrome-Erweiterungen**, und aktivieren Sie **Entwicklermodus**.
   
       ![Google Chrome - Enable Developer Mode][16]
2. Klicken Sie auf **Entpackte Erweiterung laden** , und navigieren Sie zu dem Ordner, in dem Sie die Dateien erstellt haben. Sie können optional auch das **Chrome Apps & Extensions Developer Tool** verwenden. Dieses Tool ist eine eigene Chrome-App, die aus dem Chrome Web Store installiert werden muss und erweiterte Debuggingfunktionen für die Chrome-App-Entwicklung bereitstellt.
   
       ![Google Chrome - Load Unpacked Extension][17]
3. Wenn die Chrome-App ohne Fehler erstellt wird, wird sie angezeigt.
   
       ![Google Chrome - Chrome App Display][18]
4. Geben Sie die **Projektnummer**, die Sie zuvor in der **Google Cloud Console** erhalten haben, als Absender-ID ein, und klicken Sie auf **Bei GCM registrieren**. Die Meldung **Registration with GCM succeeded**
   
       ![Google Chrome - Chrome App Customization][19]
5. Geben Sie den **Notification Hub-Namen** und die **DefaultListenSharedAccessSignature** ein, die Sie zuvor im Portal erhalten haben, und klicken Sie auf **Bei Azure Notification Hub** registrieren. Die Meldung **Notification Hub Registration successful!**  und die Details der Antwort auf die Registrierung mit der Azure Notification Hubs-Registrierungs-ID werden angezeigt.
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <a name="a-namesendasend-a-notification-to-your-chrome-app"></a><a name="send"></a>Senden einer Benachrichtigung an Ihre Chrome-App
Zu Testzwecken senden wir Chrome-Pushbenachrichtigungen mit einer .NET-Konsolenanwendung. 

> [!NOTE]
> Sie können Pushbenachrichtigungen mit Notification Hubs von jedem beliebigen Back-End über unsere öffentliche <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-Schnittstelle</a> senden. Weitere plattformübergreifende Beispiele finden Sie in unserem [Dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/) .
> 
> 

1. Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus. Klicken Sie unter **Visual C#** auf **Windows**, auf **Konsolenanwendung** und dann auf **OK**.  Damit wird ein neues Konsolenanwendungsprojekt erstellt.
2. Klicken Sie im Menü **Extras** auf **Bibliothekspaket-Manager** und danach auf **Paket-Manager-Konsole**. Daraufhin wird die Paket-Manager-Konsole angezeigt.
3. Führen Sie im Konsolenfenster folgenden Befehl aus:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference to the Azure Service Bus SDK with the <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. Öffnen Sie `Program.cs`, und fügen Sie die folgende `using`-Anweisung hinzu:
   
        using Microsoft.Azure.NotificationHubs;
5. Fügen Sie in der `Program` -Klasse die folgende Methode hinzu:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure to replace the `<hub name>` placeholder with the name of the notification hub that appears in the [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace the connection string placeholder with the connection string called `DefaultFullSharedAccessSignature` that you obtained in the notification hub configuration section.
   
   > [!NOTE]
   > Stellen Sie sicher, dass Sie die Verbindungszeichenfolge mit Vollzugriff (**Full**) verwenden, nicht mit Abhörzugriff (**Listen**). Bei der Verbindungszeichenfolge mit Abhörzugriff ( **Listen** ) werden keine Berechtigungen zum Senden von Pushbenachrichtigungen gewährt.
   > 
   > 
6. Fügen Sie der `Main` -Methode die folgenden Aufrufe hinzu:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Stellen Sie sicher, dass Chrome ausgeführt wird, und führen Sie die Konsolenanwendung aus.
8. Das folgende Benachrichtigungspopupfenster sollte auf dem Desktop angezeigt werden.
   
       ![Google Chrome - Notification][13]
9. Sie können Ihre Benachrichtigungen auch im Fenster „Chrome-Benachrichtigungen“ in der Taskleiste anzeigen (unter Windows), wenn Chrome ausgeführt wird.
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> Sie müssen die Chrome-App nicht ausführen oder im Browser öffnen (der Chrome-Browser selbst muss allerdings ausgeführt werden). Außerdem wird eine konsolidierte Ansicht all Ihrer Benachrichtigungen im Chrome-Benachrichtigungsfenster angezeigt.
> 
> 

## <a name="next-steps"> </a>Nächste Schritte
Weitere Informationen zu Notification Hubs finden Sie unter [Übersicht über Notification Hubs].

Informationen zur Ausrichtung auf bestimmte Benutzer finden Sie im Tutorial [Azure Notification Hubs – Benachrichtigen von Benutzern] . 

Wenn Sie Ihre Benutzer in Interessengruppen einteilen möchten, können Sie das Tutorial [Azure Notification Hubs – Aktuelle Neuigkeiten] durcharbeiten.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Notification Hub-Beispiel für Chrome-Apps]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud Console]: http://cloud.google.com/console
[Klassisches Azure-Portal]: https://manage.windowsazure.com/
[Übersicht über Notification Hubs]: notification-hubs-push-notification-overview.md
[Chrome-Apps – Übersicht]: https://developer.chrome.com/apps/about_apps
[GCM-Beispiel für Chrome-Apps]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installierbare Web-Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-Apps auf Mobilgeräten]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Erstellen der Registrierungs-NH-REST-API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[crypto-js-Bibliothek]: http://code.google.com/p/crypto-js/
[GCM mit Chrome-Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging für Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs – Benachrichtigen von Benutzern]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs – Aktuelle Neuigkeiten]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md



<!--HONumber=Nov16_HO2-->


