<properties 
	pageTitle="Datenverschiebungsaktivitäten | Microsoft Azure" 
	description="Informieren Sie sich über das Verschieben von Daten in Data Factory-Pipelines: Datenmigration zwischen Cloudspeichern, zwischen lokalen Speicherorten und der Cloud. Verwenden Sie die Kopieraktivität." 
	keywords="Datenverschiebung, Datenmigration, Daten kopieren, Daten übertragen"
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
	ms.date="07/11/2016" 
	ms.author="spelluru"/>

# Datenverschiebung und die Kopieraktivität: Migrieren von Daten in die Cloud und zwischen Cloudspeichern
Das Verschieben von Daten aus einer Quelle an eine Senke (Ziel) erfolgt mithilfe der [Kopieraktivität](#copyactivity) in Azure Data Factory. Der Kopieraktivität liegt ein sicherer, zuverlässiger, skalierbarer und [global verfügbarer Dienst](#global) zugrunde. Der Dienst wählt automatisch die optimale Region für die Datenverschiebung aus. Hierbei handelt es sich in der Regel um die Region, die dem Senkendatenspeicher am nächsten liegt.

Hier erfahren Sie, wie die Datenmigration zwischen zwei Clouddatenspeichern, zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher und von einem Datenspeicher bzw. an einen Datenspeicher auf einem virtuellen Azure-IaaS-Computer abläuft.

## Kopieren von Daten zwischen zwei Clouddatenspeichern
Wenn sowohl die Quell- als auch die Senkendatenspeicher (sprich die Zieldatenspeicher) sich in der Cloud befinden, durchläuft die Kopieraktivität die folgenden Phasen, um die Daten aus der Quelle in die Senke zu kopieren bzw. zu verschieben. Der Dienst, der als Grundlage der Kopieraktivität dient, führt Folgendes durch:

1. liest Daten aus dem Quelldatenspeicher
2.	führt die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung und Typumwandlung basierend auf der Konfiguration des Eingabedatasets, Ausgabedatasets und der Kopieraktivität durch
3.	schreibt Daten in den Zieldatenspeicher

![Cloud-zu-Cloud-Kopie](.\media\data-factory-data-movement-activities\cloud-to-cloud.png)


## Kopieren von Daten zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher
Um Daten sicher zwischen lokalen Datenspeichern hinter der Unternehmensfirewall und einem Clouddatenspeicher zu verschieben, müssen Sie auf dem lokalen Computer das Datenverwaltungsgateway installieren, wobei es sich um einen Agent handelt, der eine hybride Datenverschiebung und -verarbeitung ermöglicht. Das Datenverwaltungsgateway kann auf dem gleichen Computer wie der Datenspeicher selbst oder auf einem separaten Computer, der Zugriff auf den Datenspeicher hat, installiert werden. In diesem Szenario werden die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung und Typumwandlung vom Datenverwaltungsgateway ausgeführt. In solch einem Fall fließen die Daten nicht über den Azure Data Factory-Dienst. Das Datenverwaltungsgateway schreibt die Daten direkt in den Zielspeicher.

![Lokal-zu-Cloud-Kopie](.\media\data-factory-data-movement-activities\onprem-to-cloud.png)

Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) finden Sie eine Einführung und eine exemplarische Vorgehensweise und im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) ausführliche Informationen zum Datenverwaltungsgateway.

## Kopieren von Daten aus einem bzw. in einen Datenspeicher auf einer Azure Iaas-VM 
Sie können auch mithilfe des Datenverwaltungsgateways Daten von bzw. in unterstützte Datenspeicher verschieben, die auf Azure IaaS-VMs (Infrastructure-as-a-Service-VMs) gehostet werden. In diesem Fall kann das Datenverwaltungsgateway auf dem gleichen virtuellen Azure-Computer wie der Datenspeicher selbst oder auf einem separaten virtuellen Computer, der Zugriff auf den Datenspeicher hat, installiert werden.

