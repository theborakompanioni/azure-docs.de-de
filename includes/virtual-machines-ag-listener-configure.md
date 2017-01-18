Der Verfügbarkeitsgruppenlistener umfasst eine IP-Adresse und einen Netzwerknamen, über die die SQL Server-Verfügbarkeitsgruppe lauscht. Führen Sie die folgenden Schritte aus, um die Verfügbarkeitsgruppenlistener zu erstellen:

1. [Rufen Sie den Namen der Clusternetzwerk-Ressource ab](#getnet).

1. [Fügen Sie den Clientzugriffspunkt hinzu](#addcap).

1. [Konfigurieren Sie die IP-Ressource für die Verfügbarkeitsgruppe](#congroup).

1. [Richten Sie die Abhängigkeit der SQL Server-Verfügbarkeitsgruppen-Ressource vom Clientzugriffspunkt ein](#dependencyGroup).

1. [Legen Sie fest, dass die Clientzugriffspunkt-Ressource von der IP-Adresse abhängig ist](#listname).

1. [Legen Sie die Clusterparameter in PowerShell fest](#setparam).

Die folgenden Abschnitte enthalten ausführliche Anleitungen zu diesen Schritten. 

#### <a name="a-namegetnetaget-the-name-of-the-cluster-network-resource"></a><a name="getnet"></a>Abrufen des Namens der Clusternetzwerk-Ressource

1. Stellen Sie eine RDP-Verbindung mit dem virtuellen Azure-Computer her, der das primäre Replikat hostet. 

1. Öffnen Sie den Failovercluster-Manager.

1. Wählen Sie den Knoten **Netzwerke** aus, und notieren Sie den Netzwerknamen des Clusters. Verwenden Sie diesen Namen im PowerShell-Skript in der Variablen `$ClusterNetworkName` .

   In der folgenden Abbildung lautet der Name des Clusternetzwerks **Cluster Network 1**:

   ![Name des Clusternetzwerks](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

#### <a name="a-nameaddcapaadd-the-client-access-point"></a><a name="addcap"></a>Hinzufügen des Clientzugriffspunkts

Der Clientzugriffspunkt ist der Netzwerkname, der von den Anwendungen zum Herstellen der Verbindung mit den Datenbanken einer Verfügbarkeitsgruppe verwendet wird. Erstellen Sie den Clientzugriffspunkt im Failovercluster-Manager. 

1. Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.

1. Klicken Sie im Bereich **Rollen** mit der rechten Maustaste auf den Verfügbarkeitsgruppennamen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt**.

   ![Clientzugriffspunkt](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

1. Erstellen Sie im Feld **Name** einen Namen für diesen neuen Listener. 

   Der Name für den neuen Listener ist der Netzwerkname, den Anwendungen beim Herstellen einer Verbindung mit Datenbanken in der SQL Server-Verfügbarkeitsgruppe verwenden.
   
   Klicken Sie zum Abschließen der Listenererstellung zweimal auf **Weiter** und dann auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource jetzt noch nicht online.
   
#### <a name="a-namecongroupaconfigure-the-ip-resource-for-the-availability-group"></a><a name="congroup"></a>Konfigurieren der IP-Ressource für die Verfügbarkeitsgruppe

1. Klicken Sie auf die Registerkarte **Ressourcen**, und erweitern Sie dann den erstellten Clientzugriffspunkt. Der Clientzugriffspunkt ist offline.

   ![Clientzugriffspunkt](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

1. Klicken Sie mit der rechten Maustaste auf die IP-Ressource, und klicken Sie auf „Eigenschaften“. Notieren Sie sich den Namen der IP-Adresse. Verwenden Sie diesen Namen im PowerShell-Skript in der Variablen `$IPResourceName` .

1. Klicken Sie unter **IP-Adresse** auf **Statische IP-Adresse**. Legen Sie die IP-Adresse auf die gleiche Adresse fest, die Sie beim Festlegen der Adresse für den Lastenausgleich im Azure-Portal verwendet haben.

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

<!-----------------------I don't see this option on server 2016
1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
------------------------->

#### <a name="a-name--dependencygroupamake-the-sql-server-availability-group-resource-dependent-on-the-client-access-point"></a><a name = "dependencyGroup"></a>Einrichten der Abhängigkeit der SQL Server-Verfügbarkeitsgruppen-Ressource vom Clientzugriffspunkt

1. Klicken Sie im Failovercluster-Manager auf **Rollen** und dann auf Ihre Verfügbarkeitsgruppe.

1. Klicken Sie auf der Registerkarte **Ressourcen** unter **Servername** mit der rechten Maustaste auf die Verfügbarkeitsgruppen-Ressource, und klicken Sie dann auf **Eigenschaften**. 

1. Fügen Sie auf der Registerkarte „Abhängigkeiten“ die Namensressource hinzu. Diese Ressource ist der Clientzugriffspunkt. 

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

1. Klicken Sie auf **OK**.

#### <a name="a-namelistnameamake-the-client-access-point-resource-dependent-on-the-ip-address"></a><a name="listname"></a>Festlegen, dass die Clientzugriffspunkt-Ressource von der IP-Adresse abhängig ist

1. Klicken Sie im Failovercluster-Manager auf **Rollen** und dann auf Ihre Verfügbarkeitsgruppe. 

1. Klicken Sie auf der Registerkarte **Ressourcen** unter **Servername** mit der rechten Maustaste auf den Clientzugriffspunkt, und klicken Sie dann auf **Eigenschaften**. 

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

1. Klicken Sie auf die Registerkarte **Abhängigkeiten** . Legen Sie eine Abhängigkeit für den Listenerressourcennamen fest. Wenn mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen OR-Abhängigkeiten aufweisen (statt AND-Abhängigkeiten). Klicken Sie auf **OK**. 

   ![IP-Ressource](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

1. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Online schalten**. 

#### <a name="a-namesetparamaset-the-cluster-parameters-in-powershell"></a><a name="setparam"></a>Festlegen der Clusterparameter in PowerShell

1. Kopieren Sie das folgende PowerShell-Skript auf einen Ihrer Computer mit SQL Server. Aktualisieren Sie die Variablen für Ihre Umgebung.     
   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<IPResourceName>" # the IP Address resource name
   $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

2. Legen Sie die Clusterparameter fest, indem Sie das PowerShell-Skript auf einem der Clusterknoten ausführen.  

> [!NOTE]
> Falls sich Ihre SQL Server in unterschiedlichen Regionen befinden, muss das PowerShell-Skript zweimal ausgeführt werden. Verwenden Sie beim ersten Mal die Elemente `$ILBIP` und `$ProbePort` der ersten Region. Verwenden Sie beim zweiten Mal die Elemente `$ILBIP` und `$ProbePort` der zweiten Region. Der Clusternetzwerkname und der Cluster-IP-Ressourcenname sind identisch. 




<!--HONumber=Jan17_HO2-->


