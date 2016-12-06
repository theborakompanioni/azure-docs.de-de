## <a name="quick-steps"></a>Kurzanleitung
Im Artikel wird vorausgesetzt, dass Sie sich im Portal bei Ihrem Abonnement angemeldet und mithilfe des Resource Manager-Bereitstellungsmodells einen virtuellen Computer mit den verfügbaren Images erstellt haben. Führen Sie die folgenden Schritte aus, nachdem die Ausführung des virtuellen Computers begonnen hat:

1. Wählen Sie im Portal Ihren virtuellen Computer aus. Der DNS-Name ist leer. Klicken Sie auf **Öffentliche IP-Adresse**:
   
   ![Klicken Sie im Portal auf die öffentliche IP-Ressource.](./media/virtual-machines-common-portal-create-fqdn/locatePublicIP.PNG)

2. Geben Sie den gewünschten DNS-Namen ein, und klicken Sie anschließend auf **Speichern**.
   
   ![Geben Sie einen DNS-Namen für die öffentliche IP-Ressource ein.](./media/virtual-machines-common-portal-create-fqdn/dnsNameLabel.PNG)
   
   Die öffentliche IP-Ressource zeigt nun diesen neuen DNS-Namen in ihrem Blatt an.

3. Schließen Sie die Blätter für die öffentliche IP-Adresse, und navigieren Sie im Portal wieder zum Blatt mit der VM-Übersicht. Nach einigen Sekunden werden die Einstellungen im Portal aktualisiert. Vergewissern Sie sich, dass neben der IP-Adresse für die Ressource **Öffentliche IP-Adresse** der DNS-Name/FQDN angezeigt wird.
   
   ![Vergewissern Sie sich, dass die neue DNS-Bezeichnung festgelegt ist.](./media/virtual-machines-common-portal-create-fqdn/fqdnCreated.PNG)



<!--HONumber=Nov16_HO3-->


