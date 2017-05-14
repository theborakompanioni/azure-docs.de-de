---
title: "Herstellen einer Verbindung mit Kafka in HDInsight mithilfe von virtuellen Netzwerken – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie mit dem Kafka-Python-Client eine Remoteverbindung mit Kafka in HDInsight herstellen. Für die Konfiguration in diesem Dokument wird HDInsight in einem virtuellen Azure-Netzwerk verwendet. Der Remoteclient stellt eine Verbindung mit dem virtuellen Netzwerk über ein Point-to-Site-VPN-Gateway her."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.custom: hdinsightactive
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/18/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 9489fdc3c5388a7510bc4411b4abb05fa72fbf4f
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---

# <a name="connect-to-kafka-on-hdinsight-preview-through-an-azure-virtual-network"></a>Herstellen einer Verbindung mit Kafka in HDInsight (Vorschau) über ein virtuelles Azure-Netzwerk

Erfahren Sie, wie Sie eine Verbindung mit Kafka in HDInsight mithilfe von virtuellen Azure-Netzwerken herstellen. Kafka-Clients (Producer und Consumer) können direkt in HDInsight oder auf Remotesystemen ausgeführt werden. Remoteclients müssen eine Verbindung mit Kafka in HDInsight über ein virtuelles Azure-Netzwerk herstellen. Verwenden Sie die Informationen in diesem Dokument, um zu verstehen, wie Remoteclients mithilfe von virtuellen Azure-Netzwerken eine Verbindung mit HDInsight herstellen können.

