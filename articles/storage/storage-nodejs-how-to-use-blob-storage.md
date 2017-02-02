---
title: Verwenden des Blobspeichers mit Node.js | Microsoft Docs
description: Speichern Sie nicht strukturierte Daten in der Cloud mit Azure Blob Storage (Objektspeicher).
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 931503f56b32ce9d1b11283dff7224d7e2f015ae
ms.openlocfilehash: 15485bc6032fb12b18cdac171ec5d6e6c617f4e7


---
# <a name="how-to-use-blob-storage-from-nodejs"></a>Verwenden des Blob-Speichers mit Node.js
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht
In diesem Artikel wird die Durchführung gängiger Szenarien mit Blob Storage demonstriert. Die Beispiele werden über die Node.js-API geschrieben. Die behandelten Szenarien umfassen das Hochladen, Auflisten, Herunterladen und Löschen von Blobs.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Erstellen einer Node.js-Anwendung
Anleitungen zum Erstellen von Node.js-Anwendungen finden Sie unter [Erstellen einer Node.js-Web-App in Azure App Service], [Erstellen und Bereitstellen einer Node.js-Anwendung in einem Azure-Clouddienst] – mithilfe von Windows PowerShell – oder [Erstellen und Bereitstellen einer Node.js-Web-App für Azure mithilfe von WebMatrix].

## <a name="configure-your-application-to-access-storage"></a>Konfigurieren der Anwendung für den Speicherzugriff
Um Azure-Speicher verwenden zu können, müssen Sie das Azure Storage-SDK für Node.js herunterladen. Es enthält eine Reihe von Bibliotheken, die mit den REST-Speicherdiensten kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden von Node Package Manager (NPM) zum Beziehen des Pakets
1. Verwenden Sie eine Befehlszeilenschnittstelle, z.B. **PowerShell** (Windows,) **Terminal** (Mac) oder **Bash** (Unix), und navigieren Sie zum Ordner, in dem Sie die Beispielanwendung erstellt haben.
2. Geben Sie **npm install azure-storage** in das Befehlsfenster ein. Die Ausgabe des Befehls ähnelt dem folgenden Codebeispiel.

  azure-storage@0.5.0 node_modules\azure-storage +-- extend@1.2.1 +-- xmlbuilder@0.4.3 +-- mime@1.2.11 +-- node-uuid@1.4.3 +-- validator@3.22.2 +-- underscore@1.4.4 +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1) +-- xml2js@0.2.7 (sax@0.5.2) +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3. Sie können den Befehl **ls** manuell ausführen, um zu überprüfen, ob der Ordner **node\_modules** erstellt wurde. In diesem Ordner finden Sie das Paket **azure-storage** mit den Bibliotheken, die Sie benötigen, um auf den Speicher zuzugreifen.

### <a name="import-the-package"></a>Importieren des Pakets
Verwenden Sie Editor oder einen anderen Texteditor, um die folgende Zeile am Anfang der Datei **server.js** der Anwendung einzufügen, in der Sie den Speicher nutzen möchten:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Azure-Speicherverbindung
Das Azure-Modul entnimmt den Umgebungsvariablen `AZURE_STORAGE_ACCOUNT` und `AZURE_STORAGE_ACCESS_KEY` oder `AZURE_STORAGE_CONNECTION_STRING` die erforderlichen Informationen zum Herstellen einer Verbindung mit Ihrem Azure-Speicherkonto. Falls diese Umgebungsvariablen nicht gesetzt sind, müssen Sie die Kontoinformationen beim Aufruf von **createBlobService**angeben.

