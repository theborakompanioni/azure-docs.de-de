

1. Starten Sie auf dem Mac **Keychain Access**. Öffnen Sie auf der linken Navigationsleiste unter **Kategorie** den Eintrag **Meine Zertifikate**. Suchen Sie das SSL-Zertifikat, das Sie im vorigen Abschnitt heruntergeladen haben, und legen Sie den Inhalt offen. Wählen Sie nur das Zertifikat (ohne den privaten Schlüssel) aus, und [exportieren Sie es](https://support.apple.com/kb/PH20122?locale=en_US).
2. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Alle durchsuchen** > **App Services** und anschließend auf Ihr Mobile Apps-Back-End. Klicken Sie unter **Einstellungen** auf **App Service Push** (App Service-Push) und dann auf den Namen Ihres Benachrichtigungs-Hubs. Wechseln Sie zu **Apple Push Notification Services** > **Zertifikat hochladen**. Laden Sie die P12-Datei hoch. Wählen Sie dabei den richtigen **Modus** aus (abhängig davon, ob Ihr Client-SSL-Zertifikat für die Produktion oder für den Sandkasten vorgesehen ist). Speichern Sie alle Änderungen.

Ihr Dienst ist jetzt so konfiguriert, dass er mit Pushbenachrichtigungen unter iOS arbeitet.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
