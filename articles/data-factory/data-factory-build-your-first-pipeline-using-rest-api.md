<properties
	pageTitle="Erstellen der ersten Data Factory (REST) | Microsoft Azure"
	description="In diesem Tutorial erstellen Sie eine Azure Data Factory-Beispielpipeline mit der Data Factory-REST-API."
	services="data-factory"
	documentationCenter=""
	authors="spelluru"
	manager="jhubbard"
	editor="monicar"
/>

<tags
	ms.service="data-factory"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="08/16/2016"
	ms.author="spelluru"/>

# Erstellen der ersten Azure Data Factory mit der Data Factory-REST-API
> [AZURE.SELECTOR]
- [Übersicht über das Tutorial](data-factory-build-your-first-pipeline.md)
- [Verwenden des Data Factory-Editors](data-factory-build-your-first-pipeline-using-editor.md)
- [Verwenden von PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Verwenden von Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Verwenden der Resource Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [Verwenden der REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Artikel erfahren Sie, wie Sie mithilfe der Data Factory-REST-API Ihre erste Azure Data Factory erstellen.

## Voraussetzungen

- Lesen Sie den Artikel [Übersicht über das Tutorial](data-factory-build-your-first-pipeline.md). In diesem Artikel werden die Grundlagen von Azure Data Factory beschrieben.
- Installieren Sie [Curl](https://curl.haxx.se/dlwiz/) auf Ihrem Computer. Sie können das Curl-Tool mit REST-Befehlen zum Erstellen einer Data Factory verwenden.
- Befolgen Sie die Anleitung in [diesem Artikel](../resource-group-create-service-principal-portal.md), um Folgendes durchzuführen:
	1. Erstellen Sie eine Webanwendung mit dem Namen **ADFGetStartedApp** in Azure Active Directory.
	2. Beschaffen Sie die **Client-ID** und den **geheimen Schlüssel**.
	3. Beschaffen Sie die **Mandanten-ID**.
	4. Weisen Sie die Anwendung **ADFGetStartedApp** der Rolle **Data Factory-Mitwirkender** zu.
- Installieren Sie [Azure PowerShell](../powershell-install-configure.md).
- Starten Sie **PowerShell**, und führen Sie den folgenden Befehl aus. Lassen Sie Azure PowerShell bis zum Ende dieses Tutorials geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.
	1. Führen Sie **Login-AzureRmAccount** aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben.
	2. Führen Sie **Get-AzureRmSubscription** aus, um alle Abonnements für dieses Konto anzuzeigen.
	3. Führen Sie „Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription“ aus.| Set-AzureRmContext** to select the subscription that you want to work with. Replace **NameOfAzureSubscription** with the name of your Azure subscription. 
3. Erstellen Sie eine Azure-Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup**, indem Sie in der PowerShell den folgenden Befehl ausführen.

		New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

	Bei einigen Schritten dieses Lernprogramms wird davon ausgegangen, dass Sie die Ressourcengruppe namens ADFTutorialResourceGroup verwenden. Bei Verwendung einer anderen Ressourcengruppe müssen Sie den Namen Ihrer Ressourcengruppe anstelle von ADFTutorialResourceGroup in diesem Tutorial verwenden.

## Erstellen von JSON-Definitionen
Erstellen Sie die folgenden JSON-Dateien in dem Ordner, in dem sich die Datei „curl.exe“ befindet.

### datafactory.json 
> [AZURE.IMPORTANT] Der Name muss global eindeutig sein, sodass sich für „ADFCopyTutorialDF“ die Verwendung eines Präfixes oder Suffixes anbietet, um die Eindeutigkeit sicherzustellen.

	{  
	    "name": "FirstDataFactoryREST",  
	    "location": "WestUS"
	}  

### azurestoragelinkedservice.json
> [AZURE.IMPORTANT] Ersetzen Sie **accountname** und **accountkey** durch den Namen bzw. Schlüssel Ihres Azure-Speicherkontos. Informationen zum Abrufen Ihres Speicherzugriffsschlüssels finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

	{
	    "name": "AzureStorageLinkedService",
	    "properties": {
	        "type": "AzureStorage",
	        "typeProperties": {
	            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	        }
	    }
	}


### hdinsightondemandlinkedservice.json

	{
		"name": "HDInsightOnDemandLinkedService",
		"properties": {
			"type": "HDInsightOnDemand",
			"typeProperties": {
				"version": "3.2",
				"clusterSize": 1,
				"timeToLive": "00:30:00",
				"linkedServiceName": "AzureStorageLinkedService"
			}
		}
	}

Die folgende Tabelle enthält eine Beschreibung der JSON-Eigenschaften, die im Codeausschnitt verwendet werden:

| Eigenschaft | Beschreibung |
| :------- | :---------- |
| Version | Gibt die Version des zu erstellenden HDInsight-Clusters als 3.2 an. | 
| ClusterSize | Gibt die Größe des HDInsight-Clusters an. | 
| TimeToLive | Gibt die Leerlaufzeit des HDInsight-Clusters an, bevor er gelöscht wird. |
| linkedServiceName | Gibt das Speicherkonto an, das verwendet wird, um die von HDInsight generierten Protokolle zu speichern. |

Beachten Sie Folgendes:

- Die Data Factory erstellt mit dem obigen JSON-Code einen **Windows-basierten** HDInsight-Cluster für Sie. Sie können sich auch für die Erstellung eines **Linux-basierten** HDInsight-Clusters entscheiden. Ausführliche Informationen finden Sie unter [Bedarfsgesteuerter verknüpfter HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
- Anstelle eines bedarfsgesteuerten HDInsight-Clusters könnten Sie **Ihren eigenen HDInsight-Cluster** verwenden. Ausführliche Informationen finden Sie unter [Verknüpfter HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
- Der HDInsight-Cluster erstellt einen **Standardcontainer** im Blobspeicher, den Sie im JSON-Code angegeben haben (**linkedServiceName**). HDInsight löscht diesen Container nicht, wenn der Cluster gelöscht wird. Dieses Verhalten ist beabsichtigt. Beim bedarfsgesteuerten verknüpften HDInsight-Dienst wird jedes Mal ein HDInsight-Cluster erstellt, wenn ein Slice verarbeitet werden muss. Dies gilt nur dann nicht, wenn ein aktiver Cluster (**timeToLive**) vorhanden ist und nach Abschluss der Verarbeitung gelöscht wird.

	Wenn mehr Segmente verarbeitet werden, werden in Ihrem Azure-Blobspeicher viele Container angezeigt. Falls Sie diese für die Problembehandlung der Aufträge nicht benötigen, sollten Sie sie ggf. löschen, um die Speicherkosten zu verringern. Die Namen dieser Container basieren auf dem folgenden Muster: „adf**IhrDataFactoryName**-**NameVerknüpfterDienst**-Datums-/Uhrzeitstempel“. Verwenden Sie Tools wie [Microsoft Storage-Explorer](http://storageexplorer.com/), um Container in Ihrem Azure-Blobspeicher zu löschen.

Ausführliche Informationen finden Sie unter [Bedarfsgesteuerter verknüpfter HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).

### inputdataset.json

	{
		"name": "AzureBlobInput",
		"properties": {
			"type": "AzureBlob",
			"linkedServiceName": "AzureStorageLinkedService",
			"typeProperties": {
				"fileName": "input.log",
				"folderPath": "adfgetstarted/inputdata",
				"format": {
					"type": "TextFormat",
					"columnDelimiter": ","
				}
			},
			"availability": {
				"frequency": "Month",
				"interval": 1
			},
			"external": true,
			"policy": {}
		}
	}


Im JSON-Code wird ein Dataset mit dem Namen **AzureBlobInput** definiert, das Eingabedaten für eine Aktivität in der Pipeline darstellt. Darüber hinaus geben Sie an, dass die Eingabedaten im Blobcontainer **adfgetstarted** und im Ordner **inputdata** gespeichert werden.

Die folgende Tabelle enthält eine Beschreibung der JSON-Eigenschaften, die im Codeausschnitt verwendet werden:

| Eigenschaft | Beschreibung |
| :------- | :---------- |
| Typ | Die Type-Eigenschaft wird auf „AzureBlob“ festgelegt, da sich Daten im Azure-Blobspeicher befinden. |  
| linkedServiceName | verweist auf den „StorageLinkedService“, den Sie zuvor erstellt haben. |
| fileName | Diese Eigenschaft ist optional. Wenn Sie diese Eigenschaft nicht angeben, werden alle Dateien in „folderPath“ übernommen. In diesem Fall wird nur „input.log“ verarbeitet. |
| Typ | Da die Protokolldateien im Textformat vorliegen, verwenden wir „TextFormat“. | 
| columnDelimiter | Spalten werden in den Protokolldateien per Komma (,) voneinander getrennt. |
| frequency/interval | „frequency“ wird auf „Month“ und „interval“ auf „1“ festgelegt, was bedeutet, dass die Eingabeslices monatlich verfügbar sind. | 
| external | Diese Eigenschaft wird auf „true“ festgelegt, wenn die Eingabedaten nicht vom Data Factory-Dienst generiert werden. | 

### outputdataset.json

	{
		"name": "AzureBlobOutput",
		"properties": {
			"type": "AzureBlob",
			"linkedServiceName": "AzureStorageLinkedService",
			"typeProperties": {
				"folderPath": "adfgetstarted/partitioneddata",
				"format": {
					"type": "TextFormat",
					"columnDelimiter": ","
				}
			},
			"availability": {
				"frequency": "Month",
				"interval": 1
			}
		}
	}

Im JSON-Code wird ein Dataset mit dem Namen **AzureBlobOutput** definiert, das Ausgabedaten für eine Aktivität in der Pipeline darstellt. Darüber hinaus geben Sie an, dass die Ergebnisse im Blobcontainer **adfgetstarted** und im Ordner **partitioneddata** gespeichert werden. Der Abschnitt **availability** gibt an, dass das Ausgabe-DataSet monatlich erzeugt wird.

### pipeline.json
> [AZURE.IMPORTANT] Ersetzen Sie **storageaccountname** durch den Namen Ihres Azure-Speicherkontos.


	{
		"name": "MyFirstPipeline",
		"properties": {
			"description": "My first Azure Data Factory pipeline",
			"activities": [{
				"type": "HDInsightHive",
				"typeProperties": {
					"scriptPath": "adfgetstarted/script/partitionweblogs.hql",
					"scriptLinkedService": "AzureStorageLinkedService",
					"defines": {
						"inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
						"partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
					}
				},
				"inputs": [{
					"name": "AzureBlobInput"
				}],
				"outputs": [{
					"name": "AzureBlobOutput"
				}],
				"policy": {
					"concurrency": 1,
					"retry": 3
				},
				"scheduler": {
					"frequency": "Month",
					"interval": 1
				},
				"name": "RunSampleHiveActivity",
				"linkedServiceName": "HDInsightOnDemandLinkedService"
			}],
			"start": "2016-07-10T00:00:00Z",
			"end": "2016-07-11T00:00:00Z",
			"isPaused": false
		}
	}

Im JSON-Codeausschnitt erstellen Sie eine Pipeline, die aus einer einzelnen Aktivität besteht. Diese nutzt Hive, um Daten in einem HDInsight-Cluster zu verarbeiten.

Die Hive-Skriptdatei **partitionweblogs.hql** ist im Azure-Speicherkonto (das durch den scriptLinkedService-Dienst namens **StorageLinkedService** angegeben ist) und im Ordner **script** im Container **adfgetstarted** gespeichert.

Der Abschnitt **defines** dient zum Angeben der Laufzeiteinstellungen, die als Hive-Konfigurationswerte (z.B. „${hiveconf:inputtable}“, „${hiveconf:partitionedtable}“) an das Hive-Skript übergeben werden.

Die Eigenschaften **start** und **end** der Pipeline geben den aktiven Zeitraum der Pipeline an.

Im JSON-Code der Aktivität geben Sie an, dass das Hive-Skript auf der Computeinstanz ausgeführt wird, die vom **linkedServiceName** – **HDInsightOnDemandLinkedService** angegeben wurde.

> [AZURE.NOTE] Einzelheiten zu JSON-Eigenschaften, die im Beispiel oben verwendet wurden, finden Sie unter [Anatomie einer Pipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline).

## Festlegen von globalen Variablen

Führen Sie in Azure PowerShell die folgenden Befehle aus, nachdem Sie die Werte durch Ihre eigenen Werte ersetzt haben:

> [AZURE.IMPORTANT] Informationen zur Beschaffung von Client-ID, geheimem Clientschlüssel, Mandanten-ID und Abonnement-ID finden Sie im Abschnitt [Voraussetzungen](#prerequisites).

	$client_id = "<client ID of application in AAD>"
	$client_secret = "<client key of application in AAD>"
	$tenant = "<Azure tenant ID>";
	$subscription_id="<Azure subscription ID>";

	$rg = "ADFTutorialResourceGroup"
	$adf = "FirstDataFactoryREST"



## Authentifizieren mit AAD

	$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
	$responseToken = Invoke-Command -scriptblock $cmd;
	$accessToken = (ConvertFrom-Json $responseToken).access_token;
	
	(ConvertFrom-Json $responseToken) 



## Erstellen einer Data Factory

In diesem Schritt erstellen Sie eine Azure Data Factory mit dem Namen **FirstDataFactoryREST**. Eine Data Factory kann eine oder mehrere Aktivitäten aufweisen. Eine Pipeline kann eine oder mehrere Aktivitäten aufweisen. Beispielsweise eine Kopieraktivität zum Kopieren von Daten aus einer Quelle in einen Zieldatenspeicher und eine HDInsight-Hive-Aktivität zum Ausführen eines Hive-Skripts zum Transformieren von Eingabedaten in Produktausgabedaten. Führen Sie die folgenden Befehle zum Erstellen der Data Factory aus:

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.

	Stellen Sie sicher, dass der hier angegebene Name der Data Factory (ADFCopyTutorialDF) mit dem Namen übereinstimmt, der in der Datei **datafactory.json** angegeben ist.

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung der Data Factory erfolgreich war, wird der JSON-Code für die Data Factory unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.

		$results

Beachten Sie Folgendes:
 
- Der Name der Azure Data Factory muss global eindeutig sein. Sollte der Fehler **Der Data Factory-Name „FirstDataFactoryREST“ ist nicht verfügbar** angezeigt werden, gehen Sie wie folgt vor:
	1. Ändern Sie den Namen in der Datei **datafactory.json** (beispielsweise in „<IhrName>FirstDataFactoryREST“). Im Thema [Data Factory – Benennungsregeln](data-factory-naming-rules.md) finden Sie Benennungsregeln für Data Factory-Artefakte.
	2. Ersetzen Sie im ersten Befehl an der Stelle, an der der Variablen **$cmd** ein Wert zugewiesen wird, „FirstDataFactoryREST“ durch den neuen Namen, und führen Sie den Befehl aus.
	3. Führen Sie die nächsten beiden Befehle zum Aufrufen der REST-API aus, um die Data Factory zu erstellen und die Ergebnisse des Vorgangs auszugeben.
- Data Factory-Instanzen können nur von Mitwirkenden/Administratoren des Azure-Abonnements erstellt werden.
- Der Name der Data Factory kann in Zukunft als DNS-Name registriert und so öffentlich sichtbar werden.
- Führen Sie einen der folgenden Schritte aus, wenn Sie eine Fehlermeldung wie **Dieses Abonnement ist nicht zur Verwendung des Microsoft.DataFactory-Namespaces registriert** erhalten, und versuchen Sie, die Veröffentlichung erneut durchzuführen:

	- Führen Sie in Azure PowerShell den folgenden Befehl aus, um den Data Factory-Anbieter zu registrieren.
		
			Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
	
		Sie können den folgenden Befehl ausführen, um sich zu vergewissern, dass der Data Factory-Anbieter registriert ist.
	
			Get-AzureRmResourceProvider
	- Melden Sie sich mit dem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu einem Data Factory-Blatt, oder erstellen Sie eine Data Factory im Azure-Portal. Mit dieser Aktion wird der Anbieter automatisch für Sie registriert.

Vor dem Erstellen einer Pipeline müssen Sie zunächst einige Data Factory-Entitäten erstellen. Zuerst erstellen Sie verknüpfte Dienste zum Verknüpfen von Datenspeichern/Berechnungen mit Ihrem Datenspeicher, definieren die Datasets für Ein- und Ausgabe, um Daten in verknüpften Datenspeichern darzustellen, und erstellen dann die Pipeline mit einer Aktivität, die diese Datasets verwendet.

## Erstellen von verknüpften Diensten 
In diesem Schritt verknüpfen Sie Ihr Azure Storage-Konto und einen bedarfsgesteuerten Azure HDInsight-Cluster mit Ihrer Data Factory. Das Azure Storage-Konto enthält in diesem Beispiel die Ein- und Ausgabedaten für die Pipeline. Der verknüpfte HDInsight-Dienst wird verwendet, um das in der Aktivität der Pipeline in diesem Beispiel angegebene Hive-Skript auszuführen.

### Erstellen des mit Azure Storage verknüpften Diensts
In diesem Schritt verknüpfen Sie Ihr Azure Storage-Konto mit Ihrer Data Factory. In diesem Tutorial verwenden Sie das gleiche Azure Storage-Konto, um Ein-/Ausgabedaten und die HQL-Skriptdatei zu speichern.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.
 
		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des verknüpften Diensts erfolgreich war, wird der JSON-Code für den verknüpften Dienst unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.
  
		$results

### Erstellen des mit Azure-HDInsight verknüpften Diensts
In diesem Schritt verknüpfen Sie einen bedarfsgesteuerten HDInsight-Cluster mit Ihrer Data Factory. Der HDInsight-Cluster wird automatisch zur Laufzeit erstellt und gelöscht, nachdem die Verarbeitung abgeschlossen und die angegebene Leerlaufzeit verstrichen ist. Anstelle eines bedarfsgesteuerten HDInsight-Clusters könnten Sie Ihren eigenen HDInsight-Cluster verwenden. Weitere Informationen finden Sie unter [Verknüpfte Computedienste](data-factory-compute-linked-services.md).

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des verknüpften Diensts erfolgreich war, wird der JSON-Code für den verknüpften Dienst unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.

		$results

## Erstellen von Datasets
In diesem Schritt erstellen Sie Datasets, um die Eingabe- und Ausgabedaten für die Hive-Verarbeitung darzustellen. Diese Datasets verweisen auf den **StorageLinkedService**, den Sie zuvor in diesem Tutorial erstellt haben. Der verknüpfte Dienst weist auf ein Azure Storage-Konto, und Datasets geben Container, Ordner und Dateiname in dem Speicher an, der Eingabe- und Ausgabedaten enthält.

### Erstellen eines Eingabedatasets
In diesem Schritt erstellen Sie das Eingabedataset, das die im Azure-Blobspeicher gespeicherten Eingabedaten darstellt.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des Datasets erfolgreich war, wird der JSON-Code für das Dataset unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.
  
		$results
### Erstellen des Ausgabedatasets
In diesem Schritt erstellen Sie das Ausgabedataset, das die im Azure-Blobspeicher gespeicherten Ausgabedaten darstellt.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des Datasets erfolgreich war, wird der JSON-Code für das Dataset unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.
  
		$results 

## Erstellen der Pipeline
In diesem Schritt erstellen Sie Ihre erste Pipeline mit einer **HDInsightHive**-Aktivität. Der Eingabeslice ist monatlich verfügbar („frequency“: „Month“, „interval“: „1“), der Ausgabeslice wird monatlich erstellt, und die scheduler-Eigenschaft für die Aktivität ist ebenfalls auf „Monatlich“ (siehe unten) festgelegt. Die Einstellungen für das Ausgabedataset und den Aktivitätsplaner müssen übereinstimmen. Derzeit steuert das Ausgabedataset den Zeitplan, sodass Sie auch dann ein Ausgabedataset erstellen müssen, wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität keine Eingabe akzeptiert, können Sie das Erstellen des Eingabedatasets überspringen.

Vergewissern Sie sich, dass Sie die Datei **input.log** im Ordner **adfgetstarted/inputdata** im Azure-Blobspeicher sehen, und führen Sie den folgenden Befehl aus, um die Pipeline bereitzustellen. Da die Zeiten für **start** und **end** in der Vergangenheit festgelegt sind, und **isPaused** auf „false“ festgelegt ist, wird die Pipeline (Aktivität in der Pipeline) sofort nach der Bereitstellung ausgeführt.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des Datasets erfolgreich war, wird der JSON-Code für das Dataset unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.

		$results
5. Glückwunsch, Sie haben mit Azure PowerShell Ihre erste Pipeline erfolgreich erstellt!

## Überwachen der Pipeline
In diesem Schritt verwenden Sie die Data Factory-REST-API zum Überwachen von Slices, die von der Pipeline erstellt werden.

	$ds ="AzureBlobOutput"

	$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

	$results2 = Invoke-Command -scriptblock $cmd;

	IF ((ConvertFrom-Json $results2).value -ne $NULL) {
	    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
	} else {
    	    (convertFrom-Json $results2).RemoteException
	}

Führen Sie „Invoke-Command“ und den darauffolgenden Befehl aus, bis für den Slice der Status **Bereit** oder **Fehler** angezeigt wird. Sobald der Slice den Status „Bereit“ hat, überprüfen Sie, ob die Ausgabedaten sich in Ihrem Blobspeicher im Ordner **partitioneddata** im Container **adfgetstarted** befinden. Beachten Sie, dass die bedarfsgesteuerte Erstellung eines HDInsight-Clusters in der Regel einige Zeit dauert.

![Ausgabedaten](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Die Eingabedatei wird bei erfolgreicher Verarbeitung des Slice gelöscht. Wenn Sie den Slice erneut ausführen oder das Tutorial nochmals durchgehen möchten, laden Sie die Eingabedatei (input.log) daher in den Ordner „inputdata“ des Containers „adfgetstarted“ hoch.

Sie können das Azure-Portal auch zum Überwachen von Slices und Durchführen der Problembehandlung verwenden. Informationen hierzu finden Sie unter [Überwachen von Pipelines mit dem Azure-Portal](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline).

## Zusammenfassung 
In diesem Tutorial haben Sie eine Azure Data Factory zum Verarbeiten von Daten erstellt, indem Sie ein Hive-Skript in einem HDInsight Hadoop-Cluster ausgeführt haben. Sie haben den Data Factory-Editor im Azure-Portal verwendet, um die folgenden Schritte auszuführen:

1.	Sie haben eine Azure **Data Factory** erstellt.
2.	Sie haben zwei **verknüpfte Dienste** erstellt:
	1.	Einen verknüpften **Azure Storage**-Dienst zum Verknüpfen Ihres Azure-Blobspeichers, in dem die Eingabe- und Ausgabedateien der Data Factory enthalten sind.
	2.	Einen bedarfsgesteuerten verknüpften **Azure HDInsight**-Dienst zum Verknüpfen eines bedarfsgesteuerten HDInsight Hadoop-Clusters mit der Data Factory. Azure Data Factory erstellt einen HDInsight Hadoop-Cluster „just in time“, um Eingabedaten zu verarbeiten und Ausgabedaten zu erzeugen.
3.	Sie haben zwei **Datasets** erstellt, in denen Eingabe- und Ausgabedaten für eine HDInsight Hive-Aktivität in der Pipeline beschrieben werden.
4.	Sie haben eine **Pipeline** mit einer **HDInsight Hive**-Aktivität erstellt.

## Nächste Schritte
In diesem Artikel haben Sie eine Pipeline mit einer Transformationsaktivität (HDInsight-Aktivität) erstellt, die ein Hive-Skript in einem bedarfsgesteuerten Azure HDInsight-Cluster ausführt. Informationen zum Verwenden einer Kopieraktivität zum Kopieren von Daten aus einem Azure-Blob in Azure SQL finden Sie unter [Lernprogramm: Kopieren von Daten aus einem Azure-Blob in Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## Weitere Informationen
| Thema | Beschreibung |
| :---- | :---- |
| [Referenz zur Data Factory-REST-API](https://msdn.microsoft.com/library/azure/dn906738.aspx) | Umfassende Dokumentation zu Data Factory-Cmdlets |
| [Transformationsaktivitäten von Daten](data-factory-data-transformation-activities.md) | Dieser Artikel enthält eine Liste mit Transformationsaktivitäten von Daten (z.B. die HDInsight Hive-Transformation, die Sie in diesem Tutorial verwendet haben), die von Azure Data Factory unterstützt werden. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | In diesem Artikel werden die Planungs- und Ausführungsaspekte des Azure Data Factory-Anwendungsmodells erläutert. |
| [Pipelines](data-factory-create-pipelines.md) | In diesem Artikel erhalten Sie Informationen zu Pipelines und Aktivitäten in Azure Data Factory und erfahren, wie diese zum Erstellen datengesteuerter End-to-End-Workflows für Ihr Szenario oder Ihr Unternehmen genutzt werden können. |
| [Datasets](data-factory-create-datasets.md) | Dieser Artikel enthält Informationen zu Datasets in Azure Data Factory.
| [Überwachen und Verwalten von Pipelines über Blätter im Azure-Portal](data-factory-monitor-manage-pipelines.md) | In diesem Artikel wird das Überwachen, Verwalten und Debuggen Ihrer Pipelines über Blätter im Azure-Portal beschrieben. |
| [Überwachen und Verwalten von Pipelines mit der Überwachungs-App](data-factory-monitor-manage-app.md) | In diesem Artikel wird das Überwachen, Verwalten und Debuggen von Pipelines mit der App für die Überwachung und Verwaltung beschrieben. 

<!---HONumber=AcomDC_0824_2016-->