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
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 8a06e4b3f482943f55e998c3c68857d3530ff98a
ms.lasthandoff: 03/21/2017

---

# <a name="change-blob-path-from-the-default-private-preview"></a>Ändern eines Blobpfads von der Standardeinstellung (private Vorschau)

Dieser Artikel beschreibt, wie Sie eine Azure-Funktion einrichten, um einen Standardpfad für eine Blob-Datei umzubenennen. 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen stellen Sie sicher, dass Sie über Folgendes verfügen:
* Eine Auftragsdefinition, die korrekt in einer Ressource für Hybriddaten innerhalb einer Ressourcengruppe konfiguriert wurde.

## <a name="create-an-azure-function"></a>Erstellen einer Azure-Funktion

Führen Sie folgenden Schritte aus, um eine Azure-Funktion zu erstellen.

#### <a name="to-create-an-azure-function"></a>So erstellen Sie eine Azure-Funktion

1. Navigieren Sie zum [Azure-Portal](http://portal.azure.com/).

2. Klicken Sie in der linken oberen Ecke auf **+ Neu**. Geben Sie „Funktionen-App“ in das **Suchen**-Textfeld ein, und drücken Sie die **EINGABETASTE**.

    ![Zur Ressource „Funktionen-App“ wechseln](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

3. Klicken Sie in den Ergebnissen auf **Funktionen-App**.

    ![Ressource „Funktionen-App“ auswählen](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

4. Öffnen Sie das Fenster **Funktionen-App**, und klicken Sie auf **Erstellen**.

    ![Erstellen einer neuen Funktionen-App](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

5. Geben Sie auf dem Blatt **Konfiguration** alle einzugebenden Werte ein, und klicken Sie auf **Erstellen**.

    1. App-Name
    2. Abonnement
    3. Ressourcengruppe
    4. Hostingplan: **Consumption Plan**
    5. Standort
    6. Speicherkonto: Verwenden Sie ein vorhandenes Speicherkonto, oder erstellen Sie ein neues Speicherkonto. Ein Speicherkonto wird intern für die Funktion verwendet.

        ![Neue Funktionen-App-Konfigurationsdaten eingeben](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

6. Nachdem die Funktionen-App erstellt ist, navigieren Sie zu **Weitere Dienste >** in der unteren linken Ecke. Geben Sie „App Services“ in das Textfeld **Filter** ein, und klicken Sie auf **App Services**.

    ![Weitere Dienste >](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

7. Klicken Sie in der Liste der App Services auf **Funktionen-App-Name**.

8. Klicken Sie auf **+ Neue Funktion**. Wählen Sie **C#** in der Dropdownliste **Sprache** aus. Wählen Sie die Option **QueueTrigger-CSharp** in der Liste der Vorlagen aus. Geben Sie alle einzugebenden Werte ein.

   1. Name: Geben Sie einen Namen für die Funktion ein.
   2. Warteschlangenname: Geben Sie Ihren **Namen für die Definition des Datentransformationsauftrags** ein.
   3. Speicherkontoverbindung: Klicken Sie auf die Option **Neu**. Wählen Sie das Konto aus, das dem Datentransformationsauftrag entspricht.
      
      Notieren Sie sich den Namen in `Connection name`. Dieser Name wird später in der Azure-Funktion benötigt.

   4. Klicken Sie auf die Schaltfläche **Erstellen**.

       ![Neue C#-Funktion erstellen >](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

9. Führen Sie im Fenster **Funktion** die Datei _run.csx_ aus. Kopieren und fügen Sie den folgenden Code ein:

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
            log.Info($"Blob Uri: {myQueueItem.TargetLocation}");
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            string uriString = "windows.net/";
            string containerName = myQueueItem.TargetLocation;
            containerName = containerName.Substring(containerName.IndexOf(uriString) + uriString.Length);
            containerName = containerName.Substring(0, containerName.IndexOf("/"));
            //log.Info($"Container name: {containerName}");

            CloudBlobContainer container = blobClient.GetContainerReference(containerName);

            if(!container.Exists())
            {
                log.Info($"Container - {containerName} not exists");
                return;
            }

            string containerUri = string.Format("{0}/", container.Uri.ToString());  // Reading container Uri
            string blobUri =  myQueueItem.TargetLocation;
            string blobName = blobUri.Replace(containerUri, string.Empty).Replace("%20", " "); // Reading existing file path after container name
            string newBlobName = blobName.Substring(blobName.IndexOf(string.Format("{0}/", container.Name)) + container.Name.Length + 1); // Reading actual File path after container name

            log.Info($"Blob name: {blobName}");
            log.Info($"New blob name: {newBlobName}");

            CloudBlockBlob blobCopy = container.GetBlockBlobReference(newBlobName);
            CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
            if (!blobCopy.Exists())
            {
                blobCopy.StartCopy(blob);
                // Delete old blob, after copy the blob
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

   1. Ersetzen Sie **STORAGE_CONNECTIONNAME** in Zeile 12 durch Ihre Speicherkontoverbindung (siehe Punkt 8c).
   2. Klicken Sie auf die Schaltfläche **Speichern** in der linken oberen Ecke.

       ![Funktion speichern >](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

10.  Klicken Sie auf **Dateien anzeigen** in der rechten Ecke.

    ![Dateien anzeigen](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

   1. Klicken Sie auf **+ Hinzufügen**. Geben Sie **project.json** ein, und drücken Sie die **EINGABETASTE**.
   2. Kopieren Sie den folgenden Code, und fügen Sie ihn in die Datei **project.json** ein.

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

   2. Klicken Sie auf **Speichern**.

Sie haben eine Azure-Funktion erstellt. Diese Funktion wird jedes Mal ausgelöst, wenn vom Datentransformationsauftrag ein neues Blob generiert wird.

## <a name="next-steps"></a>Nächste Schritte

[Use StorSimple Data Manager UI to transform your data (Verwenden der StorSimple Data Manager-UI zum Transformieren von Daten) ](storsimple-data-manager-ui.md).

