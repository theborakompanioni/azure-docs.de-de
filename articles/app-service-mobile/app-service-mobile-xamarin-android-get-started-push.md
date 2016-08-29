<properties
	pageTitle="Hinzufügen von Pushbenachrichtigungen zur Xamarin.Android-App | Azure App Service"
	description="Erfahren Sie, wie Sie Azure App Service und Azure Notification Hubs verwenden, um Pushbenachrichtigungen an Ihre Xamarin.Android-App zu senden."
	services="app-service\mobile"
	documentationCenter="xamarin"
	authors="ggailey777"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-xamarin-android"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="08/12/2016"
	ms.author="glenga"/>

# Hinzufügen von Pushbenachrichtigungen zur Xamarin.Android-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##Übersicht

In diesem Tutorial fügen Sie Pushbenachrichtigungen zum Projekt [Erstellen einer Xamarin.Android-App] hinzu, damit bei jedem Einfügen eines Datensatzes eine Pushbenachrichtigung gesendet wird. Dieses Tutorial baut auf dem Tutorial [Erstellen einer Xamarin.Android-App] auf, das Sie zuerst absolvieren müssen. Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Pushbenachrichtigungs-Erweiterungspaket hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) (in englischer Sprache).

##Voraussetzungen

Für dieses Lernprogramm ist Folgendes erforderlich:

+ Ein aktives Google-Konto Sie können sich unter [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302) für ein Google-Konto registrieren.

+ [Google Cloud Messaging Client-Komponente](http://components.xamarin.com/view/GCMClient/) Sie fügen diese Komponente während des Lernprogramms hinzu.

##<a name="create-hub"></a>Erstellen eines Notification Hubs

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

##<a id="register"></a>Aktivieren von Google Cloud Messaging

[AZURE.INCLUDE [Aktivieren von GCM](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##Konfigurieren des mobilen App-Back-Ends zum Senden von Pushanforderungen

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

##<a id="update-server"></a>Aktualisieren des Serverprojekts zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Konfigurieren des Clientprojekts für Pushbenachrichtigungen

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Hinzufügen von Pushbenachrichtigungen zur App

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Testen von Pushbenachrichtigungen in der App

Sie können die App mithilfe eines virtuellen Geräts im Emulgator testen. Es sind zusätzliche Konfigurationsschritte erforderlich, wenn für das Ausführen ein Emulgator verwendet wird.

1. Stellen Sie sicher, dass die Bereitstellung oder das Debuggen auf einem virtuellen Gerät stattfindet, bei dem Google-APIs als Ziel festgelegt sind, wie unten beim Android Virtual Device (AVD)-Manager dargestellt.

	![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Fügen Sie ein Google-Konto auf dem Android-Gerät hinzu, indem Sie auf **Apps** > **Einstellungen** > **Konto hinzufügen** klicken und dann den Anweisungen folgen, um für die Erstellung eines neuen ein vorhandenes Google-Konto zum Gerät hinzuzufügen.

	![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Führen Sie wie zuvor die todolist-App aus und fügen Sie ein neues todo-Element ein. Dieses Mal wird im Benachrichtigungsbereich ein Benachrichtigungssymbol angezeigt. Öffnen Sie die Benachrichtigungsschublade, um den vollständigen Text der Benachrichtigung anzuzeigen.

	![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quickstart]: app-service-mobile-xamarin-android-get-started.md
[Erstellen einer Xamarin.Android-App]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/

<!---HONumber=AcomDC_0817_2016-->