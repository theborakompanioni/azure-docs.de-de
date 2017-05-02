---
title: Erweitern von HDInsight mit Virtual Network | Microsoft Docs
description: Erfahren Sie, wie Sie HDInsight mithilfe von Azure Virtual Network mit anderen Cloud-Ressourcen oder Ressourcen in Ihrem Rechenzentrum verbinden.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/13/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 938f6e4bf7d9d6418c9b10531725807a0a9dbce4
ms.lasthandoff: 04/14/2017


---
# <a name="extend-hdinsight-capabilities-by-using-azure-virtual-network"></a>Erweitern der HDInsight-Funktionen mit Azure Virtual Network

Erfahren Sie, wie Sie virtuelle Azure-Netzwerke mit HDInsight für die folgenden Szenarien verwenden:

* Beschränken des Zugriffs auf HDInsight. Sie können z.B. eingehenden Datenverkehr aus dem Internet verhindern.

* Direktes Zugreifen auf Dienste in HDInsight, die nicht über das Internet verfügbar gemacht werden. Sie können z.B. direkt mit Kafka-Brokern arbeiten oder die HBase-Java-API verwenden.

* Direktes Verbinden von Diensten mit HDInsight. Sie können z.B. Oozie zum Importieren oder Exportieren von Daten in eine SQL Server-Instanz in Ihrem Rechenzentrum verwenden.

