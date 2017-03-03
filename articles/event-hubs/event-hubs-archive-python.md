---
title: "Exemplarische Vorgehensweise für Azure Event Hubs Archive | Microsoft-Dokumentation"
description: "Enthält ein Beispiel, in dem das Azure Python SDK verwendet wird, um das Event Hubs-Archiv-Feature zu demonstrieren."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: darosa;sethm
translationtype: Human Translation
ms.sourcegitcommit: 25dd25d8f8f0388ed7ef11bb26344ad7199fde2e
ms.openlocfilehash: 3f0487fba592426c835d81a46a752697ecf34d8b
ms.lasthandoff: 02/03/2017


---
# <a name="event-hubs-archive-walkthrough-python"></a>Exemplarische Vorgehensweise für Event Hubs-Archiv: Python
Event Hubs Archive ist ein neues Feature von Event Hubs, mit dem Sie Streamingdaten in Ihrem Event Hub automatisch an ein Azure Blob Storage-Konto Ihrer Wahl übermitteln können. Dies erleichtert die Batchverarbeitung für Echtzeit-Streamingdaten. In diesem Artikel wird beschrieben, wie Sie das Event Hubs-Archiv mit Python nutzen. Weitere Informationen zum Event Hubs-Archiv finden Sie im [Übersichtsartikel](event-hubs-archive-overview.md).

