---
title: "Kennenlernen von Node.js – Tutorial zu Azure Cosmos DB Node.js | Microsoft-Dokumentation"
description: Kennenlernen von Node.js! Im Tutorial wird beschrieben, wie Sie mit Microsoft Azure Cosmos DB Daten aus einer in Azure-Websites gehosteten Node.js Express-Webanwendung speichern und darauf zugreifen.
keywords: Anwendungsentwicklung, Datenbanktutorial, node.js erlernen, node.js-Tutorial, DocumentDB, Azure, Microsoft Azure
services: cosmos-db
documentationcenter: nodejs
author: syamkmsft
manager: jhubbard
editor: cgronlun
ms.assetid: 9da9e63b-e76a-434e-96dd-195ce2699ef3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 05/23/2017
ms.author: syamk
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 511c9e4d6f68b3e063559acb5996111acd3c653f
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="_Toc395783175"></a>Erstellen einer Node.js-Webanwendung mithilfe von Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

In diesem Node.js-Tutorial erfahren Sie, wie Sie mithilfe von Azure Cosmos DB und der DocumentDB-API Daten aus einer in Azure Websites gehosteten Node.js Express-Anwendung speichern und abrufen. Sie erstellen eine einfache webbasierte Anwendung zur Aufgabenverwaltung (eine To-Do-Anwendung), mit der Sie Aufgaben erstellen, abrufen und durchführen können. Die Aufgaben werden als JSON-Dokumente in Azure Cosmos DB gespeichert. Dieses Lernprogramm führt Sie durch die einzelnen Schritte zur Erstellung und Bereitstellung der App und erläutert die Vorgänge in den einzelnen Ausschnitten.

![Screenshot der in diesem Node.js-Lernprogramm erstellten Anwendung "My Todo List"](./media/documentdb-nodejs-application/image1.png)

Sie haben keine Zeit, um das Lernprogramm abzuschließen, und möchten nur die vollständige Lösung abrufen? Kein Problem, Sie können die vollständige Beispiellösung von [GitHub][GitHub] abrufen. Anweisungen zum Ausführen der App finden Sie in der [Infodatei](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md).

## <a name="_Toc395783176"></a>Voraussetzungen
> [!TIP]
> In diesem Node.js-Lernprogramm wird davon ausgegangen, dass Sie bereits Erfahrung mit Node.js und Azure-Websites haben.
> 
> 

Bevor Sie diesen Artikel durcharbeiten, sollten Sie sicherstellen, dass Folgendes vorhanden ist:

* Ein aktives Azure-Konto. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/).

   OR

   Lokale Installation des [Azure Cosmos DB-Emulators](local-emulator.md)
