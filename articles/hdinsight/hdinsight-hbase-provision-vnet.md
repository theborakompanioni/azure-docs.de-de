---
title: Erstellen von HBase-Clustern in einem virtuellen Netzwerk | Microsoft Docs
description: "Erste Schritte mit HBase in Azure HDInsight. Erfahren Sie, wie Sie HDInsight HBase-Cluster in Azure Virtual Network erstellen können."
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/22/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: fbd5c31a4ca1fa26d14001ba5765adfc91142a10
ms.lasthandoff: 04/27/2017


---
# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Erstellen von HBase-Clustern in Azure Virtual Network
Erfahren Sie, wie Sie Azure HDInsight HBase-Cluster in [Azure Virtual Network][1] erstellen können.

Mit der Integration in virtuelle Netzwerke können HBase-Cluster im selben virtuellen Netzwerk bereitgestellt werden wie Ihre Anwendungen, sodass Anwendungen direkt mit HBase kommunizieren können. Es ergeben sich folgende Vorteile:

* Direkte Konnektivität der Webanwendung mit den Knoten des HBase-Clusters, wodurch die Kommunikation über HBase Java-RPC-APIs (Remoteprozeduraufrufe) ermöglicht wird.
* Optimierte Leistung, da Datenverkehr nicht über mehrere Gateways und Lastenausgleichsmodule fließt.
* Möglichkeit einer höheren Sicherheit bei der Verarbeitung sensibler Informationen ohne exponierten öffentlichen Endpunkt.

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Eine Arbeitsstation mit Azure PowerShell**. Siehe [Install and use Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)(Installieren und Verwenden von Azure PowerShell, in englischer Sprache).

## <a name="create-hbase-cluster-into-virtual-network"></a>Erstellen von HBase-Clustern in einem virtuellen Netzwerk
In diesem Abschnitt erstellen Sie mit einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md) einen Linux-basierten HBase-Cluster mit dem abhängigen Azure Storage-Konto in einem virtuellen Azure-Netzwerk. Andere Methoden zur Erstellung von Clustern und Informationen zu den Einstellungen finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Weitere Informationen zur Verwendung einer Vorlage zum Erstellen von Hadoop-Clustern in HDInsight finden Sie unter [Erstellen Linux-basierter Hadoop-Cluster in HDInsight mithilfe von Azure Resource Manager-Vorlagen](hdinsight-hadoop-create-windows-clusters-arm-templates.md).

> [!NOTE]
> Einige Eigenschaften sind in der Vorlage hartcodiert. Beispiel:
>
> * **Standort:** USA, Osten 2
> * _Clusterversion: 3.4
> * **Anzahl von Workerknoten im Cluster**: 4
> * **Standard-Speicherkonto**: eine eindeutige Zeichenfolge
> * **Name des virtuellen Netzwerks**: &lt;Clustername>-vnet
> * **Adressraum des virtuellen Netzwerks**: 10.0.0.0/16
> * **Subnetzname**: subnet1
> * **Subnetzadressbereich**: 10.0.0.0/24
>
> &lt;Clustername> wird durch den Clusternamen ersetzt, den Sie bei Verwendung der Vorlage angeben.
>
>

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen: Die Vorlage finden Sie in den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Wechseln Sie vom Blatt **Benutzerdefinierte Bereitstellung** zu den folgenden Optionen:

   * **Abonnement**: Wählen Sie ein Azure-Abonnement, um den HDInsight-Cluster, das abhängige Speicherkonto und das virtuelle Azure-Netzwerk zu erstellen.
   * **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
   * **Standort**: Wählen Sie einen Standort für die Ressourcengruppe aus.
   * **Clustername**: Geben Sie einen Namen für den Hadoop-Cluster ein, der erstellt wird.
   * **Cluster-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **admin**.
   * **SSH-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet **sshuser**.  Sie können auch einen anderen Namen festlegen.
   * **Ich stimme den oben genannten Geschäftsbedingungen zu**: (Auswählen)
3. Klicken Sie auf **Kaufen**. Das Erstellen eines Clusters dauert ca. 20 Minuten. Wenn der Cluster erstellt wurde, öffnen Sie ihn, indem Sie im Portal auf das Clusterblatt klicken.

Löschen Sie den Cluster, wenn Sie das Tutorial beendet haben. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Anweisungen zum Löschen eines Clusters finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-management-portal.md#delete-clusters).

