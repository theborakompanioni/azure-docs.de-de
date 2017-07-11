---
title: "Herstellen einer Verbindung mit Azure-Datenbank für PostgreSQL per Node.js | Microsoft-Dokumentation"
description: "Dieser Schnellstart enthält ein Node.js-Codebeispiel, das Sie nutzen können, um zu den Daten von Azure-Datenbank für PostgreSQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/23/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 6a51fcf4f4494e5b32ccf6dabb19f8d004bb20d4
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---

<a id="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data" class="xliff"></a>

# Azure-Datenbank für PostgreSQL: Verwenden von Node.js zum Herstellen von Verbindungen mit Daten und Durchführen von Abfragen
In diesem Schnellstart wird gezeigt, wie Sie eine Verbindung mit einer Azure-Datenbank für PostgreSQL per [Node.js](https://nodejs.org/) auf Windows-, Ubuntu Linux- und Mac-Plattformen herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der Node.js-Entwicklung vertraut sind und noch keine Erfahrung mit Azure-Datenbank für PostgreSQL haben.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen einer Datenbank – Portal](quickstart-create-server-database-portal.md)
- [Erstellen einer Datenbank – CLI](quickstart-create-server-database-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installieren von [Node.js](https://nodejs.org)
- Installieren Sie das [pg](https://www.npmjs.com/package/pg)-Paket. 

<a id="install-nodejs" class="xliff"></a>

## Installieren von Node.js 
Gehen Sie je nach Plattform wie folgt vor, um Node.js zu installieren:

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Geben Sie die folgenden Befehle zum Installieren von **brew** ein, einem einfach zu verwendenden Paket-Manager für Mac OS X und **Node.js**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Geben Sie die folgenden Befehle zum Installieren von **Node.js** und **npm**, dem Paket-Manager für Node.js, ein.

```bash
sudo apt-get install -y nodejs npm
```

<a id="windows" class="xliff"></a>

### **Windows**
Besuchen Sie die [Node.js-Seite „Downloads“](https://nodejs.org/en/download/), und wählen Sie die gewünschte Windows Installer-Option aus.

<a id="install-pg-client" class="xliff"></a>

## Installieren des pg-Clients
Installieren Sie [pg](https://www.npmjs.com/package/pg). Dies ist ein reiner nicht blockierender JavaScript-Client für Node.js, der zum Herstellen einer Verbindung mit PostgreSQL und Durchführen von Abfragen nützlich ist.

Führen Sie hierzu den Knoten-Paket-Manager (npm) für JavaScript an der Befehlszeile aus, um den pg-Client zu installieren.
```bash
npm install pg
```

Überprüfen Sie die Installation, indem Sie die installierten Pakete auflisten.
```bash
npm list
```
Die Ausgabe des list-Befehls bestätigt die Version der einzelnen Komponenten. 
```
`-- pg@6.2.3
  +-- buffer-writer@1.0.1
  +-- packet-reader@0.3.1
etc...
```

<a id="get-connection-information" class="xliff"></a>

## Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure-Datenbank für PostgreSQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem Server **mypgserver-20170401**, den Sie soeben erstellt haben.
3. Klicken Sie auf den Servernamen **mypgserver-20170401**.
4. Wählen Sie die Seite **Übersicht** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Azure-Datenbank für PostgreSQL – Anmeldename des Serveradministrators](./media/connect-nodejs/1-connection-string.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

<a id="running-the-javascript-code-in-nodejs" class="xliff"></a>

## Ausführen des JavaScript-Codes in Node.js
Sie können Node.js über die Eingabeaufforderung der Bash-Shell oder von Windows starten, indem Sie `node` eingeben und anschließend den JavaScript-Beispielcode interaktiv ausführen. Kopieren Sie ihn hierfür in die Eingabeaufforderung. Alternativ können Sie den JavaScript-Code in einer Textdatei speichern und `node filename.js` mit dem Dateinamen als Parameter für die Ausführung starten.

<a id="connect-create-table-and-insert-data" class="xliff"></a>

## Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten zu laden, indem Sie die SQL-Anweisungen **CREATE TABLE** und **INSERT INTO** verwenden.
Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }
});

function queryDatabase()
{
    client.query(
        ' \
            DROP TABLE IF EXISTS inventory; \
            CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER); \
            INSERT INTO inventory (name, quantity) VALUES (\'banana\', 150); \
            INSERT INTO inventory (name, quantity) VALUES (\'orange\', 154); \
            INSERT INTO inventory (name, quantity) VALUES (\'apple\', 100); \
        ',
        function (err)
    {
        console.log("Connection established");

        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;

                // Else closing connection finished without error
                console.log("Closed client connection");
            });

            console.log("Finished execution, exiting now");
            process.exit()
        }
    });
}
```

<a id="read-data" class="xliff"></a>

## Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};


const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;

    else
    {
        console.log("Connected to Azure Database for PostgreSQL server:" + config.host);
        queryDatabase();
    }
});

function queryDatabase()
{
    // Declare array to hold query result set
    const results = [];

    console.log("Running query to PostgreSQL server:" + config.host);

    // Perform query
    var query = client.query('SELECT * FROM inventory;');

    // Print result set
    query.on('row', function(row)
    {
        console.log("Read " + JSON.stringify(row));
    });

    // Exit program after execution
    query.on('end', function(row)
    {
        console.log("Finished execution, exiting now");
        process.exit()
    });
}
```

<a id="update-data" class="xliff"></a>

## Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }   
});

function queryDatabase()
{
    client.query('UPDATE inventory SET quantity= 1000 WHERE name=\'banana\';', function (err, result)
    {
        console.log("Connection established");

        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;
                
                // Else closing connection finished without error
                console.log("Closed client connection");
            });             
        }

        console.log("Finished execution, exiting now");
        process.exit()
    });
}
```

<a id="delete-data" class="xliff"></a>

## Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

var config =
{
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: 'mypgsqldb',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(function (err)
{
    if (err)
        throw err;
    else
    {
        queryDatabase();
    }   
});

function queryDatabase()
{
    client.query('DELETE FROM inventory WHERE name=\'apple\';', function (err, result)
    {
        console.log("Connection established");
        
        if (err)
            throw err;
        else
        {
            client.end(function (err)
            {
                if (err)
                    throw err;
                
                // Else closing connection finished without error
                console.log("Closed client connection");
            });

            console.log("Finished execution, exiting now");
            process.exit()          
        }
    }); 
}
```

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

