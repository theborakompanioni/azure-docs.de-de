1. Wechseln Sie wieder zum Failovercluster-Manager. Erweitern Sie **Rollen**, und markieren Sie dann die Verfügbarkeitsgruppe. Klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf "Eigenschaften".
2. Klicken Sie auf die Registerkarte **Abhängigkeiten**. Wenn mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen OR-Abhängigkeiten aufweisen (statt AND-Abhängigkeiten). Klicken Sie auf **OK**.
3. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Online schalten**.
4. Sobald der Listener online ist, klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste auf die Verfügbarkeitsgruppe, und klicken Sie auf **Eigenschaften**.
   
    ![Konfigurieren der Verfügbarkeitsgruppenressource](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)
5. Erstellen Sie eine Abhängigkeit für die Ressource des Listenernamens (nicht den Ressourcennamen der IP-Adresse). Klicken Sie auf **OK**.
   
    ![Hinzufügen von Abhängigkeiten zum Listenernamen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)
6. Starten Sie **SQL Server Management Studio**, und stellen Sie eine Verbindung mit dem primären Replikat her.
7. Navigieren Sie zu **Hohe Verfügbarkeit mit AlwaysOn** | **Verfügbarkeitsgruppen** | **<AvailabilityGroupName>** | **Verfügbarkeitsgruppenlistener**.
8. Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.
9. Geben Sie mit dem zuvor verwendeten "$EndpointPort" im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an (in diesem Lernprogramm war 1433 der Standardwert), und klicken Sie dann auf **OK**.

<!---HONumber=Oct15_HO3-->