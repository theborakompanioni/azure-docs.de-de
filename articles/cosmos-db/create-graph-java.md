---
title: Erstellen einer Azure Cosmos DB-Java-Anwendung mit der Graph-API | Microsoft-Dokumentation
description: "Hier finden Sie ein Java-Codebeispiel, das Sie zum Herstellen einer Verbindung mit und zum Abfragen von Diagrammdaten in Azure Cosmos DB mit Gremlin verwenden können."
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
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8279ffc8dc69f0899ad7b5d3a528393fc2165b77
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-build-a-java-application-using-the-graph-api"></a>Azure Cosmos DB: Erstellen einer Java-Anwendung mit der Graph-API

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

Dieser Schnellstart veranschaulicht, wie Sie ein Azure Cosmos DB-Konto für die Graph-API (Vorschau), eine Datenbank und einen Graph mithilfe des Azure-Portals erstellen können. Anschließend erstellen Sie eine Konsolen-App mithilfe des Open-Source-Software-Treibers (OSS) [Gremlin Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) und führen diese aus.  

## <a name="prerequisites"></a>Voraussetzungen

* Bevor Sie dieses Beispiel ausführen können, müssen folgende Voraussetzungen erfüllt sein:
   * JDK 1.7 und höher (Führen Sie `apt-get install default-jdk` aus, wenn Sie nicht über JDK verfügen)
   * Maven (Führen Sie `apt-get install maven` aus, wenn Sie nicht über Maven verfügen)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Hinzufügen eines Graphs

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Nun klonen wir eine Graph-API-App (Vorschau) aus Github, legen die Verbindungszeichenfolge fest und führen sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `cd` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Es folgt ein kurzer Überblick zu den Ereignissen in der App. Öffnen Sie die Datei `Program.java`. Dort finden Sie folgende Codezeilen. 

* Gremlin `Client` wird aus der Konfiguration in `src/remote-secure.yaml` initialisiert, die Sie zuvor festgelegt haben.

    ```java
    Cluster cluster = Cluster.build(new File("src/remote.yaml")).create();
    
    Client client = cluster.connect();
    ```

* Es werden einige Schritte mit Gremlin mithilfe der `client.submit`-Methode ausgeführt.

    ```java
    ResultSet results = client.submit("g.V()");

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```
## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Schlüssel**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms kopieren Sie im nächsten Schritt die URI und den Primärschlüssel in die Datei `Program.java`.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf dem Blatt „Schlüssel“](./media/create-documentdb-dotnet/keys.png)

2. Öffnen Sie die Datei `src/remote-secure.yaml`. 

3. Füllen Sie die Konfigurationen *Host*, *Port*, *Benutzername*, *Kennwort*, *connectionPool* und *Serialisierungsprogramm* in der Datei `src/remote-secure.yaml` aus:

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Host|***.graphs.azure.com|Ihr URI des Graph-Dienst, den Sie aus dem Azure-Portal abrufen können
    Port|443|Auf 443 festlegen
    Benutzername|*Ihr Benutzername*|Die Ressource in der Form `/dbs/<db>/colls/<coll>`.
    Kennwort|*Ihr primärer Hauptschlüssel*|Ihr primärer Hauptschlüssel für Azure Cosmos DB
    ConnectionPool|{enableSsl: true}|Die Verbindungspooleinstellung für SSL
    serializer|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Auf diesen Wert festlegen

## <a name="run-the-console-app"></a>Ausführen der Konsolenanwendung

1. Führen Sie `mvn package` in einem Terminal aus, um erforderliche NPM-Module zu installieren

2. Führen Sie `mvn exec:java -D exec.mainClass=GetStarted.Program` in einem Terminal aus, um Ihre Java-Anwendung zu starten.

Jetzt können Sie zum Daten-Explorer zurückkehren, um diese neue Daten anzuzeigen, abzufragen, anzupassen und mit ihnen zu arbeiten. 

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


