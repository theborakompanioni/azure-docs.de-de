---
title: 'Azure Cosmos DB: Erstellen einer Web-App mit .NET und der DocumentDB-API | Microsoft-Dokumentation'
description: "Hier finden Sie ein .NET-Codebeispiel, das Sie zum Herstellen einer Verbindung mit und zum Abfragen der Azure Cosmos DB DocumentDB-API verwenden können."
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
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: d17f90d5ed5440dc336d1e3ae890a13077e33c4d
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
<a id="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-the-azure-portal" class="xliff"></a>

# Azure Cosmos DB: Erstellen einer Web-App mit einer DocumentDB-API mit .NET und dem Azure-Portal

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

In diesem Schnellstart wird veranschaulicht, wie Sie ein Azure Cosmos DB-Konto, eine Dokumentendatenbank und eine Sammlung mithilfe des Azure-Portals erstellen. Sie erstellen dann eine Web-App für To-Do-Listen basierend auf der [DocumentDB .NET-API](documentdb-sdk-dotnet.md), wie im folgenden Screenshot gezeigt, und stellen diese anschließend bereit. 

![To-Do-App mit Beispieldaten](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
<a id="create-a-database-account" class="xliff"></a>

## Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
<a id="add-a-collection" class="xliff"></a>

## Hinzufügen einer Sammlung

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
<a id="add-sample-data" class="xliff"></a>

## Hinzufügen von Beispieldaten

Sie können nun mithilfe des Daten-Explorers Daten zu einer neuen Sammlung hinzufügen.

1. Im Daten-Explorer wird die neue Datenbank im Bereich „Sammlungen“ angezeigt. Erweitern Sie die Datenbank **Aufgaben** und die Sammlung **Elemente**, und klicken Sie auf **Dokumente** und anschließend auf **Neue Dokumente**. 

   ![Neue Dokumente im Daten-Explorer im Azure-Portal erstellen](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Fügen Sie jetzt einige Dokumente zu der Sammlung mit der folgenden Struktur hinzu, in dem Sie eindeutige Werte für die ID jedes Dokuments einfügen und die anderen Eigenschaften nach Bedarf ändern. Ihre neuen Dokumente können jede gewünschte Struktur aufweisen, da Azure Cosmos DB kein Schema für Ihre Daten vorgibt.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Nachdem Sie den JSON-Code auf der Registerkarte **Dokumente** hinzugefügt haben, klicken Sie auf **Speichern**.

    ![Kopieren Sie JSON-Daten, und klicken Sie im Azure-Portal im Daten-Explorer auf „Speichern“.](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

     Sie können jetzt zum Abrufen Ihrer Daten Abfragen im Daten-Explorer verwenden. Standardmäßig verwendet der Daten-Explorer `SELECT * FROM c` zum Abrufen aller Dokumente in der Sammlung, aber Sie können das zu `SELECT * FROM c ORDER BY c.name ASC` ändern, um alle Dokumente in alphabetischer aufsteigender Reihenfolge der Eigenschaft Name zurückzugeben. 
 
     Der Daten-Explorer kann auch zum Erstellen von gespeicherten Prozeduren, UDFs und Triggern verwendet werden, um sowohl serverseitige Geschäftslogik als auch einen Skalierungsdurchsatz auszuführen. Der Daten-Explorer stellt den gesamten integrierten programmgesteuerten Datenzugriff in den APIs zur Verfügung, bietet jedoch einfachen Zugriff auf Ihre Daten im Azure-Portal.

<a id="clone-the-sample-application" class="xliff"></a>

## Klonen der Beispielanwendung

Klonen Sie nun eine DocumentDB-API-App aus GitHub, legen die Verbindungszeichenfolge fest und führen sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `CD` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Öffnen Sie anschließend die Projektmappendatei für die Aufgabenliste in Visual Studio. 

<a id="review-the-code" class="xliff"></a>

## Überprüfen des Codes

Es folgt ein kurzer Überblick zu den Ereignissen in der App. Öffnen Sie die Datei „DocumentDBRepository.cs“. Sie stellen fest, dass mit diesen Codezeilen die Azure Cosmos DB-Ressourcen erstellt werden. 

* Der DocumentClient wird in Zeile 73 initialisiert.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Eine neue Datenbank wird in Zeile 88 erstellt.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Eine neue Sammlung wird in Zeile 107 erstellt.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

<a id="update-your-connection-string" class="xliff"></a>

## Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Schlüssel**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms kopieren Sie im nächsten Schritt die URI und den Primärschlüssel in die Datei „web.config“.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf dem Blatt „Schlüssel“](./media/create-documentdb-dotnet/keys.png)

2. Öffnen Sie in Visual Studio 2017 die Datei „web.config“. 

3. Kopieren Sie den URI-Wert aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in „web.config“ als Wert des Endpunktschlüssels fest. 

    `<add key="endpoint" value="FILLME" />`

4. Kopieren Sie anschließend den Wert für PRIMARY KEY aus dem Portal, und legen Sie ihn in „web.config“ als Wert des authKey fest. Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

    `<add key="authKey" value="FILLME" />`
    
<a id="run-the-web-app" class="xliff"></a>

## Ausführen der Web-App
1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. 

2. Geben Sie im NuGet-Feld **Durchsuchen** den Suchbegriff *DocumentDB* ein.

3. Installieren Sie die in den Ergebnissen enthaltene Bibliothek **Microsoft.Azure.DocumentDB**. Dadurch wird das Paket „Microsoft.Azure.DocumentDB“ mit all seinen Abhängigkeiten installiert.

4. Drücken Sie STRG + F5, um die Anwendung auszuführen. Ihre App wird im Browser angezeigt. 

5. Klicken Sie im Browser auf **Neu erstellen**, und erstellen Sie einige neue Aufgaben in Ihrer To-Do-App.

   ![To-Do-App mit Beispieldaten](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

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