## Unterstützte Datenspeicher
Die Kopieraktivität kopiert die Daten aus einem **Quelldatenspeicher** in einen **Senkendatenspeicher**. Data Factory unterstützt die folgenden Datenspeicher. **Außerdem können Daten aus beliebigen Quellen in beliebige Senken geschrieben werden.** Klicken Sie auf einen Datenspeicher, um zu erfahren, wie Daten in den/aus dem Speicher kopiert werden.

| Quellen| Senken |
|:------- | :---- |
| <ul><li>[Azure-Blob](data-factory-azure-blob-connector.md)</li><li>[Azure-Tabelle](data-factory-azure-table-connector.md)</li><li>[Azure SQL-Datenbank](data-factory-azure-sql-connector.md)</li><li>[Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)</li><li>[Azure DocumentDB (siehe Hinweis weiter unten)](data-factory-azure-documentdb-connector.md)</li><li>[Azure Data Lake Store](data-factory-azure-datalake-connector.md)</li><li>[OData-Quellen](data-factory-odata-connector.md)</li><li>[Webtabelle (HTML-basierte Tabelle)](data-factory-web-table-connector.md)</li><li>[GE Historian lokal/Azure IaaS](data-factory-odbc-connector.md#ge-historian-store)</li><li>[Salesforce](data-factory-salesforce-connector.md)</li><li>[SQL Server lokal/Azure IaaS](data-factory-sqlserver-connector.md)</li><li>[Dateisystem lokal/Azure IaaS](data-factory-onprem-file-system-connector.md)</li><li>[Oracle-Datenbank lokal/Azure IaaS](data-factory-onprem-oracle-connector.md)</li><li>[MySQL-Datenbank lokal/Azure IaaS ](data-factory-onprem-mysql-connector.md)</li><li>[DB2-Datenbank lokal/Azure IaaS](data-factory-onprem-db2-connector.md)</li><li>[Teradata-Datenbank lokal/Azure IaaS ](data-factory-onprem-teradata-connector.md)</li><li>[Sybase-Datenbank lokal/Azure IaaS](data-factory-onprem-sybase-connector.md)</li><li>[PostgreSQL-Datenbank lokal/Azure IaaS](data-factory-onprem-postgresql-connector.md)</li><li>[ODBC-Datenquellen lokal/Azure IaaS](data-factory-odbc-connector.md)</li><li>[Hadoop Distributed File System (HDFS) lokal/Azure IaaS](data-factory-hdfs-connector.md)</li><li>[Cassandra-Datenbank lokal/Azure IaaS](data-factory-onprem-cassandra-connector.md)</li></ul> | <ul><li>[Azure-Blob](data-factory-azure-blob-connector.md)</li><li>[Azure-Tabelle](data-factory-azure-table-connector.md)</li><li>[Azure SQL-Datenbank](data-factory-azure-sql-connector.md)</li><li>[Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)</li><li>[Azure DocumentDB (siehe Hinweis weiter unten)](data-factory-azure-documentdb-connector.md)</li><li>[Azure Data Lake Store](data-factory-azure-datalake-connector.md)</li><li>[SQL Server lokal/Azure IaaS](data-factory-sqlserver-connector.md)</li><li>[Dateisystem lokal/Azure IaaS](data-factory-onprem-file-system-connector.md)</li><li>[Oracle-Datenbank lokal/Azure IaaS](data-factory-onprem-oracle-connector.md)</li></ul> |


> [AZURE.NOTE] Das Kopieren von Daten aus Azure DocumentDB an lokale/Azure IaaS-basierte Datenspeicher (und umgekehrt) wird derzeit nicht unterstützt. Die vollständige Matrix für Azure DocumentDB wird in Kürze unterstützt.

Wenn Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der **Kopieraktivität** nicht unterstützt wird, können Sie die **benutzerdefinierte Aktivität** in Data Factory mit Ihrer eigenen Logik zum Kopieren/Verschieben der Daten verwenden. Weitere Informationen zum Erstellen und Verwenden einer benutzerdefinierten Aktivität finden Sie im Artikel [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md).

## Lernprogramm:
Ein kurzes Lernprogramm zur Verwendung der Kopieraktivität finden Sie unter [Lernprogramm: Verwenden der Kopieraktivität in einer Azure Data Factory-Pipeline](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). In diesem Lernprogramm kopieren Sie mit der Kopieraktivität Daten aus einem Azure-Blob-Speicher in eine Azure SQL-Datenbank.

## <a name="copyactivity"></a>Kopieraktivität
Die Kopieraktivität kopiert Daten aus einem Eingabedataset (**Quelle**) in ein Ausgabedataset (**Senke**). Das Kopieren der Daten erfolgt als Batch entsprechend dem für die Aktivität angegebenen Zeitplan. Grundlegende Informationen zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines und Aktivitäten in Azure Data Factory](data-factory-create-pipelines.md).

