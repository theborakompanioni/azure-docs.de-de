---
title: "Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern | Microsoft Azure"
description: "Es wird beschrieben, wie Sie Verfügbarkeitsgruppenlistener unter dem Azure Resource Manager-Modell konfigurieren, indem Sie ein internes Lastenausgleichsmodul mit einer oder mehreren IP-Adressen verwenden."
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/28/2016
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: 407b189af12116d633ed505facf4bcfde9be5822
ms.openlocfilehash: dd6eb530f715d98c39ab8730ee33922887d5f9cc


---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern – Resource Manager
Dieses Thema beschreibt Folgendes:

* Erstellen eines internen Lastenausgleichsmoduls für SQL Server-Verfügbarkeitsgruppen mit PowerShell-Cmdlets
* Hinzufügen von zusätzlichen IP-Adressen zu einem Load Balancer für die Unterstützung von mehreren Verfügbarkeitsgruppen 

Ein Verfügbarkeitsgruppenlistener ist der Name eines virtuellen Netzwerks, mit dem Clients eine Verbindung herstellen, um Zugriff auf die Datenbank zu erhalten. Auf virtuellen Azure-Computern enthält ein Lastenausgleich die IP-Adresse für den Listener. Mit dem Lastenausgleichsmodul wird Datenverkehr auf die Instanz von SQL Server geleitet, die über den Testport lauscht. Normalerweise wird für eine Verfügbarkeitsgruppe ein interner Load Balancer verwendet. Mit einem internen Azure Load Balancer kann auch eine größere Anzahl von IP-Adressen gehostet werden. Für jede IP-Adresse wird ein bestimmter Testport verwendet. In diesem Dokument wird beschrieben, wie Sie mit PowerShell einen Load Balancer erstellen oder einem vorhandenen Load Balancer für SQL Server-Verfügbarkeitsgruppen IP-Adressen hinzufügen. 

