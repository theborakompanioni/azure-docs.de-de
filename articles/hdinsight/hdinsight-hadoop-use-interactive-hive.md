---
title: Verwenden von Interactive Hive in HDInsight | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Interactive Hive (Hive unter LLAP) in HDInsight verwenden.
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 6971e123b388eab689e02a5c67809509c2ad29c6


---
# <a name="use-interactive-hive-in-hdinsight-preview"></a>Verwenden von Interactive Hive in HDInsight (Vorschau)
Interactive Hive (auch als [Live Long and Process](https://cwiki.apache.org/confluence/display/Hive/LLAP) bezeichnet) ist ein neuer HDInsight-[Clustertyp](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interactive Hive ermöglicht speicherinternes Caching, um zu erreichen, dass Hive-Abfragen deutlich interaktiver und schneller werden. Dank diesem neuen Feature wird HDInsight zu einer der weltweit leistungsstärksten und flexibelsten offenen Big Data-Lösungen in der Cloud, indem speicherinternes Caching (per Hive und Spark) und erweiterte Analysefunktionen mit tiefer Integration in R Services eingesetzt werden. 

Der Interactive Hive-Cluster unterscheidet sich vom Hadoop-Cluster. Er enthält nur den Hive-Dienst. 

> [!NOTE]
> MapReduce, Pig, Sqoop, Oozie und andere Dienste werden in Kürze aus diesem Clustertyp entfernt.
> Der Hive-Dienst im Interactive Hive-Cluster ist nur über die Ambari-Hive-Ansicht, Beeline und Hive ODBC zugänglich. Über die Hive-Konsole, Templeton, Azure CLI und Azure PowerShell kann darauf nicht zugegriffen werden. 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>Erstellen eines Interactive Hive-Clusters
Interactive Hive-Cluster werden nur in Linux-basierten Clustern unterstützt. Informationen zum Erstellen von HDInsight-Clustern finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="execute-hive-from-interactive-hive"></a>Ausführen von Hive über Interactive Hive
Es gibt verschiedene Möglichkeiten zum Ausführen von Hive-Abfragen:

* Ausführen von Hive per Ambari-Hive-Ansicht
  
    Informationen zur Verwendung der Hive-Ansicht finden Sie unter [Verwenden der Hive-Ansicht mit Hadoop in HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).
* Ausführen von Hive per Beeline
  
    Informationen zur Verwendung von Beeline in HDInsight finden Sie unter [Verwenden von Hive mit Hadoop in HDInsight über Beeline](hdinsight-hadoop-use-hive-beeline.md).
  
    Sie verwenden Beeline entweder über den Hauptknoten oder einen leeren Edgeknoten.  Es wird empfohlen, Beeline über einen leeren Edgeknoten zu nutzen.  Informationen zum Erstellen eines HDInsight-Clusters mit einem leeren Edgeknoten finden Sie unter [Verwenden leerer Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md).
* Ausführen von Hive per Hive ODBC
  
    Weitere Informationen zur Verwendung von Hive ODBC finden Sie unter [Verbinden von Excel über den Microsoft Hive ODBC-Treiber mit Hadoop](hdinsight-connect-excel-hive-odbc-driver.md).

**Gehen Sie wie folgt vor, um nach der JDBC-Verbindungszeichenfolge zu suchen:**

1. Melden Sie sich an Ambari an, indem Sie die folgende URL verwenden: https://<ClusterName>.AzureHDInsight.net.
2. Klicken Sie im linken Menü auf **Hive** .
3. Klicken Sie auf das hervorgehobene Symbol, um die URL zu kopieren:
   
   ![HDInsight Hadoop Interactive Hive LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Weitere Informationen
* [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Es wird beschrieben, wie Sie Interactive Hive-Cluster in HDInsight erstellen.
* [Verwenden von Hive mit Hadoop in HDInsight über Beeline](hdinsight-hadoop-use-hive-beeline.md): Es wird beschrieben, wie Sie Beeline zum Senden von Hive-Abfragen verwenden.
* [Verbinden von Excel über den Microsoft Hive ODBC-Treiber mit Hadoop](hdinsight-connect-excel-hive-odbc-driver.md): Es wird beschrieben, wie Sie für Excel eine Verbindung mit Hive herstellen.




<!--HONumber=Nov16_HO3-->


