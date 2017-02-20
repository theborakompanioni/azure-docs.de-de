---
title: "Leitfaden für die Optimierung der Leistung von Azure Data Lake Store für Hive | Microsoft-Dokumentation"
description: "Leitfaden für die Optimierung der Leistung von Azure Data Lake Store für Hive"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
translationtype: Human Translation
ms.sourcegitcommit: ab688288c7ae0d6bebdff5156fad6f7f5e9b4224
ms.openlocfilehash: e10bf8f7cbae2b81d22823ff74fe652c6bcb2da3


---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a>Anleitung für die Leistungsoptimierung für Hive in HDInsight und Azure Data Lake Store

Die Standardeinstellungen bieten eine gute Leistung für viele verschiedene Anwendungsfälle.  Für E/A-intensive Abfragen kann Hive optimiert werden, um eine bessere Leistung mit Azure Data Lake Store zu bieten.  

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake-Speicher](data-lake-store-get-started-portal.md)
* **Azure HDInsight-Cluster** mit Zugriff auf ein Data Lake-Speicherkonto. Informationen finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Stellen Sie sicher, dass Remotedesktop für den Cluster aktiviert ist.
* **Ausführung von Hive in HDInsight**.  Informationen zum Ausführen von Hive-Aufträgen in HDInsight finden Sie unter [Verwenden von Hive in HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive).
* **Leitfaden für die Optimierung der Leistung von Azure Data Lake Store**.  Allgemeine Leistungskonzepte finden Sie unter [Anleitung für die Leistungsoptimierung von Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).

## <a name="parameters"></a>Parameter

Im Folgenden finden Sie die wichtigsten Einstellungen für die Optimierung der Azure Data Lake Store-Leistung:

* **hive.tez.container.size** – die von jedem Task verwendete Arbeitsspeichermenge

* **tez.grouping.min-size** – die Mindestgröße jedes Mappers

* **tez.grouping.max-size** – die maximale Größe jedes Mappers

* **hive.exec.reducer.bytes.per.reducer** – die Größe jedes Reducers

**hive.tez.container.size**: Die Containergröße bestimmt die Menge an Arbeitsspeicher, die für jeden Task verfügbar ist.  Dies ist die wichtigste Information für die Steuerung der Parallelität in Hive.  

**tez.grouping.min-size**: Dieser Parameter ermöglicht es Ihnen, die Mindestgröße jedes Mappers festzulegen.  Wenn die Anzahl von Mappern, die Tez auswählt, kleiner ist als der Wert dieses Parameters, verwendet Tez den hier festgelegten Wert.  

**tez.grouping.max-size**: Dieser Parameter ermöglicht es Ihnen, die maximale Größe jedes Mappers festzulegen.  Wenn die Anzahl von Mappern, die Tez auswählt, größer ist als der Wert dieses Parameters, verwendet Tez den hier festgelegten Wert.  

**hive.exec.reducer.bytes.per.reducer**: Dieser Parameter legt die Größe jedes Reducers fest.  Standardmäßig ist jeder Reducer 256 MB groß.  

## <a name="guidance"></a>Anleitungen

**Set hive.exec.reducer.bytes.per.reducer**: Der Standardwert funktioniert gut, wenn die Daten nicht komprimiert sind.  Bei komprimierten Daten sollten Sie den Reducer verkleinern.  

**Set hive.tez.container.size**: Der Arbeitsspeicher wird in jedem Knoten von „yarn.nodemanager.resource.memory-mb“ angegeben und sollte im HDI-Cluster standardmäßig richtig festgelegt sein.  Weitere Informationen zum Festlegen des geeigneten Speichers in YARN finden Sie in diesem [Beitrag](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

E/A-intensive Workloads können von einem höheren Maß an Parallelität profitieren, indem Sie die Größe der Tez-Container reduzieren. So können Benutzer mehr Container verwenden, wodurch die Parallelität verbessert wird.  Einige Hive-Abfragen erfordern sehr viel Arbeitsspeicher (z.B. MapJoin).  Wenn der Task nicht über genügend Arbeitsspeicher verfügt, wird zur Laufzeit eine Ausnahme wegen unzureichenden Arbeitsspeichers angezeigt.  Wenn Sie eine solche Ausnahme erhalten, sollten Sie den Arbeitsspeicher vergrößern.   

Die Anzahl von gleichzeitigen Tasks bzw. die Parallelität wird durch die Gesamtmenge an YARN-Arbeitsspeicher begrenzt.  Die Anzahl von YARN-Containern bestimmt, wie viele gleichzeitige Tasks ausgeführt werden können.  Die Menge an YARN-Arbeitsspeicher pro Knoten finden Sie bei Ambari.  Navigieren Sie zu YARN, und zeigen Sie die Registerkarte für die Konfiguration an.  Die Größe des YARN-Arbeitsspeichers wird in diesem Fenster angezeigt.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
Entscheidend für die Verbesserung der Leistung bei Azure Data Lake Store ist es, die Parallelität so weit wie möglich zu erhöhen.  Tez berechnet automatisch die Anzahl von Tasks, die erstellt werden müssen – darum müssen Sie sich nicht kümmern.   

## <a name="example-calculation"></a>Beispielberechnung

Angenommen, Sie haben einen D14-Cluster mit 8 Knoten.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Einschränkungen
**Drosselung bei Azure Data Lake Store** 

Wenn Sie die Bandbreitengrenzwerte von Azure Data Lake Store erreichen, werden Taskfehler angezeigt. Dies lässt sich durch Beobachten der Drosselungsfehler in den Taskprotokollen ermitteln.  Sie können die Parallelität verringern, indem Sie die Tez-Containergröße erhöhen.  Wenn Sie für einen Auftrag mehr Parallelität benötigen, wenden Sie sich an uns.   

Um zu prüfen, ob eine Drosselung vorliegt, müssen Sie die Debugprotokollierung auf Clientseite aktivieren. Gehen Sie hierzu wie folgt vor:

1. Fügen Sie folgende Eigenschaft in die log4j-Eigenschaften in der Hive-Konfigurationsdatei ein. Dies kann über die Ambari-Ansicht erfolgen: log4j.logger.com.microsoft.azure.datalake.store=DEBUG. Starten Sie alle Knoten und Dienste neu, damit die Konfiguration wirksam wird.

2. Wenn eine Drosselung vorliegt, wird in der Hive-Protokolldatei ein HTTP 429-Fehlercode angezeigt. Die Hive-Protokolldatei befindet sich hier: /tmp/&lt;Benutzer&gt;/hive.log.

## <a name="further-information-on-hive-tuning"></a>Weitere Informationen zur Hive-Optimierung

Diese Blogs können Sie bei der Optimierung Ihrer Hive-Abfragen unterstützen:
* [Optimize Hive queries for Hadoop in HDInsight](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/) (Optimieren von Hive-Abfragen für Hadoop in HDInsight)
* [Troubleshooting Hive query performance](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) (Behandlung von Problemen mit der Hive-Abfrageleistung)
* [Ignite talk on optimize Hive on HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25) (Diskussion über die Optimierung von Hive in HDInsight)



<!--HONumber=Feb17_HO1-->


