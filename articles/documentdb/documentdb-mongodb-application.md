---
title: "Verwenden der Azure Cosmos DB-API für MongoDB zum Erstellen einer Web-App | Microsoft-Dokumentation"
description: "Ein Azure Cosmos DB-Tutorial, das mithilfe der API für MongoDB eine Onlinedatenbank-Web-App erstellt."
keywords: MongoDB-Beispiele
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/28/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 3242e3382c5920e69db5ecdbfc2462456aea94bd
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a>Azure Cosmos DB: Herstellen einer Verbindung mit einer MongoDB-App mittels .NET

Dieses Beispiel zeigt Ihnen, wie Sie mithilfe von .NET eine API für MongoDB-Web-App erstellen.

Um dieses Beispiel zu verwenden, müssen Sie Folgendes tun:

* [Erstellen](documentdb-create-mongodb-account.md) einer Azure Cosmos DB-Datenbank.
* Rufen Sie die Informationen zur MongoDB-[Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) auf.

Sie können das Tutorial [Erstellen einer Web-App in Azure, die eine Verbindung mit einer auf einem virtuellen Computer ausgeführten MongoDB herstellt](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) mit minimalen Änderungen verwenden, um schnell eine MongoDB-Anwendung einzurichten (lokal oder veröffentlicht in einer Azure-Web-App), die mit einem API für MongoDB-Konto verbunden wird.  

1. Führen Sie das Tutorial mit einer Änderung durch.  Ersetzen Sie den Code „Dal.cs“ durch Folgendes:

    ```csharp   
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    using System.Security.Authentication;
   
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            //private MongoServer mongoServer = null;
            private bool disposed = false;
   
            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from the MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
   
            // Creates a Task and inserts it into the collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }
   
            private IMongoCollection<MyTask> GetTasksCollection()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10250);
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
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
                MongoClientSettings settings = new MongoClientSettings();
                settings.Server = new MongoServerAddress(host, 10250);
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
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            # region IDisposable
   
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
   
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                    }
                }
   
                this.disposed = true;
            }
   
            # endregion
        }
    }
    ```

2. Ändern Sie die folgenden Variablen in der Datei „Dal.cs“ entsprechend den Einstellungen Ihres Kontos:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Verwenden Sie die App.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie mit Ihrer Azure Cosmos DB-API für MongoDB-Konto [MongoChef ](documentdb-mongodb-mongochef.md) und [RoboMongo](documentdb-mongodb-robomongo.md) verwenden.

