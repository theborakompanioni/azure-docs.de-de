---
title: Datenszenarien in Zusammenhang mit Data Lake Store | Microsoft Docs
description: "Kennen lernen der verschiedenen Szenarien und Tools, mit denen Daten, die in einem Data Lake-Speicher gespeichert sind, erfasst, verarbeitet, heruntergeladen und visualisiert werden können"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 37409a71-a563-4bb7-bc46-2cbd426a2ece
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: a1349e53774732ffbfacefc7bd8dcc493f78fbd6
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Verwenden von Azure Data Lake-Speicher für Big Data-Anforderungen
Es gibt vier wichtige Phasen in der Big Data-Verarbeitung:

* Erfassung von großen Datenmengen in einem Datenspeicher, in Echtzeit oder in Batches
* Verarbeiten der Daten
* Herunterladen der Daten
* Visualisieren der Daten

In diesem Artikel betrachten wir diese Phasen in Bezug auf den Azure Data Lake-Speicher, um die Optionen und Tools kennen zu lernen, die zur Erfüllung Ihrer Big Data-Anforderungen verfügbar sind.

## <a name="ingest-data-into-data-lake-store"></a>Erfassen von Daten im Data Lake-Speicher
In diesem Abschnitt werden die unterschiedlichen Quellen von Daten hervorgehoben, sowie die verschiedenen Methoden, mit denen Daten in einem Data Lake-Speicherkonto erfasst werden können.

![Erfassen von Daten in Data Lake Store](./media/data-lake-store-data-scenarios/ingest-data.png "Erfassen von Daten in Data Lake Store")

### <a name="ad-hoc-data"></a>Ad-hoc-Daten:
Dies steht für kleinere Datasets, die zum Erstellen von Prototypen einer Big Data-Anwendung verwendet werden. Es gibt, abhängig von der Quelle der Daten, verschiedene Möglichkeiten zum Erfassen von Ad-hoc-Daten.

| Datenquelle | Erfassen Sie mit |
| --- | --- |
| Lokalem Computer |<ul> <li>[Azure-Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Plattformübergreifende Azure-CLI](data-lake-store-get-started-cli.md)</li> <li>[Verwenden von Data Lake-Tools für Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Azure Storage-Blob |<ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)</li> <li>[AdlCopy-Tool](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Ausführung von DistCp auf einem HDInsight-Cluster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Streamingdaten
Diese Daten können von verschiedenen Quellen wie Anwendungen, Geräten, Sensoren etc. generiert werden. Diese Daten können in einem Data Lake-Speicher von verschiedenen Tools erfasst werden. Diese Tools erfassen und verarbeiten die Daten in der Regel Ereignis für Ereignis in Echtzeit und schreiben dann die Ereignisse stapelweise in den Data Lake-Speicher, damit sie weiterverarbeitet werden können.

Folgende Tools können Sie verwenden:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md): In Event Hubs erfasste Ereignisse können mithilfe einer Azure Data Lake Store-Ausgabe in Azure Data Lake geschrieben werden.
* [Azure HDInsight Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) : Daten aus dem Storm-Cluster können direkt in Data Lake Store geschrieben werden.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md): Sie können Ereignisse von Event Hubs empfangen und dann mit dem [Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md) in Data Lake Store schreiben.

### <a name="relational-data"></a>Relationale Daten
Sie können auch Daten aus relationalen Datenbanken erfassen. Relationale Datenbanken sammeln über einen Zeitraum hinweg umfangreiche Datenmengen, die bei Verarbeitung durch eine Big Data-Pipeline wichtige Einblicke bieten können. Mit den folgenden Tools können Sie solche Daten in den Data Lake-Speicher verschieben.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Webserverprotokolldaten (mit benutzerdefinierten Anwendungen hochladen)
Auf diesen Datasettyp wird besonders hingewiesen, da die Analyse von Webserverprotokolldaten eine gängige Praxis für Big Data-Anwendungen ist und voraussetzt, dass zahlreiche Protokolldateien in den Data Lake-Speicher hochgeladen werden. Sie können jedes der folgenden Tools nutzen, um eigene Skripts oder Anwendungen zum Hochladen solcher Daten zu schreiben.

