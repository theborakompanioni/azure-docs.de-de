<properties 
	pageTitle="Verschieben von Daten mit der Kopieraktivität | Microsoft Azure" 
	description="Informieren Sie sich über das Verschieben von Daten in Data Factory-Pipelines: Datenmigration zwischen Cloudspeichern, zwischen lokalen Speicherorten und der Cloud. Verwenden Sie die Kopieraktivität." 
	keywords="Daten kopieren, Datenverschiebung, Datenmigration, Daten übertragen"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/08/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten mit der Kopieraktivität

## Übersicht
Azure Data Factory ermöglicht Ihnen die Verwendung der Kopieraktivität zum Kopieren von Daten verschiedener Formen aus einer Vielzahl von lokalen und Clouddatenquellen in Azure, damit diese weiter transformiert und analysiert werden können. Die Kopieraktivität kann auch zum Veröffentlichen von Transformations- und Analyseergebnissen für die Verwendung für Business Intelligence (BI) und in Anwendungen verwendet werden.

![Rolle der Kopieraktivität](media/data-factory-data-movement-activities/copy-activity.png)

Der Kopieraktivität liegt ein sicherer, zuverlässiger, skalierbarer und [global verfügbarer Dienst](#global) zugrunde. Dieser Artikel enthält Details zum Verschieben von Daten in Data Factory und zur Kopieraktivität. Zunächst erfahren Sie, wie die Datenmigration zwischen zwei Clouddatenspeichern und zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher abläuft.

> [AZURE.NOTE] Grundlegende Informationen zum Definieren von Aktivitäten finden Sie im Artikel [Grundlegendes zu Pipelines und Aktivitäten](data-factory-create-pipelines.md).

### Kopieren von Daten zwischen zwei Clouddatenspeichern
Wenn sowohl die Quell- als auch die Senkendatenspeicher (sprich die Zieldatenspeicher) sich in der Cloud befinden, durchläuft die Kopieraktivität die folgenden Phasen, um die Daten aus der Quelle in die Senke zu kopieren bzw. zu verschieben. Der Dienst, der als Grundlage der Kopieraktivität dient, führt Folgendes durch:

1. liest Daten aus dem Quelldatenspeicher
2. Führt die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung und Typumwandlung basierend auf der Konfiguration des Eingabedatasets, des Ausgabedatasets und der Kopieraktivität durch
3.	schreibt Daten in den Zieldatenspeicher

Der Dienst wählt automatisch die optimale Region für die Datenverschiebung aus. Hierbei handelt es sich in der Regel um die Region, die dem Senkendatenspeicher am nächsten liegt.

![Cloud-zu-Cloud-Kopie](./media/data-factory-data-movement-activities/cloud-to-cloud.png)


### Kopieren von Daten zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher
Um Daten sicher zwischen lokalen Datenspeichern hinter der Unternehmensfirewall und einem Clouddatenspeicher zu verschieben, müssen Sie auf dem lokalen Computer das Datenverwaltungsgateway installieren. Dabei handelt es sich um einen Agent, der eine hybride Datenverschiebung und -verarbeitung ermöglicht. Das Datenverwaltungsgateway kann auf dem gleichen Computer wie der Datenspeicher selbst oder auf einem separaten Computer, der Zugriff auf den Datenspeicher hat, installiert werden. In diesem Szenario werden die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung und Typumwandlung vom Datenverwaltungsgateway ausgeführt. In solch einem Fall fließen die Daten nicht über den Azure Data Factory-Dienst. Das Datenverwaltungsgateway schreibt die Daten direkt in den Zielspeicher.

![Lokal-zu-Cloud-Kopie](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie eine Einführung und eine exemplarische Vorgehensweise und im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) ausführliche Informationen zum Datenverwaltungsgateway.

Sie können auch mithilfe des Datenverwaltungsgateways Daten von bzw. in unterstützte Datenspeicher verschieben, die auf Azure IaaS-VMs (Infrastructure-as-a-Service-VMs) gehostet werden. In diesem Fall kann das Datenverwaltungsgateway auf dem gleichen virtuellen Azure-Computer wie der Datenspeicher selbst oder auf einem separaten virtuellen Computer, der Zugriff auf den Datenspeicher hat, installiert werden.

## Unterstützte Datenspeicher und Formate
Die Kopieraktivität kopiert die Daten aus einem **Quelldatenspeicher** in einen **Senkendatenspeicher**. Data Factory unterstützt die folgenden Datenspeicher. **Außerdem können Daten aus beliebigen Quellen in beliebige Senken geschrieben werden.** Klicken Sie auf einen Datenspeicher, um zu erfahren, wie Daten in den/aus dem Speicher kopiert werden.

Kategorie | Datenspeicher | Als Quelle unterstützt | Als Senke unterstützt
:------- | :--------- | :------------------ | :-----------------
Azure | [Azure Blob](data-factory-azure-blob-connector.md) <br/> [Azure Data Lake Store](data-factory-azure-datalake-connector.md) <br/> [Azure SQL-Datenbank](data-factory-azure-sql-connector.md) <br/> [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) <br/> [Azure-Tabelle](data-factory-azure-table-connector.md) <br/> [Azure DocumentDB](data-factory-azure-documentdb-connector.md) <br/> | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ 
Datenbanken | [SQL Server](data-factory-sqlserver-connector.md)* <br/> [Oracle](data-factory-onprem-oracle-connector.md)* <br/> [MySQL](data-factory-onprem-mysql-connector.md)* <br/> [DB2](data-factory-onprem-db2-connector.md)* <br/> [Teradata](data-factory-onprem-teradata-connector.md)* <br/> [PostgreSQL](data-factory-onprem-postgresql-connector.md)* <br/> [Sybase](data-factory-onprem-sybase-connector.md)* <br/>[Cassandra](data-factory-onprem-cassandra-connector.md)* <br/>[MongoDB](data-factory-on-premises-mongodb-connector.md)* | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓<br/> ✓ <br/> ✓ <br/> ✓ | ✓ <br/> ✓ <br/> &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;<br/> &nbsp; 
File | [Dateisystem](data-factory-onprem-file-system-connector.md)* <br/> [Hadoop Distributed File System (HDFS)](data-factory-hdfs-connector.md)* | ✓ <br/> ✓ <br/> | ✓ <br/> &nbsp; 
Andere | [Salesforce](data-factory-salesforce-connector.md)<br/> [Generisches ODBC](data-factory-odbc-connector.md)* <br/> [Generisches OData](data-factory-odata-connector.md) <br/> [Webtabelle (Tabelle aus HTML)](data-factory-web-table-connector.md) <br/> [GE Historian](data-factory-odbc-connector.md#ge-historian-store)* | ✓ <br/> ✓ <br/> ✓ <br/> ✓ <br/> ✓ | &nbsp; <br/> &nbsp; <br/> &nbsp; <br/> &nbsp;<br/> &nbsp;<br/> &nbsp;

> [AZURE.NOTE] Datenspeicher mit * können lokal oder auf Azure IaaS verfügbar sein. Für ihre Verwendung müssen Sie das [Datenverwaltungsgateway](data-factory-data-management-gateway.md) auf einem lokalen oder einem Azure IaaS-Computer installieren.

Wenn Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der **Kopieraktivität** nicht unterstützt wird, können Sie die **benutzerdefinierte Aktivität** in Data Factory mit Ihrer eigenen Logik zum Kopieren/Verschieben der Daten verwenden. Weitere Informationen zum Erstellen und Verwenden einer benutzerdefinierten Aktivität finden Sie im Artikel [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md).

### Unterstützte Dateiformate
Die Kopieraktivität kann Dateien unverändert zwischen zwei dateibasierten Datenspeichern wie Azure-Blob, Dateisystem und Hadoop Distributed File System (HDFS) kopieren. Zu diesem Zweck können Sie den [Formatabschnitt](data-factory-create-datasets.md) in den Definitionen für Ein- und Ausgabedataset überspringen. Die Daten werden dann direkt und ohne Serialisierung/Deserialisierung kopiert.

Die Kopieraktivität liest und schreibt Dateien in den folgenden Formaten: Text, Avro, ORC und JSON. Im Folgenden finden Sie einige Beispiele für Aktionen, die Sie mit der Kopieraktivität durchführen können:

-	Kopieren von Daten im Textformat (CSV) von Azure-Blob und Schreiben in Azure SQL
-	Kopieren von Dateien im Textformat (CSV) von lokalen Dateisystem und Schreiben in Azure-Blob im Avro-Format
-	Kopieren von Daten in Azure SQL-Datenbank und Schreiben in ein lokales HDFS im ORC-Format



## <a name="global"></a>Global verfügbare Datenverschiebung
Der Dienst, der die Kopieraktivität unterstützt, steht weltweit in den folgenden Regionen und Gebieten zur Verfügung, obwohl Azure Data Factory selbst nur in den Regionen „USA, Westen“, „USA, Osten“ und „Europa, Norden“ verfügbar ist. Die global verfügbare Topologie gewährleistet effiziente Datenverschiebungen und vermeidet meist regionsübergreifende Hops. Unter [Dienste nach Region](https://azure.microsoft.com/regions/#services) erfahren Sie, in welchen Regionen der Data Factory-Dienst und die Datenverschiebung verfügbar sind.

### Kopieren von Daten zwischen Clouddatenspeichern
Wenn sich Quell- und Senkendatenspeicher in der Cloud befinden, verwendet Azure Data Factory für die Durchführung der Datenverschiebung eine Dienstbereitstellung in der Region, die dem Gebiet der Senke am nächsten liegt. Informationen zur Zuordnung erhalten Sie in der folgenden Tabelle:

Region des Zieldatenspeichers | Verwendete Region für die Datenverschiebung
:----------------------------------- | :----------------------------
USA (Ost) | USA (Ost)
USA (Ost) 2 | USA (Ost) 2
USA (Mitte) | USA (Mitte)
USA (West) | USA (West)
USA (Mitte/Norden) | USA (Mitte/Norden)
USA (Mitte/Süden) | USA (Mitte/Süden)
Nordeuropa | Nordeuropa
Westeuropa | Westeuropa
Südostasien | Südostasien
Ostasien | Südostasien
Japan Ost | Japan Ost
Japan (Westen) | Japan Ost
Brasilien Süd | Brasilien Süd
Australien (Osten) | Australien (Osten)
Australien (Südosten) | Australien (Südosten)


> [AZURE.NOTE] Wenn die Region des Zieldatenspeichers in der obigen Liste nicht enthalten ist, schlägt die Kopieraktivität fehl, anstatt eine Alternativregion auszuwählen.

### Kopieren von Daten zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher
Wenn Daten zwischen einem lokalen Datenspeicher (oder einer Azure IaaS-VM) und der Cloud kopiert werden, wird die Datenverschiebung vom [Datenverwaltungsgateway](data-factory-data-management-gateway.md) auf einem lokalen Computer oder einer Azure IaaS-VM durchgeführt. Der Datenfluss erfolgt nicht über den Dienst in der Cloud, es sei denn, Sie verwenden die [gestaffelte Kopierfunktion](data-factory-copy-activity-performance.md#staged-copy). In diesem Fall erfolgt der Datenfluss über den gestaffelten Azure Blob Storage, bevor die Daten in den Senkendatenspeicher geschrieben werden.


## Erstellen einer Pipeline mit Kopieraktivität 
Sie können eine Pipeline mit einer Kopieraktivität auf verschiedene Arten erstellen:

### Verwenden des Kopier-Assistenten
Der **Assistent zum Kopieren in Data Factory** ermöglicht das Erstellen einer Pipeline mit einer Kopieraktivität, mit der Sie Daten aus unterstützten Quellen an Ziele kopieren können, **ohne JSON-Definitionen** für verknüpfte Dienste, Datasets und Pipelines zu schreiben. Im Tutorial für den [Assistenten zum Kopieren in Data Factory](data-factory-copy-wizard.md) finden Sie Einzelheiten zum Assistenten.

### Verwenden von JSON-Skripts
Sie können den Data Factory-Editor im Azure-Portal, Visual Studio oder Azure PowerShell verwenden, um eine JSON-Definition für eine Pipeline mit Kopieraktivität zu erstellen, und diese bereitstellen, um die Pipeline in Data Factory zu erstellen. Unter [Tutorial: Verwenden der Kopieraktivität in einer Azure Data Factory-Pipeline](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie ein Tutorial mit schrittweisen Anweisungen.

JSON-Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen, Richtlinien usw. sind für alle Arten von Aktivitäten verfügbar. Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren.

Bei der Kopieraktivität variiert der **typeProperties**-Abschnitt je nach Art der Quellen und Senken. Klicken Sie im [Abschnitt mit den unterstützten Quellen/Senken](#supported-data-stores) auf eine Quelle/Senke, um Informationen zu den Typeigenschaften zu erhalten, die von der Kopieraktivität für diesen Datenspeicher unterstützt werden.

Dies ist eine Beispiel-JSON-Definition:

	{
	  "name": "ADFTutorialPipeline",
	  "properties": {
	    "description": "Copy data from Azure blob to Azure SQL table",
	    "activities": [
	      {
	        "name": "CopyFromBlobToSQL",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "InputBlobTable"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "OutputSQLTable"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "BlobSource"
	          },
	          "sink": {
	            "type": "SqlSink",
	            "writeBatchSize": 10000,
	            "writeBatchTimeout": "60:00:00"
	          }
	        },
	        "Policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "NewestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	    ],
	    "start": "2016-07-12T00:00:00Z",
	    "end": "2016-07-13T00:00:00Z"
	  }
	} 

Der im Ausgabedataset definierte Zeitplan legt fest, wann die Aktivität ausgeführt wird (z.B. **täglich**: Häufigkeit: Tag, Intervall: 1). Die Aktivität kopiert Daten aus einem Eingabedataset (**Quelle**) in ein Ausgabedataset (**Senke**). Sie können mehr als ein Eingabedataset in der Kopieraktivität angeben, mit denen dann die Abhängigkeiten überprüft werden, bevor die Aktivität ausgeführt wird. Es werden aber nur die Daten aus dem ersten Dataset in das Zieldataset kopiert. Weitere Informationen finden Sie unter [Planung und Ausführung](data-factory-scheduling-and-execution.md).

## Leistung und Optimierung 
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken. Außerdem wird die beobachtete Leistung bei internen Tests aufgeführt und verschiedene Methoden zur Optimierung der Leistung der Kopieraktivität erläutert.

## Planen und sequenzielles Kopieren
Ausführliche Informationen zur Planung und Ausführung in Data Factory finden Sie im Artikel [Planung und Ausführung](data-factory-scheduling-and-execution.md).

Es ist möglich, mehrere Kopiervorgänge nacheinander sequenziell/sortiert auszuführen. Lesen Sie auch den Abschnitt [Sortierte Kopie](data-factory-scheduling-and-execution.md#ordered-copy) in dem Artikel.

## Typkonvertierungen 
Verschiedene Datenspeicher weisen unterschiedliche systemeigene Typsysteme auf. Die Kopieraktivität führt automatische Typumwandlungen von Quelltypen in Senkentypen mithilfe des folgenden aus zwei Schritten bestehenden Ansatzes durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Sie finden die Zuordnung für ein bestimmtes natives Typsystem zu .NET für den Datenspeicher in den entsprechenden Artikeln (klicken Sie auf den jeweiligen Link in der Tabelle [Datenspeicher](#supported-data-stores)). Über diese Zuordnungen können Sie beim Erstellen von Tabellen die geeigneten Typen bestimmen, damit während der Kopieraktivität die richtigen Umwandlungen ausgeführt werden.

 
## Nächste Schritte
- Unter [Kopieren von Daten aus Azure-Blob in Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) erfahren Sie ganz allgemein, wie Sie die Kopieraktivität verwenden, um Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher zu verschieben.
- Unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren Sie, wie Sie Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher verschieben.
 

<!---HONumber=AcomDC_0810_2016-->