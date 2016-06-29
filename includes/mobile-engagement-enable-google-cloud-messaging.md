
###Erstellen eines Google Cloud Messaging-Projekts mit API-Schlüssel

>[AZURE.NOTE] Sie benötigen ein Google-Konto mit verifizierter E-Mail-Adresse, um diesen Vorgang abzuschließen. Besuchen Sie die Seite <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>, um ein neues Google-Konto zu erstellen.

1. Navigieren Sie zur [Google Cloud Console](https://console.developers.google.com/project), und melden Sie sich mit den Anmeldeinformationen Ihres Google-Kontos an.

2. Wechseln Sie zu **Alle Projekte**, und klicken Sie dann auf **Projekt erstellen**.

3. Geben Sie einen **Projektnamen** ein, und klicken Sie auf **Erstellen**.

4. Notieren Sie sich nach der Erstellung des Projekts die **Projektnummer**. Hierbei handelt es sich um einen langen numerischen Wert. Sie finden diesen Wert, den Sie später noch benötigen, in den **Einstellungen** Ihres Projekts im Abschnitt **IAM & Admin** (IAM und Admin).
 
	![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)

5. Wir erstellen jetzt einen Schlüssel für die Google Cloud Messaging-Plattform, die von unserer Plattform zum Senden von Benachrichtigungen an die Android-Geräte verwendet wird. Wechseln Sie in den Abschnitt **API Manager**, und klicken Sie unter **Mobile APIs** auf **Google Cloud Messaging** .

	![](./media/mobile-engagement-enable-google-cloud-messaging/gcm.png)

6. Klicken Sie auf der nächsten Seite auf **Enable**. Im Dashboard werden Sie zum Erstellen von Anmeldeinformationen aufgefordert. Klicken Sie hierfür auf die Schaltfläche **Go to Credentials**.

	![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)

6. Wählen Sie im ersten Dropdownfeld die Option **Google Cloud Messaging** und im zweiten Feld die Option **Web server**, und klicken Sie dann auf **What credentials do I need?**.

   	![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

7. Klicken Sie unter **Add credentials to your project** auf **Create API key**.

   	![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)

8. Notieren Sie sich den **API KEY**-Wert. Sie verwenden diesen API-Schlüsselwert später für die Konfiguration im Abschnitt „Systemeigener Push“. Klicken Sie jetzt auf **Done**.

<!---HONumber=AcomDC_0622_2016-->