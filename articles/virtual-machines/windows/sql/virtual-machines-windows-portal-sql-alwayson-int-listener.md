---
title: "Erstellen von SQL Server-Verfügbarkeitsgruppenlistenern – Azure Virtual Machines | Microsoft-Dokumentation"
description: "Eine schrittweise Anleitung zum Erstellen eines Listeners für eine AlwaysOn-Verfügbarkeitsgruppe für SQL Server auf virtuellen Azure-Computern"
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/28/2016
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: 407b189af12116d633ed505facf4bcfde9be5822
ms.openlocfilehash: 6a37e9e786a4e399c49cb77758a23793790888c9


---
# <a name="configure-an-internal-load-balancer-for-an-always-on-availability-group-in-azure"></a>Konfigurieren eines internen Load Balancers für eine AlwaysOn-Verfügbarkeitsgruppe in Azure
In diesem Thema erfahren Sie, wie Sie einen internen Load Balancer für eine SQL Server AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern erstellen, die unter dem Resource Manager-Modell ausgeführt werden. Eine Verfügbarkeitsgruppe benötigt einen Load Balancer, wenn sich die SQL Server-Instanzen auf virtuellen Azure-Computern befinden. Der Load-Balancer speichert die IP-Adresse für den Verfügbarkeitsgruppenlistener. Wenn sich eine Verfügbarkeitsgruppe über mehrere Regionen erstreckt, benötigt jede Region einen Load Balancer.

Für diese Aufgabe benötigen Sie eine SQL Server-Verfügbarkeitsgruppe, die auf virtuellen Azure-Computern unter dem Resource Manager-Modell bereitgestellt wird. Beide virtuellen SQL Server-Computer müssen der gleichen Verfügbarkeitsgruppe angehören. Mithilfe der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie die Verfügbarkeitsgruppe in Azure Resource Manager automatisch erstellen. Diese Vorlage nimmt Ihnen die Erstellung des internen Load Balancers ab. 

