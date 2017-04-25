---
title: Herstellen einer Verbindung mit Azure SQL-Datenbank mithilfe von PHP | Microsoft-Dokumentation
description: "Zeigt ein PHP-Codebeispiel an, das Sie zum Herstellen einer Verbindung mit Azure SQL-Datenbank und Senden von entsprechenden Abfragen verwenden können."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: quick start connect
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 04/17/2017
ms.author: meetb;carlrab;sstein
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: e39d108e9d6962647cbf76e50299b73939fe5977
ms.lasthandoff: 04/18/2017


---
# <a name="azure-sql-database-use-php-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von PHP zum Herstellen einer Verbindung und Abfragen von Daten

In diesem Schnellstart wird veranschaulicht, wie Sie [PHP](http://php.net/manual/en/intro-whatis.php) nutzen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen. Anschließend können Sie Transact-SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank über Mac OS-, Windows- und Ubuntu Linux-Plattformen verwenden.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="install-php-and-database-communications-software"></a>Installieren von PHP und Datenbankkommunikationssoftware
### <a name="mac-os"></a>**Mac OS**
Öffnen Sie Ihren Terminal, und geben Sie die folgenden Befehle ein, um von **brew**, **Microsoft ODBC Driver for Mac** und die **Microsoft PHP-Treiber für SQL Server** zu installieren. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql 
pecl install sqlsrv-4.1.7preview
pecl install pdo_sqlsrv-4.1.7preview
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Geben Sie die folgenden Befehle ein, um **Microsoft ODBC Driver for Linux** und die **Microsoft PHP-Treiber für SQL Server** zu installieren.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev gcc g++ php-dev
sudo pecl install sqlsrv pdo_sqlsrv
sudo echo "extension= pdo_sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
sudo echo "extension= sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
```

### <a name="windows"></a>**Windows**
- Installieren Sie PHP 7.1.1 (x64) [über den Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx?lang=). 
- Installieren Sie den [Microsoft ODBC-Treiber 13.1](https://www.microsoft.com/download/details.aspx?id=53339). 
- Laden Sie die nicht threadsicheren DLLs für den [Microsoft-PHP-Treiber für SQL Server](https://pecl.php.net/package/sqlsrv/4.1.6.1/windows) herunter, und fügen Sie die Binärdateien im Ordner „PHP\v7.x\ext“ ein.
- Bearbeiten Sie als Nächstes die Datei „php.ini“ (C:\Programme\PHP\v7.1\php.ini), und fügen Sie den Verweis auf die DLL hinzu. Beispiel:
      
      extension=php_sqlsrv.dll
      extension=php_pdo_sqlsrv.dll

An diesem Punkt sollten die DLLs bei PHP registriert sein.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungszeichenfolge im Azure-Portal ab. Sie verwenden die Verbindungszeichenfolge zum Herstellen einer Verbindung mit der Azure SQL-Datenbank.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der Abbildung unten dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen.  

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbankserver vergessen haben, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.     
    
## <a name="select-data"></a>Auswählen von Daten
Verwenden Sie den folgenden Code zum Abfragen Ihrer Azure SQL-Datenbank mithilfe der Funktion [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) und einer Transact-SQL-Anweisung des Typs [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql). Mit der Funktion „sqlsrv_query“ können Sie ein Resultset aus einer Abfrage einer SQL-Datenbank abrufen. Diese Funktion akzeptiert jede Abfrage und gibt ein Resultset zurück, das mithilfe von [sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php) durchlaufen werden kann. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
     FROM [SalesLT].[ProductCategory] pc
     JOIN [SalesLT].[Product] p
     ON pc.productcategoryid = p.productcategoryid";
$getResults= sqlsrv_query($conn, $tsql);
echo ("Reading data from table" . PHP_EOL);
if ($getResults == FALSE)
    echo (sqlsrv_errors());
while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
}
sqlsrv_free_stmt($getResults);
?>
```


## <a name="insert-data"></a>Einfügen von Daten
Verwenden Sie den folgenden Code zum Einfügen eines neuen Produkts in die Tabelle „SalesLT.Product“ in der angegebenen Datenbank mithilfe der Funktion[sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) und der Transact-SQL-Anweisung [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";
$params = array("BrandNewProduct", "200989", "Blue", 75, 80, "7/1/2016");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) inserted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code zum Aktualisieren von Daten in Ihrer Azure SQL-Datenbank-Instanz mithilfe der Funktion [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) und der Transact-SQL-Anweisung [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben.

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "UPDATE SalesLT.Product SET ListPrice =? WHERE Name = ?";
$params = array(50,"BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) updated" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code zum Löschen von Daten aus Ihrer Azure SQL-Datenbank-Instanz mithilfe der Funktion [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) und der Transact-SQL-Anweisung [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben.

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "DELETE FROM SalesLT.Product WHERE Name = ?";
$params = array("BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) deleted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Microsoft PHP-Treiber für SQL Server](https://github.com/Microsoft/msphpsql/).
- [Posten Sie Probleme, oder stellen Sie Fragen](https://github.com/Microsoft/msphpsql/issues).
- Weitere Informationen zum Herstellen einer Verbindung und Durchführen von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](sql-database-connect-query-dotnet.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js](sql-database-connect-query-nodejs.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Java finden Sie unter [Verbinden und Abfragen mit Java](sql-database-connect-query-java.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Python finden Sie unter [Verbinden und Abfragen mit Python](sql-database-connect-query-python.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Ruby finden Sie unter [Verbinden und Abfragen mit Ruby](sql-database-connect-query-ruby.md).

