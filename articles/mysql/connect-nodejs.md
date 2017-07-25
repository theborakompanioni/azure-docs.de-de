---
title: "Herstellen einer Verbindung mit Azure-Datenbank für MySQL per Node.js | Microsoft-Dokumentation"
description: "Diese Schnellstartanleitung enthält mehrere Node.js-Codebeispiele, mit deren Hilfe Sie eine Verbindung mit Azure-Datenbank für MySQL herstellen und Daten daraus abfragen können."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/17/2017
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 0c0bd4b707c114d2991e5f0473a4bfbe9e463e3c
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="azure-database-for-mysql-use-nodejs-to-connect-and-query-data"></a>Azure-Datenbank für MySQL: Verwenden von Node.js zum Herstellen von Verbindungen mit Daten und Durchführen von Abfragen
In diesem Schnellstart wird gezeigt, wie Sie eine Verbindung mit einer Azure-Datenbank für MySQL per [Node.js](https://nodejs.org/) auf Windows-, Ubuntu Linux- und Mac-Plattformen herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der Node.js-Entwicklung vertraut sind und noch keine Erfahrung mit Azure-Datenbank für MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installation der [Node.js](https://nodejs.org)-Laufzeit
- Installation des [mysql2](https://www.npmjs.com/package/mysql2)-Pakets zum Herstellen der Verbindung mit MySQL für die Node.js-Anwendung 

## <a name="install-nodejs-and-the-mysql-connector"></a>Installation von Node.js und MySQL-Connector
Führen Sie je nach Plattform die entsprechende Anleitung für die Installation von Node.js aus. Verwenden Sie npm, um das mysql2-Paket und die Abhängigkeiten in Ihrem Projektordner zu installieren.

### <a name="windows"></a>**Windows**
1. Besuchen Sie die [Node.js-Seite „Downloads“](https://nodejs.org/en/download/), und wählen Sie die gewünschte Windows Installer-Option aus.
2. Erstellen Sie einen lokalen Projektordner, z.B. `nodejsmysql`. 
3. Starten Sie die Eingabeaufforderung, und wechseln Sie in den Projektordner, z.B.`cd c:\nodejsmysql\`
4. Führen Sie das NPM-Tool aus, um die mysql2-Bibliothek im Projektordner zu installieren.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. Überprüfen Sie die Installation, indem Sie sich den Ausgabetext von `npm list` für `mysql2@1.3.5` ansehen.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Führen Sie die folgenden Befehle zum Installieren von **Node.js** und **npm** (Paket-Manager für Node.js) aus.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Führen Sie die folgenden Befehle aus, um den Projektordner `mysqlnodejs` zu erstellen und das mysql2-Paket in diesem Ordner zu installieren.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. Überprüfen Sie die Installation, indem Sie sich den Ausgabetext von „npm list“ für `mysql2@1.3.5` ansehen.

### <a name="mac-os"></a>**Mac OS**
1. Geben Sie die folgenden Befehle zum Installieren von **brew** ein, einem einfach zu verwendenden Paket-Manager für Mac OS X und **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Führen Sie die folgenden Befehle aus, um den Projektordner `mysqlnodejs` zu erstellen und das mysql2-Paket in diesem Ordner zu installieren.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. Überprüfen Sie die Installation, indem Sie sich den Ausgabetext von `npm list` für `mysql2@1.3.6` ansehen. Die Versionsnummer kann hiervon abweichen, wenn neue Patches veröffentlicht werden.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im linken Bereich auf **Alle Ressourcen**, und suchen Sie dann nach dem Server, den Sie erstellt haben (z.B. **myserver4demo**).
3. Klicken Sie auf den Servernamen **myserver4demo**.
4. Wählen Sie die Seite mit den **Eigenschaften** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Azure-Datenbank für MySQL – Anmeldename des Serveradministrators](./media/connect-nodejs/1_server-properties-name-login.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

## <a name="running-the-javascript-code-in-nodejs"></a>Ausführen des JavaScript-Codes in Node.js
1. Fügen Sie den JavaScript-Code in Textdateien ein, und speichern Sie ihn in einem Projektordner mit der Dateierweiterung „.js“, z.B. „C:\nodejsmysql\createtable.js“ oder „/home/username/nodejsmysql/createtable.js“.
2. Starten Sie die Eingabeaufforderung oder die Bash-Shell. Wechseln Sie in das Verzeichnis Ihres Projektordners `cd nodejsmysql`.
3. Geben Sie anschließend den Node-Befehl gefolgt vom Dateinamen ein, z.B. `node createtable.js`, um die Anwendung auszuführen.
4. Unter Windows müssen Sie unter Umständen den vollständigen Pfad verwenden, um die Node-Anwendung zu starten, z.B. `"C:\Program Files\nodejs\node.exe" createtable.js`, wenn sich die Node-Anwendung nicht unter dem Pfad der Umgebungsvariablen befindet.

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten zu laden, indem Sie die SQL-Anweisungen **CREATE TABLE** und **INSERT INTO** verwenden.

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die Funktion [connect()](https://github.com/mysqljs/mysql#establishing-connections) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [query()](https://github.com/mysqljs/mysql#performing-queries) wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. 

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die [connect()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Verbindung mit dem Server herzustellen. Die [query()](https://github.com/mysqljs/mysql#performing-queries)-Methode wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. Das results-Array wird für die Ergebnisse der Abfrage verwendet.

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu verbinden und zu lesen. 

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die [connect()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Verbindung mit dem Server herzustellen. Die [query()](https://github.com/mysqljs/mysql#performing-queries)-Methode wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. 

Die [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Kommunikation mit dem MySQL-Server zu ermöglichen. Die [connect()](https://github.com/mysqljs/mysql#establishing-connections)-Methode wird verwendet, um die Verbindung mit dem Server herzustellen. Die [query()](https://github.com/mysqljs/mysql#performing-queries)-Methode wird verwendet, um die SQL-Abfrage für die MySQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter `host`, `user`, `password` und `database` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./concepts-migrate-import-export.md)

