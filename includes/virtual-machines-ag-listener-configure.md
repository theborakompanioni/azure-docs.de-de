Der Verfügbarkeitsgruppenlistener umfasst eine IP-Adresse und einen Netzwerknamen, über die die SQL Server-Verfügbarkeitsgruppe lauscht. Gehen Sie wie folgt vor, um den Verfügbarkeitsgruppenlistener zu erstellen:

1. <a name="getnet"></a>Rufen Sie den Namen der Clusternetzwerk-Ressource ab.

    a. Stellen Sie eine RDP-Verbindung mit dem virtuellen Azure-Computer her, der das primäre Replikat hostet. 

    b. Öffnen Sie den Failovercluster-Manager.

    c. Wählen Sie den Knoten **Netzwerke** aus, und notieren Sie den Netzwerknamen des Clusters. Verwenden Sie diesen Namen im PowerShell-Skript in der Variablen `$ClusterNetworkName` . In der folgenden Abbildung lautet der Name des Clusternetzwerks **Cluster Network 1**:

   ![Name des Clusternetzwerks](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <a name="addcap"></a>Fügen Sie den Clientzugriffspunkt hinzu.  
    Der Clientzugriffspunkt ist der Netzwerkname, der von den Anwendungen zum Herstellen der Verbindung mit den Datenbanken einer Verfügbarkeitsgruppe verwendet wird. Erstellen Sie den Clientzugriffspunkt im Failovercluster-Manager.

    a. Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.

    b. Klicken Sie im Bereich **Rollen** mit der rechten Maustaste auf den Verfügbarkeitsgruppennamen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt** aus.

   ![Clientzugriffspunkt](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. Erstellen Sie im Feld **Name** einen Namen für diesen neuen Listener. 
   Der Name für den neuen Listener ist der Netzwerkname, den Anwendungen beim Herstellen einer Verbindung mit Datenbanken in der SQL Server-Verfügbarkeitsgruppe verwenden.
   
    d. Klicken Sie zum Abschließen der Listenererstellung zweimal auf **Weiter** und dann auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource jetzt noch nicht online.

3. <a name="congroup"></a>Konfigurieren Sie die IP-Ressource für die Verfügbarkeitsgruppe.

    a. Klicken Sie auf die Registerkarte **Ressourcen**, und erweitern Sie dann den erstellten Clientzugriffspunkt.  
    Der Clientzugriffspunkt ist offline.

   ![Clientzugriffspunkt](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. Klicken Sie mit der rechten Maustaste auf die IP-Ressource, und klicken Sie dann auf „Eigenschaften“. Notieren Sie den Namen der IP-Adresse, und verwenden Sie ihn in der Variablen `$IPResourceName` im PowerShell-Skript.

    c. Klicken Sie unter **IP-Adresse** auf **Statische IP-Adresse**. Legen Sie die IP-Adresse auf die gleiche Adresse fest, die Sie beim Festlegen der Adresse für den Lastenausgleich im Azure-Portal verwendet haben.

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!------------Diese Option wird auf Server 2016 nicht angezeigt
    1. Deaktivieren Sie NetBIOS für diese Adresse, und klicken Sie auf **OK**. Wiederholen Sie diesen Schritt für jede IP-Ressource, wenn die Lösung mehrere Azure-VNets umfasst. 
    ------------------------->

4. <a name = "dependencyGroup"></a>Richten Sie die Abhängigkeit der SQL Server-Verfügbarkeitsgruppenressource vom Clientzugriffspunkt ein.

    a. Klicken Sie im Failovercluster-Manager auf **Rollen** und dann auf Ihre Verfügbarkeitsgruppe.

    b. Klicken Sie auf der Registerkarte **Ressourcen** unter **Sonstige Ressourcen** mit der rechten Maustaste auf die Verfügbarkeitsressourcengruppe, und klicken Sie dann auf **Eigenschaften**. 

    c. Fügen Sie auf der Registerkarte „Abhängigkeiten“ den Namen der Clientzugriffspunkt-Ressource (Listener) hinzu.

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. Klicken Sie auf **OK**.

5. <a name="listname"></a>Legen Sie fest, dass die Clientzugriffspunkt-Ressource von der IP-Adresse abhängig ist.

    a. Klicken Sie im Failovercluster-Manager auf **Rollen** und dann auf Ihre Verfügbarkeitsgruppe. 

    b. Klicken Sie auf der Registerkarte **Ressourcen** unter **Servername** mit der rechten Maustaste auf den Clientzugriffspunkt, und klicken Sie dann auf **Eigenschaften**. 

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Klicken Sie auf die Registerkarte **Abhängigkeiten** . Stellen Sie sicher, dass die IP-Adresse eine Abhängigkeit ist. Wenn dies nicht der Fall ist, legen Sie eine Abhängigkeit für die IP-Adresse fest. Wenn mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen OR-Abhängigkeiten aufweisen (statt AND-Abhängigkeiten). Klicken Sie auf **OK**. 

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    d. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie dann auf **Online schalten**. 

    >[!TIP]
    >Sie können überprüfen, ob die Abhängigkeiten ordnungsgemäß konfiguriert sind. Wechseln Sie im Failovercluster-Manager zu „Rollen“, klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, klicken Sie auf **Weitere Aktionen**, und klicken Sie dann auf **Abhängigkeitsbericht anzeigen**. Wenn die Abhängigkeiten ordnungsgemäß konfiguriert sind, ist die Verfügbarkeitsgruppe vom Netzwerknamen und der Netzwerkname von der IP-Adresse abhängig. 


6. <a name="setparam"></a>Legen Sie die Clusterparameter in PowerShell fest.
    
    a. Kopieren Sie das folgende PowerShell-Skript in eine Ihrer SQL Server-Instanzen. Aktualisieren Sie die Variablen für Ihre Umgebung.     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    b. Legen Sie die Clusterparameter fest, indem Sie das PowerShell-Skript auf einem der Clusterknoten ausführen.  

    > [!NOTE]
    > Falls sich Ihre SQL Server-Instanzen in unterschiedlichen Regionen befinden, muss das PowerShell-Skript zweimal ausgeführt werden. Verwenden Sie beim ersten Mal die Elemente `$ILBIP` und `$ProbePort` der ersten Region. Verwenden Sie beim zweiten Mal die Elemente `$ILBIP` und `$ProbePort` der zweiten Region. Der Name des Clusternetzwerks und der Cluster-IP-Ressourcenname sind identisch. 
