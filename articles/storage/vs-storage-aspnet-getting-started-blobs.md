---
title: Erste Schritte mit Azure-Blobspeicher und verbundenen Visual Studio-Diensten (ASP.NET) | Microsoft-Dokumentation
description: "Erfahren Sie etwas über die ersten Schritte mit Azure-Blobspeicher in einem ASP.NET-Projekt in Visual Studio, nachdem Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit einem Speicherkonto hergestellt haben."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: c9b16bd5cd3dc2429a30fd30f9ec942c568abe42
ms.openlocfilehash: 802249883c6adf3fedc9873e35fb64a9085d905b


---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Erste Schritte mit Azure-Blobspeicher und verbundenen Visual Studio-Diensten (ASP.NET)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Der Azure-Blobspeicher ist ein Dienst, bei dem unstrukturierte Daten in der Cloud als Objekte/Blobs gespeichert werden. In Blob Storage können alle Arten von Text- oder Binärdaten gespeichert werden, z. B. ein Dokument, eine Mediendatei oder ein Installer einer Anwendung. Der Blobspeicher wird auch als Objektspeicher bezeichnet.

In diesem Tutorial wird gezeigt, wie Sie ASP.NET-Code für einige häufig verwendete Szenarien mit Azure-Blobspeicher schreiben. Zu den Szenarien zählen das Erstellen eines Blobcontainers sowie das Hochladen, Auflisten, Herunterladen und Löschen von Blobs.

##<a name="prerequisites"></a>Voraussetzungen

