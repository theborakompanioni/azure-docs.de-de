<properties 
	pageTitle="Data Factory – .NET API-Änderungsprotokoll | Microsoft Azure" 
	description="Beschreibt Änderungen, hinzugefügte Features und Fehlerbehebungen usw. in einer bestimmten Version der .NET-API für Azure Data Factory." 
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
	ms.date="09/21/2016" 
	ms.author="spelluru"/>

# Azure Data Factory – .NET-API-Änderungsprotokoll 
Dieser Artikel enthält Informationen zu Änderungen am Azure Data Factory SDK einer bestimmten Version. [Hier](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) finden Sie das neueste NuGet-Paket für Azure Data Factory.

## Version 4.11.0
Hinzugefügte Features:

- Die folgenden verknüpften Diensttypen wurden hinzugefügt:
	- [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
	- [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
	- [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Die folgenden Dataset-Typen wurden hinzugefügt:
	- [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
	- [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Die folgenden Typen von Kopierquellen wurden hinzugefügt:
	- [MongoDbSource](https://msdn.microsoft.com/de-DE/library/mt765123.aspx)

## Version 4.10.0
- Die folgenden optionalen Eigenschaften wurden zu TextFormat hinzugefügt:
	- [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
	- [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
	- [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Die folgenden verknüpften Diensttypen wurden hinzugefügt:
	- [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
	- [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Die folgenden Dataset-Typen wurden hinzugefügt:
	- [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Die folgenden Typen von Kopierquellen wurden hinzugefügt:
	- [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- Hinzufügen der Eigenschaft [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) zu AzureMLBatchExecutionActivity
	- Ermöglicht das Übergeben mehrerer Webdiensteingaben an ein Azure Machine Learning-Experiment


## Version 4.9.1

### Fehlerbehebung

- Ausmusterung der WebApi-basierten Authentifizierung für [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx)

## Version 4.9.0

### Hinzugefügte Features

- Hinzufügen der Eigenschaften [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) und [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) zu „CopyActivity“ Ausführliche Informationen zu diesem Feature finden Sie unter [Gestaffeltes Kopieren](data-factory-copy-activity-performance.md#staged-copy).


### Fehlerbehebung

- Führen Sie eine Überladung der [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx)-Methode hinzu, die eine [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx)-Instanz akzeptiert.
- Markieren Sie [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) und [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) in „CopySink“ als optional.

## Version 4.8.0

### Hinzugefügte Features
- Dem Kopieraktivitätstyp wurden die folgenden optionalen Eigenschaften hinzugefügt, um die Optimierung der Kopierleistung zu ermöglichen:
	- [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
	- [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## Version 4.7.0

### Hinzugefügte Features
* Ein neuer StorageFormat-Typ vom Typ [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) wurde hinzugefügt, um Dateien im einspaltig optimierten Zeilenformat (ORC) zu kopieren.
* Hinzufügen von [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx)- und PolyBaseSettings-Eigenschaften zu „SqlDWSink“.
    * Ermöglicht die Verwendung von PolyBase zum Kopieren von Daten in SQL Data Warehouse.

## Version 4.6.1

### Fehlerbehebungen
* Repariert die HTTP-Anforderung zum Auflisten von Aktivitätsfenstern.
    * Entfernt den Ressourcengruppennamen und den Data Factory-Namen von der Anforderungsnutzlast.

## Version 4.6.0

### Hinzugefügte Features

- Die folgenden Eigenschaften wurden zu [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx) hinzugefügt:
	- [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
	- [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
	- [Datasets](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- Die folgenden Eigenschaften wurden zu [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx) hinzugefügt:
	- [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Ein neuer [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx)-Typ vom Typ [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) wurde hinzugefügt, um Datasets zu definieren, deren Daten im JSON-Format vorliegen.

## Version 4.5.0

### Hinzugefügte Features
* [Listenvorgänge für Aktivitätsfenster](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx) wurden hinzugefügt.
    * Methoden zum Abrufen von Aktivitätsfenstern mit auf den Entitätstypen basierenden Filtern wurden hinzugefügt (d.h. Data Factorys, Datasets, Pipelines und Aktivitäten).
* Die folgenden verknüpften Diensttypen wurden hinzugefügt:
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Die folgenden Dataset-Typen wurden hinzugefügt:
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Die folgenden Typen von Kopierquellen wurden hinzugefügt:
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## Version 4.4.0

### Hinzugefügte Features

- Der folgende Typ für verknüpfte Dienste wurde als Datenquellen und Senken für Kopieraktivitäten hinzugefügt:
	- [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Grundlegende Informationen und Beispiele finden Sie unter [Mit Azure Storage SAS verknüpfter Dienst](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service).

## Version 4.3.0

### Hinzugefügte Features

- Die folgenden Typen für verknüpfte Dienste wurden als Datenquellen und Senken für Kopieraktivitäten hinzugefügt:
	- [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Grundlegende Informationen und Beispiele finden Sie unter [Verschieben von Daten aus einem lokalen HDFS mithilfe von Azure Data Factory](data-factory-hdfs-connector.md).
	- [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Grundlegende Informationen und Beispiele finden Sie unter [Verschieben von Daten aus ODBC-Datenspeichern mithilfe von Azure Data Factory](data-factory-odbc-connector.md).

## Version 4.2.0

### Hinzugefügte Features

- Der folgende neue Aktivitätstyp wurde hinzugefügt: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Einzelheiten zu dieser Aktivität finden Sie unter [Aktualisieren von Azure ML-Modellen mithilfe der Ressourcenaktualisierungsaktivität](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Die neue optionale Eigenschaft [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) wurde der Klasse [AzureMLLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx) hinzugefügt.
- Die Eigenschaften [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) und [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) wurden der Klasse [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) hinzugefügt.
- Erlauben Sie die Konfiguration von Timeouts bei Clientaufrufen für den Data Factory-Dienst.


## Version 4.1.0

### Hinzugefügte Features
* Die folgenden verknüpften Diensttypen wurden hinzugefügt:
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Die folgenden Aktivitätstypen wurden hinzugefügt:
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Die folgenden Dataset-Typen wurden hinzugefügt:
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Die folgenden Quellen- und Senkentypen wurden für die Kopieraktivität hinzugefügt:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## Version 4.0.1

### Wichtige Änderungen
Die folgenden Klassen wurden umbenannt. Die neuen Namen entsprechen den ursprünglichen Namen der Klassen vor Version 4.0.0.
 
Name in 4.0.0 | Name in 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## Version 4.0.0

### Wichtige Änderungen



- Die folgenden Klassen/Schnittstellen wurden umbenannt.

| Alter Name | Neuer Name |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Table | [Dataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- Die **List-**-Methoden geben jetzt ausgelagerte Ergebnisse zurück. Wenn die Antwort eine nicht leere **NextLink**-Eigenschaft enthält, muss die Clientanwendung das Abrufen der nächsten Seite fortsetzen, bis alle Seiten zurückgegeben wurden. Beispiel:

		PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
	    var pipelines = new List<Pipeline>(response.Pipelines);
	
	    string nextLink = response.NextLink;
	    while (string.IsNullOrEmpty(response.NextLink))
	    {
	        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
	        pipelines.AddRange(nextResponse.Pipelines);
	
	        nextLink = nextResponse.NextLink;
	    }
	
- Die **List**-Pipeline-API gibt nur die Zusammenfassung einer Pipeline anstatt alle Details zurück. Beispielsweise enthalten Aktivitäten in einer Pipelinezusammenfassung nur Name und Typ.

### Hinzugefügte Features
- Die [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx)-Klasse unterstützt die beiden neuen Eigenschaften **SliceIdentifierColumnName** und **SqlWriterCleanupScript** zur Unterstützung einer idempotenten Kopie in Azure SQL Data Warehouse. Im Artikel [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) finden Sie insbesondere in den Abschnitten [Mechanismus 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) und [Mechanismus 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) weitere Informationen zu diesen Eigenschaften.

- Wir unterstützen jetzt im Rahmen der Kopieraktivität das Ausführen einer gespeicherten Prozedur für Azure SQL-Datenbank- und Azure SQL Data Warehouse-Datenquellen. Die Klassen [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) und [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) verfügen über die folgenden Eigenschaften: **SqlReaderStoredProcedureName** und **StoredProcedureParameters**. In den Artikeln [Azure SQL-Datenbank](data-factory-azure-sql-connector.md#sqlsource) und [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) auf Azure.com finden Sie ausführliche Informationen zu diesen Eigenschaften.

<!---HONumber=AcomDC_0921_2016-->