* [Plattformübergreifende Azure-CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [.NET SDK für den Azure Data Lake-Speicher](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Für das Hochladen von Webserverprotokolldaten und auch anderer Arten von Daten (z. B. soziale Stellungnahmen) ist es ein guter Ansatz, wenn Sie Ihre eigenen benutzerdefinierten Skripts/Anwendungen schreiben, denn dies gibt Ihnen die Flexibilität, Ihre Datenhochladekomponente als Teil in Ihre Big Data-Anwendung einzubeziehen. In einigen Fällen kann dieser Code die Form eines Skripts oder eines einfachen Befehlszeilenprogramms annehmen. In anderen Fällen kann der Code verwendet werden, die Big Data-Verarbeitung in eine Geschäftsanwendung oder -lösung zu integrieren.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Mit Azure HDInsight-Clustern verknüpfte Daten 
Die meisten HDInsight-Clustertypen (Hadoop, HBase, Storm) unterstützen den Data Lake-Speicher als Datenspeicherrepository. HDInsight-Cluster greifen aus Azure Storage-Blobs (WASB) auf Daten zu. Zur Verbesserung der Leistung können Sie die Daten aus dem WASB in ein mit dem Cluster verknüpftes Data Lake-Speicherkonto kopieren. Mit den folgenden Tools können Sie die Daten kopieren.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy-Dienst](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>In lokalen oder IaaS Hadoop-Clustern gespeicherte Daten
Große Datenmengen können in vorhandenen Hadoop-Clustern oder lokal auf Computern gespeichert werden, die HDFS verwenden. Die Hadoop-Cluster können sich in einer lokalen Bereitstellung oder innerhalb eines IaaS-Clusters in Azure befinden. Solche Daten müssen möglicherweise einmalig oder regelmäßig in den Azure Data Lake-Speicher kopiert werden. Hierfür stehen verschiedene Möglichkeiten zur Verfügung. Im Folgenden finden Sie eine Liste der Alternativen und die jeweiligen Vor- und Nachteile.

| Vorgehensweise | Details | Vorteile | Überlegungen |
| --- | --- | --- | --- |
| Verwenden Sie Azure Data Factory (ADF), um Daten direkt aus Hadoop-Clustern in den Azure Data Lake-Speicher zu kopieren. |[ADF unterstützt HDFS als Datenquelle.](../data-factory/data-factory-hdfs-connector.md) |ADF bietet sofortige Unterstützung für HDFS und erstklassige End-to-End-Verwaltung und -Überwachung. |Ein Datenverwaltungsgateway muss lokal oder im IaaS-Cluster bereitgestellt werden. |
| Exportieren Sie Daten in Form von Dateien aus Hadoop. Kopieren Sie die Dateien dann mit einem geeigneten Mechanismus in den Azure Data Lake-Speicher. |Sie können Dateien mit folgenden Verfahren in Azure Data Lake Store kopieren:  <ul><li>[Azure PowerShell für Windows-Betriebssystem](data-lake-store-get-started-powershell.md)</li><li>[Plattformübergreifende Azure-CLI für andere Betriebssysteme als Windows](data-lake-store-get-started-cli.md)</li><li>Benutzerdefinierte, beliebiges Data Lake Store-SDK verwendende App</li></ul> |Lässt sich schnell einrichten. Benutzerdefinierte Uploads sind möglich. |Der Prozess erfordert mehrere Schritte und verschiedene Technologien. Da die Tools benutzerdefiniert sind, werden Verwaltung und Überwachung im Lauf der Zeit schwierig. |
| Verwenden Sie Distcp, um Daten von Hadoop in Azure Storage zu kopieren. Kopieren Sie die Daten dann mit einem geeigneten Mechanismus von Azure Storage in den Azure Data Lake-Speicher. |Sie können Daten mit folgenden Verfahren von Azure Storage in Data Lake Store kopieren:  <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy-Tool](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Ausführung von Apache DistCp auf HDInsight-Clustern](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Sie können Open Source-Tools verwenden. |Der Prozess erfordert mehrere Schritte und verschiedene Technologien. |

### <a name="really-large-datasets"></a>Sehr große Datasets
Das Hochladen von Datasets im Bereich mehrerer Terabyte kann mithilfe der oben beschriebenen Methoden manchmal langsam und kostspielig sein. In solchen Fällen können Sie die folgenden Optionen verwenden:

* **Verwenden von Azure ExpressRoute**. Azure ExpressRoute ermöglicht Ihnen, private Verbindungen zwischen Azure-Rechenzentren und Ihrer lokalen Infrastruktur zu erstellen. Dies ist eine zuverlässige Option zur Übertragung großer Datenmengen. Weitere Informationen finden Sie in der [Dokumentation zu Azure ExpressRoute](../expressroute/expressroute-introduction.md).
* **Offline-Datenupload**. Wenn Azure ExpressRoute aus irgendeinem Grund nicht verwendet werden kann, können Sie über den [Azure Import/Export-Dienst](../storage/storage-import-export-service.md) Festplattenlaufwerke mit Ihren Daten an ein Azure-Rechenzentrum senden. Ihre Daten werden zunächst in Azure Storage-Blobs hochgeladen. Anschließend können Sie mit [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) oder dem [AdlCopy-Tool](data-lake-store-copy-data-azure-storage-blob.md) Daten aus Azure Storage-Blobs in Data Lake Store kopieren.

  > [!NOTE]
  > Wenn Sie den Import/Export-Dienst verwenden, sollte die Größe der Dateien auf den Datenträgern, die Sie an das Azure-Rechenzentrum senden, nicht größer als 195 GB sein.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Verarbeiten von Daten, die im Data Lake-Speicher gespeichert sind
Sobald die Daten im Data Lake-Speicher verfügbar sind, können Sie mit den unterstützten Big Data-Anwendungen eine Analyse dieser Daten ausführen. Derzeit können Sie mit Azure HDInsight und Azure Data Lake Analytics Datenanalyseaufträge ausführen, die sich auf Daten im Data Lake-Speicher beziehen.

![Analysieren von Daten in Data Lake Store](./media/data-lake-store-data-scenarios/analyze-data.png "Analysieren von Daten in Data Lake Store")

Betrachten Sie die folgenden Beispiele.

* [Erstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Herunterladen von Daten aus dem Data Lake-Speicher
Vielleicht möchten Sie auch für Szenarien wie die folgenden Daten aus dem Azure Data Lake-Speicher herunterladen oder verschieben:

* Verschieben von Daten aus anderen Repositorys zur Verbindung mit Ihren vorhandenen Datenverarbeitungspipelines. Sie möchten z. B. Daten aus dem Data Lake-Speicher in die Azure SQL-Datenbank oder auf einen lokalen SQL Server verschieben.
* Herunterladen von Daten auf Ihren lokalen Computer für die Verarbeitung in IDE-Umgebungen beim Erstellen von Anwendungsprototypen.

![Ausgeben von Daten aus Data Lake Store](./media/data-lake-store-data-scenarios/egress-data.png "Ausgeben von Daten aus Data Lake Store")

In solchen Fällen können Sie eine der folgenden Optionen verwenden:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Sie können auch mit den folgenden Methoden Ihr eigenes Skript/Ihre eigene Anwendung zum Herunterladen von Daten aus dem Data Lake-Speicher schreiben.

* [Plattformübergreifende Azure-CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [.NET SDK für den Azure Data Lake-Speicher](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Visualisieren von Daten im Data Lake-Speicher
Sie können eine Kombination von Diensten verwenden, um visuelle Darstellungen der Daten zu erstellen, die im Data Lake-Speicher gespeichert sind.

![Visualisieren von Daten in Data Lake Store](./media/data-lake-store-data-scenarios/visualize-data.png "Visualisieren von Daten in Data Lake Store")

* Sie können beginnen, indem Sie [Daten mithilfe von Azure Data Factory aus Data Lake Store nach Azure SQL Data Warehouse verschieben](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
* Danach können Sie [Power BI in Azure SQL Data Warehouse integrieren](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) , um eine visuelle Darstellung der Daten zu erstellen.

