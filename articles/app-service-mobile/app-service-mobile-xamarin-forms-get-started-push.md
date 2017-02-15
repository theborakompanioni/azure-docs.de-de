---
title: "Hinzufügen von Pushbenachrichtigungen zur Xamarin.Forms-App | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Azure-Dienste verwenden, um plattformübergreifende Pushbenachrichtigungen an Ihre Xamarin.Forms-Apps zu senden."
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: dwrede
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: f6989f65f80027be7803a889c0a87f6afbae150d
ms.openlocfilehash: 703ab64e96894df6c974fd441b737568fb905004


---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Hinzufügen von Pushbenachrichtigungen zur Xamarin.Forms-App
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Übersicht
In diesem Tutorial fügen Sie allen Projekten, die auf dem [Xamarin.Forms-Schnellstart](app-service-mobile-xamarin-forms-get-started.md) basieren, Pushbenachrichtigungen hinzu. Dies bedeutet, dass immer dann eine Pushbenachrichtigung an alle plattformübergreifenden Clients gesendet wird, wenn ein Eintrag eingefügt wird.

Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Erweiterungspaket für Pushbenachrichtigungen hinzufügen. Weitere Informationen finden Sie unter [Arbeiten mit dem .NET Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen
Für iOS benötigen Sie eine [Apple-Entwicklerprogramm-Mitgliedschaft](https://developer.apple.com/programs/ios/) und ein physisches iOS-Gerät. Der [iOS-Simulator unterstützt keine Pushbenachrichtigungen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="a-nameconfigure-hubaconfigure-a-notification-hub"></a><a name="configure-hub"></a>Konfigurieren eines Notification Hubs
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Aktualisieren des Serverprojekts zum Senden von Pushbenachrichtigungen
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Konfigurieren und Ausführen des Android-Projekts (optional)
Arbeiten Sie diesen Abschnitt durch, um Pushbenachrichtigungen für das Xamarin.Forms-Droid-Projekt für Android zu aktivieren.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Aktivieren von Firebase Cloud Messaging (FCM)
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>Konfigurieren des Mobile Apps-Back-Ends zum Senden von Pushanforderungen per FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Hinzufügen von Pushbenachrichtigungen zum Android-Projekt
Wenn das Back-End per FCM konfiguriert wurde, können Sie dem Client für die Registrierung bei FCM Komponenten und Codes hinzufügen. Außerdem können Sie die Registrierung für Pushbenachrichtigungen mit Azure Notification Hubs über das Mobile Apps-Back-End durchführen und Benachrichtigungen erhalten.

1. Klicken Sie im Projekt **Droid** mit der rechten Maustaste auf den Ordner **Components**, und klicken Sie dann auf **Get More Components...**. Suchen Sie dann nach der Komponente **Google Cloud Messaging Client**, und fügen Sie sie dem Projekt hinzu. Diese Komponente unterstützt Pushbenachrichtigungen für ein Xamarin Android-Projekt.
2. Öffnen Sie die Projektdatei „MainActivity.cs“, und fügen Sie am Anfang der Datei die folgende Anweisung hinzu:

        using Gcm.Client;
3. Fügen Sie den folgenden Code der **OnCreate**-Methode nach dem Aufruf von **LoadApplication** hinzu:

        try
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. Fügen Sie wie folgt eine neue **CreateAndShowDialog** -Hilfsmethode hinzu:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Fügen Sie der **MainActivity** -Klasse den folgenden Code hinzu:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Hierdurch wird die aktuelle **MainActivity**-Instanz verfügbar gemacht, um die Ausführung im UI-Hauptthread zu ermöglichen.
6. Initialisieren Sie die Variable `instance` am Anfang der **OnCreate**-Methode wie folgt:

        // Set the current instance of MainActivity.
        instance = this;
7. Fügen Sie dem Projekt **Droid** eine neue Klassendatei mit dem Namen `GcmService.cs` hinzu, und stellen Sie sicher, dass oben in der Datei die folgenden **using**-Anweisungen vorhanden sind:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. Fügen Sie am Anfang der Datei die folgenden Berechtigungsanforderungen nach den **using**-Anweisungen und vor der **namespace**-Deklaration hinzu.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. Fügen Sie dem Namespace die folgende Klassendefinition hinzu.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Ersetzen Sie **<PROJECT_NUMBER>** durch die zuvor notierte Projektnummer.    
   >
   >
10. Ersetzen Sie die leere **GcmService** -Klasse durch den folgenden Code, in dem der neue Übertragungsempfänger verwendet wird:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Fügen Sie der **GcmService**-Klasse den folgenden Code hinzu. Der **OnRegistered**-Ereignishandler wird überschrieben, und es wird eine **Register**-Methode implementiert.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration.
12. Fügen Sie den folgenden Code hinzu, mit dem **OnMessage**implementiert wird:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Hiermit werden eingehende Benachrichtigungen behandelt und zum Anzeigen an den Benachrichtigungs-Manager gesendet.
13. Für **GcmServiceBase** ist es auch erforderlich, die Handlermethoden **OnUnRegistered** und **OnError** zu implementieren. Dies ist wie folgt möglich:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Sie können nun Pushbenachrichtigungen in der App testen, die auf einem Android-Gerät oder im Emulator ausgeführt wird.

### <a name="test-push-notifications-in-your-android-app"></a>Testen von Pushbenachrichtigungen in der Android-App
Die ersten beiden Schritte sind nur beim Testen in einem Emulator erforderlich.

1. Stellen Sie sicher, dass die Bereitstellung oder das Debuggen auf einem virtuellen Gerät stattfindet, bei dem Google-APIs als Ziel festgelegt sind. Dies ist unten für den Android Virtual Device-Manager dargestellt.
2. Fügen Sie auf dem Android-Gerät ein Google-Konto hinzu, indem Sie auf **Apps** > **Einstellungen** > **Konto hinzufügen** klicken. Befolgen Sie anschließend die Anweisungen zum Hinzufügen eines vorhandenen Google-Kontos zum Gerät bzw. zum Erstellen eines neuen Kontos.
3. Klicken Sie in Visual Studio oder Xamarin Studio mit der rechten Maustaste auf das Projekt **Droid**, und klicken Sie dann auf **Als Startprojekt festlegen**.
4. Klicken Sie auf **Ausführen**, um das Projekt zu erstellen und die App auf Ihrem Android-Gerät oder im Emulator zu starten.
5. Geben Sie in der App eine Aufgabe ein, und klicken Sie dann auf das Pluszeichen (**+**).
6. Überprüfen Sie, ob beim Hinzufügen eines Elements eine Benachrichtigung empfangen wird.

## <a name="configure-and-run-the-ios-project-optional"></a>Konfigurieren und Ausführen des iOS-Projekts (optional)
Dieser Abschnitt bezieht sich auf das Ausführen des Xamarin iOS-Projekts für iOS-Geräte. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>Konfigurieren des Notification Hubs für APNS
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Im nächsten Schritt konfigurieren Sie die iOS-Projekteinstellung in Xamarin Studio oder Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer iOS-App
1. Öffnen Sie im Projekt **iOS** die Datei „AppDelegate.cs“, und fügen Sie oben in der Codedatei die folgende Anweisung hinzu.

        using Newtonsoft.Json.Linq;
2. Fügen Sie der **AppDelegate**-Klasse eine Überschreibung für das **RegisteredForRemoteNotifications**-Ereignis hinzu, um sich für Benachrichtigungen zu registrieren:

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. Fügen Sie in **AppDelegate** auch die folgende Überschreibung für den **DidReceiveRemoteNotification**-Ereignishandler hinzu:

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Diese Methode verarbeitet eingehende Benachrichtigungen, während die App ausgeführt wird.
4. Fügen Sie in der **AppDelegate**-Klasse der **FinishedLaunching**-Methode den folgenden Code hinzu:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Dies ermöglicht die Unterstützung von Remotebenachrichtigungen und fordert die Pushregistrierung an.

Ihre App kann Pushbenachrichtigungen nun unterstützen.

#### <a name="test-push-notifications-in-your-ios-app"></a>Testen von Pushbenachrichtigungen in der iOS-App
1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und klicken Sie auf **Als Startprojekt festlegen**.
2. Klicken Sie auf die Schaltfläche **Ausführen**, oder drücken Sie in Visual Studio die Taste **F5**, um das Projekt zu erstellen und die App in einem iOS-Gerät zu starten. Klicken Sie dann auf **OK**, um Pushbenachrichtigungen zu akzeptieren.

   > [!NOTE]
   > Sie müssen Pushbenachrichtigungen von Ihrer App ausdrücklich akzeptieren. Diese Anforderung tritt nur beim ersten Lauf der App auf.
   >
   >
3. Geben Sie in der App eine Aufgabe ein, und klicken Sie dann auf das Pluszeichen (**+**).
4. Stellen Sie sicher, dass Sie eine Benachrichtigung erhalten haben, und klicken Sie dann auf **OK**, um diese zu schließen.

## <a name="configure-and-run-windows-projects-optional"></a>Konfigurieren und Ausführen von Windows-Projekten (optional)
In diesem Abschnitt geht es um das Ausführen der Projekte „Xamarin.Forms WinApp“ und „WinPhone81“ für Windows-Geräte. Diese Schritte eignen sich auch für UWP-Projekte (Universelle Windows-Plattform). Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Registrieren der Windows-App für Pushbenachrichtigungen mit dem Windows-Benachrichtigungsdienst (Windows Notification Service, WNS)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>Konfigurieren des Notification Hubs für WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer Windows-App
1. Öffnen Sie in Visual Studio in einem Windows-Projekt die Datei **App.xaml.cs**, und fügen Sie die folgenden Anweisungen hinzu.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Ersetzen Sie `<your_TodoItemManager_portable_class_namespace>` durch den Namespace Ihres portierbaren Projekts, das die `TodoItemManager`-Klasse enthält.
2. Fügen Sie der Datei „App.xaml.cs“ die folgende **InitNotificationsAsync**-Methode hinzu:

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Diese Methode ruft den Pushbenachrichtigungskanal ab und registriert eine Vorlage für das Empfangen von Vorlagenbenachrichtigungen von Ihrem Notification Hub. Eine Vorlagenbenachrichtigung, die *messageParam* unterstützt, wird an diesen Client übermittelt.
3. Aktualisieren Sie in „App.xaml.cs“ die Definition der Ereignishandlermethode **OnLaunched**, indem Sie den Modifizierer `async` hinzufügen. Fügen Sie am Ende der Methode die folgende Codezeile hinzu:

        await InitNotificationsAsync();

    So wird sichergestellt, dass die Registrierung der Pushbenachrichtigung bei jedem Start der App erstellt oder aktualisiert wird. Dies ist wichtig, um sicherzustellen, dass der WNS-Pushkanal immer aktiv ist.  
4. Öffnen Sie im Projektmappen-Explorer von Visual Studio die Datei **Package.appxmanifest**, und legen Sie unter **Benachrichtigungen** für **Toastfähig** die Option **Ja** fest.
5. Erstellen Sie die App, und stellen Sie sicher, dass keine Fehler auftreten. Sie sollten nun Ihre Client-App für die Vorlagenbenachrichtigungen vom Mobile Apps-Back-End registrieren. Wiederholen Sie die Schritte in diesem Abschnitt für alle Windows-Projekte Ihrer Projektmappe.

#### <a name="test-push-notifications-in-your-windows-app"></a>Testen von Pushbenachrichtigungen in Ihrer Windows-App
1. Klicken Sie in Visual Studio mit der rechten Maustaste auf ein Windows-Projekt, und klicken Sie auf **Als Startprojekt festlegen**.
2. Klicken Sie auf die Schaltfläche **Ausführen** , um das Projekt zu erstellen und die App zu starten.
3. Geben Sie in der App einen Namen für ein neues TodoItem ein, und klicken Sie dann auf das Pluszeichen (**+**), um es hinzuzufügen.
4. Überprüfen Sie, ob beim Hinzufügen des Elements eine Benachrichtigung empfangen wird.

## <a name="next-steps"></a>Nächste Schritte
Sie können sich ausführlicher über Pushbenachrichtigungen informieren:

* [Azure Notification Hubs - Diagnosis guidelines](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  (Azure Notification Hubs – Diagnoserichtlinien): Benachrichtigungen können aus unterschiedlichen Gründen verloren gehen oder nicht bei Geräten ankommen. In diesem Thema wird gezeigt, wie Sie die Grundursache von Pushbenachrichtigungsfehlern analysieren und ermitteln.

Außerdem können Sie mit einem der folgenden Tutorials fortfahren:

* [Hinzufügen der Authentifizierung zu Ihrer App ](app-service-mobile-xamarin-forms-get-started-users.md)  
  Hier erhalten Sie Informationen zur Authentifizierung von Benutzern der App bei einem Identitätsanbieter.
* [Aktivieren der Offlinesynchronisierung für Ihre App](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informieren Sie sich, wie Sie die Offlineunterstützung für Ihre App mit einem Mobile Apps-Back-End hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (also das Anzeigen, Hinzufügen oder Ändern von Daten) auch dann, wenn keine Netzwerkverbindung besteht.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333



<!--HONumber=Dec16_HO2-->


