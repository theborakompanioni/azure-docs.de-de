---
title: "Übersicht über das iOS-SDK für Azure Mobile Engagement | Microsoft Docs"
description: "Neueste Updates und Verfahren für das iOS-SDK für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3a03bbd6-bcf8-436c-9775-5a8188629252
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 09/14/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8bfadc110b8b2e0de470185ec9d84343125c960d


---
# <a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK für Azure Mobile Engagement
Beginnen Sie hier, um alle Details zur Integration von Azure Mobile Engagement in eine iOS-App zu erhalten. Wenn Sie es zunächst nur probieren möchten, stellen Sie sicher, dass Sie das [15-Minuten-Lernprogramm](mobile-engagement-ios-get-started.md)durchführen

Klicken Sie, um den [SDK-Inhalt](mobile-engagement-ios-sdk-content.md)

## <a name="integration-procedures"></a>Integrationsverfahren
1. Beginnen Sie hier: [Integration von Mobile Engagement in Ihre iOS-App](mobile-engagement-ios-integrate-engagement.md)
2. Für Benachrichtigungen: [Integration von Reach (Benachrichtigungen) in Ihre iOS-App](mobile-engagement-ios-integrate-engagement-reach.md)
3. Tag-Plan-Implementierung: [Verwenden der erweiterten Mobile Engagement-Tagging-API in Ihrer iOS-App](mobile-engagement-ios-use-engagement-api.md)

## <a name="release-notes"></a>Versionshinweise
### <a name="400-09122016"></a>4.0.0 (12.09.2016)
* Feste Benachrichtigung auf iOS 10-Geräten nicht umgesetzt.
* XCode 7 als veraltet markiert.

Eine frühere Version finden Sie unter [Vollständige Versionshinweise](mobile-engagement-ios-release-notes.md)

## <a name="upgrade-procedures"></a>Upgrade-Verfahren
Wenn Sie bereits eine ältere Version von Engagement in Ihre Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Möglicherweise müssen Sie mehrere Verfahren ausführen, wenn Sie mehrere SDK-Versionen übersprungen haben. Siehe [Upgradeverfahren](mobile-engagement-ios-upgrade-procedure.md) für vollständige Informationen.

Für jede neue SDK-Version müssen Sie zunächst die Ordner "EngagementSDK" und "EngagementReach" ersetzen (entfernen und in Xcode neu importieren).

### <a name="from-300-to-400"></a>Von 3.0.0 zu 4.0.0
### <a name="xcode-8"></a>XCode 8
Ab Version 4.0.0 des SDK ist XCode 8 zwingend erforderlich.

> [!NOTE]
> Wenn Sie auf XCode 7 tatsächlich nicht verzichten können, bietet sich das [iOS Engagement SDK 3.2.4](https://aka.ms/r6oouh)an. Im Reichweitenmodul dieser Vorgängerversion tritt bei Ausführung auf iOS 10-Geräten ein bekannter Fehler auf: Systembenachrichtigungen werden nicht umgesetzt. Zur Behebung dieses Problems müssen Sie die veraltete API `application:didReceiveRemoteNotification:` wie folgt in Ihrem App-Delegaten implementieren:
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Wir empfehlen diese Problemumgebung nicht** , weil diese iOS-API veraltet ist und sich dieses Verhalten in anstehenden (auch kleineren) iOS-Versionsupgrades ändern kann. Sie sollten so bald wie möglich zu XCode 8 wechseln.
> 
> 

#### <a name="usernotifications-framework"></a>„UserNotifications“-Framework
Sie müssen in den Buildphasen das `UserNotifications`-Framework hinzufügen.

Öffnen Sie im Projektexplorer Ihren Projektbereich, und wählen Sie das richtige Ziel aus. Öffnen Sie dann die Registerkarte **Buildphasen**. Fügen Sie anschließend im Menü **Binärdatei mit Bibliotheken verknüpfen** das Framework `UserNotifications.framework` hinzu. Legen Sie den Link auf `Optional` fest.

#### <a name="application-push-capability"></a>Pushfunktion für Anwendungen
XCode 8 setzt u.U. die Pushfunktion Ihrer App zurück. Überprüfen Sie dies auf der Registerkarte `capability` des ausgewählten Zielgeräts.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Hinzufügen des neuen iOS 10-Benachrichtigungsregistrierungscodes
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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Bei bereits vorhandener eigener Implementierung von „UNUserNotificationCenterDelegate“
Das SDK hat eine eigene Implementierung des Protokolls „UNUserNotificationCenterDelegate“. Es wird vom SDK verwendet, um den Lebenszyklus der Engagement-Benachrichtigungen auf Geräten zu überwachen, die unter iOS 10 oder höher ausgeführt werden. Wenn das SDK Ihre Stellvertretung erkennt, verwendet es nicht seine eigene Implementierung, da es pro Anwendung nur eine „UNUserNotificationCenter“-Stellvertretung geben darf. Dies bedeutet, dass Sie Ihrer eigenen Stellvertretung die Engagement-Logik hinzufügen müssen.

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

> [!NOTE]
> Sie können bestimmen, ob eine Benachrichtigung von Engagement stammt oder nicht, indem das zugehörige `userInfo`-Wörterbuch an die `isEngagementPushPayload:`-Klassenmethode des Agents übergeben wird.
> 
> 




<!--HONumber=Nov16_HO3-->


