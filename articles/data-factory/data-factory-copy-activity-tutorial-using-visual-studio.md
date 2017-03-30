---
title: "Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe von Visual Studio | Microsoft Docs"
description: "In diesem Tutorial erstellen Sie mithilfe von Visual Studio eine Azure Data Factory-Pipeline mit Kopieraktivität."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/02/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 5e6ffbb8f1373f7170f87ad0e345a63cc20f08dd
ms.openlocfilehash: 39824fa66dee9f1bd57687e59ece97f4f4636b7d
ms.lasthandoff: 03/24/2017


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe von Visual Studio
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

In diesem Tutorial wird veranschaulicht, wie Sie eine Azure Data Factory mit Visual Studio erstellen und überwachen. Die Pipeline in der Data Factory verwendet eine Kopieraktivität zum Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank.

> [!NOTE]
> Die Datenpipeline in diesem Tutorial kopiert Daten aus einem Quelldatenspeicher in einen Zieldatenspeicher. Sie transformiert keine Eingabedaten in Ausgabedaten. Ein Tutorial zum Transformieren von Daten mithilfe von Azure Data Factory finden Sie unter [Tutorial: Erstellen der ersten Pipeline zum Verarbeiten von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md).
> 
> Sie können zwei Aktivitäten verketten (nacheinander ausführen), indem Sie das Ausgabedataset einer Aktivität als Eingabedataset der anderen Aktivität festlegen. Ausführliche Informationen finden Sie unter [Data Factory – Planung und Ausführung](data-factory-scheduling-and-execution.md).

Hier sind die Schritte angegeben, die Sie im Rahmen dieses Tutorials ausführen:

1. Erstellen von zwei verknüpften Diensten: **AzureStorageLinkedService1** und **AzureSqlinkedService1**. 
   
    Der AzureStorageLinkedService1 verknüpft einen Azure-Speicher, und AzureSqlLinkedService1 verknüpft eine Azure SQL-Datenbank mit der Data Factory: **ADFTutorialDataFactoryVS**. Die Eingabedaten für die Pipeline befinden sich in einem Blobcontainer im Azure-Blobspeicher. Ausgabedaten werden in einer Tabelle in der Azure SQL-Datenbank gespeichert. Daher fügen Sie diese beiden Datenspeicher als verknüpfte Dienste der Data Factory hinzu.
2. Erstellen Sie zwei Datasets: **InputDataset** und **OutputDataset**. Die Datasets stehen für die Eingabe- bzw. Ausgabedaten, die in den Datenspeichern gespeichert werden. 
   
    Geben Sie für „InputDataset“ den Blobcontainer an, der ein Blob mit den Quelldaten enthält. Geben Sie für „OutputDataset“ die SQL-Tabelle an, in der die Ausgabedaten gespeichert werden. Sie können auch andere Eigenschaften wie Struktur, Verfügbarkeit und die Richtlinie angeben.
3. Erstellen einer Pipeline mit dem Namen **ADFTutorialPipeline** in ADFTutorialDataFactoryVS. 
   
    Die Pipeline enthält eine **Kopieraktivität** , die Eingabedaten aus dem Azure-Blob in die Azure SQL-Ausgabetabelle kopiert. Die Kopieraktivität dient zum Verschieben von Daten in Azure Data Factory. Sie basiert auf einem global verfügbaren Dienst, mit dem Daten zwischen verschiedenen Datenspeichern sicher, zuverlässig und skalierbar kopiert werden können. Ausführliche Informationen zur Kopieraktivität finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) . 
4. Erstellen Sie eine Data Factory namens **VSTutorialFactory**. Stellen Sie die Data Factory und alle Data Factory-Entitäten (verknüpfte Dienste, Tabellen und die Pipeline) bereit.    

