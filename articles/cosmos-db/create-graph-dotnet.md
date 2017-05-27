---
title: Erstellen einer Azure Cosmos DB-.NET-Anwendung mit der Graph-API | Microsoft-Dokumentation
description: "Hier finden Sie ein .NET-Codebeispiel, das Sie zum Herstellen einer Verbindung mit und zum Abfragen von Azure Cosmos DB verwenden können."
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmosdb
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/21/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 597b12b98edb07e453fc3cd5a0884c584a5bfd14
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="azure-cosmos-db-build-a-net-application-using-the-graph-api"></a>Azure Cosmos DB: Erstellen einer .NET-Anwendung mit der Graph-API

Azure Cosmos DB ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB zugrunde liegen. 

In diesem Schnellstart wird veranschaulicht, wie Sie Azure Cosmos DB-Konto, -Datenbank und -Graph (Container) mithilfe des Azure-Portals erstellen. Anschließend erstellen und führen Sie eine Konsolen-App aus, die auf der [Graph-API](graph-sdk-dotnet.md) (Vorschauversion) basiert.  

## <a name="prerequisites"></a>Voraussetzungen

Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Hinzufügen eines Graphs

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie jetzt eine Graph-API-App aus GitHub, legen Sie die Verbindungszeichenfolge fest und führen Sie diese aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `cd` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Öffnen Sie anschließend die Projektmappendatei in Visual Studio. 

## <a name="review-the-code"></a>Überprüfen des Codes

Es folgt ein kurzer Überblick zu dem, was in der App geschieht. Öffnen Sie die Datei „Program.cs“. Sie stellen fest, dass mit diesen Codezeilen die Azure Cosmos DB-Ressourcen erstellt werden. 

* Der DocumentClient wird initialisiert. In der Vorschauversion wurde eine Graph-Erweiterungs-API für den DocumentDB-Client hinzugefügt. Wir arbeiten mit einem eigenständigen Graph-Client, der von DocumentDB-Client und -Ressourcen entkoppelt ist.

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* Es wird eine neue Datenbank erstellt.

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* Es wird ein neuer Graph erstellt.

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* Eine Reihe von Gremlin-Schritten wird mit mithilfe der `CreateGremlinQuery`-Methode ausgeführt.

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Schlüssel**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms kopieren Sie im nächsten Schritt den URI und den Primärschlüssel in die Datei `App.config`.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal, Blatt „Schlüssel“](./media/create-graph-dotnet/keys.png)

2. Öffnen Sie in Visual Studio 2017 die Datei `App.config`. 

3. Kopieren Sie den URI-Wert aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in `App.config` als Wert des Endpunktschlüssels fest. 

    `<add key="Endpoint" value="FILLME.documents.azure.com:443" />`

4. Kopieren Sie anschließend den Wert für PRIMARY KEY aus dem Portal, und legen Sie ihn in `App.config` als Wert des authKey fest. 

    `<add key="AuthKey" value="FILLME" />`

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

## <a name="run-the-console-app"></a>Ausführen der Konsolenanwendung

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **GraphGetStarted** im **Projektmappen-Explorer**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. 

2. Geben Sie im NuGet-Feld **Browse** (Durchsuchen) *Microsoft.Azure.Graphs* ein, und aktivieren Sie das Kontrollkästchen **Includes prerelease** (Einschließlich Vorabversion). 

3. Installieren Sie die in den Ergebnissen enthaltene Bibliothek **Microsoft.Azure.Graphs**. Dadurch wird das Azure Cosmos DB-Graph-Bibliothekerweiterungspaket mit sämtlichen Abhängigkeiten installiert.

4. Drücken Sie STRG+F5, um die Anwendung auszuführen.

   Im Konsolenfenster werden die Scheitelpunkte und Ränder angezeigt, die dem Graph hinzugefügt werden. Drücken Sie bei Abschluss des Skripts zwei Mal die EINGABETASTE, um das Konsolenfenster zu schließen. 

## <a name="browse-using-the-data-explorer"></a>Durchsuchen mit dem Daten-Explorer

Sie können nun zum Daten-Explorer im Azure-Portal zurückkehren und die neuen Graph-Daten durchsuchen und abfragen.

* Im Daten-Explorer wird die neue Datenbank im Bereich „Sammlungen“ angezeigt. Erweitern Sie **graphdb**, **graphcoll**, und klicken Sie anschließend auf **Graph**.

    Die von der Beispiel-App generierten Daten werden im Graphen-Bereich angezeigt.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese App nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen im Azure-Portal. Führen Sie dazu folgende Schritte durch: 

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, einen Graph mit dem Daten-Explorer erstellen und eine App ausführen. Nun können Sie komplexere Abfragen erstellen und leistungsfähige Logik zum Traversieren von Diagrammen mit Gremlin implementieren. 

> [!div class="nextstepaction"]
> [Query using Gremlin (Abfragen mithilfe von Gremlin)](tutorial-query-graph.md)


