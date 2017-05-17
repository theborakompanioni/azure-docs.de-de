---
title: 'Azure Cosmos DB: Entwickeln mit der Graph-API in .NET | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie mit der DocumentDB-API von Azure Cosmos DB unter Verwendung von .NET entwickeln können."
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmosdb
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 64c412fc6b47da73e5430a4db4c007e4cf95aa47
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a>Azure Cosmos DB: Entwickeln mit der Graph-API in .NET
Azure Cosmos DB ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

In diesem Tutorial wird veranschaulicht, wie Sie ein Azure Cosmos DB-Konto im Azure-Portal und wie Sie eine Diagrammdatenbank und einen Container erstellen. Die Anwendung erstellt dann mit der [Graph-API](graph-sdk-dotnet.md) (Vorschau) ein einfaches soziales Netzwerk mit vier Personen und setzt dann Gremlin ein, um das Diagramm zu traversieren und abzufragen.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen eines Azure Cosmos DB-Kontos 
> * Erstellen einer Diagrammdatenbank und eines Containers
> * Serialisieren von Scheitelpunkten und Kanten in .NET-Objekte
> * Hinzufügen von Scheitelpunkten und Kanten
> * Abfragen des Diagramms mit Gremlin

## <a name="graphs-in-azure-cosmos-db"></a>Diagramme in Azure Cosmos DB
Sie können mit Azure Cosmos DB Diagramme mithilfe der [Microsoft.Azure.Graphs](graph-sdk-dotnet.md)-Bibliothek erstellen, aktualisieren und abfragen. Die Microsoft.Azure.Graph-Bibliothek stellt eine einzelne Erweiterungsmethode `CreateGremlinQuery<T>` auf der Basis der `DocumentClient`-Klasse zum Ausführen von Gremlin-Abfragen bereit.

Gremlin ist eine funktionale Programmiersprache, die Schreibvorgänge (DML) sowie Abfrage- und Traversiervorgänge unterstützt. Wir behandeln in diesem Artikel einige Beispiele, damit Sie Gremlin kennenlernen. Unter [Azure Cosmos DB Gremlin graph support](gremlin-support.md) (Azure Cosmos DB – Gremlin-Diagrammunterstützung) finden Sie eine ausführliche exemplarische Vorgehensweise der in Azure Cosmos DB verfügbaren Gremlin-Funktionen. 

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Sie über Folgendes verfügen:

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren. 
    * Als Alternative können Sie den [Azure DocumentDB-Emulator](../documentdb/documentdb-nosql-local-emulator.md) für dieses Tutorial verwenden.
