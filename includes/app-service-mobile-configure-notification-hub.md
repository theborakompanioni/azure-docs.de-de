Mobile App Service-Apps verwenden [Notification Hubs] für das Senden von Pushbenachrichtigungen. Deshalb müssen Sie einen Notification Hub für Ihre mobile App konfigurieren.

1. Navigieren Sie im [Azure-Portal] zu **App Services**, und klicken Sie dann auf Ihr Back-End. Klicken Sie unter **Einstellungen** auf **Push**.
2. Klicken Sie auf **Verbinden**, um der App eine Notification Hub-Ressource hinzuzufügen. Sie können einen Hub erstellen oder einen vorhandenen Hub verwenden.
   
    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Jetzt haben Sie einen Nofitication Hub mit Ihrem Mobile App-Back-End verbunden. Später konfigurieren Sie diesen Notification Hub so, dass eine Verbindung mit einem Plattformbenachrichtigungssystem (Platform Notification System, PNS) hergestellt wird, um Pushbenachrichtigungen an Geräte zu senden.

[Azure-Portal]: https://portal.azure.com/
[Notification Hubs]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/

<!--HONumber=Nov16_HO3-->


