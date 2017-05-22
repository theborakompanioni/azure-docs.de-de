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
ms.date: 05/07/2017
ms.author: meetb;carlrab;sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 2c0dc1bef0bf2b7b78e8858943ad6e0ede214681
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="azure-sql-database-use-php-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von PHP zum Herstellen einer Verbindung und Abfragen von Daten

In diesem Schnellstart wird veranschaulicht, wie Sie [PHP](http://php.net/manual/en/intro-whatis.php) nutzen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen. Anschließend können Sie Transact-SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank über MacOS-, Windows- und Ubuntu Linux-Plattformen verwenden.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="install-php-and-database-communications-software"></a>Installieren von PHP und Datenbankkommunikationssoftware

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der PHP-Entwicklung vertraut sind und noch keine Erfahrung mit Azure SQL-Datenbank haben. Falls Sie noch keine Erfahrung mit der PHP-Entwicklung haben, navigieren Sie zu [Erstellen einer App mithilfe von SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/), und wählen Sie erst **PHP** und dann Ihr Betriebssystem aus.

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

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der Abbildung unten dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen.  

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.     
    
## <a name="select-data"></a>Auswählen von Daten
Verwenden Sie den folgenden Code, um mit der Transact-SQL-Anweisung [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) und der Funktion [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) die 20 wichtigsten Produkte nach Kategorie abzufragen. Mit der Funktion „sqlsrv_query“ können Sie ein Resultset aus einer Abfrage einer SQL-Datenbank abrufen. Diese Funktion akzeptiert jede Abfrage und gibt ein Resultset zurück, das mithilfe von [sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php) durchlaufen werden kann. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

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
Verwenden Sie den folgenden Code, um mit der Transact-SQL-Anweisung [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) und der Funktion [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) ein neues Produkt in die Tabelle „SalesLT.Product“ einzufügen. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

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
Verwenden Sie den folgenden Code, um das neue zuvor hinzugefügte Produkt mit der Funktion [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) und der Transact-SQL-Anweisung [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) zu löschen. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben.

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
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Microsoft PHP Drivers for SQL Server](https://github.com/Microsoft/msphpsql/) (Microsoft-PHP-Treiber für SQL Server)
- [Melden von Problemen/Stellen von Fragen](https://github.com/Microsoft/msphpsql/issues)


