<properties
	pageTitle="Hinzufügen von Pushbenachrichtigungen zu Apache Cordova-App mit Azure Mobile Apps | Azure App Service"
	description="Erfahren Sie mehr über die Verwendung von Azure Mobile Apps zum Senden von Pushbenachrichtigungen an Ihre Apache Cordova-App."
	services="app-service\mobile"
	documentationCenter="javascript"
	manager="ggailey777"
	editor=""
	authors="adrianhall"/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-html"
	ms.devlang="javascript"
	ms.topic="article"
	ms.date="05/02/2016"
	ms.author="glenga"/>

# Hinzufügen von Pushbenachrichtigungen zu Ihrer Apache Cordova-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## Übersicht

In diesem Tutorial fügen Sie Pushbenachrichtigungen zum [Apache Cordova-Schnellstartprojekt] hinzu, damit jedes Mal, wenn ein Datensatz eingefügt wird, eine Pushbenachrichtigung gesendet wird. Dieses Tutorial baut auf dem [Apache Cordova-Schnellstarttutorial] auf, das Sie zuerst abschließen müssen. Wenn Sie über ein ASP.NET-Back-End verfügen und das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Pushbenachrichtigungs-Erweiterungspaket hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps] (in englischer Sprache).

##<a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial behandelt eine Apache Cordova-Anwendung, die in Visual Studio 2015 entwickelt und auf dem Google Android Emulator ausgeführt wird. Sie können dieses Tutorial darüber hinaus auch mit anderen Emulatoren oder Geräten sowie auf anderen Plattformen durchführen.

Für dieses Lernprogramm benötigen Sie Folgendes:

