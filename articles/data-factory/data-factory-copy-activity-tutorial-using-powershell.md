<properties 
	pageTitle="Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der Azure PowerShell | Microsoft Azure" 
	description="In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline mit Kopieraktivität mithilfe der Azure PowerShell." 
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
	ms.date="08/01/2016" 
	ms.author="spelluru"/>

# Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe der Azure PowerShell
> [AZURE.SELECTOR]
- [Übersicht über das Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Verwenden des Data Factory-Editors](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Verwenden von PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Verwenden von Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Verwenden der REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Verwenden der .NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Verwenden des Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md)

Im Tutorial [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) erfahren Sie, wie Sie eine Azure Data Factory mit dem [Azure-Portal][azure-portal] erstellen und überwachen. In diesem Tutorial erstellen und überwachen Sie eine Azure Data Factory mithilfe von Azure PowerShell-Cmdlets. Die Pipeline in der Data Factory, die Sie in diesem Lernprogramm erstellen, verwendet eine Kopieraktivität zum Kopieren von Daten aus einem Azure-Blob in eine Azure SQL-Datenbank.

Die Kopieraktivität dient zum Verschieben von Daten in Azure Data Factory. Sie basiert auf einem global verfügbaren Dienst, mit dem Daten zwischen verschiedenen Datenspeichern sicher, zuverlässig und skalierbar kopiert werden können. Ausführliche Informationen zur Kopieraktivität finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

> [AZURE.IMPORTANT] 
Lesen Sie sich den Artikel [Übersicht über das Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) durch, und führen Sie die vorbereitenden Schritte aus, bevor Sie mit diesem Tutorial beginnen.
>   
> In diesem Artikel werden nicht alle Data Factory-Cmdlets behandelt. In der [Data Factory-Cmdlet-Referenz](https://msdn.microsoft.com/library/dn820234.aspx) finden Sie eine umfassende Dokumentation zu Data Factory-Cmdlets.
  

##Voraussetzungen
Neben den im Thema „Übersicht über das Tutorial“ aufgeführten vorausgesetzten Komponenten müssen Sie Folgendes durchführen bzw. installieren:

- **Azure PowerShell**. Befolgen Sie die Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) zum Installieren von Azure PowerShell auf Ihrem Computer.

##Dieses Lernprogramm umfasst folgende Punkte
In der folgenden Tabelle sind die in diesem Tutorial auszuführenden Schritte mit den dazugehörigen Beschreibungen aufgeführt.

Schritt | Beschreibung
-----| -----------
[Erstellen einer Azure Data Factory](#create-data-factory) | In diesem Schritt erstellen Sie eine Azure Data Factory mit dem Namen **ADFTutorialDataFactoryPSH**. 
[Erstellen von verknüpften Diensten](#create-linked-services) | In diesem Schritt erstellen Sie die beiden verknüpften Dienste **StorageLinkedService** und **AzureSqlLinkedService**. Der Dienst StorageLinkedService verbindet einen Azure-Speicher und der Dienst AzureSqlLinkedService die Azure SQL-Datenbank mit "ADFTutorialDataFactoryPSH".
[Erstellen von Eingabe- und Ausgabedatasets](#create-datasets) | In diesem Schritt definieren Sie zwei Datasets (**EmpTableFromBlob** und **EmpSQLTable**). Diese werden als Ein- und Ausgabetabellen für die **Kopieraktivität** in der ADFTutorialPipeline verwendet, die im nächsten Schritt erstellt wird.
[Erstellen und Ausführen einer Pipeline](#create-pipeline) | In diesem Schritt erstellen Sie die Pipeline **ADFTutorialPipeline** in der Data Factory **ADFTutorialDataFactoryPSH**. Die Pipeline weist eine **Kopieraktivität** auf, die Daten aus einem Azure-Blob in eine Ausgabedatenbanktabelle in Azure kopiert.
[Überwachen der Datasets und der Pipeline](#monitor-pipeline) | In diesem Schritt überwachen Sie die Datasets und die Pipeline mit Azure PowerShell.

## Erstellen einer Data Factory
In diesem Schritt erstellen Sie mit Azure PowerShell eine Azure Data Factory namens **ADFTutorialDataFactoryPSH**.

1. Starten Sie **PowerShell**, und führen Sie den folgenden Befehl aus. Lassen Sie Azure PowerShell bis zum Ende dieses Tutorials geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.
	1. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:
	
			Login-AzureRmAccount   
	2. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

			Get-AzureRmSubscription 
	3. Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **&lt;NameOfAzureSubscription&gt;** durch den Namen Ihres Azure-Abonnements.

			Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

3. Erstellen Sie eine Azure-Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup**, indem Sie den folgenden Befehl ausführen.
   
		New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

	Bei einigen Schritten dieses Lernprogramms wird davon ausgegangen, dass Sie die Ressourcengruppe namens **ADFTutorialResourceGroup** verwenden. Bei Verwendung einer anderen Ressourcengruppe müssen Sie diese anstelle der Ressourcengruppe ADFTutorialResourceGroup verwenden.
4. Führen Sie das Cmdlet **New-AzureRmDataFactory** zum Erstellen einer Data Factory mit dem Namen **ADFTutorialDataFactoryPSH** aus.

		New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"

	
Beachten Sie Folgendes:
 
- Der Name der Azure Data Factory muss global eindeutig sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen (beispielsweise in „<IhrName>ADFTutorialDataFactoryPSH“). Verwenden Sie diesen Namen beim Ausführen der Schritte in diesem Lernprogramm anstelle von "ADFTutorialDataFactoryPSH". Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Data Factory – Benennungsregeln](data-factory-naming-rules.md).
	
		Data factory name “ADFTutorialDataFactoryPSH” is not available
- Data Factory-Instanzen können nur von Mitwirkenden/Administratoren des Azure-Abonnements erstellt werden.
- Der Name der Data Factory kann in Zukunft als DNS-Name registriert und so öffentlich sichtbar werden.
- Führen Sie einen der folgenden Schritte aus, wenn Sie eine Fehlermeldung wie **Dieses Abonnement ist nicht zur Verwendung des Microsoft.DataFactory-Namespaces registriert** erhalten, und versuchen Sie, die Veröffentlichung erneut durchzuführen:

	- Führen Sie in Azure PowerShell den folgenden Befehl aus, um den Data Factory-Anbieter zu registrieren.
		
			Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
	
		Sie können den folgenden Befehl ausführen, um sich zu vergewissern, dass der Data Factory-Anbieter registriert ist.
	
			Get-AzureRmResourceProvider
	- Melden Sie sich mit dem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu einem Data Factory-Blatt, oder erstellen Sie eine Data Factory im Azure-Portal. Mit dieser Aktion wird der Anbieter automatisch für Sie registriert.

## Erstellen von verknüpften Diensten
Verknüpfte Dienste verknüpfen Datenspeicher oder Serverdienste mit einer Azure Data Factory. Ein Datenspeicher kann ein Azure-Speicher, eine Azure SQL-Datenbank oder eine lokale SQL Server-Datenbank sein, die Eingabedaten enthält oder die Ausgabedaten für eine Data Factory-Pipeline speichert. Ein Serverdienst ist der Dienst, der Eingabedaten verarbeitet und Ausgabedaten erzeugt.

In diesem Schritt erstellen Sie die beiden verknüpften Dienste **StorageLinkedService** und **AzureSqlLinkedService**. Der verknüpfte Dienst "StorageLinkedService" verknüpft ein Azure-Speicherkonto und der Dienst "AzureSqlLinkedService" eine Azure SQL-Datenbank mit der Data Factory **ADFTutorialDataFactoryPSH**. Später in diesem Tutorial erstellen Sie eine Pipeline, die Daten aus einem Blobcontainer in „StorageLinkedService“ in eine SQL-Tabelle in „AzureSqlLinkedService“ kopiert.

### Erstellen eines verknüpften Diensts für ein Azure-Speicherkonto
1.	Erstellen Sie in **C:\\ADFGetStartedPSH** eine JSON-Datei mit dem Namen **StorageLinkedService.json** mit folgendem Inhalt. Erstellen Sie den Ordner "ADFGetStartedPSH", falls dieser noch nicht vorhanden ist.

			{
		  		"name": "StorageLinkedService",
		  		"properties": {
	    			"type": "AzureStorage",
		    		"typeProperties": {
		      			"connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
		    		}
		  		}
			}

	Ersetzen Sie **accountname** und **accountkey** durch den Namen und den Schlüssel Ihres Azure-Speicherkontos.
2.	Wechseln Sie in **Azure PowerShell** zum Ordner **ADFGetStartedPSH**.
3.	Verwenden Sie das Cmdlet **New-AzureRmDataFactoryLinkedService**, um einen verknüpften Dienst zu erstellen. Für dieses Cmdlet und andere Data Factory-Cmdlets, die Sie in diesem Tutorial verwenden, ist das Eingeben von Werten für die Parameter **ResourceGroupName** und **DataFactoryName** erforderlich. Sie können auch **Get-AzureRmDataFactory** verwenden, um ein DataFactory-Objekt abzurufen und das Objekt ohne Eingabe von „ResourceGroupName“ und „DataFactoryName“ bei jeder Ausführung eines Cmdlets zu übergeben. Führen Sie folgenden Befehl aus, um die Ausgabe des Cmdlets **Get-AzureRmDataFactory** der Variablen **$df** zuzuweisen.

		$df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

4.	Führen Sie nun das Cmdlet **New-AzureRmDataFactoryLinkedService** zum Erstellen des verknüpften Diensts **StorageLinkedService** aus.

		New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

	Wenn Sie das Cmdlet **Get-AzureRmDataFactory** noch nicht ausgeführt und die Ausgabe nicht der Variablen **$df** zugewiesen hätten, müssten Sie für die Parameter „ResourceGroupName“ und „DataFactoryName“ die folgenden Werte angeben.
		
		New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

	Wenn Sie Azure PowerShell mitten im Tutorial schließen, müssen Sie das Cmdlet „Get-AzureRmDataFactory“ beim nächsten Start von Azure PowerShell ausführen, um das Tutorial abzuschließen.

### Erstellen eines verknüpften Diensts für eine Azure SQL-Datenbank
1.	Erstellen Sie die JSON-Datei "AzureSqlLinkedService.json" mit folgendem Inhalt.

			{
				"name": "AzureSqlLinkedService",
				"properties": {
					"type": "AzureSqlDatabase",
					"typeProperties": {
				      	"connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=user@server;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
					}
		  		}
			}

	Ersetzen Sie **servername**, **databasename**, **username@servername** und **password** durch die Namen von Azure SQL-Server, -Datenbank, -Benutzerkonto und -Kennwort.

2.	Führen Sie den folgenden Befehl aus, um einen verknüpften Dienst zu erstellen.
	
		New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json

	Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für Ihren Azure SQL-Server aktiviert ist. Führen Sie zum Prüfen und Aktivieren die folgenden Schritte aus:

	1. Klicken Sie links auf den Hub **DURCHSUCHEN** und dann auf **SQL-Server**.
	2. Wählen Sie Ihren Server aus, und klicken Sie auf dem Blatt "SQL SERVER" auf **EINSTELLUNGEN**.
	3. Klicken Sie auf dem Blatt **EINSTELLUNGEN** auf **Firewall**.
	4. Klicken Sie auf dem Blatt **Firewalleinstellungen** für **Zugriff auf Azure-Dienste erlauben** auf **EIN**.
	5. Klicken Sie links auf den Hub **AKTIV**, um zum Blatt **Data Factory** zu wechseln, auf dem Sie sich zuvor befunden haben.
	

## Erstellen von Datasets

Im vorherigen Schritt haben Sie die verknüpften Dienste **StorageLinkedService** und **AzureSqlLinkedService** erstellt, um ein Azure-Speicherkonto und eine Azure SQL-Datenbank mit der Data Factory **ADFTutorialDataFactoryPSH** zu verknüpfen. In diesem Schritt erstellen Sie Datasets mit den Eingabe- und Ausgabedaten für die Kopieraktivität in der Pipeline, die Sie im nächsten Schritt erstellen.

Eine Tabelle ist ein rechteckiges Dataset und die einzige Art von Dataset, die derzeit unterstützt wird. Die Eingabetabelle in diesem Tutorial bezieht sich auf einen Blobcontainer im Azure-Speicher, auf den „StorageLinkedService“ verweist. Die Ausgabetabelle bezieht sich auf eine SQL-Tabelle in der Azure SQL-Datenbank, auf die „AzureSqlLinkedService“ verweist.

### Vorbereiten von Azure Blob-Speicher und Azure SQL-Datenbank für das Lernprogramm
Überspringen Sie diesen Schritt, wenn Sie das Tutorial im Artikel [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) durchgeführt haben.

Führen Sie die folgenden Schritte zur Vorbereitung des Azure-Blobspeichers und der Azure SQL-Datenbank für dieses Lernprogramm aus.
 
* Erstellen Sie im Azure-Blobspeicher einen Blobcontainer namens **adftutorial**, auf den **StorageLinkedService** verweist.
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

	Wenn Sie SQL Server 2014 auf Ihrem Computer installiert haben: Befolgen Sie die Anweisungen unter [Schritt 2: Herstellen einer Verbindung mit der SQL-Datenbank](../sql-database/sql-database-manage-azure-ssms.md) im Artikel "Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio", um eine Verbindung mit Ihrem Azure SQL-Server herzustellen und das SQL-Skript auszuführen.

	Wenn Ihr Client nicht auf den Azure SQL-Server zugreifen darf, müssen Sie die Firewall für Ihren Azure SQL-Server so konfigurieren, dass der Zugriff über Ihren Computer (bzw. dessen IP-Adresse) ermöglicht wird. Schritte zum Konfigurieren der Firewall für Ihren Azure SQL-Server finden Sie in [diesem Artikel](../sql-database/sql-database-configure-firewall-settings.md).
		
### Erstellen eines Eingabedatasets 
Eine Tabelle ist ein rechteckiges Dataset und verfügt über ein Schema. In diesem Schritt erstellen Sie eine Tabelle namens **EmpBlobTable**. Sie verweist auf einen Blobcontainer in der Azure Storage-Instanz, die vom verknüpften Dienst **StorageLinkedService** dargestellt wird. Dieser Blob-Container (**adftutorial**) enthält die Eingabedaten in der Datei **emp.txt**.

1.	Erstellen Sie im Ordner **C:\\ADFGetStartedPSH** eine JSON-Datei mit dem Namen **EmpBlobTable.json** mit folgendem Inhalt:

			{
			  "name": "EmpTableFromBlob",
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
			    "linkedServiceName": "StorageLinkedService",
			    "typeProperties": {
				  "fileName": "emp.txt",
			      "folderPath": "adftutorial/",
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
	
	Beachten Sie Folgendes:
	
	- **dataset type** ist auf **AzureBlob** festgelegt.
	- **linkedServiceName** ist auf **StorageLinkedService** festgelegt.
	- **folderPath** ist auf den Container **adftutorial** festgelegt.
	- **fileName** hat den Wert **emp.txt**. Wenn Sie nicht den Namen des Blobs angeben, werden Daten aus allen Blobs im Container als Eingabedaten betrachtet.
	- **format: type** ist auf **TextFormat** festgelegt.
	- Die Textdatei enthält die beiden Felder **FirstName** und **LastName**, die durch ein Komma getrennt sind (**columnDelimiter**).
	- Die Verfügbarkeit (**availability**) ist auf stündlich (**hourly**) festgelegt (**frequency** auf **hour** und **interval** auf **1**). Der Data Factory-Dienst sucht also stündlich im Stammordner des angegebenen Blobcontainers (**adftutorial**) nach Eingabedaten.

	Wenn Sie keinen **fileName** für eine **Eingabe****tabelle** angeben, werden alle Dateien/Blobs aus dem Eingabeordner (**folderPath**) als Eingaben angesehen. Wenn Sie einen Dateinamen in der JSON-Datei angeben, wird nur die angegebene Datei/der angegebene Blob als Eingabe betrachtet.
 
	Wenn Sie keinen **fileName** für eine **Ausgabetabelle** angeben, werden die generierten Dateien in **folderPath** im folgenden Format benannt: Data.<Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).

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

	Informationen zu JSON-Eigenschaften finden Sie in der [JSON-Skriptreferenz](http://go.microsoft.com/fwlink/?LinkId=516971).

2.	Führen Sie den folgenden Befehl zum Erstellen des Data Factory-Datensatzes aus.

		New-AzureRmDataFactoryDataset $df -File .\EmpBlobTable.json

### Erstellen des Ausgabedatasets
In diesem Schritt erstellen Sie ein Ausgabedataset namens **EmpSQLTable**. Dieses Dataset verweist auf eine SQL-Tabelle (**emp**) in der durch **AzureSqlLinkedService** dargestellten Azure SQL-Datenbank. Die Pipeline kopiert Daten aus dem Eingabeblob in die Tabelle **emp**.

1.	Erstellen Sie im Ordner **C:\\ADFGetStartedPSH** eine JSON-Datei mit dem Namen **EmpSQLTable.json** mit folgendem Inhalt:
		
			{
			  "name": "EmpSQLTable",
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

     Beachten Sie Folgendes:
	
	* **dataset type** ist auf **AzureSQLTable** festgelegt.
	* **linkedServiceName** ist auf **AzureSqlLinkedService** festgelegt.
	* **tablename** ist auf **emp** festgelegt.
	* Die emp-Tabelle in der Datenbank enthält drei Spalten: **ID**, **FirstName** und **LastName**. Da es sich bei „ID“ um eine Identitätsspalte handelt, müssen Sie hier lediglich **FirstName** und **LastName** angeben.
	* Die Verfügbarkeit (**availability**) ist auf **hourly**, (**frequency** auf **hour** und **interval** auf **1**) festgelegt. Der Data Factory-Dienst generiert in der Tabelle **emp** in der Azure SQL-Datenbank stündlich einen Ausgabedatenslice.

2.	Führen Sie den folgenden Befehl zum Erstellen des Data Factory-Datensatzes aus.
	
		New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json


## Erstellen der Pipeline
In diesem Schritt erstellen Sie eine Pipeline mit einer **Kopieraktivität**, die **EmpTableFromBlob** als Eingabe und **EmpSQLTable** als Ausgabe verwendet.

1.	Erstellen Sie eine JSON-Datei mit dem Namen **ADFTutorialPipeline.json** im Ordner **C:\\ADFGetStartedPSH ** mit dem folgenden Inhalt:
	
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
			            "name": "EmpTableFromBlob"
			          }
			        ],
			        "outputs": [
			          {
			            "name": "EmpSQLTable"
			          }
			        ],
			        "typeProperties": {
			          "source": {
			            "type": "BlobSource"
			          },
			          "sink": {
			            "type": "SqlSink"
			          }
			        },
			        "Policy": {
			          "concurrency": 1,
			          "executionPriorityOrder": "NewestFirst",
			          "style": "StartOfInterval",
			          "retry": 0,
			          "timeout": "01:00:00"
			        }
			      }
			    ],
			    "start": "2016-08-09T00:00:00Z",
			    "end": "2016-08-10T00:00:00Z",
			    "isPaused": false
			  }
			}

	Beachten Sie Folgendes:

	- Der Abschnitt „Activities“ enthält nur eine Aktivität, deren **type** auf **Copy** festgelegt ist.
	- Die Eingabe für die Aktivität ist auf **EmpTableFromBlob** und die Ausgabe auf **EmpSQLTable** festgelegt.
	- Im Abschnitt **transformation** ist **BlobSource** als Quelltyp und **SqlSink** als Senkentyp angegeben.

	Ersetzen Sie den Wert der Eigenschaft **start** durch den aktuellen Tag und den Wert der Eigenschaft **end** durch den nächsten Tag. Die Start- und Endzeit von Datums-/Uhrzeitangaben müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601) angegeben werden. Beispiel: 2014-10-14T16:32:41Z. Die Zeitangabe **end** ist optional, wird aber in diesem Tutorial verwendet.
	
	Wenn für die **end**-Eigenschaft kein Wert angegeben wird, wird sie als „**start + 48 Stunden**“ berechnet. Um die Pipeline auf unbestimmte Zeit auszuführen, geben Sie als Wert für die **end**-Eigenschaft **9/9/9999** an.
	
	In diesem Beispiel ergeben sich 24 Datenslices, da jede Stunde ein Datenslice erstellt wird.
	
	Informationen zu JSON-Eigenschaften finden Sie in der [JSON-Skriptreferenz](http://go.microsoft.com/fwlink/?LinkId=516971).
2.	Führen Sie den folgenden Befehl zum Erstellen der Data Factory-Tabelle aus.
		
		New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json

**Glückwunsch!** Sie haben erfolgreich eine Azure Data Factory, verknüpfte Dienste, Tabellen und eine Pipeline erstellt und die Pipeline geplant.

## Überwachen der Pipeline
In diesem Schritt verwenden Sie Azure PowerShell zur Überwachung der Aktivitäten in einer Azure Data Factory.

1.	Führen Sie **Get-AzureRmDataFactory** aus, und weisen Sie die Ausgabe der Variablen „$df“ zu.

		$df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
 
2.	Führen Sie **Get-AzureRmDataFactorySlice** aus, um Details zu allen Slices in der Tabelle **EmpSQLTable** zu erhalten. Dies ist die Ausgabetabelle der Pipeline.

		Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T00:00:00

	Ersetzen Sie den Jahres-, Monats- und Datumsteil des Parameters **StartDateTime** durch aktuelle Werte für Jahr, Monat und Datum. Diese Einstellung muss mit dem Wert **Start** im JSON-Code der Pipeline übereinstimmen.

	Es sollten 24 Slices angezeigt werden (einer für jede Stunde von 12:00 Uhr des aktuellen Tages bis 12:00 Uhr des nächsten Tages).
	
	**Erster Slice:**

		ResourceGroupName : ADFTutorialResourceGroup
		DataFactoryName   : ADFTutorialDataFactoryPSH
		TableName         : EmpSQLTable
		Start             : 3/3/2015 12:00:00 AM
		End               : 3/3/2015 1:00:00 AM
		RetryCount        : 0
		Status            : Waiting
		LatencyStatus     :
		LongRetryCount    : 0

	**Letzter Slice:**

		ResourceGroupName : ADFTutorialResourceGroup
		DataFactoryName   : ADFTutorialDataFactoryPSH
		TableName         : EmpSQLTable
		Start             : 3/4/2015 11:00:00 PM
		End               : 3/4/2015 12:00:00 AM
		RetryCount        : 0
		Status            : Waiting
		LatencyStatus     : 
		LongRetryCount    : 0

3.	Führen Sie **Get-AzureRmDataFactoryRun** aus, um die Details der Aktivitätsausführungen für einen **bestimmten** Slice abzurufen. Ändern Sie den Wert des Parameters **StartDateTime**, sodass er dem Zeitwert **Start** für den Slice aus der Ausgabe entspricht. Der Wert von **StartDateTime** muss im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601) angegeben sein. Beispiel: 2014-03-03T22:00:00Z.

		Get-AzureRmDataFactoryRun $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T22:00:00

	Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

		Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
		ResourceGroupName   : ADFTutorialResourceGroup
		DataFactoryName     : ADFTutorialDataFactoryPSH
		TableName           : EmpSQLTable
		ProcessingStartTime : 3/3/2015 11:03:28 PM
		ProcessingEndTime   : 3/3/2015 11:04:36 PM
		PercentComplete     : 100
		DataSliceStart      : 3/8/2015 10:00:00 PM
		DataSliceEnd        : 3/8/2015 11:00:00 PM
		Status              : Succeeded
		Timestamp           : 3/8/2015 11:03:28 PM
		RetryAttempt        : 0
		Properties          : {}
		ErrorMessage        :
		ActivityName        : CopyFromBlobToSQL
		PipelineName        : ADFTutorialPipeline
		Type                : Copy

