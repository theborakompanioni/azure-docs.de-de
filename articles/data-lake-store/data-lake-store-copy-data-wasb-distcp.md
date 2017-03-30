---
title: Kopieren von Daten in und aus WASB in Data Lake Store mit Distcp | Microsoft Docs
description: "Verwenden des Distcp-Tools zum Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher und umgekehrt"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 1e66ce0af4d3d33b584f9f3a881c34aa04e783b7
ms.lasthandoff: 03/07/2017


---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Kopieren von Daten zwischen Azure Storage-Blobs und Data Lake-Speicher mithilfe von Distcp
> [!div class="op_single_selector"]
> * [Verwenden von DistCp](data-lake-store-copy-data-wasb-distcp.md)
> * [Verwenden von AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Nachdem Sie einen HDInsight-Cluster erstellt haben, der Zugriff auf ein Data Lake Store-Konto besitzt, können Sie Hadoop-Ökosystemtools wie Distcp zum Kopieren von Daten **aus** einem HDInsight-Clusterspeicher (WASB) in ein Data Lake Store-Konto (und umgekehrt) verwenden. Dieser Artikel enthält Anweisungen zur Vorgehensweise.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake-Speicher](data-lake-store-get-started-portal.md)
* **Azure HDInsight-Cluster** mit Zugriff auf ein Data Lake-Speicherkonto. Informationen finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Stellen Sie sicher, dass Remotedesktop für den Cluster aktiviert ist.

