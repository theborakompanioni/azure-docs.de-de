---
title: 'Azure Cosmos DB: Erstellen einer Web-App mit Java und der DocumentDB-API | Microsoft-Dokumentation'
description: "Hier finden Sie ein Java-Codebeispiel, das Sie zum Herstellen einer Verbindung mit und zum Abfragen von Azure Cosmos DB DocumentDB-API verwenden können."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 06/27/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: c178646f0ec10cb08e90c1eda544a2488782187f
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
<a id="azure-cosmos-db-build-a-documentdb-api-app-with-java-and-the-azure-portal" class="xliff"></a>

# Azure Cosmos DB: Erstellen einer Web-App von DocumentDB-API mit Java und dem Azure-Portal

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

In diesem Schnellstart wird veranschaulicht, wie Sie ein Azure Cosmos DB-Konto, eine Dokumentendatenbank und eine Sammlung mithilfe des Azure-Portals erstellen. Anschließend erstellen und führen Sie eine Konsolen-App aus, die auf der [DocumentDB Java-API](documentdb-sdk-java.md) basiert.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

* Bevor Sie dieses Beispiel ausführen können, müssen folgende Voraussetzungen erfüllt sein:
   * JDK 1.7 und höher (Führen Sie `apt-get install default-jdk` aus, wenn Sie nicht über JDK verfügen)
   * Maven (Führen Sie `apt-get install maven` aus, wenn Sie nicht über Maven verfügen)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-database-account" class="xliff"></a>

## Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="add-a-collection" class="xliff"></a>

## Hinzufügen einer Sammlung

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="clone-the-sample-application" class="xliff"></a>

## Klonen der Beispielanwendung

Klonen Sie nun eine DocumentDB-API-App aus GitHub, legen die Verbindungszeichenfolge fest und führen sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `CD` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

<a id="review-the-code" class="xliff"></a>

## Überprüfen des Codes

Es folgt ein kurzer Überblick zu dem, was in der App geschieht. Öffnen Sie die Datei `Program.java`, und suchen Sie die Codezeilen, mit denen die Azure Cosmos DB-Ressourcen erstellt werden. 

* `DocumentClient` wird initialisiert.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* Es wird eine neue Datenbank erstellt.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* Es wird eine neue Sammlung erstellt.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    // DocumentDB collections can be reserved with throughput
    // specified in request units/second. 1 RU is a normalized
    // request equivalent to the read of a 1KB document. Here we
    // create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Einige Dokumente werden erstellt.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* Eine SQL-Abfrage über JSON wird ausgeführt.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

<a id="update-your-connection-string" class="xliff"></a>

## Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Schlüssel**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms kopieren Sie im nächsten Schritt den URI und den Primärschlüssel in die Datei `Program.java`.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf dem Blatt „Schlüssel“](./media/create-documentdb-dotnet/keys.png)

2. Öffnen Sie die Datei `Program.java`. 

3. Kopieren Sie den URI-Wert aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in `Program.java` als Wert des Endpunktschlüssels für den DocumentClient-Konstruktor fest. 

    `"https://FILLME.documents.azure.com"`

4. Anschließend kopieren Sie den Wert für PRIMARY KEY aus dem Portal und ersetzen den zweiten Parameter „FILL ME“ durch den Schlüssel im DocumentClient-Konstruktor in „Program.java“. Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 
    
<a id="run-the-app" class="xliff"></a>

## Ausführen der App

1. Führen Sie `mvn package` in einem Terminal aus, um die erforderlichen Java-Pakete zu installieren.

2. Führen Sie `mvn exec:java -D exec.mainClass=GetStarted.Program` in einem Terminal aus, um Ihre Java-Anwendung zu starten.

Jetzt können Sie zum Daten-Explorer zurückkehren, um diese neue Daten anzuzeigen, abzufragen, anzupassen und mit ihnen zu arbeiten. 

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Bereinigen von Ressourcen

Wenn Sie diese App nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen im Azure-Portal. Führen Sie dazu folgende Schritte durch:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, eine Sammlung mit dem Daten-Explorer erstellen und eine Web-App ausführen. Jetzt können Sie zusätzliche Daten in Ihr Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB (Importieren von Daten in Azure Cosmos DB)](import-data.md)



