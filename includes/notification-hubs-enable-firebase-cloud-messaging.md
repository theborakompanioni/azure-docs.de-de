

1. Melden Sie sich bei der [Firebase-Konsole](https://firebase.google.com/console/) an. Erstellen Sie ein neues Firebase-Projekt, falls Sie noch keins besitzen.
2. Klicken Sie nach der Erstellung des Projekts auf **Add Firebase to your Android app** (Firebase der Android-App hinzufügen), und führen Sie die angegebenen Schritte aus.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Klicken Sie in der Firebase-Konsole auf das Zahnrad für Ihr Projekt und dann auf **Projekteinstellungen**.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Klicken Sie in den Projekteinstellungen auf die Registerkarte **Cloud Messaging**, und kopieren Sie den Wert unter **Serverschlüssel** und **Absender-ID**. Sie verwenden diese Werte später, um die Zugriffsrichtlinie für den Benachrichtigungs-Hub und Ihren Benachrichtigungshandler in der App zu konfigurieren.
