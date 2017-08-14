---
title: "Erste Schritte mit Azure Mobile Engagement für Xamarin.iOS"
description: "Erfahren Sie mehr über die Verwendung von Azure Mobile Engagement mit Analysefunktionen und Pushbenachrichtigungen für Xamarin.iOS-Apps."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0448209e-fff6-47bd-985c-2cf074bac12f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 9938c3e994acf31244825b1afb347f8c9f90ebe3
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Erste Schritte mit Azure Mobile Engagement für Xamarin.iOS-Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema erfahren Sie, wie Sie mithilfe von Azure Mobile Engagement die Nutzung Ihrer App nachvollziehen und Pushbenachrichtigungen an segmentierte Benutzer in einer Xamarin.iOS-Anwendung senden.
In diesem Tutorial erstellen Sie eine leere Xamarin.iOS-App, die einfache Daten erfasst und Pushbenachrichtigungen mithilfe des Apple-Pushbenachrichtigungsdiensts (APNS) empfängt.

> [!NOTE]
> Der Azure Mobile Engagement-Dienst wird im März 2018 eingestellt und ist zurzeit nur für Bestandskunden verfügbar. Weitere Informationen finden Sie im Artikel zu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Für dieses Lernprogramm ist Folgendes erforderlich:

* [Xamarin Studio](http://xamarin.com/studio). Sie können auch Visual Studio mit Xamarin verwenden, in den Anweisungen in diesem Tutorial wird jedoch Xamarin Studio eingesetzt. Installationsanweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).
> 
> 

## <a id="setup-azme"></a>Einrichten von Mobile Engagement für Ihre iOS-App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
In diesem Lernprogramm wird eine „einfache Integration“ dargestellt. Dabei handelt es sich um den minimalen erforderlichen Satz zur Sammlung von Daten und zum Senden einer Pushbenachrichtigung.

Wir erstellen eine einfache App mit Xamarin, um die Integration zu veranschaulichen:

### <a name="create-a-new-xamarinios-project"></a>Erstellen eines neuen Xamarin.iOS-Projekts
1. Starten Sie Xamarin Studio. Navigieren Sie zu **Datei** -> **Neu** -> **Lösung**. 
   
    ![][1]
2. Wählen Sie **Single View Application**, stellen Sie sicher, dass als Sprache **C#** ausgewählt ist, und klicken Sie dann auf **Weiter**.
   
    ![][2]
3. Geben Sie **App-Name** und **Organisations-ID** ein, und klicken Sie dann auf **Weiter**. 
   
    ![][3]
   
   > [!IMPORTANT]
   > Stellen Sie sicher, dass das Veröffentlichungsprofil, mit dem Sie die iOS-App später bereitstellen, eine App-ID verwendet, die genau der hier angegebenen Paket-ID entspricht. 
   > 
   > 
4. Aktualisieren Sie ggf. **Projektname**, **Projektmappenname** und **Speicherort**, und klicken Sie auf **Erstellen**.
   
    ![][4]

Xamarin Studio erstellt die Demo-App, in die wir Mobile Engagement integrieren. 

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End.
1. Klicken Sie im Projektmappenfenster mit der rechten Maustaste auf den Ordner **Pakete**, und wählen Sie **Pakete hinzufügen...**.
   
    ![][5]
2. Suchen Sie nach dem **Microsoft Azure Mobile Engagement Xamarin SDK** , und fügen Sie es zur Projektmappe hinzu.  
   
    ![][6]
3. Öffnen Sie **AppDelegate.cs** , und fügen Sie die folgende using-Anweisung hinzu:
   
        using Microsoft.Azure.Engagement.Xamarin;
4. Fügen Sie in der **FinishedLaunching** -Methode Folgendes hinzu, um die Verbindung mit dem Mobile Engagement-Back-End zu initialisieren. Fügen Sie auch **ConnectionString**hinzu. In diesem Code wird auch ein Dummy-Wert für **NotificationIcon** verwendet, der vom Mobile Engagement SDK hinzugefügt wird und den Sie unter Umständen ersetzen möchten. 
   
        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

## <a id="monitor"></a>Aktivieren der Überwachung in Echtzeit
Um mit dem Senden von Daten zu beginnen und sicherzustellen, dass die Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm an das Mobile Engagement-Back-End schicken.

1. Öffnen Sie **ViewController.cs** , und fügen Sie die folgende using-Anweisung hinzu:
   
        using Microsoft.Azure.Engagement.Xamarin;
2. Ändern Sie die Klasse, von der `ViewController` erbt, von `UIViewController` in `EngagementViewController`. 

## <a id="monitor"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging
Mit Mobile Engagement können Sie mit Ihren Benutzern und mit REACH mit Push-Benachrichtigungen und In-App-Nachrichten im Rahmen von Kampagnen interagieren. Dieses Modul nennt sich REACH im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre App für den Empfang ein.

### <a name="modify-your-application-delegate"></a>Ändern des Anwendungsdelegaten
1. Öffnen Sie **AppDelegate.cs** , und fügen Sie die folgende using-Anweisung hinzu:
   
        using System; 
2. Fügen Sie nun in der `FinishedLaunching`-Methode nach `EngagementAgent.init(...)` Folgendes hinzu, um Pushmeldungen zu registrieren.
   
        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }
3. Aktualisieren Sie abschließend folgende Methoden, oder fügen Sie diese hinzu:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }
   
        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }
   
        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }
4. Bestätigen Sie in der Projektmappe in der Datei **Info.plist**, dass die **Paket-ID** mit der **App-ID** aus dem Bereitstellungsprofil in Apple Dev Center übereinstimmt. 
   
    ![][7]
5. Stellen Sie in derselben Datei **Info.plist** sicher, dass die Optionen **Hintergrundmodi aktivieren** und **Remotebenachrichtigungen** aktiviert sind. 
   
     ![][8]
6. Führen Sie die App auf dem Gerät aus, das Sie diesem Veröffentlichungsprofil zugeordnet haben. 

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png

