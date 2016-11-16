---
title: "Erste Schritte mit Azure Mobile Engagement für iOS in Objective C | Microsoft Docs"
description: "Erfahren Sie mehr über die Verwendung von Azure Mobile Engagement mit Analysefunktionen und Pushbenachrichtigungen für iOS-Apps."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 117b5742-522b-41de-98c5-f432da2d98f8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/05/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1dc9885e4cdbad1153ac476e3f0c0068ec391374


---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Erste Schritte mit Azure Mobile Engagement für iOS-Apps in Objective C
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema erfahren Sie, wie Sie mithilfe von Azure Mobile Engagement die Nutzung Ihrer App verstehen und Pushbenachrichtigungen an segmentierte Benutzer einer iOS-Anwendung senden können.
In diesem Lernprogramm erstellen Sie eine leere iOS-App, die einfache Daten erfasst und Pushbenachrichtigungen mithilfe des Apple-Pushbenachrichtigungsdiensts (APNS) empfängt.

Für dieses Lernprogramm ist Folgendes erforderlich:

* Xcode 8 (Installation über MAC App Store)
* Das [Mobile Engagement iOS SDK]

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Mobile Engagement-Lernprogramme für iOS-Apps.

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).
> 
> 

## <a name="a-idsetupazmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>Einrichten von Mobile Engagement für Ihre iOS-App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="a-idconnectingappaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
In diesem Lernprogramm wird eine "einfache Integration" dargestellt. Dabei handelt es sich um den minimalen erforderlichen Satz zur Sammlung von Daten und zum Senden einer Pushbenachrichtigung. Die vollständige Dokumentation zur Integration finden Sie im Artikel [Integrieren von Mobile Engagement unter iOS](mobile-engagement-ios-sdk-overview.md).

Wir erstellen eine einfache App mit Xcode, um die Integration zu veranschaulichen.

### <a name="create-a-new-ios-project"></a>Erstellen eines neuen iOS-Projekts
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
1. Laden Sie das [Mobile Engagement iOS SDK]herunter.
2. Extrahieren Sie die ".tar.gz"-Datei in einen Ordner auf Ihrem Computer.
3. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Add files to**aus.
   
    ![][1]
4. Navigieren Sie zu dem Ordner, in dem Sie das SDK extrahiert haben, und wählen Sie den Ordner `EngagementSDK` aus. Klicken Sie anschließend auf **OK**.
   
    ![][2]
5. Öffnen Sie die Registerkarte **Build Phases** (Entwicklungsphasen), und fügen Sie im Menü **Link Binary With Libraries** (Binärdatei mit Bibliotheken verknüpfen) wie unten gezeigt die Frameworks hinzu:
   
    ![][3]
6. Wechseln Sie zurück zum Azure-Portal auf der Seite **Verbindungsinformationen** Ihrer App und kopieren Sie die Verbindungszeichenfolge.
   
    ![][4]
7. Fügen Sie in der Datei **AppDelegate.m** die folgende Codezeile hinzu.
   
        #import "EngagementAgent.h"
8. Fügen Sie jetzt die Verbindungszeichenfolge in den `didFinishLaunchingWithOptions` -Delegaten ein.
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. `setTestLogEnabled` ist eine optionale Anweisung, die in SDK-Protokollen das Identifizieren von Problemen ermöglicht. 

## <a name="a-idmonitoraenable-realtime-monitoring"></a><a id="monitor"></a>Aktivieren der Überwachung in Echtzeit
Um mit dem Senden von Daten zu beginnen und sicherzustellen, dass die Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) an das Mobile Engagement-Back-End schicken.

1. Öffnen Sie die Datei **ViewController.h**, und importieren Sie **EngagementViewController.h**:
   
    `# import "EngagementViewController.h"`
2. Ersetzen Sie jetzt die übergeordnete Klasse der **ViewController**-Schnittstelle durch `EngagementViewController`:
   
    `@interface ViewController : EngagementViewController`

## <a name="a-idmonitoraconnect-app-with-realtime-monitoring"></a><a id="monitor"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="a-idintegratepushaenable-push-notifications-and-inapp-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging
Mit Mobile Engagement können Sie mit Ihren Benutzern und mit REACH mit Push-Benachrichtigungen und In-App-Nachrichten im Rahmen von Kampagnen interagieren. Dieses Modul nennt sich REACH im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre App für den Empfang ein.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Aktivieren der App für den Empfang von stillen Pushbenachrichtigungen
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Fügen Sie die Reach-Bibliothek zum Projekt hinzu.
1. Klicken Sie mit der rechten Maustaste auf das Projekt.
2. Wählen Sie **Add file to**aus.
3. Navigieren Sie zu dem Ordner, in dem Sie das SDK extrahiert haben.
4. Wählen Sie den Ordner `EngagementReach` aus.
5. Klicken Sie auf **Hinzufügen**.

### <a name="modify-your-application-delegate"></a>Ändern des Anwendungsdelegaten
1. Importieren Sie in der Datei **AppDeletegate.m** das Engagement Reach-Modul.
   
        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. Erstellen Sie in der `application:didFinishLaunchingWithOptions` -Methode ein Reach-Modul, und übergeben Sie es an die vorhandene Engagement-Initialisierungszeile:
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a>Aktivieren Ihrer App für den Empfang von APNS-Pushbenachrichtigungen.
1. Fügen Sie die folgende Zeile in die `application:didFinishLaunchingWithOptions` Methode ein:
   
        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }
2. Fügen Sie die `application:didRegisterForRemoteNotificationsWithDeviceToken`-Methode wie folgt hinzu:
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. Fügen Sie die `didFailToRegisterForRemoteNotificationsWithError`-Methode wie folgt hinzu:
   
        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
   
           NSLog(@"Failed to get token, error: %@", error);
        }
4. Fügen Sie die `didReceiveRemoteNotification:fetchCompletionHandler` -Methode wie folgt hinzu:
   
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png




<!--HONumber=Nov16_HO2-->


