---
title: "Entwickeln für Azure File Storage mit Java | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Java-Anwendungen und -Dienste entwickeln, die Azure File Storage zum Speichern von Dateidaten verwenden.
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 3bfbfa7f-d378-4fb4-8df3-e0b6fcea5b27
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 05/27/2017
ms.author: robinsh
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 16924599e49990265e07f7a58613756d93c46942
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# <a name="develop-for-azure-file-storage-with-java"></a>Entwickeln für Azure File Storage mit Java
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>Informationen zu diesem Tutorial
Dieses Tutorial veranschaulicht die grundlegende Verwendung von Java bei der Entwicklung von Anwendungen oder Diensten, die Azure File Storage zum Speichern von Dateidaten verwenden. In diesem Tutorial erstellen wir eine einfache Konsolenanwendung und zeigen Ihnen, wie Sie grundlegende Aktionen mit Java und Azure File Storage ausführen:

* Erstellen und Löschen von Azure-Dateifreigaben
* Erstellen und Löschen von Verzeichnissen
* Auflisten von Dateien und Verzeichnissen in einer Azure-Dateifreigabe
* Hochladen, Herunterladen und Löschen einer Datei

> [!Note]  
> Da auf Azure File Storage über SMB zugegriffen werden kann, können Sie unter Verwendung der standardmäßigen Java-E/A-Klassen einfache Anwendungen mit Zugriff auf die Azure-Dateifreigabe schreiben. In diesem Artikel erfahren Sie, wie Sie Anwendungen schreiben, die das Azure Storage Java SDK verwenden, das über die [Azure File Storage-REST-API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) mit Azure File Storage kommuniziert.

## <a name="create-a-java-application"></a>Erstellen einer Java-Anwendung
Zum Erstellen der Beispiele benötigen Sie das Java Development Kit (JDK) und das [Azure Storage-SDK für Java][]. Sie sollten auch ein Azure-Speicherkonto erstellt haben.

## <a name="setup-your-application-to-use-azure-file-storage"></a>Einrichten der Anwendung für das Verwenden von Azure File Storage
Um die Azure-Speicher-APIs zu verwenden, fügen Sie die folgende Anweisung am Anfang der Java-Datei dort ein, wo der Zugriff auf den Speicherdienst erfolgen soll.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Azure-Speicherverbindungszeichenfolge
Zur Verwendung von Azure File Storage müssen Sie eine Verbindung mit Ihrem Azure-Speicherkonto herstellen. Im ersten Schritt muss eine Verbindungszeichenfolge konfiguriert werden, über die die Verbindung mit dem Speicherkonto hergestellt wird. Definieren Sie dazu eine statische Variable.

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Ersetzen Sie "your_storage_account_name" und "your_storage_account_key" durch die tatsächlichen Werte für Ihr Speicherkonto.
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a>Herstellen einer Verbindung mit einem Azure-Speicherkonto
Um eine Verbindung mit Ihrem Speicherkonto herzustellen, müssen Sie das **CloudStorageAccount**-Objekt verwenden, um eine Verbindungszeichenfolge an die zugehörige **parse**-Methode zu übergeben.

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

**CloudStorageAccount.parse** löst eine InvalidKeyException aus, daher müssen Sie sie in einem try/catch-Block platzieren.

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe
Alle Dateien und Verzeichnisse in Azure File Storage befinden sich in einem Container mit dem Namen **Freigabe**. Das Speicherkonto kann so viele Freigaben aufweisen, wie es die Kapazität des Kontos zulässt. Für den Zugriff auf eine Freigabe und ihren Inhalt muss ein Azure File Storage-Client verwendet werden.

```java
// Create the Azure File storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Mit dem Azure File Storage-Client können Sie dann einen Verweis auf die Freigabe abrufen.

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

Um die Freigabe zu erstellen, verwenden Sie die **createIfNotExists** -Methode des CloudFileShare-Objekts.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

An diesem Punkt enthält **share** einen Verweis auf eine Freigabe mit dem Namen **sampleshare**.

## <a name="delete-an-azure-file-share"></a>Löschen einer Azure-Dateifreigabe
Das Löschen einer Freigabe erfolgt durch Aufrufen der **deleteIfExists** -Methode für ein CloudFileShare-Objekt. Hier ein Beispielcode dazu.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Sie können zudem den Speicher organisieren, indem Sie Dateien in Unterverzeichnissen ablegen, anstatt alle Dateien im Stammverzeichnis zu speichern. Mit Azure File Storage können Sie so viele Verzeichnisse erstellen wie in Ihrem Konto zugelassen. Mit dem folgenden Code wird ein Unterverzeichnis mit dem Namen **sampledir** im Stammverzeichnis erstellt.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses
Das Löschen eines Verzeichnisses ist eine ziemlich einfache Angelegenheit. Dabei ist jedoch zu beachten, dass Verzeichnisse, die noch Dateien oder andere Verzeichnisse enthalten, nicht gelöscht werden können.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Auflisten von Dateien und Verzeichnissen in einer Azure-Dateifreigabe
Eine Liste von Dateien und Verzeichnissen in einer Freigabe kann einfach durch Aufrufen von **listFilesAndDirectories** in einem CloudFileDirectory-Verweis abgerufen werden. Die Methode gibt eine Liste von ListFileItem-Objekten zurück, die Sie durchlaufen können. Mit dem folgenden Code werden beispielsweise Dateien und Verzeichnisse im Stammverzeichnis aufgelistet.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Hochladen einer Datei
Eine Azure-Dateifreigabe enthält mindestens ein Stammverzeichnis, in dem Dateien gespeichert werden können. In diesem Abschnitt erfahren Sie, wie Sie eine Datei vom lokalen Speicher in das Stammverzeichnis einer Freigabe hochladen.

Im ersten Schritt beim Hochladen einer Datei wird ein Verweis auf das Verzeichnis abgerufen, in dem sie gespeichert werden soll. Rufen Sie dazu die **getRootDirectoryReference** -Methode des Freigabeobjekts auf.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Da Sie nun einen Verweis auf das Stammverzeichnis der Freigabe erstellt haben, können Sie mit dem folgenden Code eine Datei in dieses Verzeichnis hochladen.

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Herunterladen einer Datei
Ein Vorgang, den Sie in Bezug auf Azure File Storage häufiger durchführen werden, ist das Herunterladen von Dateien. Im folgenden Beispiel wird mit dem Code die Datei "SampleFile.txt" heruntergeladen und ihr Inhalt angezeigt.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a>Löschen von Dateien
Ein weiterer häufiger Azure File Storage-Vorgang ist das Löschen von Dateien. Mit dem folgenden Code wird die Datei "SampleFile.txt" gelöscht, die im Verzeichnis **sampledir**gespeichert ist.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a>Nächste Schritte
Folgen Sie diesen Links, wenn Sie mehr über andere Azure-Speicher-APIs erfahren möchten.

* [Java Developer Center](http://azure.microsoft.com/develop/java/)
* [Azure Storage-SDK für Java](https://github.com/azure/azure-storage-java)
* [Azure Storage-SDK für Android](https://github.com/azure/azure-storage-android)
* [Referenz für Azure Storage-Client-SDKs](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Storage-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
* [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)
