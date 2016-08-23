<properties 
	pageTitle="Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der REST-API" 
	description="In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline mit Kopieraktivität mithilfe der REST-API." 
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
	ms.topic="get-started-article" 
	ms.date="08/17/2016" 
	ms.author="spelluru"/>

# Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der REST-API
> [AZURE.SELECTOR]
- [Übersicht über das Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Verwenden des Data Factory-Editors](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Mithilfe von PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Verwenden von Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Verwenden der REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Verwenden des Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md)

In diesem Tutorial wird veranschaulicht, wie Sie eine Azure Data Factory mit der REST-API erstellen und überwachen. Die Pipeline in der Data Factory verwendet eine Kopieraktivität zum Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank.

Die Kopieraktivität führt die Datenverschiebung in Azure Data Factory durch, und die Aktivität wird von einem global verfügbaren Dienst gestützt, mit dem Daten zwischen verschiedenen Datenspeichern auf sichere, zuverlässige und skalierbare Weise kopiert werden können. Ausführliche Informationen zur Kopieraktivität finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

> [AZURE.NOTE] 
In diesem Artikel werden nicht alle Komponenten der Data Factory-REST-API behandelt. In der [Data Factory REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn906738.aspx) finden Sie eine umfassende Dokumentation zu Data Factory-Cmdlets.
  

## Voraussetzungen

