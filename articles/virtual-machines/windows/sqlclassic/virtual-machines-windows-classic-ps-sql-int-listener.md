---
title: "Konfigurieren eines ILB-Listeners für Always On-Verfügbarkeitsgruppen | Microsoft Docs"
description: "In diesem Tutorial werden Ressourcen verwendet, die mit dem klassischen Bereitstellungsmodell erstellt wurden, und es wird mithilfe eines internen Load Balancers (ILB) eine Always On-Verfügbarkeitsgruppe in Azure erstellt."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/01/2017
ms.author: mikeray
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: 8e59988f24748a82d4e143295bab9bdaa65cf8e4
ms.lasthandoff: 01/11/2017


---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfigurieren eines ILB-Listeners für AlwaysOn-Verfügbarkeitsgruppen in Azure
> [!div class="op_single_selector"]
> * [Interner Listener](virtual-machines-windows-classic-ps-sql-int-listener.md)
> * [Externer Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)
> 
> 

## <a name="overview"></a>Übersicht
In diesem Thema erfahren Sie, wie Sie mit dem **internen Load Balancer (ILB)**einen Listener für eine Always On-Verfügbarkeitsgruppe konfigurieren.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

Informationen zum Konfigurieren eines ILB-Listeners für eine Always On-Verfügbarkeitsgruppe im Resource Manager-Modell finden Sie unter [Konfigurieren eines internen Load Balancers für eine Always On-Verfügbarkeitsgruppe in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

Ihre Verfügbarkeitsgruppe kann Replikate enthalten, die ausschließlich lokal, ausschließlich in Azure oder sowohl lokal als auch in Azure verfügbar sind (Hybridkonfigurationen). Azure-Replikate können sich innerhalb derselben Region oder in mehreren Regionen befinden, wobei mehrere virtuelle Netzwerke (VNets) verwendet werden. Bei den nachfolgenden Schritten wird davon ausgegangen, dass bereits eine [Verfügbarkeitsgruppe konfiguriert wurde](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md), Sie jedoch noch keinen Listener konfiguriert haben.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Richtlinien und Einschränkungen für interne Listener
Beachten Sie die folgenden Richtlinien, die für Verfügbarkeitsgruppenlistener in Azure bei Verwendung des internen Lastenausgleichs gelten:

* Der Verfügbarkeitsgruppenlistener wird unter Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 unterstützt.
* Da der Lister mit dem internen Load Balancer (ILB) konfiguriert wird und nur ein ILB pro Clouddienst vorhanden ist, wird pro Clouddienst maximal ein Verfügbarkeitsgruppenlistener unterstützt. Es ist jedoch möglich, mehrere externe Listener zu erstellen. Weitere Informationen finden Sie unter [Konfigurieren eines externen Listeners für Always On-Verfügbarkeitsgruppen in Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

## <a name="determine-the-accessibility-of-the-listener"></a>Festlegen des Zugriffs auf den Listener
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Der Schwerpunkt dieses Artikels liegt auf dem Erstellen eines Listeners, der den **internen Load Balancer**verwendet. Wenn Sie einen öffentlichen/externen Listener benötigen, finden Sie die entsprechenden Schritte im Artikel zum Einrichten eines [externen Listeners](virtual-machines-windows-classic-ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Erstellen von VM-Endpunkten mit Lastenausgleich und Direct Server Return
Sie müssen für ILB zunächst den internen Load Balancer einrichten. Verwenden Sie zu diesem Zweck das folgende Skript.

Sie müssen einen Endpunkt mit Lastenausgleich für jeden virtuellen Computer mit einem Azure-Replikat erstellen. Wenn Sie Replikate in mehreren Regionen haben, muss sich jedes Replikat für diese Region im selben Clouddienst im selben VNet befinden. Das Erstellen von Verfügbarkeitsgruppenreplikaten, die sich über mehrere Azure-Regionen erstrecken, erfordert die Konfiguration mehrerer VNets. Weitere Informationen zum Konfigurieren von VNet-übergreifenden Verbindungen finden Sie unter [Konfigurieren von Verbindungen zwischen VNets](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Navigieren Sie im Azure-Portal zu jedem virtuellen Computer, der ein Replikat hostet, und zeigen Sie die Details an.
2. Klicken Sie auf die Registerkarte **Endpunkte** für jeden virtuellen Computer.
3. Stellen Sie sicher, dass die Werte für **Name** und **Öffentlicher Port** des gewünschten Listenerendpunkts nicht bereits verwendet werden. Im folgenden Beispiel ist der Name "MyEndpoint" und der Port "1433".
4. Laden Sie auf dem lokalen Client [das aktuelle PowerShell-Modul](https://azure.microsoft.com/downloads/)herunter, und installieren Sie es.
5. Starten Sie **Azure PowerShell**. Eine neue PowerShell-Sitzung wird geöffnet, und die Azure-Verwaltungsmodule werden geladen.
6. Führen Sie **Get-AzurePublishSettingsFile**aus. Dieses Cmdlet leitet Sie zu einem Browser weiter, damit Sie eine Datei mit Veröffentlichungseinstellungen in ein lokales Verzeichnis herunterladen können. Möglicherweise werden Sie aufgefordert, die Anmeldeinformationen für Ihr Azure-Abonnement einzugeben.
7. Führen Sie den Befehl **Import-AzurePublishSettingsFile** mit dem Pfad der Datei mit Veröffentlichungseinstellungen aus, die Sie heruntergeladen haben:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Nachdem die Datei mit Veröffentlichungseinstellungen importiert wurde, können Sie Ihr Azure-Abonnement in der PowerShell-Sitzung verwalten.
    
1. Für **ILB**sollten statische IP-Adressen zugewiesen werden. Untersuchen Sie zunächst die aktuelle VNET-Konfiguration, indem Sie den folgenden Befehl ausführen:
   
        (Get-AzureVNetConfig).XMLConfiguration
2. Notieren Sie sich den unter **Subnetz** angegebenen Namen für das Subnetz, in dem sich die zum Hosten der Replikate verwendeten VMs befinden. Dieser Name wird für den Parameter **$SubnetName** des Skripts verwendet.
3. Notieren Sie sich anschließend den für **VirtualNetworkSite** angegebenen Namen und den Wert des startenden **AddressPrefix** für das Subnetz, in dem sich die zum Hosten der Replikate verwendeten VMs befinden. Suchen Sie nach einer verfügbaren IP-Adresse, indem Sie diese beiden Werte im Befehl **Test-AzureStaticVNetIP** verwenden und **AvailableAddresses** untersuchen. Beispiel: Wenn der Name des VNETs *MyVNet* lautet und der Adressbereich des Subnetzes bei *172.16.0.128* beginnt, werden die verfügbaren Adressen mit folgendem Befehl aufgelistet:
   
        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
4. Wählen Sie eine der verfügbaren Adressen, und verwenden Sie diese im folgenden Skript als Parameter **$ILBStaticIP** .
5. Kopieren Sie das unten stehende PowerShell-Skript in einen Text-Editor, und legen Sie die Variablenwerte gemäß Ihrer Umgebung fest (beachten Sie, dass für einige Parameter die Standardwerte angegeben wurden). Hinweis: Bei vorhandenen Bereitstellungen, die Affinitätsgruppen verwenden, kann kein interner Lastenausgleich hinzugefügt werden. Weitere Informationen zu den Voraussetzungen für den internen Lastenausgleich finden Sie unter [Interner Load Balancer](../../../load-balancer/load-balancer-internal-overview.md). Wenn Ihre Verfügbarkeitsgruppe sich über mehrere Azure-Regionen erstreckt, müssen Sie das Skript zudem in jedem Datencenter für den Clouddienst und die Knoten ausführen, die sich in diesem Datencenter befinden.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value
   
        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP
   
        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }
6. Nachdem Sie die Variablen festgelegt haben, kopieren Sie das Skript zur Ausführung aus dem Text-Editor in Ihre Azure PowerShell-Sitzung. Wenn die Aufforderung weiterhin >> anzeigt, geben Sie erneut ENTER ein, um die Skriptausführung zu starten. Hinweis:

> [!NOTE]
> Das klassische Azure-Portal bietet gegenwärtig keine Unterstützung für den internen Lastenausgleich. Folglich werden im klassischen Azure-Portal weder das interne Lastenausgleichsmodul noch die Endpunkte angezeigt. Wenn der Lastenausgleich auf einem Endpunkt ausgeführt wird, gibt **Get-AzureEndpoint** jedoch eine interne IP-Adresse zurück. Andernfalls wird null zurückgegeben.
> 
> 

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Stellen Sie gegebenenfalls sicher, dass KB2854082 installiert ist.
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Öffnen der Firewallports in Verfügbarkeitsgruppenknoten
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Erstellen des Verfügbarkeitsgruppenlisteners

Erstellen Sie den Verfügbarkeitsgruppenlistener in zwei Schritten. Zunächst erstellen Sie die Clientzugriffspunkt-Clusterressource und konfigurieren Abhängigkeiten. Im zweiten Schritt konfigurieren Sie die Clusterressourcen mit PowerShell.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>Erstellen des Clientzugriffspunkts und Konfigurieren der Clusterabhängigkeiten
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>Konfigurieren der Clusterressourcen mit PowerShell
1. Für ILB muss die IP-Adresse des internen Load Balancer verwendet werden, der zuvor erstellt wurde. Verwenden Sie das folgende Skript, um diese IP-Adresse in PowerShell abzurufen.
   
        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress
2. Kopieren Sie in einer der VMs das unten stehende PowerShell-Skript für Ihr Betriebssystem in einen Text-Editor, und legen Sie die Variablen auf die zuvor notierten Werte fest.
   
    Verwenden Sie für Windows Server 2012 oder höher das folgende Skript:
   
        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)
   
        Import-Module FailoverClusters
   
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   
    Verwenden Sie für Windows Server 2008 R2 oder höher das folgende Skript:
   
        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)
   
        Import-Module FailoverClusters
   
        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
3. Nachdem Sie die Variablen festgelegt haben, öffnen Sie ein Windows PowerShell-Fenster mit erhöhten Rechten, und kopieren Sie das Skript aus dem Text-Editor zur Ausführung in Ihre Azure PowerShell-Sitzung. Wenn die Aufforderung weiterhin >> anzeigt, geben Sie erneut ENTER ein, um die Skriptausführung zu starten.
4. Wiederholen Sie diesen Schritt für jeden virtuellen Computer. Mit diesem Skript wird die IP-Adressressource mit der IP-Adresse des Clouddienst konfiguriert. Außerdem werden weitere Parameter wie z. B. der Testport festgelegt. Wenn die IP-Adressressource online geschaltet wird, kann sie auf Abrufvorgänge am Testport reagieren, die vom Endpunkt mit Lastenausgleich ausgehen, der zuvor in diesem Tutorial erstellt wurde.

## <a name="bring-the-listener-online"></a>Onlineschalten des Listeners
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Aufgaben nach der Erstellung
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testen des Verfügbarkeitsgruppenlisteners (innerhalb desselben VNets)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]


