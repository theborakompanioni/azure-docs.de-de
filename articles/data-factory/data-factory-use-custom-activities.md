---
title: "Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline"
description: "Erfahren Sie, wie Sie benutzerdefinierte Aktivitäten erstellen und in einer Azure Data Factory-Pipeline verwenden."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: b4b0a8139b69a31e4af40e1f8231d4d7772fee0b
ms.lasthandoff: 03/18/2017


---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-Aktivität](data-factory-hive-activity.md) 
> * [Pig-Aktivität](data-factory-pig-activity.md)
> * [MapReduce-Aktivität](data-factory-map-reduce.md)
> * [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
> * [Spark-Aktivität](data-factory-spark.md)
> * [Machine Learning-Batchausführungsaktivität](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-Ressourcenaktualisierungsaktivität](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivität „Gespeicherte Prozedur“](data-factory-stored-proc-activity.md)
> * [U-SQL-Aktivität für Data Lake Analytics](data-factory-usql-activity.md)
> * [Benutzerdefinierte .NET-Aktivität](data-factory-use-custom-activities.md)


Es existieren zwei Aktivitätstypen, die Sie in einer Azure Data Factory-Pipeline verwenden können.

- [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) zum Verschieben von Daten zwischen [unterstützten Quell- und Senkendatenspeichern](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) zum Transformieren von Daten mithilfe von Computediensten wie Azure HDInsight, Azure Batch und Azure Machine Learning. 

Erstellen Sie zum Verschieben von Daten in oder aus einem Datenspeicher, der von Data Factory nicht unterstützt wird, eine **benutzerdefinierte Aktivität** mit Ihrer eigenen Datenverschiebungslogik, und verwenden Sie die Aktivität in einer Pipeline. Wenn Sie Daten auf eine Weise transformieren/verarbeiten müssen, die von Data Factory nicht unterstützt wird, können Sie auch eine benutzerdefinierte Aktivität mit Ihrer eigenen Datentransformationslogik erstellen und in einer Pipeline verwenden. 

Sie können eine benutzerdefinierte Aktivität so konfigurieren, dass sie in einem **Azure Batch**-Pool mit virtuellen Computern oder in einem **Azure HDInsight**-Cluster ausgeführt wird. Bei Verwendung von Azure Batch können Sie nur einen vorhandenen Azure Batch-Pool verwenden. Beim Verwenden von HDInsight können Sie dagegen einen vorhandenen HDInsight-Cluster oder einen Cluster verwenden, der zur Laufzeit für Sie automatisch bedarfsabhängig erstellt wird.  

Die folgende exemplarische Vorgehensweise bietet Schritt-für-Schritt-Anleitungen zum Erstellen einer benutzerdefinierten .NET-Aktivität und zur Verwendung der benutzerdefinierten Aktivität in einer Pipeline. Für die exemplarische Vorgehensweise wird ein mit **Azure Batch** verknüpfter Dienst verwendet. Falls Sie stattdessen einen verknüpften Azure HDInsight-Dienst verwenden möchten, erstellen Sie einen verknüpften Dienst vom Typ **HDInsight** (eigener HDInsight-Cluster) oder **HDInsightOnDemand** (Data Factory erstellt bedarfsabhängig einen HDInsight-Cluster). Konfigurieren Sie anschließend die benutzerdefinierte Aktivität für die Verwendung des verknüpften HDInsight-Diensts. Weitere Informationen zur Verwendung von Azure HDInsight zum Ausführen der benutzerdefinierten Aktivität finden Sie im Abschnitt [Verwenden von mit Azure HDInsight verknüpften Diensten](#use-hdinsight-compute-service) .

> [!NOTE]
> Ein Datenverwaltungsgateway kann nicht über eine benutzerdefinierte Aktivität verwendet werden, um auf lokale Datenquellen zuzugreifen. Das [Datenverwaltungsgateway](data-factory-data-management-gateway.md) unterstützt derzeit nur die Kopieraktivität und die Aktivität mit gespeicherten Prozeduren in Data Factory.   

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
### <a name="prerequisites"></a>Voraussetzungen
* Visual Studio 2012/2013/2015
* Laden Sie das [Azure .NET SDK][azure-developer-center] herunter, und installieren Sie es.

### <a name="azure-batch-prerequisites"></a>Azure Batch-Voraussetzungen
In der exemplarischen Vorgehensweise führen Sie Ihre benutzerdefinierten .NET-Aktivitäten mit Azure Batch als Computeressource aus. Bei **Azure Batch** handelt es sich um eine Plattform zur Ausführung umfangreicher paralleler und leistungsstarker Anwendungen (High Performance Computing, HPC) in der Cloud. Azure Batch plant die Ausführung rechenintensiver Aufgaben mit einer verwalteten **Sammlung virtueller Computer** und kann Computeressourcen automatisch skalieren, um den Anforderungen Ihrer Aufträge gerecht zu werden. Eine ausführliche Übersicht über den Azure Batch-Dienst finden Sie im Artikel [Azure Batch – Grundlagen][batch-technical-overview].

Erstellen Sie für das Tutorial ein Azure Batch-Konto mit einem VM-Pool. Gehen Sie wie folgt vor:

1. Erstellen Sie über das **Azure-Portal** ein [Azure Batch-Konto](http://portal.azure.com). Anweisungen finden Sie im Artikel [Erstellen und Verwalten eines Azure Batch-Kontos][batch-create-account].
2. Notieren Sie sich den Azure Batch-Kontonamen, Kontoschlüssel, URI und Poolnamen. Sie benötigen diese Angaben, um einen verknüpften Azure Batch-Dienst zu erstellen.
    1. Auf der Startseite des Azure Batch-Kontos wird eine **URL** im folgenden Format angezeigt: **https://myaccount.westus.batch.azure.com**. In diesem Beispiel ist **myaccount** der Name des Azure Batch-Kontos. Der URI, den Sie in der Definition für den verknüpften Dienst verwenden, ist die URL ohne den Kontonamen. Beispiel: **https://westus.batch.azure.com**.
    2. Klicken Sie links im Menü auf **Schlüssel**, und kopieren Sie den **PRIMÄREN ZUGRIFFSSCHLÜSSEL**.
    3. Klicken Sie zum Verwenden eines vorhandenen Pools im Menü auf **Pools**, und notieren Sie sich die **ID** des Pools. Fahren Sie mit dem nächsten Schritt fort, falls Sie keinen vorhandenen Pool besitzen.     
2. Erstellen Sie einen **Azure Batch-Pool**.

   1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Menü auf **Durchsuchen** und dann auf **Batch-Konten**.
   2. Wählen Sie Ihr Azure Batch-Konto aus, um das Blatt **Batch-Konto** zu öffnen.
   3. Klicken Sie auf die Kachel **Pools** .
   4. Klicken Sie auf dem Blatt **Pools** auf der Symbolleiste auf die Schaltfläche „Hinzufügen“, um einen Pool hinzuzufügen.
      1. Geben Sie eine ID für den Pool ein (**Pool-ID**). Notieren Sie sich die **ID des Pools**. Sie wird beim Erstellen der Data Factory-Projektmappe benötigt.
      2. Geben Sie **Windows Server 2012 R2** für die Einstellung „Betriebssystem-Familie“ ein.
      3. Wählen Sie einen **Knotentarif**aus.
      4. Geben Sie **2** als Wert für die Einstellung **Zuordnung für Ziel** ein.
      5. Geben Sie **2** als Wert für die Einstellung **Max. Tasks pro Knoten** ein.
   5. Klicken Sie auf **OK** , um den Pool zu erstellen.
   6. Notieren Sie sich die **ID** des Pools. 



### <a name="high-level-steps"></a>Allgemeine Schritte
Hier sind die beiden allgemeinen Schritte angegeben, die Sie im Rahmen dieser exemplarischen Vorgehensweise ausführen: 

1. Erstellen Sie eine benutzerdefinierte Aktivität, die eine einfache Logik für die Datentransformation/-verarbeitung enthält.
2. Erstellen Sie eine Instanz von Azure Data Factory mit einer Pipeline, die die benutzerdefinierte Aktivität verwendet.

## <a name="create-a-custom-activity"></a>Erstellen einer benutzerdefinierten Aktivität
Um eine benutzerdefinierte .NET-Aktivität zu erstellen, müssen Sie ein **.NET-Klassenbibliotheksprojekt** mit einer Klasse erstellen, die die **IDotNetActivity**-Schnittstelle implementiert. Diese Schnittstelle verfügt lediglich über eine Methode: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) , und ihre Signatur ist:

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


Die Methode akzeptiert vier Parameter:

- **LinkedServices**. Diese Eigenschaft ist eine aufzählbare Liste mit verknüpften Datenspeicherdiensten, auf die von Eingabe-/Ausgabedatasets für die Aktivität verwiesen wird.   
- **Datasets**. Diese Eigenschaft ist eine aufzählbare Liste mit Eingabe-/Ausgabedatasets für die Aktivität. Sie können diesen Parameter zum Abrufen der Speicherorte und Schemas verwenden, die von den Eingabe- und Ausgabedatasets definiert werden.
- **Aktivität**. Diese Eigenschaft steht für die aktuelle Aktivität. Sie kann zum Zugreifen auf erweiterte Eigenschaften verwendet werden, die der benutzerdefinierten Aktivität zugeordnet sind. Ausführliche Informationen finden Sie unter [Zugreifen auf erweiterte Eigenschaften](#access-extended-properties).
- **Protokollierungstool**. Mit diesem Objekt können Sie Debugkommentare schreiben, die dann als Benutzerprotokoll für die Pipeline erscheinen.

Die Methode gibt ein Wörterbuch zurück, das künftig zum Verketten benutzerdefinierter Aktivitäten verwendet werden kann. Dieses Feature ist noch nicht implementiert. Von der Methode wird also ein leeres Wörterbuch zurückgegeben.  

### <a name="procedure"></a>Vorgehensweise
1. Erstellen Sie ein **.NET-Klassenbibliotheksprojekt** .
   <ol type="a">
     <li>Starten Sie <b>Visual Studio 2015</b>, <b>Visual Studio 2013</b> oder <b>Visual Studio 2012</b>.</li>
     <li>Klicken Sie auf <b>Datei</b>, zeigen Sie auf <b>Neu</b>, und klicken Sie auf <b>Projekt</b>.</li>
     <li>Erweitern Sie <b>Vorlagen</b>, und wählen Sie <b>Visual C#</b> aus. In dieser exemplarischen Vorgehensweise verwenden Sie C#, Sie können jedoch jede .NET-Sprache verwenden, um benutzerdefinierte Aktivität zu entwickeln.</li>
     <li>Wählen Sie in der Liste mit den Projekttypen auf der rechten Seite den Eintrag <b>Klassenbibliothek</b> aus.</li>
     <li>Geben Sie <b>MyDotNetActivity</b> als <b>Namen</b> ein.</li>
     <li>Wählen Sie <b>C:\ADFGetStarted</b> als <b>Speicherort</b>.</li>
     <li>Klicken Sie auf <b>OK</b> , um das Projekt zu erstellen.</li>
   </ol>
2. Klicken Sie auf **Extras**, zeigen Sie auf **NuGet-Paket-Manager**, und klicken Sie auf **aket-Manager-Konsole**.
3. Führen Sie in der Paket-Manager-Konsole den folgenden Befehl zum Importieren von **Microsoft.Azure.Management.DataFactories**aus.

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importieren Sie das **Azure Storage** NuGet-Paket in das Projekt.

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Das Startprogramm des Data Factory-Diensts erfordert Version 4.3 von WindowsAzure.Storage. Wenn Sie in Ihrem benutzerdefinierten Aktivitätsprojekt einen Verweis auf eine höhere Version der Azure Storage-Assembly hinzufügen, wird bei der Ausführung der Aktivität ein Fehler angezeigt. Informationen zum Beheben des Fehlers finden Sie im Abschnitt [Anwendungsdomänenisolierung](#appdomain-isolation). 
5. Fügen Sie die folgenden **using** -Anweisungen der Quelldatei im Projekt hinzu.

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Ändern Sie den Namen des **Namespace** in **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Ändern Sie den Namen der Klasse in **MyDotNetActivity**, und leiten Sie ihn von der **IDotNetActivity**-Schnittstelle ab, wie im folgenden Codeausschnitt zu sehen.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementieren bzw. fügen Sie die **Execute**-Methode der **IDotNetActivity**-Schnittstelle der **MyDotNetActivity**-Klasse hinzu, und kopieren Sie den folgenden Beispielcode in die Methode.

    Das folgende Beispiel zählt, wie oft der Suchbegriff („Microsoft“) in jedem Blob, das einem Datenslice zugeordnet ist, vorkommt.

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    
    public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
    {
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // to log information, use the logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get the input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables to hold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from the dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and the other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get the first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using the same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get the connection string in the linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get the folder path from the input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.
    
        // create storage client for input. Pass the connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
        // initialize the continuation token before using it in the do-while loop.
        BlobContinuationToken continuationToken = null;
        do
        {   // get the list of input blobs from the input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                     true,
                                     BlobListingDetails.Metadata,
                                     null,
                                     continuationToken,
                                     null,
                                     null);
    
            // Calculate method returns the number of occurrences of
            // the search term (“Microsoft”) in each blob associated
               // with the data slice. definition of the method is shown in the next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for the output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get the folder path from the output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log the output folder path    
        logger.Write("Writing blob to the folder: {0}", folderPath);
    
        // create a storage object for the output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log the output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);
    
        // The dictionary can be used to chain custom activities together in the future.
        // This feature is not implemented yet, so just return an empty dictionary.  
    
        return new Dictionary<string, string>();
    }
    ```
9. Fügen Sie die folgenden Hilfsmethoden hinzu: 

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    
    private static string GetFolderPath(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of the dataset    
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the folder path found in the type properties
        return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets the fileName value from the input/output dataset.   
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }
    
        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the blob/file name in the type properties
        return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
        string output = string.Empty;
        logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
        foreach (IListBlobItem listBlobItem in Bresult.Results)
        {
            CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
            if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
            {
                string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                logger.Write("input blob text: {0}", blobText);
                string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                var matchQuery = from word in source
                                 where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                 select word;
                int wordCount = matchQuery.Count();
                output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
            }
        }
        return output;
    }
    ```

    Die „GetFolderPath“-Methode gibt den Pfad zum Ordner zurück, auf den das Dataset verweist, und die „GetFileName“-Methode gibt den Namen des Blobs oder der Datei zurück, auf das oder die das Dataset verweist. Wenn Sie Ordnerpfade mit Variablen wie „{Year}“, „{Month}“, „{Day}“ oder Ähnlichem definiert haben, gibt die Methode die Zeichenfolge zurück, ohne diese Variablen durch Laufzeitwerte zu ersetzen. Weitere Informationen zum Zugreifen auf „SliceStart“, „SliceEnd“ usw. finden Sie im Abschnitt [Zugreifen auf erweiterte Eigenschaften](#access-extended-properties).    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    Die „Calculate“-Methode berechnet die Anzahl der Instanzen des Schlüsselworts „Microsoft“ in den Eingabedateien (Blobs im Ordner). Der Suchbegriff („Microsoft“) ist hartcodiert.
10. Kompilieren Sie das Projekt. Klicken Sie im Menü auf **Erstellen** und dann auf **Projektmappe erstellen**.

    > [!IMPORTANT]
    > Legen Sie Version 4.5.2 von .NET Framework als Zielframework für Ihr Projekt fest: Klicken Sie mit der rechten Maustaste auf das Projekt, und klicken Sie auf **Eigenschaften**, um das Zielframework festzulegen. Data Factory unterstützt keine benutzerdefinierten Aktivitäten, die mit .NET Framework-Versionen nach 4.5.2 kompiliert wurden.

11. Starten Sie **Windows-Explorer**, und navigieren Sie je nach Buildtyp zum Ordner **bin\debug** oder **bin\release**.
12. Erstellen Sie die ZIP-Datei **MyDotNetActivity.zip**, die alle Binärdateien im Ordner <project folder>\bin\Debug enthält. Schließen Sie die Datei **MyDotNetActivity.pdb** ein, damit Sie im Falle eines Fehlers zusätzliche Details erhalten, z.B. die Nummer der Quellcodezeile, die das Problem verursacht hat. 

    > [!IMPORTANT]
    > Alle Dateien in der ZIP-Datei für die benutzerdefinierte Aktivität müssen sich auf der **obersten Ebene** befinden und dürfen keine Unterordner besitzen.

    ![Binäre Ausgabedateien](./media/data-factory-use-custom-activities/Binaries.png)
14. Erstellen Sie einen Blobcontainer mit dem Namen **customactivitycontainer**, sofern er noch nicht vorhanden ist.
15. Laden Sie „MyDotNetActivity.zip“ als Blob in den customactivitycontainer in einem Azure-Blobspeicher für **allgemeine Zwecke** hoch (keinen Blobspeicher vom Typ „Hot/Cool“), auf den von AzureStorageLinkedService verwiesen wird.  

> [!NOTE]
> Wenn Sie dieses .NET-Aktivitätsprojekt einer Lösung in Visual Studio hinzufügen, die ein Data Factory-Projekt enthält, und dem .NET-Aktivitätsprojekt einen Verweis vom Data Factory-Anwendungsprojekt hinzufügen, müssen Sie die letzten beiden Schritte nicht durchführen (ZIP-Datei selbst erstellen und in den Azure-Blobspeicher für allgemeine Zwecke hochladen). Beim Veröffentlichen von Data Factory-Entitäten mit Visual Studio werden diese Schritte automatisch durch den Veröffentlichungsprozess ausgeführt. In den Artikeln [Erstellen der ersten Pipeline mit Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) und [Kopieren von Daten aus einem Azure-Blob in Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) erfahren Sie mehr über das Erstellen und Veröffentlichen von Data Factory-Entitäten mit Visual Studio.  


## <a name="create-a-data-factory"></a>Erstellen einer Data Factory 
Sie haben eine benutzerdefinierte Aktivität erstellt und die ZIP-Datei mit Binärdateien in einen Blobcontainer in einem Azure Storage-Konto für **allgemeine Zwecke** hochgeladen. In diesem Abschnitt erstellen Sie eine Instanz von Azure Data Factory mit einer Pipeline, die die benutzerdefinierte Aktivität verwendet.

Das Eingabedataset für die benutzerdefinierte Aktivität stellt Blobs (Dateien) im Ordner „customactivityinput“ des adftutorial-Containers im Blobspeicher dar. Das Ausgabedataset für die Aktivität stellt Ausgabeblobs im Ordner „customactivityoutput“ des adftutorial-Containers im Blobspeicher dar.

Erstellen Sie die Datei **file.txt** mit dem folgenden Inhalt, und laden Sie den Ordner **customactivityinput** des **adftutorial**-Containers hoch. Erstellen Sie den Container „adftutorial“, falls er nicht bereits vorhanden ist. 

```
test custom activity Microsoft test custom activity Microsoft
```

Der Eingabeordner entspricht einem Slice in Azure Data Factory, auch wenn der Ordner mehrere Dateien enthält. Wenn jeder Slice von der Pipeline verarbeitet wird, durchläuft die benutzerdefinierte Aktivität alle Blobs im Eingabeordner für diesen Slice.

Im Ordner „adftutorial\customactivityoutput“ befindet sich eine Ausgabedatei mit mindestens einer Zeile (identisch mit der Anzahl von Blobs im Eingabeordner):

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


In diesem Abschnitt erstellen Sie Folgendes:

1. Eine **Data Factory**.
2. Erstellen Sie **verknüpfte Dienste** für den Azure Batch-Pool mit virtuellen Computern, auf denen die benutzerdefinierte Aktivität ausgeführt wird, und die Azure Storage-Instanz, die die Eingabe- und Ausgabeblobs enthält.
3. Erstellen Sie Eingabe- und Ausgabe**datasets**, die die Eingabe und Ausgabe der benutzerdefinierten Aktivität darstellen.
4. Erstellen Sie eine **Pipeline**, die die benutzerdefinierte Aktivität verwendet.

> [!NOTE]
> Erstellen Sie die Datei **file.txt** , und laden Sie sie in einen Blobcontainer hoch, sofern nicht bereits geschehen. Hier hilft Ihnen die Anleitung im vorherigen Abschnitt weiter.   

### <a name="step-1-create-the-data-factory"></a>Schritt 1: Erstellen der Data Factory
1. Gehen Sie nach der Anmeldung beim Azure-Portal wie folgt vor:
   1. Klicken Sie im linken Menü auf **NEU** .
   2. Klicken Sie auf dem Blatt **Neu** auf **Daten und Analysen**.
   3. Klicken Sie auf dem Blatt **Datenanalyse** auf **Data Factory**.
2. Geben Sie auf dem Blatt **Neue Data Factory** als Namen die Zeichenfolge **CustomActivityFactory** ein. Der Name der Azure Data Factory muss global eindeutig sein. Bei Anzeige der Fehlermeldung **Data Factory-Name „CustomActivityFactory“ ist nicht verfügbar** ändern Sie den Namen der Data Factory (z. B.in **IhrNameCustomActivityFactory**) und wiederholen den Vorgang.
3. Klicken Sie auf **RESSOURCENGRUPPENNAME**, und wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe.
4. Stellen Sie sicher, dass Sie das richtige **Abonnement** und die richtige **Region** zum Erstellen der Data Factory verwenden.
5. Klicken Sie auf dem Blatt **Neue Data Factory** auf **Erstellen**.
6. Im **Dashboard** des Azure-Portals sehen Sie, dass die Data Factory erstellt wird.
7. Nach erfolgreicher Erstellung der Data Factory wird das Data Factory-Blatt mit dem Inhalt der Data Factory angezeigt.

### <a name="step-2-create-linked-services"></a>Schritt 2: Erstellen von verknüpften Diensten
Verknüpfte Dienste verknüpfen Datenspeicher oder Serverdienste mit einer Azure Data Factory. In diesem Schritt verknüpfen Sie Ihr Azure-Speicherkonto und Ihr Azure Batch-Konto mit Ihrer Data Factory.

#### <a name="create-azure-storage-linked-service"></a>Erstellen des mit Azure Storage verknüpften Diensts
1. Klicken Sie auf die Kachel **Erstellen und Bereitstellen** auf dem Blatt **DATA FACTORY** für **CustomActivityFactory**. Der Data Factory-Editor wird angezeigt.
2. Klicken Sie in der Befehlsleiste auf **Neuer Datenspeicher** und wählen Sie **Azure Storage**. Das JSON-Skript zum Erstellen eines mit einem Azure-Speicher verknüpften Diensts sollte im Editor angezeigt werden.

3. Ersetzen Sie **account name** durch den Namen Ihres Azure-Speicherkontos und **account key** durch den Zugriffsschlüssel des Azure-Speicherkontos. Informationen zum Abrufen Ihres Speicherzugriffsschlüssels finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md#manage-your-storage-account).

4. Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um den verknüpften Dienst bereitzustellen.

#### <a name="create-azure-batch-linked-service"></a>Verwenden des mit Azure Batch verknüpften Dienstes
1. Klicken Sie im Data Factory-Editor auf der Befehlsleiste auf **Neu berechnen**, und wählen Sie im Menü **Azure Batch** aus.
2. Nehmen Sie die folgenden Änderungen am JSON-Skript vor:

   1. Geben Sie den Azure Batch-Kontonamen für die **accountName** -Eigenschaft an. Die **URL** auf dem Blatt **Azure-Batch-Konto** hat folgendes Format: http://**Kontoname**.Region.batch.azure.com.m. Für die **batchUri**-Eigenschaft in JSON müssen Sie **„Kontoname“** aus der URL entfernen und den **Kontonamen** für die JSON-Eigenschaft **accountName** verwenden.
   2. Geben Sie den Azure Batch-Kontoschlüssel für die **accessKey** -Eigenschaft an.
   3. Geben Sie den Namen des Pools, den Sie als Teil der Voraussetzungen erstellt haben, für die **poolName** -Eigenschaft an. Sie können anstelle des Poolnamens auch die ID des Pools angeben.
   4. Geben Sie den Azure Batch-URI für die **batchUri** -Eigenschaft an. Beispiel: https://westus.batch.azure.com.  
   5. Geben Sie **AzureStorageLinkedService** for the **linkedServiceName** -Eigenschaft an.

        ```JSON
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       Für die **poolName** -Eigenschaft können Sie auch die ID des Pools anstelle des Namens des Pools angeben.

      > [!IMPORTANT]
      > Data Factory unterstützt im Gegensatz zu HDInsight für Azure Batch keine bedarfsorientierte Option. Sie können nur Ihren eigenen Azure Batch-Pool in einer Azure Data Factory verwenden.   
    

### <a name="step-3-create-datasets"></a>Schritt 3: Erstellen von Datasets
In diesem Schritt erstellen Sie Datasets zur Darstellung von Eingabe- und Ausgabedaten.

#### <a name="create-input-dataset"></a>Erstellen eines Eingabedatasets
1. Klicken Sie im **Editor** für die Data Factory auf der Symbolleiste auf **Neues Dataset** und anschließend im Dropdownmenü auf **Azure-Blobspeicher**.
2. Ersetzen Sie den JSON-Code im rechten Bereich durch den folgenden JSON-Codeausschnitt:

    ```JSON
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
         },
         "availability": {
             "frequency": "Hour",
             "interval": 1
         },
         "external": true,
         "policy": {}
     }
    }
    ```

   Im weiteren Verlauf dieser exemplarischen Vorgehensweise wird eine Pipeline mit der Startzeit „2016-11-16T00:00:00Z“ und der Endzeit „2016-11-16T05:00:00Z“ erstellt. Sie ist so eingerichtet, dass Daten stündlich erstellt werden, sodass sich fünf Eingabe-/Ausgabeslices (zwischen **00**:00:00 und **05**:00:00) ergeben.

   Die **Häufigkeit** und das **Intervall** für das Eingabedataset ist auf **Hour** und **1** festgelegt, was bedeutet, dass der Eingabeslice stündlich verfügbar ist. In diesem Beispiel nehmen wir die gleiche Datei (file.txt) im Eingabeordner.

   Dies sind die Startzeiten für jeden Slice, die durch die Systemvariable „SliceStart“ im obigen JSON-Codeausschnitt dargestellt werden.
3. Klicken Sie in der Symbolleiste auf **Bereitstellen**, um **InputDataset** bereitzustellen. Vergewissern Sie sich, dass die Meldung **TABELLE ERFOLGREICH ERSTELLT** auf der Titelleiste des Editors angezeigt wird.

#### <a name="create-an-output-dataset"></a>Erstellen eines Ausgabedatasets
1. Klicken Sie im **Data Factory-Editor** auf **Neues Dataset** und dann auf der Befehlsleiste auf **Azure-Blobspeicher**.
2. Ersetzen Sie das JSON-Skript im rechten Bereich durch das folgende JSON-Skript:

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
                "partitionedBy": [
                    {
                        "name": "slice",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy-MM-dd-HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

     Der Ausgabespeicherort ist **adftutorial/customactivityoutput/** , und der Name der Ausgabedatei lautet „yyyy-MM-dd-HH.txt“, wobei „yyyy-MM-dd-HH“ Jahr, Monat, Datum und Stunde der Sliceerstellung angibt. Ausführlichere Informationen finden Sie in der [Entwicklerreferenz][adf-developer-reference].

    Eine Ausgabedatei und ein Ausgabe-Blob wird für jeden Eingabeslice generiert. Im Folgenden sehen Sie, wie eine Ausgabedatei für jeden Slice benannt wird. Alle Ausgabedateien werden in einem Ausgabeordner generiert: **adftutorial\customactivityoutput**.

   | Slice | Startzeit | Ausgabedatei |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016-11-16-00.txt |
   | 2 |2016-11-16T01:00:00 |2016-11-16-01.txt |
   | 3 |2016-11-16T02:00:00 |2016-11-16-02.txt |
   | 4 |2016-11-16T03:00:00 |2016-11-16-03.txt |
   | 5 |2016-11-16T04:00:00 |2016-11-16-04.txt |

    Denken Sie daran, dass alle Dateien in einem Eingabeordner zu einem Slice mit den oben genannten Startzeiten gehören. Wenn dieser Slice verarbeitet wird, durchsucht die benutzerdefinierte Aktivität jede Datei und erzeugt eine Zeile in der Ausgabedatei mit der Anzahl der Vorkommnisse des Suchbegriffs („Microsoft“). Wenn im Eingabeordner drei Dateien vorhanden sind, enthält die Ausgabedatei drei Zeilen für jeden stündlichen Slice: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt usw.
3. Klicken Sie in der Befehlsleiste auf **Bereitstellen**, um das **OutputDataset** bereitzustellen.

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a>Erstellen Sie eine Pipeline, die die benutzerdefinierte Aktivität verwendet, und führen Sie sie aus.
1. Klicken Sie im Data Factory-Editor auf der Befehlsleiste auf die Schaltfläche **Neue Pipeline** . Wenn der Befehl nicht angezeigt wird, klicken Sie auf **... (Auslassungspunkte)**, um ihn anzuzeigen.
2. Ersetzen Sie das JSON-Skript im rechten Bereich durch das folgende JSON-Skript.

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    Beachten Sie folgende Punkte:

   * **Concurrency** ist auf **2** festgelegt, sodass zwei Slices parallel von zwei virtuellen Computern im Azure Batch-Pool verarbeitet werden.
   * Im Abschnitt "activities" gibt es eine Aktivität vom Typ **DotNetActivity**.
   * **AssemblyName** wird auf den Namen der DLL festgelegt: **MyDotnetActivity.dll**.
   * **EntryPoint** wird auf **MyDotNetActivityNS.MyDotNetActivity** festgelegt.
   * **PackageLinkedService** ist auf **AzureStorageLinkedService** festgelegt, das auf den Blobspeicher verweist, der die ZIP-Datei mit der benutzerdefinierten Aktivität enthält. Wenn Sie für die Eingabe- und Ausgabedateien und die ZIP-Datei mit der benutzerdefinierten Aktivität unterschiedliche Azure-Speicherkonten verwenden, wird ein weiterer verknüpfter Azure Storage-Dienst erstellt. Dieser Artikel setzt voraus, dass Sie das gleiche Azure-Speicherkonto verwenden.
   * **PackageFile** wird auf **customactivitycontainer/MyCustomActivity.zip** festgelegt. Das entspricht dem Format: containerforthezip/nameofthezip.zip.
   * Die benutzerdefinierte Aktivität verwendet **InputDataset** als Eingabe und **OutputDataset** als Ausgabe.
   * Die Eigenschaft „linkedServiceName“ der benutzerdefinierten Aktivität zeigt auf **AzureBatchLinkedService**. Dadurch wird Azure Data Factory mitgeteilt, dass die benutzerdefinierte Aktivität auf virtuellen Azure Batch-Computern ausgeführt werden muss.
   * Die Eigenschaft **isPaused** ist standardmäßig auf **false** festgelegt. Die Pipeline wird in diesem Beispiel sofort ausgeführt, da die Slices in der Vergangenheit starten. Legen Sie diese Eigenschaft auf „true“ fest, um die Pipeline anzuhalten, und legen Sie sie zum Neustart wieder auf „false“ fest.
   * Die Zeiten **start** und **end** liegen **fünf** Stunden auseinander, und Slices werden stündlich erstellt, sodass insgesamt fünf Slices von der Pipeline erstellt werden.
3. Klicken Sie in der Befehlsleiste auf **Bereitstellen**, um die Pipeline bereitzustellen.

### <a name="monitor-the-pipeline"></a>Überwachen der Pipeline
1. Klicken Sie im Azure-Portal auf dem Blatt „Data Factory“ auf **Diagramm**.

    ![Kachel "Diagramm"](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. Klicken Sie in der Diagrammansicht auf „OutputDataset“.

    ![Diagrammansicht](./media/data-factory-use-custom-activities/diagram.png)
3. Es sollte angezeigt werden, dass sich fünf Ausgabeslices im Status „Bereit“ befinden. Wenn sie noch nicht den Status „Bereit“ aufweisen, wurden sie noch nicht erstellt. 

   ![Ausgabeslices](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Stellen Sie sicher, dass die Ausgabedateien im Blobspeicher im **adftutorial** -Container generiert werden.

   ![Ausgabe der benutzerdefinierten Aktivität][image-data-factory-ouput-from-custom-activity]
5. Wenn Sie die Ausgabedatei öffnen, sollte die Ausgabe der folgenden ähneln:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. Verwenden Sie zum Überwachen Ihrer Data Factory, Pipelines und Datasets das [Azure-Portal][azure-preview-portal] oder Azure PowerShell-Cmdlets. Sie können die Meldungen vom **ActivityLogger** im Code für die benutzerdefinierte Aktivität in den Protokollen (insbesondere "user-0.log") sehen, die Sie aus dem Portal oder mithilfe von Cmdlets herunterladen können.

   ![Herunterladen von Protokollen aus einer benutzerdefinierten Aktivität][image-data-factory-download-logs-from-custom-activity]

Unter [Überwachen und Verwalten von Pipelines](data-factory-monitor-manage-pipelines.md) finden Sie eine ausführliche Anleitung zum Überwachen von Datasets und Pipelines.      

### <a name="data-factory-and-batch-integration"></a>Integration von Data Factory und Batch
Der Data Factory-Dienst erstellt in Azure Batch einen Auftrag mit dem Namen **adf-poolname: job-xxx**. Klicken Sie im Menü links auf **Aufträge**. 

![Azure Data Factory – Batch-Aufträge](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Bei jeder Aktivitätsausführung eines Slices wird eine Aufgabe erstellt. Wenn fünf Slices für die Verarbeitung bereit sind, werden in diesem Auftrag fünf Aufgaben erstellt. Falls der Batch-Pool mehrere Computeknoten enthält, werden zwei oder mehr Slices parallel ausgeführt. Sie können auch mehrere Slices auf demselben Computeknoten ausführen, wenn die maximale Anzahl von Aufgaben pro Computeknoten auf mehr als 1 festgelegt ist.

![Azure Data Factory – Aufgaben des Batch-Auftrags](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

Das folgende Diagramm veranschaulicht die Beziehung zwischen Azure Data Factory- und Batch-Aufgaben.

![Data Factory und Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Beheben von Fehlern
Die Problembehandlung umfasst einige grundlegende Verfahren:

1. Wenn der folgende Fehler angezeigt wird, kann es sein, dass Sie anstelle eines Azure-Blobspeichers für allgemeine Zwecke einen Blobspeicher vom Typ „Hot/Cool“ verwenden. Laden Sie die ZIP-Datei in ein **Azure Storage-Konto für allgemeine Zwecke** hoch. 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ``` 
2. Wenn der folgende Fehler angezeigt wird, sollten Sie sich vergewissern, dass der Name der Klasse in der CS-Datei mit dem Namen übereinstimmt, den Sie für die Eigenschaft **EntryPoint** im JSON-Code für die Pipeline angegeben haben. In der exemplarischen Vorgehensweise lautet der Name der Klasse „MyDotNetActivity“. „EntryPoint“ in JSON lautet: MyDotNetActivityNS.**MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Wenn die Namen übereinstimmen, vergewissern Sie sich, dass sich alle Binärdateien im **Stammordner** der ZIP-Datei befinden. Sprich: Wenn Sie die ZIP-Datei öffnen, müssen alle Dateien im Stammordner (nicht in Unterordnern) angezeigt werden.   
3. Wenn der Eingabeslice nicht auf **Bereit**festgelegt ist, stellen Sie sicher, dass die Struktur des Eingabeordners korrekt ist und die Datei **file.txt** im Eingabeordner vorhanden ist.
3. Verwenden Sie in der **Execute-Method**e der benutzerdefinierten Aktivität das **IActivityLogger-Objekt**, um Informationen zu protokollieren, die beim Behandeln von Problemen hilfreich sind. Die protokollierten Meldungen werden in den Benutzerprotokolldateien („user-0.log“, „user-1.log“, „user-2.log“ usw.) angezeigt.

   Klicken Sie auf dem Blatt **OutputDataset** auf den Slice, um das Blatt **DATENSLICE** für diesen Slice anzuzeigen. Die **Aktivitätsausführungen** für diesen Slice werden angezeigt. Sie sollten genau eine Aktivitätsausführung für den Slice sehen. Wenn Sie in der Befehlsleiste auf „Ausführen“ klicken, können Sie für den gleichen Slice eine weitere Aktivität ausführen.

   Wenn Sie auf die Aktivitätsausführung klicken, wird das Blatt **AKTIVITÄTSAUSFÜHRUNG – DETAILS** mit einer Protokolldateienliste angezeigt. Protokollierte Nachrichten werden in der Datei „user_0.log“ angezeigt. Tritt ein Fehler auf, werden drei Aktivitätsausführungen angezeigt, da die Anzahl von Wiederholungsversuchen in der Pipeline oder Aktivitäts-JSON auf „3“ festgelegt ist. Wenn Sie die Aktivitätsausführung starten, werden die Protokolldateien angezeigt, die Sie zum Behandeln von Fehlern überprüfen können.

   Klicken Sie in der Liste der Protokolldateien auf die Datei **user-0.log**. Im rechten Bereich werden die Ergebnisse der Verwendung der **IActivityLogger.Write** -Methode angezeigt. Wenn Sie nicht alle Meldungen sehen, überprüfen Sie, ob Sie mehrere Protokolldateien mit den folgenden Namen haben: user_1.log, user_2.log etc. Andernfalls war der Code nach der letzten protokollierten Meldung möglicherweise fehlerhaft.

   Prüfen Sie auch die Datei **system-0.log** auf Systemfehlermeldungen und -ausnahmen.
4. Schließen Sie die **PDB-Datei** in die ZIP-Datei ein, sodass in den Fehlerdetails Informationen wie etwa die **Aufrufliste** zur Verfügung stehen, wenn ein Fehler auftritt.
5. Alle Dateien in der ZIP-Datei für die benutzerdefinierte Aktivität müssen sich auf der **obersten Ebene** befinden und dürfen keine Unterordner besitzen.
6. Stellen Sie sicher, dass **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) und **packageLinkedService** (sollte auf den Azure-Blobspeicher für **allgemeine Zwecke** verweisen, der die ZIP-Datei enthält) auf die richtigen Werte festgelegt sind.
7. Wenn Sie einen Fehler behoben haben und den Slice erneut verarbeiten wollen, klicken Sie auf dem Blatt **OutputDataset** mit der rechten Maustaste auf den Slice und klicken Sie anschließend auf **Ausführen**.
8. Wenn die folgende Fehlermeldung angezeigt wird, verwenden Sie eine Version des Azure Storage-Pakets, die höher als 4.3.0 ist. Das Startprogramm des Data Factory-Diensts erfordert Version 4.3 von WindowsAzure.Storage. Wenn Sie die neuere Version der Azure Storage-Assembly verwenden müssen, helfen Ihnen die Informationen zum Umgehen dieses Problems im Abschnitt [Anwendungsdomänenisolierung](#appdomain-isolation) weiter. 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    Falls Sie Version 4.3.0 des Azure Storage-Pakets verwenden können, können Sie den vorhandenen Verweis auf das Azure Storage-Paket der Version > 4.3.0 entfernen. Führen Sie anschließend in der NuGet-Paket-Manager-Konsole den folgenden Befehl aus. 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Erstellen Sie das Projekt. Löschen Sie die Azure.Storage-Assembly der Version > 4.3.0 aus dem Ordner „bin\Debug“. Erstellen Sie eine ZIP-Datei mit den Binärdateien und der PDB-Datei. Ersetzen Sie die alte ZIP-Datei im Blobcontainer („customactivitycontainer“) durch diese neue ZIP-Datei. Führen Sie die fehlgeschlagenen Slices erneut aus (klicken Sie mit der rechten Maustaste auf den Slice, und klicken Sie auf „Ausführen“).   
8. Die benutzerdefinierte Aktivität verwendet nicht die **app.config**-Datei aus dem Paket. Wenn Ihr Code also Verbindungszeichenfolgen aus der Konfigurationsdatei liest, funktioniert er während der Laufzeit nicht. Bei Verwendung von Azure Batch empfiehlt es sich, alle geheimen Schlüssel in **Azure Key Vault** (Schlüsseltresor) aufzubewahren, den **Schlüsseltresor** mithilfe eines zertifikatbasierten Dienstprinzipals zu schützen und das Zertifikat an den Azure Batch-Pool zu verteilen. Die benutzerdefinierte .NET-Aktivität kann anschließend auf die geheimen Schlüssel aus dem Schlüsseltresor während der Laufzeit zugreifen. Dabei handelt es sich um eine generische Lösung, die auf jede Art von Geheimnis skalieren kann, nicht nur auf eine Verbindungszeichenfolge.

   Es gibt eine einfachere Problemumgehung (die allerdings nicht als bewährte Methode betrachtet wird): Sie können einen neuen **mit Azure SQL verknüpften Dienst** mit Verbindungszeichenfolgen-Einstellungen sowie ein Dataset erstellen, das den verknüpften Dienst verwendet, und das Dataset als Dummy-Eingabedataset mit der benutzerdefinierten .NET-Aktivität verketten. Sie können anschließend auf die Verbindungszeichenfolge des verknüpften Diensts im Code der benutzerdefinierten Aktivität zugreifen. Sie sollte während der Laufzeit problemlos funktionieren.  

## <a name="update-custom-activity"></a>Aktualisieren der benutzerdefinierten Aktivität
Wenn Sie den Code für die benutzerdefinierte Aktivität aktualisieren, führen Sie einen Buildvorgang aus. Laden Sie die ZIP-Datei mit den neuen Binärdateien in den Blobspeicher hoch.

## <a name="appdomain-isolation"></a>Anwendungsdomänenisolierung
Das [Beispiel für eine AppDomain-übergreifende Aktivität](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) zeigt, wie Sie eine benutzerdefinierte Aktivität erstellen, die nicht auf die vom Data Factory-Startprogramm verwendeten Assemblyversionen (z.B. WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x usw.) beschränkt ist.

## <a name="access-extended-properties"></a>Zugreifen auf erweiterte Eigenschaften
Sie können erweiterte Eigenschaften in der Aktivität JSON wie im folgenden Beispiel deklarieren:

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


In diesem Beispiel gibt es zwei erweiterte Eigenschaften: **SliceStart** und **DataFactoryName**. Der Wert für „SliceStart“ basiert auf der SliceStart-Systemvariablen. Eine Liste der unterstützten Systemvariablen finden Sie unter [Systemvariablen](data-factory-scheduling-and-execution.md#data-factory-functions-and-system-variables) . Der Wert für „DataFactoryName“ ist hartcodiert und auf „CustomActivityFactory“ festgelegt.

Verwenden Sie Code wie den folgenden, um auf diese erweiterten Eigenschaften in der **Execute**-Methode zuzugreifen:

```csharp
// to get extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// to log all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a>Automatische Skalierung von Azure Batch
Sie können einen Azure Batch-Pool auch mit dem Feature **Automatisch skalieren** erstellen. Sie können z.B. einen Azure Batch-Pool ohne dedizierte VM erstellen und dabei eine Formel für die automatische Skalierung angeben, die von der Anzahl der ausstehenden Aufgaben abhängig ist:

Je ein virtueller Computer pro ausstehender Aufgabe (Beispiel: fünf ausstehende Aufgaben -> fünf virtuelle Computer):

```
pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
$TargetDedicated = max(pendingTaskSampleVector);
```

Maximal ein virtueller Computer – unabhängig von der Anzahl ausstehender Aufgaben:

```
pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
$TargetDedicated = (max(pendingTaskSampleVector)>0)?1:0;
```

Weitere Informationen hierzu finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](../batch/batch-automatic-scaling.md) .

Wenn der Pool die Standardeinstellung für [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx)verwendet, kann es 15 bis 30 Minuten dauern, bis der Batch-Dienst den virtuellen Computer vorbereitet hat und die benutzerdefinierte Aktivität ausgeführt wird.  Wenn der Pool eine andere Einstellung für „autoScaleEvaluationInterval“ nutzt, könnte der Batch-Dienst „autoScaleEvaluationInterval“ + 10 Minuten verwenden.

## <a name="use-hdinsight-compute-service"></a>Verwenden des HDInsight-Computediensts
In der exemplarischen Vorgehensweise haben Sie ein Azure Batch-Compute verwendet, um die benutzerdefinierte Aktivität auszuführen. Sie können auch Ihren eigenen HDInsight-Cluster verwenden oder von Data Factory einen bedarfsgesteuerten HDInsight-Cluster erstellen und die benutzerdefinierte Aktivität auf dem HDInsight-Cluster ausführen lassen. Im Anschluss finden Sie die Schritte zum Verwenden eines HDInsight-Clusters:  

1. Erstellen Sie einen mit Azure HDInsight verknüpften Dienst.   
2. Verwenden Sie den mit HDInsight verknüpften Dienst anstelle von **AzureBatchLinkedService** in der JSON-Pipeline.

Wenn Sie dies anhand der exemplarischen Vorgehensweise testen möchten, können Sie die **Start**- und **Endzeiten** für die Pipeline ändern, um das Szenario mit dem Azure HDInsight-Dienst zu testen.

#### <a name="create-azure-hdinsight-linked-service"></a>Erstellen des mit Azure-HDInsight verknüpften Diensts
Der Azure Data Factory-Dienst unterstützt das Erstellen eines Clusters bei Bedarf und verwendet ihn zum Verarbeiten der Eingabe zum Erzeugen von Ausgabedaten. Sie können auch Ihren eigenen Cluster für den gleichen Zweck verwenden. Wenn Sie den bedarfsgesteuerten HDInsight-Cluster verwenden, wird für jeden Slice ein Cluster erstellt. Bei Verwendung eines eigenen HDInsight-Clusters ist der Cluster sofort zur Sliceverarbeitung bereit. Aus diesem Grund werden bei Verwendung des bedarfsgesteuerten Clusters die Ausgabedaten möglicherweise nicht so schnell angezeigt wie bei der Verwendung Ihres eigenen Clusters.

> [!NOTE]
> Zur Laufzeit wird eine Instanz einer .NET-Aktivität nur auf einem Workerknoten im HDInsight-Cluster ausgeführt. Eine Skalierung auf mehrere Knoten ist nicht möglich. Mehrere Instanzen der .NET-Aktivität können auf verschiedenen Knoten des HDInsight-Clusters parallel ausgeführt werden.
>
>

##### <a name="to-use-an-on-demand-hdinsight-cluster"></a>So verwenden Sie einen bedarfsgesteuerten HDInsight-Cluster
1. Klicken Sie im **Azure Batch-Konto**im linken Menü auf **Verfassen und bereitstellen** .
2. Klicken Sie im Data Factory-Editor auf der Befehlsleiste auf **Neu berechnen**, und wählen Sie im Menü die Option **On-demand HDInsight cluster** (Bedarfsgesteuerter HDInsight-Cluster) aus.
3. Nehmen Sie die folgenden Änderungen am JSON-Skript vor:

   1. Geben Sie für die Eigenschaft **clusterSize** die Größe des HDInsight-Clusters an.
   2. Geben Sie für die **timeToLive** -Eigenschaft an, wie lange sich der Cluster im Leerlauf befinden darf, bevor er gelöscht wird.
   3. Geben Sie für die **version** -Eigenschaft die HDInsight-Version an, die Sie verwenden möchten. Wenn Sie diese Eigenschaft ausschließen, wird die neueste Version verwendet.  
   4. Geben Sie für **linkedServiceName** den Text **AzureStorageLinkedService** an.

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```
4. Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um den verknüpften Dienst bereitzustellen.

##### <a name="to-use-your-own-hdinsight-cluster"></a>So verwenden Sie Ihren eigenen HDInsight-Cluster:
1. Klicken Sie im **Azure Batch-Konto**im linken Menü auf **Verfassen und bereitstellen** .
2. Klicken Sie im **Data Factory-Editor** auf der Befehlsleiste auf **Neu berechnen**, und wählen Sie im Menü die Option **HDInsight-Cluster** aus.
3. Nehmen Sie die folgenden Änderungen am JSON-Skript vor:

   1. Geben Sie für die **clusterUri** -Eigenschaft die URL für Ihr HDInsight ein. Beispiel: https://<clustername>.azurehdinsight.net/     
   2. Geben Sie für die **UserName** -Eigenschaft den Benutzernamen ein, der Zugriff auf den HDInsight-Cluster hat.
   3. Geben Sie für die **Password** -Eigenschaft das Kennwort für das Benutzerkonto ein.
   4. Geben Sie für die **LinkedServiceName**-Eigenschaft **AzureStorageLinkedService** ein.
4. Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um den verknüpften Dienst bereitzustellen.

Weitere Informationen finden Sie unter [Verknüpfte Computedienste](data-factory-compute-linked-services.md) .

Verwenden Sie in der **Pipeline-JSON-Datei**den mit HDInsight verknüpften (bedarfsgesteuerten oder eigenen) Dienst:

```JSON
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00Z",
    "end": "2016-11-16T05:00:00Z",
    "isPaused": false
  }
}
```

## <a name="examples"></a>Beispiele
| Beispiel | Aktion der benutzerdefinierten Aktivität |
| --- | --- |
| [Downloadprogramm für HTTP-Daten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). |Lädt Daten von einem HTTP-Endpunkt mit einer benutzerdefinierten C#-Aktivität in Data Factory in den Azure-Blobspeicher herunter. |
| [Beispiel zur Twitter-Stimmungsanalyse](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Ruft ein Azure ML-Modell auf, um Stimmungsanalysen, Bewertungen, Vorhersagen usw. durchzuführen. |
| [R-Skript ausführen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). |Ruft ein R-Skript auf, indem „RScript.exe“ in Ihrem HDInsight-Cluster ausgeführt wird, in dem R bereits installiert ist. |
| [Anwendungsdomänenübergreifende .NET-Aktivität](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Verwendet andere Assemblyversionen als das Data Factory-Startprogramm |


[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[azure-developer-center]: http://azure.microsoft.com/develop/net/
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png