Führen Sie die Schritte unter [Erste Schritte mit HBase mit Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md)aus, um mit Ihrem neu erstellten HBase-Cluster zu arbeiten.

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Verbinden Sie sich mithilfe von HBase-Java-RPC-APIs mit dem HBase-Cluster.
1. Erstellen Sie einen virtuellen IaaS-Computer (Infrastructure-as-a-Service) im gleichen virtuellen Azure-Netzwerk und im gleichen Subnetz. Anweisungen zum Erstellen eines neuen virtuellen IaaS-Computers finden Sie im Abschnitt zum [Erstellen eines virtuellen Computers unter Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Beim Ausführen der in diesem Dokument beschriebenen Schritte muss die folgende Netzwerkkonfiguration verwendet werden:

   * **Virtuelles Netzwerk**: &lt;Clustername>-vnet
   * **Subnetz**: subnet1

   > [!IMPORTANT]
   > Ersetzen Sie &lt;Clustername> durch den Namen, den Sie beim Erstellen des HDInsight-Clusters in den vorherigen Schritten verwendet haben.
   >
   >

   Mit diesen Werten wird der virtuelle Computer im selben virtuellen Netzwerk und Subnetz platziert wie der HDInsight-Cluster. Diese Konfiguration ermöglicht eine direkte Kommunikation zwischen virtuellem Computer und Cluster. Es gibt eine Möglichkeit zum Erstellen eines HDInsight-Clusters mit einem leeren Edgeknoten. Der Edgeknoten kann zum Verwalten des Clusters verwendet werden.  Weitere Informationen finden Sie unter [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md)(Verwenden leerer Edgeknoten in HDInsight).

2. Für Remote-Verbindungen zwischen Java-Anwendungen und HBase müssen Sie den vollqualifizierten Domänennamen (FQDN) verwenden. Rufen Sie hierzu das verbindungsspezifische DNS-Suffix des HBase-Clusters ab. Dazu können Sie eine der folgenden Methoden verwenden:

   * Durchführen eines Ambari-Aufrufs mithilfe eines Webbrowsers:

     Wechseln Sie zu „https://&lt;Clustername>.azurehdinsight.net/api/v1/clusters/&lt;Clustername>/hosts?minimal_response=true“. Es wird eine JSON-Datei mit den DNS-Suffixen zurückgegeben.
   * Verwenden der Ambari-Website:

     1. Wechseln Sie zu „https://&lt;Clustername>.azurehdinsight.net“.
     2. Klicken Sie im Menü am oberen Rand auf **Hosts** .
   * Durchführen von REST-Aufrufen mithilfe von Curl:

         curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

     Suchen Sie in den zurückgegebenen JSON-Daten (JavaScript Object Notation) den Eintrag "host_name". Dieser Eintrag enthält den vollqualifizierten Domänennamen (FQDN) für die Knoten im Cluster. Beispiel:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     Der Teil des Domänennamens, der mit dem Clusternamen beginnt, ist das DNS-Suffix. Zum Beispiel: mycluster.b1.cloudapp.net.
   * Mithilfe von Azure PowerShell

     Verwenden Sie das folgende Azure PowerShell-Skript, um die **Get-ClusterDetail** -Funktion zu registrieren, mit der Sie das DNS-Suffix zurückgeben können:

         function Get-ClusterDetail(
             [String]
             [Parameter( Position=0, Mandatory=$true )]
             $ClusterDnsName,
             [String]
             [Parameter( Position=1, Mandatory=$true )]
             $Username,
             [String]
             [Parameter( Position=2, Mandatory=$true )]
             $Password,
             [String]
             [Parameter( Position=3, Mandatory=$true )]
             $PropertyName
             )
         {
         <#
             .SYNOPSIS
              Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
             .Description
              This command shows the following 4 properties of an HDInsight cluster:
              1. ZookeeperQuorum (supports only HBase type cluster)
                 Shows the value of HBase property "hbase.zookeeper.quorum".
              2. ZookeeperClientPort (supports only HBase type cluster)
                 Shows the value of HBase property "hbase.zookeeper.property.clientPort".
              3. HBaseRestServers (supports only HBase type cluster)
                 Shows a list of host FQDNs that run the HBase REST server.
              4. FQDNSuffix (supports all cluster types)
                 Shows the FQDN suffix of hosts in the cluster.
             .EXAMPLE
              Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
              This command shows the value of HBase property "hbase.zookeeper.quorum".
             .EXAMPLE
              Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
              This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
             .EXAMPLE
              Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
              This command shows a list of host FQDNs that run the HBase REST server.
             .EXAMPLE
              Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
              This command shows the FQDN suffix of hosts in the cluster.
         #>

             $DnsSuffix = ".azurehdinsight.net"

             $ClusterFQDN = $ClusterDnsName + $DnsSuffix
             $webclient = new-object System.Net.WebClient
             $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

             if($PropertyName -eq "ZookeeperQuorum")
             {
                 $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                 $Response = $webclient.DownloadString($Url)
                 $JsonObject = $Response | ConvertFrom-Json
                 Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
             }
             if($PropertyName -eq "ZookeeperClientPort")
             {
                 $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                 $Response = $webclient.DownloadString($Url)
                 $JsonObject = $Response | ConvertFrom-Json
                 Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
             }
             if($PropertyName -eq "HBaseRestServers")
             {
                 $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                 $Response1 = $webclient.DownloadString($Url1)
                 $JsonObject1 = $Response1 | ConvertFrom-Json
                 $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                 $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                 $Response2 = $webclient.DownloadString($Url2)
                 $JsonObject2 = $Response2 | ConvertFrom-Json
                 foreach ($host_component in $JsonObject2.host_components)
                 {
                     $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                     Write-host $ConnectionString
                 }
             }
             if($PropertyName -eq "FQDNSuffix")
             {
                 $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                 $Response = $webclient.DownloadString($Url)
                 $JsonObject = $Response | ConvertFrom-Json
                 $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                 $pos = $FQDN.IndexOf(".")
                 $Suffix = $FQDN.Substring($pos + 1)
                 Write-host $Suffix
             }
         }

     Verwenden Sie nach der Ausführung des Azure PowerShell-Skripts den folgenden Befehl, um das DNS-Suffix mithilfe der **Get-ClusterDetail** -Funktion zurückzugeben. Geben Sie den Namen Ihres HDInsight HBase-Clusters sowie den Admin-Namen und das Admin-Kennwort an, wenn Sie diesen Befehl ausführen.

         Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

     Daraufhin wird das DNS-Suffix zurückgegeben. Zum Beispiel: **yourclustername.b4.internal.cloudapp.net**.
   * Verwenden von RDP

     Sie können auch per Remotedesktop eine Verbindung mit dem HBase-Cluster herstellen (wobei Sie eine Verbindung mit dem Hauptknoten herstellen) und **ipconfig** in einer Eingabeaufforderung ausführen, um das DNS-Suffix abzurufen. Anweisungen zum Aktivieren des Remotedesktopprotokolls (RDP) und zum Herstellen einer Verbindung mit dem Cluster mithilfe des RDP finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal][hdinsight-admin-portal].

     ![hdinsight.hbase.dns.surffix][img-dns-surffix]

