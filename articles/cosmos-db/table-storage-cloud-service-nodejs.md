---
title: Webanwendung mit Tabellenspeicher (Node.js) | Microsoft Docs
description: "Ein Lernprogramm, das auf dem Lernprogramm Web App mit Express basiert und Azure-Speicherdienste sowie das Azure-Modul hinzufügt."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: b802f880c1131abb7eb9ba00dd8f2e65017bc802
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="nodejs-web-application-using-storage"></a>Node.js-Webanwendung mit Speicher
## <a name="overview"></a>Übersicht
In diesem Tutorial erweitern Sie die Anwendung aus dem Tutorial [Node.js-Webanwendung mit Express], indem Sie die Microsoft Azure-Clientbibliotheken für Node.js für die Arbeit mit Datenverwaltungsdiensten verwenden. Sie erweitern Ihre Anwendung und erstellen eine webbasierte Anwendung mit Aufgabenlisten, die Sie in Azure bereitstellen können. Mit Aufgabenlisten können Benutzer Aufgaben abrufen, neue Aufgaben erstellen und Aufgaben als abgeschlossen markieren.

Die Aufgaben werden im Azure-Speicher gespeichert. Der Azure-Speicher bietet einen unstrukturierten Datenspeicher, der gleichzeitig fehlertolerant und hochverfügbar ist. Azure Storage umfasst verschiedene Datenstrukturen, in denen Sie Daten speichern und darauf zugreifen können. Sie können die Speicherdienste über die APIs verwenden, die im Azure SDK für Node.js enthalten sind, oder über die REST-APIs. Weitere Informationen finden Sie unter [Speichern und Zugreifen auf Daten in Azure].

In diesem Tutorial wird davon ausgegangen, dass Sie die [Node.js-Webanwendung] und [Node.js mit Express][Node.js-Webanwendung mit Express] bereits durchlaufen haben.

Es enthält die folgenden Informationen:

* Arbeiten mit der Jade Template Engine
* Arbeiten mit den Azure-Datenverwaltungsdiensten

Der folgende Screenshot zeigt die fertige Anwendung:

![Die abgeschlossene Webseite in Internet Explorer](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Einstellen der Speicher-Anmeldeinformationen in Web.Config
Sie müssen die Speicheranmeldeinformationen übergeben, um auf Azure Storage zugreifen zu können. Dies erfolgt über die Anwendungseinstellungen in „web.config“.
Diese Einstellungen in „web.config“ werden als Umgebungsvariablen an Node übergeben und anschließend vom Azure SDK gelesen.

> [!NOTE]
> Die Speicher-Anmeldeinformationen werden nur verwendet, wenn die Anwendung in Azure bereitgestellt wird. Bei der Ausführung im Emulator verwendet die Anwendung den Speicheremulator.
>
>

Führen Sie die folgenden Schritte aus, um die Anmeldeinformationen des Speicherkontos abzurufen und zu den web.config-Einstellungen hinzuzufügen:

1. Wenn es nicht bereits geöffnet ist, starten Sie Azure PowerShell aus dem **Start**-Menü, indem Sie **Alle Programme, Azure** erweitern, **Azure PowerShell** per Rechtsklick öffnen und dann **Als Administrator ausführen** auswählen.
2. Wechseln Sie in den Ordner, der Ihre Anwendung enthält. Beispiel: C:\\node\\tasklist\\WebRole1.
3. Geben Sie im Azure PowerShell-Fenster das folgende Cmdlet ein, um die Informationen für das Speicherkonto abzurufen:

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   Das Cmdlet oben ruft eine Liste der Speicherkonten und Kontoschlüssel Ihres gehosteten Diensts ab.

   > [!NOTE]
   > Da das Azure SDK beim Bereitstellen eines Dienstes ein Speicherkonto erstellt, sollten Sie bereits von den Bereitstellungen Ihrer Anwendung aus den vorherigen Tutorials ein Speicherkonto besitzen.
   >
   >
4. Öffnen Sie die Datei **ServiceDefinition.csdef** mit den Umgebungseinstellungen, die bei der Bereitstellung der Anwendung in Azure verwendet werden:

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. Fügen Sie den folgenden Block unter dem **Environment**-Element ein, und ersetzen Sie {STORAGE ACCOUNT} und {STORAGE ACCESS KEY} durch den Kontonamen und den primären Schlüssel des Speicherkontos, das Sie für die Bereitstellung verwenden möchten:

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Inhalt der Datei web.cloud.config](./media/table-storage-cloud-service-nodejs/node37.png)

