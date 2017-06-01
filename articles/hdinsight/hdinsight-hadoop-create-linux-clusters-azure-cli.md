---
title: Erstellen von Azure HDInsight (Hadoop)-Clustern mithilfe der Befehlszeile | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie HDInsight-Cluster mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle 1.0 erstellen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: ccb2c827aa95ea967d740860ed17e6cc7bd3b392
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Erstellen von HDInsight-Clustern mit der Azure-Befehlszeilenschnittstelle

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Anhand der Schritte in diesem Dokument werden Sie durch die Erstellung eines HDInsight 3.5-Clusters mithilfe von Azure CLI 1.0 geführt.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdi-version-33-nearing-retirement-date).


## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure-Befehlszeilenschnittstelle**. Die Schritte in diesem Dokument wurden mit der neuesten Version der Azure-CLI (0.10.1) getestet.

    > [!IMPORTANT]
    > Die Schritte in diesem Dokument gelten nicht für Azure CLI 2.0. Die Erstellung eines HDInsight-Clusters wird in Azure CLI 2.0 nicht unterstützt.

## <a name="log-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement an.

Führen Sie die Schritte aus, die unter [Herstellen einer Verbindung mit einem Azure-Abonnement von der Azure Befehlszeilenschnittstelle (Azure-CLI)](../xplat-cli-connect.md) dokumentiert sind, und stellen Sie über die **login** -Methode eine Verbindung mit Ihrem Abonnement her.

## <a name="create-a-cluster"></a>Erstellen eines Clusters

Die folgenden Schritte müssen in einer Befehlszeilen-, Shell- oder Terminalsitzung nach der Installation und Konfiguration der Azure-CLI erfolgen.

1. Führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Abonnement zu authentifizieren:

        azure login

    Sie werden aufgefordert, Ihren Namen und das Kennwort anzugeben. Wenn Sie über mehrere Azure-Abonnements verfügen, verwenden Sie `azure account set <subscriptionname>` zum Festlegen des Abonnements, das von den Befehlen der Azure-CLI genutzt werden soll.

2. Wechseln Sie mit dem folgenden Befehl in den Azure-Ressourcen-Manager-Modus:

        azure config mode arm

3. Erstellen Sie eine Ressourcengruppe. Diese Ressourcengruppe umfasst den HDInsight-Cluster und das zugeordnete Speicherkonto.

        azure group create groupname location

    * Ersetzen Sie **groupname** durch einen eindeutigen Gruppenamen.

    * Ersetzen Sie **location** durch die geografische Region, in der die Gruppe erstellt werden soll.

       Eine Liste der gültigen Speicherorte können Sie mithilfe des Befehls `azure location list` erzeugen. Verwenden Sie anschließend einen der Speicherorte aus der Spalte **Name**.

4. Erstellen Sie ein Speicherkonto. Dieses Speicherkonto wird als Standardspeicher für den HDInsight-Cluster verwendet.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Ersetzen Sie **groupname** durch den Namen der im vorherigen Schritt erstellten Gruppe:

    * Ersetzen Sie **location** durch den gleichen Speicherort, den Sie im vorherigen Schritt verwendet haben.

    * Ersetzen Sie **storagename** durch einen eindeutigen Namen für das Speicherkonto.

        > [!NOTE]
        > Verwenden Sie `azure storage account create -h`, um sich in der Hilfe zu diesem Befehl weitere Informationen zu den verwendeten Parametern anzeigen zu lassen.

5. Rufen Sie den Schlüssel ab, der für den Zugriff auf das Speicherkonto verwendet wird.

        azure storage account keys list -g groupname storagename

    * Ersetzen Sie **groupname** durch den Namen der Ressourcengruppe.
    * Ersetzen Sie **storagename** durch den Namen des Speicherkontos.

     Speichern Sie von den zurückgegebenen Daten den **key**-Wert für **key1**.

6. Erstellen Sie ein HDInsight-Cluster.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Ersetzen Sie **groupname** durch den Namen der Ressourcengruppe.

    * Ersetzen Sie **Hadoop** durch den zu erstellenden Clustertyp. Beispielsweise Hadoop, HBase, Storm oder Spark.

     > [!IMPORTANT]
     > HDInsight-Cluster gibt es in verschiedenen Typen, die der Workload oder Technologie entsprechen, für die der Cluster optimiert ist. Es ist keine unterstützte Methode zum Erstellen eines Clusters vorhanden, bei der mehrere Typen kombiniert werden, z.B. Storm und HBase in einem Cluster.

    * Ersetzen Sie **location** durch den gleichen Speicherort, den Sie in den vorherigen Schritten verwendet haben.

    * Ersetzen Sie **storagename** durch den Namen des Speicherkontos.

    * Ersetzen Sie **storagekey** durch den Schlüssel, den Sie im vorherigen Schritt abgerufen haben.

    * Verwenden Sie für den `--defaultStorageContainer` -Parameter den gleichen Namen wie für den Cluster.

    * Ersetzen Sie **admin** und **httppassword** durch den Namen und das Kennwort, die Sie beim Zugriff auf den Cluster über HTTPS verwenden möchten.

    * Ersetzen Sie **sshuser** und **sshuserpassword** durch den Benutzernamen und das Kennwort, die Sie beim Zugriff auf den Cluster über SSH verwenden möchten.

    > [!IMPORTANT]
    > In diesem Beispiel wird ein Cluster mit zwei Workerknoten erstellt. Wenn Sie mehr als 32 Workerknoten planen (beim Erstellen oder durch Skalieren des Clusters), müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen. Sie können die Hauptknotengröße mit dem `--headNodeSize`-Parameter festlegen.
    >
    > Weitere Informationen zu Knotengrößen und damit verbundenen Kosten finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

    Die Fertigstellung der Clustererstellung kann möglicherweise einige Minuten dauern. In der Regel dauert es etwa 15 Minuten.

## <a name="troubleshoot"></a>Problembehandlung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](hdinsight-administer-use-portal-linux.md#create-clusters) an.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen HDInsight-Cluster erfolgreich mithilfe der Azure-Befehlszeilenschnittstelle erstellt haben, nutzen Sie die folgenden Informationen, um mehr über die Arbeit mit Ihrem Cluster zu lernen:

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

