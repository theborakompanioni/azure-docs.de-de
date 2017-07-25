
1. Besuchen Sie das [Azure-Portal].
2. Klicken Sie auf **App Services** und dann auf das von Ihnen erstellte Back-End.
3. Klicken Sie in den Einstellungen der mobilen App auf **Schnellstart** > **Cordova**.
![Azure-Portal mit hervorgehobenem Mobile Apps-Schnellstart][quickstart]
4. Klicken Sie unter **Clientanwendung konfigurieren**, auf **Neue App erstellen**, und klicken Sie dann auf **Herunterladen**.
2. Entpacken Sie die heruntergeladene ZIP-Datei in ein Verzeichnis auf Ihrer Festplatte, navigieren Sie zur Projektmappendatei (SLN-Datei), und öffnen Sie sie in Visual Studio.
3. Wählen Sie in Visual Studio in der Dropdownliste neben dem Startpfeil die Plattform für die Projektmappe (Android, iOS oder Windows) aus. Wählen Sie ein bestimmtes Bereitstellungsgerät oder einen Emulator aus, indem Sie auf die Dropdownliste mit dem grünen Pfeil klicken. Sie können die standardmäßig ausgewählte Android-Plattform und den Ripple-Emulator verwenden. In komplexeren Tutorials (etwa mit Pushbenachrichtigungen) muss ein unterstütztes Gerät oder ein unterstützter Emulator ausgewählt werden.
4. Drücken Sie F5, oder klicken Sie auf den grünen Pfeil, um Ihre Cordova-App zu erstellen und auszuführen. Sollte im Emulator ein Sicherheitsdialogfeld mit einer Netzwerkzugriffsanforderung angezeigt werden, lassen Sie den Zugriff zu.
5. Nachdem die App auf dem Gerät oder im Emulator gestartet wurde, geben Sie unter **Enter new text** (Neuen Text eingeben) einen aussagekräftigen Text ein (etwa *Tutorial abschließen*), und klicken Sie dann auf die Schaltfläche **Hinzufügen**.

Das Back-End fügt Daten aus der Anforderung in die TodoItem-Tabelle in SQL-Datenbank ein und gibt Informationen zu den neu gespeicherten Elementen an die mobile App zurück. Die mobile App zeigt diese Daten in der Liste an.

Die Schritte 3 bis 5 können für andere Plattformen wiederholt werden.

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png

<!-- URLs -->
[Azure-Portal]: https://portal.azure.com/
