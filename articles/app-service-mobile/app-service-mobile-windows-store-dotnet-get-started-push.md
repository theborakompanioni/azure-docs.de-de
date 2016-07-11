<properties
	pageTitle="Hinzufügen von Pushbenachrichtigungen zu Ihrer App für die universelle Windows-Plattform (UWP) | Azure Mobile Apps"
	description="Erfahren Sie, wie Sie Azure Mobile App Service-Apps und Azure Notification Hubs verwenden, um Pushbenachrichtigungen an Ihre App für die universelle Windows-Plattform (UWP) zu senden."
	services="app-service\mobile,notification-hubs"
	documentationCenter="windows"
	authors="ggailey777"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-windows"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="05/15/2016"
	ms.author="glenga"/>

# Hinzufügen von Pushbenachrichtigungen zu Ihrer Windows-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##Übersicht

In diesem Thema erfahren Sie, wie Sie mithilfe von Mobile Apps in Azure App Service und mit Azure Notification Hubs Pushbenachrichtigungen an eine App für die universelle Windows-Plattform (UWP) senden. In diesem Szenario sendet das Mobile App-Back-End eine Pushbenachrichtigung an alle Windows-Apps, die beim Windows-Benachrichtigungsdienst (Windows Notification Service, WNS) registriert sind, wenn ein neues Element hinzugefügt wird.

Dieses Tutorial baut auf dem Mobile App-Schnellstart auf. Bevor Sie mit diesem Lernprogramm beginnen, müssen Sie zunächst das Lernprogramm [Erstellen einer Windows-App](app-service-mobile-windows-store-dotnet-get-started.md) abschließen. Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Pushbenachrichtigungs-Erweiterungspaket hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) (in englischer Sprache).

##<a name="create-hub"></a>Erstellen eines Notification Hubs

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

##Registrieren der App für Pushbenachrichtigungen

Bevor Sie Pushbenachrichtigungen von Azure an Ihre Windows-Apps senden können, müssen Sie Ihre App an den Windows Store übermitteln. Sie können dann das Serverprojekt zur Integration mit dem WNS konfigurieren.

