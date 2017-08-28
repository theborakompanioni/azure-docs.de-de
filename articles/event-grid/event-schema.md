---
title: Azure Event Grid-Ereignisschema
description: "Beschreibt die Eigenschaften, die mit Azure Event Grid für Ereignisse bereitgestellt werden."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/15/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 6736c6a60021b51db612f0a596086a9e988d7aef
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---

# <a name="event-grid-event-schema"></a>Event Grid-Ereignisschema

In diesem Artikel werden die Eigenschaften und das Schema für Ereignisse beschrieben. Ereignisse bestehen aus einer Gruppe von fünf erforderlichen Zeichenfolgeneigenschaften und einem erforderlichen **data**-Objekt. Die Eigenschaften gelten für alle Ereignisse von jedem Herausgeber. Das **data**-Objekt enthält Eigenschaften, die für den einzelnen Herausgeber spezifisch sind. Bei Systemthemen sind diese Eigenschaften spezifisch für den Ressourcenanbieter, z.B. Storage oder Event Hubs.

Ereignisse werden an Azure Event Grid in einem Array gesendet, das mehrere Ereignisobjekte enthalten kann. Bei nur einem einzelnen Ereignis hat das Array die Länge 1. 
 
## <a name="event-properties"></a>Ereigniseigenschaften

Alle Ereignisse enthalten die gleichen folgenden Daten der obersten Ebene.

| Eigenschaft | Typ | Beschreibung |
| -------- | ---- | ----------- |
| Thema | string | Vollständiger Ressourcenpfad zu der Ereignisquelle. Dieses Feld ist nicht beschreibbar. |
| subject | string | Vom Herausgeber definierter Pfad zu dem Ereignisbetreff. |
| eventType | string | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | string | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | string | Eindeutiger Bezeichner für das Ereignis. |
| data | objekt | Die für den Ressourcenanbieter spezifischen Ereignisdaten. |

## <a name="available-event-sources"></a>Verfügbare Ereignisquellen

Die folgenden Ereignisquellen veröffentlichen Ereignisse zur Nutzung über Event Grid:

* Ressourcengruppen (Verwaltungsvorgänge)
* Azure-Abonnements (Verwaltungsvorgänge)
* Event Hubs
* Benutzerdefinierte Themen

## <a name="azure-subscriptions"></a>Azure-Abonnements

Azure-Abonnements können jetzt Verwaltungsereignisse von Azure Resource Manager ausgeben, z.B. wenn ein virtueller Computer erstellt oder ein Speicherkonto gelöscht wird.

### <a name="available-event-types"></a>Verfügbare Ereignistypen

- **Microsoft.Resources.ResourceWriteSuccess:** wird ausgelöst, wenn der Erstellungs- oder Aktualisierungsvorgang einer Ressource erfolgreich durchgeführt wird.  
- **Microsoft.Resources.ResourceWriteFailure:** wird ausgelöst, wenn beim Erstellungs- oder Aktualisierungsvorgang einer Ressource ein Fehler auftritt.  
- **Microsoft.Resources.ResourceWriteCancel:** wird ausgelöst, wenn der Erstellungs- oder Aktualisierungsvorgang einer Ressource abgebrochen wird.  
- **Microsoft.Resources.ResourceDeleteSuccess:** wird ausgelöst, wenn der Löschvorgang einer Ressource erfolgreich durchgeführt wird.  
- **Microsoft.Resources.ResourceDeleteFailure:** wird ausgelöst, wenn beim Löschvorgang einer Ressource ein Fehler auftritt.  
- **Microsoft.Resources.ResourceDeleteCancel:** wird ausgelöst, wenn der Löschvorgang einer Ressource abgebrochen wird. Dies erfolgt, wenn die Vorlagenbereitstellung abgebrochen wird.

### <a name="example-event-schema"></a>Beispielereignisschema