In der [Data Factory-Cmdlet-Referenz][cmdlet-reference] finden Sie eine umfassende Dokumentation zu Data Factory-Cmdlets.

## Zusammenfassung
In diesem Lernprogramm haben Sie eine Azure Data Factory erstellt, um Daten aus einem Azure-Blob in eine Azure SQL-Datenbank zu kopieren. Sie haben PowerShell verwendet, um die Data Factory, verknüpfte Dienste, Datasets und eine Pipeline zu erstellen. Nachfolgend sind die allgemeinen Schritte aufgeführt, die Sie in diesem Tutorial ausgeführt haben:

1.	Sie haben eine Azure **Data Factory** erstellt.
2.	Sie haben **verknüpfte Dienste** erstellt:
	1. Einen verknüpften **Azure Storage**-Dienst zum Verknüpfen Ihres Azure Storage-Kontos, in dem Eingabedaten enthalten sind.
	2. Einen verknüpften **Azure SQL**-Dienst zum Verknüpfen Ihrer Azure SQL-Datenbank, in der die Ausgabedaten enthalten sind.
3.	Sie haben **Datasets** erstellt, die Eingabedaten und Ausgabedaten für Pipelines beschreiben.
4.	Sie haben eine **Pipeline** mit einer **Kopieraktivität**, mit **BlobSource** als Quelle und mit **SqlSink** als Senke erstellt.

## Weitere Informationen
| Thema | Beschreibung |
| :---- | :---- |
| [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) | Dieser Artikel enthält ausführliche Informationen zur Kopieraktivität, die Sie in diesem Tutorial verwendet haben. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | In diesem Artikel werden die Planungs- und Ausführungsaspekte des Azure Data Factory-Anwendungsmodells erläutert. |
| [Pipelines](data-factory-create-pipelines.md) | Dieser Artikel enthält Informationen zu Pipelines und Aktivitäten in Azure Data Factory. |
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
 

<!---HONumber=AcomDC_0824_2016-->