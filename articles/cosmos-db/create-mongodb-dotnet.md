---
title: 'Azure Cosmos DB: Erstellen einer Web-App mit .NET und der MongoDB-API | Microsoft-Dokumentation'
description: "Hier finden Sie ein .NET-Codebeispiel, das Sie zum Verbinden mit und zum Abfragen von Azure Cosmos DB MongoDB-API verwenden können."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: b95afa4dd3cd264ea911c7aa809c7862f9576da8
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-the-azure-portal"></a>Azure Cosmos DB: Erstellen einer Web-App mit einer MongoDB-API mit .NET und dem Azure-Portal

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

In diesem Schnellstart wird veranschaulicht, wie Sie ein Azure Cosmos DB-Konto, eine Dokumentendatenbank und eine Sammlung mithilfe des Azure-Portals erstellen. Anschließend erstellen Sie eine Web-App für Aufgabenlisten mit dem [.NET-Treiber von MongoDB](https://docs.mongodb.com/ecosystem/drivers/csharp/) und stellen diese bereit. 

## <a name="prerequisites"></a>Voraussetzungen

Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie jetzt eine MongoDB-API-App aus GitHub, legen Sie die Verbindungszeichenfolge fest, und führen Sie diese aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `cd` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. Öffnen Sie anschließend die Projektmappendatei in Visual Studio. 

## <a name="review-the-code"></a>Überprüfen des Codes

Es folgt ein kurzer Überblick zu den Ereignissen in der App. Öffnen Sie die Datei **dal.cs** im Verzeichnis **DAL**. Sie stellen fest, dass mit diesen Codezeilen die Azure Cosmos DB-Ressourcen erstellt werden. 

* Initialisieren Sie den Mongo-Client.

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credentials = new List<MongoCredential>()
        {
            new MongoCredential("SCRAM-SHA-1", identity, evidence)
        };

        MongoClient client = new MongoClient(settings);
    ```

* Rufen Sie die Datenbank und die Sammlung ab.

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* Rufen Sie alle Dokumente ab.

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Verbindungszeichenfolge**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Kopieren Sie im nächsten Schritt mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms den Benutzernamen, das Kennwort sowie den Host in die Datei „dal.cs“.

2. Öffnen Sie die Datei **dal.cs** im Verzeichnis **DAL**. 

3. Kopieren Sie den Wert des **Benutzernamens** aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in der Datei **dal.cs** als Wert des **Benutzernamens** fest. 

4. Kopieren Sie anschließend den Wert des **Hosts** aus dem Portal, und legen Sie ihn in der Datei **dal.cs** als Wert des **Hosts** fest. 

5. Kopieren Sie anschließend den Wert des **Kennworts** aus dem Portal, und legen Sie ihn in der Datei **dal.cs** als Wert des **Kennworts** fest. 

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 
    
## <a name="run-the-web-app"></a>Ausführen der Web-App

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. 

2. Geben Sie im NuGet-Feld **Durchsuchen** den Suchbegriff *MongoDB.Driver* ein.

3. Installieren Sie die Bibliothek **MongoDB.Driver** mithilfe der Ergebnisse. Dadurch wird das Paket „MongoDB.Driver“ mit all seinen Abhängigkeiten installiert.

4. Drücken Sie STRG + F5, um die Anwendung auszuführen. Ihre App wird im Browser angezeigt. 

5. Klicken Sie im Browser auf **Erstellen**, und erstellen Sie einige Aufgaben in Ihrer Aufgabenlisten-App.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese App nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen im Azure-Portal. Führen Sie dazu folgende Schritte durch:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen und eine App mithilfe der API für MongoDB ausführen. Jetzt können Sie zusätzliche Daten in Ihr Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Importieren von Daten in Azure Cosmos DB für die MongoDB-API](mongodb-migrate.md)