## <a name="prerequisites"></a>Voraussetzungen
1. Lesen Sie sich den Artikel mit der [Übersicht über das Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) durch, und führen Sie die Schritte zur Erfüllung der **Voraussetzungen** aus. 
2. Data Factory-Entitäten können nur von einem **Administrator des Azure-Abonnements** für Azure Data Factory veröffentlicht werden.  
3. Folgendes muss auf Ihrem Computer installiert sein: 
   * Visual Studio 2013 oder Visual Studio 2015
   * Laden Sie das Azure-SDK für Visual Studio 2013 oder Visual Studio 2015 herunter. Navigieren Sie zur [Azure-Downloadseite](https://azure.microsoft.com/downloads/), und klicken Sie auf **VS 2013** oder **VS 2015** im Abschnitt **.NET**.
   * Laden Sie das neueste Azure Data Factory-Plug-In für Visual Studio herunter: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) oder [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Sie können das Plug-In auch wie folgt aktualisieren: Klicken Sie im Menü auf **Tools** -> **Erweiterungen und Updates** -> **Online** -> **Visual Studio-Katalog** -> **Microsoft Azure Data Factory-Tools für Visual Studio** -> **Aktualisieren**.

## <a name="create-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts
1. Starten Sie **Visual Studio 2013**. Klicken Sie auf **Datei**, zeigen Sie auf **Neu**, und klicken Sie auf **Projekt**. Das Dialogfeld **Neues Projekt** sollte angezeigt werden.  
2. Wählen Sie im Dialogfeld **Neues Projekt** die Vorlage **DataFactory** aus, und klicken Sie auf **Leeres Data Factory-Projekt**. Wenn die Vorlage DataFactory nicht angezeigt wird, schließen Sie Visual Studio, installieren Sie Azure SDK für Visual Studio 2013, und öffnen Sie Visual Studio erneut.  
   
    ![Dialogfeld "Neues Projekt"](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Füllen Sie für das Projekt die Felder **Name**, **Speicherort** und **Lösung** aus, und klicken Sie auf **OK**.
   
    ![Projektmappen-Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Verknüpfte Dienste verknüpfen Datenspeicher oder Serverdienste mit einer Azure Data Factory. Informationen zu allen Quellen und Senken, die von der Kopieraktivität unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats) . Eine Liste mit Computediensten, die von Data Factory unterstützt werden, finden Sie unter [Verknüpfte Computedienste](data-factory-compute-linked-services.md) . In diesem Tutorial werden keine Computedienste verwendet. 

In diesem Schritt erstellen Sie zwei verknüpfte Dienste: **AzureStorageLinkedService1** und **AzureSqlLinkedService1**. Der verknüpfte Dienst „AzureStorageLinkedService1“ verknüpft ein Azure-Speicherkonto, und der Dienst „AzureSqlLinkedService“ verknüpft eine Azure SQL-Datenbank mit der Data Factory **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Erstellen des mit Azure Storage verknüpften Diensts
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verknüpfte Dienste**, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**.      
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Mit Azure-Speicher verknüpfter Dienst** aus der Liste aus, und klicken Sie auf **Hinzufügen**. 
   
    ![Neuer verknüpfter Dienst](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Ersetzen Sie `<accountname>` und `<accountkey>`* durch den Namen Ihres Azure-Speicherkontos bzw. durch den dazugehörigen Schlüssel. 
   
    ![Mit Azure-Speicher verknüpfter Dienst](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Speichern Sie die Datei **AzureStorageLinkedService1.json** .

> Details zu JSON-Eigenschaften finden Sie unter [Verschieben von Daten in einen und aus einem Azure-Blob mithilfe von Azure Data Factory](data-factory-azure-blob-connector.md#azure-storage-linked-service) .
> 
> 

### <a name="create-the-azure-sql-linked-service"></a>Erstellen des mit Azure SQL verknüpften Diensts
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste erneut auf den Knoten **Verknüpfte Dienste**, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**. 
2. Wählen Sie dieses Mal **Azure SQL Linked Service** aus, und klicken Sie dann auf **Hinzufügen**. 
3. Ersetzen Sie in der Datei **AzureSqlLinkedService1.json** die Platzhalter `<servername>`, `<databasename>`, `<username@servername>` und `<password>` durch die entsprechenden Angaben für Azure SQL-Server, -Datenbank, -Benutzerkonto und -Kennwort.    
4. Speichern Sie die Datei **AzureSqlLinkedService1.json** . 

> [!NOTE]
> Details zu JSON-Eigenschaften finden Sie unter [Verschieben von Daten in und aus Azure SQL-Datenbank mithilfe von Azure Data Factory](data-factory-azure-sql-connector.md#linked-service-properties) .
> 
> 

## <a name="create-datasets"></a>Erstellen von Datasets
Im vorherigen Schritt haben Sie die verknüpften Dienste **AzureStorageLinkedService1** und **AzureSqlLinkedService1** erstellt, um ein Azure-Speicherkonto und eine Azure SQL-Datenbank mit der Data Factory **ADFTutorialDataFactory** zu verknüpfen. In diesem Schritt definieren Sie die beiden Datasets **InputDataset** und **OutputDataset**. Sie stellen Ein- und Ausgabedaten dar, die in den Datenspeichern gespeichert werden, auf die mit „AzureStorageLinkedService1“ und „AzureSqlLinkedService1“ verwiesen wird. Geben Sie für „InputDataset“ den Blobcontainer an, der ein Blob mit den Quelldaten enthält. Geben Sie für „OutputDataset“ die SQL-Tabelle an, in der die Ausgabedaten gespeichert werden.

### <a name="create-input-dataset"></a>Erstellen eines Eingabedatasets
In diesem Schritt erstellen Sie ein Dataset namens **InputDataset**, das auf einen Blobcontainer in Azure Storage (dargestellt durch den verknüpften Dienst **AzureStorageLinkedService1**) verweist. Eine Tabelle ist ein rechteckiges Dataset und die einzige Art von Dataset, die derzeit unterstützt wird. 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Tabellen**, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**.
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Azure-Blob** aus, und klicken Sie auf **Hinzufügen**.   
3. Ersetzen Sie den JSON-Text durch den folgenden Text, und speichern Sie die **AzureBlobLocation1.json** -Datei. 

  ```json   
  {
    "name": "InputDataset",
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
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
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
   
   * „dataset **type**“ ist auf **AzureBlob** festgelegt.
   * **linkedServiceName** ist auf **AzureStorageLinkedService** festgelegt. Sie haben diesen verknüpften Dienst in Schritt 2 erstellt.
   * **folderPath** ist auf den Container **adftutorial** festgelegt. Mithilfe der **fileName** -Eigenschaft können Sie auch den Namen eines im Ordner enthaltenen Blobs angeben. Da Sie nicht den Namen des Blobs angeben, werden Daten aus allen Blobs im Container als Eingabedaten betrachtet.  
   * „format **type**“ ist auf **TextFormat** festgelegt.
   * Die Textdatei enthält die beiden Felder **FirstName** und **LastName**, die durch ein Komma getrennt sind (**columnDelimiter**).    
   * Die Verfügbarkeit (**availability**) ist auf **hourly**, (**frequency** auf **hour** und **interval** auf **1**) festgelegt. Der Data Factory-Dienst sucht also stündlich im Stammordner des angegebenen Blobcontainers (**adftutorial**) nach Eingabedaten. 
   
   Wenn Sie keinen Dateinamen (**fileName**) für ein Eingabedataset (**input**) angeben, werden alle Dateien/Blobs aus dem Eingabeordner (**folderPath**) als Eingaben betrachtet. Wenn Sie einen Dateinamen in der JSON-Datei angeben, wird nur die angegebene Datei/der angegebene Blob als Eingabe betrachtet.
   
   Wenn Sie keinen **fileName** für eine **Ausgabetabelle** angeben, werden die generierten Dateien in **folderPath** im folgenden Format benannt: Data.&lt;Guid&gt;.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).
   
   Um **folderPath** und **fileName** dynamisch basierend auf der **SliceStart**-Zeit festzulegen, verwenden Sie die **partitionedBy**-Eigenschaft. Im folgenden Beispiel verwendet folderPath die Angaben für Jahr, Monat und Tag aus „SliceStart“ (Startzeit des zu verarbeitenden Slices) und „fileName“ die Angabe für Stunde aus „SliceStart“. Wenn beispielsweise ein Slice für den Zeitpunkt „2016-09-20T08:00:00“ erzeugt wird, wird „folderName“ auf „wikidatagateway/wikisampledataout/2016/09/20“ und „fileName“ auf „08.csv“ festgelegt. 
  
    ```json   
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ```
            
> [!NOTE]
> Details zu JSON-Eigenschaften finden Sie unter [Verschieben von Daten in einen und aus einem Azure-Blob mithilfe von Azure Data Factory](data-factory-azure-blob-connector.md#dataset-properties) .
> 
> 

### <a name="create-output-dataset"></a>Erstellen des Ausgabedatasets
In diesem Schritt erstellen Sie ein Ausgabedataset namens **OutputDataset**. Dieses Dataset verweist auf eine SQL-Tabelle in der durch **AzureSqlLinkedService1**dargestellten Azure SQL-Datenbank. 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste erneut auf **Tabellen**, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**.
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Azure SQL** aus, und klicken Sie auf **Hinzufügen**. 
3. Ersetzen Sie den JSON-Text durch den folgenden JSON-Text, und speichern Sie die Datei **AzureSqlTableLocation1.json** .

    ```json
    {
     "name": "OutputDataset",
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
       "linkedServiceName": "AzureSqlLinkedService1",
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
   
   * „dataset **type**“ ist auf **AzureSQLTable** festgelegt.
   * **linkedServiceName** ist auf **AzureSqlLinkedService** festgelegt (diesen verknüpften Dienst haben Sie in Schritt 2 erstellt).
   * **tablename** ist auf **emp** festgelegt.
   * Die emp-Tabelle in der Datenbank enthält drei Spalten: **ID**, **FirstName** und **LastName**. Da es sich bei „ID“ um eine Identitätsspalte handelt, müssen Sie hier lediglich **FirstName** und **LastName** angeben.
   * Die Verfügbarkeit (**availability**) ist auf **hourly**, (**frequency** auf **hour** und **interval** auf **1**) festgelegt.  Der Data Factory-Dienst generiert in der Tabelle **emp** in der Azure SQL-Datenbank stündlich einen Ausgabedatenslice.

> [!NOTE]
> Details zu JSON-Eigenschaften finden Sie unter [Verschieben von Daten in und aus Azure SQL-Datenbank mithilfe von Azure Data Factory](data-factory-azure-sql-connector.md#linked-service-properties) .
> 
> 

## <a name="create-pipeline"></a>Erstellen der Pipeline
Sie haben bisher verknüpfte Ein-/Ausgabe-Dienste und -Tabellen erstellt. Nun erstellen Sie eine Pipeline mit einer **Kopieraktivität** , um Daten aus dem Azure-Blob in Azure SQL-Datenbank zu kopieren. 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Pipelines**, zeigen Sie auf **Hinzufügen**, und klicken Sie auf **Neues Element**.  
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Copy Data Pipeline**, und klicken Sie auf **Hinzufügen**. 
3. Ersetzen Sie den JSON-Code durch den folgenden JSON-Code, und speichern Sie die Datei **CopyActivity1.json** .

    ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob to Azure SQL table",
       "activities": [
         {
           "name": "CopyFromBlobToSQL",
           "type": "Copy",
           "inputs": [
             {
               "name": "InputDataset"
             }
           ],
           "outputs": [
             {
               "name": "OutputDataset"
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
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2015-07-12T00:00:00Z",
       "end": "2015-07-13T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
   Beachten Sie folgende Punkte:
   
   * Der Abschnitt „Activities“ enthält nur eine Aktivität, deren **Typ** auf **Copy** festgelegt ist.
   * Die Eingabe für die Aktivität ist auf **InputDataset** und die Ausgabe für die Aktivität ist auf **OutputDataset** festgelegt.
   * Im Abschnitt **typeProperties** ist **BlobSource** als Quelltyp und **SqlSink** als Senkentyp angegeben.
   
   Ersetzen Sie den Wert der **start**-Eigenschaft durch den aktuellen Tag und den der **end**-Eigenschaft durch den nächsten Tag. Sie können auch nur den Datumsteil angeben und den Uhrzeitteil überspringen. „2016-02-03“ entspricht beispielsweise „2016-02-03T00:00:00Z“.
   
   Die Start- und Endzeit von Datums-/Uhrzeitangaben müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601)angegeben werden. Beispiel: 2016-10-14T16:32:41Z. Die Zeitangabe **end** ist optional, wird aber in diesem Tutorial verwendet. 
   
   Wenn für die **end**-Eigenschaft kein Wert angegeben wird, wird sie als „**start + 48 Stunden**“ berechnet. Um die Pipeline auf unbestimmte Zeit auszuführen, geben Sie als Wert für die **end**-Eigenschaft **9999-09-09** an.
   
   Im obigen Beispiel ergeben sich 24 Datenslices, da jede Stunde ein Datenslice erstellt wird.

## <a name="publishdeploy-data-factory-entities"></a>Veröffentlichen/Bereitstellen der Data Factory-Entitäten
In diesem Schritt veröffentlichen Sie zuvor erstellte Data Factory-Entitäten (verknüpfte Dienste, Datasets und Pipeline). Außerdem geben Sie den Namen der neuen Data Factory an, die für diese Entitäten erstellt wird.  

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Veröffentlichen**. 
2. Wenn das Dialogfeld **Melden Sie sich bei Ihrem Microsoft-Konto an** angezeigt wird, geben Sie Ihre Anmeldeinformationen für das Konto mit dem Azure-Abonnement ein, und klicken Sie auf **Anmelden**.
3. Das folgende Dialogfeld sollte angezeigt werden:
   
   ![Dialogfeld „Veröffentlichen“](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Führen Sie auf der Seite zum Konfigurieren der Data Factory die folgenden Schritte aus: 
   
   1. Wählen Sie die Option **Neue Data Factory erstellen** .
   2. Geben Sie **VSTutorialFactory** als **Name** ein.  
      
      > [!IMPORTANT]
      > Der Name der Azure Data Factory muss global eindeutig sein. Falls beim Veröffentlichen ein Fehler in Verbindung mit dem Namen der Data Factory auftritt, ändern Sie den Namen der Data Factory (beispielsweise in „IhrNameVSTutorialFactory“), und wiederholen Sie den Veröffentlichungsvorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Data Factory – Benennungsregeln](data-factory-naming-rules.md) .        
      > 
      > 
   3. Wählen Sie im Feld **Abonnement** Ihr Azure-Abonnement aus.
      
      > [!IMPORTANT]
      > Gehen Sie wie folgt vor, wenn keine Abonnements angezeigt werden: Stellen Sie sicher, dass Sie mit einem Konto angemeldet sind, bei dem es sich um den Administrator oder Co-Admin des Abonnements handelt.  
      > 
      > 
   4. Wählen Sie die **Ressourcengruppe** für die zu erstellende Data Factory aus. 
   5. Wählen Sie die **Region** für die Data Factory aus. In der Dropdownliste werden nur Regionen angezeigt, die vom Data Factory-Dienst unterstützt werden.
   6. Klicken Sie auf **Weiter**, um zur Seite **Publish Items** zu wechseln.
      
       ![Seite zum Konfigurieren der Data Factory](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. Stellen Sie auf der Seite **Publish Items** sicher, dass alle Data Factory-Entitäten ausgewählt sind, und klicken Sie auf **Weiter**, um zur Seite **Zusammenfassung** zu wechseln.
   
   ![Seite zum Veröffentlichen von Elementen](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Prüfen Sie die Zusammenfassung, und klicken Sie auf **Weiter**, um den Bereitstellungsprozess zu starten und den **Bereitstellungsstatus** anzuzeigen.
   
   ![Seite mit der Veröffentlichungszusammenfassung](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. Auf der Seite **Bereitstellungsstatus** sollte der Status des Bereitstellungsprozesses angezeigt werden. Klicken Sie auf „Fertig stellen“, nachdem die Bereitstellung abgeschlossen ist.
 
   ![Seite mit dem Bereitstellungsstatus](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

Beachten Sie folgende Punkte: 

* Führen Sie einen der folgenden Schritte aus, wenn Sie eine Fehlermeldung wie**Dieses Abonnement ist nicht zur Verwendung des Microsoft.DataFactory-Namespaces registriert**erhalten, und versuchen Sie, die Veröffentlichung erneut durchzuführen: 
  
  * Führen Sie in Azure PowerShell den folgenden Befehl aus, um den Data Factory-Anbieter zu registrieren. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Sie können den folgenden Befehl ausführen, um sich zu vergewissern, dass der Data Factory-Anbieter registriert ist. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Melden Sie sich mit dem Azure-Abonnement beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu einem Data Factory-Blatt, oder erstellen Sie eine Data Factory im Azure-Portal. Mit dieser Aktion wird der Anbieter automatisch für Sie registriert.
* Der Name der Data Factory kann in Zukunft als DNS-Name registriert und so öffentlich sichtbar werden.

> [!IMPORTANT]
> Data Factory-Instanzen können nur von einem Administrator oder Co-Administrator des Azure-Abonnements erstellt werden.
> 
> 

## <a name="summary"></a>Zusammenfassung
In diesem Lernprogramm haben Sie eine Azure Data Factory erstellt, um Daten aus einem Azure-Blob in eine Azure SQL-Datenbank zu kopieren. Sie haben Visual Studio verwendet, um die Data Factory, verknüpfte Dienste, Datasets und eine Pipeline zu erstellen. Im Anschluss sind die allgemeinen Schritte aufgeführt, die Sie in diesem Tutorial ausgeführt haben:  

1. Sie haben eine Azure **Data Factory**erstellt.
2. Sie haben **verknüpfte Dienste**erstellt:
   1. Einen verknüpften **Azure Storage** -Dienst zum Verknüpfen Ihres Azure Storage-Kontos, in dem Eingabedaten enthalten sind.     
   2. Einen verknüpften **Azure SQL** -Dienst zum Verknüpfen Ihrer Azure SQL-Datenbank, in der die Ausgabedaten enthalten sind. 
3. Sie haben **Datasets**erstellt, mit denen Eingabedaten und Ausgabedaten für Pipelines beschrieben werden.
4. Sie haben eine **Pipeline** mit einer **Kopieraktivität**, mit **BlobSource** als Quelle und mit **qlSink** als Senke erstellt. 

## <a name="use-server-explorer-to-view-data-factories"></a>Verwenden von Server-Explorer zum Anzeigen von Data Factorys
1. Klicken Sie in **Visual Studio** im Menü auf **Ansicht** und dann auf **Server-Explorer**.
2. Erweitern Sie im Server-Explorer-Fenster erst die Option **Azure** und dann **Data Factory**. Wenn **Bei Visual Studio anmelden** angezeigt wird, geben Sie das mit Ihrem Azure-Abonnement verknüpfte **Konto** ein, und klicken Sie auf **Weiter**. Geben Sie Ihr **Kennwort** ein, und klicken Sie auf **Anmelden**. Visual Studio versucht, Informationen zu allen Azure Data Factorys abzurufen, die in Ihrem Abonnement enthalten sind. Der Status dieses Vorgangs wird im Fenster **Data Factory-Aufgabenliste** angezeigt.

    ![Server-Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Durch Klicken mit der rechten Maustaste auf eine Data Factory und Auswählen von „Data Factory in neues Projekt exportieren“ können Sie anhand einer vorhandenen Data Factory ein Visual Studio-Projekt erstellen.

    ![Exportieren einer Data Factory in ein Visual Studio-Projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualisieren von Data Factory-Tools für Visual Studio
Führen Sie die folgenden Schritte aus, um die Azure Data Factory-Tools für Visual Studio zu aktualisieren:

1. Klicken Sie im Menü auf **Extras**, und wählen Sie **Erweiterungen und Updates** aus. 
2. Wählen Sie im linken Bereich **Updates** und dann **Visual Studio Gallery** aus.
3. Wählen Sie **Azure Data Factory tools for Visual Studio** aus, und klicken Sie auf **Update**. Wenn dieser Eintrag nicht angezeigt wird, verfügen Sie bereits über die neueste Version der Tools. 

Unter [Überwachen von Datasets und Pipelines](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) erfahren Sie, wie Sie die in diesem Tutorial erstellte Pipeline und die entsprechenden Datasets mithilfe des Azure-Portals überwachen können.

## <a name="see-also"></a>Weitere Informationen
| Thema | Beschreibung |
|:--- |:--- |
| [Pipelines](data-factory-create-pipelines.md) |Dieser Artikel enthält Informationen zu Pipelines und Aktivitäten in Azure Data Factory. |
| [Datasets](data-factory-create-datasets.md) |Dieser Artikel enthält Informationen zu Datasets in Azure Data Factory. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) |In diesem Artikel werden die Planungs- und Ausführungsaspekte des Azure Data Factory-Anwendungsmodells erläutert. |
| [Überwachen und Verwalten von Pipelines mit der Überwachungs-App](data-factory-monitor-manage-app.md) |In diesem Artikel wird das Überwachen, Verwalten und Debuggen von Pipelines mit der App für die Überwachung und Verwaltung beschrieben. |