In diesem Beispiel wird das [Azure Python SDK](https://azure.microsoft.com/develop/python/) verwendet, um das Archive-Feature zu demonstrieren. Mit dem Programm „sender.py“ werden simulierte Telemetriedaten der Umgebung im JSON-Format an Event Hubs gesendet. Der Event Hub ist für die Verwendung des Archiv-Features konfiguriert, damit diese Daten in Batches in den Blobspeicher geschrieben werden können. Mit der App „archivereader.py“ werden dieses Blobs dann gelesen, und es wird eine Anfügedatei pro Gerät erstellt. Anschließend werden die Daten in CSV-Dateien geschrieben.

Ziele

1. Erstellen eines Azure Blob Storage-Kontos und eines darin enthaltenen Blobcontainers mit dem Azure-Portal
2. Erstellen eines Event Hub-Namespace mit dem Azure-Portal
3. Erstellen eines Event Hub mit aktiviertem Archive-Feature mit dem Azure-Portal
4. Senden von Daten an den Event Hub mit einem Python-Skript
5. Auslesen der Dateien aus dem Archiv und Verarbeiten mit einem anderen Python-Skript

Voraussetzungen

- Python 2.7.x
- Ein Azure-Abonnement
- Ein aktiver [Event Hubs-Namespace und Event Hubs](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos
1. Melden Sie sich beim [Azure-Portal][Azure portal] an.
2. Klicken Sie im Portal im Navigationsbereich auf der linken Seite auf **Neu**, **Speicher** und **Speicherkonto**.
3. Füllen Sie die Felder auf dem Blatt „Speicherkonto“ aus, und klicken Sie dann auf **Erstellen**.
   
   ![][1]
4. Klicken Sie nach dem Erscheinen der Meldung **Die Bereitstellungen waren erfolgreich** auf den Namen des neuen Speicherkontos und dann auf dem Blatt **Zusammenfassung** auf **Blobs**. Klicken Sie oben auf **+ Container**, nachdem das Blatt **Blob-Dienst** geöffnet wurde. Geben Sie dem Container den Namen **archive**, und schließen Sie das Blatt **Blob-Dienst**.
5. Klicken Sie auf dem linken Blatt auf **Zugriffsschlüssel**, und kopieren Sie den Namen des Speicherkontos und den Wert von **key1**. Speichern Sie diese Werte im Editor oder an einem anderen temporären Speicherort.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Erstellen eines Python-Skripts zum Senden von Ereignissen an Ihren Event Hub
1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code][Visual Studio Code].
2. Erstellen Sie ein Skript mit dem Namen **sender.py**. Mit diesem Skript werden 200 Ereignisse an Ihren Event Hub gesendet. Es handelt sich hierbei um einfache Umweltdaten im JSON-Format.
3. Fügen Sie den folgenden Code in „sender.py“ ein:
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
       print y
   ```
4. Aktualisieren Sie den obigen Code, damit Ihr Namespacename, die Schlüsselwerte und der Event Hub-Name verwendet werden, die Sie beim Erstellen des Event Hubs-Namespace erhalten haben.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Erstellen eines Python-Skripts zum Lesen Ihrer Archivdateien
1. Füllen Sie das Blatt aus, und klicken Sie auf **Erstellen**.
2. Erstellen Sie ein Skript mit dem Namen **archivereader.py**. Mit diesem Skript werden die Archivdateien gelesen, und es wird eine Datei pro Gerät erstellt, in die nur die Daten für das jeweilige Gerät geschrieben werden.
3. Fügen Sie den folgenden Code in „archivereader.py“ ein:
   
   ```python
   import os
   import string
   import json
   import avro.schema
   from avro.datafile import DataFileReader, DataFileWriter
   from avro.io import DatumReader, DatumWriter
   from azure.storage.blob import BlockBlobService
   
   def processBlob(filename):
       reader = DataFileReader(open(filename, 'rb'), DatumReader())
       dict = {}
       for reading in reader:
           parsed_json = json.loads(reading["Body"])
           if not 'id' in parsed_json:
               return
           if not dict.has_key(parsed_json['id']):
           list = []
           dict[parsed_json['id']] = list
       else:
           list = dict[parsed_json['id']]
           list.append(parsed_json)
       reader.close()
       for device in dict.keys():
           deviceFile = open(device + '.csv', "a")
           for r in dict[device]:
               deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
   def startProcessing(accountName, key, container):
       print 'Processor started using path: ' + os.getcwd()
       block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
       generator = block_blob_service.list_blobs(container)
       for blob in generator:
           if blob.properties.content_length != 0:
               print('Downloaded a non empty blob: ' + blob.name)
               cleanName = string.replace(blob.name, '/', '_')
               block_blob_service.get_blob_to_path(container, blob.name, cleanName)
               processBlob(cleanName)
               os.remove(cleanName)
           block_blob_service.delete_blob(container, blob.name)
   startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
   ```
4. Fügen Sie in den Aufruf von `startProcessing`die entsprechenden Werte für Ihren Speicherkontonamen und den Schlüssel ein.

## <a name="run-the-scripts"></a>Ausführen der Skripts
1. Öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie dann diese Befehle aus, um die Python-Pakete mit den erforderlichen Komponenten zu installieren:
   
   ```
   pip install azure-storage
   pip install azure-servicebus
   pip install avro
   ```
   
   Wenn Sie über eine frühere Version von „azure-storage“ oder „azure“ verfügen, müssen Sie unter Umständen die Option **--upgrade** verwenden.
   
   Ggf. müssen Sie auch Folgendes ausführen (für die meisten Systeme nicht erforderlich):
   
   ```
   pip install cryptography
   ```
2. Ändern Sie das Verzeichnis in Ihr Verzeichnis, in dem Sie „sender.py“ und „archivereader.py“ gespeichert haben, und führen Sie diesen Befehl aus:
   
   ```
   start python sender.py
   ```
   
   Ein neuer Python-Prozess zum Ausführen des Absenders wird gestartet.
3. Warten Sie einige Minuten, bis das Archiv ausgeführt wird. Geben Sie dann den folgenden Befehl in das ursprüngliche Befehlsfenster ein:
   
    ```
    python archivereader.py
    ```

    Dieser Archivprozessor nutzt das lokale Verzeichnis, um alle Blobs aus dem Speicherkonto/Container herunterzuladen. Er verarbeitet alle Blobs, die nicht leer sind, und schreibt die Ergebnisse als CSV-Dateien in das lokale Verzeichnis.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs Archive][Overview of Event Hubs Archive]
* Eine vollständige [Beispielanwendung mit Verwendung von Ereignis Hubs][sample application that uses Event Hubs].
* Das Beispiel [Horizontales Hochskalieren der Ereignisverarbeitung mit Event Hubs][Scale out Event Processing with Event Hubs]
* [Übersicht über Event Hubs][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Archive]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: ../storage/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3

