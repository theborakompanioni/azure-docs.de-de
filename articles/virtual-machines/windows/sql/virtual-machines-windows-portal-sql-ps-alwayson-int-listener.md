---
title: "Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern (Microsoft Azure) | Microsoft-Dokumentation"
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
ms.date: 05/22/2017
ms.author: mikeray
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 800a37dba526a43e490c7bdff55e3b48e5234a20
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Konfigurieren von Always On-Verfügbarkeitsgruppenlistenern – Resource Manager
Dieses Thema beschreibt Folgendes:

* Erstellen eines internen Lastenausgleichsmoduls für SQL Server-Verfügbarkeitsgruppen mit PowerShell-Cmdlets
* Hinzufügen von zusätzlichen IP-Adressen zu einem Load Balancer für die Unterstützung von mehreren Verfügbarkeitsgruppen 

Ein Verfügbarkeitsgruppenlistener ist der Name eines virtuellen Netzwerks, mit dem Clients eine Verbindung herstellen, um Zugriff auf die Datenbank zu erhalten. Auf virtuellen Azure-Computern enthält ein Lastenausgleich die IP-Adresse für den Listener. Mit dem Lastenausgleichsmodul wird Datenverkehr auf die Instanz von SQL Server geleitet, die über den Testport lauscht. Normalerweise wird für eine Verfügbarkeitsgruppe ein interner Load Balancer verwendet. Mit einem internen Azure Load Balancer kann auch eine größere Anzahl von IP-Adressen gehostet werden. Für jede IP-Adresse wird ein bestimmter Testport verwendet. In diesem Dokument wird beschrieben, wie Sie mit PowerShell einen Load Balancer erstellen oder einem vorhandenen Load Balancer für SQL Server-Verfügbarkeitsgruppen IP-Adressen hinzufügen. 

Die Möglichkeit zum Zuweisen von mehreren IP-Adressen zu einem internen Lastenausgleichsmodul ist neu in Azure und nur im Resource Manager-Modell verfügbar. Für diese Aufgabe benötigen Sie eine SQL Server-Verfügbarkeitsgruppe, die auf virtuellen Azure-Computern unter dem Resource Manager-Modell bereitgestellt wird. Beide virtuellen SQL Server-Computer müssen der gleichen Verfügbarkeitsgruppe angehören. Mithilfe der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie die Verfügbarkeitsgruppe in Azure Resource Manager automatisch erstellen. Mit dieser Vorlage wird die Verfügbarkeitsgruppe automatisch erstellt, einschließlich des internen Lastenausgleichsmoduls. Alternativ können Sie auch eine [Always On-Verfügbarkeitsgruppe manuell konfigurieren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

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
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a> Beispielskript: Hinzufügen einer IP-Adresse zu einem vorhandenen Load Balancer mit PowerShell
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

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>Festlegen des Listenerports in SQL Server Management Studio

1. Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem primären Replikat her.

1. Navigieren Sie zu **Hohe Verfügbarkeit mit Always On** | **Verfügbarkeitsgruppen** | **Verfügbarkeitsgruppenlistener**. 

1. Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.

1. Geben Sie im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an. Verwenden Sie dabei den zuvor verwendeten Wert für „$EndpointPort“ (Standardwert: 1433). Klicken Sie anschließend auf **OK**.

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