Ein Beispiel zum Festlegen der Umgebungsvariablen für eine Azure-Web-App im [Azure-Portal](https://portal.azure.com) finden Sie unter [Node.js-Web-App mit Azure-Tabellenspeicherdienst].

## <a name="create-a-container"></a>Erstellen eines Containers
Mit dem **BlobService** -Objekt können Sie auf Container und Blobs zugreifen. Der folgende Code erstellt ein **BlobService** -Objekt. Fügen Sie die folgende Zeile am Anfang von **server.js**hinzu:

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> Sie können anonym auf ein Blob zugreifen, indem Sie **createBlobServiceAnonymous** verwenden und die Hostadresse angeben. Verwenden Sie z. B. `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Verwenden Sie zum Erstellen eines neuen Containers **createContainerIfNotExists**. Im folgenden Codebeispiel wird ein neuer Container namens „mycontainer“ erstellt:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

Wenn der Container neu erstellt wird, weist `result.created` den Wert „true“ auf. Wenn der Container bereits vorhanden ist, hat `result.created` den Wert FALSE. `response` enthält Informationen über den Vorgang, einschließlich der ETag-Informationen für den Container.

### <a name="container-security"></a>Containersicherheit
Neue Container sind standardmäßig privat. Ein anonymer Zugriff ist auf sie so nicht möglich. Um den Container öffentlich zu machen, damit ein anonymer Zugriff darauf möglich ist, können Sie die Zugriffsebene des Containers auf **blob** oder **container** festlegen.

* **blob** – ermöglicht den anonymen Lesezugriff auf Blobinhalte und Metadaten in diesem Container, aber nicht auf Containermetadaten wie Listen aller Blobs in einem Container.
* **container** – ermöglicht den anonymen Lesezugriff auf Blobinhalte und Metadaten sowie auf Containermetadaten.

Das folgende Codebeispiel zeigt, wie Sie die Zugriffsebene auf **blob**setzen:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access to blob
      // content and metadata within this container
    }
});
```

Alternativ können Sie die Zugriffsebene eines Containers mit **setContainerAcl** angeben. Das folgende Codebeispiel ändert die Zugriffsebene des Containers:

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set to 'container'
  }
});
```

Das Ergebnis enthält Informationen über den Vorgang, einschließlich der **ETag** -Informationen für den Container.

### <a name="filters"></a>Filter
Sie können optionale Filtervorgänge auf Vorgänge anwenden, die mithilfe von **BlobService** ausgeführt werden. Filtervorgänge können Protokollierung, automatische Wiederholung usw. umfassen. Filter sind Objekte, die eine Methode mit der folgenden Signatur implementieren:

```nodejs
function handle (requestOptions, next)
```

Nachdem die Vorverarbeitung der Anforderungsoptionen abgeschlossen ist, muss die Methode "next" aufrufen und hierbei eine Rückruffunktion mit der folgenden Signatur übergeben:

```nodejs
function (returnObject, finalCallback, next)
```

Nachdem das "returnObject"-Objekt (die Antwort auf die an den Server gesendete Anforderung) verarbeitet wurde, muss in dieser Rückruffunktion entweder "next" aufgerufen werden, wenn die Tabelle vorhanden ist, um weitere Filter zu verarbeiten, oder es muss einfach "finallCallback" aufgerufen werden, um den Dienstaufruf zu beenden.

Zwei Filter, die eine Wiederholungslogik implementieren, sind im Azure SDK für Node.js enthalten: **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**. Der folgende Code erstellt ein **BlobService**-Objekt, das den **ExponentialRetryPolicyFilter** verwendet:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines Blobs in einen Container
Es gibt drei Arten von Blobs: Blockblobs, Seitenblobs und Anfügeblobs. Blockblobs ermöglichen Ihnen das effizientere Hochladen großer Datenmengen. Anfügeblobs sind für Anfügevorgänge optimiert. Seitenblobs sind für Lese-/Schreibvorgänge optimiert. Weitere Informationen finden Sie unter [Grundlagen zu Blockblobs, Anfügeblobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blockblobs
Verwenden Sie zum Hochladen von Daten zu einem Blockblob Folgendes:

* **createBlockBlobFromLocalFile** – erstellt einen neuen Blockblob und lädt die Inhalte einer Datei hoch.
* **createBlockBlobFromStream** – erstellt einen neuen Blockblob und lädt die Inhalte eines Datenstroms hoch.
* **createBlockBlobFromText** – erstellt einen neuen Blockblob und lädt die Inhalte einer Zeichenfolge hoch.
* **createWriteStreamToBlockBlob** – stellt einen Schreibdatenstrom für einen Blockblob bereit.

Das folgende Codebeispiel lädt den Inhalt der Datei **test.txt** in **myblob** hoch.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Das von diesen Methoden zurückgegebene `result` enthält Informationen über den Vorgang wie z. B. das **ETag** des Blobs.

### <a name="append-blobs"></a>Anfügeblobs
Verwenden Sie zum Hochladen von Daten zu einem neuen Anfügeblob Folgendes:

* **createAppendBlobFromLocalFile** – erstellt ein neues Anfügeblob und lädt die Inhalte einer Datei hoch.
* **createAppendBlobFromStream** – erstellt ein neues Anfügeblob und lädt die Inhalte eines Datenstroms hoch.
* **createAppendBlobFromText** – erstellt ein neues Anfügeblob und lädt die Inhalte einer Zeichenfolge hoch.
* **createWriteStreamToNewAppendBlob** – erstellt ein neues Anfügeblob und stellt dann einen Datenstrom bereit, um darin schreiben zu können.

Das folgende Codebeispiel lädt den Inhalt der Datei **test.txt** in **myappendblob** hoch.

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Um einen Block an einen vorhandenen Anfügeblob anzufügen, verwenden Sie Folgendes:

* **appendFromLocalFile** – fügt den Inhalt einer Datei an ein vorhandenes Anfügeblob an.
* **appendFromStream** – fügt den Inhalt eines Datenstroms an ein vorhandenes Anfügeblob an.
* **appendFromText** – fügt den Inhalt einer Zeichenfolge an ein vorhandenes Anfügeblob an.
* **appendBlockFromStream** – fügt den Inhalt eines Datenstroms an ein vorhandenes Anfügeblob an.
* **appendBlockFromText** – fügt den Inhalt einer Zeichenfolge an ein vorhandenes Anfügeblob an.

> [!NOTE]
> appendFromXXX-APIs führen einige clientseitige Validierungen aus, um damit unnötige Serveraufrufe zu vermeiden. appendBlockFromXXX-APIs machen dies nicht.
>
>

Das folgende Codebeispiel lädt den Inhalt der Datei **test.txt** in **myappendblob** hoch.

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a>Seitenblobs
Verwenden Sie zum Hochladen von Daten zu einem Seitenblob Folgendes:

* **createPageBlob** – erstellt einen neuen Seitenblob mit einer bestimmten Länge.
* **createPageBlobFromLocalFile** – erstellt einen neuen Seitenblob und lädt die Inhalte einer Datei hoch.
* **createPageBlobFromStream** – erstellt einen neuen Seitenblob und lädt die Inhalte eines Datenstroms hoch.
* **createWriteStreamToExistingPageBlob** – stellt einen Schreibdatenstrom für einen vorhandenen Seitenblob bereit.
* **createWriteStreamToNewPageBlob** – erstellt ein neues Seitenblob und stellt dann einen Datenstrom bereit, um darin schreiben zu können.

Das folgende Codebeispiel lädt den Inhalt der Datei **test.txt** in **mypageblob** hoch.

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> Seitenblobs bestehen aus 512-Byte-"Seiten". Ihnen wird ein Fehler angezeigt, wenn Daten mit einer Größe, die kein Vielfaches von 512 ist, hochgeladen werden.
>
>

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container
Verwenden Sie zum Auflisten des Blobs in einem Container die Methode **listBlobsSegmented** . Verwenden Sie **listBlobsSegmentedWithPrefix**, wenn Sie Blobs mit einem bestimmten Präfix ausgeben möchten.

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains the entries
      // If not all blobs were returned, result.continuationToken has the continuation token.
  }
});
```

