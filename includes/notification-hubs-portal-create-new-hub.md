

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Wählen Sie **Neu** > **Web und mobil** > **Notification Hub**.
   
      ![Azure-Portal – Erstellen von Notification Hubs](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Geben Sie im Feld **Notification Hub** einen eindeutigen Namen ein. Wählen Sie **Region**, **Abonnement** und **Ressourcengruppe** (sofern bereits vorhanden). 
   
    Gehen Sie wie folgt vor, wenn Sie bereits über einen Service Bus-Namespace verfügen, den Sie im Hub erstellen möchten:

    a. Wählen Sie im Bereich **Namespace** den Link **Vorhandene auswählen**. 
   
    b. Klicken Sie auf **Erstellen**.

    Wenn Sie noch nicht über einen Service Bus-Namespace verfügen, können Sie den Standardnamen verwenden, der basierend auf dem Hub-Namen erstellt wird (sofern der Namespace-Name verfügbar ist).
   
      ![Azure-Portal – Festlegen von Eigenschaften für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

    Nachdem Sie den Namespace und den Notification Hub erstellt haben, wird das Azure-Portal geöffnet. 
   
      ![Azure-Portal – Portalseite für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-page.png)

4. Wählen Sie **Einstellungen** > **Zugriffsrichtlinien**. Notieren Sie sich die beiden Verbindungszeichenfolgen, die für Sie verfügbar sind. Sie benötigen sie später für die Pushbenachrichtigungen.
   
      ![Azure-Portal – Verbindungszeichenfolgen für den Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