* [Microsoft Visual Studio](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)
* [Azure-Speicherkonto](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Erstellen eines MVC-Controllers 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Controller**, und wählen Sie im Kontextmenü **Hinzufügen > Controller** aus.

    ![Hinzufügen eines Controllers zu einer ASP.NET-MVC-App](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **MVC 5 Controller - Leer** und dann **Hinzufügen**.

    ![Angeben des MVC-Controllertyps](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. Benennen Sie den Controller im Dialogfeld **Controller hinzufügen** mit *BlobsController*, und wählen Sie **Hinzufügen**.

    ![Benennen des MVC-Controllers](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Fügen Sie die folgenden *using*-Anweisungen der `BlobsController.cs`-Datei hinzu.

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>Erstellen eines Blobcontainers

Ein Blobcontainer ist eine geschachtelte Hierarchie von Blobs und Ordnern. Die folgenden Schritte veranschaulichen, wie Sie einen Blobcontainer erstellen.

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens **CreateBlobContainer** hinzu, die ein **ActionResult** zurückgibt.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Rufen Sie in der **CreateBlobContainer**-Methode ein **CloudStorageAccount**-Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *&lt;storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Rufen Sie ein **CloudBlobClient**-Objekt ab, das einen Blob-Dienst-Client darstellt.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Rufen Sie ein **CloudBlobContainer**-Objekt ab, das einen Verweis auf den gewünschten Blobcontainernamen darstellt. Die **CloudBlobClient.GetContainerReference**-Methode stellt keine Anforderung an den Blobspeicher. Der Verweis wird zurückgegeben, und zwar unabhängig davon, ob der Blobcontainer vorhanden ist oder nicht. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Rufen Sie die **CloudBlobContainer.CreateIfNotExists**-Methode auf, um den Container zu erstellen, wenn er noch nicht vorhanden ist. Die **CloudBlobContainer.CreateIfNotExists**-Methode gibt **true** zurück, wenn der Container nicht vorhanden war und erfolgreich erstellt wurde; andernfalls wird **false** zurückgegeben.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Aktualisieren Sie **ViewBag** mit dem Namen des Blobcontainers.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten**, klicken Sie mit der rechten Maustaste auf **Blobs**, und wählen Sie im Kontextmenü **Hinzufügen > Ansicht** aus.

1. Geben Sie im Dialogfeld **Ansicht hinzufügen** für den Ansichtsnamen **CreateBlobContainer** ein, und wählen Sie **Hinzufügen**.

1. Öffnen Sie `CreateBlobContainer.cshtml`, und ändern Sie die Datei folgendermaßen:

    ```csharp
    @{
        ViewBag.Title = "Create blob container";
    }
    
    <h2>Create blob container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten **Html.ActionLink** den folgenden **Html.ActionLink** hinzu.

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blob-Container erstellen**. Die Ergebnisse ähneln denen im folgenden Screenshot. 
  
    ![Erstellen eines Blobcontainers](./media/vs-storage-aspnet-getting-started-blobs/results.png)

    Wie bereits erwähnt, gibt die **CloudBlobContainer.CreateIfNotExists**-Methode nur dann **true** zurück, wenn der Container nicht vorhanden ist und erstellt wird. Aus diesem Grund gibt die Methode **false** zurück, wenn Sie die App ausführen, wenn der Container vorhanden ist. Um die App mehrere Male auszuführen, müssen Sie den Container vor dem erneuten Ausführen der App löschen. Sie können den Container mit der **CloudBlobContainer.Delete**-Methode löschen. Sie können den Container auch über das [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oder den [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) löschen.  

## <a name="upload-a-blob-into-a-blob-container"></a>Hochladen eines Blobs in einen Blobcontainer

Sobald Sie [einen Blobcontainer erstellt](#create-a-blob-container) haben, können Sie Dateien in diesen Container hochladen. Dieser Abschnitt führt Sie durch die Schritte zum Hochladen einer lokalen Datei in einen Blobcontainer. Die Schritte setzen voraus, dass Sie einen Blobcontainer namens *test-blob-container* erstellt haben. Sie müssen diesen Namen in den Ihres Blobcontainers ändern. 

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens **UploadBlob** hinzu, die ein **EmptyResult** zurückgibt.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Rufen Sie in der **UploadBlob**-Methode ein **CloudStorageAccount**-Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *&lt;storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Rufen Sie ein **CloudBlobClient**-Objekt ab, das einen Blob-Dienst-Client darstellt.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Rufen Sie ein **CloudBlobContainer**-Objekt ab, das einen Verweis auf den Blobcontainernamen darstellt. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Wie bereits erwähnt, unterstützt Azure-Speicher verschiedene Blobtypen. Um einen Verweis auf ein Seitenblob abzurufen, rufen Sie die **CloudBlobContainer.GetPageBlobReference**-Methode auf. Um einen Verweis auf ein Blockblob abzurufen, rufen Sie die **CloudBlobContainer.GetBlockBlobReference**-Methode auf. Gewöhnlich wird die Verwendung von Blockblobs empfohlen. (Ändern Sie *<blob-name>* in den Namen, den Sie dem Blob nach dem Hochladen geben möchten.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Sobald Sie über einen Blobverweis verfügen, können Sie jeden Datenstrom in diesen hochladen, indem Sie die **UploadFromStream**-Methode des Blobverweisobjekts aufrufen. Die **UploadFromStream**-Methode erstellt das Blob, falls es nicht vorhanden ist, oder überschreibt es, falls es vorhanden ist. (Ändern Sie *&lt;file-to-upload>* in einen vollqualifizierten Pfad zu der Datei, die Sie hochladen möchten.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten**, klicken Sie mit der rechten Maustaste auf **Blobs**, und wählen Sie im Kontextmenü **Hinzufügen > Ansicht** aus.

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten **Html.ActionLink** den folgenden **Html.ActionLink** hinzu.

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blob hochladen**.  
  
Im nächsten Abschnitt wird das Auflisten von Blobs in einem Blobcontainer veranschaulicht.     

## <a name="list-the-blobs-in-a-blob-container"></a>Auflisten von Blobs in einem Blobcontainer

In diesem Abschnitt wird das Auflisten von Blobs in einem Blobcontainer veranschaulicht. Der Beispielcode verweist auf den *test-blob-container*, der im Abschnitt [Erstellen eines Blobcontainers](#create-a-blob-container) erstellt wurde.

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens **ListBlobs** hinzu, die ein **ActionResult** zurückgibt.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Rufen Sie in der **ListBlobs**-Methode ein **CloudStorageAccount**-Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *&lt;storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Rufen Sie ein **CloudBlobClient**-Objekt ab, das einen Blob-Dienst-Client darstellt.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Rufen Sie ein **CloudBlobContainer**-Objekt ab, das einen Verweis auf den Blobcontainernamen darstellt. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Verwenden Sie zum Auflisten des Blobs in einem Blobcontainer die Methode **CloudBlobContainer.ListBlobs**. Die **CloudBlobContainer.ListBlobs**-Methode gibt ein **IListBlobItem**-Objekt zurück, das Sie in ein **CloudBlockBlob**-, **CloudPageBlob**- oder **CloudBlobDirectory**-Objekt umwandeln. Der folgende Codeausschnitt listet alle Blobs in einem Blobcontainer auf, wobei das zurückgegebene Objekt auf Grundlage seines Typs in das entsprechende Objekt umgewandelt wird, und fügt den Namen (oder URI im Falle eines **CloudBlobDirectory**) einer Liste hinzu, die angezeigt werden kann.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Zusätzlich zu den Blobs können Blobcontainer Verzeichnisse enthalten. Angenommen, Sie haben einen Blobcontainer namens *test-blob-container* mit der folgenden Hierarchie:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Im vorangehenden Codebeispiel enthält die Zeichenfolgenliste **blobs** Werte, die den folgenden ähneln:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Wie Sie sehen, enthält die Liste nur die Entitäten der obersten Ebene, nicht die geschachtelten (*bar.png* und *baz.png*). Um alle Entitäten in einem Blobcontainer aufzulisten, müssen Sie die **CloudBlobContainer.ListBlobs**-Methode aufrufen und **true** für den **useFlatBlobListing**-Parameter übergeben.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    Wenn Sie den **useFlatBlobListing**-Parameter auf **true** festlegen, wird eine flache Liste aller Entitäten im Blobcontainer zurückgegeben, und dies führt zu folgenden Ergebnissen:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten**, klicken Sie mit der rechten Maustaste auf **Blobs**, und wählen Sie im Kontextmenü **Hinzufügen > Ansicht** aus.

1. Geben Sie im Dialogfeld **Ansicht hinzufügen** für den Ansichtsnamen **ListBlobs** ein, und wählen Sie **Hinzufügen**.

1. Öffnen Sie `ListBlobs.cshtml`, und ändern Sie die Datei folgendermaßen:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten **Html.ActionLink** den folgenden **Html.ActionLink** hinzu.

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blobs auflisten**. Die Ergebnisse ähneln denen im folgenden Screenshot. 
  
    ![Blobauflistung](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Herunterladen von Blobs

Die folgenden Schritte veranschaulichen, wie Sie ein Blob herunterladen und entweder im lokalen Speicher beibehalten oder den Inhalt in eine Zeichenfolge lesen. Der Beispielcode verweist auf den *test-blob-container*, der im Abschnitt [Erstellen eines Blobcontainers](#create-a-blob-container) erstellt wurde.

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens **DownloadBlob** hinzu, die ein **ActionResult** zurückgibt.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Rufen Sie in der **DownloadBlob**-Methode ein **CloudStorageAccount**-Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *&lt;storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Rufen Sie ein **CloudBlobClient**-Objekt ab, das einen Blob-Dienst-Client darstellt.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Rufen Sie ein **CloudBlobContainer**-Objekt ab, das einen Verweis auf den Blobcontainernamen darstellt. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Rufen Sie ein Blobverweisobjekt durch Aufrufen der **CloudBlobContainer.GetBlockBlobReference**- oder **CloudBlobContainer.GetPageBlobReference**-Methode ab. (Ändern Sie *&lt;blob-name>* in den Namen des Blobs, das Sie herunterladen.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Verwenden Sie zum Herunterladen von Blobs abhängig vom Blobtyp die **CloudBlockBlob.DownloadToStream**- oder **CloudPageBlob.DownloadToStream**-Methode. Im folgenden Codeausschnitt wird die **CloudBlockBlob.DownloadToStream**-Methode verwendet, um den Blobinhalt in ein Datenstromobjekt zu übertragen, das danach in einer lokalen Datei gespeichert wird. (Ändern Sie *&lt;local-file-name>* in den vollqualifizierten Dateinamen, der angibt, wohin der Blob heruntergeladen werden soll.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten **Html.ActionLink** den folgenden **Html.ActionLink** hinzu.

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blob herunterladen** zum Herunterladen des Blobs. Das im Aufruf der **CloudBlobContainer.GetBlockBlobReference**-Methode angegebene Blob wird in den Speicherort heruntergeladen, dem Sie im Aufruf der **File.OpenWrite**-Methode angeben. 

## <a name="delete-blobs"></a>Löschen von Blobs

Die folgenden Schritte veranschaulichen, wie Sie ein Blob löschen. 

1. Öffnen Sie die Datei `BlobsController.cs` .

1. Fügen Sie eine Methode namens **DeleteBlob** hinzu, die ein **ActionResult** zurückgibt.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *&lt;storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Rufen Sie ein **CloudBlobClient**-Objekt ab, das einen Blob-Dienst-Client darstellt.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Rufen Sie ein **CloudBlobContainer**-Objekt ab, das einen Verweis auf den Blobcontainernamen darstellt. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Rufen Sie ein Blobverweisobjekt durch Aufrufen der **CloudBlobContainer.GetBlockBlobReference**- oder **CloudBlobContainer.GetPageBlobReference**-Methode ab. (Ändern Sie *&lt;blob-name>* in den Namen des Blobs, das Sie löschen.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. To delete a blob, use the **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Views > Shared**, und öffnen Sie `_Layout.cshtml`.

1. Fügen Sie nach dem letzten **Html.ActionLink** den folgenden **Html.ActionLink** hinzu.

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Führen Sie die Anwendung aus, und wählen Sie **Blob löschen** zum Löschen des Blobs, das Sie im Aufruf der **CloudBlobContainer.GetBlockBlobReference**-Methode angegeben haben. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu zusätzlichen Optionen für das Speichern von Daten in Azure finden Sie in den anderen Featureleitfäden.

  * [Erste Schritte mit Tabellenspeicher und verbundenen Visual Studio-Diensten (ASP.NET)](./vs-storage-aspnet-getting-started-tables.md)
  * [Erste Schritte mit Azure-Warteschlangenspeicher und verbundenen Visual Studio-Diensten](./vs-storage-aspnet-getting-started-queues.md)


<!--HONumber=Dec16_HO3-->