Das `result` enthält eine `entries`-Sammlung. Hierbei handelt es sich um ein Array von Objekten, die die einzelnen Blobs beschreiben. Wenn nicht alle Blobs zurückgegeben werden können, stellt das `result` auch ein `continuationToken` bereit, das als zweiter Parameter zum Abrufen zusätzlicher Einträge verwendet werden kann.

## <a name="download-blobs"></a>Herunterladen von Blobs
Verwenden Sie zum Herunterladen von Daten aus einem Blob Folgendes:

* **getBlobToLocalFile** – schreibt die Blobinhalte in eine Datei.
* **getBlobToStream** – schreibt die Blobinhalte in einen Datenstrom.
* **getBlobToText** – schreibt die Blobinhalte in eine Zeichenfolge.
* **createReadStream** – stellt einen Datenstrom bereit, um aus dem Blob zu lesen.

Das folgende Codebeispiel verwendet **getBlobToStream**, um den Inhalt des **myblob**-Blobs herunterzuladen und mithilfe eines Datenstroms in der Datei **output.txt** zu speichern:

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

Das `result` enthält Informationen zum Blob, einschließlich der **ETag** -Informationen.

## <a name="delete-a-blob"></a>Löschen eines BLOBs
Um einen Blob zu löschen, rufen Sie **deleteBlob**auf. Im folgenden Codebeispiel wird das Blob namens **myblob**gelöscht.

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a>Paralleler Zugriff
Um den parallelen Zugriff auf eine Blob von mehreren Clients oder mehreren Prozessinstanzen zu unterstützen, können Sie **ETags** oder **Leases** verwenden.

