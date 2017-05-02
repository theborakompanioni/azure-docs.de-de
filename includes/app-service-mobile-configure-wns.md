
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Alle durchsuchen** > **App Services** und anschließend auf Ihr Mobile Apps-Back-End. Klicken Sie unter **Einstellungen** auf **App Service Push** (App Service-Push) und dann auf den Namen Ihres Benachrichtigungs-Hubs.
2. Navigieren Sie zu **Windows (WNS)**, geben Sie den **Sicherheitsschlüssel** (Clientgeheimnis) und die **Paket-SID** von der Live Services-Website ein, und klicken Sie dann auf **Speichern**.

    ![Festlegen des WNS-Schlüssels im Portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Das Back-End ist jetzt für die Verwendung von WNS zum Senden von Pushbenachrichtigungen konfiguriert.
