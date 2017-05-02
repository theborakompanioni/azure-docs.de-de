
1. Klicken Sie im Projektmappen-Explorer von Visual Studio mit der rechten Maustaste auf das Windows Store-App-Projekt, und klicken Sie dann auf **Store** > **App mit Store verknüpfen**.

    ![Zuordnen der App zu Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Klicken Sie im Assistenten auf **Weiter**, und melden Sie sich mit Ihrem Microsoft-Konto an. Geben Sie unter **Neuen App-Namen reservieren** einen Namen für Ihre App ein, und klicken Sie anschließend auf **Reservieren**.
3. Nachdem die App-Registrierung erfolgreich erstellt wurde, wählen Sie den Namen der neuen App aus, klicken Sie auf **Weiter** und dann auf **Zuordnen**. Auf diese Weise werden die erforderlichen Windows Store-Registrierungsinformationen zum Anwendungsmanifest hinzugefügt.
4. Wiederholen Sie die Schritte 1 und 3 für das Windows Phone Store-App-Projekt, und verwenden Sie dabei die zuvor erstellte Registrierung für die Windows Store-App.  
5. Navigieren Sie zum [Windows Dev Center](https://dev.windows.com/en-us/overview), und melden Sie sich mit Ihrem Microsoft-Konto an. Klicken Sie unter **Meine Apps** auf die neue App-Registrierung, und erweitern Sie **Dienste** > **Pushbenachrichtigungen**.
6. Klicken Sie auf der Seite **Pushbenachrichtigungen** unter **Windows-Pushbenachrichtigungsdienste (WNS) und Microsoft Azure Mobile Services** auf **Live Services-Website**. Notieren Sie sich die Werte für **Paket-SID** und den *aktuellen* Wert für **Geheimer Anwendungsschlüssel**. 

    ![App-Einstellung im Developer Center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Der geheime Schlüssel der Anwendung und die Paket-SID sind wichtige Sicherheitsanmeldeinformationen. Geben Sie diese Werte nicht weiter, und verteilen Sie sie nicht mit Ihrer Anwendung.
   >
   >
