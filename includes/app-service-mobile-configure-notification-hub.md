Das Mobile Apps-Feature von Azure App Service verwendet [Azure Notification Hubs] für den Versand von Pushbenachrichtigungen. Deshalb müssen Sie einen Benachrichtigungs-Hub für Ihre mobile App konfigurieren.

1. Navigieren Sie im [Azure-Portal] zu **App Services**, und klicken Sie dann auf Ihr App-Back-End. Klicken Sie unter **Einstellungen** auf **Push**.
2. Klicken Sie auf **Verbinden**, um der App eine Notification Hub-Ressource hinzuzufügen. Sie können einen Hub erstellen oder einen vorhandenen Hub verwenden.

    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Sie haben nun einen Benachrichtigungs-Hub mit Ihrem Mobile Apps-Back-End-Projekt verbunden. Später konfigurieren Sie diesen Notification Hub so, dass eine Verbindung mit einem Plattformbenachrichtigungssystem (Platform Notification System, PNS) hergestellt wird, um Pushbenachrichtigungen an Geräte zu senden.

[Azure-Portal]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/