- Lesen Sie sich die [Übersicht über das Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) durch.
- Installieren Sie [Curl](https://curl.haxx.se/dlwiz/) auf Ihrem Computer. Sie können das Curl-Tool mit REST-Befehlen zum Erstellen einer Data Factory verwenden.
- Befolgen Sie die Anleitung in [diesem Artikel](../resource-group-create-service-principal-portal.md), um Folgendes durchzuführen:
	1. Erstellen Sie eine Webanwendung mit dem Namen **ADFCopyTutorialApp** in Azure Active Directory.
	2. Beschaffen Sie die **Client-ID** und den **geheimen Schlüssel**.
	3. Beschaffen Sie die **Mandanten-ID**.
	4. Weisen Sie die Anwendung **ADFCopyTutorialApp** der Rolle **Data Factory-Mitwirkender** zu.
- Installieren Sie [Azure PowerShell](../powershell-install-configure.md).
- Starten Sie **PowerShell**, und führen Sie den folgenden Befehl aus. Lassen Sie Azure PowerShell bis zum Ende dieses Tutorials geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.
	1. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:
	
			Login-AzureRmAccount   
	2. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

			Get-AzureRmSubscription 
	3. Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **&lt;NameOfAzureSubscription&gt;** durch den Namen Ihres Azure-Abonnements.

			Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
	1. Erstellen Sie eine Azure-Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup**, indem Sie in PowerShell den folgenden Befehl ausführen.

			New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

		Falls die Ressourcengruppe bereits vorhanden ist, können Sie angeben, ob sie aktualisiert (J) oder beibehalten (N) werden soll.

		Bei einigen Schritten dieses Lernprogramms wird davon ausgegangen, dass Sie die Ressourcengruppe namens ADFTutorialResourceGroup verwenden. Bei Verwendung einer anderen Ressourcengruppe müssen Sie den Namen Ihrer Ressourcengruppe anstelle von ADFTutorialResourceGroup in diesem Tutorial verwenden.
  
## Erstellen von JSON-Definitionen
Erstellen Sie die folgenden JSON-Dateien in dem Ordner, in dem sich die Datei „curl.exe“ befindet.

### datafactory.json 
> [AZURE.IMPORTANT] Der Name muss global eindeutig sein, sodass sich für „ADFCopyTutorialDF“ die Verwendung eines Präfixes oder Suffixes anbietet, um die Eindeutigkeit sicherzustellen.

	{  
	    "name": "ADFCopyTutorialDF",  
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

### azuresqllinkedservice.json
> [AZURE.IMPORTANT] Ersetzen Sie **servername**, **databasename**, **username** und **password** durch den Namen Ihres Azure SQL-Servers, den Namen der SQL-Datenbank, das Benutzerkonto bzw. das Kennwort für das Konto.

	{
	    "name": "AzureSqlLinkedService",
	    "properties": {
	        "type": "AzureSqlDatabase",
	        "description": "",
	        "typeProperties": {
	            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
	        }
	    }
	}


### inputdataset.json

	{
	  "name": "AzureBlobInput",
	  "properties": {
	    "structure": [
	      {
	        "name": "FirstName",
	        "type": "String"
	      },
	      {
	        "name": "LastName",
	        "type": "String"
	      }
	    ],
	    "type": "AzureBlob",
	    "linkedServiceName": "AzureStorageLinkedService",
	    "typeProperties": {
	      "folderPath": "adftutorial/",
	      "fileName": "emp.txt",
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": ","
	      }
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

Mit der JSON-Definition wird ein Dataset mit dem Namen **AzureBlobInput** definiert, das Eingabedaten für eine Aktivität in der Pipeline darstellt. Darüber hinaus wird angegeben, dass die Eingabedaten in der Datei **emp.txt** enthalten sind, die sich im Blobcontainer **adftutorial** befindet.

 Beachten Sie Folgendes:

- **dataset type** ist auf **AzureBlob** festgelegt.
- **linkedServiceName** ist auf **AzureStorageLinkedService** festgelegt.
- **folderPath** ist auf den Container **adftutorial** und **fileName** auf **emp.txt** festgelegt.
- **format: type** ist auf **TextFormat** festgelegt.
- Die Textdatei enthält die beiden Felder **FirstName** und **LastName**, die durch ein Komma getrennt sind (**columnDelimiter**).
- Die Verfügbarkeit (**availability**) ist auf **hourly** festgelegt („frequency“ auf „hour“ und „interval“ auf „1“), sodass der Data Factory-Dienst den Stammordner im angegebenen Blobcontainer (**adftutorial**) stündlich auf Eingabedaten untersucht.

Wenn Sie keinen Dateinamen (**fileName**) für ein Eingabedataset angeben, werden alle Dateien/Blobs aus dem Eingabeordner (**folderPath**) als Eingaben angesehen. Wenn Sie einen Dateinamen im JSON-Code angeben, wird nur die angegebene Datei bzw. das Blob als Eingabe betrachtet.

Wenn Sie **fileName** für eine **Ausgabetabelle** nicht angeben, werden die generierten Dateien in **folderPath** im folgenden Format benannt: Data.&lt;GUID&gt;.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

Um **folderPath** und **fileName** dynamisch basierend auf der **SliceStart**-Zeit festzulegen, verwenden Sie die **partitionedBy**-Eigenschaft. Im folgenden Beispiel verwendet folderPath die Angaben für Jahr, Monat und Tag aus „SliceStart“ (Startzeit des zu verarbeitenden Slices) und „fileName“ die Angabe für Stunde aus „SliceStart“. Wenn beispielsweise ein Slice für den Zeitpunkt "2014-10-20T08:00:00" erzeugt wird, wird "folderName" auf "wikidatagateway/wikisampledataout/2014/10/20" und "filName" auf "08.csv" festgelegt.

  	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
    	{ "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],


### outputdataset.json
	
	{
	  "name": "AzureSqlOutput",
	  "properties": {
	    "structure": [
	      {
	        "name": "FirstName",
	        "type": "String"
	      },
	      {
	        "name": "LastName",
	        "type": "String"
	      }
	    ],
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "emp"
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}


Mit der JSON-Definition wird ein Dataset mit dem Namen **AzureSqlOutput** definiert, das Ausgabedaten für eine Aktivität in der Pipeline darstellt. Darüber hinaus wird angegeben, dass die Ergebnisse in der Tabelle **emp** in der Datenbank gespeichert werden, die von „AzureSqlLinkedService“ dargestellt wird. Im Abschnitt **availability** wird angegeben, dass das Ausgabedataset stündlich erstellt wird („frequency: hour“ und „interval: 1“).

Beachten Sie Folgendes:

- **dataset type** ist auf **AzureSQLTable** festgelegt.
- **linkedServiceName** ist auf **AzureSqlLinkedService** festgelegt.
- **tablename** ist auf **emp** festgelegt.
- Die Tabelle "emp" der Datenbank enthält die drei Spalten **ID**, **FirstName** und **LastName**. "ID" ist jedoch eine Identitätsspalte, weshalb Sie hier nur **FirstName** und **LastName** angeben müssen.
- Die Verfügbarkeit (**availability**) ist auf **hourly**, (**frequency** auf **hour** und **interval** auf **1**) festgelegt. Der Data Factory-Dienst generiert in der Tabelle **emp** in der Azure SQL-Datenbank stündlich einen Ausgabedatenslice.

### pipeline.json

	{
	  "name": "ADFTutorialPipeline",
	  "properties": {
	    "description": "Copy data from a blob to Azure SQL table",
	    "activities": [
	      {
	        "name": "CopyFromBlobToSQL",
	        "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureBlobInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureSqlOutput"
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


Beachten Sie Folgendes:

- Der Abschnitt "activities" enthält nur eine Aktivität, deren **type** auf **CopyActivity** festgelegt ist.
- Die Eingabe für die Aktivität ist auf **AzureBlobInput** und die Ausgabe für die Aktivität ist auf **AzureSqlOutput** festgelegt.
- Im Abschnitt **transformation** ist **BlobSource** als Quelltyp und **SqlSink** als Senkentyp angegeben.

Ersetzen Sie den Wert der **start** Eigenschaft durch den aktuellen Tag und den der **End**-Eigenschaft durch den nächsten Tag. Sie können auch nur den Datumsteil angeben und den Uhrzeitteil überspringen. "2015-02-03" entspricht z. B. "2015-02-03T00:00:00Z"

Die Start- und Endzeit von Datums-/Uhrzeitangaben müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601) angegeben werden. Beispiel: 2014-10-14T16:32:41Z. Die Zeitangabe **end** ist optional, wird aber in diesem Tutorial verwendet.

Wenn für die **end**-Eigenschaft kein Wert angegeben wird, wird sie als „**start + 48 Stunden**“ berechnet. Um die Pipeline auf unbestimmte Zeit auszuführen, geben Sie als Wert für die **end**-Eigenschaft **9999-09-09** an.

Im obigen Beispiel ergeben sich 24 Datenslices, da jede Stunde ein Datenslice erstellt wird.
	
> [AZURE.NOTE] Einzelheiten zu JSON-Eigenschaften, die im Beispiel oben verwendet wurden, finden Sie unter [Anatomie einer Pipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline).

## Festlegen von globalen Variablen

Führen Sie in Azure PowerShell die folgenden Befehle aus, nachdem Sie die Werte durch Ihre eigenen Werte ersetzt haben:

> [AZURE.IMPORTANT] Informationen zur Beschaffung von Client-ID, geheimem Clientschlüssel, Mandanten-ID und Abonnement-ID finden Sie im Abschnitt [Voraussetzungen](#prerequisites).

	$client_id = "<client ID of application in AAD>"
	$client_secret = "<client key of application in AAD>"
	$tenant = "<Azure tenant ID>";
	$subscription_id="<Azure subscription ID>";

	$rg = "ADFTutorialResourceGroup"
	$adf = "ADFCopyTutorialDF"

## Authentifizieren mit AAD 
Führen Sie den folgenden Befehl aus, um die Authentifizierung für Azure Active Directory (AAD) durchzuführen.

	$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
	$responseToken = Invoke-Command -scriptblock $cmd;
	$accessToken = (ConvertFrom-Json $responseToken).access_token;
	
	(ConvertFrom-Json $responseToken) 

## Erstellen einer Data Factory

In diesem Schritt erstellen Sie eine Azure Data Factory mit dem Namen **ADFCopyTutorialDF**. Eine Data Factory kann eine oder mehrere Aktivitäten aufweisen. Eine Pipeline kann eine oder mehrere Aktivitäten aufweisen. Beispielsweise eine Kopieraktivität zum Kopieren von Daten aus einer Quelle in einen Zieldatenspeicher und eine HDInsight-Hive-Aktivität zum Ausführen eines Hive-Skripts zum Transformieren von Eingabedaten in Produktausgabedaten. Führen Sie die folgenden Befehle zum Erstellen der Data Factory aus:

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.

	Stellen Sie sicher, dass der hier angegebene Name der Data Factory (ADFCopyTutorialDF) mit dem Namen übereinstimmt, der in der Datei **datafactory.json** angegeben ist.

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung der Data Factory erfolgreich war, wird der JSON-Code für die Data Factory unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.

		$results

Beachten Sie Folgendes:
 
- Der Name der Azure Data Factory muss global eindeutig sein. Gehen Sie bei Anzeige des Fehlers **Der Data Factory-Name „ADFCopyTutorialDF“ ist nicht verfügbar** wie folgt vor:
	1. Ändern Sie den Namen (z.B. „ihrnameADFCopyTutorialDF“) in der Datei **datafactory.json**.
	2. Ersetzen Sie im ersten Befehl an der Stelle, an der der Variablen **$cmd** ein Wert zugewiesen wird, „ADFCopyTutorialDF“ durch den neuen Namen, und führen Sie den Befehl aus.
	3. Führen Sie die nächsten beiden Befehle zum Aufrufen der REST-API aus, um die Data Factory zu erstellen und die Ergebnisse des Vorgangs auszugeben.
	
	Im Thema [Data Factory – Benennungsregeln](data-factory-naming-rules.md) finden Sie Benennungsregeln für Data Factory-Artefakte.
- Um Data Factory-Instanzen erstellen zu können, müssen Sie unter dem Azure-Abonnement ein Mitwirkender oder Administrator sein.
- Der Name der Data Factory kann in Zukunft als DNS-Name registriert und so öffentlich sichtbar werden.
- Führen Sie einen der folgenden Schritte aus, wenn Sie eine Fehlermeldung wie **Dieses Abonnement ist nicht zur Verwendung des Microsoft.DataFactory-Namespaces registriert** erhalten, und versuchen Sie, die Veröffentlichung erneut durchzuführen:

	- Führen Sie in Azure PowerShell den folgenden Befehl aus, um den Data Factory-Anbieter zu registrieren.
		
			Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
	
		Sie können den folgenden Befehl ausführen, um sicherzustellen, dass der Data Factory-Anbieter registriert ist.
	
			Get-AzureRmResourceProvider
	- Melden Sie sich mit dem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu einem Data Factory-Blatt, oder erstellen Sie eine Data Factory im Azure-Portal. Mit dieser Aktion wird der Anbieter automatisch für Sie registriert.

Vor dem Erstellen einer Pipeline müssen Sie zunächst einige Data Factory-Entitäten erstellen. Zuerst erstellen Sie verknüpfte Dienste zum Verknüpfen von Quell- und Zieldatenspeichern mit Ihrem Datenspeicher, definieren die Datasets für Ein- und Ausgabe, um Daten in verknüpften Datenspeichern darzustellen, und erstellen dann die Pipeline mit einer Aktivität, die diese Datasets verwendet.

## Erstellen von verknüpften Diensten
Verknüpfte Dienste verknüpfen Datenspeicher oder Serverdienste mit einer Azure Data Factory. Ein Datenspeicher kann ein Azure-Speicher, eine Azure SQL-Datenbank oder eine lokale SQL Server-Datenbank sein, die Eingabedaten enthält oder die Ausgabedaten für eine Data Factory-Pipeline speichert. Ein Serverdienst ist der Dienst, der Eingabedaten verarbeitet und Ausgabedaten erzeugt.

In diesem Schritt erstellen Sie zwei verknüpfte Dienste: **AzureStorageLinkedService** und **AzureSqlLinkedService**. Der verknüpfte Dienst „AzureStorageLinkedService“ verknüpft ein Azure Storage-Konto, und der Dienst „AzureSqlLinkedService“ eine Azure SQL-Datenbank mit der Data Factory **ADFCopyTutorialDF**. Später in diesem Tutorial erstellen Sie eine Pipeline, die Daten aus einem Blobcontainer in „AzureStorageLinkedService“ in eine SQL-Tabelle in „AzureSqlLinkedService“ kopiert.

### Erstellen des mit Azure Storage verknüpften Diensts
In diesem Schritt verknüpfen Sie Ihr Azure Storage-Konto mit Ihrer Data Factory. In diesem Tutorial verwenden Sie das Azure Storage-Konto zum Speichern der Eingabedaten.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.
 
		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des verknüpften Diensts erfolgreich war, wird der JSON-Code für den verknüpften Dienst unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.
  
		$results

### Erstellen eines mit Azure SQL verknüpften Diensts
In diesem Schritt verknüpfen Sie die Azure SQL-Datenbank mit Ihrer Data Factory. In diesem Tutorial verwenden Sie die gleiche Azure SQL-Datenbank zum Speichern der Ausgabedaten.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.
 
		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des verknüpften Diensts erfolgreich war, wird der JSON-Code für den verknüpften Dienst unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.
  
		$results

## Erstellen von Datasets

Im vorherigen Schritt haben Sie die verknüpften Dienste **AzureStorageLinkedService** und **AzureSqlLinkedService** erstellt, um ein Azure Storage-Konto und eine Azure SQL-Datenbank mit der Data Factory **ADFCopyTutorialDF** zu verknüpfen. In diesem Schritt erstellen Sie Datasets mit den Eingabe- und Ausgabedaten für die Kopieraktivität in der Pipeline, die Sie im nächsten Schritt erstellen.

Das Eingabedataset in diesem Tutorial verweist auf den Blobcontainer im Azure-Speicher, auf den „AzureStorageLinkedService“ verweist. Das Ausgabedataset verweist auf die SQL-Tabelle in der Azure SQL-Datenbank, auf die „AzureSqlLinkedService“ verweist.

### Vorbereiten von Azure Blob-Speicher und Azure SQL-Datenbank für das Lernprogramm
Führen Sie die folgenden Schritte zur Vorbereitung des Azure-Blobspeichers und der Azure SQL-Datenbank für dieses Tutorial aus.
 
* Erstellen Sie im Azure-Blobspeicher einen Blobcontainer namens **adftutorial**, auf den **AzureStorageLinkedService** verweist.
* Erstellen Sie die Textdatei **emp.txt**, und laden Sie diese als Blob in den Container **adftutorial** hoch.
* Erstellen Sie eine Tabelle namens **emp** in der Azure SQL-Datenbank, auf die **AzureSqlLinkedService** verweist.


1. Öffnen Sie den Editor, fügen Sie den folgenden Text ein, und speichern Sie die Datei als **emp.txt** im Ordner **C:\\ADFGetStartedPSH** auf Ihrer Festplatte.

        John, Doe
		Jane, Doe
				
2. Verwenden Sie Tools wie [Azure-Speicher-Explorer](https://azurestorageexplorer.codeplex.com/) zum Erstellen des Containers **adftutorial** und zum Hochladen der Datei **emp.txt** in den Container.

    ![Azure-Speicher-Explorer](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Verwenden Sie das folgende SQL-Skript, um die Tabelle **emp** in der Azure SQL-Datenbank zu erstellen.


        CREATE TABLE dbo.emp 
		(
			ID int IDENTITY(1,1) NOT NULL,
			FirstName varchar(50),
			LastName varchar(50),
		)
		GO

		CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

	Wenn Sie SQL Server 2014 auf Ihrem Computer installiert haben: Befolgen Sie die Anweisungen unter [Schritt 2: Herstellen einer Verbindung mit der SQL-Datenbank][sql-management-studio] im Artikel "Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio", um eine Verbindung mit Ihrem Azure SQL-Server herzustellen und das SQL-Skript auszuführen.

	Wenn Sie Visual Studio 2013 auf Ihrem Computer installiert haben: Klicken Sie im Azure-Portal ([http://portal.azure.com](http://portal.sazure.com)) links auf den Hub **Durchsuchen** und dann auf **SQL-Server**. Wählen Sie Ihre Datenbank aus, und klicken Sie auf der Symbolleiste auf die Schaltfläche **In Visual Studio öffnen**, um eine Verbindung mit Ihrem Azure SQL-Server herzustellen und das SQL-Skript auszuführen. Wenn Ihr Client nicht auf den Azure SQL-Server zugreifen darf, müssen Sie die Firewall für Ihren Azure SQL-Server so konfigurieren, dass der Zugriff über Ihren Computer (bzw. dessen IP-Adresse) ermöglicht wird. Im vorherigen Artikel finden Sie Schritte zum Konfigurieren der Firewall für Ihren Azure SQL-Server.
		
### Erstellen eines Eingabedatasets 
In diesem Schritt erstellen Sie ein Dataset namens **AzureBlobInput**, das auf einen Blobcontainer im Azure-Speicher verweist. Dieser wird vom verknüpften Dienst **AzureStorageLinkedService** dargestellt. Dieser Blob-Container (**adftutorial**) enthält die Eingabedaten in der Datei **emp.txt**.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.

		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des Datasets erfolgreich war, wird der JSON-Code für das Dataset unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.
  
		$results

### Erstellen des Ausgabedatasets
In diesem Teil des Schritts erstellen Sie eine Ausgabetabelle namens **AzureSqlOutput**, die auf eine SQL-Tabelle (**emp**) in der Azure SQL-Datenbank verweist. Diese wird durch den verknüpften Dienst **AzureSqlLinkedService** dargestellt. Die Pipeline kopiert Daten aus dem Eingabeblob in die Tabelle **emp**.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des Datasets erfolgreich war, wird der JSON-Code für das Dataset unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.
  
		$results 

## Erstellen der Pipeline
In diesem Schritt erstellen Sie eine Pipeline mit einer **Kopieraktivität**, für die **AzureBlobInput** als Eingabe und **AzureSqlOutput** als Ausgabe verwendet wird.

1. Weisen Sie den Befehl einer Variablen mit dem Namen **cmd** zu.
 
		$cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Führen Sie den Befehl mithilfe von **Invoke-Command** aus.

		$results = Invoke-Command -scriptblock $cmd;
3. Zeigen Sie die Ergebnisse an. Wenn die Erstellung des Datasets erfolgreich war, wird der JSON-Code für das Dataset unter **Ergebnisse** angezeigt. Andernfalls sehen Sie eine Fehlermeldung.

		$results

**Glückwunsch!** Sie haben die Erstellung einer Azure Data Factory mit einer Pipeline, die Daten aus Azure Blob Storage in Azure SQL-Datenbank kopiert, erfolgreich abgeschlossen.

## Überwachen der Pipeline
In diesem Schritt verwenden Sie die Data Factory-REST-API zum Überwachen von Slices, die von der Pipeline erstellt werden.

	$ds ="AzureSqlOutput"

	$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

	$results2 = Invoke-Command -scriptblock $cmd;


	IF ((ConvertFrom-Json $results2).value -ne $NULL) {
	    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
	} else {
    	    (convertFrom-Json $results2).RemoteException
	}

Führen Sie diese Befehle aus, bis ein Slice mit dem Status **Bereit** oder **Fehler** angezeigt wird. Wenn sich der Slice im Status „Bereit“ befindet, können Sie die Tabelle **emp** in Ihrer Azure SQL-Datenbank auf Ausgabedaten prüfen.

Für jeden Slice werden zwei Zeilen mit Daten aus der Quelldatei in die Tabelle „emp“ in der Azure SQL-Datenbank kopiert. Aus diesem Grund sehen Sie in der Tabelle „emp“ 24 neue Datensätze, wenn alle Slices erfolgreich verarbeitet werden (Status „Bereit“).


## Zusammenfassung
In diesem Tutorial haben Sie mit der REST-API eine Azure Data Factory erstellt, um Daten aus einem Azure-Blob in eine Azure SQL-Datenbank zu kopieren. Nachfolgend sind die allgemeinen Schritte aufgeführt, die Sie in diesem Tutorial ausgeführt haben:

1.	Sie haben eine Azure **Data Factory** erstellt.
2.	Sie haben **verknüpfte Dienste** erstellt:
	1. Einen verknüpften Azure Storage-Dienst zum Verknüpfen Ihres Azure Storage-Kontos, in dem Eingabedaten enthalten sind.
	2. Einen verknüpften Azure SQL-Dienst zum Verknüpfen Ihrer Azure SQL-Datenbank, in der die Ausgabedaten enthalten sind.
3.	Sie haben **Datasets** erstellt, mit denen Eingabedaten und Ausgabedaten für Pipelines beschrieben werden.
4.	Sie haben eine **Pipeline** mit einer Kopieraktivität und „BlobSource“ als Quelle und „SqlSink“ als Senke erstellt.

## Weitere Informationen
| Thema | Beschreibung |
| :---- | :---- |
| [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) | Dieser Artikel enthält ausführliche Informationen zur Kopieraktivität, die Sie in diesem Tutorial verwendet haben. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | In diesem Artikel werden die Planungs- und Ausführungsaspekte des Azure Data Factory-Anwendungsmodells erläutert. |
| [Pipelines](data-factory-create-pipelines.md) | In diesem Artikel erhalten Sie Informationen zu Pipelines und Aktivitäten in Azure Data Factory und erfahren, wie diese zum Erstellen datengesteuerter End-to-End-Workflows für Ihr Szenario oder Ihr Unternehmen genutzt werden können. |
| [Datasets](data-factory-create-datasets.md) | Dieser Artikel enthält Informationen zu Datasets in Azure Data Factory.
| [Überwachen und Verwalten von Pipelines mit der Überwachungs-App](data-factory-monitor-manage-app.md) | In diesem Artikel wird das Überwachen, Verwalten und Debuggen von Pipelines mit der App für die Überwachung und Verwaltung beschrieben. 



[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: ../powershell-install-configure.md
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md
 

<!---HONumber=AcomDC_0817_2016-->