Die Kopieraktivität bietet die folgenden Funktionen:

### <a name="global"></a>Global verfügbare Datenverschiebung
Obwohl Azure Data Factory selbst nur in den Regionen „USA, Westen“, „USA, Osten“ und „Europa, Norden“ verfügbar ist, steht der Dienst, der die Kopieraktivität unterstützt, global in den folgenden Regionen und Gebieten zur Verfügung. Die global verfügbare Topologie gewährleistet effiziente Datenverschiebungen und vermeidet in den meisten Fällen regionsübergreifende Hops.

Das **Datenverwaltungsgateway** oder **Azure Data Factory** führt die Datenverschiebung basierend auf dem Ort des Quell- und Zieldatenspeichers in Form eines Kopiervorgangs durch. Die folgende Tabelle enthält die Details hierzu:

Speicherort der Quelldaten | Speicherort der Zieldaten | Datenverschiebung erfolgt per  
-------------------------- | ------------------------------- | ----------------------------- 
lokal/Azure-VM (IaaS) | Cloud | **Datenverwaltungsgateway** auf lokalem Computer/virtuellem Azure-Computer. Die Daten fließen nicht über den Dienst in der Cloud. <br/><br/>Hinweis: Das Datenverwaltungsgateway kann sich auf dem gleichen lokalen Computer/virtuellen Azure-Computer wie der Datenspeicher oder auf einem anderen lokalen Computer/virtuellen Azure-Computer befinden, solange eine Verbindung mit beiden Datenspeichern hergestellt werden kann.
Cloud | lokal/Azure-VM (IaaS) | Wie oben. 
lokal/Azure-VM (IaaS) | lokal/Azure-VM | **Datenverwaltungsgateway, das der Quelle zugeordnet ist.** Die Daten fließen nicht über den Dienst in der Cloud. Siehe Hinweis oben.   
Cloud | Cloud | **Der Clouddienst, der der Kopieraktivität zugrunde liegt.** Azure Data Factory verwendet die Bereitstellung dieses Diensts in der Region, die der Senkenposition in der gleichen Region am nächsten liegt. Die folgende Tabelle bietet eine Übersicht: <br/><br/><table><tr><th>Region des Zieldatenspeichers</th> <th>Für Datenverschiebung verwendete Region</th></tr><tr><td>USA, Osten</td><td>USA, Osten</td></tr><tr><td>USA, Osten 2</td><td>USA, Osten 2</td><tr/><tr><td>USA, Mitte</td><td>USA, Mitte</td><tr/><tr><td>USA, Westen</td><td>USA, Westen</td></tr><tr><td>USA, Norden-Mitte</td><td>USA, Norden-Mitte</td></tr><tr><td>USA, Süden-Mitte</td><td>USA, Süden-Mitte</td></tr><tr><td>Europa, Norden</td><td>Europa, Norden</td></tr><tr><td>Europa, Westen</td><td>Europa, Westen</td></tr><tr><td>Asien, Südosten</td><td>Asien, Südosten</td></tr><tr><td>Asien, Osten</td><td>Asien, Südosten</td></tr><tr><td>Japan, Osten</td><td>Japan, Osten</td></tr><tr><td>Japan, Westen</td><td>Japan, Osten</td></tr><tr><td>Brasilien, Süden</td><td>Brasilien, Süden</td></tr><tr><td>Australien, Osten</td><td>Australien, Osten</td></tr><tr><td>Australien, Südosten</td><td>Australien, Südosten</td></tr></table>