6. Speichern Sie die Datei und schließen Sie Notepad.

### <a name="install-additional-modules"></a>Installieren zusätzlicher Module
1. Verwenden Sie den folgenden Befehl, um die Module [azure], [node-uuid], [nconf] und [async] lokal zu installieren und um einen Eintrag für sie in der Datei **package.json** zu speichern:

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  Die Ausgabe dieses Befehls sollte ungefähr wie folgt aussehen:

  ```
  node-uuid@1.4.1 node_modules\node-uuid

  nconf@0.6.9 node_modules\nconf
  ├── ini@1.1.0
  ├── async@0.2.9
  └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

  azure-storage@0.1.0 node_modules\azure-storage
  ├── extend@1.2.1
  ├── xmlbuilder@0.4.3
  ├── mime@1.2.11
  ├── underscore@1.4.4
  ├── validator@3.1.0
  ├── node-uuid@1.4.1
  ├── xml2js@0.2.7 (sax@0.5.2)
  └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)
  ```

## <a name="using-the-table-service-in-a-node-application"></a>Verwenden des Tabellendiensts in einer Node-Anwendung
In diesem Abschnitt erweitern Sie die durch den Befehl **express** erstellte Grundanwendung, indem Sie die Datei **task.js** hinzufügen, die das Modell für Ihre Aufgaben enthält. Sie ändern die vorhandene Datei **app.js** und erstellen eine neue Datei **tasklist.js**, die das Modell verwendet.

### <a name="create-the-model"></a>Erstellen des Modells
1. Erstellen Sie im Verzeichnis **WebRole1** ein neues Verzeichnis namens **models**.
2. Erstellen Sie im Verzeichnis **models** eine neue Datei namens **task.js**. Diese Datei enthält das Modell für die von Ihrer Anwendung erstellten Aufgaben.
3. Fügen Sie am Anfang der Datei **task.js** den folgenden Code hinzu, um auf die erforderlichen Bibliotheken zu verweisen:

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. Anschließend fügen Sie Code hinzu, um das Task-Objekt zu definieren und zu exportieren. Das Task-Objekt stellt die Verbindung mit der Tabelle her.

    ```nodejs
    module.exports = Task;

    function Task(storageClient, tableName, partitionKey) {
      this.storageClient = storageClient;
      this.tableName = tableName;
      this.partitionKey = partitionKey;
      this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
        if(error) {
          throw error;
        }
      });
    };
    ```

5. Fügen Sie als Nächstes den folgenden Code hinzu, um zusätzliche Methoden für das Task-Objekt zu definieren, die Interaktionen mit in der Tabelle gespeicherten Daten ermöglichen:

    ```nodejs
    Task.prototype = {
      find: function(query, callback) {
        self = this;
        self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
          if(error) {
            callback(error);
          } else {
            callback(null, result.entries);
          }
        });
      },

      addItem: function(item, callback) {
        self = this;
        // use entityGenerator to set types
        // NOTE: RowKey must be a string type, even though
        // it contains a GUID in this example.
        var itemDescriptor = {
          PartitionKey: entityGen.String(self.partitionKey),
          RowKey: entityGen.String(uuid()),
          name: entityGen.String(item.name),
          category: entityGen.String(item.category),
          completed: entityGen.Boolean(false)
        };

        self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
          if(error){
            callback(error);
          }
          callback(null);
        });
      },

      updateItem: function(rKey, callback) {
        self = this;
        self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
          if(error) {
            callback(error);
          }
          entity.completed._ = true;
          self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
            if(error) {
              callback(error);
            }
            callback(null);
          });
        });
      }
    }
    ```

