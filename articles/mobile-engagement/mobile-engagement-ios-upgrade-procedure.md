<properties
    pageTitle="Upgradeverfahren für das Azure Mobile Engagement iOS SDK | Microsoft Azure"
    description="Neueste Updates und Verfahren für das iOS-SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />


#<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn Sie bereits eine ältere Version von Engagement in Ihre Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Für jede neue SDK-Version müssen Sie zunächst die Ordner "EngagementSDK" und "EngagementReach" ersetzen (entfernen und in Xcode neu importieren).

##<a name="from-3.0.0-to-4.0.0"></a>Von 3.0.0 zu 4.0.0

### <a name="xcode-8"></a>XCode 8
Ab Version 4.0.0 des SDK ist XCode 8 zwingend erforderlich.

> [AZURE.NOTE] Wenn Sie auf XCode 7 tatsächlich nicht verzichten können, bietet sich das [iOS Engagement SDK 3.2.4](https://aka.ms/r6oouh)an. Im Reichweitenmodul dieser Vorgängerversion tritt bei Ausführung auf iOS 10-Geräten ein bekannter Fehler auf: Systembenachrichtigungen werden nicht umgesetzt. Zur Behebung dieses Problems müssen Sie die veraltete API `application:didReceiveRemoteNotification:` wie folgt in Ihrem App-Delegaten implementieren:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Wir empfehlen diese Problemumgebung nicht** , weil diese iOS-API veraltet ist und sich dieses Verhalten in anstehenden (auch kleineren) iOS-Versionsupgrades ändern kann. Sie sollten so bald wie möglich zu XCode 8 wechseln.

### <a name="usernotifications-framework"></a>„UserNotifications“-Framework
Sie müssen in den Buildphasen das `UserNotifications`-Framework hinzufügen.

Öffnen Sie im Projektexplorer Ihren Projektbereich, und wählen Sie das richtige Ziel aus. Öffnen Sie dann die Registerkarte **Buildphasen**. Fügen Sie anschließend im Menü **Binärdatei mit Bibliotheken verknüpfen** das Framework `UserNotifications.framework` hinzu. Legen Sie den Link auf `Optional` fest.

### <a name="application-push-capability"></a>Pushfunktion für Anwendungen
XCode 8 setzt u.U. die Pushfunktion Ihrer App zurück. Überprüfen Sie dies auf der Registerkarte `capability` des ausgewählten Zielgeräts.

### <a name="add-the-new-ios-10-notification-registration-code"></a>Hinzufügen des neuen iOS 10-Benachrichtigungsregistrierungscodes
Der ältere Codeausschnitt zum Registrieren der App für Benachrichtigungen funktioniert zwar noch, er verwendet unter iOS 10 jedoch veraltete APIs.

Importieren Sie das `User Notification` -Framework:

        #import <UserNotifications/UserNotifications.h> 

Ersetzen Sie in der `application:didFinishLaunchingWithOptions` -Methode des Anwendungsdelegaten:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

durch:

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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Bei bereits vorhandener eigener Implementierung von „UNUserNotificationCenterDelegate“

Das SDK hat auch eine eigene Implementierung des Protokolls „UNUserNotificationCenterDelegate“. Es wird vom SDK verwendet, um den Lebenszyklus der Engagement-Benachrichtigungen auf Geräten zu überwachen, die unter iOS 10 oder höher ausgeführt werden. Wenn das SDK Ihre Stellvertretung erkennt, verwendet es nicht seine eigene Implementierung, da es pro Anwendung nur eine „UNUserNotificationCenter“-Stellvertretung geben darf. Dies bedeutet, dass Sie Ihrer eigenen Stellvertretung die Engagement-Logik hinzufügen müssen.

Hierfür gibt es zwei Möglichkeiten.

Durch einfaches Weiterleiten der Stellvertretungsaufrufe an das SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Oder durch Erben von der `AEUserNotificationHandler`-Klasse

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Sie können bestimmen, ob eine Benachrichtigung von Engagement stammt oder nicht, indem das zugehörige `userInfo`-Wörterbuch an die `isEngagementPushPayload:`-Klassenmethode des Agents übergeben wird.

##<a name="from-2.0.0-to-3.0.0"></a>Von 2.0.0 zu 3.0.0
Unterstützung für iOS 4.X eingestellt. Ab dieser Version muss das Bereitstellungsziel Ihrer Anwendung mindestens über iOS 6 verfügen.

Wenn Sie Reach in Ihrer Anwendung verwenden, müssen Sie den `remote-notification`-Wert zum `UIBackgroundModes`-Array in Ihrer Datei "Info.plist" hinzufügen, um Remotebenachrichtigungen zu erhalten.

Die Methode `application:didReceiveRemoteNotification:` muss in Ihrem Anwendungsdelegaten durch `application:didReceiveRemoteNotification:fetchCompletionHandler:` ersetzt werden.

"AEPushDelegate.h" ist eine veraltete Schnittstelle, und Sie müssen alle Verweise darauf entfernen. Dies umfasst das Entfernen von `[[EngagementAgent shared] setPushDelegate:self]` und der Delegatmethoden aus Ihrem Anwendungsdelegaten:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1.16.0-to-2.0.0"></a>Version 1.16.0 bis 2.0.0
Im Folgenden wird beschrieben, wie Sie die Migration einer SDK-Integration vom Capptain-Dienst, der von Capptain-SAS angeboten wird, in eine durch Azure Mobile Engagement unterstützte App durchführen.
Wenn Sie von einer früheren Version migrieren, sehen Sie auf der Capptain-Website nach, wie eine Migration auf Version 1.16 durchgeführt wird. Führen Sie anschließend das folgende Verfahren aus.

>[AZURE.IMPORTANT] Capptain und Mobile Engagement sind nicht dieselben Dienste, und die unten beschriebene Vorgehensweise hebt nur hervor, wie die Migration der Clientapp durchzuführen ist. Bei der Migration des SDK in die App werden Ihre Daten NICHT von den Capptain-Servern zu den Mobile Engagement-Servern migriert

### <a name="agent"></a>Agent

Die Methode `registerApp:` wurde durch die neue Methode `init:` ersetzt. Ihr Anwendungsdelegat muss entsprechend aktualisiert werden und eine Verbindungszeichenfolge verwenden:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Die SmartAd-Verfolgung wurde aus dem SDK entfernt, und Sie müssen lediglich alle Instanzen der Klasse `AETrackModule` entfernen.

### <a name="class-name-changes"></a>Änderungen von Klassennamen

Im Rahmen der Umfirmierung müssen verschiedene Klassen-/Dateinamen geändert werden.

Alle Klassen mit dem Präfix "CP" erhalten stattdessen das Präfix "AE".

Beispiel:

-   `CPModule.h` wird umbenannt in `AEModule.h`.

Alle Klassen mit dem Präfix "Capptain" erhalten stattdessen das Präfix "Engagement".

Beispiele:

-   Die Klasse `CapptainAgent` wird umbenannt in `EngagementAgent`.
-   Die Klasse `CapptainTableViewController` wird umbenannt in `EngagementTableViewController`.
-   Die Klasse `CapptainUtils` wird umbenannt in `EngagementUtils`.
-   Die Klasse `CapptainViewController` wird umbenannt in `EngagementViewController`.



<!--HONumber=Oct16_HO2-->


