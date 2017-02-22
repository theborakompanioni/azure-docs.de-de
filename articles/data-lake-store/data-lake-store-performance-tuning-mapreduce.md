---
title: "Leitfaden für die Optimierung der Leistung von Azure Data Lake Store für MapReduce | Microsoft-Dokumentation"
description: "Leitfaden für die Optimierung der Leistung von Azure Data Lake Store für MapReduce"
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
ms.sourcegitcommit: c145642c06e477dd47e4d8d651262046519b656b
ms.openlocfilehash: 564141d09bc54fbf4beb36d28bec160a7097f897


---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a>Anleitung für die Leistungsoptimierung für MapReduce in HDInsight und Azure Data Lake Store


## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake-Speicher](data-lake-store-get-started-portal.md)
* **Azure HDInsight-Cluster** mit Zugriff auf ein Data Lake-Speicherkonto. Informationen finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Stellen Sie sicher, dass Remotedesktop für den Cluster aktiviert ist.
* **Verwendung von MapReduce in HDInsight**.  Weitere Informationen finden Sie unter [Verwenden von MapReduce mit Hadoop in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce).  
* **Leitfaden für die Optimierung der Leistung von Azure Data Lake Store**.  Allgemeine Leistungskonzepte finden Sie unter [Anleitung für die Leistungsoptimierung von Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

## <a name="parameters"></a>Parameter

Im Folgenden finden Sie die wichtigsten Parameter, die Sie für die Ausführung von MapReduce konfigurieren können, um die Leistung in Azure Data Lake Store zu verbessern:

* **Mapreduce.map.memory.mb** – die Menge an Arbeitsspeicher, die jedem Mapper zugeordnet werden soll
* **Mapreduce.job.maps** – die Anzahl an Zuordnungstasks pro Auftrag
* **Mapreduce.reduce.memory.mb** – die Menge an Arbeitsspeicher, die jedem Reducer zugeordnet werden soll
* **Mapreduce.job.reduces** – die Anzahl von Reduzierungstasks pro Auftrag

**Mapreduce.map.memory / Mapreduce.reduce.memory**: Diese Zahl muss angepasst werden, je nachdem, wie viel Arbeitsspeicher für den Zuordnungs- oder Reduzierungstask erforderlich ist.  Die Standardwerte von „mapreduce.map.memory“ und „mapreduce.reduce.memory“ können in Ambari über die YARN-Konfiguration angezeigt werden.  Navigieren Sie in Ambari zu YARN, und zeigen Sie die Registerkarte für die Konfiguration an.  Der Arbeitsspeicher wird angezeigt.     

**Mapreduce.job.maps / Mapreduce.job.reduces**: Diese Einstellung legt die maximale Anzahl von Mappern oder Reducern fest, die erstellt werden.  Die Anzahl der Teilungen legt fest, wie viele Mapper für den MapReduce-Auftrag erstellt werden.  Daher erhalten Sie möglicherweise weniger Mapper als angefordert, wenn weniger Teilungen als angeforderte Mapper vorhanden sind.       

## <a name="guidance"></a>Anleitungen

**Schritt 1: Ermitteln der Anzahl der ausgeführten Aufträge**: Standardmäßig verwendet MapReduce den gesamten Cluster für Ihren Auftrag.  Sie können einen kleineren Teil des Clusters verwenden, indem Sie weniger Mapper verwenden als Container verfügbar sind.  Bei den Anleitungen in diesem Dokument wird davon ausgegangen, dass Ihre Anwendung die einzige Anwendung ist, die in Ihrem Cluster ausgeführt wird.      

**Schritt 2: Festlegen von „mapreduce.map.memory/mapreduce.reduce.memory“**: Die Größe des Arbeitsspeichers für Zuordnungs- und Reduzierungstasks richtet sich nach dem jeweiligen Auftrag.  Sie können die Arbeitsspeichergröße reduzieren, wenn Sie die Parallelität erhöhen möchten.  Die Anzahl von gleichzeitig ausgeführten Tasks richtet sich nach der Anzahl von Containern.  Durch Verringern der Arbeitsspeichermenge pro Mapper oder Reducer können mehr Container erstellt werden, wodurch mehr Mapper oder Reducer gleichzeitig ausgeführt werden können.  Wenn die Arbeitsspeichermenge zu stark verringert wird, steht für einige Prozesse möglicherweise nicht genügend Arbeitsspeicher zur Verfügung.  Wenn Sie beim Ausführen Ihres Auftrags einen Heapfehler erhalten, sollten Sie den Arbeitsspeicher pro Mapper oder Reducer erhöhen.  Denken Sie daran, dass beim Hinzufügen weiterer Container auch zusätzlicher Overhead für jeden zusätzlichen Container entsteht, wodurch die Leistung beeinträchtigt werden kann.  Eine andere Möglichkeit besteht darin, mehr Arbeitsspeicher zur Verfügung zu stellen, indem Sie einen Cluster verwenden, der über mehr Arbeitsspeicher verfügt, oder indem Sie die Anzahl von Knoten in Ihrem Cluster erhöhen.  Eine größere Menge an Arbeitsspeicher ermöglicht die Verwendung einer größeren Anzahl von Containern, was wiederum mehr Parallelität bedeutet.  

**Schritt 3: Ermitteln des gesamten YARN-Arbeitsspeichers**: Beim Optimieren von „mapreduce.job.maps/mapreduce.job.reduces“ müssen Sie die Menge an YARN-Arbeitsspeicher berücksichtigen, die zur Verwendung verfügbar ist.  Diese Informationen sind in Ambari verfügbar.  Navigieren Sie zu YARN, und zeigen Sie die Registerkarte für die Konfiguration an.  Die Größe des YARN-Arbeitsspeichers wird in diesem Fenster angezeigt.  Um den YARN-Gesamtarbeitsspeicher zu erhalten, müssen Sie den YARN-Arbeitsspeicher pro Knoten mit der Anzahl von Knoten in Ihrem Cluster multiplizieren.

    Total YARN memory = nodes * YARN memory per node
Wenn Sie einen leeren Cluster verwenden, kann der Arbeitsspeicher der gesamte YARN-Arbeitsspeicher für den Cluster sein.  Wenn andere Anwendungen auch Arbeitsspeicher belegen, können Sie nur einen Teil des Clusterarbeitsspeichers nutzen, indem Sie die Anzahl von Mappern oder Reducern auf die Anzahl von Containern verringern, die Sie verwenden möchten.  

**Schritt 4: Berechnen der Anzahl von YARN-Containern**: Die Anzahl von YARN-Containern bestimmt, wie viel Parallelität für den Auftrag verfügbar ist.  Teilen Sie den YARN-Gesamtarbeitsspeicher durch „mapreduce.map.memory“.  

    # of YARN containers = total YARN memory / mapreduce.map.memory
    
Sie müssen mindestens so viele Mapper und Reducer verwenden, wie YARN-Container vorhanden sind, um eine optimale Parallelität zu erzielen.  Sie können weiter experimentieren, indem Sie die Anzahl von Mappern und Reducern erhöhen, um zu ermitteln, ob sich die Leistung verbessert.  Denken Sie daran, dass eine größere Anzahl von Mappern zu weiterem Overhead führt und zu viele Mapper daher die Leistung beeinträchtigen können.  

Hinweis: CPU-Planung und -Isolierung sind standardmäßig deaktiviert, sodass die Anzahl von YARN-Containern durch den Arbeitsspeicher begrenzt ist.

## <a name="example-calculation"></a>Beispielberechnung

Angenommen, Sie haben einen Cluster mit 8 D14-Knoten und möchten einen E/A-intensiven Auftrag ausführen.  Dann müssen Sie folgende Berechnungen anstellen:

**Schritt 1: Ermitteln der Anzahl von ausgeführten Aufträgen**: Für das Beispiel nehmen wir an, dass Ihr Auftrag der einzige ausgeführte Auftrag ist.  

**Schritt 2: Festlegen von „mapreduce.map.memory/mapreduce.reduce.memory“**: In diesem Beispiel führen Sie einen E/A-intensiven Auftrag aus und entscheiden, dass 3 GB Arbeitsspeicher für Zuordnungstasks ausreichend sind.

    mapreduce.map.memory = 3GB
**Schritt 3: Ermitteln des YARN-Arbeitsspeichers insgesamt** 

    total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**Schritt 4: Berechnen der Anzahl von YARN-Containern**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

## <a name="limitations"></a>Einschränkungen

**Drosselung bei Azure Data Lake Store** 

Azure Data Lake Store ist ein mehrinstanzenfähiger Dienst und legt daher Bandbreitengrenzwerte auf Kontoebene fest.  Wenn Sie diese Grenzwerte erreichen, werden Taskfehler angezeigt. Dies lässt sich durch Beobachten der Drosselungsfehler in den Taskprotokollen ermitteln.  Wenn Sie für einen Auftrag mehr Bandbreite benötigen, wenden Sie sich an uns.   

Um zu prüfen, ob eine Drosselung vorliegt, müssen Sie die Debugprotokollierung auf Clientseite aktivieren. Gehen Sie hierzu wie folgt vor:

1. Fügen Sie die folgende Eigenschaft in die log4j-Eigenschaften unter „Ambari > YARN > Config > Advanced yarn-log4j“ ein: log4j.logger.com.microsoft.azure.datalake.store=DEBUG.

2. Starten Sie alle Knoten und Dienste neu, damit die Konfiguration wirksam wird.

3. Wenn eine Drosselung vorliegt, wird in der YARN-Protokolldatei ein HTTP 429-Fehlercode angezeigt. Die YARN-Protokolldatei befindet sich hier: /tmp/&lt;Benutzer&gt;/yarn.log

## <a name="examples-to-run"></a>Beispiele für die Ausführung

Um zu zeigen, wie MapReduce in Azure Data Lake Store ausgeführt wird, finden Sie hier einen Beispielcode, der mit folgenden Einstellungen in einem Cluster ausgeführt wurde:

* D14v2 mit&16; Knoten
* Hadoop-Cluster mit HDI 3.5

Im Folgenden finden Sie einige Beispielbefehle zum Ausführen von MapReduce Teragen, Terasort und Teravalidate.  Sie können diese Befehle basierend auf Ihren Ressourcen anpassen.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=2048 -Dmapred.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=2048 -Dmapred.map.memory.mb=3072 -Dmapred.reduce.tasks=512 -Dmapred.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=512 -Dmapred.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate



<!--HONumber=Jan17_HO2-->


