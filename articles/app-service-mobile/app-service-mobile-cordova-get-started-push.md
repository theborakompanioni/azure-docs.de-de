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
	ms.date="08/11/2016"
	ms.author="glenga"/>

# Hinzufügen von Pushbenachrichtigungen zu Ihrer Apache Cordova-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## Übersicht

In diesem Tutorial fügen Sie Pushbenachrichtigungen zum [Apache Cordova-Schnellstartprojekt] hinzu, damit jedes Mal, wenn ein Datensatz eingefügt wird, eine Pushbenachrichtigung gesendet wird. Dieses Tutorial baut auf dem [Apache Cordova-Schnellstarttutorial] auf, das Sie zuerst abschließen müssen. Wenn Sie über ein ASP.NET-Back-End verfügen und das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Pushbenachrichtigungs-Erweiterungspaket hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps] \(in englischer Sprache).

##<a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird eine Apache Cordova-Anwendung beschrieben, die in Visual Studio 2015 entwickelt und auf dem Google Android Emulator, einem Android-Gerät einem Windows-Gerät und einem iOS-Gerät ausgeführt wird.

Für dieses Tutorial benötigen Sie Folgendes:

* Einen PC mit [Visual Studio Community 2015] oder höher.
* [Visual Studio-Tools für Apache Cordova].
* Ein [aktives Azure-Konto](https://azure.microsoft.com/pricing/free-trial/).
* Ein abgeschlossenes [Apache Cordova-Schnellstartprojekt]. Sie können vorher andere Tutorials (z.B. zur [Authentifizierung]) abschließen, aber dies ist nicht unbedingt erforderlich.
* (Android) Ein [Google-Konto] mit einer verifizierten E-Mail-Adresse und ein Android-Gerät.
* (iOS) Eine Apple Developer Program-Mitgliedschaft und ein iOS-Gerät (iOS Simulator unterstützt keine Pushvorgänge).
* (Windows) Ein Windows Store-Entwicklerkonto und ein Windows 10-Gerät.

Pushbenachrichtigungen werden auf Android-Emulatoren zwar unterstützt, dies ist jedoch nicht zuverlässig, daher empfehlen wir das Testen von Pushbenachrichtigungen auf Emulatoren nicht.

##<a name="create-hub"></a>Erstellen eines Notification Hubs

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

[Ansehen eines Videos mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##Aktualisieren des Serverprojekts zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Ändern der Cordova-App für den Empfang von Pushbenachrichtigungen

Sie müssen sicherstellen, dass Ihr Apache Cordova-Projekt zur Verarbeitung von Pushbenachrichtigungen bereit ist. Sie müssen das Cordova-Push-Plug-In sowie alle plattformspezifischen Pushdienste installieren.

#### Aktualisieren Sie die Cordova-Version in Ihrem Projekt.

Es wird empfohlen, das Clientprojekt auf Cordova 6.1.1 zu aktualisieren, wenn Sie Ihr Projekt mit einer älteren Version konfiguriert haben. Klicken Sie zum Aktualisieren des Projekts mit der rechten Maustaste auf die Datei „config.xml“, um den Konfigurations-Designer zu öffnen. Wählen Sie die Registerkarte „Plattformen“ und im Textfeld **Cordova CLI** dann die Version 6.1.1.

Wählen Sie **Erstellen** und dann **Projektmappe erstellen**, um das Projekt zu aktualisieren.

#### Installieren des Push-Plug-Ins

Apache Cordova-Anwendungen bieten keine native Verarbeitung von Geräte- und Netzwerkfunktionen. Diese Funktionen werden über Plug-Ins bereitgestellt, die entweder auf [npm](https://www.npmjs.com/) oder GitHub veröffentlicht werden. Das `phonegap-plugin-push`-Plug-In wird zur Verarbeitung von Netzwerk-Pushbenachrichtigungen verwendet.

Das Push-Plug-In lässt sich auf eine der folgenden Arten installieren:

**Über die Befehlszeile:**

Führen Sie den folgenden Befehl aus:

    cordova plugin add phonegap-plugin-push

**In Visual Studio:**

1.  Öffnen Sie im Projektmappen-Explorer die Datei `config.xml`, klicken Sie auf **Plug-Ins** > **Benutzerdefiniert**, wählen Sie **Git** als Installationsquelle aus, und geben Sie dann `https://github.com/phonegap/phonegap-plugin-push` als Quelle ein.

	![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Klicken Sie auf den Pfeil neben der Installationsquelle.

3. Falls Sie bereits über eine numerische Projekt-ID für das Google Developers Console-Projekt verfügen, können Sie diese hier unter **SENDER\_ID** hinzufügen. Geben Sie andernfalls einen Platzhalterwert ein, z.B. 777777. Falls Sie Android nutzen möchten, können Sie diesen Wert später noch in der Datei „config.xml“ ändern.

4. Klicken Sie auf **Hinzufügen**.

Das Push-Plug-In ist jetzt installiert.

####Installieren des Geräte-Plug-Ins

Verwenden Sie das gleiche Verfahren, das Sie zum Installieren des Push-Plug-Ins genutzt haben. Das Geräte-Plug-In befindet sich aber in der Liste mit den Kern-Plug-Ins (klicken Sie auf **Plug-Ins** > **Kern**). Sie benötigen dieses Plug-In zum Abrufen des Plattformnamens (`device.platform`).

#### Registrieren des Geräts zum Push bei Start

Zuerst fügen wir nur minimalen Code für Android ein. Später nehmen wir einige kleine Änderungen für die Ausführung unter iOS oder Windows 10 vor.

1. Fügen Sie einen Aufruf von **registerForPushNotifications** während des Rückrufs für den Anmeldeprozess oder am Ende der **onDeviceReady**-Methode hinzu:

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

	Dieses Beispiel zeigt den Aufruf von **registerForPushNotifications** nach erfolgter Authentifizierung, was sich empfiehlt, wenn Sie sowohl Pushbenachrichtigungen als auch Authentifizierung in Ihrer App verwenden.

2. Fügen Sie die neue **registerForPushNotifications**-Methode wie folgt hinzu:

		// Register for Push Notifications. Requires that phonegap-plugin-push be installed.
		var pushRegistration = null;
		function registerForPushNotifications() {
		  pushRegistration = PushNotification.init({
		      android: { senderID: 'Your_Project_ID' },
		      ios: { alert: 'true', badge: 'true', sound: 'true' },
		      wns: {}
		  });

		// Handle the registration event.
		pushRegistration.on('registration', function (data) {
		  // Get the native platform of the device.
		  var platform = device.platform;
		  // Get the handle returned during registration.
		  var handle = data.registrationId;
		  // Set the device-specific message template.
		  if (platform == 'android' || platform == 'Android') {
		      // Register for GCM notifications.
		      client.push.register('gcm', handle, {
		          mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
		      });
		  } else if (device.platform === 'iOS') {
		      // Register for notifications.            
		      client.push.register('apns', handle, {
		          mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
		      });
		  } else if (device.platform === 'windows') {
		      // Register for WNS notifications.
		      client.push.register('wns', handle, {
		          myTemplate: {
		              body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
		              headers: { 'X-WNS-Type': 'wns/toast' } }
		      });
		  }
		});

		pushRegistration.on('notification', function (data, d2) {
		  alert('Push Received: ' + data.message);
		});

		pushRegistration.on('error', handleError);
		}

3. (Android) Ersetzen Sie im obigen Code `Your_Project_ID` durch die numerische Projekt-ID für Ihre App aus der [Google Developers Console].

## (Optional) Konfigurieren und Ausführen der App unter Android

Arbeiten Sie diesen Abschnitt durch, um Pushbenachrichtigungen für Android zu aktivieren.

####<a name="enable-gcm"></a>Aktivieren von Google Cloud Messaging

Da dieses Tutorial ursprünglich für die Google-Android-Plattform bestimmt war, müssen Sie Google Cloud Messaging aktivieren. Im Fall von Microsoft Windows-Geräten müssen Sie dagegen die WNS-Unterstützung aktivieren.

[AZURE.INCLUDE [mobile-services-enable-google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

[Ansehen eines Videos mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-4-Set-up-gcm-for-push)

####<a name="configure-backend"></a>Konfigurieren des mobilen App-Back-Ends zum Senden von Pushanforderungen per GCM

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####Konfigurieren der Cordova-App für Android

Öffnen Sie in der Cordova-App die Datei „config.xml“, und ersetzen Sie `Your_Project_ID` durch die numerische Projekt-ID für Ihre App aus der [Google Developers Console].

		<plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
			<variable name="SENDER_ID" value="Your_Project_ID" />
		</plugin>

Öffnen Sie die Datei „index.js“, und aktualisieren Sie den Code, um die numerische Projekt-ID zu verwenden.

		pushRegistration = PushNotification.init({
			android: { senderID: 'Your_Project_ID' },
			ios: { alert: 'true', badge: 'true', sound: 'true' },
			wns: {}
		});

####<a name="configure-device"></a>Konfigurieren Ihres Android-Geräts für USB-Debugging

Bevor Sie Ihre Anwendung auf Ihrem Android-Gerät bereitstellen können, müssen Sie das USB-Debugging aktivieren. Führen Sie die folgenden Schritte auf Ihrem Android-Telefon aus:

1. Wechseln Sie zu **Einstellungen** > **Über das Telefon**, und tippen Sie dann mehrfach auf die **Build-Nummer**, bis der Entwicklermodus aktiviert wird (ungefähr siebenmal).

2. Wechseln Sie zurück zu **Einstellungen** > **Entwickleroptionen**, und aktivieren Sie dort **USB-Debugging**. Schließen Sie dann Ihr Android-Telefon mit einem USB-Kabel an den Entwicklungs-PC an.

Wir haben diese Vorgehensweise mit einem Google Nexus 5X-Gerät unter Android 6.0 (Marshmallow) getestet. Das Verfahren ist jedoch für alle modernen Android-Versionen gleich.

#### Installieren der Google Play-Dienste

Das Push-Plug-In nutzt Android Google Play-Dienste für Pushbenachrichtigungen.

1.  Klicken Sie in **Visual Studio** auf **Tools** > **Android** > **Android SDK-Manager**, erweitern Sie den Ordner **Extras**, und aktivieren Sie die Kontrollkästchen, damit die folgenden SDKs installiert werden.
    * Android Support Repository, Version 20 oder höher
    * Google Play-Dienste, Version 27 oder höher
    * Google Repository, Version 22 oder höher

2.  Klicken Sie auf **Pakete installieren**, und warten Sie, bis die Installation abgeschlossen ist.

Die aktuellen erforderlichen Bibliotheken werden in der [phonegap-plugin-push installation documentation]   \(Dokumentation zur Pushinstallation des PhoneGap-Plug-Ins) aufgelistet.

#### Testen von Pushbenachrichtigungen in der App unter Android

Sie können die Pushbenachrichtigungen jetzt testen, indem Sie die App ausführen und Elemente in die TodoItem-Tabelle einfügen. Dies können Sie vom gleichen Gerät aus oder von einem zweiten Gerät aus erledigen, sofern Sie das gleiche Back-End verwenden. Testen Sie Ihre Cordova-App auf der Android-Plattform mit einer der folgenden Methoden:

- **Auf einem physischen Gerät:** Schließen Sie Ihr Android-Gerät mit einem USB-Kabel an den Entwicklungscomputer an. Wählen Sie anstelle von **Google Android Emulator** den Eintrag **Gerät** aus. Visual Studio stellt die Anwendung auf dem Gerät bereit und führt sie aus. Anschließend können Sie mit der Anwendung auf dem Gerät interagieren. Verbessern Sie Ihre Entwicklungsumgebung. Anwendungen für die Bildschirmübertragung, z.B. [Mobizen], können Sie bei der Entwicklung einer Android-Anwendung unterstützen, indem Sie den Android-Bildschirm in einen Webbrowser auf dem PC projizieren.

- **Mit einem Android-Emulator:** Es sind zusätzliche Konfigurationsschritte erforderlich, wenn für das Ausführen ein Emulator verwendet wird.

	Stellen Sie sicher, dass die Bereitstellung oder das Debuggen auf einem virtuellen Gerät stattfindet, bei dem Google-APIs als Ziel festgelegt sind, wie unten beim Android Virtual Device (AVD)-Manager dargestellt.

	![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

	Wenn Sie einen schnelleren x86-Emulator verwenden möchten, [installieren Sie den HAXM-Treiber](https://taco.visualstudio.com/de-DE/docs/run-app-apache/#HAXM), und konfigurieren Sie den Emulator so, dass er diesen Treiber nutzt.

	Fügen Sie ein Google-Konto auf dem Android-Gerät hinzu, indem Sie auf **Apps** > **Einstellungen** > **Konto hinzufügen** klicken und dann den entsprechenden Anweisungen folgen (wir empfehlen die Verwendung eines vorhandenen Kontos, anstatt ein neues zu erstellen).

	![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

	Führen Sie wie zuvor die todolist-App aus und fügen Sie ein neues todo-Element ein. Dieses Mal wird im Benachrichtigungsbereich ein Benachrichtigungssymbol angezeigt. Öffnen Sie die Benachrichtigungsschublade, um den vollständigen Text der Benachrichtigung anzuzeigen.

	![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##(Optional) Konfigurieren und Ausführen unter iOS

In diesem Abschnitt wird die Ausführung des Cordova-Projekts auf iOS-Geräten beschrieben. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

####Installieren und Ausführen des iOS-remotebuild-Agents auf einem Macintosh-Computer oder in einem Clouddienst

Bevor Sie eine Cordova-App unter iOS mit Visual Studio ausführen können, müssen Sie im [iOS Setup Guide](http://taco.visualstudio.com/de-DE/docs/ios-guide/) (iOS-Einrichtungsleitfaden) die Schritte zum Installieren und Ausführen des remotebuild-Agents ausführen.

Vergewissern Sie sich, dass Sie die App für iOS erstellen können. Die Schritte im Einrichtungsleitfaden sind erforderlich, um die Erstellung für iOS in Visual Studio durchzuführen. Falls Sie nicht über einen Macintosh-Computer verfügen, können Sie für iOS den remotebuild-Agent unter einem Dienst wie MacInCloud nutzen. Weitere Informationen finden Sie unter [Build and simulate a Cordova iOS app in the cloud](http://taco.visualstudio.com/de-DE/docs/build_ios_cloud/) (Erstellen und Simulieren einer Cordova-iOS-App in der Cloud).

>[AZURE.NOTE] Zur Verwendung des Push-Plug-Ins unter iOS wird mindestens XCode 7 benötigt.

####Suchen nach der ID zur Verwendung als App-ID

Öffnen Sie vor dem Registrieren Ihrer App für Pushbenachrichtigungen die Datei „config.xml“ in Ihrer Cordova-App, suchen Sie im Widgetelement nach dem Attributwert `id`, und kopieren Sie ihn zur späteren Verwendung. Im folgenden XML-Code lautet die ID `io.cordova.myapp7777777`.

		<widget defaultlocale="de-DE" id="io.cordova.myapp7777777"
  		version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
			xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Verwenden Sie diesen Bezeichner später beim Erstellen einer App-ID im Entwicklerportal von Apple. (Falls Sie im Entwicklerportal eine andere App-ID erstellen und nutzen möchten, müssen Sie weiter unten in diesem Tutorial einige zusätzliche Schritte ausführen, um diese ID in „config.xml“ zu ändern. Die ID im Widget-Element muss mit der App-ID im Entwicklerportal übereinstimmen.)

####Registrieren der App für Pushbenachrichtigungen im Apple-Entwicklerportal

[AZURE.INCLUDE [Notification Hubs Xamarin – Aktivieren von Apple-Pushbenachrichtigungen](../../includes/notification-hubs-xamarin-enable-apple-push-notifications.md)]

[Ansehen eines Videos mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####Konfigurieren von Azure zum Senden von Pushbenachrichtigungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Klicken Sie auf **Durchsuchen** > **Mobile Apps** > Ihre mobile App > **Einstellungen** > **Push** > **Apple (APNS)** > **Zertifikat hochladen**. Laden Sie die P12-Pushzertifikatsdatei hoch, die Sie zuvor exportiert haben. Stellen Sie sicher, dass **Sandkasten** ausgewählt ist, wenn Sie ein Pushzertifikat für Entwicklung und Tests erstellt haben. Wählen Sie andernfalls **Produktion**. Ihr Dienst ist jetzt so konfiguriert, dass er mit Pushbenachrichtigungen für iOS funktioniert.

	![](./media/app-service-mobile-cordova-get-started-push/mobile-app-upload-apns-cert.png)

####Überprüfen, ob die App-ID mit der Cordova-App übereinstimmt

Wenn die App-ID, die Sie unter Ihrem Apple-Entwicklerkonto erstellt haben, bereits mit der ID des Widget-Elements in der Datei „config.xml“ übereinstimmt, können Sie diesen Schritt überspringen. Gehen Sie wie folgt vor, wenn die IDs nicht übereinstimmen:

1. Löschen Sie den Ordner „platforms“ aus Ihrem Projekt.

2. Löschen Sie den Ordner „plugins“ aus Ihrem Projekt.

3. Löschen Sie den Ordner „node\_modules“ aus Ihrem Projekt.

4. Aktualisieren Sie das Attribut „id“ des Widget-Elements in der Datei „config.xml“, um die App-ID zu verwenden, die Sie im Apple-Entwicklerkonto erstellt haben.

5. Erstellen Sie das Projekt neu.

#####Testen von Pushbenachrichtigungen in der iOS-App

1. Stellen Sie in Visual Studio sicher, dass **iOS** als Bereitstellungsziel ausgewählt ist, und wählen Sie dann die Option **Gerät** für die Ausführung auf Ihrem verbundenen iOS-Gerät.

	Die Ausführung ist auf einem iOS-Gerät möglich, das per iTunes mit dem PC verbunden ist. Der iOS-Simulator unterstützt keine Pushbenachrichtigungen.

2. Klicken Sie in Visual Studio auf die Schaltfläche **Ausführen**, oder drücken Sie die Taste **F5**, um das Projekt zu erstellen und die App auf einem iOS-Gerät zu starten. Klicken Sie dann auf **OK**, um Pushbenachrichtigungen zu akzeptieren.

	>[AZURE.NOTE] Sie müssen Pushbenachrichtigungen von Ihrer App ausdrücklich akzeptieren. Diese Anforderung tritt nur beim ersten Lauf der App auf.

3. Geben Sie in der App eine Aufgabe ein, und klicken Sie dann auf das Pluszeichen (+).

4. Stellen Sie sicher, dass Sie eine Benachrichtigung erhalten haben, und klicken Sie dann auf OK, um diese zu schließen.

##(Optional) Konfigurieren und Ausführen unter Windows

In diesem Abschnitt wird die Ausführung des Apache Cordova-App-Projekts auf Windows 10-Geräten beschrieben. (Das PhoneGap-Push-Plug-In wird unter Windows 10 unterstützt). Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

####Registrieren der Windows-App für Pushbenachrichtigungen mit WNS

Um die Store-Optionen in Visual Studio zu nutzen, wählen Sie in der Liste mit den Projektmappenplattformen ein Windows-Ziel aus, z.B. **Windows-x64** oder **Windows-x86** (meiden Sie **Windows-AnyCPU** für Pushbenachrichtigungen).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Ansehen eines Videos mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####Konfigurieren des Notification Hubs für WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####Konfigurieren der Cordova-App für die Unterstützung von Windows-Pushbenachrichtigungen

Öffnen Sie den Konfigurations-Designer (per Klick mit der rechten Maustaste auf die Datei „config.xml“ und Auswahl von **Designer anzeigen**), und wählen Sie die Registerkarte **Windows** und unter **Windows-Zielversion** dann **Windows 10** aus.

>[AZURE.NOTE] Bei Verwendung einer Cordova-Version vor Cordova 5.1.1 (6.1.1 wird empfohlen) müssen Sie in „config.xml“ auch das Flag „Toastfähig“ auf „true“ festlegen.

Öffnen Sie die Datei „build.json“, um Pushbenachrichtigungen in Ihren Standardbuilds (Debugbuilds) zu unterstützen. Kopieren Sie die „Releasekonfiguration“ in Ihre Debugkonfiguration.

		"windows": {
			"release": {
				"packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
				"publisherId": "CN=yourpublisherID"
			}
		}

Nach der Aktualisierung sollte der obige Code wie folgt aussehen.

	"windows": {
		"release": {
			"packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
			"publisherId": "CN=yourpublisherID"
			},
		"debug": {
			"packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
			"publisherId": "CN=yourpublisherID"
			}
		}

Erstellen Sie die App, und stellen Sie sicher, dass keine Fehler auftreten. Sie sollten Ihre Client-App nun für die Benachrichtigungen vom Back-End für die mobile App registrieren. Wiederholen Sie die Schritte in diesem Abschnitt für alle Windows-Projekte Ihrer Projektmappe.

####Testen von Pushbenachrichtigungen in Ihrer Windows-App

Stellen Sie in Visual Studio sicher, dass eine Windows-Plattform als Bereitstellungsziel ausgewählt ist, z.B. **Windows-x64** oder **Windows-x86**. Wählen Sie zum Ausführen der App auf einem Windows 10-PC, auf dem Visual Studio gehostet wird, die Option **Lokaler Computer**.

Klicken Sie auf die Schaltfläche Ausführen, um das Projekt zu erstellen und die App zu starten.

Geben Sie in der App einen Namen für ein neues TodoItem ein, und klicken Sie dann auf das Pluszeichen (+), um es hinzuzufügen.

Überprüfen Sie, ob beim Hinzufügen des Elements eine Benachrichtigung empfangen wird.

##<a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Pushbenachrichtigungen finden Sie unter [Notification Hubs].
* Falls noch nicht erfolgt, setzen Sie das Tutorial mit dem Schritt [Hinzufügen von Authentifizierung] zu Ihrer Apache Cordova-App fort.

Erfahren Sie, wie Sie die SDKs nutzen,

* [Apache Cordova-SDK]
* [ASP.NET Server-SDK]
* [Node.js Server-SDK]

<!-- URLs -->
[Hinzufügen von Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova-Schnellstartprojekt]: app-service-mobile-cordova-get-started.md
[Apache Cordova-Schnellstarttutorial]: app-service-mobile-cordova-get-started.md
[Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google-Konto]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google Developers Console]: https://console.developers.google.com/home/dashboard
[phonegap-plugin-push installation documentation]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/de-DE/features/cordova-vs.aspx
[Notification Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

<!---HONumber=AcomDC_0907_2016-->