* [Node.js][Node.js] Version v0.10.29 oder höher.
* [Express Generator](http://www.expressjs.com/starter/generator.html) (Installation über `npm install express-generator -g`)
* [Git][Git].

## <a name="_Toc395637761"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Datenbankkontos
Wir beginnen, indem wir ein Azure Cosmos DB-Konto erstellen. Falls Sie bereits ein Konto besitzen oder den Azure Cosmos DB-Emulator für dieses Tutorial verwenden, können Sie mit [Schritt 2: Erstellen einer neuen Node.js-Anwendung](#_Toc395783178) fortfahren.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>Schritt 2: Erstellen einer neuen Node.js-Anwendung
Nun erfahren Sie, wie Sie ein einfaches "Hello World"-Node.js-Projekt mithilfe des [Express](http://expressjs.com/) -Frameworks erstellen.

1. Öffnen Sie Ihr bevorzugtes Terminal (beispielsweise die Node.js-Eingabeaufforderung).
2. Navigieren Sie zu dem Verzeichnis, in dem Sie die neue Anwendung speichern möchten.
3. Verwenden Sie den Express Generator, um eine neue Anwendung namens **todo**zu erstellen.
   
        express todo
4. Öffnen Sie das neue **todo** -Verzeichnis, und installieren Sie alle Abhängigkeiten.
   
        cd todo
        npm install
5. Führen Sie die neue Anwendung aus.
   
        npm start
6. Sie können die neue Anwendung anzeigen, indem Sie in Ihrem Browser zu [http://localhost:3000](http://localhost:3000) navigieren.
   
    ![Kennenlernen von Node.js - Screenshot der "Hello World"-Anwendung in einem Browserfenster](./media/documentdb-nodejs-application/image12.png)

    Drücken Sie zum Beenden der Anwendung im Terminalfenster STRG+C, und klicken Sie dann auf **J**, um den Batchauftrag zu beenden.

## <a name="_Toc395783179"></a>Schritt 3: Installieren zusätzlicher Module
Die Datei **package.json** ist eine der im Stammverzeichnis des Projekts erstellten Dateien. Diese Datei enthält eine Liste zusätzlicher Module, die für Ihre Node.js-Anwendung erforderlich sind. Wenn Sie später diese Anwendung in Azure-Websites bereitstellen, wird anhand dieser Datei bestimmt, welche Module auf Azure installiert werden müssen, um Ihre Anwendung zu unterstützen. Für dieses Lernprogramm müssen zwei weitere Pakete installiert werden.

1. Wechseln Sie zum Terminal, und installieren Sie das **async** -Modul über npm.
   
        npm install async --save
2. Installieren Sie das **documentdb** -Modul über npm. Dieses Modul ist für die DocumentDB-Funktionen zuständig.
   
        npm install documentdb --save
3. Bei einer schnellen Überprüfung der Datei **package.json** der Anwendung können Sie die zusätzlichen Module anzeigen. Diese Datei weist Azure beim Ausführen der Anwendung an, welche Pakete heruntergeladen und installiert werden sollen. Sie sollte dem unten stehenden Beispiel ähneln.
   
        {
          "name": "todo",
          "version": "0.0.0",
          "private": true,
          "scripts": {
            "start": "node ./bin/www"
          },
          "dependencies": {
            "async": "^2.1.4",
            "body-parser": "~1.15.2",
            "cookie-parser": "~1.4.3",
            "debug": "~2.2.0",
            "documentdb": "^1.10.0",
            "express": "~4.14.0",
            "jade": "~1.11.0",
            "morgan": "~1.7.0",
            "serve-favicon": "~2.3.0"
          }
        }
   
    Dadurch wird Node (und später Azure) darüber informiert, dass Ihre Anwendung von diesen zusätzlichen Modulen abhängig ist.

## <a name="_Toc395783180"></a>Schritt 4: Verwenden des Azure Cosmos DB-Diensts in einer Knotenanwendung
Dies ist für die anfängliche Einrichtung und Konfiguration erforderlich. Jetzt möchten wir zur eigentlichen Aufgabe übergehen, also zum Erstellen von Code mithilfe von Azure Cosmos DB.

### <a name="create-the-model"></a>Erstellen des Modells
1. Erstellen Sie unter dem Projektverzeichnis in dem Verzeichnis mit der Datei „package.json“ ein neues Verzeichnis namens **models**.
2. Erstellen Sie im Verzeichnis **models** eine neue Datei namens **taskDao.js**. Diese Datei enthält das Modell für die von Ihrer Anwendung erstellten Aufgaben.
3. Erstellen Sie im gleichen Verzeichnis **models** eine weitere neue Datei namens **docdbUtils.js**. Diese Datei enthält einigen nützlichen, wiederverwendbaren Code, den wir in unserer Anwendung verwenden werden. 
4. Kopieren Sie den folgenden Code in **docdbUtils.js**
   
        var DocumentDBClient = require('documentdb').DocumentClient;
   
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
   
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
   
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
   
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };               
   
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
   
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
   
        module.exports = DocDBUtils;
   
   > [!TIP]
   > createCollection akzeptiert einen optionalen requestOptions-Parameter, der für die Festlegung des Angebotstyps für die Sammlung verwendet werden kann. Wenn kein requestOptions.offerType-Wert angegeben ist, wird die Sammlung mit den Standardangebotstyp erstellt.
   > 
   > Weitere Informationen zu Azure Cosmos DB-Angebotstypen finden Sie unter [Performance levels in Azure Cosmos DB](performance-levels.md) (Leistungsstufen in Azure Cosmos DB). 
   > 
   > 
5. Speichern und schließen Sie die Datei **docdbUtils.js** .
6. Fügen Sie am Anfang der Datei **taskDao.js** folgenden Code ein, um auf die oben erstellten **DocumentDBClient** und **docdbUtils.js** zu verweisen:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. Anschließend fügen Sie Code hinzu, um das Task-Objekt zu definieren und zu exportieren. Dieser ist verantwortlich für das Initialisieren des Task-Objekts und für das Einrichten der Datenbank und der Dokumentenauflistung, die wir verwenden werden.
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. Fügen Sie als Nächstes den folgenden Code hinzu, um zusätzliche Methoden für das Task-Objekt zu definieren, die Interaktionen mit in Azure Cosmos DB gespeicherten Daten ermöglichen.
   
        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
   
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
   
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
   
            find: function (querySpec, callback) {
                var self = this;
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results);
                    }
                });
            },
   
            addItem: function (item, callback) {
                var self = this;
   
                item.date = Date.now();
                item.completed = false;
   
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, doc);
                    }
                });
            },
   
            updateItem: function (itemId, callback) {
                var self = this;
   
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        doc.completed = true;
   
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
   
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
   
            getItem: function (itemId, callback) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };
