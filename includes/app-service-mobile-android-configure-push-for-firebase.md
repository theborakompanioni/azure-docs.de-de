
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Alle durchsuchen** > **App Services** und dann auf Ihr Mobile App-Back-End. Klicken Sie unter **Einstellungen** auf **App Service Push** (App Service-Push) und dann auf den Namen Ihres Notification Hubs.
2. Wechseln Sie zu **Google (GCM)**, geben den **Serverschlüssel** ein, den Sie von Firebase im vorherigen Verfahren erhalten haben, und klicken dann auf **Speichern**.
   
    ![Festlegen des GCM-API-Schlüssels im Portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

Das Mobile App-Back-End ist jetzt so konfiguriert, dass es auf Firebase Cloud Messaging zurückgreift, um Pushbenachrichtigungen von Ihrer auf einem Android-Gerät ausgeführten App über den Notification Hub zu senden.

<!-- URLs. -->


<!-- images -->


<!--HONumber=Nov16_HO3-->


