---
title: 'Tutorial: Erstellen einer Pipeline zum Verschieben von Daten mithilfe von Azure PowerShell | Microsoft-Dokumentation'
description: "In diesem Tutorial erstellen Sie mithilfe von Azure PowerShell eine Azure Data Factory-Pipeline mit Kopieraktivität."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 8b5cb66ea958cf6643fa34abb8d484b97b212373
ms.lasthandoff: 04/27/2017


---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a>Tutorial: Erstellen einer Data Factory-Pipeline zum Verschieben von Daten mithilfe von Azure PowerShell
> [!div class="op_single_selector"]
> * [Übersicht und Voraussetzungen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kopier-Assistent](data-factory-copy-data-wizard-tutorial.md)
> * [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

In diesem Tutorial erstellen und überwachen Sie eine Azure Data Factory-Instanz mithilfe von Azure PowerShell-Cmdlets. Die Pipeline in der Data Factory, die Sie in diesem Tutorial erstellen, verwendet eine Kopieraktivität, um Daten aus einem Azure-Blob in eine Azure SQL-Datenbank zu kopieren.

Das Kopieraktivitätsfeature führt die Datenverschiebung in Data Factory durch. Sie basiert auf einem global verfügbaren Dienst, mit dem Daten zwischen verschiedenen Datenspeichern sicher, zuverlässig und skalierbar kopiert werden können. Ausführliche Informationen zur Kopieraktivität finden Sie unter [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).   

> [!NOTE]
> In diesem Artikel werden nicht alle Data Factory-Cmdlets behandelt. Eine umfassende Dokumentation zu diesen Cmdlets finden Sie in der [Data Factory-Cmdlet-Referenz](/powershell/module/azurerm.datafactories).
>
> Die Datenpipeline in diesem Tutorial kopiert Daten aus einem Quelldatenspeicher in einen Zieldatenspeicher. Sie transformiert keine Eingabedaten in Ausgabedaten. Ein Tutorial zum Transformieren von Daten mithilfe von Azure Data Factory finden Sie unter [Tutorial: Erstellen der ersten Pipeline zum Verarbeiten von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Voraussetzungen
- Verschaffen Sie sich unter [Übersicht und Voraussetzungen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) einen Überblick über das Tutorial, und führen Sie die Schritte aus, die zur Erfüllung der **Voraussetzungen** erforderlich sind.
- Installieren Sie Azure PowerShell. Befolgen Sie die Anweisungen unter [Get started with Azure PowerShell cmdlets](../powershell-install-configure.md) (Erste Schritte mit Azure PowerShell-Cmdlets).

## <a name="in-this-tutorial"></a>Dieses Lernprogramm umfasst folgende Punkte
Die folgende Tabelle enthält die einzelnen Schritte dieses Tutorials.

| Schritt | Beschreibung |
| --- | --- |
| [Erstellen einer Azure Data Factory-Instanz](#create-data-factory) |In diesem Schritt erstellen Sie eine Azure Data Factory mit dem Namen **ADFTutorialDataFactoryPSH**. |
| [Erstellen von verknüpften Diensten](#create-linked-services) |In diesem Schritt erstellen Sie die beiden verknüpften Dienste **StorageLinkedService** und **AzureSqlLinkedService**. „StorageLinkedService“ verknüpft einen Azure Storage-Dienst, und „AzureSqlLinkedService“ verknüpft eine Azure SQL-Datenbank mit „ADFTutorialDataFactoryPSH“. |
| [Erstellen von Eingabe- und Ausgabedatasets](#create-datasets) |In diesem Schritt definieren Sie zwei Datasets (EmpTableFromBlob und EmpSQLTable). Diese werden als Ein- und Ausgabetabellen für die **Kopieraktivität** im ADFTutorialPipeline-Element verwendet, das im nächsten Schritt erstellt wird. |
| [Erstellen und Ausführen einer Pipeline](#create-pipeline) |In diesem Schritt erstellen Sie die Pipeline **ADFTutorialPipeline** in der Data Factory „ADFTutorialDataFactoryPSH“. Die Pipeline verwendet eine Kopieraktivität, um Daten aus einem Azure-Blob in eine Azure-Ausgabedatenbanktabelle zu kopieren. |
| [Überwachen der Datasets und der Pipeline](#monitor-pipeline) |In diesem Schritt überwachen Sie die Datasets und die Pipeline mit Azure PowerShell. |

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
In diesem Schritt erstellen Sie mithilfe von Azure PowerShell eine Azure Data Factory-Instanz namens **ADFTutorialDataFactoryPSH**.

1. Starten Sie **PowerShell**. Lassen Sie Azure PowerShell bis zum Ende dieses Tutorials geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.

    Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen:

    ```PowerShell
    Get-AzureRmSubscription
    ```

    Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **&lt;NameOfAzureSubscription**&gt; durch den Namen Ihres Azure-Abonnements:

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. Erstellen Sie eine Azure-Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup** , indem Sie den folgenden Befehl ausführen:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    Bei einigen Schritten dieses Lernprogramms wird davon ausgegangen, dass Sie die Ressourcengruppe namens **ADFTutorialResourceGroup**verwenden. Bei Verwendung einer anderen Ressourcengruppe müssen Sie diese anstelle der Ressourcengruppe ADFTutorialResourceGroup verwenden.
3. Führen Sie das Cmdlet **New-AzureRmDataFactory** aus, um eine Data Factory namens **ADFTutorialDataFactoryPSH** zu erstellen:  

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
Beachten Sie folgende Punkte:

* Der Name der Azure Data Factory muss global eindeutig sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen (beispielsweise in „<IhrName>ADFTutorialDataFactoryPSH“). Verwenden Sie diesen Namen beim Ausführen der Schritte in diesem Lernprogramm anstelle von "ADFTutorialDataFactoryPSH". Informationen zu Data Factory-Artefakten finden Sie unter [Data Factory – Benennungsregeln](data-factory-naming-rules.md).

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* Data Factory-Instanzen können nur von Mitwirkenden oder Administratoren des Azure-Abonnements erstellt werden.
* Der Name der Data Factory kann später möglicherweise als DNS-Name registriert und so öffentlich sichtbar werden.
* Unter Umständen erhalten Sie folgenden Fehler: **Das Abonnement ist nicht für die Verwendung des Namespace „Microsoft.DataFactory“ registriert.** Führen Sie eine der folgenden Aktionen aus, und wiederholen Sie die Veröffentlichung:

  * Führen Sie in Azure PowerShell den folgenden Befehl aus, um den Data Factory-Anbieter zu registrieren:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    Führen Sie den folgenden Befehl aus, um sicherzustellen, dass der Data Factory-Anbieter registriert wurde:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Melden Sie sich unter Verwendung Ihres Azure-Abonnements beim [Azure-Portal](https://portal.azure.com) an. Navigieren Sie zu einem Data Factory-Blatt, oder erstellen Sie über das Azure-Portal eine Data Factory. Mit dieser Aktion wird der Anbieter automatisch für Sie registriert.

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Verknüpfte Dienste verknüpfen Datenspeicher oder Serverdienste mit einer Azure Data Factory. Ein Datenspeicher kann ein Azure Storage-Dienst, eine Azure SQL-Datenbank oder eine lokale SQL Server-Datenbank sein, der bzw. die Eingabedaten enthält oder Ausgabedaten für eine Data Factory-Pipeline speichert. Ein Serverdienst ist der Dienst, der Eingabedaten verarbeitet und Ausgabedaten erzeugt.

In diesem Schritt erstellen Sie die beiden verknüpften Dienste **StorageLinkedService** und **AzureSqlLinkedService**. „StorageLinkedService“ verknüpft ein Azure-Speicherkonto, und „AzureSqlLinkedService“ verknüpft eine Azure SQL-Datenbank mit der Data Factory **ADFTutorialDataFactoryPSH**. Später in diesem Tutorial erstellen Sie eine Pipeline, die Daten aus einem Blobcontainer in „StorageLinkedService“ in eine SQL-Tabelle in „AzureSqlLinkedService“ kopiert.

### <a name="create-a-linked-service-for-an-azure-storage-account"></a>Erstellen eines verknüpften Diensts für ein Azure-Speicherkonto
1. Erstellen Sie im Ordner **C:\ADFGetStartedPSH** eine JSON-Datei mit dem Namen **StorageLinkedService.json** und dem unten angegebenen Inhalt. (Erstellen Sie den Ordner „ADFGetStartedPSH“, sofern er noch nicht vorhanden ist.)

    ```json
    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
     }
    ```
   Ersetzen Sie **accountname** und **accountkey** durch den Namen bzw. Schlüssel Ihres Azure-Speicherkontos.
2. Wechseln Sie in **Azure PowerShell** zum Ordner **ADFGetStartedPSH**.
3. Verwenden Sie das Cmdlet **New-AzureRmDataFactoryLinkedService** , um einen verknüpften Dienst zu erstellen. Für dieses Cmdlet und andere Data Factory-Cmdlets, die Sie in diesem Tutorial verwenden, müssen Werte für die Parameter **ResourceGroupName** und **DataFactoryName** eingegeben werden. Wenn Sie nicht bei jeder Ausführung eines Cmdlets „ResourceGroupName“ und „DataFactoryName“ eingeben möchten, können Sie alternativ mithilfe von **Get-AzureRmDataFactory** ein DataFactory-Objekt abrufen und dann das Objekt übergeben. Führen Sie folgenden Befehl aus, um die Ausgabe des Cmdlets **Get-AzureRmDataFactory** der Variablen **$df** zuzuweisen:

    ```PowerShell   
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
    ```

4. Führen Sie nun das Cmdlet **New-AzureRmDataFactoryLinkedService** zum Erstellen des verknüpften Diensts **StorageLinkedService** aus.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```

    Wenn Sie das Cmdlet **Get-AzureRmDataFactory** noch nicht ausgeführt und die Ausgabe nicht der Variablen **$df** zugewiesen hätten, müssten Sie für die Parameter „ResourceGroupName“ und „DataFactoryName“ die folgenden Werte angeben:   

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json
    ```

Wenn Sie Azure PowerShell mitten im Tutorial schließen, müssen Sie beim nächsten Start von Azure PowerShell das Cmdlet „Get-AzureRmDataFactory“ ausführen, um das Tutorial abzuschließen.

### <a name="create-a-linked-service-for-an-azure-sql-database"></a>Erstellen eines verknüpften Diensts für eine Azure SQL-Datenbank
1. Erstellen Sie die JSON-Datei „AzureSqlLinkedService.json“ mit folgendem Inhalt:

    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
   Ersetzen Sie **servername**, **databasename****username@servername** und **password** durch die Namen von Azure SQL-Server, -Datenbank, -Benutzerkonto und -Kennwort.
2. Führen Sie den folgenden Befehl aus, um einen verknüpften Dienst zu erstellen:

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```

   Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für Ihren SQL-Datenbankserver aktiviert ist. Führen Sie zum Prüfen und Aktivieren die folgenden Schritte aus:

   1. Klicken Sie links auf den Hub **DURCHSUCHEN** und dann auf **SQL Server**.
   2. Wählen Sie Ihren Server aus, und klicken Sie auf dem Blatt **SQL SERVER** auf **EINSTELLUNGEN**.
   3. Klicken Sie auf dem Blatt **EINSTELLUNGEN** auf **Firewall**.
   4. Klicken Sie auf dem Blatt **Firewalleinstellungen** für **Zugriff auf Azure-Dienste erlauben** auf **EIN**.
   5. Klicken Sie links auf den Hub **AKTIV**, um zum Blatt **Data Factory** zurückzukehren.

## <a name="create-datasets"></a>Erstellen von Datasets
Im vorherigen Schritt haben Sie Dienste erstellt, um ein Azure-Speicherkonto und eine Azure SQL-Datenbank mit der Data Factory zu verknüpfen. In diesem Schritt erstellen Sie Datasets mit den Eingabe- und Ausgabedaten für die Kopieraktivität in der Pipeline, die Sie im nächsten Schritt erstellen.

Eine Tabelle ist ein rechteckiges Dataset. Momentan wird ausschließlich diese Art von Dataset unterstützt. Die Eingabetabelle in diesem Tutorial verweist auf einen Blobcontainer in Azure Storage. Die Ausgabetabelle verweist auf eine SQL-Tabelle in der Azure SQL-Datenbank.  

### <a name="prepare-azure-blob-storage-and-azure-sql-database-for-the-tutorial"></a>Vorbereiten von Azure Blob-Speicher und Azure SQL-Datenbank für das Tutorial
Überspringen Sie diesen Schritt, wenn Sie bereits das Tutorial unter [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) absolviert haben.

Führen Sie die folgenden Schritte aus, um den Blobspeicher und die SQL-Datenbank für dieses Tutorial vorzubereiten:

1. Erstellen Sie einen Blobcontainer namens **adftutorial** in dem Blobspeicher, auf den **StorageLinkedService** verweist.
2. Erstellen Sie die Textdatei **emp.txt**, und laden Sie sie als Blob in den Container **adftutorial** hoch.
3. Erstellen Sie eine Tabelle namens **emp** in der Azure SQL-Datenbank, auf die **AzureSqlLinkedService** verweist.

4. Starten Sie den Editor. Kopieren Sie den folgenden Text, und speichern Sie ihn in der Datei **emp.txt** im Ordner **C:\ADFGetStartedPSH** auf Ihrer Festplatte.

    ```
    John, Doe
    Jane, Doe
    ```
5. Erstellen Sie mithilfe von Tools wie [Azure-Speicher-Explorer](https://azurestorageexplorer.codeplex.com/) den Container **adftutorial**, und laden Sie die Datei **emp.txt** in den Container hoch.

    ![Azure-Speicher-Explorer](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
6. Verwenden Sie das folgende SQL-Skript, um die Tabelle **emp** in Ihrer SQL-Datenbank zu erstellen.  

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    Falls auf Ihrem Computer SQL Server 2014 installiert ist, befolgen Sie die Anweisungen unter [Schritt 2: Herstellen einer Verbindung mit der SQL-Datenbank](../sql-database/sql-database-manage-azure-ssms.md) des Artikels „Verwalten einer Azure SQL-Datenbank mit SQL Server Management Studio“, um eine Verbindung mit Ihrem SQL-Datenbankserver herzustellen und das SQL-Skript auszuführen.

    Falls Ihr Client keinen Zugriff auf den SQL-Datenbankserver hat, müssen Sie die Firewall für Ihren SQL-Datenbankserver so konfigurieren, dass der Zugriff über Ihren Computer (bzw. dessen IP-Adresse) möglich ist. Eine entsprechende Anleitung finden Sie [hier](../sql-database/sql-database-configure-firewall-settings.md).

### <a name="create-an-input-dataset"></a>Erstellen eines Eingabedatasets
Eine Tabelle ist ein rechteckiges Dataset mit einem Schema. In diesem Schritt erstellen Sie eine Tabelle namens **EmpBlobTable**. Diese Tabelle verweist auf einen Blobcontainer in der Azure Storage-Instanz, die vom verknüpften Dienst **StorageLinkedService** dargestellt wird. Dieser Blobcontainer (adftutorial) enthält die Eingabedaten in der Datei **emp.txt**.

1. Erstellen Sie im Ordner **C:\ADFGetStartedPSH** eine JSON-Datei namens **EmpBlobTable.json** mit folgendem Inhalt:

    ```json
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
    ```

   Beachten Sie folgende Punkte:

   * Für das Dataset ist **type** auf **AzureBlob** festgelegt.
   * **linkedServiceName** ist auf **StorageLinkedService** festgelegt.
   * **folderPath** ist auf den Container **adftutorial** festgelegt.
   * **fileName** hat den Wert **emp.txt**. Wenn Sie keinen Blobnamen angeben, werden Daten aus allen Blobs im Container als Eingabedaten betrachtet.  
   * Für das Format ist **type** auf **TextFormat** festgelegt.
   * Die Textdatei enthält die beiden Felder **FirstName** und **LastName**, die durch ein Komma getrennt sind (columnDelimiter).    
   * Die Verfügbarkeit (**availability**) ist auf **hourly** („frequency“ auf „hour“ und „interval“ auf „1“) festgelegt. Der Data Factory-Dienst sucht also stündlich im Stammordner des Blobcontainers (adftutorial) nach Eingabedaten.

   Wenn Sie für eine **Eingabetabelle** keinen Dateinamen (**fileName**) angeben, werden alle Dateien und Blobs aus dem Eingabeordner (folderPath) als Eingaben betrachtet. Wenn Sie einen Dateinamen im JSON-Format angeben, wird nur die angegebene Datei bzw. das angegebene Blob als Eingabe betrachtet.

   Wenn Sie keinen **fileName** für eine **Ausgabetabelle** angeben, werden die generierten Dateien in **folderPath** im folgenden Format benannt: Data.<Guid\>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   Um **folderPath** und **fileName** dynamisch basierend auf der **SliceStart**-Zeit festzulegen, verwenden Sie die **partitionedBy**-Eigenschaft. Im folgenden Beispiel verwendet folderPath die Angaben für Jahr, Monat und Tag aus „SliceStart“ (Startzeit des zu verarbeitenden Slices) und „fileName“ die Angabe für Stunde aus „SliceStart“. Wenn beispielsweise ein Slice für den Zeitpunkt „2016-10-20T08:00:00“ erzeugt wird, wird „folderName“ auf „wikidatagateway/wikisampledataout/2016/10/20“ und „fileName“ auf „08.csv“ festgelegt.

    ```json
     "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
     "fileName": "{Hour}.csv",
     "partitionedBy":
     [
         { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
         { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
         { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
         { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
     ],
    ```

   Informationen zu JSON-Eigenschaften finden Sie in der [JSON-Skriptreferenz](data-factory-data-movement-activities.md).
2. Führen Sie den folgenden Befehl zum Erstellen des Data Factory-Datensatzes aus.

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\EmpBlobTable.json
    ```

### <a name="create-an-output-dataset"></a>Erstellen eines Ausgabedatasets
In diesem Schritt erstellen Sie ein Ausgabedataset namens **EmpSQLTable**. Dieses Dataset verweist auf eine SQL-Tabelle (emp) in der durch **AzureSqlLinkedService** dargestellten Azure SQL-Datenbank. Die Pipeline kopiert Daten aus dem Eingabeblob in die Tabelle **emp** .

1. Erstellen Sie im Ordner **C:\ADFGetStartedPSH** eine JSON-Datei mit dem Namen **EmpSQLTable.json** mit folgendem Inhalt:

    ```json
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
    ```

   Beachten Sie folgende Punkte:

   * Für das Dataset ist **type** auf **AzureSqlTable** festgelegt.
   * **linkedServiceName** ist auf **AzureSqlLinkedService** festgelegt.
   * **tablename** ist auf **emp** festgelegt.
   * Die emp-Tabelle in der Datenbank enthält drei Spalten: **ID**, **FirstName** und **LastName**. Da es sich bei „ID“ um eine Identitätsspalte handelt, müssen Sie hier lediglich **FirstName** und **LastName** angeben.
   * Die Verfügbarkeit (**availability**) ist auf **hourly** („frequency“ auf „hour“ und „interval“ auf „1“) festgelegt. Der Data Factory-Dienst generiert in der Tabelle **emp** in der Azure SQL-Datenbank stündlich einen Ausgabedatenslice.
2. Führen Sie den folgenden Befehl aus, um das Data Factory-Dataset zu erstellen:

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json
    ```

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline
In diesem Schritt erstellen Sie eine Pipeline mit **Kopieraktivität**. Die Pipeline verwendet **EmpTableFromBlob** als Eingabe und **EmpSQLTable** als Ausgabe.

1. Erstellen Sie im Ordner **C:\ADFGetStartedPSH** eine JSON-Datei namens **ADFTutorialPipeline.json** mit folgendem Inhalt:

    ```json
    {
        "name": "ADFTutorialPipeline",
        "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
                {
                    "name": "CopyFromBlobToSQL",
                    "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                    "type": "Copy",
                    "inputs": [{ "name": "EmpTableFromBlob" }],
                    "outputs": [{ "name": "EmpSQLTable" }],
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
    ```
   Beachten Sie folgende Punkte:

   * Der Abschnitt „Activities“ enthält nur eine Aktivität, deren **type** auf **Copy** festgelegt ist.
   * Die Eingabe für die Aktivität ist auf **EmpTableFromBlob** und die Ausgabe auf **EmpSQLTable** festgelegt.
   * Im Abschnitt **transformation** ist **BlobSource** als Quelltyp und **SqlSink** als Senkentyp angegeben.

   Ersetzen Sie den Wert der Eigenschaft **start** durch den aktuellen Tag und den Wert der Eigenschaft **end** durch den nächsten Tag. Die Start- und Endzeit von Datums-/Uhrzeitangaben müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601)angegeben werden. Beispiel: 2016-10-14T16:32:41Z. Die Zeitangabe **end** wird in diesem Tutorial verwendet, ist aber optional.

   Wenn für die **end**-Eigenschaft kein Wert angegeben wird, wird sie als „**start + 48 Stunden**“ berechnet. Um die Pipeline auf unbestimmte Zeit auszuführen, geben Sie als Wert für die **end**-Eigenschaft **9/9/9999** an.

   In diesem Beispiel ergeben sich 24 Datenslices, da jede Stunde ein Datenslice erstellt wird.

   Weitere Informationen zu JSON-Eigenschaften finden Sie in der [JSON-Skriptreferenz](data-factory-data-movement-activities.md).
2. Führen Sie den folgenden Befehl aus, um die Data Factory-Tabelle zu erstellen:

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

Glückwunsch! Sie haben erfolgreich eine Azure Data Factory, verknüpfte Dienste, Tabellen und eine Pipeline erstellt. Außerdem haben Sie die Pipeline geplant.

## <a name="monitor-the-pipeline"></a>Überwachen der Pipeline
In diesem Schritt verwenden Sie Azure PowerShell zur Überwachung der Aktivitäten in einer Azure Data Factory.

1. Führen Sie **Get-AzureRmDataFactory** aus, und weisen Sie die Ausgabe der Variablen „$df“ zu.

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
    ```

2. Führen Sie **Get-AzureRmDataFactorySlice** aus, um Details zu allen Slices in der Tabelle **EmpSQLTable** zu erhalten. Dies ist die Ausgabetabelle der Pipeline.  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2016-08-09T00:00:00
    ```

   Ersetzen Sie den Jahres-, Monats- und Datumsteil des Parameters **StartDateTime** durch aktuelle Werte für Jahr, Monat und Datum. Diese Einstellung muss mit dem Wert **Start** im JSON-Code der Pipeline übereinstimmen.

   Es sollten 24 Slices angezeigt werden (einer für jede Stunde von 12:00 Uhr des aktuellen Tages bis 12:00 Uhr des nächsten Tages).

   **Beispielausgabe:**

    ``` 
     ResourceGroupName : ADFTutorialResourceGroup
     DataFactoryName   : ADFTutorialDataFactoryPSH
     TableName         : EmpSQLTable
     Start             : 8/9/2016 12:00:00 AM
     End               : 8/9/2016 1:00:00 AM
     RetryCount        : 0
     Status            : Waiting
     LatencyStatus     :
     LongRetryCount    : 0
    ```
3. Führen Sie **Get-AzureRmDataFactoryRun** aus, um die Details der Aktivitätsausführungen für einen **bestimmten** Slice abzurufen. Ändern Sie den Wert des Parameters **StartDateTime**, sodass er dem Zeitwert **Start** für den Slice aus der Ausgabe entspricht. Der Wert von **StartDateTime** muss im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601)angegeben sein.

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName EmpSQLTable -StartDateTime 2016-08-09T00:00:00
    ```

   Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:

    ```  
    Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH
    TableName           : EmpSQLTable
    ProcessingStartTime : 8/9/2016 11:03:28 PM
    ProcessingEndTime   : 8/9/2016 11:04:36 PM
    PercentComplete     : 100
    DataSliceStart      : 8/9/2016 10:00:00 PM
    DataSliceEnd        : 8/9/2016 11:00:00 PM
    Status              : Succeeded
    Timestamp           : 8/9/2016 11:03:28 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy
    ```

Eine umfassende Dokumentation zu Data Factory-Cmdlets finden Sie in der [Data Factory-Cmdlet-Referenz][cmdlet-reference].

## <a name="summary"></a>Zusammenfassung
In diesem Lernprogramm haben Sie eine Azure Data Factory erstellt, um Daten aus einem Azure-Blob in eine Azure SQL-Datenbank zu kopieren. Sie haben PowerShell verwendet, um die Data Factory, verknüpfte Dienste, Datasets und eine Pipeline zu erstellen. Nachfolgend sind die allgemeinen Schritte aufgeführt, die Sie in diesem Tutorial ausgeführt haben:  

1. Sie haben eine Azure **Data Factory**erstellt.
2. Sie haben **verknüpfte Dienste**erstellt:

   a. Einen verknüpften **Azure Storage**-Dienst zum Verknüpfen Ihres Azure-Speicherkontos, in dem die Eingabedaten enthalten sind.     
   b. Einen verknüpften **Azure SQL**-Dienst zum Verknüpfen Ihrer SQL-Datenbank, in der die Ausgabedaten enthalten sind.
3. Sie haben **Datasets** erstellt, die Eingabedaten und Ausgabedaten für Pipelines beschreiben.
4. Sie haben eine **Pipeline** mit **Kopieraktivität**, mit **BlobSource** als Quelle und mit **SqlSink** als Senke erstellt.

## <a name="see-also"></a>Weitere Informationen
| Thema | Beschreibung |
|:--- |:--- |
| [Cmdlet-Referenz zu Data Factory](/powershell/module/azurerm.datafactories) | Dieser Abschnitt enthält Informationen zu allen Data Factory-Cmdlets. |
| [Pipelines](data-factory-create-pipelines.md) |Dieser Artikel enthält Informationen zu Pipelines und Aktivitäten in Azure Data Factory. |
| [datasets](data-factory-create-datasets.md) |Dieser Artikel enthält Informationen zu Datasets in Azure Data Factory. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) |In diesem Artikel werden die Planungs- und Ausführungsaspekte des Azure Data Factory-Anwendungsmodells erläutert. |

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