1. Klicken Sie im Projektmappen-Explorer von Visual Studio mit der rechten Maustaste auf das UWP-App-Projekt, und klicken Sie dann auf **Store** > **App mit Store verknüpfen...**

    ![Zuordnen der App zu Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. Klicken Sie im Assistenten auf **Weiter**, melden Sie sich mit Ihrem Microsoft-Konto an, geben Sie unter **App-Namen reservieren** einen Namen für Ihre App ein, und klicken Sie dann auf **Reservieren**.

3. Nachdem die App-Registrierung erfolgreich erstellt wurde, wählen Sie den Namen der neuen App aus, klicken Sie auf **Weiter** und dann auf **Zuordnen**. Auf diese Weise werden die erforderlichen Windows Store-Registrierungsinformationen zum Anwendungsmanifest hinzugefügt.

7. Navigieren Sie zum [Windows-Entwicklungscenter](https://dev.windows.com/de-DE/overview), melden Sie sich mit Ihrem Microsoft-Konto an, klicken Sie auf die neue App-Registrierung unter **Meine Apps**, und erweitern Sie dann **Dienste** > **Pushbenachrichtigungen**.

8. Klicken Sie auf der Seite **Pushbenachrichtigungen** auf **Live-Dienste-Website** unter **Microsoft Azure Mobile Services**.

9. Notieren Sie sich den auf der Registrierungsseite unter **Anwendungsgeheimnisse** angezeigten Wert sowie die **Paket-SID**. Sie benötigen diese Daten später beim Konfigurieren des Back-Ends für die mobile App.

	![Zuordnen der App zu Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Der geheime Clientschlüssel und die Paket-SID sind wichtige Sicherheitsanmeldeinformationen. Geben Sie diese Werte nicht weiter, und verteilen Sie sie nicht mit Ihrer Anwendung. Die **Anwendungs-ID** wird zusammen mit dem geheimen Schlüssel zum Konfigurieren der Microsoft-Kontoauthentifizierung verwendet.

##Konfigurieren des Back-Ends zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


##<a id="update-service"></a>Aktualisieren des Servers zum Senden von Pushbenachrichtigungen

Nachdem die Pushbenachrichtigungen in der App aktiviert wurden, müssen Sie ihr App-Back-End aktualisieren, um Pushbenachrichtigungen senden zu können. Nutzen Sie das Ihrem Back-End-Projekttyp entsprechende folgende Verfahren – entweder [.NET-Back-End](#dotnet) oder [Node.js-Back-End](#nodejs).

### <a name="dotnet"></a>.NET-Back-End-Projekt

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Serverprojekt, und klicken Sie auf **NuGet-Pakete verwalten**. Suchen Sie „Microsoft.Azure.Mobile.NotificationHubs“, und klicken Sie anschließend auf **Installieren**. Dadurch wird die Notification Hubs-Clientbibliothek installiert.

2. Erweitern Sie **Controller**, öffnen Sie „TodoItemController.cs“, und fügen Sie die folgenden using-Anweisungen hinzu:

		using System.Collections.Generic;
		using Microsoft.Azure.NotificationHubs;
		using Microsoft.Azure.Mobile.Server.Config;

3. Fügen Sie in der **PostTodoItem**-Methode den folgenden Code nach dem Aufruf von **InsertAsync** hinzu:

	    // Get the settings for the server project.
	    HttpConfiguration config = this.Configuration;
	    MobileAppSettingsDictionary settings =
	        this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

	    // Get the Notification Hubs credentials for the Mobile App.
	    string notificationHubName = settings.NotificationHubName;
	    string notificationHubConnection = settings
	        .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

	    // Create the notification hub client.
	    NotificationHubClient hub = NotificationHubClient
	        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

	    // Define a WNS payload
	    var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
	                            + item.Text + @"</text></binding></visual></toast>";
	    try
	    {
	        // Send the push notification.
	        var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

	        // Write the success result to the logs.
	        config.Services.GetTraceWriter().Info(result.State.ToString());
	    }
	    catch (System.Exception ex)
	    {
	        // Write the failure result to the logs.
	        config.Services.GetTraceWriter()
	            .Error(ex.Message, null, "Push.SendAsync Error");
	    }

	Dieser Code weist das Benachrichtigungshub an, nach dem Einfügen eines neuen Elements eine Pushbenachrichtigung zu senden.

4. Veröffentlichen Sie das Serverprojekt erneut.

### <a name="nodejs"></a>Node.js-Back-End-Projekt

1. Falls nicht bereits geschehen, [laden Sie das Schnellstartprojekt herunter](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) oder verwenden Sie den [Online-Editor im Azure-Portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Ersetzen Sie den bestehenden Code in der Datei „todoitem.js“ durch den folgenden Code:

		var azureMobileApps = require('azure-mobile-apps'),
	    promises = require('azure-mobile-apps/src/utilities/promises'),
	    logger = require('azure-mobile-apps/src/logger');

		var table = azureMobileApps.table();

		table.insert(function (context) {
	    // For more information about the Notification Hubs JavaScript SDK,
	    // see http://aka.ms/nodejshubs
	    logger.info('Running TodoItem.insert');

	    // Define the WNS payload that contains the new item Text.
	    var payload = "<toast><visual><binding template=\ToastText01><text id="1">"
		                            + context.item.text + "</text></binding></visual></toast>";

	    // Execute the insert.  The insert returns the results as a Promise,
	    // Do the push as a post-execute action within the promise flow.
	    return context.execute()
	        .then(function (results) {
	            // Only do the push if configured
	            if (context.push) {
					// Send a WNS native toast notification.
	                context.push.wns.sendToast(null, payload, function (error) {
	                    if (error) {
	                        logger.error('Error while sending push notification: ', error);
	                    } else {
	                        logger.info('Push notification sent successfully!');
	                    }
	                });
	            }
	            // Don't forget to return the results from the context.execute()
	            return results;
	        })
	        .catch(function (error) {
	            logger.error('Error while running context.execute: ', error);
	        });
		});

		module.exports = table;

	Damit wird eine WNS-Popupbenachrichtigung gesendet, die „item.text“ enthält, wenn ein neues Todo-Element eingefügt wird.

2. Beim Bearbeiten der Datei auf Ihrem lokalen Computer veröffentlichen Sie das Serverprojekt erneut.

##<a id="update-app"></a>Hinzufügen von Pushbenachrichtigungen zur App

Als nächstes muss die App beim Start für Pushbenachrichtigungen registriert werden. Wenn Sie die Authentifizierung bereits aktiviert haben, achten Sie darauf, dass sich der Benutzer anmeldet, bevor die Registrierung für Pushbenachrichtigungen ausgeführt wird. Weitere Informationen finden Sie unter [Authenticate first](https://github.com/Azure-Samples/app-service-mobile-windows-quickstart/blob/master/README.md#authenticate-first) (Zuerst Authentifizieren) im vollständigen Schnellstartbeispiel.

1. Öffnen Sie die Projektdatei **App.xaml.cs**, und fügen Sie die folgenden `using`-Anweisungen hinzu:

		using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Fügen Sie in derselben Datei der **App**-Klasse die folgende **InitNotificationsAsync**-Methodendefinition hinzu:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Dieser Code ruft "ChannelURI" für die App aus WNS ab und registriert anschließend "ChannelURI" für Ihre mobile App Service-App.

3. Fügen Sie in der Datei **App.xaml.cs** am Anfang des **OnLaunched**-Ereignishandlers den **async**-Modifizierer zur Methodendefinition hinzu, und fügen Sie den folgenden Aufruf wie im Beispiel unten zu der neuen **InitNotificationsAsync**-Methode hinzu:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

			// ...
		}

    Dadurch wird sichergestellt, dass der kurzlebige ChannelURI-Wert bei jedem Anwendungsstart registriert wird.

4. Erstellen Sie Ihr UWP-App-Projekt erneut. Ihre App kann jetzt Popupbenachrichtigungen empfangen.

##<a id="test"></a>Testen von Pushbenachrichtigungen in der App

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Nächste Schritte

Informieren Sie sich ausführlicher über Pushbenachrichtigungen:

* [Verwenden des verwalteten Clients für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications): Mithilfe von Vorlagen können Sie flexibel plattformübergreifende Pushvorgänge und lokalisierte Pushvorgänge senden. Hier erfahren Sie, wie Sie Vorlagen registrieren.

* [Verwenden des .NET-Back-End-Server-SDKs für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-add-tags-to-a-device-installation-to-enable-push-to-tags): Mit Tags können Sie Pushvorgänge auf segmentierte Kunden ausrichten. Hier erfahren Sie, wie Sie einer Geräteinstallation Tags hinzufügen.

* [Azure Notification Hubs - Diagnosis guidelines](../notification-hubs/notification-hubs-push-notification-fixer.md) (Azure Notification Hubs – Diagnoserichtlinien): Benachrichtigungen können aus unterschiedlichen Gründen verloren gehen oder nicht bei Geräten ankommen. In diesem Thema erfahren Sie, wie Sie die Ursache für Fehler bei Pushbenachrichtigungen analysieren und ermitteln.

Sie können mit einem der folgenden Tutorials fortfahren:

+ [Hinzufügen von Authentifizierung zur App](app-service-mobile-windows-store-dotnet-get-started-users.md) Hier erhalten Sie Informationen zur Authentifizierung von Benutzern der App mit einem Identitätsanbieter.

+ [Aktivieren der Offlinesynchronisierung für Ihre App](app-service-mobile-windows-store-dotnet-get-started-offline-data.md) Hier erfahren Sie, wie Sie Ihrer App mithilfe eines Mobile App-Back-Ends Offlineunterstützung hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (Anzeigen, Hinzufügen und Ändern von Daten), auch wenn keine Netzwerkverbindung vorhanden ist.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

<!---HONumber=AcomDC_0629_2016-->