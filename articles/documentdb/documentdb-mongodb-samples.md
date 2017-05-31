---
title: Verwenden von MongoDB-APIs zum Erstellen einer Azure Cosmos DB-App | Microsoft-Dokumentation
description: "Ein Tutorial, das mithilfe von DocumentDB-API für MongoDB eine Onlinedatenbank erstellt."
keywords: MongoDB-Beispiele
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: d4243fc1400c4e9ad545060f253ae46a579e406f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Erstellen von Azure Cosmos DB: API für MongoDB-App mithilfe von Node.js
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.js für MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
>

Dieses Beispiel zeigt Ihnen, wie Sie mithilfe von Node.js eine Azure Cosmos DB-API für die MongoDB-Konsolen-App erstellen.

Um dieses Beispiel zu verwenden, müssen Sie Folgendes tun:

* [Erstellen](documentdb-create-mongodb-account.md) einer Azure Cosmos DB-API für MongoDB-Konto.
* Rufen Sie die Informationen zur MongoDB-[Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) auf.

## <a name="create-the-app"></a>Erstellen der App

1. Erstellen Sie eine *app.js*-Datei, kopieren Sie den unten stehenden Code, und fügen Sie ihn in die Datei ein.

    ```nodejs
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
    ```

2. Ändern Sie die folgenden Variablen in der *app.js*-Datei gemäß Ihren Kontoeinstellungen (informieren Sie sich darüber, wie Sie Ihre [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) finden):
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10250/?ssl=true';
    ```
     
3. Öffnen Sie Ihr bevorzugtes Terminal, führen Sie **npm install mongodb --save** aus, und führen Sie dann Ihre App mit **node app.js** aus.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie Azure Cosmos DB mit [MongoChef verwenden](documentdb-mongodb-mongochef.md): API für MongoDB-Konto.