6. Speichern und schließen Sie die Datei **task.js** .

### <a name="create-the-controller"></a>Erstellen des Controllers
1. Erstellen Sie im Verzeichnis **WebRole1/routes** eine neue Datei namens **tasklist.js**, und öffnen Sie sie in einem Text-Editor.
2. Fügen Sie **tasklist.js**den folgenden Code hinzu. Dieser Code lädt die Module „azure“ und „async“, die von **tasklist.js** verwendet werden, und definiert die **TaskList**-Funktion, an die eine Instanz des zuvor definierten **Task**-Objekts übergeben wird:

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. Fügen Sie außerdem zur Datei **tasklist.js** die Methoden **showTasks**, **addTask** und **completeTasks** hinzu:

    ```nodejs
    TaskList.prototype = {
      showTasks: function(req, res) {
        self = this;
        var query = azure.TableQuery()
          .where('completed eq ?', false);
        self.task.find(query, function itemsFound(error, items) {
          res.render('index',{title: 'My ToDo List ', tasks: items});
        });
      },

      addTask: function(req,res) {
        var self = this
        var item = req.body.item;
        self.task.addItem(item, function itemAdded(error) {
          if(error) {
            throw error;
          }
          res.redirect('/');
        });
      },

      completeTask: function(req,res) {
        var self = this;
        var completedTasks = Object.keys(req.body);
        async.forEach(completedTasks, function taskIterator(completedTask, callback) {
          self.task.updateItem(completedTask, function itemsUpdated(error) {
            if(error){
              callback(error);
            } else {
              callback(null);
            }
          });
        }, function goHome(error){
          if(error) {
            throw error;
          } else {
            res.redirect('/');
          }
        });
      }
    }
    ```

4. Speichern Sie die Datei **tasklist.js**.

### <a name="modify-appjs"></a>Ändern von app.js
1. Öffnen Sie im Verzeichnis **WebRole1** die Datei **app.js** in einem Texteditor.
2. Fügen Sie am Anfang der Datei Folgendes zum Laden des Azure-Moduls hinzu, und legen Sie den Tabellennamen und Partitionsschlüssel fest:

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. Blättern Sie in der Datei "app.js" nach unten, bis Sie die folgende Zeile sehen:

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    Ersetzen Sie die Zeilen oben durch den folgenden Code. Dieser Code initialisiert eine Instanz von <strong>Task</strong> mit einer Verbindung mit Ihrem Speicherkonto. Dieser <strong>Task</strong> wird an die <strong>TaskList</strong> übergeben, die darüber mit dem Tabellenspeicherdienst kommuniziert:

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. Speichern Sie die Datei **app.js** .

### <a name="modify-the-index-view"></a>Ändern der Indexansicht
1. Wechseln Sie zum Verzeichnis **views**, und öffnen Sie die Datei **index.jade** in einem Texteditor.
2. Ersetzen Sie den Inhalt der Datei **index.jade** durch den folgenden Code. Mit diesem Code werden die Ansicht zur Anzeige vorhandener Aufgaben und ein Formular für das Hinzufügen neuer Aufgaben und das Markieren von vorhandenen Aufgaben als abgeschlossen definiert.

    ```
    extends layout

    block content
      h1= title
      br

      form(action="/completetask", method="post")
        table.table.table-striped.table-bordered
          tr
            td Name
            td Category
            td Date
            td Complete
          if tasks != []
            tr
              td
          else
            each task in tasks
              tr
                td #{task.name._}
                td #{task.category._}
                - var day   = task.Timestamp._.getDate();
                - var month = task.Timestamp._.getMonth() + 1;
                - var year  = task.Timestamp._.getFullYear();
                td #{month + "/" + day + "/" + year}
                td
                  input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
        button.btn(type="submit") Update tasks
      hr
      form.well(action="/addtask", method="post")
        label Item Name:
        input(name="item[name]", type="textbox")
        label Item Category:
        input(name="item[category]", type="textbox")
        br
        button.btn(type="submit") Add item
    ```

