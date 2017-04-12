---
title: Debuggen und Analysieren von Hadoop-Diensten mit Heapdumps | Microsoft Docs
description: "Lassen Sie Heapdumps für Hadoop-Dienste automatisch sammeln und zum Debuggen und Analysieren in das Azure-Blobspeicherkonto einfügen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e4ec4ebb-fd32-4668-8382-f956581485c4
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jgao
ROBOTS: NOINDEX
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: 937f4735f2babf6582787ed860e79d8e09ea3b7e
ms.lasthandoff: 04/11/2017


---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Sammeln von Heapdumps im Blobspeicher zum Debuggen und Analysieren von Hadoop-Diensten
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heapdumps enthalten eine Momentaufnahme des Speichers der Anwendung, einschließlich der Werte von Variablen zum Zeitpunkt der Dumperstellung. Daher sind sie sehr nützlich zum Diagnostizieren von Problemen, die bei der Ausführung auftreten. Heapdumps können für Hadoop-Dienste automatisch gesammelt und im Azure-Blobspeicherkonto eines Benutzers unter "HDInsightHeapDumps/" abgelegt werden.

Die Sammlung von Heapdumps für verschiedene Dienste muss für Dienste in einzelnen Clustern aktiviert werden. Standardmäßig ist dieses Feature für einen Cluster deaktiviert. Diese Heapdumps können groß sein, sodass es ratsam ist, das Blobspeicherkonto zu überwachen, in dem sie gespeichert werden, nachdem die Sammlung aktiviert wurde.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date). Die Informationen in diesem Artikel gelten nur für Windows-basierte HDInsight-Cluster. Informationen zu Linux-basierten HDInsight-Clustern finden Sie unter [Aktivieren von Heapdumps für Hadoop-Dienste auf Linux-basierten HDInsight-Clustern](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Verfügbare Dienste für Heapdumps
Sie können Heapdumps für die folgenden Dienste aktivieren:

* **hcatalog** : tempelton
* **hive** : hiveserver2, metastore, derbyserver
* **mapreduce** : jobhistoryserver
* **yarn** : resourcemanager, nodemanager, timelineserver
* **hdfs** : datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Die Konfigurationselemente, mit denen Heapdumps aktiviert werden
Um Heapdumps für einen Dienst zu aktivieren, müssen Sie die entsprechenden Konfigurationselemente im Abschnitt des jeweiligen Diensts festlegen, der von **service_name** angegeben wird.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Der Wert von **service_name** kann einer der oben angegebenen Dienste sein: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode oder namenode.

## <a name="enable-using-azure-powershell"></a>Aktivieren mithilfe von Azure PowerShell
Um Heapdumps mit Azure PowerShell z. B. für den Dienst "jobhistoryserver" zu aktivieren, müssen Sie die folgenden Schritte ausführen:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Aktivieren mithilfe von .NET SDK
Um Heapdumps mit dem .NET SDK z. B. für "jobhistoryserver" zu aktivieren, müssen Sie die folgenden Schritte ausführen:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

