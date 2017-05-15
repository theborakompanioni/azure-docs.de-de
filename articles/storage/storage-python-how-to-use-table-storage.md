---
title: Verwenden des Tabellenspeichers mit Python | Microsoft Docs
description: Speichern Sie strukturierte Daten mit Azure Table Storage, einem NoSQL-Datenspeicher, in der Cloud.
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 34d075658514045e28d6a784c579528bb3eac376
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-use-table-storage-from-python"></a>Verwenden des Tabellenspeichers mit Python
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Übersicht
In diesem Leitfaden wird die Durchführung häufiger Szenarien mit dem Azure-Tabellenspeicherdienst demonstriert. Die Beispiele sind in Python geschrieben und verwenden das [Microsoft Azure Storage-SDK für Python](https://github.com/Azure/azure-storage-python). Die behandelten Szenarien umfassen das Erstellen und Löschen einer Tabelle sowie das Einfügen und Abfragen von Tabellenentitäten.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Erstellen einer Tabelle
Das **TableService** -Objekt ermöglicht Ihnen das Arbeiten mit Tabellenspeicherdiensten. Der folgende Code erstellt ein **TableService** -Objekt. Fügen Sie am Anfang jeder Python-Datei, in der Sie programmgesteuert auf Azure-Speicher zugreifen möchten, den folgenden Code hinzu:

```python
from azure.storage.table import TableService, Entity
```

Der folgende Code erstellt ein **TableService** -Objekt unter Verwendung des Speicherkontonamens und Kontoschlüssels.  Ersetzen Sie „myaccount“ und „mykey“ durch Ihren Kontonamen und Schlüssel.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle
Um eine Entität hinzuzufügen, erstellen Sie zunächst ein Wörterbuch oder eine Entität zum Definieren des Eigenschaftsnamens der Entität und ihrer Werte. Beachten Sie, dass Sie für jede Entität **PartitionKey** und **RowKey** angeben müssen. Dies sind die eindeutigen Bezeichner der Entität. Sie können diese Werte viel schneller als andere Eigenschaften abfragen. Das System verwendet **PartitionKey** , um die Entitäten der Tabelle automatisch über viele Speicherknoten zu verteilen.
Entitäten mit dem gleichen **PartitionKey** werden auf dem gleichen Knoten gespeichert. **RowKey** -Wert ist eine eindeutige ID der Entität innerhalb der Partition, zu der sie gehört.

Um eine Entität zu Ihrer Tabelle hinzuzufügen, übergeben Sie das Wörterbuchobjekt an die **insert\_entity**-Methode.

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Sie können auch eine Instanz der **Entity**-Klasse an die **insert\_entity**-Methode übergeben.

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '2'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

## <a name="update-an-entity"></a>Aktualisieren einer Entität
Dieser Code zeigt, wie Sie die alte Version einer existierenden Entität durch eine neue Version ersetzen können.

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')
```

Der Aktualisierungsvorgang schlägt fehl, wenn die zu aktualisierende Entität nicht existiert. Daher sollten Sie **insert\_or\_replace_entity** verwenden, wenn Sie eine Entität unabhängig davon speichern möchten, ob diese bereits vorhanden ist.
Der erste Aufruf im folgenden Beispiel ersetzt die existierende Entität. Der zweite Aufruf fügt eine neue Entität ein, da keine Entität mit dem angegebenen **PartitionKey** und **RowKey** in der Tabelle existiert.

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')
```

## <a name="change-a-group-of-entities"></a>Ändern einer Gruppe von Entitäten
Gelegentlich ist es sinnvoll, mehrere Vorgänge zusammen in einem Batch zu senden, um die atomische Verarbeitung durch den Server sicherzustellen. Verwenden Sie hierzu die **TableBatch** -Klasse. Wenn Sie den Stapel übermitteln möchten, rufen Sie **commit\_batch** auf. Beachten Sie, dass sich alle Entitäten in derselben Partition befinden müssen, um diese als Batch ändern zu können. Das folgende Beispiel fügt zwei Entitäten als Batch ein.

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task10)
batch.insert_entity(task11)
table_service.commit_batch('tasktable', batch)
```

Batches können auch mit der Kontext-Manager-Syntax verwendet werden:

```python
task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task12)
    batch.insert_entity(task13)
```

## <a name="query-for-an-entity"></a>Abfragen einer Entität
Um eine Entität in einer Tabelle abzufragen, verwenden Sie die **get\_entity**-Methode und übergeben **PartitionKey** und **RowKey**.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Abfragen einer Gruppe von Entitäten
Dieses Beispiel fragt alle Aufgaben in Seattle anhand des **PartitionKey**-Werts ab.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Abfragen einer Teilmenge von Entitätseigenschaften
Mit einer Abfrage einer Tabelle können nur einige wenige Eigenschaften einer Entität aufgerufen werden.
Diese Technik namens *Projektion*reduziert die Bandbreite und kann die Abfrageleistung, besonders für große Entitäten, verbessern. Verwenden Sie den **select** -Parameter und übergeben Sie die Namen der Eigenschaften, die an den Client übermittelt werden sollen.

Mit der Abfrage im folgenden Code werden nur die Beschreibungen von Entitäten in der Tabelle zurückgegeben.

> [!NOTE]
> Der folgende Codeausschnitt funktioniert nur mit Azure Storage. Er wird nicht vom Speicheremulator unterstützt.
>
>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Löschen einer Entität
Sie können eine Entität unter Verwendung ihres Partitions- und Zeilenschlüssels löschen.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '1')
```

## <a name="delete-a-table"></a>Löschen einer Tabelle
Mit dem folgenden Code wird eine Tabelle aus einem Speicherkonto gelöscht.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Nächste Schritte

* Beim [Microsoft Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.
* [Python Developer Center](/develop/python/)
* [REST-API für Azure-Speicherdienste](http://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure Storage-SDK für Python](https://github.com/Azure/azure-storage-python)
