---
title: "Leitlinien für die Optimierung der Leistung von Azure Data Lake Store | Microsoft Docs"
description: "Leitlinien für die Optimierung der Leistung von Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/02/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: af11866fc812cd8a375557b7bf9df5cdc9bba610
ms.openlocfilehash: f0d0c05c08ce198e2702c76ad35b348107c664c7
ms.lasthandoff: 01/18/2017


---
# <a name="performance-tuning-guidance-for-azure-data-lake-store"></a>Anleitung für die Leistungsoptimierung von Azure Data Lake Store

Dieser Artikel enthält Hinweise zum Erreichen einer optimalen Leistung beim Schreiben von Daten in oder Lesen von Daten aus Azure Data Lake Store. In diesem Artikel sollen Benutzern die Parameter erläutert werden, die für häufig verwendete Tools zum Hoch- und Herunterladen von Daten und Datenanalyse-Workloads konfiguriert werden. Die Optimierung in diesem Leitfaden ist speziell auf ressourcenintensive Workloads ausgelegt, bei denen eine große Menge von Daten aus Data Lake Store gelesen bzw. in Data Lake Store geschrieben wird.

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake-Speicher](data-lake-store-get-started-portal.md)
* **Azure HDInsight-Cluster** mit Zugriff auf ein Data Lake-Speicherkonto. Informationen finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Stellen Sie sicher, dass Remotedesktop für den Cluster aktiviert ist.


## <a name="guidelines-for-data-ingestion-tools"></a>Leitlinien für Datenerfassungstools

Dieser Abschnitt bietet allgemeine Anleitungen zum Verbessern der Leistung beim Kopieren oder Verschieben von Daten in Data Lake Store. In diesem Abschnitt werden Faktoren, die die Leistung einschränken, und Möglichkeiten zum Beseitigen dieser Einschränkungen behandelt. Die folgenden Aspekte sollten Sie berücksichtigen.

* **Quelldaten**: Es gibt zahlreiche Einschränkungen, die aufgrund des Ursprungs der Quelldaten auftreten können. Der Durchsatz kann einen Engpass darstellen, wenn sich die Quelldaten auf langsamen Datenträgern oder in Remotespeicher mit geringer Durchsatzkapazität befinden. SSD-Datenträger, vorzugsweise für die lokale Festplatte, bieten aufgrund des höheren Durchsatzes die beste Leistung.

* **Netzwerk**: Wenn sich Ihre Quelldaten auf virtuellen Computern befinden, ist die Netzwerkverbindung zwischen dem virtuellen Computer und Data Lake Store wichtig. Verwenden Sie virtuelle Computer mit der größten verfügbaren Netzwerkkarte, um mehr Netzwerkbandbreite zu erhalten.

* **Regionsübergreifendes Kopieren**: Eine regionsübergreifende Datenein- und -ausgabe sorgt für eine hohe Netzwerklast, z.B. bei Ausführen eines Datenerfassungstools auf einer VM in der Region USA, Osten 2 zum Schreiben von Daten in ein Data Lake Store-Konto in der Region USA, Mitte. Durch regionsübergreifendes Kopieren von Daten kann die Leistung beeinträchtigt werden. Zum Maximieren des Netzwerkdurchsatzes empfehlen wir das Arbeiten mit Datenerfassungsaufträgen auf VMs in der Region, in der sich auch das Data Lake Store-Zielkonto befindet.                                                                                                                                        

* **Cluster**: Wenn Sie Datenerfassungsaufträge mithilfe eines HDInsight-Clusters (wie z.B. für DistCp) ausführen, empfehlen wir den Einsatz von VMs der D-Serie für den Cluster, da diese mehr Arbeitsspeicher bieten. Eine größere Anzahl von Kernen hilft auch beim Erhöhen des Durchsatzes.                                                                                                                                                                                                                                                                                                            

