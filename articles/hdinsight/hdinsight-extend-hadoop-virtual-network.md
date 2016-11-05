---
title: Erweitern von HDInsight mit Virtual Network | Microsoft Docs
description: Erfahren Sie, wie Sie HDInsight mithilfe von Azure Virtual Network mit anderen Cloud-Ressourcen oder Ressourcen in Ihrem Rechenzentrum verbinden.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun

ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/13/2016
ms.author: larryfr

---
# Erweitern der HDInsight-Funktionen mit Azure Virtual Network
Mit Azure Virtual Network können Sie Ihre Hadoop-Lösungen erweitern, um lokale Ressourcen wie SQL Server zu integrieren oder sichere private Netzwerke zwischen Ressourcen in der Cloud zu erstellen.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

## <a id="whatis"></a>Was ist Azure Virtual Network?
[Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) ermöglicht das Erstellen eines sicheren, beständigen Netzwerk mit allen Ressourcen, die Sie für Ihre Lösung benötigen. Virtuelle Netzwerke ermöglichen Folgendes:

* Verbinden von Cloudressourcen in einem privaten Netzwerk (nur in der Cloud)
  
    ![Diagramm der Nur-Cloud-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/cloud-only.png)
  
    Die Verwendung von Azure Virtual Network zum Verknüpfen von Azure-Diensten mit HDInsight ermöglicht die folgenden Szenarien:
  
  * **Aufrufen von HDInsight-Diensten oder -Aufträgen** über Azure-Websites oder Dienste, die auf virtuellen Azure-Computern ausgeführt werden.
  * **Direkte Übertragung von Daten** zwischen HDInsight und einer Azure SQL-Datenbank, SQL Server oder einer anderen Datenspeicherlösung, die auf einem virtuellen Computer ausgeführt wird.
  * **Kombination mehrerer HDInsight-Server** zu einer einzelnen Lösung. Ein Beispiel ist die Verwendung eines HDInsight Storm-Servers zum Verarbeiten eingehender Daten mit anschließender Speicherung der verarbeiteten Daten auf einem HDInsight HBase-Server. Die Rohdaten können auch für eine künftige Analyse mit MapReduce auf einem HDInsight Hadoop-Server gespeichert werden.
* Verbinden Ihrer Cloudressourcen mit dem Netzwerk in Ihrem lokalen Datencenter (Standort-zu-Standort oder Punkt-zu-Standort) mithilfe eines virtuellen privaten Netzwerks (VPN)
  
    Mit einer Standort-zu-Standort-Konfiguration können Sie mehrere Ressourcen aus Ihrem Datencenter mit dem virtuellen Azure-Netzwerk über ein Hardware-VPN oder den Routing- und RAS-Dienst verbinden.
  
    ![Diagramm der Standort-zu-Standort-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/site-to-site.png)
  
    Mit einer Punkt-zu-Standort-Konfiguration können Sie eine bestimmte Ressource über ein Software-VPN mit dem virtuellen Azure-Netzwerk verbinden.
  
    ![Diagramm der Punkt-zu-Standort-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/point-to-site.png)
  
    Das Verwenden von Virtual Network zum Verbinden der Cloud mit Ihrem Datencenter ermöglicht ähnliche Szenarien wie die rein cloudbasierte Konfiguration. Doch anstatt auf das Arbeiten mit Ressourcen in der Cloud beschränkt zu sein, können Sie auch mit Ressourcen in Ihrem Datencenter arbeiten.
  
  * **Direkte Übertragung von Daten** zwischen HDInsight und Ihrem Datencenter. Ein Beispiel ist die Übertragung von Daten mithilfe von Sqoop zu oder von SQL Server oder das Lesen von Daten, die von einer Branchenanwendung generiert wurden.
  * **Aufrufen von HDInsight-Diensten oder -Aufträgen** aus einer Branchenanwendung. Ein Beispiel ist die Verwendung von HBase Java-APIs zum Speichern und Abrufen von Daten aus einem HDInsight HBase-Cluster.

