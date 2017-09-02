---
title: Erstellen einer Azure Cosmos DB-.NET-Anwendung mit der Table-API | Microsoft-Dokumentation
description: Erste Schritte mit der Table-API von Cosmos DB mithilfe von .NET
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/22/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: 29e7eebda5177d6e852ef04ad82d9d38a8d30ed8
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: Erstellen einer .NET-Anwendung mit der Table-API

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen. 

Dieser Schnellstart veranschaulicht, wie Sie ein Azure Cosmos DB-Konto und eine Tabelle in diesem Konto mithilfe des Azure-Portals erstellen können. Dann schreiben Sie Code, um Entitäten einzufügen, zu aktualisieren und zu löschen, und führen mit dem neuen Paket [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget) (Vorschauversion) von NuGet einige Abfragen aus. Diese Bibliothek enthält dieselben Klassen- und Methodensignaturen wie das öffentliche [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), kann aber auch eine Verbindung mit Konten von Azure Cosmos DB mithilfe der [Table-API](table-introduction.md) (Vorschauversion) herstellen. 

## <a name="prerequisites"></a>Voraussetzungen

Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Hinzufügen einer Tabelle

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Hinzufügen von Beispieldaten

Sie können nun mithilfe des Daten-Explorers (Vorschauversion) Daten zu einer neuen Tabelle hinzufügen.

1. Erweitern Sie im Daten-Explorer **sample-database**, klicken Sie auf **Entitäten**, und klicken Sie anschließend auf **Entität hinzufügen**.

   ![Erstellen neuer Entitäten im Daten-Explorer im Azure-Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Geben Sie nun Daten in das Wertfeld „PartitionKey“ und „RowKey“ ein, und klicken Sie auf **Entität hinzufügen**.

   ![Festlegen des Partitions- und Zeilenschlüssels für eine neue Entität](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Jetzt können Sie weitere Entitäten in Ihre Tabelle hinzufügen, diese Entitäten bearbeiten und Ihre Daten im Daten-Explorer abfragen. Im Daten-Explorer können Sie zudem Ihren Durchsatz skalieren und der Tabelle gespeicherte Prozeduren, benutzerdefinierte Funktionen und Trigger hinzufügen.

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie jetzt eine Tabellen-App aus GitHub, legen Sie die Verbindungszeichenfolge fest und führen Sie diese aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `cd` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Öffnen Sie anschließend die Projektmappendatei in Visual Studio. 

## <a name="review-the-code"></a>Überprüfen des Codes

Es folgt ein kurzer Überblick zu dem, was in der App geschieht. Öffnen Sie die Datei „Program.cs“. Sie stellen fest, dass mit diesen Codezeilen die Azure Cosmos DB-Ressourcen erstellt werden. 

* CloudTableClient wird initialisiert.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Es wird eine neue Tabelle erstellt, wenn keine vorhanden ist.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Es wird ein neuer Tabellencontainer erstellt. Beachten Sie, dass dieser Code dem regulären Azure Table Storage-SDK sehr ähnlich ist. 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Jetzt aktualisieren wir die Informationen zur Verbindungszeichenfolge, damit Ihre App mit Azure Cosmos DB kommunizieren kann. 

1. Öffnen Sie in Visual Studio die Datei „app.config“. 

2. Klicken Sie im [Azure-Portal](http://portal.azure.com/) im linken Azure Cosmos DB-Navigationsmenü auf **Verbindungszeichenfolge**. Klicken Sie dann im neuen Bereich für die Verbindungszeichenfolge auf die Schaltfläche „kopieren“. 

    ![Zeigen Sie im Bereich „Verbindungszeichenfolge“ den Endpunkt- und Kontoschlüssel an, und kopieren Sie ihn.](./media/create-table-dotnet/keys.png)

3. Fügen Sie den Wert in der Datei „app.config“ als Wert für „PremiumStorageConnectionString“ ein. 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    „StandardStorageConnectionString“ müssen Sie nicht verändern.

Sie haben die App nun mit allen erforderlichen Informationen für die Kommunikation mit Azure Cosmos DB aktualisiert. 

## <a name="run-the-web-app"></a>Ausführen der Web-App

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **PremiumTableGetStarted** im **Projektmappen-Explorer**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. 

2. Geben Sie im NuGet-Feld **Durchsuchen** *WindowsAzure.Storage PremiumTable* ein.

3. Aktivieren Sie das Kontrollkästchen **Vorabversion einbeziehen**. 

4. Installieren Sie die Bibliothek **WindowsAzure.Storage-PremiumTable** aus den Ergebnissen. Dadurch werden das Vorschaupaket der Azure Cosmos DB Table-API sowie dessen Abhängigkeiten installiert. Beachten Sie, dass es sich hierbei um ein anderes NuGet-Paket als das Windows Azure Storage-Paket handelt, das von Azure Table Storage verwendet wird. 

5. Drücken Sie STRG + F5, um die Anwendung auszuführen.

    Das Konsolenfenster zeigt die Daten an, die in der Tabelle hinzugefügt, abgerufen, abgefragt, ersetzt und gelöscht werden. Drücken Sie bei Abschluss des Skripts zwei Mal eine beliebige Taste, um das Konsolenfenster zu schließen. 
    
    ![Konsolenausgabe des Schnellstarts](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. Wenn Sie neue Entitäten im Daten-Explorer anzeigen möchten, kommentieren Sie in der Datei „program.cs“ einfach die Zeilen 188–208 aus, sodass sie nicht gelöscht werden, und führen Sie dann das Beispiel erneut aus. 

    Kehren Sie jetzt zurück in den Daten-Explorer, klicken Sie auf **Aktualisieren**, erweitern Sie die Tabelle **Personen**, klicken Sie auf **Entitäten**, und arbeiten Sie dann mit diesen neuen Daten. 

    ![Neue Entitäten im Daten-Explorer](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese App nicht weiter verwenden möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen im Azure-Portal. Führen Sie dazu folgende Schritte durch: 

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, eine Tabelle mit dem Daten-Explorer erstellen und eine App ausführen.  Jetzt können Sie Ihre Daten mit der Table-API abfragen.  

> [!div class="nextstepaction"]
> [Abfragen mithilfe der Table-API](tutorial-query-table.md)