## <a name="do-you-learn-fast-with-videos"></a>Lernen Sie schnell mithilfe von Videos?
[Sehen Sie sich dieses Video](https://mix.office.com/watch/1liuojvdx6sie) zum Kopieren von Daten zwischen Azure Storage-Blobs und Data Lake-Speicher mithilfe von DistCp an.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Verwenden von Distcp eines HDInsight-Linux-Clusters

Ein HDInsight-Cluster enthält das Distcp-Dienstprogramm, das zum Kopieren von Daten aus verschiedenen Quellen in einen HDInsight-Cluster verwendet werden kann. Wenn Sie den HDInsight-Cluster so konfiguriert haben, dass er den Data Lake-Speicher als zusätzlichen Speicher verwendet, kann das Distcp-Dienstprogramm ebenfalls direkt zum Kopieren von Daten in ein bzw. aus einem Data Lake-Speicherkonto genutzt werden. In diesem Abschnitt wird die Verwendung des Distcp-Dienstprogramms erläutert.

1. Verwenden Sie SSH auf Ihrem Desktop, um eine Verbindung mit dem Cluster herzustellen. Informationen hierzu finden Sie unter [Verbinden mit einem Linux-basierten HDInsight-Cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect). Führen Sie die Befehle von der SSH-Eingabeaufforderung aus.

2. Überprüfen Sie, ob Sie auf die Azure Storage-Blobs (WASB) zugreifen können. Führen Sie den folgenden Befehl aus:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Dadurch sollte eine Liste der Inhalte im Speicherblob angezeigt werden.
3. Überprüfen Sie zudem, ob Sie vom Cluster auf das Data Lake-Speicherkonto zugreifen können. Führen Sie den folgenden Befehl aus:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Dadurch sollte eine Liste der Dateien bzw. Ordner im Data Lake-Speicherkonto bereitgestellt werden.
4. Verwenden Sie Distcp zum Kopieren von Daten aus WASB in ein Data Lake-Speicherkonto.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Dadurch wird der Inhalt des Ordners **/example/data/gutenberg/** in WASB in den Ordner **/myfolder** im Data Lake Store-Konto kopiert.
5. Verwenden Sie Distcp ebenso zum Kopieren von Daten aus einem Data Lake-Speicherkonto in WASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Dadurch wird der Inhalt des Ordners **/myfolder** im Data Lake Store-Konto in den Ordner **/example/data/gutenberg/** in WASB kopiert.

## <a name="performance-considerations-while-using-distcp"></a>Überlegungen zur Leistung bei der Verwendung von DistCp

Da die kleinste Granularitätseinheit von DistCp eine einzelne Datei ist, ist die maximale Anzahl gleichzeitiger Kopien der wichtigste Parameter für die Data Lake Store-Optimierung. Um diesen Parameter zu steuern, wird der Parameter für die Anzahl von Zuordnungen (m) über die Befehlszeile festgelegt. Dieser Parameter gibt die maximale Anzahl von Zuordnungen an, die zum Kopieren von Daten verwendet werden. Der Standardwert ist 20.

**Beispiel**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Wie bestimme ich die Anzahl der zu verwendenden Zuordnungen?

Hier sind einige hilfreiche Informationen zur Vorgehensweise angegeben.

* **Schritt 1: Bestimmen des YARN-Gesamtarbeitsspeichers** – Zunächst muss der YARN-Arbeitsspeicher bestimmt werden, der für den Cluster zur Verfügung steht, in dem Sie den DistCp-Auftrag ausführen. Diese Information finden Sie im dem Cluster zugeordneten Ambari-Portal. Navigieren Sie zu YARN, und zeigen Sie die Konfigurationsregisterkarte an, um den YARN-Arbeitsspeicher zu ermitteln. Um den YARN-Gesamtarbeitsspeicher zu erhalten, multiplizieren Sie den YARN-Arbeitsspeicher pro Knoten mit der Anzahl von Knoten in Ihrem Cluster.

* **Schritt 2: Berechnen der Anzahl von Zuordnungen** – Der Wert von **m** entspricht dem Quotienten des YARN-Gesamtarbeitsspeichers dividiert durch die YARN-Containergröße. Die YARN-Containergröße finden Sie ebenfalls im Ambari-Portal. Navigieren Sie zu YARN, zeigen Sie die Konfigurationsregisterkarte an. Die YARN-Containergröße wird in diesem Fenster angezeigt. Die Formel zur Berechnung der Anzahl von Zuordnungen (**m**) sieht wie folgt aus:

        m = (number of nodes * YARN memory for each node) / YARN container size

**Beispiel**

Angenommen, Sie verfügen über einen Cluster mit vier D14v2-Knoten und möchten 10 TB an Daten aus zehn unterschiedlichen Ordnern übertragen. Jeder Ordner enthält unterschiedliche Datenmengen, und auch die Dateigrößen in den einzelnen Ordnern sind unterschiedlich.

* YARN-Gesamtarbeitsspeicher: Über das Ambari-Portal ermitteln Sie, dass der YARN-Arbeitsspeicher für einen D14-Knoten 96 GB beträgt. Für einen Cluster mit vier Knoten beträgt der YARN-Gesamtarbeitsspeicher demnach: 

        YARN memory = 4 * 96GB = 384GB

* Anzahl von Zuordnungen: Über das Ambari-Portal ermitteln Sie, dass die YARN-Containergröße für einen D14-Clusterknoten 3072 beträgt. Die Anzahl von Zuordnungen beträgt somit:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Wenn andere Anwendungen Arbeitsspeicher verwenden, können Sie festlegen, dass für DistCp nur ein Teil des YARN-Arbeitsspeichers Ihres Clusters verwendet werden soll.

### <a name="copying-large-datasets"></a>Kopieren umfangreicher Datasets

Wenn das zu verschiebende Dataset sehr groß ist (beispielsweise > 1 TB) oder Sie über zahlreiche unterschiedliche Ordner verfügen, empfiehlt sich unter Umständen die Verwendung mehrerer DistCp-Aufträge. Dadurch ergibt sich zwar wahrscheinlich kein Leistungsgewinn, die Aufträge werden jedoch verteilt, sodass im Falle eines Fehlers bei einem Auftrag nicht das gesamte Projekt, sondern lediglich der betroffene Auftrag neu gestartet werden muss.

### <a name="limitations"></a>Einschränkungen

* Zur Optimierung der Leistung versucht DistCp, Zuordnungen mit ähnlicher Größe zu erstellen. Eine höhere Anzahl von Zuordnungen führt nicht unbedingt zu mehr Leistung.

* DistCp ist auf eine einzelne Zuordnung pro Datei beschränkt. Aus diesem Grund sollte die Anzahl von Zuordnungen die Anzahl von Dateien nicht überschreiten. Der Umstand, dass DistCp einer Datei nur eine einzelne Zuordnung zuweisen kann, beschränkt den Umfang der Parallelität, die beim Kopieren großer Dateien verwendet werden kann.

* Wenn Sie über eine geringe Anzahl großer Dateien verfügen, sollten Sie diese in Dateiblöcke mit jeweils 256 MB aufteilen, um das Parallelitätspotenzial zu erhöhen. 
 
* Beim Kopieren aus einem Azure Blob Storage-Konto wird Ihr Kopierauftrag unter Umständen auf der Blob Storage-Seite gedrosselt. Dies beeinträchtigt die Leistung Ihres Kopierauftrags. Weitere Informationen zu den Einschränkungen von Azure Blob Storage finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md) unter „Speichergrenzwerte“.

## <a name="see-also"></a>Siehe auch
* [Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher](data-lake-store-copy-data-azure-storage-blob.md)
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)

