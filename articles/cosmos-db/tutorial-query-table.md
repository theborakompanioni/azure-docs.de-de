---
title: "Wie werden Abfragen von Tabellendaten in Azure Cosmos DB durchgeführt? | Microsoft Docs"
description: Erfahren Sie, wie Sie Tabellendaten in Azure Cosmos DB abfragen.
services: cosmosdb
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmosdb
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: govindk
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: cdd855aeac7dd30c52accb407289ca6db7dab4ae
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="azure-cosmos-db-how-to-query-with-the-table-api-preview"></a>Azure Cosmos DB: Wie werden Abfragen mit der Tabellen-API (Vorschau) durchgeführt?

Die [Tabellen-API](table-introduction.md) (Vorschau) von Azure Cosmos DB unterstützt OData- und [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service)-Abfragen von Schlüssel-/Wertdaten (Tabellendaten).  

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Abfragen von Daten mit der Tabellen-API

## <a name="sample-table"></a>Beispieltabelle

Die Abfragen in diesem Artikel verwenden die folgende Beispieltabelle `People`:

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Walter | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

## <a name="about-the-table-api-preview"></a>Informationen zur Tabellen-API (Vorschau)

Da Azure Cosmos DB mit den Azure Table Storage-APIs kompatibel ist, informieren Sie sich unter [Querying Tables and Entities] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) (Abfragen von Tabellen und Entitäten) näher über Abfragen mit der Tabellen-API. 

Weitere Informationen zu Premium-Funktionen von Azure Cosmos DB finden Sie unter [Introduction to Azure Cosmos DB: Table API](table-introduction.md) (Einführung in Azure Cosmos DB: Table-API) und [Azure Cosmos DB: Develop with the Table API in .NET](tutorial-develop-table-dotnet.md) (Azure Cosmos DB: Entwickeln mit der Tabellen-API mit .NET). 

## <a name="prerequisites"></a>Voraussetzungen

Diese Abfragen können nur funktionieren, wenn Sie über ein Azure Cosmos DB-Konto verfügen und Entitätsdaten im Container vorliegen. Sie haben beides nicht? Absolvieren Sie den [5-Minuten-Schnellstart](https://aka.ms/acdbtnetqs) oder das [Entwicklertutorial](https://aka.ms/acdbtabletut) zum Erstellen eines Kontos und Füllen Ihrer Datenbank.

## <a name="querying-on-partition-key-and-row-key"></a>Abfragen von Partitionsschlüssel und Zeilenschlüssel
Da die Eigenschaften PartitionKey und RowKey den Primärschlüssel einer Entität bilden, können Sie eine spezielle Syntax verwenden, um die Entität wie folgt zu identifizieren: 

**Abfragen**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Ergebnisse**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

Alternativ können Sie diese Eigenschaften als Teil der `$filter`-Option angeben, wie im folgenden Abschnitt gezeigt. Beachten Sie, dass bei den Schlüsseleigenschaftsnamen und Konstantenwerten die Groß-/Kleinschreibung berücksichtigt wird. Die Eigenschaften PartitionKey und RowKey sind vom Typ Zeichenfolge. 

## <a name="querying-with-an-odata-filter"></a>Abfragen mit einem OData-Filter
Wenn Sie eine Filterzeichenfolge erstellen, beachten Sie diese Regeln: 

* Verwenden Sie die von der OData-Protokollspezifikation definierten logischen Operatoren, um eine Eigenschaft mit einem Wert zu vergleichen. Es ist nicht möglich, eine Eigenschaft mit einem dynamischen Wert zu vergleichen; eine Seite des Ausdrucks muss eine Konstante sein. 
* Eigenschaftenname, Operator und Konstantenwert müssen durch URL-codierte Leerzeichen getrennt werden. Ein Leerzeichen wird als `%20` URL-codiert. 
* Bei allen Teilen der Filterzeichenfolge ist die Groß-/Kleinschreibung zu beachten. 
* Der konstante Wert muss den gleichen Datentyp besitzen wie die Eigenschaft, damit vom Filter gültige Ergebnisse zurückgegeben werden. Weitere Informationen zu unterstützten Eigenschaftentypen finden Sie unter [Grundlegendes zum Tabellenspeicherdienst-Datenmodell](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Diese Beispielabfrage zeigt das Filtern nach PartitionKey und die Verwendung eines OData-`$filter` durch die Email-Eigenschaft.

**Abfragen**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Weitere Informationen zur Vorgehensweise beim Erstellen von Filterausdrücken für verschiedene Datentypen sind unter [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities) (Abfragen von Tabellen und Entitäten) verfügbar.

**Ergebnisse**

| PartitionKey | RowKey | Email | PhoneNumber |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="querying-with-linq"></a>Abfragen mit LINQ 
Sie können auch Abfragen mit LINQ durchführen, wobei die Übersetzung in die entsprechenden OData-Abfrageausdrücke erfolgt. Hier ist ein Beispiel zum Erstellen von Abfragen mit dem .NET SDK.

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Sie haben erfahren, wie Sie Abfragen mit der Tabellen-API (Vorschau) erstellen. 

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Daten global verteilen.

> [!div class="nextstepaction"]
> [Globales Verteilen Ihrer Daten](tutorial-global-distribution-documentdb.md)