* Ein [Google-Konto] mit einer verifizierten E-Mail-Adresse.
* Einen PC mit [Visual Studio Community 2015] oder höher.
* [Visual Studio-Tools für Apache Cordova].
* Ein [aktives Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
* Ein abgeschlossenes [Apache Cordova Schnellstartprojekt]. Sie können vorher andere Tutorials (z. B. zur [Authentifizierung]) abschließen, dies ist jedoch nicht erforderlich.
* Ein Android-Gerät.

Pushbenachrichtigungen werden auf Android-Emulatoren zwar unterstützt, dies ist jedoch nicht zuverlässig, daher empfehlen wir das Testen von Pushbenachrichtigungen auf Emulatoren nicht.

##<a name="create-hub"></a>Erstellen eines Notification Hubs

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

##<a name="enable-gcm"></a>Aktivieren von Google Cloud Messaging

Da dieses Tutorial auf die Google-Android-Plattform abzielt, müssen Sie Google Cloud Messaging aktivieren. Wenn Sie für Apple iOS-Geräte entwickeln, müssen Sie die APNS-Unterstützung aktivieren. Im Fall von Microsoft Windows-Geräten müssen Sie die WNS- oder MPNS-Unterstützung aktivieren.

[AZURE.INCLUDE [mobile-services-enable-google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-backend"></a>Konfigurieren des mobilen App-Back-Ends zum Senden von Pushanforderungen

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

##<a name="update-service"></a>Aktualisieren des Serverprojekts zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

##<a name="configure-device"></a>Konfigurieren Ihres Android-Geräts für USB-Debugging

Bevor Sie Ihre Anwendung auf Ihrem Android-Gerät bereitstellen können, müssen Sie das USB-Debugging aktivieren. Führen Sie die folgenden Schritte auf Ihrem Android-Telefon aus:

1. Wechseln Sie zu **Einstellungen** > **Über das Telefon**, und tippen Sie dann mehrfach auf die **Build-Nummer**, bis der Entwicklermodus aktiviert wird (ungefähr sieben Mal).
 
2. Gehen Sie zurück zu **Einstellungen** > **Entwickleroptionen**, und aktivieren Sie dort **USB-Debugging**. Schließen Sie dann Ihr Android-Telefon mit einem USB-Kabel an den Entwicklungs-PC an.

Wir haben diese Vorgehensweise mit einem Google Nexus 5X-Gerät unter Android 6.0 (Marshmallow) getestet. Das Verfahren ist jedoch für alle modernen Android-Versionen gleich.

##<a name="add-push-to-app"></a>Hinzufügen von Pushbenachrichtigungen zur App

Sie müssen sicherstellen, dass Ihr Apache Cordova-Projekt zur Verarbeitung von Pushbenachrichtigungen bereit ist. Sie müssen das Cordova-Push-Plug-In sowie alle plattformspezifischen Pushdienste installieren.

### Installieren des Push-Plug-Ins

Apache Cordova-Anwendungen bieten keine native Verarbeitung von Geräte- und Netzwerkfunktionen. Diese Funktionen werden über Plug-Ins bereitgestellt, die entweder auf [npm](https://www.npmjs.com/) oder GitHub veröffentlicht werden. Das `phonegap-plugin-push`-Plug-In wird zur Verarbeitung von Netzwerk-Pushbenachrichtigungen verwendet.

Das Push-Plug-In lässt sich auf eine der folgenden Arten installieren:

**Über die Befehlszeile:**

Führen Sie den folgenden Befehl aus:

    cordova plugin add phonegap-plugin-push

**In Visual Studio:**

1.  Öffnen Sie im Projektmappen-Explorer die Datei `config.xml`, klicken Sie auf **Plug-Ins** > **Benutzerdefiniert**, wählen Sie **Git** als Installationsquelle aus, und geben Sie dann `https://github.com/phonegap/phonegap-plugin-push` als Quelle ein.

	![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Klicken Sie auf den Pfeil neben der Installationsquelle, und klicken Sie auf **Hinzufügen**.

Das Push-Plug-In ist jetzt installiert.

### Installieren der Google Play-Dienste

Das Push-Plug-In nutzt Android Google Play-Dienste für Pushbenachrichtigungen.

1.  Klicken Sie in **Visual Studio** auf **Tools** > **Android** > **Android SDK-Manager**, erweitern Sie den Ordner **Extras**, und aktivieren Sie die Kontrollkästchen, damit jedes der folgenden SDKs installiert wird.    
    * Android Support Library, Version 23 oder höher
    * Android Support Repository, Version 20 oder höher
    * Google Play-Dienste, Version 27 oder höher
    * Google Repository, Version 22 oder höher
     
2.  Klicken Sie auf **Pakete installieren**, und warten Sie, bis die Installation abgeschlossen ist.

Die aktuell erforderlichen Bibliotheken werden in der [phonegap-plugin-push installation documentation] (Dokumentation zur Pushinstallation des PhoneGap-Plug-Ins) aufgelistet.

### Registrieren des Geräts zum Push bei Start

1. Fügen Sie einen Aufruf von **registerForPushNotifications** während des Callbacks für den Anmeldeprozess oder am Ende der **onDeviceReady**-Methode hinzu:


		// Login to the service.
		client.login('google')
		    .then(function () {
		        // Create a table reference
		        todoItemTable = client.getTable('todoitem');

		        // Refresh the todoItems
		        refreshDisplay();

		        // Wire up the UI Event Handler for the Add Item
		        $('#add-item').submit(addItemHandler);
		        $('#refresh').on('click', refreshDisplay);

				// Added to register for push notifications.
		        registerForPushNotifications();

		    }, handleError);

	Das Beispiel zeigt den Aufruf von **registerForPushNotifications** nach erfolgter Authentifizierung, was beim Gebrauch von Pushbenachrichtigungen und Authentifizierung in Ihrer App empfehlenswert ist.

2. Fügen Sie die neue `registerForPushNotifications()`-Methode wie folgt hinzu:

	    // Register for Push Notifications.
		// Requires that phonegap-plugin-push be installed.
	    var pushRegistration = null;
	    function registerForPushNotifications() {
	        pushRegistration = PushNotification.init({
	            android: {
	                senderID: 'Your_Project_ID'
	            },
	            ios: {
	                alert: 'true',
	                badge: 'true',
	                sound: 'true'
	            },
	            wns: {

	            }
	        });

	        pushRegistration.on('registration', function (data) {
	            client.push.register('gcm', data.registrationId);
	        });

	        pushRegistration.on('notification', function (data, d2) {
	            alert('Push Received: ' + data.message);
	        });

	        pushRegistration.on('error', handleError);
	    }

3. Ersetzen Sie im obigen Code `Your_Project_ID` durch die numerische Projekt-ID für Ihre App aus der [Google-Entwicklerkonsole].

## Testen von Pushbenachrichtigungen in der App 

Sie können die Pushbenachrichtigungen jetzt testen, indem Sie die App ausführen und Elemente in die TodoItem-Tabelle einfügen. Dies können Sie vom gleichen Gerät aus oder von einem zweiten Gerät aus erledigen, sofern Sie das gleiche Back-End verwenden. Testen Sie Ihre Cordova-App auf der Android-Plattform mit einer der folgenden Methoden:

- **Auf einem physischen Gerät:** Schließen Sie Ihr Android-Gerät mit einem USB-Kabel an den Entwicklungscomputer an. Wählen Sie anstelle von **Google Android Emulator** den Eintrag **Gerät** aus. Visual Studio stellt die Anwendung auf dem Gerät bereit und führt sie aus. Anschließend können Sie mit der Anwendung auf dem Gerät interagieren. Verbessern Sie Ihre Entwicklungsumgebung. Anwendungen für die Bildschirmübertragung wie beispielsweise [Mobizen] können Sie bei der Entwicklung einer Android-Anwendung unterstützen, indem sie den Android-Bildschirm in einen Webbrowser auf dem PC projizieren.

- **Auf einem Android-Emulator:** Bevor Sie Pushbenachrichtigungen empfangen können, müssen Sie im Emulator ein Google-Konto hinzufügen.

##<a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Pushbenachrichtigungen finden Sie unter [Notification Hubs].
* Falls noch nicht erfolgt, setzen Sie das Tutorial mit dem [Hinzufügen von Authentifizierung] zu Ihrer Apache Cordova-App fort.

Erfahren Sie, wie Sie die SDKs nutzen,

* [Apache Cordova-SDK]
* [ASP.NET Server-SDK]
* [Node.js Server-SDK]

<!-- URLs -->
[Hinzufügen von Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova Schnellstartprojekt]: app-service-mobile-cordova-get-started.md
[Apache Cordova-Schnellstartprojekt]: app-service-mobile-cordova-get-started.md
[Apache Cordova-Schnellstarttutorial]: app-service-mobile-cordova-get-started.md
[Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google-Konto]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google-Entwicklerkonsole]: https://console.developers.google.com/home/dashboard
[phonegap-plugin-push installation documentation]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/de-DE/features/cordova-vs.aspx
[Notification Hubs]: ../notification-hubs/notification-hubs-overview.md
[Apache Cordova-SDK]: app-service-mobile-codova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

<!---HONumber=AcomDC_0504_2016-->