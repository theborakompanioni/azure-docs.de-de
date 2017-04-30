
1. Navigieren Sie zur [Google Developers Console](https://console.developers.google.com/project), und melden Sie sich mit den Anmeldeinformationen Ihres Google-Kontos an. 
2. Klicken Sie auf **Projekt** erstellen, geben Sie einen Projektnamen ein, und klicken Sie auf **Erstellen**. Führen, falls angefordert, die SMS-Verifizierung aus, und klicken Sie erneut auf **Create** .
   
    ![Erstellen eines neuen Projekts](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     Geben Sie Ihren neuen **Projektnamen** ein und klicken Sie auf **Projekt erstellen**.
3. Klicken Sie auf die Schaltfläche **Dienstprogramme und mehr** und anschließend auf **Projektinformationen**. Notieren Sie sich die **Projektnummer**. Sie müssen diesen Wert in der Client-App als `SenderId` -Variable festlegen.
   
    ![Hilfsprogramme und mehr](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. Klicken Sie im Projektdashboard unter **Mobile APIs** auf **Google Cloud Messaging** und dann auf der nächsten Seite auf **API aktivieren**, und akzeptieren Sie die Nutzungsbedingungen. 
   
    ![Aktivieren von GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Aktivieren von GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. Klicken Sie im Projektdashboard auf **Anmeldeinformationen** > **Anmeldeinformationen erstellen** > **API-Schlüssel**. 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. Klicken Sie unter **Neuen Schlüssel erstellen** auf **Serverschlüssel**, geben Sie einen Namen für den Schlüssel ein, und klicken Sie dann auf **Erstellen**.
7. Notieren Sie sich den **API KEY** -Wert.
   
    Mit diesem API-Schlüsselwert geben Sie Azure die Möglichkeit, sich bei GCM zu authentifizieren und Pushbenachrichtigungen im Auftrag Ihrer App zu verschicken.