3. Speichern und schließen Sie die Datei **index.jade** .

### <a name="modify-the-global-layout"></a>Modifizieren des globalen Layouts
Die Datei **layout.jade** im Verzeichnis **views** dient als globale Vorlage für andere **.jade**-Dateien. In diesem Schritt ändern Sie die Datei **layout.jade**, um [Twitter Bootstrap](https://github.com/twbs/bootstrap) zu verwenden, ein Toolkit zum einfachen Gestalten ansprechender Websites.

1. Laden Sie die Dateien für [Twitter Bootstrap](http://getbootstrap.com/)herunter, und extrahieren Sie sie. Kopieren Sie die Datei **bootstrap.min.css** aus dem Ordner **bootstrap\\dist\\css** in das Verzeichnis **public\\stylesheets** Ihrer Aufgabenlistenanwendung.
2. Öffnen Sie im Ordner **views** die Datei **layout.jade** in einem Text-Editor, und ersetzen Sie den Inhalt durch Folgendes:

    doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content

3. Speichern Sie die Datei **layout.jade**.

### <a name="running-the-application-in-the-emulator"></a>Ausführen der Anwendung im Emulator
Verwenden Sie den folgenden Befehl, um die Anwendung im Emulator zu starten.

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

Der Browser wird geöffnet und zeigt die folgende Seite an:

![Eine Webseite mit dem Titel My Task List, einer Tabelle mit Aufgaben und Feldern zum Hinzufügen neuer Aufgaben.](./media/table-storage-cloud-service-nodejs/node44.png)

Verwenden Sie das Formular zum Hinzufügen von Elementen, oder entfernen Sie vorhandene Elemente, indem Sie sie als abgeschlossen markieren.

## <a name="publishing-the-application-to-azure"></a>Veröffentlichen der Anwendung in Azure
Rufen Sie im Windows PowerShell-Fenster das folgende Cmdlet auf, um Ihren gehosteten Dienst erneut auf Azure zu veröffentlichen.

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

Ersetzen Sie **myuniquename** durch einen eindeutigen Namen für diese Anwendung. Ersetzen Sie **datacentername** durch den Namen eines Azure-Rechenzentrums wie **USA, Westen**.

Nach Abschluss der Bereitstellung sollten Sie eine Ausgabe ähnlich der folgenden erhalten:

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

Da Sie im Cmdlet die Option **-launch** angegeben haben, öffnet sich der Browser mit Ihrer Anwendung in Azure nach Abschluss der Veröffentlichung.

![Ein Browserfenster, das die My Task List-Seite anzeigt. Die URL zeigt an, dass die Seite nun in Azure gehostet wird.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Anhalten und Löschen Ihrer Anwendung
Nach der Bereitstellung der Anwendung empfiehlt es sich, diese zu deaktivieren, damit Sie andere Anwendungen im Rahmen der kostenlosen Serverzeit erstellen und bereitstellen können.

Bei Azure werden Webrolleninstanzen pro genutzter Serverzeitstunde berechnet.
Serverzeit wird genutzt, sobald die Anwendung bereitgestellt wird, selbst wenn die Instanzen nicht ausgeführt werden und sich im Status Beendet befinden.

In den folgenden Schritten erfahren Sie, wie Sie die Anwendung beenden und löschen.

1. Stoppen Sie im Windows PowerShell-Fenster die im vorherigen Abschnitt erstellte Dienstbereitstellung mit folgendem Cmdlet:

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   Das Stoppen des Diensts kann einige Minuten dauern. Wenn der Dienst gestoppt wurde, erhalten Sie eine entsprechende Nachricht.

2. Um den Dienst zu löschen, rufen Sie folgendes Cmdlet auf:

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   Geben Sie nach entsprechender Aufforderung **Y** ein, um den Dienst zu löschen.

   Das Löschen des Diensts kann einige Minuten dauern. Nachdem der Dienst gelöscht wurde, erhalten Sie eine entsprechende Nachricht.

[Node.js-Webanwendung mit Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[Speichern und Zugreifen auf Daten in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-Webanwendung]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/



