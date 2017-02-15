---
title: Verwenden von Azure File Storage mit Python | Microsoft Docs
description: "Erfahren Sie, wie Sie Azure File Storage mit Python nutzen können, um Dateien hoch- und herunterzuladen, aufzulisten und zu löschen."
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
translationtype: Human Translation
ms.sourcegitcommit: fefebeae665ccd14f15b0197241b30d33830fd09
ms.openlocfilehash: 9973da827ea5a9311904d7d6d4c22d59b5e2d0ce


---
# <a name="how-to-use-azure-file-storage-from-python"></a>Gewusst wie: Verwenden von Azure File Storage mit Python
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Übersicht
In diesem Artikel wird die Durchführung gängiger Szenarios mit File Storage demonstriert. Die Beispiele sind in Python geschrieben und verwenden das [Microsoft Azure Storage-SDK für Python]. Die hier beschriebenen Szenarios umfassen das Hochladen, Auflisten, Herunterladen und Löschen von Dateien.

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Erstellen einer Freigabe
Das **FileService** -Objekt ermöglicht das Arbeiten mit Freigaben, Verzeichnissen und Dateien. Der folgende Code erstellt ein **FileService** -Objekt. Fügen Sie am Anfang jeder Python-Datei, in der Sie programmgesteuert auf Azure-Speicher zugreifen möchten, den folgenden Code hinzu:

```python
from azure.storage.file import FileService
```

Der folgende Code erstellt ein **FileService** -Objekt unter Verwendung des Speicherkontonamens und Kontoschlüssels.  Ersetzen Sie „myaccount“ und „mykey“ durch Ihren Kontonamen und Schlüssel.

```python
file_service = **FileService** (account_name='myaccount', account_key='mykey')
```

Im folgenden Codebeispiel können Sie die Freigabe mithilfe eines **FileService** -Objekts erstellen, falls sie nicht vorhanden ist.

```python
file_service.create_share('myshare')
```

## <a name="upload-a-file-into-a-share"></a>Hochladen einer Datei in eine Freigabe
Eine Azure-Dateispeicherfreigabe enthält mindestens ein Stammverzeichnis, in dem Dateien gespeichert werden können. In diesem Abschnitt erfahren Sie, wie Sie eine Datei vom lokalen Speicher in das Stammverzeichnis einer Freigabe hochladen.

Verwenden Sie zum Erstellen einer Datei und zum Hochladen von Daten die Methoden **create\_file\_from\_path**, **create\_file\_from\_stream**, **create\_file\_from\_bytes** oder **create\_file\_from\_text**. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

**create\_file\_from\_path** lädt den Inhalt einer Datei aus dem angegebenen Pfad hoch, **create\_file\_from\_stream** lädt den Inhalt einer Datei aus einer bereits geöffneten Datei/einem bereits geöffneten Stream. **create\_file\_from\_bytes** lädt ein Byte-Array hoch, **create\_file\_from\_text** lädt den angegebenen Textwert wie vorgegeben codiert hoch (Standardcodierung ist UTF-8).

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

## <a name="how-to-create-a-directory"></a>Erstellen eines Verzeichnisses
Sie können zudem den Speicher organisieren, indem Sie Dateien in Unterverzeichnissen ablegen, anstatt alle Dateien im Stammverzeichnis zu speichern. Mit Azure File Storage können Sie so viele Verzeichnisse erstellen, wie in Ihrem Konto zugelassen sind. Mit dem folgenden Code wird ein Unterverzeichnis mit dem Namen **sampledir** im Stammverzeichnis erstellt.

```python
file_service.create_directory('myshare', 'sampledir')
```

## <a name="how-to-list-files-and-directories-in-a-share"></a>Auflisten von Dateien und Verzeichnissen in einer Freigabe
Verwenden Sie zum Auflisten der Dateien und Verzeichnisse in einer Freigabe die Methode **list\_directories\_and\_files**. Diese Methode gibt einen Generator zurück. Der folgende Code gibt den **Namen** der einzelnen Dateien und Verzeichnisse in einer Freigabe an der Konsole aus.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

## <a name="download-files"></a>Herunterladen von Dateien
Verwenden Sie zum Herunterladen von Daten aus einer Datei **get\_file\_to\_path**, **get\_file\_to\_stream**, **get\_file\_to\_bytes** oder **get\_file\_to\_text**. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

Das folgende Beispiel verwendet **get\_file\_to\_path**, um den Inhalt der Datei **myfile** herunterzuladen und in der Datei **out-sunset.png** zu speichern.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

## <a name="delete-a-file"></a>Löschen von Dateien
Um eine Datei zu löschen, rufen Sie **delete_file** auf.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von File Storage vertraut gemacht haben, lesen Sie die folgenden Artikel, um mehr zu erfahren.

* [Python Developer Center](/develop/python/)
* [REST-API für Azure-Speicherdienste](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure Storage-Teamblog]
* [Microsoft Azure Storage-SDK für Python]

[Azure Storage-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage-SDK für Python]: https://github.com/Azure/azure-storage-python



<!--HONumber=Nov16_HO3-->