* **Etag** – bietet eine Möglichkeit, um zu ermitteln, dass das Blob oder der Container durch einen anderen Prozess geändert wurde.
* **Lease** – bietet eine Möglichkeit zum Abrufen eines exklusiven, erneuerbaren Lese- oder Schreibzugriffs auf ein Blob für einen Zeitraum.

### <a name="etag"></a>ETag
Verwenden Sie ETags, wenn mehrere Clients oder Instanzen gleichzeitig in das Blockblob oder das Seitenblob schreiben müssen. Das ETag erlaubt Ihnen zu ermitteln, ob der Container oder das Blob geändert wurde, seitdem Sie es erstmals gelesen oder erstellt haben. Dadurch können Sie Überschreibungsänderungen vermeiden, die durch einen anderen Client oder Prozess verübt wurden.

Sie können ETag-Bedingungen mithilfe des optionalen `options.accessConditions` -Parameters festlegen. Im folgenden Codebeispiel wird die Datei **test.txt** nur hochgeladen, wenn das Blob bereits vorhanden ist und den ETag-Wert aufweist, der in `etagToMatch` enthalten ist.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

Wenn Sie ETags verwenden, lautet das allgemeine Muster:

1. Rufen Sie das ETag als Ergebnis eines "create"-, "list"- oder "get"-Vorgangs ab.
2. Führen Sie eine Aktion aus, bei der Sie sicherstellen, dass der ETag-Wert nicht geändert wurde.

Wenn der Wert geändert wurde, deutet dies darauf hin, dass ein anderer Client oder eine Instanz das Blob oder den Container geändert hat, seitdem Sie den ETag-Wert abgerufen haben.

### <a name="lease"></a>Lease
Sie können eine neue Lease mithilfe der Methode **acquireLease** abrufen, die das Blob oder den Container angibt, für das bzw. den Sie eine Lease abrufen möchten. Mit dem folgenden Code wird beispielsweise eine Lease auf **myblob** abgerufen.

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

Nachfolgende Vorgänge mit **myblob** müssen den `options.leaseId`-Parameter angeben. Die Lease-ID wird als `result.id` von **acquireLease**zurückgegeben.

> [!NOTE]
> Die Leasedauer ist standardmäßig unendlich. Sie können eine endliche Dauer (zwischen 15 und 60 Sekunden) festlegen, indem Sie den `options.leaseDuration` -Parameter angeben.
>
>

Verwenden Sie zum Entfernen einer Lease **releaseLease**. Zum Unterbrechen einer Lease, während andere darin gehindert werden sollen, eine neue Lease abzurufen, bis die ursprüngliche Dauer abgelaufen ist, verwenden Sie **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Arbeiten mit Shared Access Signatures
Shared Access Signatures (SAS) sind eine sichere Möglichkeit zum Bereitstellen einen granularen Zugriffs auf Blobs und Container, ohne dass dabei Ihr Speicherkontoname oder -schlüssel angegeben werden müssen. Shared Access Signatures werden oftmals zum Bereitstellen des beschränkten Zugriffs auf Ihre Daten verwendet, dazu zählt beispielsweise auch, einer mobilen App den Zugriff auf Blobs zu gewähren.

> [!NOTE]
> Während Sie auch den anonymen Zugriff auf Blobs erlauben können, verleihen Ihnen Shared Access Signatures einen kontrollierteren Zugriff, da Sie die Shared Access Signatures generieren müssen.
>
>

Eine vertrauenswürdige Anwendung wie ein cloudbasierter Dienst generiert mittels **generateSharedAccessSignature** von **BlobService** Shared Access Signatures und stellt diese für eine nicht vertrauenswürdige oder teilweise vertrauenswürdige Anwendung wie beispielsweise eine mobile App bereit. Shared Access Signatures werden mithilfe einer Richtlinie generiert, die das Anfangs- und das Enddatum der Gültigkeit der Shared Access Signatures sowie die Zugriffsstufe definiert, die dem Inhaber der Shared Access Signatures gewährt wird.

Im folgenden Codebeispiel wird eine neue Richtlinie für den geteilten Zugriff generiert, wodurch der Inhaber von Shared Access Signatures Lesevorgänge im **myblob** -Blob vornehmen kann, die 100 Minuten nach ihrer Erstellung abläuft.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

Beachten Sie, dass auch die Hostinformationen angegeben werden müssen, da sie erforderlich sind, wenn der Inhaber von Shared Access Signatures versucht, auf den Container zuzugreifen.

Die Clientanwendung verwendet dann die Shared Access Signatures mit **BlobServiceWithSAS** , um Vorgänge auf das Blob auszuführen. Im Folgenden werden Informationen über **myblob**abgerufen.

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

Da Shared Access Signatures mit einem schreibgeschützten Zugriff generiert wurden, wird ein Fehler bei dem Versuch zurückgegeben, den Blob zu ändern.

### <a name="access-control-lists"></a>Zugriffssteuerungslisten
Sie können auch eine Zugriffssteuerungsliste (Access Control List, ACL) verwenden, um die Zugriffsrichtlinie für SAS festzulegen. Dies ist nützlich, wenn mehrere Clients auf einen Container zugreifen sollen und für jeden Client unterschiedliche Zugriffsrichtlinien bereitgestellt werden.

Eine ACL wird in einem Array von Zugriffsrichtlinien implementiert, wobei jeder Richtlinie eine ID zugeordnet wird. Im folgenden Codebeispiel werden zwei Richtlinien definiert, eine für „user1“ und eine für „user2“:

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Im folgenden Codebeispiel wird die aktuelle Zugriffssteuerungsliste für **mycontainer** abgerufen. Anschließend werden die neuen Richtlinien mithilfe von **setBlobAcl** hinzugefügt. Dieser Ansatz ermöglicht Folgendes:

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Nachdem die ACL festgelegt wurde, können Sie basierend auf der ID für eine Richtlinie Shared Access Signatures erstellen. Im folgenden Codebeispiel werden neue Shared Access Signatures für „user2“ erstellt:

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen.

* [API-Referenz zum Azure Storage-SDK für Node][Azure Storage SDK for Node API Reference]
* [Azure Storage-Teamblog][Azure Storage Team Blog]
* [Azure Storage SDK für Node.js][Azure Storage SDK for Node]-Repository auf GitHub
* [Node.js Developer Center](/develop/nodejs/)
* [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[Erstellen einer Node.js-Web-App in Azure App Service]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js-Web-App mit Azure-Tabellenspeicherdienst]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Erstellen und Bereitstellen einer Node.js-Web-App für Azure mithilfe von WebMatrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure portal]: https://portal.azure.com
[Erstellen und Bereitstellen einer Node.js-Anwendung in einem Azure-Clouddienst]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html



<!--HONumber=Dec16_HO2-->


