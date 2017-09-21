---
title: Verwenden von Interactive Hive mit Azure HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Interactive Hive (Hive LLAP) mit HDInsight verwenden.
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 6c4e3d0e97eb9ad4500d684c7da3b19c5669994f
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="use-interactive-hive-with-hdinsight-preview"></a>Verwenden von Interactive Hive mit HDInsight (Vorschau)
Interactive Hive (auch Hive LLAP oder [Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP) genannt) ist ein neuer Azure HDInsight-[Clustertyp](hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Interactive Hive unterstützt speicherinternes Caching, wodurch Hive-Abfragen schneller und viel interaktiver werden. 

Ein Interactive Hive-Cluster unterscheidet sich von einem Hadoop-Cluster. Er enthält nur den Hive-Dienst. 

> [!NOTE]
> MapReduce, Pig, Sqoop, Oozie und andere Dienste werden in Kürze aus diesem Clustertyp entfernt.
> Sie können im Interactive Hive-Cluster nur über die Ambari-Hive-Ansicht, Beeline und den Microsoft Hive Open Database Connectivity-Treiber (ODBC Hive) auf den Hive-Dienst zugreifen. Sie können nicht über die Hive-Konsole, Templeton, das Azure-Befehlszeilentool (Azure CLI) oder Azure PowerShell darauf zugreifen. 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>Erstellen eines Interactive Hive-Clusters
Interactive Hive-Cluster können Sie nur auf Linux-basierten Clustern verwenden. Informationen zum Erstellen von HDInsight-Clustern finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="execute-hive-queries-from-interactive-hive"></a>Ausführen von Hive-Abfragen über Interactive Hive
Um Hive-Abfragen auszuführen, können Sie folgende Optionen nutzen:

* Ausführen von Hive per Ambari-Hive-Ansicht.
  
    Informationen zum Verwenden der Hive-Ansicht finden Sie unter [Verwenden der Ambari-Hive-Ansicht mit Hadoop in HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).
* Ausführen von Hive per Beeline.
  
    Informationen zur Verwendung von Beeline in HDInsight finden Sie unter [Verwenden des Beeline-Clients mit Apache Hive](hdinsight-hadoop-use-hive-beeline.md).
  
    Sie können Beeline entweder über den Hauptknoten oder von einem leeren Edgeknoten aus verwenden. Am besten verwenden Sie Beeline von einem leeren Edgeknoten aus. Informationen zum Erstellen eines HDInsight-Clusters mit einem leeren Edgeknoten finden Sie unter [Verwenden leerer Edgeknoten in Hadoop-Clustern in HDInsight](hdinsight-apps-use-edge-node.md).
* Ausführen von Hive per Hive ODBC.
  
    Weitere Informationen zur Verwendung von Hive ODBC finden Sie unter [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](hdinsight-connect-excel-hive-odbc-driver.md).

So suchen Sie die Verbindungszeichenfolge für die Java Database Connectivity (JDBC):

1. Melden Sie sich mit folgender URL bei Ambari an: https://\<Clustername\>.AzureHDInsight.net.
2. Wählen Sie im Menü auf der linken Seite die Option **Hive** aus.
3. Wählen Sie das Symbol für die Zwischenablage zum Kopieren der URL aus:
   
   ![HDInsight Hadoop Interactive Hive LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie [Cluster in HDInsight mit Hadoop, Spark, Kafka usw. einrichten](hdinsight-hadoop-provision-linux-clusters.md).
* Erfahren Sie, wie Sie den [Beeline-Client mit Apache Hive verwenden](hdinsight-hadoop-use-hive-beeline.md).
* Erfahren Sie, wie Sie [Excel mithilfe des Microsoft Hive ODBC-Treibers mit Hadoop in Azure HDInsight verbinden](hdinsight-connect-excel-hive-odbc-driver.md).


