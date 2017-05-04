### <a name="determine-the-dns-name-of-the-virtual-machine"></a>Bestimmen des DNS-Namens des virtuellen Computers
Um sich von einem anderen Computer aus mit dem SQL Server-Datenbankmodul zu verbinden, müssen Sie den DNS-Namen (Domain Name System) des virtuellen Computers kennen. (Dies ist der Name, den das Internet verwendet, um den virtuellen Computer zu identifizieren. Sie können die IP-Adresse verwenden, aber die IP-Adresse ändert sich möglicherweise, wenn Azure aufgrund von Redundanz oder Wartungsarbeiten Ressourcen verschiebt. Der DNS-Name bleibt bestehen, da er zu einer neuen IP-Adresse weitergeleitet werden kann.)  

1. Wählen Sie im Azure-Portal (oder im vorherigen Schritt) **Virtuelle Computer (klassisch)**aus.
2. Wählen Sie die SQL-VM aus.
3. Kopieren Sie auf dem Blatt **Virtueller Computer** den **DNS-Namen** für den virtuellen Computer.
   
    ![DNS-Name](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)

### <a name="connect-to-the-database-engine-from-another-computer"></a>Verbinden mit dem Datenbankmodul von einem anderen Computer aus
1. Öffnen Sie auf einem mit dem Internet verbundenen Computer SQL Server Management Studio.
2. Geben Sie im Dialogfeld **Verbindung mit Server herstellen** oder **Verbindung mit Datenbankmodul herstellen** im Feld **Servername** den DNS-Namen des virtuellen Computers (in der vorherigen Aufgabe festgelegt) sowie die Portnummer eines öffentlichen Endpunkts im Format *DNSName,Portnummer* (z.B. **mysqlvm.cloudapp.net,57500**) ein.
   
    ![Verbinden mit SSMS](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
   
    Wenn Sie die Portnummer des öffentlichen Endpunkts, den Sie zuvor erstellt haben, vergessen haben, finden Sie sie im Bereich **Endpunkte** auf dem Blatt **Virtueller Computer**.
   
    ![Öffentlicher Port](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. Wählen Sie im Feld **Authentifizierung** den Eintrag **SQL Server-Authentifizierung**.
4. Geben Sie im Feld **Anmeldename** den Anmeldenamen ein, den Sie vorher erstellt haben.
5. Geben Sie im Feld **Kennwort** das Kennwort des Anmeldenamens ein, den Sie vorher erstellt haben.
6. Klicken Sie auf **Verbinden**.

