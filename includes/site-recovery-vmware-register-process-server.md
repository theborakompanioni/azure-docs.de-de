* Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Prozessservercomputer her.
* Sie können „cspsconfigtool.exe“ starten, indem Sie auf dem Desktop auf die Verknüpfung klicken. (Das Tool wird automatisch gestartet, wenn Sie sich zum ersten Mal am Prozessserver anmelden.)
  - Vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) oder IP-Adresse des Konfigurationsservers
  - Port, an dem der Konfigurationsserver lauscht. Der Wert sollte „443“ lauten.
  - Verbindungspassphrase zum Herstellen der Verbindung mit dem Konfigurationsserver
  - Zu konfigurierender Datenübertragungsport für diesen Prozessserver. Behalten Sie den Standardwert bei (es sei denn, Sie haben die Portnummer in Ihrer Umgebung geändert).

    ![Registrieren des Prozessservers](./media/site-recovery-vmware-register-process-server/register-ps.png)
* Klicken Sie auf die Schaltfläche „Speichern“, um die Konfiguration zu speichern und den Prozessserver zu registrieren.
