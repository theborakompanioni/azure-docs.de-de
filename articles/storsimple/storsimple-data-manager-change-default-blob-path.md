---
title: "Ändern eines Blobpfads von der Standardeinstellung | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie eine Azure-Funktion einrichten, um den Pfad einer Blob-Datei umzubenennen (private Vorschau).
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 057d4d7370207859617eb63238bf425bfa6d3e16
ms.lasthandoff: 04/12/2017

---

# <a name="change-a-blob-path-from-the-default-path-private-preview"></a>Ändern eines Blobpfads von der Standardeinstellung (private Vorschau)

Dieser Artikel beschreibt, wie Sie eine Azure-Funktion einrichten, um einen Standardpfad für eine Blob-Datei umzubenennen. 

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich, dass Sie über eine Auftragsdefinition verfügen, die korrekt in einer Ressource für Hybriddaten innerhalb einer Ressourcengruppe konfiguriert ist.

## <a name="create-an-azure-function"></a>Erstellen einer Azure-Funktion

Gehen Sie wie folgt vor, um eine Azure-Funktion zu erstellen:

1. Öffnen Sie das [Azure-Portal](http://portal.azure.com/).

2. Klicken Sie oben im linken Bereich auf **Neu**. 

3. Geben Sie im Feld **Suche** den Text **Funktionen-App** ein, und drücken Sie dann die EINGABETASTE.

    ![Geben Sie im Suchfeld „Funktionen-App“ ein.](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. Klicken Sie in der Liste **Ergebnisse** auf **Funktionen-App**.

    ![Wählen Sie die Ressource zur Funktionen-App in der Ergebnisliste aus.](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    Das Fenster **Funktionen-App** wird geöffnet.

5. Klicken Sie auf **Erstellen**.

    ![Die Schaltfläche „Erstellen“ im Fenster „Funktionen-App“](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. Gehen Sie auf dem Konfigurationsblatt **Funktionen-App** wie folgt vor:

    a. Geben Sie im Feld **App-Name** den Namen der App ein.
    
    b. Geben Sie im Feld **Abonnement** den Namen des Abonnements ein.

    c. Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen**, und geben Sie den Namen der Ressourcengruppe ein.

    d. Geben Sie im Feld **Hostingplan** den Text **Verbrauchstarif** ein.

    e. Geben Sie im Feld **Speicherort** den Speicherort ein.

    f. Wählen Sie unter **Speicherkonto** ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues Speicherkonto. Ein Speicherkonto wird intern für die Funktion verwendet.

    ![Neue Funktionen-App-Konfigurationsdaten eingeben](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. Klicken Sie auf **Erstellen**.  
    Die Funktionen-App wird erstellt.

8. Klicken Sie im linken Bereich auf **Weitere Dienste**, und gehen Sie dann wie folgt vor:
    
    a. Geben Sie im Feld **Filter** den Text **App Services** ein.
    
    b. Klicken Sie auf **App Services**. 

    ![Klicken Sie im linken Bereich auf „Weitere Dienste“.](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. Klicken Sie in der Liste der App Services auf den Namen der Funktionen-App.  
    Die Seite „Funktionen-App“ wird geöffnet.

10. Klicken Sie im linken Bereich auf **Neue Funktion**, und gehen Sie dann wie folgt vor: 

    a. Wählen Sie in der Liste **Sprache** den Eintrag **C#** aus.
    
    b. Wählen Sie im Raster der Vorlagenkacheln **QueueTrigger-CSharp** aus.

    c. Geben Sie im Feld **Funktion benennen** einen Namen für Ihre Funktion ein.

    d. Geben Sie im Feld **Warteschlangenname** den Namen für die Auftragdefinition zur Datentransformation ein.

    e. Klicken Sie unter **Speicherkontoverbindung** auf **Neu**, und wählen Sie dann das Konto aus, das dem Datentransformationsauftrag entspricht.  
        Notieren Sie sich den Verbindungsnamen. Der Name wird später in der Azure-Funktion benötigt.

       ![Erstellen Sie eine neue C#-Funktion.](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. Klicken Sie auf **Erstellen**.  
    Das Fenster **Funktion** wird geöffnet.

11. Führen Sie im Fenster **Funktion** die _CSX_-Datei aus, und gehen Sie dann wie folgt vor:

    a. Fügen Sie folgenden Code ein:

    ```
    using System;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Queue;
    using Microsoft.WindowsAzure.Storage;
    using System.Collections.Generic;
    using System.Linq;

    public static void Run(QueueItem myQueueItem, TraceWriter log)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

        string storageAccUriEndswith = "windows.net/";
        string uri = myQueueItem.TargetLocation.Replace("%20", " ");
        log.Info($"Blob Uri: {uri}");

        // Remove storage account uri string
        uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

        string containerName = uri.Substring(0, uri.IndexOf("/")); 

        // Remove container name string
        uri = uri.Substring(containerName.Length + 1);

        // Current blob path
        string blobName = uri; 

        string volumeName = uri.Substring(containerName.Length + 1);
        volumeName = uri.Substring(0, uri.IndexOf("/"));

        // Remove volume name string
        uri = uri.Substring(volumeName.Length + 1);

        string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
        string newBlobName = uri.Substring(newContainerName.Length + 1);

        log.Info($"Container name: {containerName}");
        log.Info($"Volume name: {volumeName}");
        log.Info($"New container name: {newContainerName}");

        log.Info($"Blob name: {blobName}");
        log.Info($"New blob name: {newBlobName}");

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Container reference
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
        newContainer.CreateIfNotExists();

        if(!container.Exists())
        {
            log.Info($"Container - {containerName} not exists");
            return;
        }

        if(!newContainer.Exists())
        {
            log.Info($"Container - {newContainerName} not exists");
            return;
        }

        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
        if (!blob.Exists())
        {
            // Skip to copy the blob to new container, if source blob doesn't exist
            log.Info($"The specified blob does not exist.");
            log.Info($"Blob Uri: {blob.Uri}");
            return;
        }

        CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
        if (!blobCopy.Exists())
        {
            blobCopy.StartCopy(blob);
            // Delete old blob, after copy to new container
            blob.DeleteIfExists();
            log.Info($"Blob file path renamed completed successfully");
        }
        else
        {
            log.Info($"Blob file path renamed already done");
            // Delete old blob, if already exists.
            blob.DeleteIfExists();
        }
    }

    public class QueueItem
    {
        public string SourceLocation {get;set;}
        public long SizeInBytes {get;set;}
        public string Status {get;set;}
        public string JobID {get;set;}
        public string TargetLocation {get; set;}
    }

    ```

    b. Ersetzen Sie **STORAGE_CONNECTIONNAME** in Zeile 11 durch Ihre Speicherkontoverbindung (siehe Punkt 8c).

    c. Klicken Sie links oben auf **Speichern**.

    ![Funktion speichern](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. Um die Funktion zu vervollständigen, fügen Sie wie folgt eine weitere Datei hinzu:

    a. Klicken Sie auf **Dateien anzeigen**.

       ![Link „Dateien anzeigen“](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. Klicken Sie auf **Hinzufügen**.
    
    c. Geben Sie **project.json** ein, und drücken Sie dann die EINGABETASTE.
    
    d. Fügen Sie folgenden Code in die Datei **project.json** ein:

    ```
    {
    "frameworks": {
        "net46":{
        "dependencies": {
            "windowsazure.storage": "8.1.1"
        }
        }
    }
    }

    ```

    e. Klicken Sie auf **Speichern**.

Sie haben eine Azure-Funktion erstellt. Diese Funktion wird jedes Mal ausgelöst, wenn vom Datentransformationsauftrag ein neues Blob generiert wird.

## <a name="next-steps"></a>Nächste Schritte

[Use StorSimple Data Manager UI to transform your data (Verwenden der StorSimple Data Manager-UI zum Transformieren von Daten)](storsimple-data-manager-ui.md)

