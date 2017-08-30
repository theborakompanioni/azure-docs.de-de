---
title: "Herstellen einer Verbindung mit Azure-Datenbank für PostgreSQL per Node.js | Microsoft-Dokumentation"
description: "Dieser Schnellstart enthält ein Node.js-Codebeispiel, das Sie nutzen können, um zu den Daten von Azure-Datenbank für PostgreSQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 06/23/2017
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: f6c98833c73b70bcf1f8ca53596a34f09807b276
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data"></a>Azure-Datenbank für PostgreSQL: Verwenden von Node.js zum Herstellen von Verbindungen mit Daten und Durchführen von Abfragen
Dieser Schnellstart zeigt, wie Sie mit [Node.js](https://nodejs.org/) eine Verbindung mit einer Azure-Datenbank für PostgreSQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der Node.js-Entwicklung vertraut sind und noch keine Erfahrung mit Azure-Datenbank für PostgreSQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen einer Datenbank – Portal](quickstart-create-server-database-portal.md)
- [Erstellen einer Datenbank – CLI](quickstart-create-server-database-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installieren von [Node.js](https://nodejs.org)

## <a name="install-pg-client"></a>Installieren des pg-Clients
Installieren Sie [pg](https://www.npmjs.com/package/pg), den PostgreSQL-Client für Node.js.

Führen Sie hierzu den Knoten-Paket-Manager (npm) für JavaScript an der Befehlszeile aus, um den pg-Client zu installieren.
```bash
npm install pg
```

Überprüfen Sie die Installation, indem Sie die installierten Pakete auflisten.
```bash
npm list
```

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure-Datenbank für PostgreSQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem Server, den Sie gerade erstellt haben.
3. Klicken Sie auf den Servernamen.
4. Wählen Sie die Seite **Übersicht** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Azure-Datenbank für PostgreSQL – Anmeldename des Serveradministrators](./media/connect-nodejs/1-connection-string.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

## <a name="running-the-javascript-code-in-nodejs"></a>Ausführen des JavaScript-Codes in Node.js
Sie können Node.js über die Eingabeaufforderung der Bash-Shell oder von Windows starten, indem Sie `node` eingeben und anschließend den JavaScript-Beispielcode interaktiv ausführen. Kopieren Sie ihn hierfür in die Eingabeaufforderung. Alternativ können Sie den JavaScript-Code in einer Textdatei speichern und `node filename.js` mit dem Dateinamen als Parameter für die Ausführung starten.

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten zu laden, indem Sie die SQL-Anweisungen **CREATE TABLE** und **INSERT INTO** verwenden.
Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DROP TABLE IF EXISTS inventory;
        CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
        INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
        INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
        INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    `;

    client
        .query(query)
        .then(() => {
            console.log('Table created successfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else { queryDatabase(); }
});

function queryDatabase() {
  
    console.log(`Running query to PostgreSQL server: ${config.host}`);

    const query = 'SELECT * FROM inventory;';

    client.query(query)
        .then(res => {
            const rows = res.rows;

            rows.map(row => {
                console.log(`Read: ${JSON.stringify(row)}`);
            });

            process.exit();
        })
        .catch(err => {
            console.log(err);
        });
}
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        UPDATE inventory 
        SET quantity= 1000 WHERE name='banana';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Update completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. Das [pg.Client](https://github.com/brianc/node-postgres/wiki/Client)-Objekt wird verwendet, um die Kommunikation mit dem PostgreSQL-Server zu ermöglichen. Die Funktion [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wird verwendet, um die Verbindung mit dem Server herzustellen. Die Funktion [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. 

Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) {
        throw err;
    } else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DELETE FROM inventory 
        WHERE name = 'apple';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Delete completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