9. Speichern und schließen Sie die Datei **taskDao.js** . 

### <a name="create-the-controller"></a>Erstellen des Controllers
1. Erstellen Sie im Verzeichnis **routes** des Projekts eine neue Datei namens **tasklist.js**. 
2. Fügen Sie **tasklist.js**den folgenden Code hinzu. Hierdurch werden die Module "DocumentDBClient" und "async" geladen, die von **tasklist.js**verwendet werden. Darüber hinaus wird hierdurch die Funktion **TaskList** definiert, an die eine Instanz des zuvor definierten **Task**-Objekts übergeben wird:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. Fügen Sie außerdem zur Datei **tasklist.js** die Methoden **showTasks, addTask** und **completeTasks** hinzu:
   
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
   
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
   
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
   
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
   
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
   
                    res.redirect('/');
                });
            },
   
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
   
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };
4. Speichern und schließen Sie die Datei **tasklist.js** .

### <a name="add-configjs"></a>Fügen Sie config.js hinzu.
1. Erstellen Sie im Projektverzeichnis eine neue Datei namens **config.js**.
2. Fügen Sie **config.js**Folgendes hinzu. Dadurch werden die für Ihre Anwendung erforderlichen Konfigurationseinstellungen und Werte definiert.
   
        var config = {}
   
        config.host = process.env.HOST || "[the URI value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. Aktualisieren Sie in der Datei **config.js** die Werte für „HOST“ und „AUTH_KEY“ mit den Werten auf dem Blatt „Schlüssel“ Ihres Azure Cosmos DB-Kontos im [Microsoft Azure-Portal](https://portal.azure.com).
4. Speichern und schließen Sie die Datei **config.js** .

### <a name="modify-appjs"></a>Ändern von app.js
1. Öffnen Sie im Projektverzeichnis die Datei **app.js** . Diese Datei wurde bereits während der Erstellung der Express-Webanwendung erstellt.
2. Fügen Sie am Anfang von **app.js**
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. Dieser Code definiert die zu verwendende Konfigurationsdatei und liest Werte aus dieser Datei für einige Variablen, die wir später verwenden.
4. Ersetzen Sie die folgenden beiden Zeilen in der Datei **app.js** :
   
        app.use('/', index);
        app.use('/users', users); 
   
      durch den folgenden Codeausschnitt:
   
        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');