Die Möglichkeit zum Zuweisen von mehreren IP-Adressen zu einem internen Lastenausgleichsmodul ist neu in Azure und nur im Resource Manager-Modell verfügbar. Für diese Aufgabe benötigen Sie eine SQL Server-Verfügbarkeitsgruppe, die auf virtuellen Azure-Computern unter dem Resource Manager-Modell bereitgestellt wird. Beide virtuellen SQL Server-Computer müssen der gleichen Verfügbarkeitsgruppe angehören. Mithilfe der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie die Verfügbarkeitsgruppe in Azure Resource Manager automatisch erstellen. Mit dieser Vorlage wird die Verfügbarkeitsgruppe automatisch erstellt, einschließlich des internen Lastenausgleichsmoduls. Alternativ können Sie aber auch eine [Always On-Verfügbarkeitsgruppe manuell konfigurieren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dieses Thema setzt voraus, dass Ihre Verfügbarkeitsgruppen bereits konfiguriert sind.  

Verwandte Themen:

* [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen in einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Konfigurieren der Windows-Firewall
Konfigurieren Sie die Windows-Firewall so, dass der SQL Server-Zugriff zulässig ist. Die Firewallregeln lassen TCP-Verbindungen mit den Ports für die SQL Server-Instanz und den Listenertest zu. Weitere Informationen finden Sie unter [Konfigurieren einer Windows-Firewall für Datenbankmodulzugriff](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Erstellen Sie für den SQL Server-Port und den Testport eine Regel für eingehenden Datenverkehr.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Beispielskript: Erstellen eines internen Lastenausgleichsmoduls mit PowerShell
> [!NOTE]
> Wenn Sie Ihre Verfügbarkeitsgruppe mit der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) erstellt haben, wurde der interne Load Balancer bereits erstellt. 
> 
> 

Mit dem folgenden PowerShell-Skript wird ein internes Lastenausgleichsmodul erstellt, die Lastenausgleichsregeln werden erstellt und eine IP-Adresse für das Lastenausgleichsmodul wird festgelegt. Öffnen Sie Windows PowerShell ISE, und fügen Sie das Skript im Skriptbereich ein, um es auszuführen. Melden Sie sich mithilfe von `Login-AzureRMAccount` bei PowerShell an. Verwenden Sie bei mehreren Azure-Abonnements `Select-AzureRmSubscription ` , um das Abonnement festzulegen. 

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

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

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

## <a name="a-nameadd-ipa-example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a><a name="Add-IP"></a> Beispielskript: Hinzufügen einer IP-Adresse zu einem vorhandenen Load Balancer mit PowerShell
Wenn Sie mehrere Verfügbarkeitsgruppen verwenden möchten, fügen Sie dem Load Balancer eine zusätzliche IP-Adresse hinzu. Für jede IP-Adresse sind eine eigene Lastenausgleichsregel, ein Testport und ein Front-End-Port erforderlich.

Der Front-End-Port ist der Port, der von Anwendungen zum Herstellen einer Verbindung mit der SQL Server-Instanz genutzt wird. IP-Adressen für unterschiedliche Verfügbarkeitsgruppen können denselben Front-End-Port verwenden.

> [!NOTE]
> Bei SQL Server-Verfügbarkeitsgruppen wird für jede IP-Adresse ein bestimmter Testport benötigt. Wenn für eine IP-Adresse eines Lastenausgleichsmoduls beispielsweise der Testport 59999 verwendet wird, können keine anderen IP-Adressen des Lastenausgleichsmoduls den Testport 59999 nutzen.

* Informationen zu den Grenzwerten für Load Balancer finden Sie im Abschnitt **Private Front-End-IP pro Load Balancer** unter [Netzwerklimits – Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Informationen zu den Grenzwerten von Verfügbarkeitsgruppen finden Sie unter [Einschränkungen (Verfügbarkeitsgruppen)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Mit dem folgenden Skript wird einem vorhandenen Lastenausgleichsmodul eine neue IP-Adresse hinzugefügt. Der interne Load Balancer verwendet den Listenerport für den Front-End-Port des Load Balancers. Dieser Port kann der Port sein, über den SQL Server lauscht. Für Standardinstanzen von SQL Server lautet der Port 1433. Für die Load Balancer-Regel einer Verfügbarkeitsgruppe wird eine Floating IP-Adresse (Direct Server Return) benötigt, sodass der Back-End-Port dem Front-End-Port entspricht. Aktualisieren Sie die Variablen für Ihre Umgebung. 

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

## <a name="configure-the-listener"></a>Konfigurieren des Listeners

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]


<!------------------------------- The content below is duplicated. Pointing to the link. Thinking about an include. 

## Configure the cluster to use the load balancer IP address
The next step is to configure the listener on the cluster, and bring the listener online. To accomplish this, do the following: 

1. Create the availability group listener on the failover cluster  
2. Bring the listener online

## Create the availability group listener on the failover cluster

The availability group listener is an IP address and network name that the SQL Server availability group listens on. To create the availability group listener, do the following steps:

1. [Get the name of the cluster network resource](#getnet).

1. [Add the client access point](#addcap).

1. [Configure the IP resource for the availability group](#congroup).

1. [Make the availability group resource dependent on the listener resource name](#listname).

1. [Set the cluster parameters in PowerShell](#setparam).

The following sections provide detailed instructions for each of these steps. 

### <a name="getnet">Get the name of the cluster network resource</a> 

1. Use RDP to connect to the Azure virtual machine that hosts the primary replica. 

1. Open Failover Cluster Manager.

1. Select the **Networks** node, and note the cluster network name. Use this name in the `$ClusterNetworkName` variable in the PowerShell script.

### <a name="addcap">Add the client access point</a>

1. Expand the cluster name, and then click **Roles**.

1. In the **Roles** pane, right-click the availability group name and then select **Add Resource** > **Client Access Point**.

1. In the **Name** box, create a name for this new listener. 

   The name for the new listener is the network name that applications will use to connect to databases in the SQL Server availability group.
   
   To finish creating the listener, click **Next** twice, and then click **Finish**. Do not bring the listener or resource online at this point.
   
### <a name="congroup">Configure the IP resource for the availability group</a>

1. Click the **Resources** tab, then expand the Client Access Point you just created. Right-click the IP resource and click properties. Note the name of the IP address. You will use this name in the `$IPResourceName` variable in the PowerShell script.

1. Under **IP Address** click **Static IP Address** and set the static IP address to the same address that you used when you set the load balancer IP address on the Azure portal. 

1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 

### <a name="listname">Make the availability group resource dependent on the listener resource</a>

1. In Failover Cluster Manager click **Roles** and click your Availability Group. 

1. On the **Resources** tab, right-click the availability resource group and click **Properties**. 

1. Click the **Dependencies** tab. Set a dependency on the listener resource name. If there are multiple resources listed, verify that the IP addresses have OR, not AND, dependencies. Click **OK**. 

1. Right-click the listener name and click **Bring Online**. 

### <a name="setparam">Set the cluster parameters in PowerShell</a>

Set the cluster parameters. To do this, update the following PowerShell script. Set the variables with the values for your environment. Run the PowerShell script on one of the cluster nodes.  
    
   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<IPResourceName>" # the IP Address resource name
   $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```
> [!NOTE]
> If your SQL Servers are in separate regions, you need to run the PowerShell script twice. The first time use the `$ILBIP` and `$ProbePort` from the first region. The second time, use the `$ILBIP` and `$ProbePort` from the second region. The cluster network name, and the cluster IP resource name are the same. 

## Set the listener port in SQL Server Management Studio

1. Launch SQL Server Management Studio and connect to the primary replica.

1. Navigate to **AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**. 

1. You should now see the listener name that you created in Failover Cluster Manager. Right-click the listener name and click **Properties**.

1. In the **Port** box, specify the port number for the availability group listener by using the $EndpointPort you used earlier (1433 was the default), then click **OK**.

You now have a SQL Server availability group in Azure virtual machines running in Resource Manager mode. 
-------------------------------->

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>Festlegen des Listenerports in SQL Server Management Studio

1. Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem primären Replikat her.

1. Navigieren Sie zu **Hohe Verfügbarkeit mit Always On** | **Verfügbarkeitsgruppen** | **Verfügbarkeitsgruppenlistener**. 

1. Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.

1. Geben Sie im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an. Verwenden Sie dabei den zuvor verwendeten Wert für „$EndpointPort“ (Standardwert:&1433;). Klicken Sie anschließend auf **OK**.

## <a name="test-the-connection-to-the-listener"></a>Testen der Verbindung mit dem Listener

Gehen Sie wie folgt vor, um die Verbindung zu testen:

1. Stellen Sie eine RDP-Verbindung mit einem SQL Server her, der sich im gleichen virtuellen Netzwerk befindet, aber nicht für das Replikat zuständig ist. Hierbei kann es sich um den anderen SQL Server im Cluster handeln.

1. Testen Sie die Verbindung mithilfe des **sqlcmd** -Hilfsprogramms. Das folgende Skript stellt beispielsweise über den Listener eine **sqlcmd** -Verbindung mit Windows-Authentifizierung mit dem primären Replikat her:
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    Geben Sie den Port in der Verbindungszeichenfolge an, wenn der Listener einen anderen Port als den Standardport (1433) verwendet. Mit dem folgenden sqlcmd-Befehl wird beispielsweise eine Verbindung mit einem Listener über Port 1435 hergestellt: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Die sqlcmd-Verbindung wird automatisch mit der SQL Server-Instanz hergestellt, die das primäre Replikat hostet. 

> [!NOTE]
> Vergewissern Sie sich, dass der angegebene Port in der Firewall beider SQL Server geöffnet ist. Beide Server benötigen eine eingehende Regel für den TCP-Port, den Sie verwenden möchten. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten einer Firewallregel](http://technet.microsoft.com/library/cc753558.aspx) . 
> 
> 

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen
Für Verfügbarkeitsgruppenlistener in Azure mit internem Load Balancer gelten folgenden Richtlinien:

* Bei Verwendung eines internen Load Balancers erfolgt der Zugriff auf den Listener nur innerhalb desselben virtuellen Netzwerks.


## <a name="for-more-information"></a>Weitere Informationen
Weitere Informationen finden Sie unter [Manuelles Konfigurieren der Always On-Verfügbarkeitsgruppe auf virtuellen Azure-Computern](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

## <a name="powershell-cmdlets"></a>PowerShell-Cmdlets
Verwenden Sie die folgenden PowerShell-Cmdlets, um ein internes Lastenausgleichsmodul für virtuelle Azure-Computer zu erstellen.

* [New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) erstellt einen Load Balancer. 
* [New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) erstellt eine Front-End-IP-Konfiguration für einen Load Balancer. 
* [New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) erstellt eine Regelkonfiguration für einen Load Balancer. 
* [New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) erstellt eine Back-End-Adresspoolkonfiguration für einen Load Balancer. 
* [New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) erstellt eine Testkonfiguration für einen Load Balancer.
* [Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) entfernt einen Load Balancer aus einer Azure-Ressourcengruppe.



<!--HONumber=Jan17_HO2-->


