---
title: "Wie werden Abfragen von Diagrammdaten in Azure Cosmos DB durchgeführt? | Microsoft Docs"
description: "Erfahren Sie, wie Sie Abfragen von Diagrammdaten in Azure Cosmos DB durchführen."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 630aeba6a881c814a6afb030ad2c064055fc452f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="azure-cosmos-db-how-to-query-with-the-graph-api-preview"></a>Azure Cosmos DB: Wie werden Abfragen mit der Graph-API (Vorschau) durchgeführt?

Die [Graph-API](graph-introduction.md) (Vorschau) von Azure Cosmos DB unterstützt [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/)-Abfragen. Dieser Artikel enthält Beispieldokumente und Abfragen, um Ihnen den Einstieg zu erleichtern. Eine detaillierte Gremlin-Referenz finden Sie im Artikel [Azure Cosmos DB Gremlin graph support](gremlin-support.md) (Azure Cosmos DB: Gremlin-Graphunterstützung).

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Abfragen von Daten mit Gremlin

## <a name="prerequisites"></a>Voraussetzungen

Diese Abfragen können nur funktionieren, wenn Sie über ein Azure Cosmos DB-Konto verfügen und Diagrammdaten im Container vorliegen. Sie haben beides nicht? Absolvieren Sie den [5-Minuten-Schnellstart](create-graph-dotnet.md) oder das [Entwicklertutorial](tutorial-query-graph.md) zum Erstellen eines Kontos und Füllen Ihrer Datenbank. Sie können die folgenden Abfragen mit der [Azure Cosmos DB .NET-Graphbibliothek](graph-sdk-dotnet.md), der [Gremlin-Konsole](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) oder Ihrem bevorzugten Gremlin-Treiber durchführen.

## <a name="count-vertices-in-the-graph"></a>Anzahl der Scheitelpunkte im Diagramm

Der folgende Codeausschnitt zeigt, wie Sie die Anzahl der Scheitelpunkte im Diagramm zählen:

```
g.V().count()
```

## <a name="filters"></a>Filter

Filter können Sie in den Gremlin-Schritten `has` und `hasLabel` anwenden und sie mithilfe von `and`, `or`, und `not` kombinieren, um komplexere Filter zu erstellen. Azure Cosmos DB bietet schemaunabhängige Indizierung aller Eigenschaften in den Scheitelpunkten und Graden für schnelle Abfragen:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Projektion

Sie können bestimmte Eigenschaften in den Abfrageergebnissen mit dem `values`-Schritt projizieren:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Suchen verwandter Kanten und Scheitelpunkte

Bisher haben wir nur Abfrageoperatoren gesehen, die in jeder Datenbank funktionieren. Diagramme sind schnell und effizient bei Traversierungen, wenn Sie zu verwandten Kanten und Scheitelpunkten navigieren müssen. Wir suchen nun alle Freunde von Thomas. Hierzu suchen wir mit Schritt `outE` von Gremlin alle Out-Edges von Thomas und traversieren dann mit dem Gremlin-Schritt `inV` zu den In-Vertices dieser Kanten:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

Die nächste Abfrage führt durch zweimaligen Aufruf von `outE` und `inV` zwei Hops aus, um alle „Freunde von Freunden“ von Thomas zu finden. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Mit Gremlin können Sie komplexere Abfragen erstellen und leistungsstarke Logik zur Graphdurchsuchung implementieren, einschließlich Mischen von Filterausdrücken, Ausführen von Schleifen mithilfe des `loop`-Schritts und Implementieren bedingter Navigation mithilfe des `choose`-Schritts. Erfahren Sie mehr darüber, was Sie mit [Gremlin-Unterstützung](gremlin-support.md) tun können!

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Sie haben gelernt, wie Sie Abfragen mithilfe von Graph durchführen. 

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Daten global verteilen.

> [!div class="nextstepaction"]
> [Globales Verteilen Ihrer Daten](tutorial-global-distribution-documentdb.md)
