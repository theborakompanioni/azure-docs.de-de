---
title: "Senden plattformübergreifender Benachrichtigungen an Benutzer mit Notification Hubs (ASP.NET)"
description: "Erfahren Sie, wie Sie mithilfe von Notification Hubs-Vorlagen in einer einzigen Anforderung eine plattformunabhängige Benachrichtigung senden können, die auf alle Plattformen abzielt."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0cc620324c75ed6ffee26fe442014d61673ba1e6


---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Senden plattformübergreifender Benachrichtigungen an Benutzer mit Benachrichtigungs-Hubs
Im vorherigen Lernprogramm [Benachrichtigen von Benutzern mit Notification Hubs]haben Sie gelernt, wie Sie Pushbenachrichtigungen an alle Geräte eines bestimmten authentifizierten Benutzers senden können. Dieses Lernprogramm benötigte mehrere Anfragen für den Versand einer Benachrichtigung an jede unterstützte Client-Plattform. Notification Hubs unterstützen Vorlagen, mit denen Sie angeben können, wie ein bestimmtes Gerät Benachrichtigungen empfangen soll. Damit können Sie den Versand plattformübergreifender Benachrichtigungen vereinfachen. Dieser Artikel beschreibt den Einsatz von Vorlagen für den Versand einer plattformunabhängigen Benachrichtigung für alle Plattformen mit nur einer Anfrage. Weitere Informationen zu Vorlagen finden Sie unter [Überblick über Azure Notification Hubs][Vorlagen].

> [!NOTE]
> Mit Notification Hubs kann ein Gerät mehrere Vorlagen mit demselben Tag registrieren. In diesem Fall werden bei einer eingehenden Nachricht für das entsprechende Tag mehrere Benachrichtigungen an das Gerät verschickt (eine pro Vorlage). Auf diese Weise können Sie dieselbe Nachricht in mehreren visuellen Darstellungen anzeigen, z. B. als Signal und als Popupbenachrichtigung in einer Windows Store-App.
> 
> 

Führen Sie die folgenden Schritte aus, um plattformunabhängige Benachrichtigungen mit Vorlagen zu verschicken:

1. Erweitern Sie im Projektmappen-Explorer in Visual Studio den Ordner **Controllers** und öffnen Sie die Datei RegisterController.cs.
2. Suchen Sie in der **Post**-Methode den Codeblock, der eine neue Registrierung erstellt, und ersetzen Sie den Inhalt von `switch` durch den folgenden Code:
   
        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    Dieser Code ruft die plattformspezifische Methode zur Erstellung einer Vorlagenregistrierung anstelle der Systemregistrierung auf. Existierende Registrierungen müssen nicht verändert werden, da Vorlagenregistrierungen von Systemregistrierungen abgeleitet sind.
3. Ersetzen Sie im **Notifications**-Controller die Methode **sendNotification** durch den folgenden Code:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Dieser Code sendet eine Benachrichtigung an alle Plattformen gleichzeitig, ohne dass eine System-Nutzlast angegeben werden muss. Über Notification Hubs wird die richtige Nutzlast an alle Geräte mit dem angegebenen *tag*-Wert erstellt und übermittelt, wie in den registrierten Vorlagen festgelegt.
4. Veröffentlichen Sie Ihr WebAPI-Back-End-Projekt erneut.
5. Führen Sie die Client-App erneut aus und vergewissern Sie sich, dass die Registrierung erfolgreich.
6. (Optional) Stellen Sie die Client-App auf einem zweiten Gerät bereit und führen Sie die App aus.
   
    Beachten Sie, dass auf jedem Gerät eine Benachrichtigung angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte
Nach Abschluss dieses Lernprogramms finden Sie weitere Informationen über Notification Hubs und Vorlagen in den folgenden Themen:

* **[Verwenden von Notification Hubs zum Übermitteln von aktuellen Nachrichten]** <br/>Stellt ein weiteres Szenario für die Verwendung von Vorlagen dar.
* **[Überblick über Azure Notification Hubs][Vorlagen]**<br/>Dieser Übersichtsartikel enthält weitere Detailinformationen zu Vorlagen.

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Pushbenachrichtigungen an Benutzer – ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Pushbenachrichtigungen an Benutzer – Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express für Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Verwenden von Notification Hubs zum Übermitteln von aktuellen Nachrichten]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Benachrichtigen von Benutzern mit Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Vorlagen]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hubs – Anleitungen für Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx



<!--HONumber=Nov16_HO3-->


