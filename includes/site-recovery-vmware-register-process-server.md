1. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Prozessservercomputer her.
2. Nach der Anmeldung wird automatisch das Konfigurationstool für den Prozessserver gestartet. Folgende Angaben sind erforderlich:
  * Vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) oder IP-Adresse des Konfigurationsservers
  * Port, an dem der Konfigurationsserver lauscht. Der Wert sollte „443“ lauten.
  * Verbindungspassphrase zum Herstellen der Verbindung mit dem Konfigurationsserver
  * Zu konfigurierender Datenübertragungsport für diesen Prozessserver. Behalten Sie den Standardwert bei (es sei denn, Sie haben die Portnummer in Ihrer Umgebung geändert).

    ![Registrieren des Prozessservers](./media/site-recovery-vmware-register-process-server/register-ps.png)
3. Klicken Sie auf die Schaltfläche „Speichern“, um die Konfiguration zu speichern.
4. Nach Abschluss der Registrierung wird der Prozessserver unter Ihrem Konfigurationsserver angezeigt und steht für Failbacks zur Verfügung.

> [!TIP]
> Das Hilfsprogramm für die Prozessserverkonfiguration kann mittels Doppelklick auf die Verknüpfung **cspsconfigtool** gestartet werden, die sich auf dem Desktop des virtuellen Computers befindet.


<!--HONumber=Feb17_HO1-->


