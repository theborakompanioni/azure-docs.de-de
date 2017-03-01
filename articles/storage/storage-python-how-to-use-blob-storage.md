---
title: Verwenden des Azure-Blobspeichers (Objektspeicher) mit Python | Microsoft Docs
description: Speichern Sie nicht strukturierte Daten in der Cloud mit Azure Blob Storage (Objektspeicher).
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 0348e360-b24d-41fa-bb12-b8f18990d8bc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 2/24/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: f0875344b05b7eb8c6f9f6e39c3b864c202e2931
ms.openlocfilehash: 968814db9496fd410162d482191592c8a56101f0
ms.lasthandoff: 02/27/2017


---
# <a name="how-to-use-azure-blob-storage-from-python"></a>Verwenden des Azure-Blob-Speichers mit Python
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht
Der Azure-BLOB-Speicher ist ein Dienst, bei dem unstrukturierte Daten in der Cloud als Objekte/Blobs gespeichert werden. In Blob Storage können alle Arten von Text- oder Binärdaten gespeichert werden, z. B. ein Dokument, eine Mediendatei oder ein Installer einer Anwendung. Der Blobspeicher wird auch als Objektspeicher bezeichnet.

In diesem Artikel wird die Durchführung gängiger Szenarien mit dem Blob-Speicher demonstriert. Die Beispiele sind in Python geschrieben und verwenden das [Microsoft Azure Storage-SDK für Python]. Die behandelten Szenarien umfassen das Hochladen, Auflisten, Herunterladen und Löschen von Blobs.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Erstellen eines Containers
Erstellen Sie basierend auf dem Typ des Blobs, das Sie verwenden möchten, ein **BlockBlobService**-, ein **AppendBlobService**- oder ein **PageBlobService**-Objekt. Der folgende Code verwendet ein **BlockBlobService** -Objekt. Fügen Sie am Anfang jeder Python-Datei, in der Sie programmgesteuert auf Azure-Blockblobspeicher zugreifen möchten, den folgenden Code hinzu.

```python
from azure.storage.blob import BlockBlobService
```

Mit dem folgenden Code wird unter Verwendung des Speicherkontonamens und des Kontoschlüssels ein **BlockBlobService** -Objekt erstellt.  Ersetzen Sie „myaccount“ und „mykey“ durch Ihren Kontonamen und Schlüssel.

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Im folgenden Codebeispiel können Sie den Container mithilfe eines **BlockBlobService** -Objekts erstellen, falls er nicht vorhanden ist.

```python
block_blob_service.create_container('mycontainer')
```

Standardmäßig ist der neue Container privat, sodass Sie Ihren Speicherzugriffsschlüssel wie zuvor angeben müssen, um Blobs aus diesem Container herunterzuladen. Wenn die Blobs im Container für alle verfügbar sein sollen, können Sie den Container mithilfe des folgenden Codes erstellen und öffentlich zugänglich machen.

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

Alternativ können Sie einen Container nach der Erstellung mit dem folgenden Code ändern.

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

Nach dieser Änderung kann jeder Benutzer im Internet Blobs in einem öffentlichen Container sehen, aber nur Sie können die Blobs ändern oder löschen.

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines Blobs in einen Container
Verwenden Sie zum Erstellen eines Blockblobs und zum Hochladen von Daten die Methoden **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** oder **create\_blob\_from\_text**. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

**create\_blob\_from\_path** lädt den Inhalt einer Datei aus dem angegebenen Pfad hoch, **create\_blob\_from\_stream** lädt den Inhalt einer Datei aus einer bereits geöffneten Datei/einem bereits geöffneten Stream. **create\_blob\_from\_bytes** lädt ein Byte-Array hoch, **create\_blob\_from\_text** lädt den angegebenen Textwert wie vorgegeben codiert hoch (Standardcodierung ist UTF-8).

Das folgende Beispiel lädt den Inhalt der Datei **sunset.png** in das Blob **myblob** hoch.

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container
Verwenden Sie zum Auflisten des Blobs in einem Container die Methode **list\_blobs**. Diese Methode gibt einen Generator zurück. Der folgende Code gibt den **Namen** der einzelnen Blobs in einem Container in der Konsole aus.

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a>Herunterladen von Blobs
Um Daten aus einem Blob herunterzuladen, verwenden Sie **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes** oder **get\_blob\_to\_text**. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

Das folgende Beispiel verwendet **get\_blob\_to\_path**, um den Inhalt des Blobs **myblob** herunterzuladen und in der Datei **out-sunset.png** zu speichern.

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a>Löschen eines Blobs
Um ein Blob zu löschen, rufen Sie **delete_blob** auf.

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-to-an-append-blob"></a>Beschreiben eines Anfügeblobs
Anfügeblobs sind für Anfügevorgäng wie die Protokollierung optimiert. Ein Anfügeblob besteht wie ein Blockblob aus Blöcken. Allerdings ist es bei einem Anfügeblob so, dass ein neuer Block immer ans Ende des Blobs angefügt wird. Das Aktualisieren oder Löschen eines vorhandenen Blocks ist in einem Anfügeblob nicht möglich. Anders als bei Blockblobs sind die Block-IDs sind für Anfügeblobs nicht verfügbar.

In einem Anfügeblob kann jeder Block unterschiedlich groß sein, bis maximal 4 MB. Insgesamt können bis zu 50.000 Blöcke enthalten sein. Die maximale Größe eines Anfügeblobs ist deshalb etwas mehr als 195 GB (4 MB X 50.000 Blöcke).

Das folgende Beispiel erstellt ein neues Anfügeblob und fügt einige Daten hinzu, um eine einfache Protokollierung zu simulieren.

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# The same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von Blobspeichern vertraut gemacht haben, lesen Sie die folgenden Artikel, um mehr zu erfahren.

* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [REST-API für Azure-Speicherdienste](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure Storage-Teamblog]
* [Microsoft Azure Storage-SDK für Python]

[Azure Storage-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage-SDK für Python]: https://github.com/Azure/azure-storage-python

