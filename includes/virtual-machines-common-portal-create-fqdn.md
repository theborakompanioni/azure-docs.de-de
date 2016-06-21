## Kurzanleitung 

Im Artikel wird vorausgesetzt, dass Sie sich im Portal bei Ihrem Abonnement angemeldet und mithilfe des Resource Manager-Bereitstellungsmodells einen virtuellen Computer mit den verfügbaren Images erstellt haben. Führen Sie die folgenden Schritte aus, nachdem die Ausführung des virtuellen Computers begonnen hat:

1.  Zeigen Sie die Einstellungen des virtuellen Computers im Portal an, und klicken Sie auf die öffentliche IP-Adresse.

    ![IP-Ressource suchen](./media/virtual-machines-common-portal-create-fqdn/locatePublicIP.PNG)

2.  Beachten Sie, dass der DNS-Name für die öffentliche IP-Adresse leer ist. Klicken Sie für das Blatt mit der öffentlichen IP auf **Alle Einstellungen**.

    ![Einstellungen IP](./media/virtual-machines-common-portal-create-fqdn/settingsIP.PNG)

3.  Öffnen Sie in den Einstellungen für die öffentliche IP die Registerkarte **Konfiguration**. Geben Sie den gewünschten DNS-Namen ein, und **speichern** Sie diese Konfiguration.

    ![DNS-Name eingeben](./media/virtual-machines-common-portal-create-fqdn/dnsNameLabel.PNG)

    Die öffentliche IP-Ressource zeigt nun diesen neuen DNS-Namen in ihrem Blatt an.

4.  Schließen Sie die Blätter der öffentlichen IP und gehen Sie zurück zum Blatt des virtuellen Computers im Portal. Überprüfen Sie, ob der DNS-Name/ der FQDN für die öffentliche IP-Ressource neben der IP-Adresse angezeigt wird.

    ![FQDN wird erstellt](./media/virtual-machines-common-portal-create-fqdn/fqdnCreated.PNG)

<!---HONumber=AcomDC_0608_2016-->