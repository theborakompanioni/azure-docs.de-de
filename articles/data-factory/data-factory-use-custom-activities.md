<properties
    pageTitle="Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline"
    description="Erfahren Sie, wie Sie benutzerdefinierte Aktivitäten erstellen und in einer Azure Data Factory-Pipeline verwenden."
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
    ms.date="10/17/2016"
    ms.author="spelluru"/>


# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline
> [AZURE.SELECTOR]
[Hive](data-factory-hive-activity.md)  
[Pig](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop-Datenströme](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Data Lake Analytics U-SQL](data-factory-usql-activity.md)
[Benutzerdefinierte .NET-Aktivität](data-factory-use-custom-activities.md)

Es existieren zwei Aktivitätstypen, die Sie in einer Azure Data Factory-Pipeline verwenden können.
 
- [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) zum Verschieben von Daten zwischen [unterstützten Datenspeichern](data-factory-data-movement-activities.md#supported-data-stores).
- [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) zum Transformieren/Verarbeiten von Daten mithilfe von Computes wie Azure HDInsight, Azure Batch und Azure Machine Learning. Zum Beispiel: HDInsight Hive- und Machine Learning-Batchausführung.  

Wenn Sie Daten von/zu einem Datenspeicher verschieben müssen, der von Azure Data Factory nicht unterstützt wird, können Sie eine benutzerdefinierte .NET-Aktivität mit Ihrer eigenen Datenverschiebungslogik erstellen und die Aktivität in der Pipeline verwenden. 

Ebenso können Sie eine benutzerdefinierte Aktivität mit Ihrer eigenen Datenverarbeitungslogik erstellen und in der Pipeline verwenden, wenn Sie Daten auf eine Weise transformieren/verarbeiten müssen, die von Data Factory nicht unterstützt wird.

> [AZURE.NOTE] Das Datenverwaltungsgateway unterstützt derzeit nur die Kopieraktivität und die Aktivität mit gespeicherten Prozeduren in Data Factory. Das Gateway kann nicht über eine benutzerdefinierte Aktivität verwendet werden, um auf lokale Datenquellen zuzugreifen.
 
Sie können die benutzerdefinierte .NET-Aktivität so konfigurieren, dass sie entweder mithilfe eines **Azure Batch**-Diensts oder eines **Azure HDInsight**-Clusters ausgeführt wird.   

Die folgende exemplarische Vorgehensweise bietet Schritt-für-Schritt-Anleitungen zum Erstellen einer benutzerdefinierten .NET-Aktivität und zur Verwendung der benutzerdefinierten Aktivität in einer Pipeline. Für die exemplarische Vorgehensweise wird der mit **Azure Batch** verknüpfte Dienst verwendet. Um stattdessen den mit Azure HDInsight verknüpften Dienst zu verwenden, erstellen Sie einen verknüpften Dienst vom Typ **HDInsight** (wenn Sie Ihren eigenen HDInsight-Cluster verwenden) oder **HDInsightOnDemand** (wenn Sie möchten, dass Data Factory einen bedarfsgesteuerten HDInsight-Cluster erstellt), und verwenden Sie diesen im Aktivitätsbereich der Pipeline-JSON (**linkedServiceName**). Weitere Informationen zur Verwendung von Azure HDInsight zum Ausführen der benutzerdefinierten Aktivität finden Sie im Abschnitt [Verwenden von mit Azure HDInsight verknüpften Diensten](#use-azure-hdinsight-linked-services) .


## <a name="walkthrough"></a>Exemplarische Vorgehensweise 

### <a name="prerequisites"></a>Voraussetzungen

- Visual Studio 2012/2013/2015
- Herunterladen und Installieren von [Azure .NET SDK][azure-developer-center]


### <a name="azure-batch-prerequisites"></a>Azure Batch-Voraussetzungen
In der exemplarischen Vorgehensweise führen Sie Ihre benutzerdefinierten .NET-Aktivitäten mit Azure Batch als Computeressource aus. Unter [Grundlagen von Azure Batch][batch-technical-overview] finden Sie eine Übersicht über den Azure Batch-Dienst. Unter [Erste Schritte mit der Azure Batch-Bibliothek für .NET][batch-get-started] werden die ersten Schritte mit dem Azure Batch-Dienst gezeigt.

Für das Tutorial müssen Sie ein Azure Batch-Konto mit einem VM-Pool erstellen. Gehen Sie wie folgt vor:

1. Erstellen Sie über das **Azure-Portal** ein [Azure Batch-Konto](http://manage.windowsazure.com). Anweisungen finden Sie im Artikel [Erstellen und Verwalten eines Azure Batch-Kontos][batch-create-account]. Notieren Sie den Azure Batch-Kontonamen und -Kontoschlüssel.

    Sie können auch das [New-AzureBatchAccount][new-azure-batch-account]-Cmdlet verwenden, um ein Azure Batch-Konto zu erstellen. Ausführliche Anweisungen zur Verwendung dieses Cmdlets finden Sie unter [Verwenden von Azure PowerShell zum Verwalten des Azure Batch-Kontos][azure-batch-blog].
2. Erstellen Sie einen **Azure Batch-Pool**.
    1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Menü auf **Durchsuchen** und dann auf **Batch-Konten**. 
    2. Wählen Sie Ihr Azure Batch-Konto aus, um das Blatt **Batch-Konto** zu öffnen. 
    3. Klicken Sie auf die Kachel **Pools** .
    4. Klicken Sie auf dem Blatt **Pools** auf der Symbolleiste auf die Schaltfläche „Hinzufügen“, um einen Pool hinzuzufügen.
        1. Geben Sie eine ID für den Pool ein (**Pool-ID**). Notieren Sie sich die **ID des Pools**. Sie wird beim Erstellen der Data Factory-Projektmappe benötigt. 
        2. Geben Sie **Windows Server 2012 R2** für die Einstellung „Betriebssystem-Familie“ ein.
        3. Wählen Sie einen **Knotentarif**aus. 
        3. Geben Sie **2** als Wert für die Einstellung **Zuordnung für Ziel** ein.
        4. Geben Sie **2** als Wert für die Einstellung **Max. Tasks pro Knoten** ein.
    5. Klicken Sie auf **OK** , um den Pool zu erstellen. 
 
    Sie können auch das [New-AzureBatchPool](https://msdn.microsoft.com/library/mt628690.aspx) -Cmdlet verwenden, um einen Azure Batch-Pool zu erstellen.    

### <a name="high-level-steps"></a>Allgemeine Schritte 
1.  **Erstellen Sie eine benutzerdefinierte Aktivität** , um eine Data Factory-Pipeline zu verwenden. Die benutzerdefinierte Aktivität in diesem Beispiel enthält die Datentransformations-/Datenverarbeitungslogik. 
    1.  Erstellen Sie in Visual Studio ein .NET-Klassenbibliotheksprojekt, fügen Sie den Code zum Verarbeiten von Eingabedaten ein, und kompilieren Sie das Projekt. 
    2.  Zippen Sie alle Binärdateien und die (optionale) PDB-Datei im Ausgabeordner.  
    3.  Laden Sie die Zip-Datei in den Azure Blobspeicher hoch. Eine ausführliche Anleitung finden Sie im Abschnitt „Benutzerdefinierte Aktivität erstellen“. 
2. **Erstellen Sie eine Azure Data Factory, die die benutzerdefinierte Aktivität verwendet**:
    1. Erstellen Sie eine Azure Data Factory.
    2. Erstellen Sie verknüpfte Dienste.
        1. AzureStorageLinkedService: Stellt die Speicher-Anmeldeinformationen für den Zugriff auf Blobs bereit
        2. AzureBatchLinkedService: Gibt Azure Batch als Compute an.
    3. Erstellen Sie Datasets.
        1. InputDataset: Gibt die Speichercontainer und den Ordner für die Eingabe-Blobs an.
        1. OutputDataset: Gibt die Speichercontainer und den Ordner für die Ausgabe-Blobs an.
    2. Erstellen Sie eine Pipeline, die die benutzerdefinierte Aktivität verwendet.
    3. Führen Sie die Pipeline aus und testen Sie sie.
    4. Debuggen Sie die Pipeline.

## <a name="create-the-custom-activity"></a>Erstellen der benutzerdefinierten Aktivität
Um eine benutzerdefinierte .NET-Aktivität zu erstellen, müssen Sie ein **.NET-Klassenbibliotheksprojekt** mit einer Klasse erstellen, die die **IDotNetActivity**-Schnittstelle implementiert. Diese Schnittstelle verfügt lediglich über eine Methode: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) , und ihre Signatur ist:

    public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices, 
            IEnumerable<Dataset> datasets, 
            Activity activity, 
            IActivityLogger logger)
        

Die Methode akzeptiert vier Parameter:

- **LinkedServices**. Dies ist eine aufzählbare Liste von verknüpften Diensten, die Eingabe- und Ausgabedatenquellen (z. B.: Azure Blob Storage) mit der Data Factory verknüpfen. In diesem Beispiel gibt es nur einen verknüpften Dienst des Typs Azure Storage, der sowohl für die Eingabe als auch Ausgabe verwendet wird. 
- **Datasets**. Dies ist eine aufzählbare Liste von Datasets. Sie können diesen Parameter zum Abrufen der Speicherorte und Schemas verwenden, die von den Eingabe- und Ausgabedatasets definiert werden.
- **Aktivität**. Dieser Parameter stellt die aktuelle Compute-Entität dar – in diesem Fall ein Azure Batch.
- **Protokollierungstool**. Mit dem Protokollierungstool können Sie Debugkommentare schreiben, die dann als Benutzerprotokoll für die Pipeline erscheinen. 

Die Methode gibt ein Wörterbuch zurück, das künftig zum Verketten benutzerdefinierter Aktivitäten verwendet werden kann. Dieses Feature ist noch nicht implementiert. Von der Methode wird also ein leeres Wörterbuch zurückgegeben.  

### <a name="procedure"></a>Vorgehensweise 
1.  Erstellen Sie ein **.NET-Klassenbibliotheksprojekt** .
    <ol type="a">
        <li>Starten Sie <b>Visual Studio 2015</b>, <b>Visual Studio 2013</b> oder <b>Visual Studio 2012</b>.</li>
        <li>Klicken Sie auf <b>Datei</b>, zeigen Sie auf <b>Neu</b>, und klicken Sie auf <b>Projekt</b>.</li>
        <li>Erweitern Sie <b>Vorlagen</b>, und wählen Sie <b>Visual C#</b> aus. In dieser exemplarischen Vorgehensweise verwenden Sie C#, Sie können jedoch jede .NET-Sprache verwenden, um benutzerdefinierte Aktivität zu entwickeln.</li>
        <li>Wählen Sie in der Liste mit den Projekttypen auf der rechten Seite den Eintrag <b>Klassenbibliothek</b> aus.</li>
        <li>Geben Sie <b>MyDotNetActivity</b> als <b>Namen</b> ein.</li>
        <li>Wählen Sie <b>C:\ADFGetStarted</b> als <b>Speicherort</b>.</li>
        <li>Klicken Sie auf <b>OK</b> , um das Projekt zu erstellen.</li>
    </ol>
2.  Klicken Sie auf **Extras**, zeigen Sie auf **NuGet-Paket-Manager**, und klicken Sie auf **Paket-Manager-Konsole**.
3.  Führen Sie in der Paket-Manager-Konsole den folgenden Befehl zum Importieren von **Microsoft.Azure.Management.DataFactories**aus.

        Install-Package Microsoft.Azure.Management.DataFactories

4. Importieren Sie das **Azure Storage** NuGet-Paket in das Projekt.

        Install-Package Azure.Storage

5. Fügen Sie die folgenden **using** -Anweisungen der Quelldatei im Projekt hinzu.

        using System.IO;
        using System.Globalization;
        using System.Diagnostics;
        using System.Linq;

        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Runtime;

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

6. Ändern Sie den Namen des **Namespace** in **MyDotNetActivityNS**.

        namespace MyDotNetActivityNS

7. Ändern Sie den Namen der Klasse in **MyDotNetActivity**, und leiten Sie ihn von der **IDotNetActivity**-Schnittstelle ab, wie im folgenden Codeausschnitt zu sehen.

        public class MyDotNetActivity : IDotNetActivity

8. Implementieren bzw. fügen Sie die **Execute**-Methode der **IDotNetActivity**-Schnittstelle der **MyDotNetActivity**-Klasse hinzu, und kopieren Sie den folgenden Beispielcode in die Methode.

    Das folgende Beispiel zählt, wie oft der Suchbegriff („Microsoft“) in jedem Blob, das einem Datenslice zugeordnet ist, vorkommt. 

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
        
            // declare types for input and output data stores
            AzureStorageLinkedService inputLinkedService;

            // declare dataset types
            CustomDataset inputLocation;
            AzureBlobDataset outputLocation;

            Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
            inputLocation = inputDataset.Properties.TypeProperties as CustomDataset;

            foreach (LinkedService ls in linkedServices)
                logger.Write("linkedService.Name {0}", ls.Name);

            // using First method instead of Single since we are using the same 
            // Azure Storage linked service for input and output. 
            inputLinkedService = linkedServices.First(
                linkedService =>
                linkedService.Name ==
                inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
                as AzureStorageLinkedService;

            string connectionString = inputLinkedService.ConnectionString; 

            // To create an input storage client.
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
            // convert to blob location object.
            outputLocation = outputDataset.Properties.TypeProperties as AzureBlobDataset;

            folderPath = GetFolderPath(outputDataset);

            logger.Write("Writing blob to the folder: {0}", folderPath);

            // create a storage object for the output blob.
            CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
            // write the name of the file. 
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

            logger.Write("output blob URI: {0}", outputBlobUri.ToString());
            // create a blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

            // The dictionary can be used to chain custom activities together in the future.
            // This feature is not implemented yet, so just return an empty dictionary.  

            return new Dictionary<string, string>();
        }

9. Fügen Sie die folgenden Hilfsmethoden hinzu. Die **Execute** -Methode ruft diese Hilfsmethoden auf. Die **GetConnectionString**-Methode ruft die Azure Storage-Verbindungszeichenfolge ab, und die **GetFolderPath**-Methode ruft den Blobspeicherort ab. Vor allem isoliert die **Calculate** -Methode den Code, der jeden Blob durchläuft.

        /// <summary>
        /// Gets the folderPath value from the input/output dataset.
        /// </summary>

        private static string GetFolderPath(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

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

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

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

    Die „GetFolderPath“-Methode gibt den Pfad zum Ordner zurück, auf den das Dataset verweist, und die „GetFileName“-Methode gibt den Namen des Blobs oder der Datei zurück, auf das oder die das Dataset verweist. Wenn Sie Ordnerpfade mit Variablen wie „{Year}“, „{Month}“, „{Day}“ oder Ähnlichem definiert haben, gibt die Methode die Zeichenfolge zurück, ohne diese Variablen durch Laufzeitwerte zu ersetzen. Weitere Informationen zum Zugreifen auf „SliceStart“, „SliceEnd“ usw. finden Sie im Abschnitt [Zugreifen auf erweiterte Eigenschaften](#access-extended-properties).    
    
            "name": "InputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "file.txt",
                    "folderPath": "adftutorial/inputfolder/",
    
    Die „Calculate“-Methode berechnet die Anzahl der Instanzen des Schlüsselworts „Microsoft“ in den Eingabedateien (Blobs im Ordner). Der Suchbegriff („Microsoft“) ist hartcodiert.

10. Kompilieren Sie das Projekt. Klicken Sie im Menü auf **Erstellen** und dann auf **Projektmappe erstellen**.
11. Starten Sie **Windows-Explorer**, und navigieren Sie je nach Buildtyp zum Ordner **bin\debug** oder **bin\release**.
12. Erstellen Sie die ZIP-Datei **MyDotNetActivity.zip**, die alle Binärdateien im Ordner <project folder>\bin\Debug enthält. Es empfiehlt sich unter Umständen, die Datei **MyDotNetActivity.pdb** einzuschließen, damit Sie im Falle eines Fehlers zusätzliche Details wie etwa die Nummer der Quellcodezeile erhalten, die das Problem verursacht hat. Alle Dateien in der ZIP-Datei für die benutzerdefinierte Aktivität müssen sich auf der **obersten Ebene** befinden und dürfen keine Unterordner besitzen.

    ![Binäre Ausgabedateien](./media/data-factory-use-custom-activities/Binaries.png)
13. Laden Sie **MyDotNetActivity.zip** als Blob in den Blobcontainer **customactivitycontainer** im Azure-Blobspeicher hoch, den der verknüpfte Dienst **AzureStorageLinkedService** in **ADFTutorialDataFactory** verwendet.  Erstellen Sie den Blobcontainer **customactivitycontainer** , sofern er noch nicht vorhanden ist.

> [AZURE.NOTE] Wenn Sie dieses .NET-Aktivitätsprojekt einer Lösung in Visual Studio hinzufügen, die ein Data Factory-Projekt enthält, und dem .NET-Aktivitätsprojekt einen Verweis vom Data Factory-Anwendungsprojekt hinzufügen, müssen Sie die letzten beiden Schritte (ZIP-Datei selbst erstellen und in den Azure-Blobspeicher hochladen) nicht durchführen. Beim Veröffentlichen von Data Factory-Entitäten mit Visual Studio werden diese Schritte automatisch durch den Veröffentlichungsprozess ausgeführt. In den Artikeln [Erstellen der ersten Pipeline mit Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) und [Kopieren von Daten aus einem Azure-Blob in Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) erfahren Sie mehr über das Erstellen und Veröffentlichen von Data Factory-Entitäten mit Visual Studio.  

### <a name="execute-method"></a>Execute-Methode

Dieser Abschnitt enthält weitere Details und Hinweise zum Code in der **Execute** -Methode.
 
1. Die Elemente zum Durchlaufen der Eingabesammlung finden Sie unter dem Namespace [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) . Zum Durchlaufen der Blob-Sammlung müssen Sie die **BlobContinuationToken** -Klasse verwenden. Im Wesentlichen müssen Sie eine do-while-Schleife mit dem Token als Mechanismus zum Beenden der Schleife verwenden. Weitere Informationen finden Sie unter [Verwenden von Blob-Speicher aus .NET](../storage/storage-dotnet-how-to-use-blobs.md). Eine einfache Schleife ist hier dargestellt:

        // Initialize the continuation token.
        BlobContinuationToken continuationToken = null;
        do
        {   
            // Get the list of input blobs from the input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                      true,
                                      BlobListingDetails.Metadata,
                                      null,
                                      continuationToken,
                                      null,
                                      null);
            // Return a string derived from parsing each blob.
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
        } while (continuationToken != null);

    Einzelheiten finden Sie in der Dokumentation für die [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) -Methode.

2.  Der Code für die Verwendung durch den Satz von Blobs geht logischerweise in die do-while-Schleife ein. In der **Execute**-Methode übergibt die do-while-Schleife die Liste der Blobs an eine Methode namens **Calculate**. Die Methode gibt eine Zeichenfolgenvariable mit dem Namen **output** zurück, die das Ergebnis des Durchlaufens aller Blobs im Segment ist. 

    Sie gibt die Anzahl der Vorkommnisse des Suchbegriffs (**Microsoft**) im Blob zurück, die an die **Calculate**-Methode übergeben wurden. 

            output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);

3.  Sobald die **Calculate** -Methode mit der Arbeit fertig ist, muss sie in ein neues Blob geschrieben werden. Also kann für jeden Satz an Blobs, die verarbeitet werden, ein neues Blob mit den Ergebnissen geschrieben werden. Um ein neues Blob zu schreiben, müssen Sie zunächst das Ausgabedataset suchen. 

            // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
            Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

            // Convert to blob location object.
            outputLocation = outputDataset.Properties.TypeProperties as AzureBlobDataset;

4.  Der Code ruft auch eine Hilfsmethode auf: **GetFolderPath** zum Abrufen des Ordnerpfads (der Name des Speichercontainers).
 
            folderPath = GetFolderPath(outputDataset);

    **GetFolderPath** wandelt das DataSet-Objekt in ein AzureBlobDataSet-Objekt um, das über eine Eigenschaft namens „FolderPath“ verfügt.
            
            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            
            return blobDataset.FolderPath;

5.  Der Code ruft die **GetFileName** -Methode zum Abrufen des Dateinamens (Blob-Name) auf. Der Code ähnelt dem obigen Code zum Abrufen des Ordnerpfads. 

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

            return blobDataset.FileName;

6.  Der Name der Datei wird durch Erstellen eines URI-Objekts geschrieben. Der URI-Konstruktor verwendet die **BlobEndpoint** -Eigenschaft, um den Containernamen zurückzugeben. Ordnerpfad und Dateiname werden hinzugefügt, um den URI für den Ausgabe-Blob zu erstellen.  

            // Write the name of the file. 
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

7.  Der Name der Datei wurde geschrieben und Sie können jetzt die Ausgabezeichenfolge aus der Calculate-Methode in ein neues Blob schreiben:

            // Create a blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

## <a name="create-the-data-factory-using-azure-portal"></a>Erstellen der Data Factory mithilfe des Azure-Portals

Sie haben im Abschnitt **Erstellen der benutzerdefinierten Aktivität** eine benutzerdefinierte Aktivität erstellt und die Zip-Datei mit Binärdateien und der PDB-Datei in einen Azure-Blob-Container hochgeladen. In diesem Abschnitt erstellen Sie eine Instanz von Azure **Data Factory** mit einer **Pipeline**, die die **benutzerdefinierte Aktivität** verwendet.
 
Das Eingabedataset für die benutzerdefinierte Aktivität stellt die Blobs (Dateien) im Eingabeordner (adftutorial\inputfolder) im Blobspeicher dar. Das Ausgabedataset für die Aktivität stellt die Ausgabeblobs im Ausgabeordner (adftutorial\outputfolder) im Blobspeicher dar. 

Erstellen Sie eine Datei mit dem Namen **file.txt** und folgendem Inhalt, und laden Sie sie in **adftutorial\inputfolder** hoch („adftutorial“ ist der Name des Azure-Blobcontainers, „inputfolder“ der Name des Ordners in diesem Container).

    test custom activity Microsoft test custom activity Microsoft

Der Eingabeordner entspricht einem Slice in Azure Data Factory, auch wenn der Ordner mehrere Dateien enthält. Wenn jeder Slice von der Pipeline verarbeitet wird, durchläuft die benutzerdefinierte Aktivität alle Blobs im Eingabeordner für diesen Slice. 

Im Ordner „adftutorial\output“ befindet sich eine Ausgabedatei mit mindestens einer Zeile (identisch mit der Anzahl von Blobs im Eingabeordner):
 
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.


In diesem Abschnitt erstellen Sie Folgendes:

1. Eine **Data Factory**.
2. **Verknüpfte Dienste** für den Azure Batch-Pool virtueller Computer, auf denen die benutzerdefinierte Aktivität ausgeführt wird, und den Azure-Speicher, der die Eingabe- und Ausgabeblobs enthält. 
2. **Eingabe- und Ausgabedatasets** , die die Eingabe und Ausgabe der benutzerdefinierten Aktivität darstellen. 
3. **Pipeline** , die die benutzerdefinierte Aktivität verwendet.
4. **Data Factory**. Wird erstellt, wenn Sie diese Entitäten für Azure veröffentlichen. 

> [AZURE.NOTE] Erstellen Sie die Datei **file.txt** , und laden Sie sie in einen Blobcontainer hoch, sofern nicht bereits geschehen. Anweisungen finden Sie weiter oben.  

### <a name="step-1:-create-the-data-factory"></a>Schritt 1: Erstellen der Data Factory

1.  Gehen Sie nach der Anmeldung beim Azure-Portal wie folgt vor:
    1.  Klicken Sie im linken Menü auf **NEU** .
    2.  Klicken Sie auf dem Blatt **Neu** auf **Daten und Analysen**.
    3.  Klicken Sie auf dem Blatt **Datenanalyse** auf **Data Factory**.
2.  Geben Sie auf dem Blatt **Neue Data Factory** als Namen die Zeichenfolge **CustomActivityFactory** ein. Der Name der Azure Data Factory muss global eindeutig sein. Bei Anzeige der Fehlermeldung **Data Factory-Name „CustomActivityFactory“ ist nicht verfügbar** ändern Sie den Namen der Data Factory (z. B.in **IhrNameCustomActivityFactory**) und wiederholen den Vorgang.
3.  Klicken Sie auf **RESSOURCENGRUPPENNAME**, und wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe. 
4.  Stellen Sie sicher, dass Sie das richtige **Abonnement** und die richtige **Region** zum Erstellen der Data Factory verwenden. 
5.  Klicken Sie auf dem Blatt **Neue Data Factory** auf **Erstellen**.
6.  Im **Dashboard** des Azure-Portals sehen Sie, dass die Data Factory erstellt wird.
7.  Nach erfolgreicher Erstellung der Data Factory wird das Data Factory-Blatt mit dem Inhalt der Data Factory angezeigt.

### <a name="step-2:-create-linked-services"></a>Schritt 2: Erstellen von verknüpften Diensten

Verknüpfte Dienste verknüpfen Datenspeicher oder Serverdienste mit einer Azure Data Factory. In diesem Schritt verknüpfen Sie Ihr Azure-Speicherkonto und Ihr Azure Batch-Konto mit Ihrer Data Factory.

#### <a name="create-azure-storage-linked-service"></a>Erstellen des mit Azure Storage verknüpften Diensts

1.  Klicken Sie auf die Kachel **Erstellen und Bereitstellen** auf dem Blatt **DATA FACTORY** für **CustomActivityFactory**. Der Data Factory-Editor wird gestartet.
2.  Klicken Sie in der Befehlsleiste auf **Neuer Datenspeicher** und wählen Sie **Azure Storage**. Das JSON-Skript zum Erstellen eines mit einem Azure-Speicher verknüpften Diensts sollte im Editor angezeigt werden.
3.  Ersetzen Sie **Kontoname** durch den Namen Ihres Azure-Speicherkontos und **Kontoschlüssel** durch den Zugriffsschlüssel des Azure-Speicherkontos. Informationen zum Abrufen Ihres Speicherzugriffsschlüssels finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
4.  Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um den verknüpften Dienst bereitzustellen.

#### <a name="create-azure-batch-linked-service"></a>Verwenden des mit Azure Batch verknüpften Dienstes

2. Klicken Sie im Data Factory-Editor auf der Befehlsleiste auf **Neu berechnen**, und wählen Sie im Menü **Azure Batch** aus.
3. Führen Sie im JSON-Skript folgende Schritte aus:
    1. Geben Sie den Azure Batch-Kontonamen für die **accountName** -Eigenschaft an. Die **URL** auf dem Blatt **Azure-Batch-Konto** hat folgendes Format: http://**Kontoname**.Region.batch.azure.com.m. Für die **batchUri**-Eigenschaft in JSON müssen Sie **„Kontoname“** aus der URL entfernen und den **Kontonamen** für die JSON-Eigenschaft **accountName** verwenden.
    2. Geben Sie den Azure Batch-Kontoschlüssel für die **accessKey** -Eigenschaft an. 
    3. Geben Sie den Namen des Pools, den Sie als Teil der Voraussetzungen erstellt haben, für die **poolName** -Eigenschaft an. Sie können anstelle des Poolnamens auch die ID des Pools angeben.
    4. Geben Sie den Azure Batch-URI für die **batchUri** -Eigenschaft an. Lesen Sie weiter oben die Hinweise zur **accountName** -Eigenschaft. Beispiel: https://westus.batch.azure.com.  
    5. Geben Sie **AzureStorageLinkedService** for the **linkedServiceName** -Eigenschaft an.
        
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

        Für die **poolName** -Eigenschaft können Sie auch die ID des Pools anstelle des Namens des Pools angeben.

    > [AZURE.NOTE] Data Factory unterstützt im Gegensatz zu HDInsight für Azure Batch keine bedarfsorientierte Option. Sie können nur Ihren eigenen Azure Batch-Pool in einer Azure Data Factory verwenden.
    
### <a name="step-3:-create-datasets"></a>Schritt 3: Erstellen von Datasets
In diesem Schritt erstellen Sie Datasets zur Darstellung von Eingabe- und Ausgabedaten.

#### <a name="create-input-dataset"></a>Erstellen eines Eingabedatasets
1.  Klicken Sie im **Editor** für die Data Factory auf der Symbolleiste auf **Neues Dataset** und anschließend im Dropdownmenü auf **Azure-Blobspeicher**.
2.  Ersetzen Sie den JSON-Code im rechten Bereich durch den folgenden JSON-Codeausschnitt:

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

    Im weiteren Verlauf dieser exemplarischen Vorgehensweise wird eine Pipeline mit der Startzeit „2015-11-16T00:00:00Z“ und der Endzeit „2015-11-16T05:00:00Z“ erstellt. Sie ist so eingerichtet, dass Daten stündlich erstellt werden, wodurch sich fünf Eingabe-/Ausgabeslices (zwischen **00**:00:00 und **05**:00:00) ergeben. 

    Die **Häufigkeit** und das **Intervall** für das Eingabedataset ist auf **Hour** und **1** festgelegt, was bedeutet, dass der Eingabeslice stündlich verfügbar ist. In diesem Beispiel nehmen wir die gleiche Datei (file.txt) im Eingabeordner. 

    Dies sind die Startzeiten für jeden Slice, die durch die Systemvariable „SliceStart“ im obigen JSON-Codeausschnitt dargestellt werden. 

    
3.  Klicken Sie in der Symbolleiste auf **Bereitstellen**, um **InputDataset** bereitzustellen. Vergewissern Sie sich, dass die Meldung **TABELLE ERFOLGREICH ERSTELLT** auf der Titelleiste des Editors angezeigt wird.


#### <a name="create-an-output-dataset"></a>Erstellen eines Ausgabedatasets

1. Klicken Sie im **Data Factory-Editor** auf **Neues Dataset** und dann auf der Befehlsleiste auf **Azure-Blobspeicher**.
2. Ersetzen Sie das JSON-Skript im rechten Bereich durch das folgende JSON-Skript:

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

    Der Ausgabespeicherort ist **adftutorial/customactivityoutput/** , und der Name der Ausgabedatei lautet „yyyy-MM-dd-HH.txt“, wobei „yyyy-MM-dd-HH“ Jahr, Monat, Datum und Stunde der Sliceerstellung angibt. Ausführlichere Informationen finden Sie in der [Entwicklerreferenz][adf-developer-reference].

    Eine Ausgabedatei und ein Ausgabe-Blob wird für jeden Eingabeslice generiert. Im Folgenden sehen Sie, wie eine Ausgabedatei für jeden Slice benannt wird. Alle Ausgabedateien werden in einem Ausgabeordner generiert: **adftutorial\customactivityoutput**.

  	| Slice | Startzeit | Ausgabedatei |
  	| :---- | :--------- | :---------- | 
  	| 1 | 2015-11-16T00:00:00 | 2015-11-16-00.txt |
  	| 2 | 2015-11-16T01:00:00 | 2015-11-16-01.txt |
  	| 3 | 2015-11-16T02:00:00 | 2015-11-16-02.txt |
  	| 4 | 2015-11-16T03:00:00 | 2015-11-16-03.txt |
  	| 5 | 2015-11-16T04:00:00 | 2015-11-16-04.txt |

    Denken Sie daran, dass alle Dateien in einem Eingabeordner zu einem Slice mit den oben genannten Startzeiten gehören. Wenn dieser Slice verarbeitet wird, durchsucht die benutzerdefinierte Aktivität jede Datei und erzeugt eine Zeile in der Ausgabedatei mit der Anzahl der Vorkommnisse des Suchbegriffs („Microsoft“). Wenn im Eingabeordner drei Dateien vorhanden sind, enthält die Ausgabedatei drei Zeilen für jeden stündlichen Slice: 2015-11-16-00.txt, 2015-11-16:01:00:00.txt usw. 


2. Klicken Sie in der Befehlsleiste auf **Bereitstellen**, um das **OutputDataset** bereitzustellen.


### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a>Erstellen Sie eine Pipeline, die die benutzerdefinierte Aktivität verwendet, und führen Sie sie aus.

1. Klicken Sie im Data Factory-Editor auf der Befehlsleiste auf die Schaltfläche **Neue Pipeline** . Wenn der Befehl nicht angezeigt wird, klicken Sie auf **... (Auslassungspunkte)**, um ihn anzuzeigen.
2. Ersetzen Sie das JSON-Skript im rechten Bereich durch das folgende JSON-Skript. 

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
            "start": "2015-11-16T00:00:00Z",
            "end": "2015-11-16T05:00:00Z",
            "isPaused": false
          }
        }

    Beachten Sie Folgendes:

    - **Concurrency** ist auf **2** festgelegt, sodass zwei Slices parallel von zwei virtuellen Computern im Azure Batch-Pool verarbeitet werden.
    - Im Abschnitt "activities" gibt es eine Aktivität vom Typ **DotNetActivity**.
    - **AssemblyName** wird auf den Namen der DLL festgelegt: **MyDotnetActivity.dll**.
    - **EntryPoint** wird auf **MyDotNetActivityNS.MyDotNetActivity** festgelegt.
    - **PackageLinkedService** ist auf **AzureStorageLinkedService** festgelegt, das auf den Blobspeicher verweist, der die ZIP-Datei mit der benutzerdefinierten Aktivität enthält. Wenn Sie für die Eingabe- und Ausgabedateien und die ZIP-Datei mit der benutzerdefinierten Aktivität unterschiedliche Azure-Speicherkonten verwenden, wird ein weiterer verknüpfter Azure Storage-Dienst erstellt. Dieser Artikel setzt voraus, dass Sie das gleiche Azure Storage-Konto verwenden.
    - **PackageFile** wird auf **customactivitycontainer/MyCustomActivity.zip** festgelegt. Das entspricht dem Format: containerforthezip/nameofthezip.zip.
    - Die benutzerdefinierte Aktivität verwendet **InputDataset** als Eingabe und **OutputDataset** als Ausgabe.
    - Die Eigenschaft „linkedServiceName“ der benutzerdefinierten Aktivität zeigt auf **AzureBatchLinkedService**. Dadurch wird Azure Data Factory mitgeteilt, dass die benutzerdefinierte Aktivität auf virtuellen Azure Batch-Computern ausgeführt werden muss.
    - Die Eigenschaft **isPaused** ist standardmäßig auf **false** festgelegt. Die Pipeline wird in diesem Beispiel sofort ausgeführt, da die Slices in der Vergangenheit starten. Legen Sie diese Eigenschaft auf „true“ fest, um die Pipeline anzuhalten, und legen Sie sie zum Neustart wieder auf „false“ fest. 
    - Die Zeiten **start** und **end** liegen **fünf** Stunden auseinander, und Slices werden stündlich erstellt, sodass insgesamt fünf Slices von der Pipeline erstellt werden. 

4. Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um die Pipeline bereitzustellen.

### <a name="monitor-the-pipeline"></a>Überwachen der Pipeline
 
8. Klicken Sie im Azure-Portal auf dem Blatt „Data Factory“ auf **Diagramm**.
    
    ![Kachel "Diagramm"](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
 
9. Klicken Sie in der Diagrammansicht auf „OutputDataset“.
 
    ![Diagrammansicht](./media/data-factory-use-custom-activities/diagram.png)

10. Sie sollten sehen können, dass die fünf Ausgabeslices im Zustand „Bereit“ sind, wenn sie bereits erzeugt wurden.

    ![Ausgabeslices](./media/data-factory-use-custom-activities/OutputSlices.png)
    
12. Stellen Sie sicher, dass die Ausgabedateien im Blobspeicher im **adftutorial** -Container generiert werden.

    ![Ausgabe der benutzerdefinierten Aktivität][image-data-factory-ouput-from-custom-activity]

9. Wenn Sie die Ausgabedatei öffnen, sollte die Ausgabe ähnlich der folgenden angezeigt werden:

    Es wurden zwei Vorkommen für den Suchbegriff „Microsoft“ in der Datei inputfolder/2015-11-16-00/file.txt gefunden.

10. Verwenden Sie das [Azure-Portal][azure-preview-portal] oder die Azure PowerShell-Cmdlets zum Überwachen Ihrer Data Factory, Pipelines und Datasets. Sie können die Meldungen vom **ActivityLogger** im Code für die benutzerdefinierte Aktivität in den Protokollen (insbesondere "user-0.log") sehen, die Sie aus dem Portal oder mithilfe von Cmdlets herunterladen können.

    ![Herunterladen von Protokollen aus einer benutzerdefinierten Aktivität][image-data-factory-download-logs-from-custom-activity]


Unter [Überwachen und Verwalten von Pipelines](data-factory-monitor-manage-pipelines.md) finden Sie eine ausführliche Anleitung zum Überwachen von Datasets und Pipelines.      

### <a name="data-factory-and-batch-integration"></a>Data Factory und Batch-Integration
Der Data Factory-Dienst erstellt in Azure Batch einen Auftrag mit dem Namen **adf-poolname:job-xxx**. 

![Azure Data Factory – Batch-Aufträge](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Bei jeder Aktivitätsausführung eines Slices wird eine Aufgabe erstellt. Wenn zehn Slices zur Verarbeitung bereitstehen, werden zehn Aufgaben in diesem Auftrag erstellt. Sie können mehrere Slices parallel ausführen, wenn Sie über mehrere Compute-Knoten im Pool verfügen. Sie können auch mehrere Slices auf dem gleichen Compute-Knoten ausführen, wenn die maximale Anzahl der Aufgaben pro Compute-Knoten auf mehr als 1 festgelegt ist. 

![Azure Data Factory – Aufgaben des Batch-Auftrags](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

Das folgende Diagramm veranschaulicht die Beziehung zwischen Azure Data Factory- und Batch-Aufgaben. 

![Data Factory und Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)


## <a name="debug-the-pipeline"></a>Debuggen der Pipeline
Das Debuggen umfasst einige grundlegende Verfahren:

1.  Sollte die folgende Fehlermeldung angezeigt werden, vergewissern Sie sich, dass der Name der Klasse in der CS-Datei mit dem Namen übereinstimmt, den Sie für die **EntryPoint**-Eigenschaft in der Pipeline-JSON angegeben haben. In der obigen exemplarischen Vorgehensweise lautet der Name der Klasse „MyDotNetActivity“. „EntryPoint“ in JSON lautet: MyDotNetActivityNS.**MyDotNetActivity**. 

            MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly

    Wenn die Namen übereinstimmen, vergewissern Sie sich, dass sich alle Binärdateien im **Stammordner** der ZIP-Datei befinden. Sprich: Wenn Sie die ZIP-Datei öffnen, müssen alle Dateien im Stammordner (nicht in Unterordnern) angezeigt werden.   
2.  Wenn der Eingabeslice nicht auf **Bereit**festgelegt ist, stellen Sie sicher, dass die Struktur des Eingabeordners korrekt ist und die Datei **file.txt** im Eingabeordner vorhanden ist. 
2.  Verwenden Sie in der **Execute-Method**e der benutzerdefinierten Aktivität das **IActivityLogger-Objekt**, um Informationen zu protokollieren, die beim Behandeln von Problemen hilfreich sind. Die protokollierten Meldungen werden in den Benutzerprotokolldateien („user-0.log“, „user-1.log“, „user-2.log“ usw.) angezeigt. 

    Klicken Sie auf dem Blatt **OutputDataset** auf den Slice, um das Blatt **DATENSLICE** für diesen Slice anzuzeigen. Die **Aktivitätsausführungen** für diesen Slice werden angezeigt. Sie sollten genau eine Aktivitätsausführung für den Slice sehen. Wenn Sie in der Befehlsleiste auf „Ausführen“ klicken, können Sie für den gleichen Slice eine weitere Aktivität ausführen. 

    Wenn Sie auf die Aktivitätsausführung klicken, wird das Blatt **AKTIVITÄTSAUSFÜHRUNG – DETAILS** mit einer Protokolldateienliste angezeigt. Protokollierte Nachrichten werden in der Datei „user_0.log“ angezeigt. Tritt ein Fehler auf, werden drei Aktivitätsausführungen angezeigt, da die Anzahl von Wiederholungsversuchen in der Pipeline oder Aktivitäts-JSON auf „3“ festgelegt ist. Wenn Sie die Aktivitätsausführung starten, werden die Protokolldateien angezeigt, die Sie zum Behandeln von Fehlern überprüfen können. 

    Klicken Sie in der Liste der Protokolldateien auf die Datei **user-0.log**. Im rechten Bereich werden die Ergebnisse der Verwendung der **IActivityLogger.Write** -Methode angezeigt. Wenn Sie nicht alle Meldungen sehen, überprüfen Sie, ob Sie mehrere Protokolldateien mit den folgenden Namen haben: user_1.log, user_2.log etc. Andernfalls war der Code nach der letzten protokollierten Meldung möglicherweise fehlerhaft.

    Sie sollten außerdem die Datei **system-0.log** auf alle Systemfehlermeldungen und -ausnahmen überprüfen.

3.  Schließen Sie die **PDB-Datei** in die ZIP-Datei ein, sodass in den Fehlerdetails Informationen wie etwa die **Aufrufliste** zur Verfügung stehen, wenn ein Fehler auftritt.
4.  Alle Dateien in der ZIP-Datei für die benutzerdefinierte Aktivität müssen sich auf der **obersten Ebene** befinden und dürfen keine Unterordner besitzen.
5.  Stellen Sie sicher, dass **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) und **packageLinkedService** (sollte auf den Azure Blob-Speicher verweisen, der die ZIP-Datei enthält) auf die richtigen Werte festgelegt sind. 
6.  Wenn Sie einen Fehler behoben haben und den Slice erneut verarbeiten wollen, klicken Sie auf dem Blatt **OutputDataset** mit der rechten Maustaste auf den Slice und klicken Sie anschließend auf **Ausführen**. 
7.  Die benutzerdefinierte Aktivität verwendet nicht die Datei **app.config** aus Ihrem Paket. Wenn Ihr Code also Verbindungszeichenfolgen aus der Konfigurationsdatei liest, funktioniert er während der Laufzeit nicht. Bei Verwendung von Azure Batch empfiehlt es sich, alle geheimen Schlüssel in **Azure Key Vault** (Schlüsseltresor) aufzubewahren, den **Schlüsseltresor** mithilfe eines zertifikatbasierten Dienstprinzipals zu schützen und das Zertifikat an den Azure Batch-Pool zu verteilen. Die benutzerdefinierte .NET-Aktivität kann anschließend auf die geheimen Schlüssel aus dem Schlüsseltresor während der Laufzeit zugreifen. Dabei handelt es sich um eine generische Lösung, die auf jede Art von geheimem Schlüssel skalieren kann, nicht nur auf eine Verbindungszeichenfolge.

    Es gibt eine einfachere Problemumgehung (die allerdings nicht als bewährte Methode betrachtet wird): Sie können einen neuen **mit Azure SQL verknüpften Dienst** mit Verbindungszeichenfolgen-Einstellungen sowie ein Dataset erstellen, das den verknüpften Dienst verwendet, und das Dataset als Dummy-Eingabedataset mit der benutzerdefinierten .NET-Aktivität verketten. Sie können anschließend auf die Verbindungszeichenfolge des verknüpften Diensts im Code der benutzerdefinierten Aktivität zugreifen. Sie sollte während der Laufzeit problemlos funktionieren.  



## <a name="update-the-custom-activity"></a>Aktualisieren der benutzerdefinierten Aktivität
Wenn Sie den Code für die benutzerdefinierte Aktivität aktualisieren, führen Sie einen Buildvorgang aus. Laden Sie die ZIP-Datei mit den neuen Binärdateien in den Blobspeicher hoch.

## <a name="copy/move-data"></a>Daten kopieren/verschieben 
Die Kopieraktivität kopiert die Daten aus einem **Quelldatenspeicher** in einen **Senkendatenspeicher**. Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores) . 

Wenn Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der **Kopieraktivität** nicht unterstützt wird, können Sie die **benutzerdefinierte Aktivität** in Data Factory mit Ihrer eigenen Logik zum Kopieren/Verschieben der Daten verwenden. Weitere Informationen finden Sie auch im [Beispiel für ein Downloadprogramm für HTTP-Daten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) auf GitHub.  

## <a name="appdomain-isolation"></a>Anwendungsdomänenisolierung 
Das [Beispiel für eine AppDomain-übergreifende Aktivität](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) zeigt, wie Sie eine benutzerdefinierte .NET-Aktivität für Azure Data Factory erstellen, die nicht auf die vom Azure Data Factory-Startprogramm verwendeten Assemblyversionen (WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x usw.) beschränkt ist.

## <a name="access-extended-properties"></a>Zugreifen auf erweiterte Eigenschaften
Sie können erweiterte Eigenschaften in der Aktivität JSON wie folgt deklarieren: 

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

Der obige Code enthält zwei erweiterte Eigenschaften: **SliceStart** und **DataFactoryName**. Der Wert für „SliceStart“ basiert auf der SliceStart-Systemvariablen. Eine Liste der unterstützten Systemvariablen finden Sie unter [Systemvariablen](data-factory-scheduling-and-execution.md#data-factory-system-variables) . Der Wert für „DataFactoryName“ ist hartcodiert und auf „CustomActivityFactory“ festgelegt. 

Verwenden Sie Code wie den folgenden, um auf diese erweiterten Eigenschaften in der **Execute** -Methode zuzugreifen: 

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

## <a name="auto-scaling-feature-of-azure-batch"></a>Feature für die automatische Skalierung von Azure Batch
Sie können einen Azure Batch-Pool auch mit dem Feature **Automatisch skalieren** erstellen. Sie können z.B. einen Azure Batch-Pool ohne dedizierte VM erstellen und dabei eine Formel für die automatische Skalierung angeben, die von der Anzahl der ausstehenden Aufgaben abhängig ist:

Je ein virtueller Computer pro ausstehender Aufgabe (Beispiel: fünf ausstehende Aufgaben -> fünf virtuelle Computer):

    pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
    $TargetDedicated = max(pendingTaskSampleVector);

Maximal ein virtueller Computer – unabhängig von der Anzahl ausstehender Aufgaben:

    pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
    $TargetDedicated = (max(pendingTaskSampleVector)>0)?1:0;

Weitere Informationen hierzu finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](../batch/batch-automatic-scaling.md) . 

Wenn der Pool die Standardeinstellung für [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx)verwendet, kann es 15 bis 30 Minuten dauern, bis der Batch-Dienst den virtuellen Computer vorbereitet hat und die benutzerdefinierte Aktivität ausgeführt wird.  Wenn der Pool eine andere Einstellung für „autoScaleEvaluationInterval“ nutzt, könnte der Batch-Dienst „autoScaleEvaluationInterval“ + 10 Minuten verwenden.

## <a name="use-azure-hdinsight-linked-services"></a>Verwenden von mit Azure HDInsight verknüpften Diensten
In der exemplarischen Vorgehensweise haben Sie ein Azure Batch-Compute verwendet, um die benutzerdefinierte Aktivität auszuführen. Sie können auch Ihren eigenen HDInsight-Cluster verwenden oder von Data Factory einen bedarfsgesteuerten HDInsight-Cluster erstellen und die benutzerdefinierte Aktivität auf dem HDInsight-Cluster ausführen lassen. Im Anschluss finden Sie die Schritte zum Verwenden eines HDInsight-Clusters:  

1. Erstellen Sie einen mit Azure HDInsight verknüpften Dienst.   
2. Verwenden Sie den mit HDInsight verknüpften Dienst anstelle von **AzureBatchLinkedService** in der JSON-Pipeline. 

Sie können die **Start**- und **Endzeiten** für die Pipeline bei Bedarf ändern, um das Szenario mit dem Azure HDInsight-Dienst zu testen.

#### <a name="create-azure-hdinsight-linked-service"></a>Erstellen des mit Azure-HDInsight verknüpften Diensts 
Der Azure Data Factory-Dienst unterstützt das Erstellen eines Clusters bei Bedarf und verwendet ihn zum Verarbeiten der Eingabe zum Erzeugen von Ausgabedaten. Sie können auch Ihren eigenen Cluster für den gleichen Zweck verwenden. Wenn Sie den bedarfsgesteuerten HDInsight-Cluster verwenden, wird für jeden Slice ein Cluster erstellt. Bei Verwendung eines eigenen HDInsight-Clusters ist der Cluster sofort zur Sliceverarbeitung bereit. Aus diesem Grund werden bei Verwendung des bedarfsgesteuerten Clusters die Ausgabedaten möglicherweise nicht so schnell angezeigt wie bei der Verwendung Ihres eigenen Clusters.

> [AZURE.NOTE] Zur Laufzeit wird eine Instanz einer .NET-Aktivität nur auf einem Workerknoten im HDInsight-Cluster ausgeführt. Eine Skalierung auf mehrere Knoten ist nicht möglich. Mehrere Instanzen der .NET-Aktivität können auf verschiedenen Knoten des HDInsight-Clusters parallel ausgeführt werden.

##### <a name="to-use-an-on-demand-hdinsight-cluster"></a>So verwenden Sie einen bedarfsgesteuerten HDInsight-Cluster

1. Klicken Sie im **Azure Batch-Konto**im linken Menü auf **Verfassen und bereitstellen** .
2. Klicken Sie im Data Factory-Editor auf der Befehlsleiste auf **Neu berechnen**, und wählen Sie im Menü die Option **On-demand HDInsight cluster** (Bedarfsgesteuerter HDInsight-Cluster) aus.
2. Führen Sie im JSON-Skript folgende Schritte aus:
    1. Geben Sie für die Eigenschaft **clusterSize** die Größe des HDInsight-Clusters an.
    3. Geben Sie für die **timeToLive** -Eigenschaft an, wie lange sich der Cluster im Leerlauf befinden darf, bevor er gelöscht wird.
    4. Geben Sie für die **version** -Eigenschaft die HDInsight-Version an, die Sie verwenden möchten. Wenn Sie diese Eigenschaft ausschließen, wird die neueste Version verwendet.  
    5. Geben Sie für **linkedServiceName** den **AzureStorageLinkedService** an, den Sie im Tutorial „Erste Schritte“ erstellt haben.

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

2. Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um den verknüpften Dienst bereitzustellen.

##### <a name="to-use-your-own-hdinsight-cluster:"></a>So verwenden Sie Ihren eigenen HDInsight-Cluster:

1. Klicken Sie im **Azure Batch-Konto**im linken Menü auf **Verfassen und bereitstellen** .
2. Klicken Sie im **Data Factory-Editor** auf der Befehlsleiste auf **Neu berechnen**, und wählen Sie im Menü die Option **HDInsight-Cluster** aus.
2. Führen Sie im JSON-Skript folgende Schritte aus:
    1. Geben Sie für die **clusterUri** -Eigenschaft die URL für Ihr HDInsight ein. Beispiel: https://<clustername>.azurehdinsight.net/     
    2. Geben Sie für die **UserName** -Eigenschaft den Benutzernamen ein, der Zugriff auf den HDInsight-Cluster hat.
    3. Geben Sie für die **Password** -Eigenschaft das Kennwort für das Benutzerkonto ein.
    4. Geben Sie für die **LinkedServiceName**-Eigenschaft **AzureStorageLinkedService** ein. Diesen verknüpften Dienst haben Sie im Tutorial mit den ersten Schritten erstellt.

2. Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um den verknüpften Dienst bereitzustellen.

Weitere Informationen finden Sie unter [Verknüpfte Computedienste](data-factory-compute-linked-services.md) . 

Verwenden Sie in der **Pipeline-JSON-Datei**den mit HDInsight verknüpften (bedarfsgesteuerten oder eigenen) Dienst: 

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
        "start": "2015-11-16T00:00:00Z",
        "end": "2015-11-16T05:00:00Z",
        "isPaused": false
      }
    }

## <a name="examples"></a>Beispiele

Beispiel | Aktion der benutzerdefinierten Aktivität 
------ | ----------- 
[Downloadprogramm für HTTP-Daten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). | Lädt Daten von einem HTTP-Endpunkt mit einer benutzerdefinierten C#-Aktivität in Data Factory in den Azure-Blobspeicher herunter.
[Beispiel zur Twitter-Stimmungsanalyse](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) | Ruft ein Azure ML-Modell auf, um Stimmungsanalysen, Bewertungen, Vorhersagen usw. durchzuführen.
[R-Skript ausführen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). | Ruft ein R-Skript auf, indem „RScript.exe“ in Ihrem HDInsight-Cluster ausgeführt wird, in dem R bereits installiert ist. 
[Anwendungsdomänenübergreifende .NET-Aktivität](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | Verwendet andere Assemblyversionen als das Data Factory-Startprogramm  
 

## <a name="see-also"></a>Siehe auch

[Azure Data Factory-Aktualisierungen: Ausführen von benutzerdefinierten ADF-.NET-Aktivitäten mit Azure Batch](https://azure.microsoft.com/blog/2015/05/01/azure-data-factory-updates-execute-adf-custom-net-activities-using-azure-batch/).

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



<!--HONumber=Oct16_HO2-->


