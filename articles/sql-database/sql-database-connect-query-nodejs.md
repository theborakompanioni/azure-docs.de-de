---
title: Herstellen von Verbindungen mit Azure SQL-Datenbank mithilfe von Node.js | Microsoft-Dokumentation
description: "Zeigt ein Node.js -Codebeispiel an, das Sie zum Herstellen einer Verbindung mit Azure SQL-Datenbank und Senden entsprechender Abfragen verwenden können."
services: sql-database
documentationcenter: 
author: LuisBosquez
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: lbosq
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 4d3c3541749870b09aecc9efb63413f7c045e044
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="azure-sql-database-use-nodejs-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von Node.js zum Herstellen einer Verbindung und Abfragen von Daten

In diesem Schnellstart wird veranschaulicht, wie Sie [Node.js](https://nodejs.org/en/) nutzen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen. Anschließend können Sie Transact-SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank auf Windows-, Ubuntu Linux- und MacOS-Plattformen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)
- [Erstellen einer Datenbank – PowerShell](sql-database-get-started-powershell.md)

## <a name="install-nodejs"></a>Installieren von Node.js 

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Node.js-Entwicklung vertraut sind und noch keine Erfahrung mit Azure SQL-Datenbank haben. Falls Sie noch keine Erfahrung mit der Node.js-Entwicklung haben, navigieren Sie zu [Erstellen einer App mithilfe von SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/), und wählen Sie erst **Node.js** und dann Ihr Betriebssystem aus.

### <a name="mac-os"></a>**Mac OS**
Geben Sie die folgenden Befehle zum Installieren von **brew** ein, einem einfach zu verwendenden Paket-Manager für Mac OS X und **Node.js**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Geben Sie die folgenden Befehle zum Installieren von **Node.js** und **npm**, dem Paket-Manager für Node.js, ein.

```bash
sudo apt-get install -y nodejs npm
```

### <a name="windows"></a>**Windows**
Besuchen Sie die [Node.js-Seite „Downloads“](https://nodejs.org/en/download/), und wählen Sie die gewünschte Windows Installer-Option aus.


## <a name="install-the-tedious-sql-server-driver-for-nodejs"></a>Installieren des SQL Server-Treibers „tedious“ für Node.js
Der empfohlene Treiber für Node.js ist **[tedious](https://github.com/tediousjs/tedious)**. tedious ist eine Open-Source-Initiative, in deren Rahmen Microsoft für Node.js-Anwendungen auf beliebigen Plattformen Beiträge leistet. Für dieses Tutorial benötigen Sie ein leeres Verzeichnis für Ihren Code und die `npm`-Abhängigkeiten, die wir installieren.

Zum Installieren des **tedious**-Treibers in Ihrem Verzeichnis führen Sie den folgenden Befehl aus:

```cmd
npm install tedious
```

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der Abbildung unten dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen. 

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbankserver vergessen haben, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.
    
## <a name="select-data"></a>Auswählen von Daten

Verwenden Sie den folgenden Code, um Ihre Azure SQL-Datenbank auf die 20 wichtigsten Produkte nach Kategorie abzufragen. Importieren Sie zunächst die Treiberklassen „Connect“ und „Request“ aus der Treiberbibliothek von tedious. Erstellen Sie anschließend das Konfigurationsobjekt, und ersetzen Sie die Variablen **username**, **password**, **server** und **database** durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. Erstellen Sie ein `Connection`-Objekt mit dem angegebenen `config`-Objekt. Definieren Sie danach den Rückruf für das `connect`-Ereignis des `connection`-Objekts zum Ausführen der `queryDatabase()`-Funktion.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}
var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        queryDatabase()
    }
});

function queryDatabase(){
    console.log('Reading rows from the Table...');

    // Read all rows from table
    request = new Request(
        "SELECT TOP 1 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    
    request.on('row', function(columns) {
        columns.forEach(function(column) {
            console.log("%s\t%s", column.metadata.colName, column.value);
        });
    });

    connection.execSql(request);
}
```

## <a name="insert-data-into-the-database"></a>Einfügen von Daten in die Datenbank
Verwenden Sie den folgenden Code, um mit der Transact-SQL-Anweisung [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) und der Funktion `insertIntoDatabase()` ein neues Produkt in die Tabelle „SalesLT.Product“ einzufügen. Ersetzen Sie die Variablen **username**, **password**, **server** und **database** durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        insertIntoDatabase()
    }
});

function insertIntoDatabase(){
    console.log("Inserting a brand new product into database...");
    request = new Request(
        "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) inserted');
        }
    );
    connection.execSql(request);
}
```

## <a name="update-data-in-the-database"></a>Aktualisieren von Daten in der Datenbank
Verwenden Sie den folgenden Code, um das neue zuvor hinzugefügte Produkt mit der Funktion `updateInDatabase()` und der Transact-SQL-Anweisung [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) zu löschen. Ersetzen Sie die Variablen **username**, **password**, **server** und **database** durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. Dieses Beispiel verwendet den Produktnamen, der im vorherigen Beispiel eingefügt wurde.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        updateInDatabase()
    }
});

function updateInDatabase(){
    console.log("Updating the price of the brand new product in database...");
    request = new Request(
        "UPDATE SalesLT.Product SET ListPrice = 50 WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) updated');
        }
    );
    connection.execSql(request);
}
```

## <a name="delete-data-from-the-database"></a>Löschen von Daten aus der Datenbank
Verwenden Sie den folgenden Code, um Daten aus der Datenbank zu löschen. Ersetzen Sie die Variablen **username**, **password**, **server** und **database** durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. Verwenden Sie dieses Mal in der `deleteFromDatabase()`-Funktion eine **DELETE-Anweisung**. Dieses Beispiel verwendet auch den Produktnamen, der im vorherigen Beispiel eingefügt wurde.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        deleteFromDatabase()
    }
});

function deleteFromDatabase(){
    console.log("Deleting the brand new product in database...");
    request = new Request(
        "DELETE FROM SalesLT.Product WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    connection.execSql(request);
}
```


## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/) (Microsoft Node.js-Treiber für SQL Server)
- [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md)
- [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md)