> [AZURE.NOTE] Wenn die Region des Zieldatenspeichers in der obigen Liste nicht enthalten ist, schlägt die Kopieraktivität fehl, anstatt eine Alternativregion zu wählen.


### Zuverlässige und kostengünstige Datenverschiebung
Die Kopieraktivität wurde entwickelt, um große Datenmengen zuverlässig und geschützt vor vorübergehenden Fehlern über eine Vielzahl von Datenquellen zu verschieben. Daten können auf kostengünstige Weise mit der Option zum Aktivieren der Komprimierung über das Netzwerk kopiert werden.

### Typkonvertierungen in andere Typsysteme
Verschiedene Datenspeicher weisen unterschiedliche systemeigene Typsysteme auf. Die Kopieraktivität führt automatische Typumwandlungen von Quelltypen in Senkentypen mithilfe des folgenden aus zwei Schritten bestehenden Ansatzes durch:

1. Konvertieren von systemeigenen Quelltypen in den .NET-Typ
2. Konvertieren vom .NET-Typ in systemeigenen Senkentyp

Sie finden die Zuordnung für ein bestimmtes systemeigenes Typsystem zu .NET für den Datenspeicher im entsprechenden Artikel zum Connector des Datenspeichers. Über diese Zuordnungen können Sie beim Erstellen von Tabellen die geeigneten Typen bestimmen, damit während der Kopieraktivität die richtigen Umwandlungen ausgeführt werden.

### Arbeiten mit unterschiedlichen Dateiformaten
Bei der Kopieraktivität werden verschiedene Dateiformate unterstützt, einschließlich Binär-, Text-, Avro-, ORC- und JSON-Formate für dateibasierte Speicher. Sie können mit der Kopieraktivität Daten von einem Format in ein anderes konvertieren. Beispiel: Text (CSV) in Avro. Bei unstrukturierten Daten können Sie die **Structure**-Eigenschaft in der JSON-Definition des [Datasets](data-factory-create-datasets.md) weglassen.

### Eigenschaften der Kopieraktivität
Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen, verschiedene Richtlinien usw. sind für alle Arten von Aktivitäten verfügbar. Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren.

