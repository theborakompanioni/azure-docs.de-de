
###Erstellen eines Google Cloud Messaging-Projekts mit API-Schlüssel

>[AZURE.NOTE] Sie benötigen ein Google-Konto mit verifizierter E-Mail-Adresse, um diesen Vorgang abzuschließen. Besuchen Sie die Seite <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>, um ein neues Google-Konto zu erstellen.

1. Navigieren Sie zur [Google Cloud Console](https://console.developers.google.com/project), und melden Sie sich mit den Anmeldeinformationen Ihres Google-Kontos an.

2. Klicken Sie auf **Go to project**, und klicken Sie dann auf **Create Project**.
<!--
   	![](./media/mobile-engagement-enable-google-cloud-messaging/new-project.png)

   	![](./media/mobile-engagement-enable-google-cloud-messaging/new-project-2.png)   
-->
3. Geben Sie einen Projektnamen ein.

4. Notieren Sie sich die Projektnummer, die unter dem Textfeld **Project name** angezeigt wird. Sie benötigen sie später in diesem Tutorial, um die Android-Manifestdatei aufzufüllen. ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)
5. Klicken Sie auf **Erstellen**.

6. Vergewissern Sie sich, dass in der linken Spalte **Overview** ausgewählt ist, und klicken Sie unter „Mobile APIs“ auf **Google Cloud Messaging**. Klicken Sie anschließend auf der nächsten Seite auf **Enable**.

	![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)
<!--
	![](./media/mobile-engagement-enable-google-cloud-messaging/enable-gcm-2.png)
-->
7. Klicken Sie auf der nächsten Seite auf **Go to Credentials**, wählen Sie auf der folgenden Seite **Google Cloud Messaging** im ersten Dropdownfeld und **Web server** im zweiten, und klicken Sie dann auf **What credentials do I need?**.

   	![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

8. Klicken Sie in **Add credentials to your project** auf **Create API key**.

   	![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)
<!--
   	![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key6.png)
-->
9. Notieren Sie sich den **API KEY**-Wert. Sie verwenden diesen API-Schlüsselwert später für die Konfiguration im Abschnitt „Systemeigener Push“. Klicken Sie jetzt auf **Done**.

<!---HONumber=AcomDC_0608_2016-->