Weitere Informationen zu Features, Vorteilen und Funktionen von virtuellen Netzwerken finden Sie unter [Überblick über virtuelle Azure-Netzwerke](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> Sie müssen das virtuelle Azure-Netzwerk erstellen, bevor Sie einen HDInsight-Cluster bereitstellen. Weitere Informationen finden Sie unter [Konfigurationsaufgaben für virtuelle Netzwerke](https://azure.microsoft.com/documentation/services/virtual-network/).
> 
> 

## Anforderungen für Virtual Networks
> [!IMPORTANT]
> Zum Erstellen eines HDInsight-Clusters in einem Virtual Network sind bestimmte Virtual Network-Konfigurationen erforderlich, die in diesem Abschnitt beschrieben werden.
> 
> 

### Standortbasierte virtuelle Netzwerke
Azure HDInsight unterstützt nur standortbasierte virtuelle Netzwerke und kann momentan nicht mit affinitätsgruppenbasierten virtuellen Netzwerken verwendet werden.

### Klassisches oder virtuelles Netzwerk der Version 2
Für Windows-basierte Cluster ist ein klassisches virtuelles Netzwerk erforderlich, Linux-basierte Cluster erfordern ein virtuelles Azure Resource Manager-Netzwerk. Wenn nicht der richtige Netzwerktyp vorhanden ist, kann das Netzwerk nicht zum Erstellen des Clusters verwendet werden.

Wenn sich Ressourcen in einem virtuellen Netzwerk befinden, das nicht von dem zu erstellenden Cluster verwendet werden kann, dann können Sie ein neues virtuelles Netzwerk erstellen, das vom Cluster verwendet werden kann, und es mit dem inkompatiblen virtuellen Netzwerk verbinden. Anschließend können Sie den Cluster in der erforderlichen Netzwerkversion erstellen, und da die beiden Netzwerke verknüpft sind, kann auf die Ressourcen im anderen Netzwerk zugegriffen werden. Weitere Informationen zum Herstellen einer Verbindung zwischen klassischen und neuen virtuellen Netzwerken finden Sie unter [Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

### Benutzerdefiniertes DNS
Wenn Sie ein virtuelles Netzwerk erstellen, stellt Azure eine Standardnamensauflösung für Azure-Dienste wie HDInsight bereit, die im Netzwerk installiert sind. Möglicherweise müssen Sie jedoch Ihr eigenes Domain Name System (DNS) für Situationen wie die plattformübergreifende Auflösung des Domänennamens verwenden. So beispielsweise bei der Kommunikation zwischen Diensten in zwei verknüpften virtuelle Netzwerken. HDInsight unterstützt sowohl die Standardnamensauflösung von Azure als auch das benutzerdefinierte DNS, wenn es mit Azure Virtual Network verwendet wird.

Weitere Informationen zur Verwendung eigener DNS-Server in Azure Virtual Network finden Sie im Abschnitt **Namensauflösung mithilfe eines eigenen DNS-Servers** des Dokuments [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

### Geschützte virtuelle Netzwerke
Der HDInsight-Dienst ist ein verwalteter Dienst, und während der Bereitstellung und Ausführung ist ein Internetzugang erforderlich. So kann Azure die Integrität des Clusters überwachen, Failover von Clusterressourcen initiieren, die Anzahl der Knoten im Cluster durch Skalieren ändern und andere Verwaltungsaufgaben ausführen.

Wenn Sie HDInsight in einem sicheren virtuellen Netzwerk installieren müssen, müssen Sie den eingehenden Zugriff über Port 443 für die folgenden IP-Adressen zulassen, die Azure das Verwalten des HDInsight-Clusters ermöglichen.

* 168\.61.49.99
* 23\.99.5.239
* 168\.61.48.131
* 138\.91.141.162

Das Zulassen des eingehenden Zugriffs über Port 443 von diesen Adressen ermöglicht Ihnen die erfolgreiche Installation von HDInsight in einem geschützten virtuellen Netzwerk.

> [!IMPORTANT]
> HDInsight unterstützt keine Einschränkung des ausgehenden Datenverkehrs, nur des eingehenden Datenverkehrs. Verwenden Sie beim Definieren von Regeln der Netzwerksicherheitsgruppe für das Subnetz, das HDInsight enthält, nur eingehende Regeln.
> 
> 

Die folgenden Beispiele zeigen das Erstellen einer neuen Netzwerksicherheitsgruppe, die die erforderlichen Adressen zulässt, und die Sicherheitsgruppe wird auf ein Subnetz in Ihrem Virtual Network angewendet. Bei diesen Schritten wird vorausgesetzt, dass Sie bereits ein Virtual Network und ein Subnetz erstellt haben, in dem Sie HDInsight installieren möchten.

**Verwenden von Azure PowerShell**

    $vnetName = "Replace with your virtual network name"
    $resourceGroupName = "Replace with the resource group the virtual network is in"
    $subnetName = "Replace with the name of the subnet that HDInsight will be installed into"
    # Get the Virtual Network object
    $vnet = Get-AzureRmVirtualNetwork `
        -Name $vnetName `
        -ResourceGroupName $resourceGroupName
    # Get the region the Virtual network is in.
    $location = $vnet.Location
    # Get the subnet object
    $subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
    # Create a new Network Security Group.
    # And add exemptions for the HDInsight health and management services.
    $nsg = New-AzureRmNetworkSecurityGroup `
        -Name "hdisecure" `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -name "hdirule1" `
            -Description "HDI health and management address 168.61.49.99" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "168.61.49.99" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 300 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule2" `
            -Description "HDI health and management 23.99.5.239" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "23.99.5.239" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 301 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule3" `
            -Description "HDI health and management 168.61.48.131" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "168.61.48.131" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 302 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule4" `
            -Description "HDI health and management 138.91.141.162" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "138.91.141.162" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 303 `
            -Direction Inbound
    # Set the changes to the security group
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    # Apply the NSG to the subnet
    Set-AzureRmVirtualNetworkSubnetConfig `
        -VirtualNetwork $vnet `
        -Name $subnetName `
        -AddressPrefix $subnet.AddressPrefix `
        -NetworkSecurityGroupId $nsg

**Verwenden der Azure-Befehlszeilenschnittstelle**

1. Verwenden Sie den folgenden Befehl, um eine neue Netzwerksicherheitsgruppe namens `hdisecure` zu erstellen. Ersetzen Sie **RESOURCEGROUPNAME** und **LOCATION** durch die Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält, und den Standort (Region), an dem die Gruppe erstellt wurde.
   
        azure network nsg create RESOURCEGROUPNAME hdisecure LOCATION
   
    Nachdem die Gruppe erstellt wurde, erhalten Sie Informationen über die neue Gruppe. Suchen Sie nach einer Zeile ähnlich der folgenden, und speichern Sie die `/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure`-Informationen. Sie werden in einem späteren Schritt verwendet.
   
        data:    Id                              : /subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure
2. Gehen Sie wie folgt vor, um der neuen Netzwerksicherheitsgruppe Regeln hinzuzufügen, die eingehende Kommunikation auf Port 443 aus dem Azure HDInsight-Integritäts- und Verwaltungsdienst ermöglichen. Ersetzen Sie **RESOURCEGROUPNAME** durch den Namen der Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält.
   
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule1 -p "*" -o "*" -u "443" -f "168.61.49.99" -e "VirtualNetwork" -c "Allow" -y 300 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule2 -p "*" -o "*" -u "443" -f "23.99.5.239" -e "VirtualNetwork" -c "Allow" -y 301 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule3 -p "*" -o "*" -u "443" -f "168.61.48.131" -e "VirtualNetwork" -c "Allow" -y 302 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule4 -p "*" -o "*" -u "443" -f "138.91.141.162" -e "VirtualNetwork" -c "Allow" -y 303 -r "Inbound"
3. Nachdem die Regeln erstellt wurden, gehen Sie wie folgt vor, um die neue Netzwerksicherheitsgruppe einem Subnetz zuzuordnen. Ersetzen Sie **RESOURCEGROUPNAME** durch den Namen der Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält. Ersetzen Sie **VNETNAME** und **SUBNETNAME** durch den Namen des virtuellen Azure-Netzwerks und des Subnetzes, die Sie bei der Installation von HDInsight verwenden.
   
        azure network vnet subnet set RESOURCEGROUPNAME VNETNAME SUBNETNAME -w "/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
   
    Nach Abschluss dieses Befehls können Sie HDInsight erfolgreich in dem in diesen Schritten verwendeten Subnetz im gesicherten Virtual Network installieren.

> [!IMPORTANT]
> Die obigen Schritte ermöglichen nur den Zugriff auf den HDInsight-Integritäts- und -Verwaltungsdienst in der Azure-Cloud. So können Sie einen HDInsight-Cluster erfolgreich in einem Subnetz installieren. Der Zugriff auf den HDInsight-Cluster von außerhalb des Virtual Network ist aber standardmäßig blockiert. Sie müssen zusätzliche Netzwerksicherheitsgruppen-Regeln hinzufügen, wenn Sie den Zugriff von außerhalb des Virtual Network ermöglichen möchten.
> 
> Um beispielsweise den SSH-Zugriff über das Internet zu ermöglichen, müssen Sie eine Regel ähnlich der folgenden hinzufügen:
> 
> * Azure PowerShell – ```Add-AzureRmNetworkSecurityRuleConfig -Name "SSSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 304 -Direction Inbound```
> * Azure-CLI – ```azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule4 -p "*" -o "*" -u "22" -f "*" -e "VirtualNetwork" -c "Allow" -y 304 -r "Inbound"```
> 
> 

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie in der [Übersicht über Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md). Informationen zum Steuern des Routings in einem virtuellen Azure-Netzwerk finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).

## <a id="tasks"></a>Aufgaben und Informationen
Dieser Abschnitt enthält Informationen zu allgemeinen Aufgaben und Informationen, die möglicherweise bei der Verwendung von HDInsight mit einem virtuellen Netzwerk erforderlich sind.

### Ermitteln des vollqualifizierten Domänennamens (FQDN)
Der HDInsight-Cluster wird für die Virtual Network-Schnittstelle einen bestimmten FQDN zugewiesen. Hierbei handelt es sich um die Adresse, die Sie von anderen Ressourcen im virtuellen Netzwerk aus beim Herstellen der Verbindung zum Cluster verwenden sollten. Verwenden Sie die folgende URL zum Abfragen des Ambari-Verwaltungsdiensts, um den FQDN zu ermitteln:

    https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/<servicename>/components/<componentname>

> [!NOTE]
> Weitere Informationen zur Verwendung von Ambari mit HDInsight finden Sie unter [Überwachen von Hadoop-Clustern in HDInsight mit der Ambari-API](hdinsight-monitor-use-ambari-api.md).
> 
> 

Sie müssen den Clusternamen und einen Dienst sowie eine Komponente angeben, die auf dem Cluster ausgeführt werden, z. B. den YARN Resource Manager.

> [!NOTE]
> Die Daten werden als JSON-Dokument (JavaScript Object Notation) zurückgegeben, das viele Informationen zur Komponente enthält. Um nur den FQDN zu extrahieren, verwenden Sie einen JSON-Parser zum Abrufen des Werts von `host_components[0].HostRoles.host_name`.
> 
> 

Wenn Sie z. B. den FQDN eines HDInsight Hadoop-Clusters zurückgegeben möchten, können Sie die Daten für den YARN Resource Manager mithilfe einer der folgenden Methoden abrufen:

* [Azure PowerShell](../powershell-install-configure.md)
  
        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix
  
        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)
  
        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/yarn/        components/resourcemanager"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        $FQDN = $JsonObject.host_components[0].HostRoles.host_name
        Write-host $FQDN
* [cURL](http://curl.haxx.se/) und [jq](http://stedolan.github.io/jq/)
  
        curl -G -u <username>:<password> https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/yarn/components/resourcemanager | jq .host_components[0].HostRoles.host_name

### Verbinden mit HBase
Damit Sie mithilfe der Java-API eine Remoteverbindung mit HBase herstellen können, müssen Sie die Zookeeper-Quorumadressen für den HBase-Cluster ermitteln und diese in Ihrer Anwendung angeben.

Verwenden Sie eine der folgenden Methoden zum Abfragen des Ambari-Verwaltungsdiensts, um die Zookeeper-Quorumadresse abzurufen:

* [Azure PowerShell](../powershell-install-configure.md)
  
        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix
  
        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)
  
        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
* [cURL](http://curl.haxx.se/) und [jq](http://stedolan.github.io/jq/)
  
        curl -G -u <username>:<password> "https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum" | jq .items[0].properties[]

> [!NOTE]
> Weitere Informationen zur Verwendung von Ambari mit HDInsight finden Sie unter [Überwachen von Hadoop-Clustern in HDInsight mit der Ambari-API](hdinsight-monitor-use-ambari-api.md).
> 
> 

Nachdem die Quoruminformationen vorliegen, verwenden Sie diese in der Clientanwendung.

Für eine Java-Anwendung, die die HBase-API verwendet, würden Sie beispielsweise die Datei **hbase-site.xml** zum Projekt hinzufügen und die Quoruminformationen wie folgt in der Datei angeben:

```
<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>zookeeper0.address,zookeeper1.address,zookeeper2.address</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.clientPort</name>
    <value>2181</value>
  </property>
</configuration>
```

### Überprüfen der Netzwerkkonnektivität
Einige Dienste wie SQL Server können eingehende Netzwerkverbindungen begrenzen. Dadurch wird verhindert, dass HDInsight mit diesen Diensten erfolgreich arbeiten kann.

Wenn beim Zugriff auf einen Dienst von HDInsight Probleme auftreten, ziehen Sie die zum Dienst gehörende Dokumentation zu Rate, um sicherzustellen, dass Sie den Netzwerkzugriff aktiviert haben. Sie können den Netzwerkzugriff auch dadurch überprüfen, dass Sie einen virtuellen Azure-Computer im selben virtuellen Netzwerk erstellen und Clienthilfsprogramme zur Überprüfung verwenden, dass der virtuelle Computer über das virtuelle Netzwerk eine Verbindung mit dem Dienst herstellen kann.

## <a id="nextsteps"></a>Nächste Schritte
In den folgenden Beispielen wird die Verwendung von HDInsight mit Azure Virtual Network veranschaulicht:

* [Analysieren von Sensordaten mit Storm und HBase in HDInsight](hdinsight-storm-sensor-data-analysis.md) – Veranschaulicht die Konfiguration eines Storm- und HBase-Clusters in einem virtuellen Netzwerk sowie das Remoteschreiben von Daten aus Storm in HBase.
* [Bereitstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) – Bietet Informationen zur Bereitstellung von Hadoop-Clustern, einschließlich Informationen zur Verwendung von Azure Virtual Network.
* [Verwenden von Sqoop mit Hadoop in HDInsight](hdinsight-use-sqoop-mac-linux.md) – Bietet Informationen zur Verwendung von Sqoop zum Übertragen von Daten mit SQL Server über ein virtuelles Netzwerk.

Weitere Informationen zu virtuellen Azure Virtual-Netzwerken finden Sie unter [Überblick über Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

<!---HONumber=AcomDC_0914_2016-->