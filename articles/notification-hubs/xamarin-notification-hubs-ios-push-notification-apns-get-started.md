---
title: "iOS-Pushbenachrichtigungen mit Notification Hubs für Xamarin-Apps | Microsoft Docs"
description: In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs eine Pushbenachrichtigung an eine Xamarin.iOS-Anwendung senden.
services: notification-hubs
keywords: iOS-Pushbenachrichtigungen,Pushnachrichten,Pushbenachrichtigungen,Pushnachricht
documentationcenter: xamarin
author: ysxu
manager: erikre
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 72a81fa0deb34ace77b8fb9b1a4e6b24ee164b35
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# iOS-Pushbenachrichtigungen mit Notification Hubs für Xamarin-Apps
<a id="ios-push-notifications-with-notification-hubs-for-xamarin-apps" class="xliff"></a>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## Übersicht
<a id="overview" class="xliff"></a>
> [!IMPORTANT]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure Notification Hubs Pushbenachrichtigungen an eine iOS-App senden.
Sie erstellen eine leere Xamarin.iOS-App, die Pushbenachrichtigungen mithilfe des [Apple-Pushbenachrichtigungsdiensts (Apple Push Notification Service, APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)empfängt. Sobald Sie dieses Lernprogramm abgeschlossen haben, können Sie über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte senden, die Ihre App ausführen. Der komplette Code ist in der [NotificationHubs-Beispiel-App][GitHub] verfügbar.

Im Lernprogramm wird ein einfaches Übertragungsszenario für Pushbenachrichtigungen mithilfe von Notification Hubs veranschaulicht.

## Voraussetzungen
<a id="prerequisites" class="xliff"></a>
Für dieses Lernprogramm ist Folgendes erforderlich:

* [Xcode 6.0][Install Xcode]
* Ein Gerät, das mit iOS 7.0 (oder einer neueren Version) kompatibel ist
* iOS-Entwicklerprogramm-Mitgliedschaft
* [Xamarin Studio]
  
  > [!NOTE]
  > Die Beispielanwendung muss aufgrund von Konfigurationsanforderungen für iOS-Pushbenachrichtigungen auf einem physischem iOS-Gerät (iPhone oder iPad) bereitgestellt und getestet werden, anstatt im Simulator.
  > 
  > 

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Notification Hubs-Lernprogramme für Xamarin.iOS-Apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## Konfigurieren Ihres Notification Hub
<a id="configure-your-notification-hub" class="xliff"></a>
Dieser Abschnitt führt Sie durch das Erstellen eines neuen Notification Hubs und Konfigurieren der Authentifizierung mit APNs mithilfe des **.p12** -Pushzertifikats, das Sie erstellt haben. Wenn Sie einen bereits erstellten Notification Hub verwenden möchten, können Sie zu Schritt 5 springen.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p>Öffnen Sie zum Konfigurieren der APNS-Verbindung im Azure-Portal die Einstellungen für Notification Hub, und klicken Sie auf <b>Notification Services</b> und dann in der Liste auf den Eintrag <b>Apple (APNS)</b>. Klicken Sie anschließend auf <b>Zertifikat hochladen</b>, und wählen Sie das Zertifikat <b>.p12</b>, das Sie oben exportiert haben, sowie das Kennwort für das Zertifikat aus.</p>

<p>Achten Sie darauf, den Modus <b>Sandbox</b> auszuwählen, da Sie Pushnachrichten in einer Entwicklungsumgebung senden. Wählen Sie die Einstellung <b>Produktion</b> nur aus, wenn Sie Pushbenachrichtigungen an Benutzer senden möchten, die Ihre App bereits im Store erworben haben.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

Der Notification Hub ist jetzt für die Arbeit mit APNS konfiguriert, und Sie besitzen die Verbindungszeichenfolge, um die App zu registrieren und Pushbenachrichtigungen zu versenden.

## Verbinden Ihrer App mit dem Notification Hub
<a id="connect-your-app-to-the-notification-hub" class="xliff"></a>
#### Erstellen eines neuen Projekts
<a id="create-a-new-project" class="xliff"></a>
1. Erstellen Sie in Xamarin Studio ein neues iOS-Projekt, und wählen Sie die Vorlage **Unified API** > **Single View Application** aus.
   
     ![Xamarin Studio – Anwendungstyp auswählen][31]
2. Fügen Sie einen Verweis auf die Azure Messaging-Komponente hinzu. Klicken Sie in der Lösungsansicht mit der rechten Maustaste auf den Ordner **Components** für Ihr Projekt, und wählen Sie **Get More Components**. Suchen Sie nach der Komponente **Azure Messaging** , und fügen Sie sie Ihrem Projekt hinzu.
3. Fügen Sie die folgende Anweisung zur Datei **AppDelegate.cs**hinzu:
   
        using WindowsAzure.Messaging;
4. Erstellen Sie eine Instanz von **SBNotificationHub**:
   
        private SBNotificationHub Hub { get; set; }
5. Erstellen Sie eine Klasse **Constants.cs** mit den folgenden Variablen:
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. Ändern Sie **FinishedLaunching()** in **AppDelegate.cs** wie folgt:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
7. Überschreiben Sie die **RegisteredForRemoteNotifications()**-Methode in **AppDelegate.cs**:
   
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
8. Überschreiben Sie die **ReceivedRemoteNotification()**-Methode in **AppDelegate.cs**:
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. Erstellen Sie die folgende **ProcessNotification()**-Methode in **AppDelegate.cs**:
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
   
   > [!NOTE]
   > Sie können **FailedToRegisterForRemoteNotifications()** überschreiben, um Situationen wie beispielsweise eine fehlende Netzwerkverbindung zu behandeln. Dies ist besonders wichtig, wenn Benutzer Ihre Anwendung ggf. im Offlinemodus starten (z. B. im Flugzeug) und Sie die Vorgehensweise bei der Behandlung von Pushnachrichten für Ihre App vorgeben möchten.
   > 
   > 
10. Führen Sie die App auf Ihrem Gerät aus.

## Senden von Pushbenachrichtigungen
<a id="sending-push-notifications" class="xliff"></a>
Sie können den Empfang von Pushbenachrichtigungen in Ihrer App testen, indem Sie Benachrichtigungen im [Azure-Portal] über die Funktion **Testsendung** im Toolset **Problembehandlung** senden. Die Funktion befindet sich direkt auf der Notification Hub-Seite, wie in der folgenden Abbildung dargestellt.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Pushbenachrichtigungen werden normalerweise über einen Back-End-Dienst mit einer kompatiblen Bibliothek gesendet, z. B. Mobile Services oder ASP.NET. Sie können Pushnachrichten auch direkt über die REST-API senden, wenn in Ihrem Fall keine Bibliothek verfügbar ist. 

In diesem Lernprogramm gehen wir einfach vor und veranschaulichen nur das Testen der Client-App, indem wir Benachrichtigungen mit dem .NET SDK für Notification Hubs in einer Konsolenanwendung senden, anstatt über einen Back-End-Dienst. Es wird empfohlen, das Tutorial [Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) als nächsten Schritt zum Senden von Benachrichtigungen von einem ASP.NET-Back-End zu nutzen. Sie können aber die folgenden Vorgehensweisen zum Senden von Benachrichtigungen verwenden:

* **REST-Schnittstelle**: Sie können Pushbenachrichtigungen auf allen Back-End-Plattformen unterstützen, indem Sie die [REST-Schnittstelle](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) verwenden.
* **Microsoft Azure Notification Hubs .NET SDK**: Führen Sie im Nuget-Paket-Manager für Visual Studio die Option [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)aus.
* **Node.js** : [Verwenden von Notification Hubs mit Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile Apps**: Ein Beispiel für das Senden von Benachrichtigungen von einem Back-End für Azure Mobile App Service-Apps, das in Notification Hubs integriert ist, finden Sie unter [Hinzufügen von Pushbenachrichtigungen zu Ihrer App](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java/PHP**: Ein Beispiel zum Senden von Pushbenachrichtigungen über die REST-APIs finden Sie unter „Verwenden von Notification Hubs von Java/PHP“ ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

#### (Optional) Senden von Pushbenachrichtigungen aus einer .NET-Konsolen-App
<a id="optional-send-push-notifications-from-a-net-console-app" class="xliff"></a>
In diesem Abschnitt senden Sie Pushbenachrichtigungen mithilfe einer einfachen .NET-Konsolen-App. In diesem Beispiel wechseln wir zu einer Windows-Entwicklungsumgebung, für die Visual Studio bereits installiert ist.

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolenanwendung:
   
       ![Visual Studio - Create a new console application][213]
2. Klicken Sie in Visual Studio auf **Extras**, auf **NuGet-Paket-Manager** und dann auf **Paket-Manager-Konsole**.
   
    Die Paket-Manager-Konsole sollte angedockt am unteren Rand Ihres Visual Studio-Arbeitsbereichs angezeigt werden.
3. Legen Sie im Fenster der Paket-Manager-Konsole als **Standardprojekt** das neue Konsolenanwendungsprojekt fest, und führen Sie dann im Konsolenfenster den folgenden Befehl aus:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Dies fügt mithilfe des <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.NotificationHubs-NuGet-Pakets</a> einen Verweis auf das Azure Notification Hubs-SDK hinzu.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Öffnen Sie die Datei `Program.cs`, und fügen Sie die folgende `using`-Anweisung hinzu. So wird sichergestellt, dass wir Azure-Klassen und -Funktionen in Ihrer main-Klasse verwenden können:
   
        using Microsoft.Azure.NotificationHubs;
5. Fügen Sie in Ihrer `Program`-Klasse die folgende Methode hinzu (Vergessen Sie nicht, **connection string** (Verbindungszeichenfolge) und **hub name** (Hub-Name) zu ersetzen):
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. Fügen Sie folgende Zeilen zur `Main` -Methode hinzu:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Drücken Sie die Taste F5, um die App auszuführen. Nach wenigen Sekunden sollte auf Ihrem Gerät eine Pushbenachrichtigung angezeigt werden. Stellen Sie unabhängig davon, ob Sie WLAN oder ein Mobilfunknetz nutzen, sicher, dass Sie auf dem Gerät über eine aktive Internetverbindung verfügen.

Sie finden alle möglichen Nutzlasten im [Apple Local and Push Notification Programming Guide](in englischer Sprache).

#### (Optional) Senden von Benachrichtigungen aus einem mobilen Service
<a id="optional-send-notifications-from-a-mobile-service" class="xliff"></a>
In diesem Abschnitt senden wir Pushbenachrichtigungen mithilfe eines mobilen Service per Knotenskript.

Zum Senden einer Benachrichtigung mithilfe eines mobilen Service folgen Sie den Anweisungen unter [Erste Schritte mit Mobile Services]. Gehen Sie dann wie folgt vor:

1. Melden Sie sich beim [klassischen Azure-Portal]an, und wählen Sie Ihren mobilen Dienst aus.
2. Wählen Sie oben die Registerkarte **Scheduler** aus.
   
       ![Azure Classic Portal - Scheduler][215]
3. Erstellen Sie einen neuen geplanten Auftrag. Fügen Sie einen Namen ein, und klicken Sie dann auf **On demand**.
   
       ![Azure Classic Portal - Create new job][216]
4. Wenn der Auftrag erstellt ist, klicken Sie auf den Auftragsnamen. Klicken Sie anschließend in der Kopfleiste auf die Registerkarte **Skript** .
5. Fügen Sie das folgende Skript in Ihre Scheduler-Funktion ein. Stellen Sie sicher, dass Sie die Platzhalter durch den Notification Hub-Namen und die Verbindungszeichenfolge für *DefaultFullSharedAccessSignature* ersetzen, die Sie zuvor erhalten haben. Klicken Sie auf **Speichern**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                  "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );
6. Klicken Sie in der unteren Leiste auf **Run Once** . Sie sollten eine Warnung auf Ihrem Gerät erhalten.

## Nächste Schritte
<a id="next-steps" class="xliff"></a>
In diesem einfachen Beispiel haben Sie Pushbenachrichtigungen an alle Ihre iOS-Geräte übertragen. Informationen zum Adressieren bestimmter Benutzer finden Sie in dem Lernprogramm [Benachrichtigen von Benutzern mit Benachrichtigungshubs]. Wenn Sie Ihre Benutzer in Interessengruppen einteilen möchten, finden Sie unter [Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]weitere Informationen. Weitere Informationen zur Verwendung von Notification Hubs finden Sie unter [Notification Hubs-Leitfaden] und [Notification Hubs-Informationen für iOS].

<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Erste Schritte mit Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[klassischen Azure-Portal]: https://manage.windowsazure.com/
[Notification Hubs-Leitfaden]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs-Informationen für iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Benachrichtigen von Benutzern mit Benachrichtigungshubs]: /manage/services/notification-hubs/notify-users-aspnet
[Verwenden von Benachrichtigungshubs zum Übermitteln von Nachrichten]: /manage/services/notification-hubs/breaking-news-dotnet

[Apple Local and Push Notification Programming Guide]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure-Portal]: https://portal.azure.com