```json
[
    {
    "topic":"/subscriptions/{subscription-id}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="resource-groups"></a>Ressourcengruppen

Ressourcengruppen können jetzt Verwaltungsereignisse von Azure Resource Manager ausgeben, z.B. wenn ein virtueller Computer erstellt oder ein Speicherkonto gelöscht wird.

### <a name="available-event-types"></a>Verfügbare Ereignistypen

- **Microsoft.Resources.ResourceWriteSuccess:** wird ausgelöst, wenn der Erstellungs- oder Aktualisierungsvorgang einer Ressource erfolgreich durchgeführt wird.  
- **Microsoft.Resources.ResourceWriteFailure:** wird ausgelöst, wenn beim Erstellungs- oder Aktualisierungsvorgang einer Ressource ein Fehler auftritt.  
- **Microsoft.Resources.ResourceWriteCancel:** wird ausgelöst, wenn der Erstellungs- oder Aktualisierungsvorgang einer Ressource abgebrochen wird.  
- **Microsoft.Resources.ResourceDeleteSuccess:** wird ausgelöst, wenn der Löschvorgang einer Ressource erfolgreich durchgeführt wird.  
- **Microsoft.Resources.ResourceDeleteFailure:** wird ausgelöst, wenn beim Löschvorgang einer Ressource ein Fehler auftritt.  
- **Microsoft.Resources.ResourceDeleteCancel:** wird ausgelöst, wenn der Löschvorgang einer Ressource abgebrochen wird. Dies erfolgt, wenn die Vorlagenbereitstellung abgebrochen wird.

### <a name="example-event"></a>Beispielereignis

```json
[
    {
    "topic":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```



## <a name="event-hubs"></a>Event Hubs

Event Hubs-Ereignisse werden derzeit nur ausgegeben, wenn eine Datei über die Funktion „Erfassen“ automatisch in den Speicher gesendet wird.

### <a name="available-event-types"></a>Verfügbare Ereignistypen

- **Microsoft.EventHub.CaptureFileCreated:** wird ausgelöst, wenn eine Erfassungsdatei erstellt wird.

### <a name="example-event"></a>Beispielereignis

In diesem Beispielereignis ist das Schema eines Event Hubs-Ereignisses dargestellt, das ausgelöst wird, wenn eine Datei über „Erfassen“ gespeichert wird. 

```json
[
    {
        "topic": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{event-hubs-ns}",
        "subject": "eventhubs/eh1",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-07-11T00:55:55.0120485Z",
        "id": "bd440490-a65e-4c97-8298-ef1eb325673c",
        "data": {
            "fileUrl": "https://gridtest1.blob.core.windows.net/acontainer/eventgridtest1/eh1/1/2017/07/11/00/54/54.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 0,
            "eventCount": 0,
            "firstSequenceNumber": -1,
            "lastSequenceNumber": -1,
            "firstEnqueueTime": "0001-01-01T00:00:00",
            "lastEnqueueTime": "0001-01-01T00:00:00"
        },
    }
]

```



## <a name="azure-blob-storage"></a>Azure Blob Storage

Azure Blob Storage in privater Vorschau mit Registrierung für die Integration in Event Grid.

### <a name="available-event-types"></a>Verfügbare Ereignistypen

- **Microsoft.Storage.BlobCreated:** wird ausgelöst, wenn ein Blob erstellt wird.
- **Microsoft.Storage.BlobDeleted:** wird ausgelöst, wenn ein Blob gelöscht wird.

### <a name="example-event"></a>Beispielereignis

In diesem Beispielereignis ist das Schema eines Speicherereignisses dargestellt, das beim Erstellen eines Blob ausgelöst wird. 

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
  }
]
```




## <a name="custom-topics"></a>Benutzerdefinierte Themen

Die Datennutzlast Ihrer benutzerdefinierten Ereignisse wird von Ihnen definiert und kann in jeglichem richtig formatierten JSON-Code vorliegen. Die Daten der obersten Ebene müssen die gleichen Felder wie über Standardressourcen definierte Ereignisse enthalten. Beim Veröffentlichen von Ereignissen in benutzerdefinierten Themen sollten Sie erwägen, den Betreff zur Unterstützung der Weiterleitung und Filterung zu modellieren.

### <a name="example-event"></a>Beispielereignis

Im folgenden Beispiel ist ein Ereignis für ein benutzerdefiniertes Thema dargestellt:
````json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.EventGrid/topics/myeventgridtopic",
    "subject": "/myapp/vehicles/motorcycles",    
    "id": "b68529f3-68cd-4744-baa4-3c0498ec19e2",
    "eventType": "recordInserted",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "data":{
      "make": "Ducati",
      "model": "Monster"
    }
  }
]

````

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Informationen zum Erstellen eines Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).

