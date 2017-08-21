---
title: "Entwickeln für Azure File Storage mit Python | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Python-Anwendungen und -Dienste entwickeln, die Azure File Storage zum Speichern von Dateidaten verwenden.
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: a1a37266908277b54e7b42d85b9b4873af77e622
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# <a name="develop-for-azure-file-storage-with-python"></a>Entwickeln für Azure File Storage mit Python
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm
Dieses Tutorial veranschaulicht die grundlegende Verwendung von Python bei der Entwicklung von Anwendungen oder Diensten, die Azure File Storage zum Speichern von Dateidaten verwenden. In diesem Tutorial erstellen wir eine einfache Konsolenanwendung und zeigen Ihnen, wie Sie grundlegende Aktionen mit Python und Azure File Storage ausführen:

* Erstellen von Azure-Dateifreigaben
* Erstellen von Verzeichnissen
* Auflisten von Dateien und Verzeichnissen in einer Azure-Dateifreigabe
* Hochladen, Herunterladen und Löschen einer Datei

> [!Note]  
> Da auf Azure File Storage über SMB zugegriffen werden kann, können Sie unter Verwendung der standardmäßigen Python-Klassen und -Funktionen für die Ein- und Ausgabe einfache Anwendungen mit Zugriff auf die Azure-Dateifreigabe schreiben. In diesem Artikel erfahren Sie, wie Sie Anwendungen schreiben, die das Azure Storage Python SDK verwenden, das über die [Azure File Storage-REST-API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) mit Azure File Storage kommuniziert.

### <a name="set-up-your-application-to-use-azure-file-storage"></a>Einrichten der Anwendung für das Verwenden von Azure File Storage
Fügen Sie am Anfang jeder Python-Quelldatei, in der Sie programmgesteuert auf Azure-Speicher zugreifen möchten, den folgenden Code hinzu.

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-to-azure-file-storage"></a>Einrichten der Verbindung mit Azure File Storage 
Das `FileService`-Objekt ermöglicht das Arbeiten mit Freigaben, Verzeichnissen und Dateien. Mit dem folgenden Code wird unter Verwendung des Speicherkontonamens und Kontoschlüssels ein `FileService`-Objekt erstellt. Ersetzen Sie `<myaccount>` und `<mykey>` durch Ihren Kontonamen und Schlüssel.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe
Im folgenden Codebeispiel können Sie die Freigabe mithilfe eines `FileService`-Objekts erstellen, falls nicht vorhanden.

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Sie können zudem den Speicher organisieren, indem Sie Dateien in Unterverzeichnissen ablegen, anstatt alle Dateien im Stammverzeichnis zu speichern. Mit Azure File Storage können Sie so viele Verzeichnisse erstellen wie in Ihrem Konto zugelassen. Mit dem folgenden Code wird ein Unterverzeichnis mit dem Namen **sampledir** im Stammverzeichnis erstellt.

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Auflisten von Dateien und Verzeichnissen in einer Azure-Dateifreigabe
Verwenden Sie zum Auflisten der Dateien und Verzeichnisse in einer Freigabe die Methode **list\_directories\_and\_files**. Diese Methode gibt einen Generator zurück. Der folgende Code gibt den **Namen** der einzelnen Dateien und Verzeichnisse in einer Freigabe an der Konsole aus.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a>Hochladen einer Datei 
Eine Azure-Dateifreigabe enthält mindestens ein Stammverzeichnis, in dem Dateien gespeichert werden können. In diesem Abschnitt erfahren Sie, wie Sie eine Datei vom lokalen Speicher in das Stammverzeichnis einer Freigabe hochladen.

Um eine Datei zu erstellen und Daten hochzuladen, verwenden die Methode `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` oder `create_file_from_text`. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

`create_file_from_path` lädt den Inhalt einer Datei aus dem angegebenen Pfad, `create_file_from_stream` den Inhalt einer Datei aus einer bereits geöffneten Datei/einem bereits geöffneten Stream hoch. `create_file_from_bytes` lädt ein Byte-Array, `create_file_from_text` den angegebenen Textwert wie vorgegeben codiert hoch (Standardcodierung ist UTF-8).

Das folgende Beispiel lädt den Inhalt der Datei **sunset.png** in die Datei **myfile** hoch.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a>Herunterladen einer Datei
Verwenden Sie zum Herunterladen von Daten aus einer Datei `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes` oder `get_file_to_text`. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

Das folgende Beispiel verwendet `get_file_to_path`, um den Inhalt der Datei **myfile** herunterzuladen und in der Datei **out-sunset.png** zu speichern.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a>Löschen von Dateien
Um eine Datei zu löschen, rufen Sie `delete_file` auf.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit dem Bearbeiten von Azure File Storage mit Python vertraut gemacht haben, folgen Sie diesen Links, um mehr zu erfahren.

* [Python Developer Center](/develop/python/)
* [REST-API für Azure-Speicherdienste](http://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure Storage-SDK für Python](https://github.com/Azure/azure-storage-python)
