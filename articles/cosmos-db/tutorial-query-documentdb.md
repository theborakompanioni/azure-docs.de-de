---
title: "Wie werden Abfragen mit SQL in Azure Cosmos DB durchgeführt? | Microsoft Docs"
description: "Erfahren Sie, wie Sie Abfragen mit DocumentDB-Daten mit SQL in Azure Cosmos DB durchführen können."
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmosdb
ms.custom: tutorial-develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: dd34ff43e78175b0d6a6e38bbd1303070f6549ab
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="azure-cosmos-db-how-to-query-using-sql"></a>Azure Cosmos DB: Wie werden Abfragen mit SQL durchgeführt?

Die [DocumentDB-API](../documentdb/documentdb-introduction.md) von Azure Cosmos DB unterstützt die Abfrage von Dokumenten mit SQL. Dieser Artikel stellt ein Beispieldokument sowie zwei SQL-Beispielabfragen und Ergebnisse vor.

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Abfragen von Daten mit SQL

## <a name="sample-document"></a>Beispieldokument

Die SQL-Abfragen in diesem Artikel verwenden das folgende Beispieldokument.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a>Wo kann ich SQL-Abfragen durchführen?

Sie können Abfragen mit dem Daten-Explorer im Azure-Portal, über die [REST-API und SDKs](../documentdb/documentdb-query-collections-query-explorer.md) und auch auf dem [Query Playground](https://www.documentdb.com/sql/demo) durchführen, der Abfragen an einem vorhandenen Satz von Beispieldaten durchführt.

Weitere Informationen zu SQL-Abfragen finden Sie hier:
* [SQL-Abfrage und SQL-Syntax](../documentdb/documentdb-sql-query.md)

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial setzt voraus, dass Sie über Azure Cosmos DB-Konto und -Sammlung verfügen. Sie haben beides nicht? Absolvieren Sie den [5-Minuten-Schnellstart](create-mongodb-nodejs.md) oder das [Entwicklertutorial](tutorial-develop-mongodb.md) zum Erstellen eines Kontos und einer Sammlung.

## <a name="example-query-1"></a>Beispielabfrage 1

Im Beispiel des obigen Familienbeispieldokuments gibt die folgende SQL-Abfrage die Dokumente zurück, in denen das Feld „id“ mit `WakefieldFamily` übereinstimmt. Da es sich um eine `SELECT *`-Anweisung handelt, ist die Ausgabe der Abfrage das komplette JSON-Dokument:

**Abfragen**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

## <a name="example-query-2"></a>Beispielabfrage 2

Die nächste Abfrage gibt alle Vornamen von Kindern der Familie zurück, deren „id“ `WakefieldFamily` entspricht, und zwar geordnet nach Schulklassen.

**Abfragen**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Ergebnisse**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Sie haben gelernt, wie Sie Abfragen mithilfe von SQL durchführen.  

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Daten global verteilen.

> [!div class="nextstepaction"]
> [Globales Verteilen Ihrer Daten](tutorial-global-distribution-documentdb.md)


