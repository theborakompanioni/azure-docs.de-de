<properties
	pageTitle="Alle Themen über den Data-Factory-Dienst | Microsoft Azure"
	description="Tabelle mit allen Themen zum Azure-Dienst namens „Data Factory“ auf „http://azure.microsoft.com/documentation/articles/“, Titel und Beschreibung."
	services="data-factory"
	documentationCenter=""
	authors="spelluru"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="data-factory"
	ms.workload="data-factory"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/21/2016"
	ms.author="spelluru"/>


# Alle Themen zum Azure Data Factory-Dienst

In diesem Thema sind alle Themen mit direktem Bezug zum **Data Factory**-Dienst von Azure aufgelistet. Sie können auf dieser Webseite mit **STRG+F** nach Schlüsselwörtern suchen, um aktuell interessante Themen zu finden.




## Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Erstellen der ersten Azure Data Factory mit der Data Factory-REST-API](data-factory-build-your-first-pipeline-using-rest-api.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Beispielpipeline mit der Data Factory-REST-API. |
| 2 | [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der REST-API](data-factory-copy-activity-tutorial-using-rest-api.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline mit Kopieraktivität mithilfe der REST-API. |
| 3 | [Assistent zum Kopieren in Data Factory](data-factory-copy-wizard.md) | Erfahren Sie, wie Sie den Assistenten zum Kopieren in Data Factory verwenden, um Daten aus unterstützten Datenquellen in Senken zu kopieren. |
| 4 | [Gateway zur Datenverwaltung](data-factory-data-management-gateway.md) | Richten Sie ein Datengateway ein, um Daten zwischen dem lokalen Speicher und der Cloud zu verschieben. Verwenden Sie das Datenverwaltungsgateway in Azure Data Factory zum Verschieben Ihrer Daten. |
| 5 | [Verschieben von Daten aus einer lokalen Cassandra-Datenbank mit Azure Data Factory](data-factory-onprem-cassandra-connector.md) | Enthält Informationen zum Verschieben von Daten aus einer lokalen Cassandra-Datenbank mit Azure Data Factory. |
| 6 | [Verschieben von Daten aus MongoDB mithilfe von Azure Data Factory](data-factory-on-premises-mongodb-connector.md) | Erfahren Sie, wie Sie Daten aus der MongoDB-Datenbank mithilfe von Azure Data Factory verschieben. |
| 7 | [Verschieben von Daten aus Salesforce mithilfe von Azure Data Factory](data-factory-salesforce-connector.md) | Erfahren Sie, wie Sie Daten mithilfe von Azure Data Factory aus Salesforce verschieben. |


## Aktualisierte Artikel

In diesem Abschnitt werden Artikel aufgelistet, die kürzlich mit einem sehr umfangreichen oder wichtigen Update aktualisiert wurden. Für jeden aktualisierten Artikel wird ein grober Ausschnitt des hinzugefügten Markdowntexts angezeigt. Die Artikel wurden zwischen dem **26.07.2016** und dem **21.08.2016** aktualisiert.

| &nbsp; | Artikel | Aktualisierter Text, Ausschnitt |
| --: | :-- | :-- |
| 8 | [Erstellen, Überwachen und Verwalten von Azure Data Factorys mithilfe des Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | **Anmeldung ohne Popupdialogfeld** Der obige Beispielcode startet ein Dialogfeld zur Eingabe der Azure-Anmeldeinformationen. Wenn Sie sich programmgesteuert ohne ein Dialogfeld anmelden müssen, lesen Sie die Informationen zur Authentifizierung eines Dienstprinzipals mit Azure Resource Manager (resource-group-authenticate-service-principal.md authenticate-service-principal-with-certificate---powershell). **Beispiel** Erstellen der GetAuthorizationHeaderNoPopup-Methode (s.u.): public static string GetAuthorizationHeaderNoPopup() { var authority = new Uri(new Uri("https://login.windows.net"), ConfigurationManager.AppSettings "ActiveDirectoryTenantId" ); var context = new AuthenticationContext(authority.AbsoluteUri); var credential = new ClientCredential(ConfigurationManager.AppSettings "AdfClientId" , ConfigurationManager.AppSettings "AdfClientSecret" ); AuthenticationResult result = context.AcquireTokenAsync(ConfigurationManager.AppSettings "WindowsManagementUri" , credential).Result; |
| 9 | [Verschieben von Daten mit der Kopieraktivität](data-factory-data-movement-activities.md) | **Unterstützte Dateiformate** Die Kopieraktivität kann Dateien unverändert zwischen zwei dateibasierten Datenspeichern wie Azure Blob, Dateisystem und Hadoop Distributed File System (HDFS) kopieren. Zu diesem Zweck können Sie den Formatabschnitt (data-factory-create-datasets.md) in den Definitionen für Ein- und Ausgabedataset überspringen. Die Daten werden dann direkt und ohne Serialisierung/Deserialisierung kopiert. Die Kopieraktivität liest und schreibt Dateien in den folgenden Formaten: Text, Avro, ORC und JSON. Im Folgenden finden Sie einige Beispiele für Aktionen, die Sie mit der Kopieraktivität durchführen können: Kopieren von Daten im Textformat (CSV) aus Azure Blob und Schreiben in Azure SQL/Kopieren von Dateien im Textformat (CSV) aus lokalen Dateisystemen und Schreiben in Azure Blob im Avro-Format/Kopieren von Daten in Azure SQL-Datenbank und Schreiben in ein lokales HDFS im ORC-Format** .a name="global"../a.Global verfügbare Datenverschiebung** Der Dienst, der die Kopieraktivität unterstützt, steht weltweit in den folgenden Regionen und Gebieten zur Verfügung, obwohl Azure Data Factory nur in den Regionen „USA, Westen“, „USA, Osten“ und „Europa, Norden“ verfügbar ist.|
| 10 | [Verschieben von Daten aus einer OData-Quelle mithilfe von Azure Data Factory](data-factory-odata-connector.md) | **Verwenden der Windows-Authentifizierung für den Zugriff auf lokale OData-Quellen** { "name": "inputLinkedService", "properties": { "type": "OData", "typeProperties": { "url": ".endpoint of on-premises OData source e.g. Dynamics CRM.", "authenticationType": "Windows", "username": "domain\\user", "password": "password", "gatewayName": "mygateway" } } } |





## Lernprogramme

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 11 | [Tutorial: Erstellen der ersten Pipeline zum Verarbeiten von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md) | In diesem Azure Data Factory-Tutorial erfahren Sie, wie Sie eine Data Factory erstellen und planen, die Daten unter Verwendung eines Hive-Skripts in einem Hadoop-Cluster verarbeitet. |
| 12 | [Tutorial: Erstellen der ersten Azure Data Factory mit einer Azure Resource Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Beispielpipeline mithilfe einer Azure-Ressourcen-Manager-Vorlage. |
| 13 | [Erstellen der ersten Azure Data Factory mit dem Azure-Portal/Data Factory-Editor](data-factory-build-your-first-pipeline-using-editor.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Beispielpipeline mit dem Data Factory-Editor im Azure-Portal. |
| 14 | [Erstellen der ersten Azure Data Factory mit Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Beispielpipeline mithilfe von Azure PowerShell. |
| 15 | [Erstellen der ersten Azure Data Factory mit Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Beispielpipeline mithilfe von Visual Studio. |
| 16 | [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Data Factory-Editors](data-factory-copy-activity-tutorial-using-azure-portal.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline mit Kopieraktivität mithilfe des Data Factory-Editors im Azure-Portal. |
| 17 | [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline mit Kopieraktivität mithilfe der Azure PowerShell. |
| 18 | [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe von Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | In diesem Tutorial erstellen Sie mithilfe von Visual Studio eine Azure Data Factory-Pipeline mit Kopieraktivität. |
| 19 | [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | In diesem Tutorial erfahren Sie, wie Sie die Kopieraktivität in einer Azure Data Factory-Pipeline verwenden, um Daten aus Blob Storage in SQL-Datenbank zu kopieren. |
| 20 | [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Data Factory-Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md) | In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline mit einer Kopieraktivität, indem Sie den von der Data Factory unterstützten Kopier-Assistenten verwenden. |



## Datenverschiebung

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 21 | [Verschieben von Daten in einen und aus einem Azure-Blob mithilfe von Azure Data Factory](data-factory-azure-blob-connector.md) | Hier erfahren Sie, wie Sie Blobdaten in Azure Data Factory kopieren. Verwenden Sie unser Beispiel zum Kopieren von Daten aus Azure-Blobspeicher in die Azure SQL-Datenbank (und umgekehrt). |
| 22 | [Verschieben von Daten in und aus Azure Data Lake-Speicher mithilfe von Azure Data Factory](data-factory-azure-datalake-connector.md) | Erfahren Sie, wie Daten mithilfe von Azure Data Factory in und aus Azure Data Lake-Speicher verschoben werden. |
| 23 | [Verschieben von Daten in und aus DocumentDB mithilfe von Azure Data Factory](data-factory-azure-documentdb-connector.md) | Erfahren Sie, wie Daten mithilfe von Azure Data Factory in und aus Azure DocumentDB verschoben werden. |
| 24 | [Verschieben von Daten in und aus Azure SQL-Datenbank mithilfe von Azure Data Factory](data-factory-azure-sql-connector.md) | Erfahren Sie, wie Daten mithilfe von Azure Data Factory in und aus Azure SQL-Datenbank verschoben werden. |
| 25 | [Verschieben von Daten in und aus Azure SQL Data Warehouse mithilfe von Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) | Erfahren Sie, wie Daten mithilfe von Azure Data Factory in und aus Azure SQL Data Warehouse verschoben werden. |
| 26 | [Verschieben von Daten in eine und aus einer Azure-Tabelle mithilfe von Azure Data Factory](data-factory-azure-table-connector.md) | Erfahren Sie, wie Daten mithilfe von Azure Data Factory in einen und aus einem Azure-Tabellenspeicher verschoben werden. |
| 27 | [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) | Erfahren Sie mehr zu wichtigen Faktoren, die sich auf die Leistung der Datenverschiebung in Azure Data Factory über die Kopieraktivität auswirken. |
| 28 | [Verschieben von Daten mit der Kopieraktivität](data-factory-data-movement-activities.md) | Informieren Sie sich über das Verschieben von Daten in Data Factory-Pipelines: Datenmigration zwischen Cloudspeichern, zwischen lokalen Speicherorten und der Cloud. Verwenden Sie die Kopieraktivität. |
| 29 | [Versionshinweise für Datenverwaltungsgateway](data-factory-gateway-release-notes.md) | Versionshinweise für Datenverwaltungsgateway |
| 30 | [Verschieben von Daten aus einem lokalem HDFS mithilfe von Azure Data Factory](data-factory-hdfs-connector.md) | Erfahren Sie, wie Sie Daten aus einem lokalem HDFS mithilfe von Azure Data Factory verschieben. |
| 31 | [Überwachen und Verwalten von Azure Data Factory-Pipelines mit der neuen App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md) | Informationen zum Verwenden der App „Überwachung und Verwaltung“ für Azure Data Factorys und Pipelines. |
| 32 | [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) | Richten Sie ein Datengateway ein, um Daten zwischen dem lokalen Speicher und der Cloud zu verschieben. Verwenden Sie das Datenverwaltungsgateway in Azure Data Factory zum Verschieben Ihrer Daten. |
| 33 | [Verschieben von Daten aus einer OData-Quelle mithilfe von Azure Data Factory](data-factory-odata-connector.md) | Erfahren Sie, wie Sie Daten aus OData-Quellen mithilfe von Azure Data Factory verschieben. |
| 34 | [Verschieben von Daten aus ODBC-Datenspeichern mithilfe von Azure Data Factory](data-factory-odbc-connector.md) | Erfahren Sie, wie Sie Daten aus ODBC-Datenspeichern mithilfe von Azure Data Factory verschieben. |
| 35 | [Verschieben von Daten aus DB2 mithilfe von Azure Data Factory](data-factory-onprem-db2-connector.md) | Erfahren Sie, wie Sie Daten aus der DB2-Datenbank mithilfe von Azure Data Factory verschieben. |
| 36 | [Verschieben von Daten in das lokale und aus dem lokalen Dateisystem mithilfe von Azure Data Factory](data-factory-onprem-file-system-connector.md) | Erfahren Sie, wie Daten mithilfe von Azure Data Factory in das und aus dem lokalen Dateisystem verschoben werden. |
| 37 | [Verschieben von Daten aus MySQL mithilfe von Azure Data Factory](data-factory-onprem-mysql-connector.md) | Erfahren Sie, wie Sie Daten aus der MySQL-Datenbank mithilfe von Azure Data Factory verschieben. |
| 38 | [Verschieben von Daten in lokales/aus lokalem Oracle mithilfe von Azure Data Factory](data-factory-onprem-oracle-connector.md) | Informationen zum Verschieben von Daten in und aus einer Oracle-Datenbank, die lokal oder mithilfe von Azure Data Factory gehostet wird. |
| 11,9 | [Verschieben von Daten aus PostgreSQL mithilfe von Azure Data Factory ](data-factory-onprem-postgresql-connector.md) | Erfahren Sie, wie Sie Daten aus der PostgreSQL-Datenbank mithilfe von Azure Data Factory verschieben. |
| 40 | [Verschieben von Daten aus Sybase mithilfe von Azure Data Factory](data-factory-onprem-sybase-connector.md) | Erfahren Sie, wie Sie Daten aus der Sybase-Datenbank mithilfe von Azure Data Factory verschieben. |
| 41 | [Verschieben von Daten aus Teradate mithilfe von Azure Data Factory](data-factory-onprem-teradata-connector.md) | Informationen zum Teradata-Connector für den Data Factory-Dienst, mit dem Sie Daten aus einer Teradata-Datenbank verschieben können |
| 42 | [Verschieben von Daten in und aus SQL Server in einer lokalen oder IaaS-Umgebung (Azure-VM) mithilfe von Azure Data Factory](data-factory-sqlserver-connector.md) | Informationen zum Verschieben von Daten in und aus einer SQL Server-Datenbank, die lokal oder mithilfe von Azure Data Factory in einer Azure-VM gehostet wird. |
| 43 | [Verschieben von Daten aus einer Webtabelle mithilfe von Azure Data Factory](data-factory-web-table-connector.md) | Erfahren Sie, wie Sie Daten aus einer lokalen Tabelle mithilfe von Azure Data Factory auf eine Webseite verschieben. |



## Datentransformation

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 44 | [Erstellen von Vorhersagepipelines mithilfe von Azure Machine Learning-Aktivitäten](data-factory-azure-ml-batch-execution-activity.md) | Beschreibt das Erstellen von Vorhersagepipelines mithilfe von Azure Data Factory und Azure Machine Learning |
| 45 | [Verknüpfte Computedienste](data-factory-compute-linked-services.md) | Erfahren Sie mehr über Compute-Umgebungen, die in Azure Data Factory-Pipelines für die Transformation und Verarbeitung von Daten verwendet werden können. |
| 46 | [Verarbeiten umfangreicher Datasets mit Data Factory und Batch](data-factory-data-processing-using-batch.md) | Beschreibt, wie Sie große Datenmengen in einer Azure Data Factory-Pipeline verarbeiten, indem Sie die parallele Verarbeitung von Azure Batch nutzen. |
| 47 | [Transformieren und Analysieren von Daten in Azure Data Factory](data-factory-data-transformation-activities.md) | Hier finden Sie Informationen zur Datentransformation in Azure Data Factory. Transformieren und Verarbeiten Sie Daten in einem Azure HDInsight-Cluster oder in Azure Batch. |
| 48 | [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md) | Erfahren Sie, wie Sie die Hadoop-Streamingaktivität in Azure Data Factory verwenden können, um Hadoop-Streamingprogramme in einem bedarfsgesteuerten oder einem eigenen HDInsight-Cluster auszuführen. |
| 49 | [Hive-Aktivität](data-factory-hive-activity.md) | Erfahren Sie, wie Sie die Hive-Aktivität in Azure Data Factory verwenden können, um Hive-Abfragen in einem bedarfsgesteuerten/eigenen HDInsight-Cluster auszuführen. |
| 50 | [Aufrufen von MapReduce-Programmen über Data Factory](data-factory-map-reduce.md) | Erfahren Sie, wie Sie Daten verarbeiten, indem Sie MapReduce-Programme mithilfe einer Azure Data Factory auf einen Azure HDInsight-Cluster anwenden. |
| 51 | [Pig-Aktivität](data-factory-pig-activity.md) | Erfahren Sie, wie Sie die Pig-Aktivität in Azure Data Factory verwenden können, um Pig-Abfragen in einem bedarfsgesteuerten/eigenen HDInsight-Cluster auszuführen. |
| 52 | [Aufrufen von Spark-Programmen aus Data Factory](data-factory-spark.md) | Erfahren Sie, wie Sie Spark-Programme mithilfe der MapReduce-Aktivität aus einer Azure Data Factory aufrufen. |
| 53 | [SQL Server-Aktivität "Gespeicherte Prozedur"](data-factory-stored-proc-activity.md) | Informationen, wie Sie die SQL Server-Aktivität "Gespeicherte Prozedur" in einer Data Factory-Pipeline zum Aufrufen einer gespeicherten Prozedur in einer Azure SQL-Datenbank oder einem Azure SQL Data Warehouse verwenden können. |
| 54 | [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md) | Erfahren Sie, wie Sie benutzerdefinierte Aktivitäten erstellen und in einer Azure Data Factory-Pipeline verwenden. |
| 55 | [Ausführen eines U-SQL-Skripts auf Azure Data Lake Analytics in Azure Data Factory](data-factory-usql-activity.md) | Erläutert die Datenverarbeitung durch Ausführen eines U-SQL-Skripts mit einem Azure Data Lake Analytics-Computedienst. |



## Beispiele

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 56 | [Azure Data Factory Editor – Beispiele](data-factory-samples.md) | Bietet Informationen zu Beispielen, die zum Funktionsumfang des Azure Data Factory-Diensts gehören. |



## Anwendungsfälle

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 57 | [Fallstudien](data-factory-customer-case-studies.md) | Erfahren Sie, wie einige unserer Kunden Azure Data Factory verwendet haben. |
| 58 | [Anwendungsfall – Erstellen von Kundenprofilen](data-factory-customer-profiling-usecase.md) | Erfahren Sie, wie Azure Data Factory zum Erstellen eines datengesteuerten Workflows (Pipeline) verwendet wird, um Profile von Spielekunden zu erstellen. |
| 59 | [Anwendungsfall – Produktempfehlungen](data-factory-product-reco-usecase.md) | Informationen zu einem Anwendungsfall, der mithilfe von Azure Data Factory zusammen mit anderen Diensten implementiert wurde. |



## Überwachen und Verwalten

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 60 | [Überwachen und Verwalten von Azure Data Factory-Pipelines](data-factory-monitor-manage-pipelines.md) | Erfahren Sie, wie Sie von Ihnen erstellte Azure Data Factorys und Pipelines mithilfe des Azure-Portals und Azure PowerShell überwachen und verwalten. |



## SDK

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 61 | [Azure Data Factory – .NET-API-Änderungsprotokoll](data-factory-api-change-log.md) | Beschreibt Änderungen, hinzugefügte Features und Fehlerbehebungen usw. in einer bestimmten Version der .NET-API für Azure Data Factory. |
| 62 | [Erstellen, Überwachen und Verwalten von Azure Data Factorys mithilfe des Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Erfahren Sie, wie Sie Azure Data Factorys mithilfe des Data Factory .NET SDK programmgesteuert erstellen, überwachen und verwalten. |
| 63 | [Azure Data Factory-Entwicklerreferenz](data-factory-sdks.md) | Lernen Sie die verschiedenen Möglichkeiten zum Erstellen, Überwachen und Verwalten von Azure Data Factorys kennen. |



## Verschiedenes

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 64 | [Azure Data Factory – Häufig gestellte Fragen](data-factory-faq.md) | Häufig gestellte Fragen zu Azure Data Factory |
| 65 | [Azure Data Factory – Funktionen und Systemvariablen](data-factory-functions-variables.md) | Enthält eine Liste der Funktionen und Systemvariablen von Azure Data Factory. |
| 66 | [Azure Data Factory – Benennungsregeln](data-factory-naming-rules.md) | Beschreibt die Benennungsregeln für Data Factory-Entitäten. |
| 67 | [Problembehandlung bei Data Factory](data-factory-troubleshoot.md) | Erfahren Sie, wie Sie Probleme mithilfe von Azure Data Factory beheben. |

<!---HONumber=AcomDC_0914_2016-->