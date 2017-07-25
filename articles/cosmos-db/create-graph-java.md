---
title: Erstellen einer Azure Cosmos DB-Java-Anwendung mit der Graph-API | Microsoft-Dokumentation
description: "Hier finden Sie ein Java-Codebeispiel, das Sie zum Herstellen einer Verbindung mit und zum Abfragen von Diagrammdaten in Azure Cosmos DB mit Gremlin verwenden können."
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 8eac406c6ef96d7ae8dd5f4931c7d16edb723be8
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---
# <a name="azure-cosmos-db-build-a-java-application-using-the-graph-api"></a>Azure Cosmos DB: Erstellen einer Java-Anwendung mit der Graph-API

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

Dieser Schnellstart veranschaulicht, wie Sie ein Azure Cosmos DB-Konto für die Graph-API (Vorschau), eine Datenbank und einen Graph mithilfe des Azure-Portals erstellen können. Anschließend erstellen Sie eine Konsolen-App mithilfe des Open-Source-Software-Treibers (OSS) [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) und führen diese aus.  

## <a name="prerequisites"></a>Voraussetzungen

* Bevor Sie dieses Beispiel ausführen können, müssen folgende Voraussetzungen erfüllt sein:
   * JDK 1.7 und höher (Führen Sie `apt-get install default-jdk` aus, wenn Sie nicht über JDK verfügen.) Legen Sie zudem Umgebungsvariablen wie `JAVA_HOME` fest.
   * Maven (Führen Sie `apt-get install maven` aus, wenn Sie nicht über Maven verfügen)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Hinzufügen eines Graphs

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Nun klonen wir eine Graph-API-App (Vorschau) aus Github, legen die Verbindungszeichenfolge fest und führen sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `cd` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Es folgt ein kurzer Überblick zu den Ereignissen in der App. Öffnen Sie die Datei `Program.java`. Dort finden Sie folgende Codezeilen. 

* `Client` von Gremlin wird aus der Konfiguration in `src/remote.yaml` initialisiert.

    ```java
    Cluster cluster = Cluster.build(new File("src/remote.yaml")).create();
    
    Client client = cluster.connect();
    ```

* Eine Reihe von Gremlin-Schritten wird mit mithilfe der `client.submit`-Methode ausgeführt.

    ```java
    ResultSet results = client.submit("g.V()");

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```
## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

1. Öffnen Sie die Datei „src/remote.yaml“. 

3. Füllen Sie die Konfigurationen für *host*, *port*, *username*, *password*, *connectionPool* und *serializer* in der Datei „src/remote.yaml“ aus:

    Einstellung|Empfohlener Wert|Beschreibung
    ---|---|---
    Hosts|[***.graphs.azure.com]|Siehe Screenshot unten. Dies ist der Wert für den Gremlin-URI auf der Seite „Übersicht“ des Azure-Portals (in Winkelklammern mit entferntem Zusatz „:443/“).<br><br>Sie können diesen Wert auch über die Registerkarte „Schlüssel“ abrufen, indem Sie den URI-Wert verwenden und „https://“ entfernen, „documents“ in „graphs“ ändern und den Zusatz „:443/“ entfernen.
    Port|443|Legen Sie den Wert 443 fest.
    Username|*Ihr Benutzername*|Die Ressource im Format `/dbs/<db>/colls/<coll>`, wobei `<db>` der Datenbankname und `<coll>` der Sammlungsname ist.
    Password|*Ihr primärer Hauptschlüssel*|Siehe zweiten Screenshot unten. Dies ist Ihr Primärschlüssel, den Sie von der Seite „Schlüssel“ des Azure-Portals im Feld „Primärschlüssel“ abrufen können. Verwenden Sie die Schaltfläche „Kopieren“ links vom Feld, um den Wert zu kopieren.
    ConnectionPool|{enableSsl: true}|Ihre Verbindungspooleinstellung für SSL.
    serializer|{ className: org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Legen Sie diesen Wert fest, und löschen Sie alle `\n`-Zeilenumbrüche, wenn Sie den Wert einfügen.

    Kopieren Sie für den Hosts-Wert den Wert des **Gremlin-URI** von der Seite **Übersicht**: ![Anzeigen und Kopieren des Gremlin-URI-Werts von der Seite „Übersicht“ im Azure-Portal](./media/create-graph-java/gremlin-uri.png)

    Kopieren Sie für den Password-Wert den **Primärschlüssel** von der Seite **Schlüssel**: ![Anzeigen und Kopieren des Primärschlüssels im Azure-Portal (Seite „Schlüssel“)](./media/create-graph-java/keys.png)

## <a name="run-the-console-app"></a>Ausführen der Konsolenanwendung

1. Führen Sie `mvn package` in einem Terminal aus, um erforderliche Java-Pakete zu installieren.

2. Führen Sie `mvn exec:java -D exec.mainClass=GetStarted.Program` in einem Terminal aus, um Ihre Java-Anwendung zu starten.

Jetzt können Sie zum Daten-Explorer zurückkehren, um diese neue Daten anzuzeigen, abzufragen, anzupassen und mit ihnen zu arbeiten. 

## <a name="browse-using-the-data-explorer"></a>Durchsuchen mit dem Daten-Explorer

Sie können nun zum Daten-Explorer im Azure-Portal zurückkehren und die neuen Graph-Daten durchsuchen und abfragen.

* Im Daten-Explorer wird die neue Datenbank im Bereich „Sammlungen“ angezeigt. Erweitern Sie **graphdb**, **graphcoll**, und klicken Sie anschließend auf **Graph**.

    Die von der Beispiel-App generierten Daten werden im Graphen-Bereich angezeigt.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese App nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen im Azure-Portal. Führen Sie dazu folgende Schritte durch: 

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, einen Graph mit dem Daten-Explorer erstellen und eine App ausführen. Nun können Sie komplexere Abfragen erstellen und leistungsfähige Logik zum Traversieren von Diagrammen mit Gremlin implementieren. 

> [!div class="nextstepaction"]
> [Query using Gremlin (Abfragen mithilfe von Gremlin)](tutorial-query-graph.md)