Alternativ können Sie aber auch eine [Verfügbarkeitsgruppe manuell konfigurieren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dieses Thema setzt voraus, dass Ihre Verfügbarkeitsgruppen bereits konfiguriert sind.  

Verwandte Themen:

* [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen auf einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Im Rahmen dieses Dokuments erstellen und konfigurieren Sie mithilfe des Azure-Portals Schritt für Schritt einen Load Balancer. Anschließend konfigurieren Sie den Cluster so, dass er die IP-Adresse aus dem Load Balancer für den Verfügbarkeitsgruppenlistener verwendet.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Erstellen und Konfigurieren des Load Balancers im Azure-Portal
Führen Sie in diesem Teil die folgenden Schritte im Azure-Portal aus:

1. Erstellen des Load Balancers und Konfigurieren der IP-Adresse
2. Konfigurieren des Back-End-Pools
3. Erstellen des Tests 
4. Festlegen der Lastenausgleichsregeln

> [!NOTE]
> Falls sich die SQL Server-Instanzen in unterschiedlichen Ressourcengruppen und Regionen befinden, müssen alle diese Schritte zweimal (je einmal pro Ressourcengruppe) ausgeführt werden.
> 
> 

### <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. Erstellen des Load Balancers und Konfigurieren der IP-Adresse
Im ersten Schritt wird zunächst der Load Balancer erstellt. Öffnen Sie im Azure-Portal die Ressourcengruppe mit den virtuellen SQL Server-Computern. Klicken Sie in der Ressourcengruppe auf **Hinzufügen**.

1. Suchen Sie nach **Load Balancer**. Wählen Sie in den Suchergebnissen den (von **Microsoft** veröffentlichten) **Load Balancer** aus.
1. Klicken Sie auf dem Blatt **Load Balancer** auf **Erstellen**.
1. Konfigurieren Sie den Load Balancer unter **Load Balancer erstellen** wie folgt:

   | Einstellung | Wert |
   | --- | --- |
   | **Name** |Namenstext für den Load Balancer. Beispiel: **sqlLB**. |
   | **Typ** |**Intern** |
   | **Virtuelles Netzwerk** |Wählen Sie das virtuelle Netzwerk aus, in dem sich die SQL Server befinden. |
   | **Subnetz** |Wählen Sie das Subnetz aus, in dem sich die SQL Server befinden. |
   | **IP-Adresszuweisung** |**Statisch** |
   | **Private IP-Adresse** |Geben Sie eine verfügbare IP-Adresse aus dem Subnetz an. Verwenden Sie diese IP-Adresse beim Erstellen eines Listeners im Cluster. Die Adresse wird später in einem PowerShell-Skript für die `$ILBIP` -Variable verwendet. |
   | **Abonnement** |Dieses Feld wird unter Umständen angezeigt, wenn Sie über mehrere Abonnements verfügen. Wählen Sie das Abonnement aus, mit dem diese Ressource verknüpft werden soll. Hierbei handelt es sich üblicherweise um das gleiche Abonnement wie bei allen Ressourcen für die Verfügbarkeitsgruppe. |
   | **Ressourcengruppe** |Wählen Sie die Ressourcengruppe aus, in der sich die SQL Server befinden. |
   | **Standort** |Wählen Sie den Azure-Standort aus, an dem sich die SQL Server befinden. |

1. Klicken Sie auf **Erstellen**. 

Azure erstellt den Load Balancer. Der Load Balancer gehört zu einem bestimmten Netzwerk, Subnetz, einer bestimmten Ressourcengruppe und einem bestimmten Standort. Überprüfen Sie nach Abschluss des Vorgangs die Einstellungen für den Load Balancer in Azure. 

### <a name="2-configure-the-backend-pool"></a>2. Konfigurieren des Back-End-Pools
Als Nächstes muss ein Back-End-Adresspool erstellt werden. Im Kontext von Azure wird der Back-End-Adresspool als *Back-End-Pool*bezeichnet. Im vorliegenden Fall enthält der Back-End-Pool die Adressen der beiden SQL Server in Ihrer Verfügbarkeitsgruppe. 

1. Klicken Sie in Ihrer Ressourcengruppe auf den zuvor erstellten Load Balancer. 
1. Klicken Sie unter **Einstellungen** auf **Back-End-Pools**.
1. Klicken Sie im Bereich **Back-End-Pools** auf **Hinzufügen**, um einen Back-End-Adresspool zu erstellen. 
1. Geben Sie im Bereich **Back-End-Pool hinzufügen** unter **Name** einen Namen für den Back-End-Pool ein.
1. Klicken Sie unter **Virtuelle Computer** auf **+ Virtuellen Computer hinzufügen**. 
1. Klicken Sie unter **Virtuelle Computer auswählen** auf **Verfügbarkeitsgruppe auswählen**, und geben Sie die Verfügbarkeitsgruppe an, zu der die virtuellen SQL Server-Computer gehören.
1. Klicken Sie nach dem Auswählen der Verfügbarkeitsgruppe auf **Choose the virtual machines**(Virtuelle Computer auswählen). Klicken Sie auf die beiden virtuellen Computer, die die SQL Server-Instanzen in der Verfügbarkeitsgruppe hosten. Klicken Sie auf **Auswählen**. 
1. Klicken Sie auf **OK**, um die Blätter zum **Auswählen der virtuellen Computer** bzw. zum **Hinzufügen des Back-End-Pools** zu schließen. 

Azure aktualisiert die Einstellungen für den Back-End-Adresspool. Die Verfügbarkeitsgruppe verfügt nun über einen Pool mit zwei SQL Server-Instanzen.

### <a name="3-create-a-probe"></a>3. Erstellen eines Tests
Im nächsten Schritt wird ein Test erstellt. Mit dem Test wird definiert, wie Azure überprüft, welcher der SQL Server gerade für den Verfügbarkeitsgruppenlistener zuständig ist. Azure testet den Dienst auf der Grundlage der IP-Adresse an einem Port, den Sie beim Erstellen des Tests definieren.

1. Klicken Sie auf dem Blatt **Einstellungen** des Load Balancers auf **Integritätstests**. 
1. Klicken Sie auf dem Blatt **Integritätstests** auf **Hinzufügen**.
1. Konfigurieren Sie den Test auf dem Blatt **Test hinzufügen** . Verwenden Sie dabei die folgenden Werte:

   | Einstellung | Wert |
   | --- | --- |
   | **Name** |Namenstext für den Test. Beispiel: **SQLAlwaysOnEndPointProbe**. |
   | **Protokoll** |**TCP** |
   | **Port** |Ein beliebiger verfügbarer Port. Beispiel: *59999*. |
   | **Intervall** |*5* |
   | **Fehlerhafter Schwellenwert** |*2* |

1.  Klicken Sie auf **OK**. 

> [!NOTE]
> Vergewissern Sie sich, dass der angegebene Port in der Firewall beider SQL Server geöffnet ist. Beide Server benötigen eine eingehende Regel für den TCP-Port, den Sie verwenden möchten. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten einer Firewallregel](http://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure erstellt den Test. Mithilfe des Tests ermittelt Azure, welche SQL Server-Instanz über den Listener für die Verfügbarkeitsgruppe verfügt.

### <a name="4-set-the-load-balancing-rules"></a>4. Festlegen der Lastenausgleichsregeln
Legen Sie die Lastenausgleichsregeln fest. Mit den Lastenausgleichsregeln wird konfiguriert, wir der Load Balancer Datenverkehr an die SQL Server weiterleitet. Aktivieren Sie für diesen Load Balancer „Direct Server Return“, da immer nur eine der beiden SQL Server-Instanzen für die Verfügbarkeitsgruppenlistener-Ressource zuständig ist.

1. Klicken Sie auf dem Blatt **Einstellungen** des Load Balancers auf **Load balancing rules** (Lastenausgleichsregeln). 
1. Klicken Sie auf dem Blatt **Load balancing rules** (Lastenausgleichsregeln) auf **Hinzufügen**.
1. Konfigurieren Sie auf dem Blatt **Lastenausgleichsregeln hinzufügen** die Lastenausgleichsregel. Verwenden Sie folgende Einstellungen: 

   | Einstellung | Wert |
   | --- | --- |
   | **Name** |Namenstext für die Lastenausgleichsregeln. Beispiel: **SQLAlwaysOnEndPointListener**. |
   | **Protokoll** |**TCP** |
   | **Port** |*1433* |
   | **Back-End-Port** |*1433*. Dieser Wert wird ignoriert, da diese Regel **Floating IP (Direct Server Return)** verwendet. |
   | **Test** |Verwenden Sie den Namen des Tests, den Sie für diesen Load Balancer erstellt haben. |
   | **Sitzungspersistenz** |**Keine** |
   | **Leerlaufzeitüberschreitung (Minuten)** |*4* |
   | **Floating IP (Direct Server Return)** |**Aktiviert** |

   > [!NOTE]
   > Möglicherweise müssen Sie auf dem Blatt einen Bildlauf nach unten durchführen, um alle Einstellungen anzuzeigen.
   > 

1. Klicken Sie auf **OK**. 
1. Azure konfiguriert die Lastenausgleichsregel. Damit ist die Lastenausgleichsregel konfiguriert, die Datenverkehr an den SQL Server weiterleitet, der den Listener für die Verfügbarkeitsgruppe hostet. 

Die Ressourcengruppe verfügt nun also über einen mit beiden SQL Server-Computern verbundenen Load Balancer. Der Load Balancer verfügt zudem über eine IP-Adresse für den SQL Server AlwaysOn-Verfügbarkeitsgruppenlistener, sodass beide Computer auf Anforderungen für die Verfügbarkeitsgruppen reagieren können.

> [!NOTE]
> Falls sich Ihre SQL Server in unterschiedlichen Regionen befinden, wiederholen Sie die Schritte in der anderen Region. Jede Region benötigt einen Load Balancer. 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurieren des Clusters, von dem die IP-Adresse des Load Balancers verwendet werden soll
Als Nächstes wird der Listener für den Cluster konfiguriert und online geschaltet. Führen Sie folgende Schritte aus: 

1. Erstellen des Verfügbarkeitsgruppenlisteners für den Failovercluster 
2. Onlineschalten des Listeners

### <a name="5-create-the-availability-group-listener-on-the-failover-cluster"></a>5. Erstellen des Verfügbarkeitsgruppenlisteners für den Failovercluster
In diesem Schritt erstellen Sie manuell den Verfügbarkeitsgruppenlistener im Failovercluster-Manager und in SQL Server Management Studio (SSMS).

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

<!---------------------------
* Use RDP to connect to the Azure virtual machine that hosts the primary replica. 
* Open Failover Cluster Manager.
* Select the **Networks** node, and note the cluster network name. This name will be used in the `$ClusterNetworkName` variable in the PowerShell script.
* Expand the cluster name, and then click **Roles**.
* In the **Roles** pane, right-click the availability group name and then select **Add Resource** > **Client Access Point**.
* In the **Name** box, create a name for this new listener, then click **Next** twice, and then click **Finish**. Do not bring the listener or resource online at this point.
  
  > [!NOTE]
  > The name for the new listener is the network name that applications will use to connect to databases in the SQL Server availability group.
  > 
  > 
* Click the **Resources** tab, then expand the Client Access Point you just created. Right-click the IP resource and click properties. Note the name of the IP address. You will use this name in the `$IPResourceName` variable in the PowerShell script.
* Under **IP Address** click **Static IP Address** and set the static IP address to the same address that you used when you set the load balancer IP address on the Azure portal. Enable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
* On the cluster node that currently hosts the primary replica, open an elevated PowerShell ISE and paste the following commands into a new script.
  
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
  
        Import-Module FailoverClusters
  
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
* Update the variables and run the PowerShell script to configure the IP address and port for the new listener.
  
  > [!NOTE]
  > If your SQL Servers are in separate regions, you need to run the PowerShell script twice. The first time use the cluster network name, cluster IP resource name, and load balancer IP address from the first resource group. The second time use the cluster network name, cluster IP resource name, and load balancer IP address from the second resource group.
  > 
  > 

Now the cluster has an availability group listener resource.

### 2. Bring the listener online
With the availability group listener resource configured, you can bring the listener online so that applications can connect to databases in the availability group with the listener.

* Navigate back to Failover Cluster Manager. Expand **Roles** and then highlight your Availability Group. On the **Resources** tab, right-click the listener name and click **Properties**.
* Click the **Dependencies** tab. If there are multiple resources listed, verify that the IP addresses have OR, not AND, dependencies. Click **OK**.
* Right-click the listener name and click **Bring Online**.
* Once the listener is online, from the **Resources** tab, right-click the availability group and click **Properties**.
* Create a dependency on the listener name resource (not the IP address resources name). Click **OK**.
* Launch SQL Server Management Studio and connect to the primary replica.
* Navigate to **AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**. 
* You should now see the listener name that you created in Failover Cluster Manager. Right-click the listener name and click **Properties**.
* In the **Port** box, specify the port number for the availability group listener by using the $EndpointPort you used earlier (1433 was the default), then click **OK**.

------------------------------->

### <a name="verify-the-configuration-of-the-listener"></a>Überprüfen der Konfiguration des Listeners

Wenn die Clusterressourcen und Abhängigkeiten ordnungsgemäß konfiguriert sind, sollten Sie den Listener in SQL Server Management Studio-Umgebungen finden. Führen Sie die folgenden Schritte aus, um den Listenerport festzulegen:

1. Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem primären Replikat her.
2. Navigieren Sie zu **Hohe Verfügbarkeit mit Always On** | **Verfügbarkeitsgruppen** | **Verfügbarkeitsgruppenlistener**. 
1. Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.
1. Geben Sie im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an. Verwenden Sie dabei den zuvor verwendeten Wert für „$EndpointPort“ (Standardwert:&1433;). Klicken Sie anschließend auf **OK**.

Sie verfügen nun über eine SQL Server AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern im Azure Resource Manager-Modus. 

## <a name="test-the-connection-to-the-listener"></a>Testen der Verbindung mit dem Listener
Gehen Sie wie folgt vor, um die Verbindung zu testen:

1. Stellen Sie eine RDP-Verbindung mit einem SQL Server her, der sich im gleichen virtuellen Netzwerk befindet, aber nicht für das Replikat zuständig ist. Hierbei kann es sich um den anderen SQL Server im Cluster handeln.
2. Testen Sie die Verbindung mithilfe des **sqlcmd** -Hilfsprogramms. Das folgende Skript stellt beispielsweise über den Listener eine **sqlcmd** -Verbindung mit Windows-Authentifizierung mit dem primären Replikat her:
   
        sqlcmd -S <listenerName> -E

Die sqlcmd-Verbindung wird automatisch mit der SQL Server-Instanz hergestellt, die das primäre Replikat hostet. 

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen
Für Verfügbarkeitsgruppenlistener in Azure mit internem Load Balancer gelten folgenden Richtlinien:

* Da der Listener für den Load Balancer konfiguriert wird und nur ein interner Load Balancer vorhanden ist, wird pro Clouddienst maximal ein interner Verfügbarkeitsgruppenlistener unterstützt. Es ist jedoch möglich, mehrere externe Listener zu erstellen. 
* Bei Verwendung eines internen Load Balancers erfolgt der Zugriff auf den Listener nur innerhalb des gleichen virtuellen Netzwerks.


<!--HONumber=Jan17_HO2-->