5. Diese Zeilen definieren eine neue Instanz unseres **TaskDao**-Objekts mit einer neuen Verbindung zu Azure Cosmos DB (unter Verwendung der aus **config.js** gelesenen Werte). Sie initialisieren das Task-Objekt und binden dann Formularaktionen an Methoden auf unserem **TaskList**-Controller. 
6. Speichern und schließen Sie abschließend die Datei **app.js**. Wir sind fast fertig.

## <a name="_Toc395783181"></a>Schritt 5: Erstellen einer Benutzeroberfläche
Jetzt konzentrieren wir uns auf die Erstellung der Benutzeroberfläche, um den Benutzern die eigentliche Interaktion mit unserer Anwendung zu ermöglichen. Die von uns erstellte Express-Anwendung verwendet **Jade** als Ansichtsmodul. Weitere Informationen zu Jade finden Sie unter [http://jade-lang.com/](http://jade-lang.com/).

1. Die Datei **layout.jade** im Verzeichnis **views** dient als globale Vorlage für andere **.jade**-Dateien. In diesem Schritt werden Sie sie modifizieren, um [Twitter Bootstrap](https://github.com/twbs/bootstrap)zu verwenden, ein Toolkit zum mühelosen Gestalten ansprechender Websites. 
2. Öffnen Sie die Datei **layout.jade**, die sich im Ordner **views** befindet, und ersetzen Sie die Inhalte durch Folgendes:
   
        doctype html
        html
           head
             title= title
             link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
             link(rel='stylesheet', href='/stylesheets/style.css')
           body
             nav.navbar.navbar-inverse.navbar-fixed-top
               div.navbar-header
                 a.navbar-brand(href='#') My Tasks
             block content
             script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
             script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')

    Dadurch wird dem **Jade**-Modul mitgeteilt, dass für unsere Anwendung einige HTML-Elemente dargestellt werden müssen. Das Modul erstellt dann einen **Block** mit der Bezeichnung **content**, in dem wir das Layout für unsere Inhaltsseiten bereitstellen können.
    Speichern und schließen Sie die Datei **layout.jade** .

3. Öffnen Sie nun die Datei **index.jade**, d. h. die Ansicht, die von der Anwendung verwendet wird, und ersetzen Sie den Inhalt der Datei durch Folgendes:
   
        extends layout
        block content
           h1 #{title}
           br
        
           form(action="/completetask", method="post")
             table.table.table-striped.table-bordered
               tr
                 td Name
                 td Category
                 td Date
                 td Complete
               if (typeof tasks === "undefined")
                 tr
                   td
               else
                 each task in tasks
                   tr
                     td #{task.name}
                     td #{task.category}
                     - var date  = new Date(task.date);
                     - var day   = date.getDate();
                     - var month = date.getMonth() + 1;
                     - var year  = date.getFullYear();
                     td #{month + "/" + day + "/" + year}
                     td
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             label Item Name:
             input(name="name", type="textbox")
             label Item Category:
             input(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   
    Dadurch wird das Layout erweitert und Inhalt für den Platzhalter **content** bereitgestellt, den wir zuvor in der Datei **layout.jade** gesehen haben.
   
    In diesem Layout haben wir zwei HTML-Formulare erstellt. 
    Das erste Formular enthält eine Tabelle für unsere Daten sowie eine Schaltfläche, mit der die Elemente durch Übermittlung an die **/completetask** -Methode des Controllers aktualisiert werden können.
    Das zweite Formular enthält zwei Eingabefelder und eine Schaltfläche, mit der Sie ein neues Element durch Übermittlung an die **/addtask** -Methode des Controllers erstellen können.
   
    Dies sollte jetzt alles sein, damit unsere Anwendung funktioniert.
4. Öffnen Sie die Datei **style.css** im Verzeichnis **Public\stylesheets**, und ersetzen Sie den Code durch Folgendes:
   
        body {
          padding: 50px;
          font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
        }
        a {
          color: #00B7FF;
        }
        .well label {
          display: block;
        }
        .well input {
          margin-bottom: 5px;
        }
        .btn {
          margin-top: 5px;
          border: outset 1px #C8C8C8;
        }
   
    Speichern und schließen Sie die Datei **style.css** .

## <a name="_Toc395783181"></a>Schritt 6: Lokales Ausführen der Anwendung
1. Um die Anwendung auf dem lokalen Computer zu testen, führen Sie `npm start` über das Terminal aus, um Ihre Anwendung zu starten, und aktualisieren Sie anschließend die Browserseite [http://localhost:3000](http://localhost:3000). Die Seite sollte nun wie folgt aussehen:
   
    ![Screenshot der Anwendung "Meine Aufgabenliste" in einem Browserfenster](./media/documentdb-nodejs-application/image18.png)

    > [!TIP]
    > Sollte eine Fehlermeldung mit einem Hinweis auf den Einzug in der Datei „layout.jade“ oder „index.jade“ ausgegeben werden, vergewissern Sie sich, dass die ersten beiden Zeilen der beiden Dateien ohne Leerzeichen linksbündig ausgerichtet sind. Sollten sich vor den ersten beiden Zeilen Leerzeichen befinden, entfernen Sie sie, speichern Sie beide Dateien, und aktualisieren Sie anschließend Ihr Browserfenster. 

2. Geben Sie unter Verwendung der Felder „Element“, „Elementname“ und „Kategorie“ eine neue Aufgabe ein, und klicken Sie anschließend auf **Element hinzufügen**. Dadurch wird in Azure Cosmos DB ein Dokument mit diesen Eigenschaften erstellt. 
3. Die Seite sollte nun das neu erstellte Element in der Aufgabenliste anzeigen.
   
    ![Screenshot der Anwendung mit einem neuen Element in der Aufgabenliste](./media/documentdb-nodejs-application/image19.png)
4. Zum Abschließen einer Aufgabe aktivieren Sie einfach das Kontrollkästchen in der entsprechenden Spalte, und klicken Sie dann auf **Aufgaben aktualisieren**. Dadurch wird das bereits erstellte Dokument aktualisiert.

5. Drücken Sie zum Beenden der Anwendung im Terminalfenster STRG+C, und klicken Sie dann auf **J**, um den Batchauftrag zu beenden.

## <a name="_Toc395783182"></a>Schritt 7: Bereitstellen Ihres Anwendungsentwicklungsprojekts auf Azure Websites
1. Falls noch nicht geschehen, aktivieren Sie ein Git-Repository für Ihre Azure-Website. Eine Anleitung hierzu finden Sie im Thema [Lokale Git-Bereitstellung in Azure App Service](../app-service-web/app-service-deploy-local-git.md) .
2. Fügen Sie Ihre Azure-Website als "Git Remote" hinzu.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Stellen Sie sie mittels Push auf Remote bereit.
   
        git push azure master
4. Dann schließt Git die Veröffentlichung Ihrer Webanwendung in wenigen Sekunden ab und startet einen Browser, in dem Sie das Ergebnis Ihrer Arbeit in Azure sehen können!

    Glückwunsch! Sie haben Ihre erste Node.js-Express-Webanwendung mit Azure Cosmos DB erstellt und auf Azure-Websites veröffentlicht.

    Die vollständige Anwendung für dieses Tutorial kann von [GitHub][GitHub] heruntergeladen und als Referenz herangezogen werden.

## <a name="_Toc395637775"></a>Nächste Schritte

* Möchten Sie Azure Cosmos DB nutzen, um Skalierungs- und Leistungstests durchzuführen? Weitere Informationen finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).
* Informieren Sie sich über das [Überwachen eines Azure Cosmos DB-Kontos](monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.
* Sehen Sie sich die [Dokumentation für Azure Cosmos DB](https://docs.microsoft.com/azure/documentdb/) an.

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app


