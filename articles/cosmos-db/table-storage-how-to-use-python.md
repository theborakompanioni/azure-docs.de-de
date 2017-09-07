---
title: Verwenden von Azure Table Storage mit Python | Microsoft-Dokumentation
description: Speichern Sie strukturierte Daten mit Azure Table Storage, einem NoSQL-Datenspeicher, in der Cloud.
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 0c46f04786ba4b62bd7ca22c5e25643123e6e136
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="how-to-use-table-storage-in-python"></a>Verwenden von Table Storage in Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

In dieser Anleitung wird gezeigt, wie Sie allgemeine Azure Table Storage-Szenarien in Python mithilfe des [Microsoft Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python) ausführen. Die behandelten Szenarien umfassen das Erstellen und Löschen einer Tabelle sowie das Einfügen und Abfragen von Entitäten.

Beim Durcharbeiten der Szenari in diesem Tutorial finden Sie bei Bedarf weiterführende Informationen in der [API-Referenz zum Azure Storage SDK für Python](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a>Installieren des Azure Storage SDK für Python

Nach dem Erstellen eines Speicherkontos installieren Sie im nächsten Schritt das [Microsoft Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python). Ausführliche Informationen zum Installieren des SDK finden Sie in der Datei [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) im Repository für das Storage SDK für Python auf GitHub.

## <a name="create-a-table"></a>Erstellen einer Tabelle

Um den Azure-Tabellendienst in Python verwenden zu können, müssen Sie das [TableService][py_TableService]-Modul importieren. Da Sie Tabellenentitäten verwenden, benötigen Sie zudem die [Entity][py_Entity]-Klasse. Fügen Sie folgenden Code am Anfang der Python-Datei ein, um beide zu importieren:

```python
from azure.storage.table import TableService, Entity
```

Erstellen Sie ein [TableService][py_TableService]-Objekt zum Übergeben des Namens und Kontoschlüssels Ihres Speicherkontos. Ersetzen Sie `myaccount` und `mykey` durch den Kontonamen und den Kontoschlüssel, und rufen Sie [create_table][py_create_table] auf, um die Tabelle in Azure Storage zu erstellen.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Zum Hinzufügen einer Entität erstellen Sie zunächst ein Objekt, das die Entität darstellt, und übergeben das Objekt dann an die [TableService][py_TableService].[insert_entity][py_insert_entity]-Methode. Das Entitätsobjekt kann ein Wörterbuch oder ein Objekt des Typs [Entity][py_Entity] sein und definiert die Eigenschaftennamen und Eigenschaftswerte der Entität. Jede Entität muss neben allen anderen Eigenschaften, die Sie für die Entität definieren, die erforderlichen Eigenschaften [PartitionKey und RowKey](#partitionkey-and-rowkey) enthalten.

In diesem Beispiel wird ein Wörterbuchobjekt erstellt, das eine Entität darstellt, und dann an die [insert_entity][py_insert_entity]-Methode übergeben, sodass es der Tabelle hinzugefügt wird:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

In diesem Beispiel wird ein [Entity][py_Entity]-Objekt erstellt und dann an die [insert_entity][py_insert_entity]-Methode übergeben, sodass es der Tabelle hinzugefügt wird:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey und RowKey

Sie müssen für jede Entität eine **PartitionKey**- und eine **RowKey**-Eigenschaft angeben. Diese bilden zusammen den Primärschlüssel einer Entität und sind daher die eindeutigen Bezeichner der Entitäten. Diese Werte können sehr viel schneller abgefragt werden als alle anderen Entitätseigenschaften, da nur diese Werte indiziert sind.

Der Tabellendienst verwendet **PartitionKey** für die intelligente Verteilung von Tabellenentitäten auf Speicherknoten. Entitäten mit dem gleichen **PartitionKey** werden auf dem gleichen Knoten gespeichert. **RowKey** ist eine eindeutige ID der Entität in der Partition, zu der sie gehört.

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Rufen Sie zum Aktualisieren aller Eigenschaftswerte einer Entität die [update_entity][py_update_entity]-Methode auf. In diesem Beispiel wird gezeigt, wie eine vorhandene Entität durch eine aktualisierte Version ersetzt wird:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Beim Aktualisierungsvorgang tritt ein Fehler auf, wenn die zu aktualisierende Entität nicht bereits vorhanden ist. Verwenden Sie [insert_or_replace_entity][py_insert_or_replace_entity], wenn Sie eine Entität unabhängig davon speichern möchten, ob sie bereits vorhanden ist. Der erste Aufruf im folgenden Beispiel ersetzt die existierende Entität. Der zweite Aufruf fügt eine neue Entität ein, da keine Entität mit dem angegebenen PartitionKey und RowKey in der Tabelle vorhanden ist.

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> Mit der [update_entity][py_update_entity]-Methode werden alle Eigenschaften und Werte einer vorhandenen Entität ersetzt. Zudem können Sie mit dieser Methode auch Eigenschaften aus einer vorhandenen Entität entfernen. Mit der [merge_entity][py_merge_entity]-Methode können Sie eine vorhandene Entität mit neuen oder geänderten Eigenschaftswerten aktualisieren, ohne die Entität vollständig ersetzen zu müssen.

## <a name="modify-multiple-entities"></a>Ändern mehrerer Entitäten

Um die unteilbare Verarbeitung einer Anforderung durch den Tabellendienst sicherzustellen, können Sie mehrere Vorgänge zusammen in einem Batch senden. Verwenden Sie zunächst die [TableBatch][py_TableBatch]-Klasse, um einem Batch mehrere Vorgänge hinzuzufügen. Rufen Sie anschließend [TableService][py_TableService].[commit_batch][py_commit_batch] auf, um die Vorgänge in einem unteilbaren Vorgang zu übermitteln. Alle als Batch zu ändernden Entitäten müssen sich auf derselben Partition befinden.

In diesem Beispiel werden zwei Entitäten als Batch hinzugefügt:

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Batches können auch mit der Kontext-Manager-Syntax verwendet werden:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Abfragen einer Entität

Zum Abfragen einer Entität in einer Tabelle übergeben Sie den zugehörigen PartitionKey und RowKey an die [TableService][py_TableService].[get_entity][py_get_entity]-Methode.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Abfragen einer Gruppe von Entitäten

Eine Gruppe von Entitäten können Sie abfragen, indem Sie mit dem **filter**-Parameter eine Filterzeichenfolge angeben. In diesem Beispiel werden durch Anwenden eines Filters für PartitionKey alle Aufgaben in Seattle abgefragt:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Abfragen einer Teilmenge von Entitätseigenschaften

Es ist auch möglich, die Eigenschaften einzuschränken, die für jede Entität in einer Abfrage zurückgegeben werden. Bei dieser Methode, der sogenannten *Projektion*, wird die Bandbreite reduziert und die Abfrageleistung kann gesteigert werden, vor allem bei großen Entitäten oder Resultsets. Verwenden Sie den **select**-Parameter, und übergeben Sie die Namen der Eigenschaften, die an den Client zurückgegeben werden sollen.

Mit der Abfrage im folgenden Code werden nur die Beschreibungen von Entitäten in der Tabelle zurückgegeben.

> [!NOTE]
> Der folgende Codeausschnitt funktioniert nur mit Azure Storage. Er wird nicht vom Speicheremulator unterstützt.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Löschen einer Entität

Eine Entität können Sie löschen, indem Sie den zugehörigen PartitionKey und RowKey an die [delete_entity][py_delete_entity]-Methode übergeben.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Löschen einer Tabelle

Wenn Sie eine Tabelle und die Entitäten darin nicht mehr benötigen, können Sie die [delete_table][py_delete_table]-Methode aufrufen, um die Tabelle endgültig in Azure Storage zu löschen.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Nächste Schritte

* [API-Referenz zum Azure Storage SDK für Python](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python)
* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage-Explorer:](../vs-azure-tools-storage-manage-with-storage-explorer.md) kostenlose, plattformübergreifende Anwendung zur visuellen Verwendung von Azure Storage-Daten unter Windows, macOS und Linux.

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch

