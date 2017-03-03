---
title: "Verfügbarkeit von Hadoop-Clustern in HDInsight | Microsoft-Dokumentation"
description: "HDInsight stellt hochverfügbare und zuverlässige Cluster mit einem zusätzlichen Hauptknoten bereit."
services: hdinsight
tags: azure-portal
editor: cgronlun
manager: jhubbard
author: mumian
documentationcenter: 
ms.assetid: ccab792d-60d6-4287-96c2-479e5d0cf358
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: a2b32f23381ed1f9912edf6432f029e51bdf1be4
ms.openlocfilehash: dd28e295df7acead773f9076d790e0e96b66adb9
ms.lasthandoff: 02/07/2017


---
# <a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Verfügbarkeit und Zuverlässigkeit von Windows-basierten Hadoop-Clustern in HDInsight
> [!IMPORTANT]
> Die Schritte in diesem Dokument gelten speziell für Windows-basierte HDInsight-Cluster. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date). Wenn Sie einen Linux-basierten Cluster verwenden, finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Hadoop-Clustern in HDInsight](hdinsight-high-availability-linux.md) entsprechende Informationen für Linux.
>
>

Mit HDInsight können Kunden verschiedene Clustertypen für unterschiedliche Datenanalyseworkloads bereitstellen. Gegenwärtig angebotene Clustertypen sind Hadoop-Cluster für Abfrage- und Analyseworkloads, HBase-Cluster für NoSQL-Workloads und Storm-Cluster für Workloads der Echtzeit-Ereignisverarbeitung. Innerhalb eines bestimmten Clustertyps gibt es unterschiedliche Rollen für die verschiedenen Knoten. Beispiel:

* Hadoop-Cluster für HDInsight werden mit zwei Rollen bereitgestellt:

  * Hauptknoten (2 Knoten)
  * Datenknoten (mindestens 1 Knoten)
* HBase-Cluster für HDInsight werden mit drei Rollen bereitgestellt:

  * Hauptserver (2 Knoten)
  * Regionsserver (mindestens 1 Knoten)
  * Master/Zookeeper-Knoten (3 Knoten)
* Storm-Cluster für HDInsight werden mit drei Rollen bereitgestellt:

  * Nimbus-Knoten (2 Knoten)
  * Supervisor-Server (mindestens 1 Knoten)
  * Zookeeper-Knoten (3 Knoten)

Standardimplementierungen von Hadoop-Clustern verfügen in der Regel nur über einen Hauptknoten. HDInsight entfernt diesen „Single Point of Failure“ durch das Hinzufügen eines zweiten Hauptknotens/Hauptservers/Nimbus-Knotens, um die Verfügbarkeit bzw. Zuverlässigkeit des Diensts zu erhöhen, die zum Verwalten von Workloads erforderlich ist. Diese Hauptknoten/Hauptserver/Nimbus-Knoten dienen zur reibungslosen Verwaltung ausgefallener Workerknoten. Bei einem Ausfall eines Master-Diensts auf dem Hauptknoten würde der Cluster jedoch ausfallen.

[ZooKeeper](http://zookeeper.apache.org/)-Knoten (ZKs) wurden hinzugefügt, führen eine sogenannte Leader Election der Hauptknoten durch und teilen Workerknoten und Gateways (GWs) mit, wann diese auf den sekundären Hauptknoten (Head Node1) umschalten müssen, wenn der aktive Hauptknoten (Node0) inaktiv wird.

![Diagramm der äußerst zuverlässigen Hauptknoten in der HDInsight Hadoop-Implementierung.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)

## <a name="check-active-head-node-service-status"></a>Überprüfen des Dienststatus für den aktiven Hauptknoten
Um festzustellen, welcher Hauptknoten aktiv ist und den Status der dort laufenden Dienste zu prüfen, müssen Sie sich unter Verwendung des Remotedesktopprotokolls (RDP) mit dem Hadoop-Cluster verbinden. Anweisungen zu RDP finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Sobald Sie eine Verbindung mit dem Cluster hergestellt haben, doppelklicken Sie auf das Symbol **Hadoop-Dienst verfügbar** auf dem Desktop, um zu erfahren, auf welchem Hauptknoten die Dienste Namenode, Jobtracker, Templeton, Oozieservice, Metastore und Hiveserver2 bzw. im Fall von HDI 3.0 die Dienste Namenode, Resource Manager, History Server, Templeton, Oozieservice, Metastore und Hiveserver2 ausgeführt werden.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Im Screenshot ist *headnode0*der aktive Hauptknoten.

## <a name="access-log-files-on-the-secondary-head-node"></a>Zugreifen auf Protokolldateien auf dem sekundären Hauptknoten
Wenn der sekundäre Hauptknoten der aktive Hauptknoten ist, können Sie für den Zugriff auf Auftragsprotokolle auf dem sekundären Hauptknoten weiterhin die JobTracker-Benutzeroberfläche verwenden, ebenso wie Sie dies für den aktiven primären Knoten tun würden. Um auf JobTracker zuzugreifen, müssen Sie, wie im vorigen Abschnitt beschrieben, unter Verwendung von RDP eine Verbindung mit dem Hadoop-Cluster herstellen. Sobald Sie die Verbindung mit dem Cluster über RDP hergestellt haben, doppelklicken Sie auf dem Desktop auf das Symbol **Hadoop-Namensknotenstatus**. Klicken Sie anschließend auf die **NameNode-Protokolle**, um das Protokollverzeichnis auf dem sekundären Hauptknoten zu öffnen.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)

## <a name="configure-head-node-size"></a>Konfigurieren der Größe des Hauptknotens
Hauptknoten werden standardmäßig als große VMs eingerichtet. Diese Größe ist angemessen für die Verwaltung der meisten im Cluster ausgeführten Hadoop-Jobs. In manchen Szenarien werden jedoch sehr große virtuelle Computer für die Hauptknoten benötigt. Dies ist z. B. dann der Fall, wenn das Cluster eine große Anzahl kleiner Oozie-Jobs verwalten muss.

Sehr große VMs können entweder mit Azure PowerShell-Cmdlets oder mit dem HDInsight SDK konfiguriert werden.

Die Erstellung und Bereitstellung eines Clusters mithilfe von Azure PowerShell ist unter [Verwalten von HDInsight mit PowerShell](hdinsight-administer-use-powershell.md)dokumentiert. Um einen sehr großen Hauptknoten zu konfigurieren, müssen Sie den `-HeadNodeVMSize ExtraLarge`-Parameter zu dem in diesem Code verwendeten `New-AzureRmHDInsightcluster`-Cmdlet hinzufügen.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName `
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Das Verfahren für das SDK ist ähnlich. Die Erstellung und Bereitstellung eines Clusters mithilfe des SDK ist unter [Verwenden von HDInsight mit dem .NET SDK](hdinsight-provision-clusters.md)dokumentiert. Um einen sehr großen Hauptknoten zu konfigurieren, müssen Sie den `HeadNodeSize = NodeVMSize.ExtraLarge`-Parameter der in diesem Code verwendeten `ClusterCreateParameters()`-Methode hinzufügen.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Nächste Schritte
* [Apache ZooKeeper](http://zookeeper.apache.org/)
* [Herstellen einer Verbindung zu HDInsight-Clustern mit RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* [Verwenden des HDInsight .NET SDK](hdinsight-provision-clusters.md)

