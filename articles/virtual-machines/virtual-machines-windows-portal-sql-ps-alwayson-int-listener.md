---
title: Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern | Microsoft Docs
description: Es wird beschrieben, wie Sie Verfügbarkeitsgruppenlistener unter dem Azure Resource Manager-Modell konfigurieren, indem Sie ein internes Lastenausgleichsmodul mit einer oder mehreren IP-Adressen verwenden.
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar

ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 09/22/2016
ms.author: MikeRayMSFT

---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manger"></a>Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern – Resource Manager
In diesem Thema werden zwei Verfahren veranschaulicht:

* Erstellen eines internen Lastenausgleichsmoduls für SQL Server-Verfügbarkeitsgruppen mit PowerShell-Cmdlets
* Hinzufügen von zusätzlichen IP-Adressen zu einem Lastenausgleichsmodul zum Unterstützen von mehr als einer SQL Server-Verfügbarkeitsgruppe 

> **Wichtig:** Die Unterstützung mehrerer Listener für Always On-Verfügbarkeitsgruppen auf virtuellen Azure-Computern ist ein Vorschaufeature und unterliegt den Vorschaubestimmungen in Ihrer Lizenzvereinbarung (z.B. Enterprise Agreement, Microsoft Azure-Vertrag oder Microsoft Online-Abonnementvertrag) sowie allen anwendbaren Bestimmungen unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](http://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
> 
> 

In SQL Server ist ein Verfügbarkeitsgruppenlistener der Name eines virtuellen Netzwerks, mit dem Clients Verbindungen herstellen, um auf eine Datenbank auf dem primären oder sekundären Replikat zuzugreifen. Auf virtuellen Azure-Computern enthält ein Lastenausgleich die IP-Adresse für den Listener. Mit dem Lastenausgleichsmodul wird Datenverkehr auf die Instanz von SQL Server geleitet, die über den Testport lauscht. In den meisten Fällen wird für eine Verfügbarkeitsgruppe ein internes Lastenausgleichsmodul verwendet. Mit einem internen Azure Load Balancer kann auch eine größere Anzahl von IP-Adressen gehostet werden. Für jede IP-Adresse wird ein bestimmter Testport verwendet. In diesem Dokument wird beschrieben, wie Sie mit PowerShell ein neues Lastenausgleichsmodul erstellen oder IP-Adressen einem vorhandenen Lastenausgleichsmodul für SQL Server-Verfügbarkeitsgruppen hinzufügen. 

Die Möglichkeit zum Zuweisen von mehreren IP-Adressen zu einem internen Lastenausgleichsmodul ist neu in Azure und nur im Resource Manager-Modell verfügbar. Für diese Aufgabe benötigen Sie eine SQL Server-Verfügbarkeitsgruppe, die auf virtuellen Azure-Computern unter dem Resource Manager-Modell bereitgestellt wird. Beide virtuellen SQL Server-Computer müssen der gleichen Verfügbarkeitsgruppe angehören. Mithilfe der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie die Verfügbarkeitsgruppe in Azure Resource Manager automatisch erstellen. Mit dieser Vorlage wird die Verfügbarkeitsgruppe automatisch erstellt, einschließlich des internen Lastenausgleichsmoduls. Alternativ können Sie aber auch eine [Always On-Verfügbarkeitsgruppe manuell konfigurieren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dieses Thema setzt voraus, dass Ihre Verfügbarkeitsgruppen bereits konfiguriert sind.  

Verwandte Themen:

* [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen in einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Konfigurieren der Windows-Firewall
Konfigurieren Sie die Windows-Firewall so, dass der SQL Server-Zugriff zulässig ist. Es ist erforderlich, dass die Firewall TCP-Verbindungen mit den Ports, die von der SQL Server-Instanz verwendet werden, und mit dem vom Listenertest verwendeten Port zulässt. Weitere Informationen finden Sie unter [Konfigurieren einer Windows-Firewall für Datenbankmodulzugriff](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Erstellen Sie für den SQL Server-Port und den Testport eine Regel für eingehenden Datenverkehr.

## <a name="example-script:-create-an-internal-load-balancer-with-powershell"></a>Beispielskript: Erstellen eines internen Lastenausgleichsmoduls mit PowerShell
> [!NOTE]
> Wenn Sie Ihre Verfügbarkeitsgruppe mit der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) erstellt haben, müssen Sie diesen Schritt nicht ausführen. 
> 
> 

Mit dem folgenden PowerShell-Skript wird ein internes Lastenausgleichsmodul erstellt, die Lastenausgleichsregeln werden erstellt und eine IP-Adresse für das Lastenausgleichsmodul wird festgelegt. Öffnen Sie Windows PowerShell ISE, und fügen Sie das Skript im Skriptbereich ein, um es auszuführen. Verwenden Sie `Login-AzureRMAccount` , um sich bei PowerShell anzumelden. Verwenden Sie bei mehreren Azure-Abonnements `Select-AzureRmSubscription ` , um das Abonnement festzulegen. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script:-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Beispielskript: Hinzufügen einer IP-Adresse zu einem vorhandenen Lastenausgleichsmodul mit PowerShell
Wenn Sie mehr als eine Verfügbarkeitsgruppe verwenden möchten, fügen Sie einem vorhandenen Lastenausgleichsmodul mit PowerShell eine zusätzliche IP-Adresse hinzu. Für jede IP-Adresse sind eine eigene Lastenausgleichsregel, ein Testport und ein Front-End-Port erforderlich.

Der Front-End-Port ist der Port, der von Anwendungen zum Herstellen einer Verbindung mit der SQL Server-Instanz genutzt wird. IP-Adressen für unterschiedliche Verfügbarkeitsgruppen können den gleichen Front-End-Port verwenden.

> [!NOTE]
> Bei SQL Server-Verfügbarkeitsgruppen wird für jede IP-Adresse ein bestimmter Testport benötigt. Wenn für eine IP-Adresse eines Lastenausgleichsmoduls beispielsweise der Testport 59999 verwendet wird, können keine anderen IP-Adressen des Lastenausgleichsmoduls den Testport 59999 nutzen. 
> 
> 

* Informationen zu den Grenzwerten für Lastenausgleichsmodule finden Sie im Abschnitt **Private Front-End-IP pro Lastenausgleichsmodul** unter [Netzwerklimits – Azure-Ressourcen-Manager](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Informationen zu den Grenzwerten von Verfügbarkeitsgruppen finden Sie unter [Einschränkungen (Verfügbarkeitsgruppen)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Mit dem folgenden Skript wird einem vorhandenen Lastenausgleichsmodul eine neue IP-Adresse hinzugefügt. Aktualisieren Sie die Variablen für Ihre Umgebung. Das interne Lastenausgleichsmodul verwendet den Listenerport für den Front-End-Port des Lastenausgleichsmoduls. Dieser Port kann der Port sein, über den SQL Server lauscht. Für Standardinstanzen von SQL Server lautet dieser Port 1433. Für die Lastenausgleichsregel einer Verfügbarkeitsgruppe wird eine Floating IP-Adresse (Direct Server Return) benötigt, sodass der Back-End-Port dem Front-End-Port entspricht.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
``` 



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurieren des Clusters, von dem die IP-Adresse des Load Balancers verwendet werden soll
Als Nächstes wird der Listener für den Cluster konfiguriert und online geschaltet. Führen Sie hierzu die folgenden Schritte aus: 

1. Erstellen des Verfügbarkeitsgruppenlisteners für den Failovercluster 
2. Onlineschalten des Listeners

## <a name="1.-create-the-availability-group-listener-on-the-failover-cluster"></a>1. Erstellen des Verfügbarkeitsgruppenlisteners für den Failovercluster
In diesem Schritt fügen Sie dem Failovercluster mit dem Failovercluster-Manager einen Clientzugriffspunkt hinzu und verwenden dann PowerShell, um die Clusterressource für das Lauschen über den Testport zu konfigurieren. 

* Stellen Sie eine RDP-Verbindung mit dem virtuellen Azure-Computer her, der das primäre Replikat hostet. 
* Öffnen Sie den Failovercluster-Manager.
* Wählen Sie den Knoten **Netzwerke** aus, und notieren Sie den Netzwerknamen des Clusters. Verwenden Sie diesen Namen im PowerShell-Skript in der Variablen `$ClusterNetworkName` .
* Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.
* Klicken Sie im Bereich **Rollen** mit der rechten Maustaste auf den Verfügbarkeitsgruppennamen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt**.
* Erstellen Sie im Feld **Name** einen Namen für diesen neuen Listener, klicken Sie dann zweimal auf **Weiter**, und klicken Sie anschließend auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource jetzt noch nicht online.
  
  > [!NOTE]
  > Der Name für den neuen Listener ist der Netzwerkname, den Anwendungen beim Herstellen einer Verbindung mit Datenbanken in der SQL Server-Verfügbarkeitsgruppe verwenden.
  > 
  > 
* Klicken Sie auf die Registerkarte **Ressourcen** , und erweitern Sie dann den Clientzugriffspunkt, den Sie gerade erstellt haben. Klicken Sie mit der rechten Maustaste auf die IP-Ressource, und klicken Sie auf „Eigenschaften“. Notieren Sie sich den Namen der IP-Adresse. Dieser Name wird im PowerShell-Skript in der `$IPResourceName` -Variablen verwendet.
* Klicken Sie unter **IP Address** click **Statische IP-Adresse** and set the static IP-Adresse to the same address that you used when you set the load balancer IP-Adresse on the Azure portal. 
* Deaktivieren Sie NetBIOS für diese Adresse, und klicken Sie auf **OK**. Wiederholen Sie diesen Schritt für jede IP-Ressource, wenn die Lösung mehrere Azure-VNets umfasst. 
* Legen Sie fest, dass die SQL Server-Verfügbarkeitsgruppenressource von der IP-Adresse abhängig ist. Klicken Sie im Cluster-Manager mit der rechten Maustaste auf die Ressource (Registerkarte **Ressourcen** unter **Other Resources** (Weitere Ressourcen)). 
* Öffnen Sie auf dem Clusterknoten, der gerade das primäre Replikat hostet, eine PowerShell-ISE, und fügen Sie die folgenden Befehle in ein neues Skript ein: Klicken Sie auf der Registerkarte **Abhängigkeiten** auf den Namen des Listeners. 
  
    ```powershell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>
  
    Import-Module FailoverClusters
  
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
* Aktualisieren Sie die Variablen, und führen Sie das PowerShell-Skript aus, um die IP-Adresse und den Port für den neuen Listener zu konfigurieren.
  
  > [!NOTE]
  > Falls sich Ihre SQL Server in unterschiedlichen Regionen befinden, muss das PowerShell-Skript zweimal ausgeführt werden. Verwenden Sie beim ersten Mal den Clusternetzwerknamen, den Cluster-IP-Ressourcennamen und die Load Balancer-IP-Adresse aus der ersten Ressourcengruppe. Verwenden Sie beim zweiten Mal den Clusternetzwerknamen, den Cluster-IP-Ressourcennamen und die Load Balancer-IP-Adresse aus der zweiten Ressourcengruppe.
  > 
  > 

Der Cluster verfügt nun über eine Verfügbarkeitsgruppenlistener-Ressource.

## <a name="2.-bring-the-listener-online"></a>2. Onlineschalten des Listeners
Nach dem Konfigurieren der Verfügbarkeitsgruppenlistener-Ressource können Sie den Listener online schalten, damit Clientanwendungen eine Verbindung mit Datenbanken in der Verfügbarkeitsgruppe mit dem Listener herstellen können.

* Wechseln Sie wieder zum Failovercluster-Manager. Erweitern Sie **Rollen** , und markieren Sie dann die Verfügbarkeitsgruppe. Klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.
* Klicken Sie auf die Registerkarte **Abhängigkeiten** . Wenn mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen OR-Abhängigkeiten aufweisen (statt AND-Abhängigkeiten). Klicken Sie auf **OK**.
* Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Online schalten**.
* Sobald der Listener online ist, klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste auf die Verfügbarkeitsgruppe, und klicken Sie auf **Eigenschaften**.
* Erstellen Sie eine Abhängigkeit für die Ressource des Listenernamens (nicht den Ressourcennamen der IP-Adresse). Klicken Sie auf **OK**.
* Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem primären Replikat her.
* Navigieren Sie zu **Hohe Verfügbarkeit mit Always On** | **Verfügbarkeitsgruppen** | **Verfügbarkeitsgruppenlistener**. 
* Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.
* Geben Sie im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an. Verwenden Sie dabei den zuvor verwendeten Wert für „$EndpointPort“ (Standardwert: 1433). Klicken Sie anschließend auf **OK**.

Sie verfügen nun über eine SQL Server-Verfügbarkeitsgruppe auf virtuellen Azure-Computern im Azure Resource Manager-Modus. 

## <a name="test-the-connection-to-the-listener"></a>Testen der Verbindung mit dem Listener
Gehen Sie wie folgt vor, um die Verbindung zu testen:

1. Stellen Sie eine RDP-Verbindung mit einem SQL Server her, der sich im gleichen virtuellen Netzwerk befindet, aber nicht für das Replikat zuständig ist. Hierbei kann es sich um den anderen SQL Server im Cluster handeln.
2. Testen Sie die Verbindung mithilfe des **sqlcmd** -Hilfsprogramms. Das folgende Skript stellt beispielsweise über den Listener eine **sqlcmd** -Verbindung mit Windows-Authentifizierung mit dem primären Replikat her:
   
        sqlmd -S <listenerName> -E
   
    Geben Sie den Port in der Verbindungszeichenfolge an, wenn der Listener einen anderen Port als den Standardport (1433) verwendet. Mit dem folgenden sqlcmd-Befehl wird beispielsweise eine Verbindung mit einem Listener über Port 1435 hergestellt: 
   
        sqlcmd -S <listenerName>,1435 -E

Die sqlcmd-Verbindung wird automatisch mit der SQL Server-Instanz hergestellt, die das primäre Replikat hostet. 

> [!NOTE]
> Vergewissern Sie sich, dass der angegebene Port in der Firewall beider SQL Server geöffnet ist. Beide Server benötigen eine eingehende Regel für den TCP-Port, den Sie verwenden möchten. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten einer Firewallregel](http://technet.microsoft.com/library/cc753558.aspx) . 
> 
> 

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen
Für Verfügbarkeitsgruppenlistener in Azure mit internem Load Balancer gelten die folgenden Richtlinien:

* Bei Verwendung eines internen Load Balancers erfolgt der Zugriff auf den Listener nur innerhalb des gleichen virtuellen Netzwerks.

## <a name="for-more-information"></a>Weitere Informationen
Weitere Informationen finden Sie unter [Manuelles Konfigurieren der AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern – Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>PowerShell-Cmdlets
Verwenden Sie die folgenden PowerShell-Cmdlets, um ein internes Lastenausgleichsmodul für virtuelle Azure-Computer zu erstellen.

* `New-AzureRmLoadBalancer` wird ein Lastenausgleichsmodul erstellt. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) . 
* `New-AzureRMLoadBalancerFrontendIpConfig` wird eine Front-End-IP-Konfiguration für ein Lastenausgleichsmodul erstellt. Weitere Informationen finden Sie unter [New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) .
* `New-AzureRmLoadBalancerRuleConfig` wird eine Regelkonfiguration für ein Lastenausgleichsmodul erstellt. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) . 
* `New-AzureRMLoadBalancerBackendAddressPoolConfig` wird eine Back-End-Adresspoolkonfiguration für ein Lastenausgleichsmodul erstellt. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) . 
* `New-AzureRmLoadBalancerProbeConfig` wird eine Testkonfiguration für ein Lastenausgleichsmodul erstellt. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) .

Verwenden Sie `Remove-AzureRmLoadBalancer`, wenn Sie ein Lastenausgleichsmodul aus einer Azure-Ressourcengruppe entfernen möchten. Weitere Informationen finden Sie unter [Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).

<!--HONumber=Oct16_HO2-->


