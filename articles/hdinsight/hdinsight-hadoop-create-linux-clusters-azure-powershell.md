---
title: Erstellen von Azure HDInsight (Hadoop)-Clustern mithilfe von PowerShell | Microsoft-Dokumentation
description: "Erfahren Sie, wie Hadoop-, HBase-, Storm- oder Spark-Cluster unter Linux für HDInsight mithilfe von Azure PowerShell erstellt werden."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 44e418e52fc18dd22820331f7d921e789da62832
ms.lasthandoff: 03/25/2017


---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Erstellen von Linux-basierten Clustern in HDInsight mit Azure PowerShell
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell ist eine leistungsstarke Skriptumgebung, mit der Sie die Bereitstellung und Verwaltung Ihrer Workloads in Microsoft Azure steuern und automatisieren können. Dieses Dokument enthält Informationen zum Erstellen eines Linux-basierten HDInsight-Clusters mit Azure PowerShell. Darüber hinaus ist ein Beispielskript enthalten.

> [!NOTE]
> Azure PowerShell ist nur auf Windows-Clients verfügbar. Wenn Sie einen Linux-, Unix- oder Mac OS X-Client verwenden, finden Sie im Artikel zum [Erstellen eines Linux-basierten HDInsight-Clusters mit der Azure-Befehlszeilenschnittstelle](hdinsight-hadoop-create-linux-clusters-azure-cli.md) Informationen zum Erstellen eines Clusters mit der Azure-CLI.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes, bevor Sie mit diesem Verfahren beginnen können:

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Azure PowerShell.
    Weitere Informationen zum Verwenden von Azure PowerShell mit HDInsight finden Sie unter [Verwalten von HDInsight mit PowerShell](hdinsight-administer-use-powershell.md). Eine Liste der HDInsight Windows PowerShell-Cmdlets finden Sie unter [HDInsight-Cmdlet-Referenz](https://msdn.microsoft.com/library/azure/dn858087.aspx).
  
    > [!IMPORTANT]
    > Die Azure PowerShell-Unterstützung zum Verwalten von HDInsight-Ressourcen mithilfe von Azure Service Manager ist **veraltet** und wurde zum 1. Januar 2017 eingestellt. Die Schritte in diesem Dokument zeigen die neuen HDInsight-Cmdlets, die mit Azure Resource Manager genutzt werden können.
    > 
    > Führen Sie zur Installation der neuesten Version von Azure PowerShell die unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) beschriebenen Schritte aus. Wenn Sie über Skripts verfügen, die für die Verwendung der neuen Cmdlets für Azure Resource Manager geändert werden müssen, finden Sie unter [Migrieren zu Azure Resource Manager-basierten Entwicklungstools für HDInsight-Cluster](hdinsight-hadoop-development-using-azure-resource-manager.md) weitere Informationen.
    > 
    > 

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Erstellen von Clustern
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Für die Erstellung eines HDInsight-Clusters mithilfe von Azure PowerShell müssen die folgenden Prozeduren ausgeführt werden:

* Erstellen einer Azure-Ressourcengruppe
* Erstellen eines Azure-Speicherkontos
* Erstellen eines Azure-Blobcontainers
* Erstellen eines HDInsight-Clusters

Die beiden wichtigsten Parameter, die für die Erstellung von Linux-Clustern festgelegt werden müssen, sind diejenigen zum Angeben des Betriebssystemtyps und der Details zum SSH-Benutzer:

* Stellen Sie sicher, dass Sie den Parameter **-OSType** als **Linux** angeben.
* Um SSH für Remotesitzungen mit dem Cluster zu verwenden, können Sie das SSH-Benutzerkennwort oder den öffentlichen SSH-Schlüssel angeben. Wenn Sie sowohl das SSH-Benutzerkennwort und als auch den öffentlichen SSH-Schlüssel angeben, wird der Schlüssel ignoriert. Wenn Sie den SSH-Schlüssel für Remotesitzungen verwenden möchten, müssen Sie bei der Aufforderung zur Eingabe ein leeres SSH-Kennwort angeben. Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

Das folgende Skript veranschaulicht das Erstellen eines neuen Clusters:

    $token ="<SpecifyAnUniqueString>"
    $subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Die Werte, die Sie für **$clusterCredentials** angeben, werden verwendet, um das Hadoop-Benutzerkonto für den Cluster zu erstellen. Verwenden Sie dieses Konto für die Verbindung mit dem Cluster.

Die Werte, die Sie für **$sshCredentials** angeben, werden verwendet, um den SSH-Benutzer für den Cluster zu erstellen. Verwenden Sie dieses Konto zum Starten einer SSH-Remotesitzung mit dem Cluster und zum Ausführen von Aufträgen.

> [!IMPORTANT]
> In diesem Skript müssen Sie die Anzahl der Workerknoten im Cluster angeben. Wenn Sie die Verwendung von mehr als 32 Workerknoten planen (entweder bei Erstellung des Clusters oder durch eine Skalierung des Clusters nach der Erstellung), müssen Sie auch eine Hauptknotengröße von mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) angeben.
> 
> Weitere Informationen zu Knotengrößen und den damit verbundenen Kosten finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).
> 
> 

Das Erstellen eines Clusters kann bis zu 20 Minuten dauern.

Das folgende Beispiel veranschaulicht, wie Sie ein zusätzliches Speicherkonto hinzufügen:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Anpassen von Clustern
* Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Löschen des Clusters
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie einen HDInsight-Cluster erfolgreich erstellt haben, nutzen Sie die folgenden Ressourcen, um zu erfahren, wie Sie mit Ihrem Cluster arbeiten.

### <a name="hadoop-clusters"></a>Hadoop-Cluster
* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-Cluster
* [Erste Schritte mit HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Entwickeln von Java-Anwendungen für HBase in HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-Cluster
* [Entwickeln von Java-Topologien für Storm in HDInsight](hdinsight-storm-develop-java-topology.md)
* [Verwenden von Python-Komponenten in Storm in HDInsight](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit Storm in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-Cluster
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeit-Streaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)