> [!IMPORTANT]
> Einige der in diesem Dokument beschriebenen Konfigurationen können mit Windows-, macOS- oder Linux-Clients verwendet werden. Das aufgeführte Point-to-Site-Beispiel ermöglicht jedoch nur einen VPN-Client für Windows.
>
> Im Beispiel wird zudem ein Python-Client ([Kafka-Python](http://kafka-python.readthedocs.io/en/master/)) zum Überprüfen der Kommunikation mit Kafka in HDInsight verwendet.

## <a name="architecture-and-planning"></a>Architektur und Planung

HDInsight-Cluster werden in einem virtuellen Azure-Netzwerk gesichert und lassen nur eingehenden SSH- und HTTPS-Datenverkehr zu. Datenverkehr wird über ein öffentliches Gateway empfangen, das keinen Datenverkehr von Kafka-Clients weiterleitet. Um von einem Remoteclient auf Kafka zuzugreifen, müssen Sie ein virtuelles Azure-Netzwerk erstellen, das ein VPN-Gateway (virtuelles privates Netzwerk) bereitstellt. Nachdem Sie das virtuelle Netzwerk und das Gateway konfiguriert haben, installieren Sie HDInsight im virtuellen Netzwerk und stellen über das VPN-Gateway eine Verbindung damit her.

![Ein Diagramm von HDInsight in einem virtuellen Azure-Netzwerk mit einem Client, der über VPN verbunden ist](media/hdinsight-apache-kafka-connect-vpn-gateway/hdinsight-in-virtual-network.png)

Die folgende Liste enthält Informationen zur Verwendung von Kafka in HDInsight mit einem virtuellen Netzwerk:

1. Erstellen Sie ein virtuelles Netzwerk. Spezifische Informationen zur Verwendung von HDInsight mit virtuellen Azure-Netzwerken finden Sie im Dokument [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) .

2. (Optional) Erstellen Sie einen virtuellen Azure-Computer im virtuellen Netzwerk, und installieren Sie darauf einen benutzerdefinierten DNS-Server. Dieser DNS-Server wird verwendet, um die Namensauflösung für Remoteclients in einer Site-to-Site- oder VNET-to-VNET-Konfiguration zu ermöglichen. Weitere Informationen finden Sie im Dokument [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

3. Erstellen Sie ein VPN-Gateway für das virtuelle Netzwerk. Weitere Informationen zu VPN-Gatewaykonfigurationen finden Sie im Dokument [Informationen zu VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

4. Erstellen Sie HDInsight im virtuellen Netzwerk. Wenn Sie einen benutzerdefinierten DNS-Server für das Netzwerk konfiguriert haben, wird HDInsight automatisch für dessen Nutzung konfiguriert.

5. (Optional) Wenn Sie keinen benutzerdefinierten DNS-Server verwendet haben und keine Namensauflösung zwischen Clients und dem virtuellen Netzwerk nutzen, müssen Sie Kafka zum Ankündigen der IP-Adresse konfigurieren. Weitere Informationen finden Sie im Abschnitt [Konfigurieren von Kafka zum Ankündigen der IP-Adresse](#configure-kafka-for-ip-advertising) dieses Dokuments.

## <a name="create-using-powershell"></a>Erstellen mithilfe von PowerShell

Die Schritte in diesem Abschnitt erstellen die folgende Konfiguration mit [Azure PowerShell](/powershell/azure/overview):

* Azure Virtual Network
* Point-to-Site-VPN-Gateway
* Azure-Speicherkonto (von HDInsight verwendet)
* Kafka in HDInsight

1. Führen Sie die Schritte im Dokument [Erstellen eines selbstsignierten Stammzertifikats für Point-to-Site-Verbindungen mit PowerShell](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) aus, um die für das Gateway benötigten Zertifikate zu erstellen.

2. Öffnen Sie eine PowerShell-Eingabeaufforderung, und verwenden Sie den folgenden Code, um sich bei Ihrem Azure-Abonnement anzumelden:

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. Verwenden Sie den folgenden Code zum Erstellen von Variablen, die Konfigurationsinformationen enthalten:

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is the resource group name?"
    $baseName = Read-Host "What is the base name? It is used to create names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want to create the resources in?"
    $rootCert = Read-Host "What is the file path to the root certificate? It is used to secure the VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter the HTTPS user name and password for the HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter the SSH user name and password for the HDInsight cluster" -UserName "sshuser"

    # Names for Azure resources
    $networkName = "net-$baseName"
    $clusterName = "kafka-$baseName"
    $storageName = "store$baseName" # Can't use dashes in storage names
    $defaultContainerName = $clusterName
    $defaultSubnetName = "default"
    $gatewaySubnetName = "GatewaySubnet"
    $gatewayPublicIpName = "GatewayIp"
    $gatewayIpConfigName = "GatewayConfig"
    $vpnRootCertName = "rootcert"
    $vpnName = "VPNGateway"

    # Network settings
    $networkAddressPrefix = "10.0.0.0/16"
    $defaultSubnetPrefix = "10.0.0.0/24"
    $gatewaySubnetPrefix = "10.0.1.0/24"
    $vpnClientAddressPool = "172.16.201.0/24"

    # HDInsight settings
    $HdiWorkerNodes = 4
    $hdiVersion = "3.5"
    $hdiType = "Kafka"
    ```

4. Verwenden Sie den folgenden Code, um die Azure-Ressourcengruppe und das virtuelle Netzwerk zu erstellen:

    ```powershell
    # Create the resource group that contains everything
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
    New-AzureRmVirtualNetworkGateway -Name $vpnName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -IpConfigurations $gatewayIpConfig `
        -GatewayType Vpn `
        -VpnType RouteBased `
        -EnableBgp $false `
        -GatewaySku Standard `
        -VpnClientAddressPool $vpnClientAddressPool `
        -VpnClientRootCertificates $p2sRootCert
    ```

    > [!WARNING]
    > Dieser Prozess kann mehrere Minuten dauern.

5. Verwenden Sie den folgenden Code, um das Azure-Speicherkonto und Blobcontainer zu erstellen:

    ```powershell
    # Create the storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. Verwenden Sie den folgenden Code zum Erstellen des HDInsight-Clusters:

    ```powershell
    # Create the HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $hdiWorkerNodes `
        -ClusterType $hdiType `
        -OSType Linux `
        -Version $hdiVersion `
        -HttpCredential $adminCreds `
        -SshCredential $sshCreds `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageKey `
        -DefaultStorageContainer $defaultContainerName `
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

  > [!WARNING]
  > Für diesen Prozess werden etwa 20 Minuten benötigt.

8. Verwenden Sie das folgende Cmdlet, um die URL für den Windows-VPN-Client für das virtuelle Netzwerk abzurufen:

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    Verwenden Sie zum Herunterladen des Windows-VPN-Clients den zurückgegebenen URI in Ihrem Webbrowser.

## <a name="configure-kafka-for-ip-advertising"></a>Konfigurieren von Kafka zum Ankündigen der IP-Adresse

Standardmäßig gibt Zookeeper den Domänennamen der Kafka-Broker an Clients zurück. Diese Konfiguration funktioniert für den VPN-Client nicht, da er die Namensauflösung nicht für Entitäten im virtuellen Netzwerk verwenden kann. Verwenden Sie die folgenden Schritte, um Kafka in HDInsight zum Ankündigen von IP-Adressen anstelle von Domänennamen zu konfigurieren:

1. Wechseln Sie in Ihrem Webbrowser zu „https://CLUSTERNAME.azurehdinsight.net“. Ersetzen Sie __CLUSTERNAME__ durch den Namen von Kafka im HDInsight-Cluster.

    Geben Sie bei entsprechender Aufforderung den HTTPS-Benutzernamen und das Kennwort für den Cluster ein. Die Ambari-Webbenutzeroberfläche für den Cluster wird angezeigt.

2. Wählen Sie zum Anzeigen von Informationen über Kafka aus der Liste auf der linken Seite __Kafka__ aus.

    ![Dienstliste, in der Kafka hervorgehoben ist](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. Wählen Sie zum Anzeigen der Kafka-Konfiguration oben in der Mitte __Konfigurationen__ aus.

    ![Konfigurationslinks für Kafka](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. Geben Sie zum Suchen der Konfiguration __kafka-env__ oben rechts in das Feld __Filter__ die Zeichenfolge `kafka-env` ein.

    ![Kafka-Konfiguration für kafka-env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. Um Kafka zum Ankündigen von IP-Adressen zu konfigurieren, fügen Sie den folgenden Text am Ende des Felds __Vorlage für kafka-env__ hinzu:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Geben Sie zum Konfigurieren der Schnittstelle, an der Kafka lauscht, oben rechts in das Feld __Filter__ die Zeichenfolge `listeners` ein.

7. Um Kafka zum Lauschen an allen Netzwerkschnittstellen zu konfigurieren, ändern Sie den Wert im Feld __Listener__ in `PLAINTEXT://0.0.0.0:92092`.

8. Klicken Sie auf die Schaltfläche __Speichern__, um die Konfigurationsänderungen zu speichern. Geben Sie eine Textnachricht ein, die die Änderungen beschreibt. Wählen Sie __OK__ aus, nachdem die Änderungen gespeichert wurden.

    ![Schaltfläche zum Speichern der Konfiguration](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. Um Fehler beim Neustart von Kafka zu vermeiden, verwenden Sie die Schaltfläche __Dienstaktionen__, und wählen Sie __Wartungsmodus aktivieren__ aus. Wählen Sie „OK“ aus, um diesen Vorgang abzuschließen.

    ![Dienstaktionen, „Wartungsmodus aktivieren“ hervorgehoben](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. Verwenden Sie zum Neustarten von Kafka die Schaltfläche __Neu starten__, und wählen Sie __Alle betroffenen Instanzen neu starten__ aus. Bestätigen Sie den Neustart, und verwenden Sie dann die Schaltfläche __OK__, wenn der Vorgang abgeschlossen ist.

    ![Schaltfläche „Neu starten“, Funktion zum Neustarten aller betroffenen Instanzen hervorgehoben](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. Um den Wartungsmodus zu deaktivieren, verwenden die Schaltfläche __Dienstaktionen__, und wählen Sie __Wartungsmodus deaktivieren__ aus. Wählen Sie **OK** aus, um diesen Vorgang abzuschließen.

## <a name="connect-to-the-vpn-gateway"></a>Herstellen einer Verbindung mit dem VPN-Gateway

Verwenden Sie zum Herstellen einer Verbindung mit dem VPN-Gateway über einen __Windows-Client__ den Abschnitt __Herstellen einer Verbindung mit Azure__ im Dokument [Konfigurieren einer Point-to-Site-VPN-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#a-nameclientcertificatea7---install-an-exported-client-certificate).

## <a name="remote-kafka-client"></a>Kafka-Remoteclient

Um über den Clientcomputer eine Verbindung mit Kafka herzustellen, müssen Sie die IP-Adresse der Kafka-Broker oder Zookeeper-Knoten (je nach Clientanforderungen) verwenden. Verwenden Sie die folgenden Schritte, um die IP-Adresse der Kafka-Broker abzurufen, und nutzen Sie diese dann über eine Python-Anwendung

1. Verwenden Sie das folgende Skript, um die IP-Adressen der Knoten im Cluster abzurufen:

    ```powershell
    # Get the NICs for the HDInsight workernodes (names contain 'workernode').
    $nodes = Get-AzureRmNetworkInterface `
        -ResourceGroupName $resourceGroupName `
        | where-object {$_.Name -like "*workernode*"}

    # Loop through each node and get the IP address
    foreach($node in $nodes) {
        $node.IpConfigurations.PrivateIpAddress
    }
    ```

    In diesem Skript wird vorausgesetzt, dass `$resourceGroupName` der Name der Azure-Ressourcengruppe ist, die das virtuelle Netzwerk enthält. Die Ausgabe des Skripts ähnelt dem folgenden Text:

        10.0.0.12
        10.0.0.6
        10.0.0.13
        10.0.0.5

    > [!NOTE]
    > Wenn Ihr Kafka-Client Zookeeper-Knoten statt Kafka-Broker verwendet, ersetzen Sie im PowerShell-Skript `*workernode*` durch `*zookeepernode*`.

    > [!WARNING]
    > Wenn Sie den Cluster skalieren oder Knoten ausfallen und ersetzt werden, können sich die IP-Adressen ändern. Derzeit besteht keine Möglichkeit, vorab bestimmte IP-Adressen zu Knoten in einem HDInsight-Cluster zuzuweisen.

2. Verwenden Sie Folgendes zum Installieren des [kafka-python](http://kafka-python.readthedocs.io/)-Clients:

        pip install kafka-python

3. Verwenden Sie zum Senden von Daten an Kafka den folgenden Python-Code:

  ```python
  from kafka import KafkaProducer
  # Replace the `ip_address` entries with the IP address of your worker nodes
  producer = KafkaProducer(bootstrap_servers=['ip_address1','ip_address2','ip_adderess3','ip_address4'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    Ersetzen Sie die `'ip_address'`-Einträge durch die Adressen, die in Schritt 1 in diesem Abschnitt zurückgegeben wurden.

    > [!NOTE]
    > Dieser Code sendet die Zeichenfolge `test message` an das Thema `testtopic`. Die Standardkonfiguration von Kafka in HDInsight ist, dass das Thema erstellt wird, wenn es nicht vorhanden ist.

4. Verwenden Sie zum Abrufen von Nachrichten von Kafka den folgenden Python-Code:

   ```python
   from kafka import KafkaConsumer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # Note: auto_offset_reset='earliest' resets the starting offset to the beginning
   #       of the topic
   consumer = KafkaConsumer(bootstrap_servers=['ip_address1','ip_address2','ip_adderess3','ip_address4'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    Ersetzen Sie die `'ip_address'`-Einträge durch die Adressen, die in Schritt 1 in diesem Abschnitt zurückgegeben wurden. Die Ausgabe enthält die Testnachricht, die im vorherigen Schritt an den Producer gesendet wurde.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Probleme beim Herstellen einer Verbindung mit dem virtuellen Netzwerk oder beim Herstellen einer Verbindung mit HDInsight über das Netzwerk haben, finden Sie Hilfe im Dokument [Problembehandlung bei Virtual Network-Gateways und -Verbindungen](../network-watcher/network-watcher-troubleshoot-manage-powershell.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen eines virtuellen Azure-Netzwerks mit Point-to-Site-VPN-Gateway finden Sie in den folgenden Dokumenten:

* [Konfigurieren einer Point-to-Site-Verbindung mit einem VNET über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Konfigurieren einer Point-to-Site-VPN-Verbindung (P2S) mit einem VNET mithilfe von PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

Weitere Informationen zur Verwendung von Kafka in HDInsight finden Sie in den folgenden Dokumenten:

* [Erste Schritte mit Apache Kafka (Vorschau) in HDInsight](hdinsight-apache-kafka-get-started.md)
* [Verwenden von MirrorMaker zum Erstellen eines Replikats von Kafka in einem HDInsight-Cluster (Vorschau)](hdinsight-apache-kafka-mirroring.md)

