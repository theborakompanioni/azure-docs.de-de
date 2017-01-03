---
title: "DocumentDB für MongoDB – Beispiele | Microsoft Docs"
description: "Sehen Sie sich Beispiele für die DocumentDB-Protokollunterstützung für MongoDB an."
keywords: MongoDB-Beispiele
services: documentdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 8d4c3aaefee502e79dd1ea3e074937bb9a1d4593
ms.openlocfilehash: 703373c2c0090a2c6ffef3433aee3a05f0f41475


---
# <a name="documentdb-protocol-support-for-mongodb-examples"></a>Beispiele für die DocumentDB-Protokollunterstützung für MongoDB
Um diese Beispiele verwenden zu können, müssen Sie folgende Aktionen ausführen:

* [Erstellen](documentdb-create-mongodb-account.md) eines Azure DocumentDB-Kontos mit Protokollunterstützung für MongoDB.
* Abrufen der Informationen zur [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) für das DocumentDB-Konto mit Protokollunterstützung für MongoDB.

## <a name="get-started-with-a-sample-nodejs-getting-started-app"></a>Erste Schritte mit einer Beispiel-App für Node.js

1. Erstellen Sie eine *app.js*-Datei, kopieren Sie den unten stehenden Code, und fügen Sie ihn in die Datei ein.

         var MongoClient = require('mongodb').MongoClient;
         var assert = require('assert');
         var ObjectId = require('mongodb').ObjectID;
         var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10250/?ssl=true';

         var insertDocument = function(db, callback) {
            db.collection('families').insertOne( {
                 "id": "AndersenFamily",
                 "lastName": "Andersen",
                 "parents": [
                     { "firstName": "Thomas" },
                     { "firstName": "Mary Kay" }
                 ],
                 "children": [
                     { "firstName": "John", "gender": "male", "grade": 7 }
                 ],
                 "pets": [
                     { "givenName": "Fluffy" }
                 ],
                 "address": { "country": "USA", "state": "WA", "city": "Seattle" }
             }, function(err, result) {
             assert.equal(err, null);
             console.log("Inserted a document into the families collection.");
             callback();
           });
         };

         var findFamilies = function(db, callback) {
            var cursor =db.collection('families').find( );
            cursor.each(function(err, doc) {
               assert.equal(err, null);
               if (doc != null) {
                  console.dir(doc);
               } else {
                  callback();
               }
            });
         };

         var updateFamilies = function(db, callback) {
            db.collection('families').updateOne(
               { "lastName" : "Andersen" },
               {
                 $set: { "pets": [
                     { "givenName": "Fluffy" },
                     { "givenName": "Rocky"}
                 ] },
                 $currentDate: { "lastModified": true }
               }, function(err, results) {
               console.log(results);
               callback();
            });
         };

         var removeFamilies = function(db, callback) {
            db.collection('families').deleteMany(
               { "lastName": "Andersen" },
               function(err, results) {
                  console.log(results);
                  callback();
               }
            );
         };

         MongoClient.connect(url, function(err, db) {
           assert.equal(null, err);
           insertDocument(db, function() {
             findFamilies(db, function() {
               updateFamilies(db, function() {
                 removeFamilies(db, function() {
                     db.close();
                 });
               });
             });
           });
         });

2. Ändern Sie die folgenden Variablen in der *app.js*-Datei gemäß Ihren Kontoeinstellungen (informieren Sie sich darüber, wie Sie Ihre [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) finden):
   
         var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10250/?ssl=true';
     
3. Öffnen Sie Ihr bevorzugtes Terminal, führen Sie **npm install mongodb --save** aus, und führen Sie dann Ihre App mit **node app.js** aus.

## <a name="get-started-with-a-sample-aspnet-mvc-task-list-application"></a>Erste Schritte mit einer Beispielanwendung für eine ASP.NET-MVC-Aufgabenliste
Sie können das Tutorial [Erstellen einer Web-App in Azure, die eine Verbindung mit einer auf einem virtuellen Computer ausgeführten MongoDB herstellt](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) mit minimalen Änderungen verwenden, um schnell eine MongoDB-Anwendung einzurichten (lokal oder veröffentlicht in einer Azure-Web-App), die mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verbunden wird.  

1. Führen Sie das Tutorial mit einer Änderung durch.  Ersetzen Sie den Code „Dal.cs“ durch Folgendes:
   
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
2. Ändern Sie die folgenden Variablen in der Datei „Dal.cs“ entsprechend den Einstellungen Ihres Kontos:
   
         private string userName = "<your user name>";
         private string host = "<your host>";
         private string password = "<your password>";
3. Verwenden Sie die App.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie [MongoChef](documentdb-mongodb-mongochef.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verwenden.



<!--HONumber=Jan17_HO1-->


