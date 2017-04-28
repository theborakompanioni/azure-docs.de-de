* Geben Sie unter **Add vCenter** (vCenter hinzufügen) einen Anzeigenamen für den vSphere-Host oder den vCenter-Server an, und geben Sie anschließend die IP-Adresse oder den FQDN des Servers an. Belassen Sie den Port als Port 443, es sei denn Ihre VMware-Server sind so konfiguriert, dass sie auf Anforderungen auf einem anderen Port lauschen können. Wählen Sie das Konto aus, unter dem die Verbindung mit dem VMware vCenter- oder vSphere ESXi-Server hergestellt werden soll. Klicken Sie auf **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Wenn Sie den VMware vCenter-Server oder den VMware vSphere-Host mit einem Konto hinzufügen, das auf dem vCenter- oder Hostserver keine Administratorrechte besitzt, müssen Sie sicherstellen, dass für das Konto folgende Berechtigungen aktiviert sind: Datencenter, Datenspeicher, Ordner, Host, Netzwerk, Ressource, virtueller Computer und vSphere Distributed Switch. Außerdem benötigt der VMware vCenter-Server die Anzeigeberechtigung für den Speicher.
