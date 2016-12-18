---
title: "NoSQL Python-Beispiele für DocumentDB | Microsoft Docs"
description: "Auf GitHub finden Sie NoSQL Python-Beispiele für gängige Aufgaben in DocumentDB, darunter CRUD-Vorgänge für JSON-Dokumente in NoSQL-Datenbanken."
keywords: Python-Beispiele
services: documentdb
author: moderakh
manager: jhubbard
editor: monicar
documentationcenter: python
ms.assetid: 7f4f8db3-e9db-4645-92ef-7819d486a349
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2016
ms.author: moderakh
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7b555732bad08c86c33ee78344ae9c4e6af7c7b8


---
# <a name="documentdb-python-examples"></a>DocumentDB Python-Beispiele
> [!div class="op_single_selector"]
> * [.NET-Beispiele](documentdb-dotnet-samples.md)
> * [Node.js-Beispiele](documentdb-nodejs-samples.md)
> * [Python-Beispiele](documentdb-python-samples.md)
> * [Katalog mit Azure-Codebeispielen](https://azure.microsoft.com/documentation/samples/?service=documentdb)
> 
> 

Beispiellösungen, in denen CRUD-Vorgänge und andere gängige Vorgänge für Azure DocumentDB-Ressourcen ausgeführt werden, finden Sie im GitHub-Repository [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) . Dieser Artikel enthält Folgendes:

* Links zu den Aufgaben in den einzelnen Python-Beispielprojektdateien. 
* Links zum zugehörigen API-Referenzinhalt.

**Voraussetzungen**

1. Sie benötigen ein Azure-Konto, um diese Python-Beispiele zu verwenden:
   * Sie können [ein Azure-Konto kostenlos erstellen](https://azure.microsoft.com/pricing/free-trial/): Sie erhalten ein Guthaben, das Sie zum Ausprobieren zahlungspflichtiger Azure-Dienste nutzen können, und Sie können das Konto selbst dann behalten und die kostenlosen Azure-Dienste wie Websites nutzen, wenn das Guthaben aufgebraucht ist. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit einer Zahlung einverstanden sind.
     * Sie können Ihre [Vorteile für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): Ihr Visual Studio-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste nutzen können.
2. Sie benötigen zudem das [Python SDK](documentdb-sdk-python.md). 
   
   > [!NOTE]
   > Jedes Beispiel ist eigenständig, richtet sich selbst ein und bereinigt sich selbst. Dementsprechend richten die Beispiele mehrere Aufrufe an [document_client.CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Bei jeder Ausführung dieses Vorgangs wird Ihrem Abonnement 1 Stunde Nutzung gemäß Leistungsebene der erstellten Sammlung in Rechnung gestellt. 
   > 
   > 

## <a name="database-examples"></a>Datenbankbeispiele
Die Datei [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) im Projekt [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) zeigt, wie die folgenden Aufgaben ausgeführt werden.

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client.CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Abfragen eines Kontos nach einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client.QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Lesen einer Datenbank nach ID](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client.ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Auflisten von Datenbanken für ein Konto](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client.ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Löschen einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client.DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |

## <a name="collection-examples"></a>Sammlungsbeispiele
Die Datei [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) im Projekt [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) zeigt, wie die folgenden Aufgaben ausgeführt werden.

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Sammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client.CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Lesen einer Liste aller Sammlungen in einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) |[document_client.ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Abrufen einer Auflistung nach ID](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client.ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Abrufen der Leistungsebene einer Sammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) |[DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Ändern der Leistungsebene einer Sammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) |[document_client.ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Löschen einer Sammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client.DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |




<!--HONumber=Nov16_HO3-->