* Erstellen von Lösungen mit mehreren HDInsight-Clustern. Verwenden Sie z.B. Spark oder Storm, um in Kafka gespeicherte Daten zu analysieren.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Befehlszeilenschnittstelle 2.0: Weitere Informationen finden Sie unter [Install and Configure Azure CLI 2.0 (Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle 2.0)](https://docs.microsoft.com/cli/azure/install-az-cli2).

* Azure PowerShell: Weitere Informationen finden Sie unter [Install and Configure Azure PowerShell (Installieren und Konfigurieren von Azure PowerShell)](https://docs.microsoft.com/powershell/azure/overview).

> [!NOTE]
> Für die Schritte in diesem Dokument benötigen Sie die neueste Version der Azure-Befehlszeilenschnittstelle und die neueste Version von Azure PowerShell. Bei Verwendung einer älteren Version weichen die Befehle möglicherweise ab. Verwenden Sie am besten die oben bereitgestellten Links, um jeweils die neueste Version zu installieren.

## <a id="whatis"></a>Was ist Azure Virtual Network?

[Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) ermöglicht das Erstellen eines sicheren, beständigen Netzwerk mit allen Ressourcen, die Sie für Ihre Lösung benötigen.

Folgende Überlegungen sollten Sie bei Verwendung von HDInsight in einem virtuellen Netzwerk anstellen:

* __Klassische und virtuelle Resource Manager-Netzwerke__: Bestimmen Sie anhand der folgenden Tabelle den zu verwendenden Netzwerktyp basierend auf dem Betriebssystem des HDInsight-Clusters:

    | HDInsight-Betriebssystem | Klassisches virtuelles Netzwerk | Virtuelles Netzwerk mit Resource Manager |
    | ---- | ---- | ---- |
    | Linux | no | Ja |
    | Windows | Ja | no |

    Verknüpfen Sie die beiden Netzwerke, um auf Ressourcen in einem inkompatiblen virtuellen Netzwerk zuzugreifen. Weitere Informationen zum Herstellen einer Verbindung zwischen klassischen und virtuellen Netzwerken mit Resource Manager finden Sie unter [Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

* __Benutzerdefiniertes DNS__: Azure bietet Namensauflösung für Azure-Dienste, die in einem virtuellen Azure-Netzwerk installiert sind. Diese Namensauflösung gilt nicht außerhalb des virtuellen Netzwerks. Um die Namensauflösung für Ressourcen außerhalb des virtuellen Netzwerks zu ermöglichen, müssen Sie einen benutzerdefinierten DNS-Server verwenden. Weitere Informationen zur Verwendung eines benutzerdefinierten DNS-Servers finden Sie im Dokument [Namensauflösung mithilfe eines eigenen DNS-Servers](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

* __Tunnelerzwingung__: HDInsight unterstützt für Azure Virtual Network keine Konfiguration mit Tunnelerzwingung.

* __Einschränken des Netzwerkdatenverkehrs__: HDInsight unterstützt die Verwendung von Netzwerksicherheitsgruppen zum Einschränken des Netzwerkdatenverkehrs, benötigt jedoch uneingeschränkten Zugriff auf verschiedene Azure-IP-Adressen. Weitere Informationen finden Sie im Abschnitt [Geschützte virtuelle Netzwerke](#secured-virtual-networks).

### <a name="connect-cloud-resources-together-in-a-private-network-cloud-only"></a>Verbinden von Cloudressourcen in einem privaten Netzwerk (nur in der Cloud)

![Diagramm der Nur-Cloud-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/cloud-only.png)

Die Verwendung von Azure Virtual Network zum Verknüpfen von Azure-Diensten mit HDInsight ermöglicht die folgenden Szenarien:

* **Aufrufen von HDInsight-Diensten oder -Aufträgen** über Azure-Websites oder Dienste, die auf virtuellen Azure-Computern ausgeführt werden.

* **Direkte Übertragung von Daten** zwischen HDInsight und einer Azure SQL-Datenbank, SQL Server oder einer anderen Datenspeicherlösung, die auf einem virtuellen Computer ausgeführt wird.

* **Kombination mehrerer HDInsight-Server** zu einer einzelnen Lösung. Es gibt viele verschiedene Typen von HDInsight-Clusters, die der Workload oder Technologie entsprechen, für die der Cluster optimiert ist. Es ist keine unterstützte Methode zum Erstellen eines Clusters vorhanden, bei der mehrere Typen kombiniert werden, z.B. Storm und HBase in einem Cluster. Die Verwendung eines virtuellen Netzwerks ermöglicht mehreren Clustern die direkte Kommunikation miteinander.

### <a name="connect-cloud-resources-to-a-local-datacenter-network"></a>Verbinden von Cloudressourcen mit dem Netzwerk eines lokalen Rechenzentrums

Mit einer Standort-zu-Standort-Konfiguration können Sie mehrere Ressourcen in Ihrem Rechenzentrum mit dem virtuellen Azure-Netzwerk verbinden. Die Verbindung kann mittels eines VPN-Hardwaregeräts oder über den Routing- und RAS-Dienst erfolgen.

![Diagramm der Standort-zu-Standort-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/site-to-site.png)

Mit einer Punkt-zu-Standort-Konfiguration können Sie eine bestimmte Ressource über ein Software-VPN mit dem virtuellen Azure-Netzwerk verbinden.

![Diagramm der Punkt-zu-Standort-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/point-to-site.png)

Das Verwenden von Virtual Network zum Verbinden der Cloud mit Ihrem Datencenter ermöglicht ähnliche Szenarien wie die rein cloudbasierte Konfiguration. Doch anstatt auf das Arbeiten mit Ressourcen in der Cloud beschränkt zu sein, können Sie auch mit Ressourcen in Ihrem Datencenter arbeiten.

* **Direkte Übertragung von Daten** zwischen HDInsight und Ihrem Datencenter. Ein Beispiel ist die Übertragung von Daten mithilfe von Sqoop zu oder von SQL Server oder das Lesen von Daten, die von einer Branchenanwendung generiert wurden.

* **Aufrufen von HDInsight-Diensten oder -Aufträgen** aus einer Branchenanwendung. Ein Beispiel ist die Verwendung von HBase Java-APIs zum Speichern und Abrufen von Daten aus einem HDInsight HBase-Cluster.

Weitere Informationen zu Features, Vorteilen und Funktionen von virtuellen Netzwerken finden Sie unter [Überblick über virtuelle Azure-Netzwerke](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> Erstellen Sie das virtuelle Azure-Netzwerk, bevor Sie einen HDInsight-Cluster bereitstellen, und geben Sie während der Erstellung des Clusters das Netzwerk an. Weitere Informationen finden Sie unter [Konfigurationsaufgaben für virtuelle Netzwerke](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="secured-virtual-networks"></a>Geschützte virtuelle Netzwerke

Der HDInsight-Dienst ist ein verwalteter Dienst, und während der Bereitstellung und Ausführung ist der Zugriff auf Azure-Verwaltungsdienste erforderlich. Die Azure-Verwaltung führt die folgenden Dienste aus:

* Überwachen der Integrität des Clusters
* Initiieren eines Failovers von Clusterressourcen
* Ändern der Anzahl von Knoten im Cluster durch Skalierungsvorgänge
* Andere Verwaltungsaufgaben

> [!NOTE]
> Diese Vorgänge erfordern keine vollen Zugriff auf das Internet. Wenn Sie den Zugriff auf das Internet einschränken, erlauben Sie eingehenden Zugriff auf Port 443 für die folgenden IP-Adressen. Dadurch kann Azure HDInsight verwalten:

Die IP-Adressen, die zugelassen werden sollten, sind spezifisch für die Region, in der sich der HDInsight-Cluster und die Virtual Network-Instanz befinden. Verwenden Sie die folgende Tabelle, um die IP-Adressen für Ihre Region zu ermitteln.

| Country | Region | Zulässige IP-Adressen | Zulässiger Port |
| ---- | ---- | ---- | ---- |
| Brasilien | Brasilien Süd | 191.235.84.104</br>191.235.87.113 | 443 |
| Kanada | Kanada, Osten | 52.229.127.96</br>52.229.123.172 | 443 |
| &nbsp; | Kanada, Mitte | 52.228.37.66</br>52.228.45.222 | 443 |
| Indien | Indien (Mitte) | 52.172.153.209</br>52.172.152.49 | 443 |
| Vereinigtes Königreich | UK, Westen | 51.141.13.110</br>51.141.7.20 | 443 |
| &nbsp; | UK, Süden | 51.140.47.39</br>51.140.52.16 | 443 |
| USA | USA, Westen-Mitte | 52.161.23.15</br>52.161.10.167 | 443 |
| &nbsp; | USA, Westen 2 | 52.175.211.210</br>52.175.222.222 | 443 |

__Wenn Ihre Region in der Tabelle nicht aufgeführt ist__, lassen Sie für die folgenden IP-Adressen Datenverkehr über Port __443__ zu:

* 168.61.49.99
* 23.99.5.239
* 168.61.48.131
* 138.91.141.162

> [!IMPORTANT]
> HDInsight unterstützt keine Einschränkung des ausgehenden Datenverkehrs, nur des eingehenden Datenverkehrs. Verwenden Sie beim Definieren von Regeln der Netzwerksicherheitsgruppe für das Subnetz, das HDInsight enthält, __nur eingehende Regeln__.

### <a name="working-with-hdinsight-in-secured-virtual-networks"></a>Arbeiten mit HDInsight in geschützten virtuellen Netzwerken

Wenn Sie den Internetzugriff blockieren, können Sie keine HDInsight-Dienste verwenden, die normalerweise über das öffentliche Gateway für einen Cluster bereitgestellt werden. Dazu gehören Ambari und SSH. Stattdessen müssen Sie über die interne IP-Adresse der Clusterhauptknoten auf Dienste zugreifen.

Die interne IP-Adresse der Hauptknoten können Sie mithilfe der Skripts im Abschnitt [Interne IP-Adressen und FQDNs](#internal-ips-and-fqdns) suchen.

### <a name="example-secured-virtual-network"></a>Beispiel: Geschütztes virtuelles Netzwerk

In den folgenden Beispielen wird veranschaulicht, wie Sie eine Netzwerksicherheitsgruppe erstellen, die eingehenden Datenverkehr über Port 443 von den folgenden IP-Adressen zulässt:

* 168.61.49.99
* 23.99.5.239
* 168.61.48.131
* 138.91.141.162

> [!IMPORTANT]
> Diese Adressen werden für Regionen verwendet, für die keine speziellen IP-Adressen aufgeführt sind. Um die IP-Adressen für Ihre Region zu ermitteln, verwenden Sie die Informationen im Abschnitt [Geschützte virtuelle Netzwerke](#secured-virtual-networks).

Bei diesen Schritten wird vorausgesetzt, dass Sie bereits ein Virtual Network und ein Subnetz erstellt haben, in dem Sie HDInsight installieren möchten. Informationen hierzu finden Sie unter [Erstellen eines virtuellen Netzwerks im Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

> [!WARNING]
> Regeln werden anhand des Netzwerkdatenverkehrs gemäß ihrer __Priorität__ getestet. Sobald eine Regel den Testkriterien entspricht, wird sie angewendet, und es werden keine weiteren Regeln für diese Anfrage getestet. Bei einer Regel, die eingehenden Datenverkehr generell blockiert (z.B. eine **Alle ablehnen**-Regel), __muss__ diese nach den Regeln angegeben werden, die Datenverkehr zulassen.
>
> Weitere Informationen zu Netzwerksicherheitsgruppen-Regeln finden Sie im Artikel [Was ist eine Netzwerksicherheitsgruppe?](../virtual-network/virtual-networks-nsg.md)

**Beispiel: Azure Resource Manager-Vorlage**

Verwenden Sie die folgende Resource Manager-Vorlage von den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/), um einen HDInsight-Cluster in einem VNET mit den Konfigurationseinstellungen für ein sicheres Netzwerk zu erstellen:

[Bereitstellen eines sicheren Azure VNET und eines HDInsight Hadoop-Clusters innerhalb des VNET](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

**Beispiel: Azure PowerShell**

```powershell
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
# Create a Network Security Group.
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
    -NetworkSecurityGroup $nsg
```

**Beispiel: Azure-Befehlszeilenschnittstelle**

1. Verwenden Sie den folgenden Befehl, um eine neue Netzwerksicherheitsgruppe namens `hdisecure`zu erstellen. Ersetzen Sie **RESOURCEGROUPNAME** durch die Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält. Ersetzen Sie **STANDORT** durch den Standort (Region), in der die Gruppe erstellt wurde.

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    Nachdem die Gruppe erstellt wurde, erhalten Sie Informationen über die neue Gruppe.

2. Gehen Sie wie folgt vor, um der neuen Netzwerksicherheitsgruppe Regeln hinzuzufügen, die eingehende Kommunikation auf Port 443 aus dem Azure HDInsight-Integritäts- und Verwaltungsdienst ermöglichen. Ersetzen Sie **RESOURCEGROUPNAME** durch den Namen der Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99/24" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239/24" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule3 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131/24" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule4 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162/24" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    ```

3. Gehen Sie nach dem Erstellen der Regeln wie folgt vor, um den eindeutigen Bezeichner für diese Netzwerksicherheitsgruppe abzurufen:

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    Dieser Befehl gibt einen Wert zurück, der in etwa wie folgt aussieht:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Schließen Sie die ID in doppelte Anführungszeichen ein, wenn Sie nicht die erwarteten Ergebnisse erhalten.

4. Verwenden Sie den folgenden Befehl, um die Netzwerksicherheitsgruppe auf ein Subnetz anzuwenden. Ersetzen Sie die Werte __GUID__ und __RESOURCEGROUPNAME__ durch die Rückgabewerte aus dem vorherigen Schritt. Ersetzen Sie __VNETNAME__ und __SUBNETNAME__ durch den Namen des virtuellen Netzwerks bzw. durch den Namen des Subnetzes, die Sie beim Erstellen eines HDInsight-Clusters verwenden möchten.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Nach Abschluss dieses Befehls können Sie HDInsight erfolgreich in dem in diesen Schritten verwendeten Subnetz im gesicherten Virtual Network installieren.

> [!IMPORTANT]
> Die nachfolgenden Schritte ermöglichen nur den Zugriff auf den HDInsight-Integritäts- und -Verwaltungsdienst in der Azure-Cloud. Jeder andere Zugriff auf den HDInsight-Cluster von außerhalb des virtuellen Netzwerks wird blockiert. Wenn Sie den Zugriff von außerhalb des virtuellen Netzwerks ermöglichen möchten, müssen Sie zusätzliche Netzwerksicherheitsgruppen-Regeln hinzufügen.
>
> Im folgenden Beispiel wird veranschaulicht, wie der SSH-Zugriff über das Internet aktiviert wird:
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 304 -Direction Inbound
> ```
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
> ```

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie in der [Übersicht über Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md). Informationen zum Steuern des Routings in einem virtuellen Azure-Netzwerk finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).

## <a name="retrieve-internal-ips-and-fqdns"></a>Abrufen interner IP-Adressen und FQDNs

Wenn Sie eine Verbindung zu HDInsight mithilfe eines virtuellen Netzwerks herstellen, können Sie sich direkt mit den Knoten im Cluster verbinden. Verwenden Sie die folgenden Skripts, um die interne IP-Adresse und den vollqualifizierten Domänennamen (FQDN) für die Knoten im Cluster zu bestimmen:

**Azure PowerShell**

```powershell
$resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"

$clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

$nodes = @()
foreach($nic in $clusterNICs) {
    $node = new-object System.Object
    $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
    $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
    $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
    $nodes += $node
}
$nodes | sort-object Type
```

__Azure-Befehlszeilenschnittstelle__

```azurecli
az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
```

> [!IMPORTANT]
> Ersetzen Sie im Beispiel für Azure CLI 2.0 `<resourcegroupname>` durch den Namen der Ressourcengruppe, die das virtuelle Netzwerk enthält.

Die Skripts funktionieren durch Abfragen der virtuellen Netzwerkkarten (NICs) für den Cluster. Die NICs befinden sich in der Ressourcengruppe, die das von HDInsight verwendete virtuelle Netzwerk enthält.

## <a id="nextsteps"></a>Nächste Schritte

In den folgenden Beispielen wird die Verwendung von HDInsight mit Azure Virtual Network veranschaulicht:

* [HBase-Cluster in Azure Virtual Network](hdinsight-hbase-provision-vnet.md)

* [Analysieren von Sensordaten mit Storm und HBase in HDInsight](hdinsight-storm-sensor-data-analysis.md)

* [Bereitstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md)

* [Verwenden von Sqoop mit Hadoop in HDInsight](hdinsight-use-sqoop-mac-linux.md)

Weitere Informationen zu virtuellen Azure Virtual-Netzwerken finden Sie unter [Überblick über Azure Virtual Network](../virtual-network/virtual-networks-overview.md).