<!--
3.    Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Führen Sie den Befehl `ping headnode0.<dns suffix>` auf dem virtuellen Computer aus, um zu überprüfen, ob der virtuelle Computer mit dem HBase-Cluster kommunizieren kann. Beispiel: ping headnode0.mycluster.b1.cloudapp.net.

Führen Sie die unter [Verwenden von Maven zur Entwicklung von Java-Anwendungen, die HBase mit HDInsight (Hadoop) nutzen](hdinsight-hbase-build-java-maven.md) beschriebenen Schritte aus, um diese Informationen in einer Java-Anwendung zu verwenden. Wenn die Anwendung eine Verbindung mit einem HBase-Remoteserver herstellen soll, müssen Sie den FQDN für Zookeeper in der Datei **hbase-site.xml** eintragen. Beispiel:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> Weitere Informationen zur Namensauflösung in virtuellen Azure-Netzwerken und zur Verwendung eigener DNS-Server finden Sie unter [Namensauflösung (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
>
>

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie erfahren, wie Sie einen HBase-Cluster erstellen. Weitere Informationen finden Sie unter:

* [Erste Schritte mit HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Verwenden leerer Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md)
* [Konfigurieren der HBase-Replikation in HDInsight](hdinsight-hbase-replication.md)
* [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Erste Schritte mit HBase mit Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md)
* [Analysieren der Twitter-Stimmungen mit HBase in HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
* [Virtuelle Netzwerke im Überblick][vnet-overview]

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: /powershell/azureps-cmdlets-docs


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Bereitstellen von Details für den neuen HBase-Cluster"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Verwenden von Skriptaktionen zum Anpassen eines HBase-Clusters"

[azure-preview-portal]: https://portal.azure.com

