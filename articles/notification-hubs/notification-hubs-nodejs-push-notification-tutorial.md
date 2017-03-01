---
title: Senden von Pushbenachrichtigungen mit Azure Notification Hubs und Node.js
description: Erfahren Sie, wie Sie Notification Hubs verwenden, um Pushbenachrichtigungen aus einer Node.js-Anwendung zu senden.
keywords: Pushbenachrichtigung, Pushbenachrichtigungen, Node.js-Push, iOS-Push
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 88e8ead2b22bf53510c9c6008e34647272326690
ms.openlocfilehash: 32d93f1ff6cf4ae78051b7208e38b6915509210f
ms.lasthandoff: 02/17/2017


---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Senden von Pushbenachrichtigungen mit Azure Notification Hubs und Node.js
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Übersicht
> [!IMPORTANT]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).
> 
> 

In diesem Leitfaden erfahren Sie, wie Sie Pushbenachrichtigungen mithilfe von Azure Notification Hubs direkt aus einer Node.js-Anwendung senden. 

Die behandelten Szenarios umfassen das Senden von Pushbenachrichtigungen an Anwendungen auf den folgenden Plattformen:

* Android
* iOS
* Windows Phone
* Universelle Windows-Plattform 

Weitere Informationen zu Notification Hubs finden Sie im Abschnitt [Nächste Schritte](#next) .

## <a name="what-are-notification-hubs"></a>Was sind Notification Hubs?
Azure Notification Hubs bieten eine einfache, plattformübergreifende und skalierbare Infrastruktur für den Versand von Pushbenachrichtigungen an mobile Geräte. Ausführliche Informationen zur Dienstinfrastruktur finden Sie auf der Seite [Übersicht über Benachrichtigungshubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

## <a name="create-a-nodejs-application"></a>Erstellen einer Node.js-Anwendung
Der erste Schritt in diesem Tutorial besteht im Erstellen einer neuen leeren Node.js-Anwendung. Anweisungen zum Erstellen von Node.js-Anwendungen finden Sie unter [Erstellen und Bereitstellen einer Node.js-Anwendung auf einer Azure-Website][nodejswebsite], [Node.js-Clouddienst][Node.js Cloud Service] mithilfe von Windows PowerShell oder [Website mit WebMatrix].

## <a name="configure-your-application-to-use-notification-hubs"></a>Konfigurieren der Anwendung für Notification Hubs
Um Azure Notification Hubs verwenden zu können, müssen Sie das Node.js-Paket [azure](https://www.npmjs.com/package/azure) herunterladen und verwenden. Dieses Paket enthält eine Reihe von Bibliotheken, die mit den REST-Pushbenachrichtigungsdiensten kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden von Node Package Manager (NPM) zum Beziehen des Pakets
1. Verwenden Sie eine Befehlszeilenschnittstelle, z.B. **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Linux), und navigieren Sie zu dem Ordner, in dem Sie die leere Anwendung erstellt haben.
2. Geben Sie **npm install azure-sb** in das Befehlsfenster ein.
3. Sie können den Befehl **ls** bzw. **dir** manuell ausführen, um zu überprüfen, ob der Ordner **node\_modules** erstellt wurde. In diesem Ordner finden Sie das **azure**-Paket, das die für den Zugriff auf den Notification Hub benötigten Bibliotheken enthält.

> [!NOTE]
> Weitere Informationen zur Installation von NPM finden Sie im offiziellen [NPM-Blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 
> 
> 

### <a name="import-the-module"></a>Importieren des Moduls
Verwenden Sie einen Texteditor, um den folgenden Code am Anfang der Datei **server.js** der Anwendung hinzuzufügen:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Einrichten einer Azure Notification Hub-Verbindung
Das **NotificationHubService** -Objekt ermöglicht Ihnen das Arbeiten mit Notification Hubs. Der folgende Code erstellt ein **NotificationHubService**-Objekt für einen Notification Hub mit dem Namen **hubname**. Fügen Sie ihn am Anfang der Datei **server.js** hinzu, nach der Anweisung zum Importieren des Azure-Moduls:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Führen Sie die folgenden Schritte im **Azure-Portal** durch, um den Wert für [connectionstring] zu erhalten:

1. Klicken Sie im linken Navigationsbereich auf **Durchsuchen**.
2. Wählen Sie **Notification Hubs**, und wählen Sie den gewünschten Hub für das Beispiel aus. Wenn Sie Hilfe beim Erstellen eines neuen Notification Hubs benötigen, lesen Sie das Tutorial [Erste Schritte mit Notification Hubs für Windows Store-Apps](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) .
3. Wählen Sie **Settings**aus.
4. Klicken Sie auf **Zugriffsrichtlinien**. Beide Verbindungszeichenfolgen (Freigabe und Vollzugriff) werden angezeigt.

![Azure-Portal – Notification Hubs](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Sie können die Verbindungszeichenfolge auch mit dem Cmdlet **Get-AzureSbNamespace** in [Azure PowerShell](/powershell/azureps-cmdlets-docs) oder dem Befehl **azure sb namespace show** in der [Azure-Befehlszeilenschnittstelle (Azure CLI)](../xplat-cli-install.md) abrufen.
> 
> 

## <a name="general-architecture"></a>Allgemeine Architektur
Das **NotificationHubService** -Objekt enthält die folgenden Objektinstanzen für den Versand von Pushbenachrichtigungen an bestimmte Geräte und Anwendungen:

* **Android**: Verwenden Sie das **GcmService**-Objekt unter **notificationHubService.gcm**.
* **iOS**: Verwenden Sie das **ApnsService**-Objekt unter **notificationHubService.apns**.
* **Windows Phone**: Verwenden Sie das **MpnsService**-Objekt unter **notificationHubService.mpns**.
* **Universelle Windows-Plattform**: Verwenden Sie das **WnsService**-Objekt unter **notificationHubService.wns**.

### <a name="how-to-send-push-notifications-to-android-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen an Android-Apps
Das **GcmService**-Objekt enthält eine **send**-Methode, mit der Sie Pushbenachrichtigungen an Android-Apps senden können. Die **send** -Methode nimmt die folgenden Parameter entgegen:

* **Tags:** die Tag-ID. Falls kein Tag angegeben ist, wird die Benachrichtigung an alle Clients gesendet.
* **Payload:** die Nutzlast der Nachricht als JSON oder unformatierte Zeichenfolge.
* **Callback:** die Rückruffunktion.

Weitere Informationen zum Nutzlastformat finden Sie im Abschnitt **Payload** (Nutzlast) im Dokument [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload) (Implementieren von GCM-Servern).

Der folgende Code verwendet die **GcmService**-Instanz, die vom **NotificationHubService** verfügbar gemacht wurde, um eine Pushbenachrichtigung an alle registrierten Clients zu senden.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen an iOS-Apps
Wie auch bei den oben beschriebenen Android-Apps enthält das **ApnsService**-Objekt eine **send**-Methode, mit der Pushbenachrichtigungen an iOS-Apps gesendet werden können. Die **send** -Methode nimmt die folgenden Parameter entgegen:

* **Tags:** die Tag-ID. Falls kein Tag angegeben ist, wird die Benachrichtigung an alle Clients gesendet.
* **Payload:** die Nutzlast der Nachricht als JSON oder Zeichenfolge.
* **Callback:** die Rückruffunktion.

Weitere Informationen zum Nutzlastformat finden Sie im Abschnitt **Notification Payload** (Benachrichtigungsnutzlast) im Dokument [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) (Leitfaden zur Programmierung von lokalen und Pushbenachrichtigungen).

Der folgende Code verwendet die **ApnsService**-Instanz, die vom **NotificationHubService** verfügbar gemacht wurde, um eine Warnmeldung an alle Clients zu schicken:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen an Windows Phone-Apps
Das **MpnsService**-Objekt enthält eine **send**-Methode, mit der Pushbenachrichtigungen an Windows Phone-Apps gesendet werden können. Die **send** -Methode nimmt die folgenden Parameter entgegen:

* **Tags:** die Tag-ID. Falls kein Tag angegeben ist, wird die Benachrichtigung an alle Clients gesendet.
* **Payload:** die Nutzlast der Nachricht als XML.
* **TargetName** - `toast` für Popupbenachrichtigungen. `token` für Kachelbenachrichtigungen.
* **NotificationClass:** die Priorität der Benachrichtigung. Eine Liste der gültigen Werte finden Sie im Abschnitt **HTTP Header Elements** (HTTP-Headerelemente) im Dokument [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) (Pushbenachrichtigungen von einem Server).
* **Options:** optionale Anforderungsheader.
* **Callback:** die Rückruffunktion.

Eine Liste der gültigen Optionen für **TargetName**, **NotificationClass** und Header finden Sie auf der Seite [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) (Pushbenachrichtigungen von einem Server).

Der folgende Beispielcode verwendet die **MpnsService**-Instanz, die vom **NotificationHubService** verfügbar gemacht wurde, um eine Popuppushbenachrichtigung zu senden:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen an universelle Windows-Plattform-Anwendungen (UWP)
Das **WnsService**-Objekt enthält eine **send**-Methode, mit der Pushbenachrichtigungen an Apps der universellen Windows-Plattform gesendet werden können.  Die **send** -Methode nimmt die folgenden Parameter entgegen:

* **Tags:** die Tag-ID. Falls kein Tag angegeben ist, wird die Benachrichtigung an alle registrierten Clients gesendet.
* **Payload:** die Nutzlast der Nachricht als XML.
* **Type:** der Benachrichtigungstyp.
* **Options:** optionale Anforderungsheader.
* **Callback:** die Rückruffunktion.

Eine Liste der gültigen Typen und Anforderungsheader finden Sie unter [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts (Windows-Runtime-Apps)](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Der folgende Code verwendet die **WnsService**-Instanz, die vom **NotificationHubService** verfügbar gemacht wurde, um eine Popuppushbenachrichtigung an eine UWP-App zu senden:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a>Nächste Schritte
Mit den obigen Beispielcodeausschnitten können Sie problemlos eine Dienstinfrastruktur zum Übermitteln von Pushbenachrichtigungen an eine Vielzahl von Geräten erstellen. Nachdem Sie mit den Grundlagen der Verwendung von Notification Hubs mit Node.js vertraut sind, erfahren Sie unter den folgenden Links mehr darüber, wie Sie diese Funktionen erweitern können.

* Weitere Informationen finden Sie in der MSDN-Referenz zu [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
* Besuchen Sie das Repository zum [Azure SDK für Node] auf GitHub, das weitere Beispiele und Implementierungsdetails enthält.

[Azure SDK für Node]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[Azure Classic Portal]: http://manage.windowsazure.com
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[Website mit WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[connectionstring]: https://portal.azure.com