* **Parallelität von Threads**: Wenn Sie einen HDInsight-Cluster zum Kopieren von Daten aus einem Speichercontainer nutzen, gelten Einschränkungen für die Anzahl von Threads, die basierend der Cluster- und Containergröße sowie den Threadeinstellungen parallel verwendet werden können. Eine der wichtigsten Methoden zum Erzielen einer besseren Data Lake Store-Leistung ist das Erhöhen der Parallelität. Optimieren Sie Ihre Einstellungen so, dass Sie zum Erreichen eines höheren Durchsatzes ein Höchstmaß an Parallelität erreichen. In der folgenden Tabelle sind die Einstellungen für jede Erfassungsmethode beschrieben, die konfiguriert werden kann, um mehr Parallelität zu erreichen. Folgen Sie den Links in der Tabelle zu Artikeln zur Verwendung des Tools zum Erfassen von Daten in Data Lake Store und zum Optimieren der Leistung des Tools zur Maximierung des Durchsatzes.

    | Tool               | Einstellung für Parallelität                                                                |
    |--------------------|------------------------------------------------------------------------------------|
    | [PowerShell](data-lake-store-get-started-powershell.md)       | PerFileThreadCount, ConcurrentFileCount |
    | [AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)    | Azure Data Lake Analytics-Einheiten         |
    | [DistCp](data-lake-store-copy-data-wasb-distcp.md)            | -m (Mapper)                             |
    | [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)| parallelCopies                          |
    | [Sqoop](data-lake-store-data-transfer-sql-sqoop.md)           | fs.azure.block.size, -m (Mapper)        |


## <a name="guidelines-while-working-with-hdinsight-workloads"></a>Leitlinien zum Arbeiten mit HDInsight-Workloads

Beim Arbeiten mit Daten in Data Lake Store zum Ausführen von Analysen empfehlen wir das Verwenden von HDInsight 3.5-Clustern, um Data Lake Store optimal zu nutzen. Wenn Ihr Auftrag E/A-intensiver ist, können zur Verbesserung der Leistung bestimmte Parameter konfiguriert werden. Wenn beispielsweise der Auftrag hauptsächlich aus Lese- oder Schreibvorgängen besteht, kann die Leistung durch Erhöhen der Parallelität für die Eingabe in und Ausgabe aus Azure Data Lake Store gesteigert werden.

Azure Data Lake Store arbeitet am besten, je höher die Parallelität ist. Es gibt einige allgemeine Möglichkeiten, die Parallelität für E/A-intensive Aufträge zu erhöhen.

1. **Ausführen einer größere Anzahl von YARN-Computecontainern anstelle einer kleinen Anzahl großer YARN-Container**: Das Vorhandensein zahlreicher Container erhöht die Parallelität für Ein- und Ausgabevorgänge, wodurch die Möglichkeiten von Data Lake Store besser genutzt werden.

    Angenommen, Ihr HDInsight-Cluster enthält zwei D3v2-Knoten. Jeder D3v2-Knoten hat 12 GB YARN-Arbeitsspeicher, sodass zwei D3v2-VMs 24 GB YARN-Arbeitsspeicher haben. Lassen Sie uns ferner annehmen, dass Sie die YARN-Containergröße auf 6 GB festgelegt haben. Dies bedeutet, dass Sie über vier Container mit je 6 GB verfügen. Deshalb können vier Aufgaben parallel ausgeführt werden. Um die Parallelität zu erhöhen, können Sie die Größe der Container auf 3 GB reduzieren, wodurch Sie acht Container mit 3 GB erhalten. Dies ermöglicht Ihnen, acht Aufgaben parallel auszuführen. Nachstehend sehen Sie eine Abbildung.

    ![Data Lake Store-Leistung](./media/data-lake-store-performance-tuning-guidance/image-1.png)

    Eine häufige Frage ist, warum ich die Containergröße nicht auf 1 GB Arbeitsspeicher reduziere, damit ich 24 Container erhalte, um die Parallelität noch weiter steigern zu können. Das hängt davon ab, ob die Aufgabe 3 GB Arbeitsspeicher benötigt oder ob 1 GB ausreicht.  Sie können einen einfachen Vorgang im Container ausführen, der nur 1 GB braucht, oder einen komplexen Vorgang, der ggf. 3 GB benötigt.  Wenn Sie die Größe des Containers zu stark verringern, erhalten Sie möglicherweise einen Speicherausnahmefehler.  In diesem Fall sollten Sie die Containergröße erhöhen.  Neben dem Arbeitsspeicher kann die Anzahl der virtuellen Kerne die Parallelität beeinflussen.

    ![Data Lake Store-Leistung](./media/data-lake-store-performance-tuning-guidance/image-2.png)

