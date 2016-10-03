<properties
	pageTitle="Verwenden des Azure-Blobspeichers (Objektspeicher) mit Python | Microsoft Azure"
	description="Speichern Sie nicht strukturierte Daten in der Cloud mit Azure Blob Storage (Objektspeicher)."
	services="storage"
	documentationCenter="python"
	authors="tamram"
	manager="carmonm"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="python"
	ms.topic="article"
    ms.date="09/20/2016"
	ms.author="jwillis;tamram"/>

# Verwenden des Azure-Blob-Speichers mit Python

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## Übersicht

Der Azure-BLOB-Speicher ist ein Dienst, bei dem unstrukturierte Daten in der Cloud als Objekte/Blobs gespeichert werden. In Blob Storage können alle Arten von Text- oder Binärdaten gespeichert werden, z. B. ein Dokument, eine Mediendatei oder ein Installer einer Anwendung. Der Blobspeicher wird auch als Objektspeicher bezeichnet.

In diesem Artikel wird die Durchführung gängiger Szenarien mit dem Blob-Speicher demonstriert. Die Beispiele sind in Python geschrieben und verwenden das [Microsoft Azure Storage-SDK für Python]. Die behandelten Szenarien umfassen das Hochladen, Auflisten, Herunterladen und Löschen von Blobs.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Erstellen eines Containers

Erstellen Sie basierend auf dem Typ des Blobs, das Sie verwenden möchten, ein **BlockBlobService**-, ein **AppendBlobService**- oder ein **PageBlobService**-Objekt. Der folgende Code verwendet ein **BlockBlobService**-Objekt. Fügen Sie am Anfang jeder Python-Datei, in der Sie programmgesteuert auf Azure-Blockblobspeicher zugreifen möchten, den folgenden Code hinzu.

	from azure.storage.blob import BlockBlobService

Mit dem folgenden Code wird unter Verwendung des Speicherkontonamens und des Kontoschlüssels ein **BlockBlobService**-Objekt erstellt. Ersetzen Sie „myaccount“ und „mykey“ durch Ihren Kontonamen und Schlüssel.

	block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Im folgenden Codebeispiel können Sie den Container mithilfe eines **BlockBlobService**-Objekts erstellen, falls er nicht vorhanden ist.

	block_blob_service.create_container('mycontainer')

Standardmäßig ist der neue Container privat, sodass Sie Ihren Speicherzugriffsschlüssel wie zuvor angeben müssen, um Blobs aus diesem Container herunterzuladen. Wenn die Blobs im Container für alle verfügbar sein sollen, können Sie den Container mithilfe des folgenden Codes erstellen und öffentlich zugänglich machen.

	from azure.storage.blob import PublicAccess
	block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)

Alternativ können Sie einen Container nach der Erstellung mit dem folgenden Code ändern.

	block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)

Nach dieser Änderung kann jeder Benutzer im Internet Blobs in einem öffentlichen Container sehen, aber nur Sie können die Blobs ändern oder löschen.

## Hochladen eines Blobs in einen Container

Verwenden Sie zum Erstellen eines Blockblobs und zum Hochladen von Daten die Methoden **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** oder **create\_blob\_from\_text**. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

**create\_blob\_from\_path** lädt den Inhalt einer Datei aus dem angegebenen Pfad hoch, und **create\_blob\_from\_stream** lädt den Inhalt aus einer bereits geöffneten Datei/einem Datenstrom. **create\_blob\_from\_bytes** lädt ein Bytearray hoch, und **create\_blob\_from\_text** lädt den angegebenen Textwert unter Verwendung der festgelegten Codierung (standardmäßig UTF-8) hoch.

Das folgende Beispiel lädt den Inhalt der Datei **sunset.png** in das Blob **myblob** hoch.

	from azure.storage.blob import ContentSettings
	block_blob_service.create_blob_from_path(
        'mycontainer',
        'myblockblob',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png')
				)

## Auflisten der Blobs in einem Container

Verwenden Sie zum Auflisten des Blobs in einem Container die Methode **list\_blobs**. Diese Methode gibt einen Generator zurück. Der folgende Code gibt den **Namen** der einzelnen Blobs in einem Container in der Konsole aus.

	generator = block_blob_service.list_blobs('mycontainer')
	for blob in generator:
		print(blob.name)

## Herunterladen von Blobs

Verwenden Sie **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes** oder **get\_blob\_to\_text**, um Daten aus einem Blob herunterzuladen. Dies sind allgemeine Methoden zur Durchführung der erforderlichen Teilung, wenn die Größe der Daten 64 MB übersteigt.

Das folgende Beispiel verwendet **get\_blob\_to\_path**, um den Inhalt des Blobs **myblob** herunterzuladen und in der Datei **out-sunset.png** zu speichern.

	block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')

## Löschen eines BLOBs

Um ein Blob zu löschen, rufen Sie **delete\_blob** auf.

	block_blob_service.delete_blob('mycontainer', 'myblockblob')

## Beschreiben eines Anfügeblobs

Anfügeblobs sind für Anfügevorgäng wie die Protokollierung optimiert. Ein Anfügeblob besteht wie ein Blockblob aus Blöcken. Allerdings ist es bei einem Anfügeblob so, dass ein neuer Block immer ans Ende des Blobs angefügt wird. Das Aktualisieren oder Löschen eines vorhandenen Blocks ist in einem Anfügeblob nicht möglich. Anders als bei Blockblobs sind die Block-IDs sind für Anfügeblobs nicht verfügbar.

In einem Anfügeblob kann jeder Block unterschiedlich groß sein, bis maximal 4 MB. Insgesamt können bis zu 50.000 Blöcke enthalten sein. Die maximale Größe eines Anfügeblobs ist deshalb etwas mehr als 195 GB (4 MB X 50.000 Blöcke).

Das folgende Beispiel erstellt ein neues Anfügeblob und fügt einige Daten hinzu, um eine einfache Protokollierung zu simulieren.

	from azure.storage.blob import AppendBlobService
	append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

	# The same containers can hold all types of blobs
	append_blob_service.create_container('mycontainer')

	# Append blobs must be created before they are appended to
	append_blob_service.create_blob('mycontainer', 'myappendblob')
	append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

	append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')

## Nächste Schritte

Nachdem Sie sich nun mit den Grundlagen von Blobspeichern vertraut gemacht haben, lesen Sie die folgenden Artikel, um mehr zu erfahren.

- [Python Developer Center](/develop/python/)
- [REST-API für Azure-Speicherdienste](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure Storage-Teamblog]
- [Microsoft Azure Storage-SDK für Python]

[Azure Storage-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage-SDK für Python]: https://github.com/Azure/azure-storage-python

<!---HONumber=AcomDC_0921_2016-->