Bei der Kopieraktivität variiert der **typeProperties**-Abschnitt je nach Art der Quellen und Senken. Klicken Sie im [Abschnitt mit den unterstützten Quellen/Senken](#supported-data-stores) auf eine Quelle/Senke, um Informationen zu den Typeigenschaften zu erhalten, die von der Kopieraktivität für diesen Datenspeicher unterstützt werden.

Auf jeder der oben genannten Seiten für Datenspeicher sind diese Eigenschaften für den jeweiligen Datenspeichertyp dokumentiert.

### Sortierte Kopie
Es ist möglich, mehrere Kopiervorgänge nacheinander sequenziell/sortiert auszuführen. Angenommen, Sie haben zwei Kopieraktivitäten in einer Pipeline: CopyActivity1 und CopyActivity2 mit den folgenden Eingabe-/Ausgabedatasets.

CopyActivity1: Eingabe: Dataset1 Ausgabe: Dataset2

CopyActivity2: Eingabe: Dataset2 Ausgabe: Dataset4

CopyActivity2 wird nur ausgeführt, wenn CopyActivity1 erfolgreich ausgeführt wurde und Dataset2 verfügbar ist.

Im obigen Beispiel kann CopyActivity2 eine andere Eingabe haben, z. B. Dataset3. Sie müssen jedoch auch Dataset2 als Eingabe für CopyActivity2 angeben, damit die Aktivität nicht so lange ausgeführt wird, bis CopyActivity1 abgeschlossen ist. Beispiel:

Kopieraktivität1: Eingabe: Dataset1 Ausgabe: Dataset2

CopyActivity2: Eingabe: Dataset3, Dataset2 Ausgabe: Dataset4

Wenn mehrere Eingaben angegeben wurden, wird nur das erste Eingabedataset zum Kopieren der Daten verwendet, die anderen Datasets werden aber als Abhängigkeiten verwendet. CopyActivity2 wird nur ausgeführt, wenn die folgenden Bedingungen erfüllt sind:

- CopyActivity2 wurde erfolgreich abgeschlossen und Dataset2 ist verfügbar. Dieses Dataset wird beim Kopieren von Daten zu Dataset4 nicht verwendet. Es fungiert nur als Terminplanungs-Abhängigkeit für CopyActivity2.
- Dataset3 ist verfügbar. Dieses Dataset stellt die Daten dar, die zum Ziel kopiert werden.


### Leistung und Optimierung der Kopieraktivität 
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken. Außerdem wird die beobachtete Leistung bei internen Tests aufgeführt und verschiedene Methoden zur Optimierung der Leistung der Kopieraktivität erläutert.


## Assistent zum Kopieren in Data Factory
Der **Assistent zum Kopieren in Data Factory** ermöglicht das Erstellen einer Pipeline, mit der Sie Daten aus unterstützten Quellen an Ziele kopieren können, ohne JSON-Definitionen für verknüpfte Dienste, Datasets und Pipelines zu schreiben. Klicken Sie zum Starten des Kopier-Assistenten auf der Startseite Ihrer Data Factory auf die Kachel **Daten kopieren**.

![Assistent zum Kopieren von Daten](./media/data-factory-data-movement-activities/copy-data-wizard.png)

### Merkmale

#### Ein intuitiver und unkomplizierter Assistent zum Kopieren von Daten 
Mit diesem Assistenten können Sie problemlos innerhalb weniger Minuten Daten von einer Quelle zu einem Ziel verschieben. Führen Sie dazu die folgenden einfachen Schritte aus:

1.	Wählen Sie die **Quelle** aus.
2.	Wählen Sie das **Ziel** aus.
3.	Konfigurieren Sie die **Einstellungen**.

![Auswählen einer Datenquelle](./media/data-factory-data-movement-activities/select-data-source-page.png)

#### Umfangreiche Datensuche und Schemazuordnungen
Sie können innerhalb des Assistenten Tabellen/Ordner durchsuchen, eine Datenvorschau anzeigen, Schemata zuordnen, Ausdrücke überprüfen und einfache Datentransformationen durchführen.

**Tabellen/Ordner durchsuchen** ![Durchsuchen von Tabellen und Ordnern](./media/data-factory-data-movement-activities/browse-tables-folders.png)

#### Skalierbare Funktionalität für unterschiedliche Daten- und Objekttypen
Das Szenario ist von Beginn an für Big Data ausgelegt. Es ist einfach und effizient, Data Factory-Pipelines zu erstellen, die hunderte von Ordnern, Dateien oder Tabellen verschieben können.

**Datenvorschau anzeigen, Schema zuordnen und einfache Transformationen durchführen** ![Dateiformateinstellungen](./media/data-factory-data-movement-activities/file-format-settings.png) ![Schemazuordnung](./media/data-factory-data-movement-activities/schema-mapping.png) ![Überprüfen von Ausdrücken](./media/data-factory-data-movement-activities/validate-expressions.png)

#### Skalierbare Funktionalität für unterschiedliche Daten- und Objekttypen
Das Szenario ist von Beginn an für Big Data ausgelegt. Mithilfe des Assistenten zum Kopieren ist es einfach und effizient, hunderte von Ordnern, Dateien oder Tabellen zu verschieben.

![Auswählen von Tabellen zum Kopieren von Daten](./media/data-factory-data-movement-activities/select-tables-to-copy-data.png)

#### Umfangreichere Planungsoptionen
Sie können den Kopiervorgang nur einmal oder nach einem Zeitplan (stündlich, täglich etc.) ausführen. Beide Optionen können für die verschiedensten Connectors über die lokale Kopie, Cloudkopie und die Kopie auf dem lokalen Desktop hinweg verwendet werden. Das einmalige Kopieren aktiviert eine einmalige Datenverschiebung von einer Quelle zu einem Ziel und wird auf Daten jeglicher Größe und jeglichen unterstützten Formats angewandt. Das Kopieren nach einem Zeitplan aktiviert das Kopieren von Daten nach einem vorgeschriebenen Wiederholungszyklus. Sie können umfangreiche Einstellungen (wie Wiederholen, Timeout, Warnungen etc.) nutzen, um das Kopieren nach einem Zeitplan zu konfigurieren.

![Planen von Eigenschaften](./media/data-factory-data-movement-activities/scheduling-properties.png)


### Ausprobieren 
Eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mit einer Kopieraktivität unter Verwendung des **Assistenten zum Kopieren in Data Factory** finden Sie unter [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Data Factory-Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md).


### Variablen im Azure Blob-Ordnerpfad
Sie können Variablen im Ordnerpfad verwenden, um Daten aus einem Ordner zu kopieren, der zur Laufzeit auf der Grundlage der [WindowStart-Systemvariablen](data-factory-functions-variables.md#data-factory-system-variables) bestimmt wird. Folgende Variablen werden unterstützt: **year**, **month**, **day**, **hour**, **minute** und **{custom}**. Beispiel: Eingabeordner/{year}/{month}/{day}.

Angenommen, Sie haben Eingabeordner im folgenden Format:
	
	2016/03/01/01
	2016/03/01/02
	2016/03/01/03
	...

Klicken Sie auf die Schaltfläche **Durchsuchen** für **die Datei oder den Ordner**, navigieren Sie zu einem der Ordner (Beispiel: 2016>03>01>02), und klicken Sie auf **Auswählen**. Das Textfeld sollte jetzt **2016/03/01/02** enthalten. Ersetzen Sie nun **2016** durch **{year}**, **03** durch **{month}**, **01** durch **{day}**, **02** durch **{hour}**, und drücken Sie die TAB-TASTE. Daraufhin sollten wie nachfolgend dargestellt Dropdownlisten zum Auswählen des **Formats** für diese vier Variablen angezeigt werden:

![Verwenden von Systemvariablen](./media/data-factory-data-movement-activities/blob-standard-variables-in-folder-path.png)

Sie können auch, wie nachfolgend dargestellt, eine **benutzerdefinierte** Variable und beliebige [unterstützte Formatzeichenfolgen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) verwenden. Wählen Sie einen Ordner mit dieser Struktur aus, indem Sie zunächst die Schaltfläche „Durchsuchen“ verwenden. Ersetzen Sie einen Wert durch **{custom}**, und drücken Sie dann die TAB-TASTE, um das Textfeld anzuzeigen, in das Sie die Formatzeichenfolge eingeben können.

![Verwenden von benutzerdefinierten Variablen](./media/data-factory-data-movement-activities/blob-custom-variables-in-folder-path.png)

## Nächste Schritte
- Unter [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) erfahren Sie ganz allgemein, wie Sie die Kopieraktivität verwenden, um Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher zu verschieben.
- Unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) erfahren Sie, wie Sie Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher verschieben.
 

<!---HONumber=AcomDC_0720_2016-->