2. **Vergrößern des Arbeitsspeichers in Ihrem Cluster für mehr Parallelität**: Sie können den Arbeitsspeicher im Cluster erhöhen, indem Sie die Größe Ihres Clusters erhöhen oder einen VM-Typ mit mehr Arbeitsspeicher wählen. Dadurch wird die Menge des verfügbaren YARN-Arbeitsspeichers vergrößert, sodass Sie mehr Container erstellen können, was die Parallelität steigert.  

    Nehmen wir z.B. an, dass Sie über einen einzelnen D3v2-Knoten in Ihrem HDInsight-Cluster verfügen, der 12 GB YARN-Arbeitsspeicher und 3-GB-Container hat.  Sie skalieren Ihren Cluster auf zwei D3v2-Knoten, wodurch sich der YARN-Arbeitsspeicher auf 24 GB erhöht.  Dies erhöht die Parallelität von 4 auf 8.

    ![Data Lake Store-Leistung](./media/data-lake-store-performance-tuning-guidance/image-3.png)

3. **Starten mit dem Anpassen der Anzahl der Aufgaben an den Grad der Parallelität**: Mittlerweile haben Sie die Containergröße bereits entsprechend festgelegt, um einen maximalen Grad an Parallelität zu erzielen. Sie müssen nun die Anzahl der Aufgaben so festlegen, dass alle diese Container genutzt werden. Es gibt unterschiedliche Namen für Aufgaben in jeder Workload.

    Sie sollten auch die Größe Ihres Auftrags berücksichtigen. Wenn der Auftrag groß ist, muss jede Aufgabe ggf. eine große Datenmenge verarbeiten. Es empfiehlt sich u.U., mit mehr Aufgaben zu arbeiten, damit jede Aufgabe nicht zu viele Daten verarbeiten muss.

    Angenommen, Sie haben sechs Container. Als Ausgangspunkt sollten wir die Anzahl unserer Aufgaben auf 6 festlegen. Sie können mit einer höheren Anzahl von Aufgaben experimentieren, um festzustellen, ob sich die Leistung verbessert. Durch Festlegen einer größeren Anzahl von Aufgaben erhöht sich die Parallelität nicht. Wenn wir die Anzahl der Aufgaben auf mehr als sechs festlegen, wird die Aufgabe erst im nächsten Durchgang ausgeführt. Wenn wir die Anzahl der Aufgaben auf weniger als sechs festlegen, wird die Parallelität nicht vollständig ausgenutzt.

    Für jede Workload gibt es mehrere Parameter zum Festlegen der Anzahl von Aufgaben. In der folgenden Tabelle sind einige davon aufgelistet.

    | Workload               | Parameter zum Festlegen von Aufgaben                                                         |
    |--------------------|------------------------------------------------------------------------------------|
    | [Spark in HDInsight](data-lake-store-performance-tuning-spark.md)       | <ul><li>Num-executors</li><li>Executor-memory</li><li>Executor-cores</li></ul> |
    | [Hive in HDInsight](data-lake-store-performance-tuning-hive.md)    | hive.tez.container.size         |
    | [MapReduce in HDInsight](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>Mapreduce.map.memory</li><li>Mapreduce.job.maps</li><li>Mapreduce.reduce.memory</li><li>Mapreduce.job.reduces</li></ul> |
    | [Storm in HDInsight](data-lake-store-performance-tuning-storm.md)| <ul><li>Anzahl von Workerprozessen</li><li>Anzahl von Spout Executor-Instanzen</li><li>Anzahl von Bolt Executor-Instanzen </li><li>Anzahl von Spout-Aufgaben</li><li>Anzahl von Bolt-Aufgaben</li></ul>|

## <a name="see-also"></a>Siehe auch
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
* [Erste Schritte mit Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