* [Visual Studio](http://www.visualstudio.com/)

## <a name="create-database-account"></a>Erstellen eines Datenbankkontos

Zunächst erstellen wir ein Azure Cosmos DB-Konto im Azure-Portal.  

> [!TIP]
> * Besitzen Sie bereits ein Azure Cosmos DB-Konto? Wenn dies der Fall ist, fahren Sie mit [Einrichten Ihrer Visual Studio-Projektmappe](#SetupVS) fort.
> * Besaßen Sie einmal ein Azure DocumentDB-Konto? Wenn dies der Fall ist, ist Ihr Konto jetzt ein Azure Cosmos DB-Konto, und Sie können mit [Einrichten Ihrer Visual Studio-Projektmappe](#SetupVS) fortfahren.  
> * Wenn Sie den Azure Cosmos DB-Emulator verwenden, führen Sie bitte die Schritte unter [Azure Cosmos DB-Emulator](../documentdb/documentdb-nosql-local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten Ihrer Visual Studio-Projektmappe](#SetupVS) fort. 
>
> 

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Einrichten Ihrer Visual Studio-Projektmappe
1. Öffnen Sie auf Ihrem Computer **Visual Studio**.
2. Wählen Sie im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.
3. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Vorlagen** / **Visual C#** / **Konsolen-App (.NET Framework)** aus, geben Sie Ihrem Projekt einen Namen, und klicken Sie dann auf **OK**.
4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die neue Konsolenanwendung, die sich unter Ihrer Visual Studio-Projektmappe befindet, und klicken Sie anschließend auf **NuGet-Pakete verwalten...**.
5. Klicken Sie auf der Registerkarte **NuGet** auf **Durchsuchen**, geben Sie **Microsoft.Azure.Graphs** in das Suchfeld ein, und aktivieren Sie **Include prerelease versions** (Vorabversionen einbeziehen).
6. Suchen Sie in den Ergebnissen nach **Microsoft.Azure.Graphs**, und klicken Sie auf **Installieren**.
   
   Wenn Sie eine Meldung im Zusammenhang mit der Überprüfung von Änderungen an der Projektmappe erhalten, klicken Sie auf **OK**. Wenn Sie eine Meldung im Zusammenhang mit der Akzeptierung der Lizenz erhalten, klicken Sie auf **Ich stimme zu**.
   
    Die `Microsoft.Azure.Graphs`-Bibliothek bietet eine einzelne Erweiterungsmethode `CreateGremlinQuery<T>` zum Ausführen von Gremlin-Vorgängen. Gremlin ist eine funktionale Programmiersprache, die Schreibvorgänge (DML) sowie Abfrage- und Traversiervorgänge unterstützt. Wir behandeln in diesem Artikel einige Beispiele, damit Sie Gremlin kennenlernen. Unter [Azure Cosmos DB Gremlin graph support](gremlin-support.md) (Azure Cosmos DB – Gremlin-Diagrammunterstützung) finden Sie eine ausführliche exemplarische Vorgehensweise der in Azure Cosmos DB verfügbaren Gremlin-Funktionen.

## <a id="add-references"></a>Herstellen einer Verbindung mit Ihrer App

Fügen Sie diese beiden Konstanten und Ihre *client*-Variable der Anwendung hinzu. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
Navigieren Sie als Nächstes wieder zum [Azure-Portal](https://portal.azure.com), um Ihre Endpunkt-URL und den primären Schlüssel abzurufen. Die Endpunkt-URL und der Primärschlüssel sind erforderlich, damit Ihre Anwendung weiß, womit die Verbindung hergestellt werden soll, und damit Azure Cosmos DB weiß, dass die Verbindung Ihrer Anwendung vertrauenswürdig ist. 

Navigieren Sie im Azure-Portal zu Ihrem Azure Cosmos DB-Konto, klicken Sie auf **Schlüssel** und anschließend auf **Lese-/Schreibschlüssel**. 

Kopieren Sie den URI vom Portal, und fügen Sie ihn über `Endpoint` in die obige Endpunkteigenschaft ein. Kopieren Sie anschließend den PRIMÄRSCHLÜSSEL aus dem Portal, und fügen Sie ihn in die obige `AuthKey`-Eigenschaft ein. 

![Screenshot des Azure-Portals, das vom Tutorial zum Erstellen einer C#-Anwendung verwendet wird. Zeigt ein Azure Cosmos DB-Konto, wobei die SCHLÜSSEL-Schaltfläche in der Azure Cosmos DB-Navigation und der URI und die PRIMÄRSCHLÜSSEL-Werte auf dem Blatt „Schlüssel“ hervorgehoben sind.][Schlüssel] 
 
## <a id="instantiate"></a>Instanziieren des DocumentClient 
Erstellen Sie als Nächstes eine neue Instanz des **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Erstellen einer Datenbank 

Als Nächstes erstellen Sie eine Azure Cosmos DB-[Datenbank](../documentdb/documentdb-resources.md#databases) mithilfe der [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx)- oder [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx)-Methode der **DocumentClient**-Klasse aus dem [DocumentDB-.NET-SDK](../documentdb/documentdb-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Erstellen eines Diagramms 

Erstellen Sie als Nächstes einen Diagrammcontainer mit der [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx)- oder [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx)-Methode der **DocumentClient**-Klasse. Eine Sammlung ist ein Container für Diagrammentitäten. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>Serialisieren von Scheitelpunkten und Kanten in .NET-Objekte
Azure Cosmos DB verwendet das [GraphSON-Gitternetzformat](gremlin-support.md), das ein JSON-Schema für Scheitelpunkte, Kanten und Eigenschaften definiert. Das Azure Cosmos DB-.NET-SDK umfasst JSON.NET als Abhängigkeit, sodass wir GraphSON in .NET-Objekte, die wir im Code verwenden können, serialisieren/deserialisieren können.

Im Beispiel arbeiten wir mit einem einfachen sozialen Netzwerk mit vier Personen. Wir behandeln das Erstellen von `Person`-Scheitelpunkten, fügen `Knows`-Beziehungen zwischen ihnen hinzu, fragen das Diagramm dann ab und traversieren es, um „Freunde von Freunden“-Beziehungen zu suchen. 

Der `Microsoft.Azure.Graphs.Elements`-Namespace stellt `Vertex`-, `Edge`-, `Property`- und `VertexProperty`-Klassen für die Deserialisierung von GraphSON-Antworten in klar definierte .NET-Objekte bereit.

## <a name="run-gremlin-using-creategremlinquery"></a>Ausführen von Gremlin mithilfe von CreateGremlinQuery
Gremlin unterstützt wie SQL Lese-, Schreib- und Abfragevorgänge. Der folgende Codeausschnitt zeigt z.B. das Erstellen von Scheitelpunkten und Kanten, das Durchführen einiger Beispielabfragen mit `CreateGremlinQuery<T>` sowie die asynchrone Iteration dieser Ergebnisse mit `ExecuteNextAsync` und HasMoreResults.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Hinzufügen von Scheitelpunkten und Kanten

Wir betrachten die im vorherigen Abschnitt vorgestellten Gremlin-Anweisungen nun im Detail. Zuerst erstellen wir einige Scheitelpunkte mit der `addV`-Methode von Gremlin. Der folgende Codeausschnitt erstellt z.B. einen Scheitelpunkt „Thomas Andersen“ des Typs „Person“ mit Eigenschaften für Vorname, Nachname und Alter.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Anschließend erstellen wir einige Kanten zwischen diesen Scheitelpunkten mit der `addE`-Methode von Gremlin. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

Wir können einen vorhandenen Scheitelpunkt mit dem Gremlin-Schritt `properties` aktualisieren. Wir überspringen den Aufruf zum Ausführen der Abfrage über `HasMoreResults` und `ExecuteNextAsync` für den Rest der Beispiele.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Sie können Kanten und Scheitelpunkte mithilfe des Gremlin-Schritts `drop` löschen. Hier ist ein Codeausschnitt, der das Löschen von Scheitelpunkten und Kanten veranschaulicht. Beachten Sie, dass mit dem Löschen eines Scheitelpunkts die zugeordneten Kanten kaskadierend gelöscht werden.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a>Abfragen des Diagramms

Sie können Abfragen und Traversierungen auch mit Gremlin ausführen. Der folgende Codeausschnitt zeigt z.B., wie Sie die Anzahl der Scheitelpunkte im Diagramm zählen:

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Mit den Gremlin-Schritten `has` und `hasLabel` können Sie Filter anwenden und mit `and`, `or`, und `not` zu komplexeren Filtern kombinieren:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

Sie können bestimmte Eigenschaften in den Abfrageergebnissen mit dem `values`-Schritt projizieren:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Bisher haben wir nur Abfrageoperatoren gesehen, die in jeder Datenbank funktionieren. Diagramme sind schnell und effizient bei Traversierungen, wenn Sie zu verwandten Kanten und Scheitelpunkten navigieren müssen. Wir suchen nun alle Freunde von Thomas. Hierzu suchen wir mit Schritt `outE` von Gremlin alle Out-Edges von Thomas und traversieren dann mit dem Gremlin-Schritt `inV` zu den In-Vertices dieser Kanten:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

Die nächste Abfrage führt durch zweimaligen Aufruf von `outE` und `inV` zwei Hops aus, um alle „Freunde von Freunden“ von Thomas zu finden. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Mit Gremlin können Sie komplexere Abfragen erstellen und leistungsstarke Logik zur Graphdurchsuchung implementieren, einschließlich Mischen von Filterausdrücken, Ausführen von Schleifen mithilfe des `loop`-Schritts und Implementieren bedingter Navigation mithilfe des `choose`-Schritts. Erfahren Sie mehr darüber, was Sie mit [Gremlin-Unterstützung](gremlin-support.md) tun können!

Das ist alles, dieses Azure Cosmos DB-Tutorial ist abgeschlossen! 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese App nicht weiterhin verwenden, löschen Sie mit den folgenden Schritten im Azure-Portal sämtliche Ressourcen, die mit diesem Tutorial erstellt wurden.  

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Erstellen eines Azure Cosmos DB-Kontos 
> * Erstellen einer Diagrammdatenbank und eines Containers
> * Serialisieren von Scheitelpunkten und Kanten in .NET-Objekte
> * Hinzufügen von Scheitelpunkten und Kanten
> * Abfragen des Diagramms mit Gremlin

Nun können Sie komplexere Abfragen erstellen und leistungsfähige Logik zum Traversieren von Diagrammen mit Gremlin implementieren. 

> [!div class="nextstepaction"]
> [Abfragen mit Gremlin](tutorial-query-graph.md)

