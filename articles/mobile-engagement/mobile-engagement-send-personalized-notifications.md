---
title: Senden einer personalisierten Benachrichtigung mit Azure Mobile Engagement
description: "Erfahren Sie, wie Sie personalisierte Benachrichtigungen senden, indem Sie Benutzerprofilinformationen in Benachrichtigungen einfügen, z. B. den Namen."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: 
ms.assetid: 51007089-156a-4bac-bb1b-a590633bf2a2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: all
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b6debaa3a27cfc2a24d23a9c586fce9962bd9605


---
# <a name="personalize-notifications-by-including-user-name"></a>Personalisieren von Benachrichtigungen durch das Einbinden des Benutzernamens
Wenn Sie versuchen, Benachrichtigungen für Ihre App-Benutzer ansprechender zu gestalten, können Sie eine Personalisierung erwägen. Ein wirkungsvoller Ansatz besteht darin, die Namen der App-Benutzer selektiv in den Benachrichtigungen einzusetzen, um sie persönlicher zu gestalten. Hierzu ein Wort der Warnung: Sie sollten beim Hinzufügen von Benutzernamen zu Benachrichtigungen vorsichtig vorgehen. Wenn Sie diese Strategie überstrapazieren, kann dies von einigen App-Benutzern als unangebracht empfunden werden. Stellen Sie außerdem sicher, dass Benutzer der Bereitstellung dieser persönlichen Angaben in der mobilen App ausdrücklich zustimmen können, bevor Sie diese Daten verwenden. 

Sie können die Personalisierung der Benachrichtigungen per Azure Mobile Engagement erreichen, indem Sie die unten angegebenen Schritte ausführen. Darin binden wir den Benutzernamen in die Benachrichtigungen ein. Sie nutzen das Konzept der App-Informationen oder Tags, deren Werte entweder mit den SDKs übergeben werden können, die in die mobile App integriert sind, oder über APIs. Diese App-Informationen oder Tags können dann wie folgt verwendet werden:

1. Zum Ausrichten von Benachrichtigungen auf bestimmte Benutzer anhand der Werte von App-Informationen oder 
2. Als Platzhalter in den Benachrichtigungen, die durch geräte- oder benutzerbezogene Werte ersetzt werden, während Benachrichtigungen an das Gerät gesendet werden 

> [!IMPORTANT]
> Beachten Sie, dass die Geschwindigkeit zum Senden von Benachrichtigungen reduziert wird. Dies liegt an dem zusätzlichen Verarbeitungsaufwand, der für das Ersetzen der Werte von App-Informationen für jede Benachrichtigung anfällt. 
> 
> 

## <a name="register-app-info-in-the-mobile-engagement-portal"></a>Registrieren von App-Informationen im Mobile Engagement-Portal
1) Sie beginnen, indem Sie App-Informationen oder Tags im Azure-Portal registrieren. Wechseln Sie dafür zu **Einstellungen** -> **Tag (App-Informationen)**.  

![][1]    

2) Klicken Sie auf **Neues Tag (App-Informationen)**, und geben Sie als Name *user_nam*e und als Typ *string* ein. Klicken Sie anschließend auf **Senden**. 

![][2]

3) Am Ende sehen Sie, dass diese App-Informationen wie folgt registriert werden:

![][3]

## <a name="send-app-info-from-the-client-sdk"></a>Senden von App-Informationen über das Client-SDK
Hier verwenden wir das Beispiel für die universelle Windows-App. Es sind aber auch gleichwertige Methoden für unsere anderen SDKs vorhanden. 

Angenommen, Sie verfügen in der mobilen App über eine Methode, bei der Sie die Profilinformationen des Benutzers abrufen (normalerweise nach der Authentifizierung). Sie rufen die `SendAppInfo`-Methode hier auf und fügen den Wert der App-Informationen für `user_name` ein, die Sie vorher auf dem Back-End des Mobile Engagement-Diensts registriert haben. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

## <a name="send-personalized-notifications"></a>Senden von personalisierten Benachrichtigungen
Sie können jetzt Benachrichtigungen senden, indem Sie diesen **user_name** verwenden. 

1) Greifen Sie im Mobile Engagement-Portal auf die Registerkarte **Reach** zu, um eine Benachrichtigung zu erstellen. Sie können diesen Platzhalter im folgenden Format an einer beliebigen Stelle im Titel oder Text der Benachrichtigung verwenden. 

![][4]    

> [!NOTE]
> Alle Benutzer, für die die App-Information „user_name“ nicht festgelegt ist, erhalten keine Benachrichtigung. Wenn die Benachrichtigungskampagne im Testmodus ausgeführt wird und Sie keine App-Informationen festgelegt haben, senden wir das „?“-Zeichen, um den Platzhalter zu ersetzen. 
> 
> 

2) Wenn von Mobile Engagement ein Gerät zum Senden dieser Benachrichtigung ausgewählt wird, wird diese App-Information geprüft, und der Wert im Platzhalter wird ersetzt.  
Wenn wir für einen Benutzer beispielsweise `str = "Scott"` festgelegt haben, wird die Geräteregistrierung mit der App-Information **user_name = SCOTT** für den Benutzer zugeordnet, und dem Benutzer wird außerhalb der App eine Pushbenachrichtigung im folgenden Format angezeigt. 

![][5]    

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png




<!--HONumber=Nov16_HO3-->


