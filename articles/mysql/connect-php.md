---
title: "Herstellen einer Verbindung mit Azure-Datenbank für MySQL per PHP | Microsoft-Dokumentation"
description: "Dieser Schnellstart enthält mehrere PHP-Codebeispiele, die Sie nutzen können, um zu den Daten von Azure-Datenbank für MySQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 07/12/2017
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 92c34e7ac074ded1702e0df96298d33c60227ab3
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="azure-database-for-mysql-use-php-to-connect-and-query-data"></a>Azure-Datenbank für MySQL: Verwenden von PHP zum Verbinden und Abfragen von Daten
Dieser Schnellstart zeigt, wie Sie mit einer [PHP](http://php.net/manual/intro-whatis.php)-Anwendung eine Verbindung mit einer Azure-Datenbank für MySQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. In diesem Artikel wird davon ausgegangen, dass Sie mit der Entwicklung per PHP vertraut sind, aber noch keine Erfahrung mit Azure-Datenbank für MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a>Installieren von PHP
Installieren Sie PHP auf Ihrem eigenen Server, oder erstellen Sie eine Azure-[Web-App](../app-service/app-service-web-overview.md), die PHP enthält.

### <a name="macos"></a>macOS
- Herunterladen der [Version PHP 7.1.4](http://php.net/downloads.php)
- Installieren von PHP (Informationen zur weiteren Konfiguration finden Sie im [PHP-Handbuch](http://php.net/manual/install.macosx.php))

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Herunterladen von [Version PHP 7.1.4 Non-Thread Safe (x64)](http://php.net/downloads.php)
- Installieren von PHP (Informationen zur weiteren Konfiguration finden Sie im [PHP-Handbuch](http://php.net/manual/install.unix.php))

### <a name="windows"></a>Windows
- Herunterladen von [Version PHP 7.1.4 Non-Thread Safe (x64)](http://windows.php.net/download#php-7.1)
- Installieren von PHP (Informationen zur weiteren Konfiguration finden Sie im [PHP-Handbuch](http://php.net/manual/install.windows.php))

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im linken Bereich auf **Alle Ressourcen**, und suchen Sie dann nach dem Server, den Sie erstellt haben (z.B. **myserver4demo**).
3. Klicken Sie auf den Servernamen.
4. Wählen Sie die Seite mit den **Eigenschaften** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-php/1_server-properties-name-login.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

## <a name="connect-and-create-a-table"></a>Herstellen einer Verbindung und Erstellen einer Tabelle
Verwenden Sie den folgenden Code, um mit der **CREATE TABLE**-SQL-Anweisung eine Verbindung herzustellen und eine Tabelle zu erstellen. 

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code werden die Methoden [mysqli_init](http://php.net/manual/mysqli.init.php) und [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) aufgerufen, um eine Verbindung mit MySQL herzustellen. Anschließend wird die [mysqli_query](http://php.net/manual/mysqli.query.php)-Methode aufgerufen, um die Abfrage auszuführen. Als Nächstes wird die [mysqli_close](http://php.net/manual/mysqli.close.php)-Methode aufgerufen, um die Verbindung zu schließen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a>Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und per **INSERT**-SQL-Anweisung Daten einzufügen.

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_prepare](http://php.net/manual/mysqli.prepare.php)-Methode verwendet, um eine vorbereitete INSERT-Anweisung zu erstellen, und anschließend werden die Parameter für jeden eingefügten Spaltenwert mit der [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php)-Methode gebunden. Im Code wird die Anweisung mit der [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php)-Methode ausgeführt, und anschließend wird die Anweisung mit der [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php)-Methode geschlossen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close the connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen.  Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_query](http://php.net/manual/mysqli.query.php)-Methode zum Durchführen der SQL-Abfrage genutzt, und mit der [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php)-Methode werden die sich ergebenden Zeilen abgerufen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten per **UPDATE**-SQL-Anweisung zu aktualisieren.

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_prepare](http://php.net/manual/mysqli.prepare.php)-Methode verwendet, um eine vorbereitete UPDATE-Anweisung zu erstellen, und anschließend werden die Parameter für jeden aktualisierten Spaltenwert mit der [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php)-Methode gebunden. Im Code wird die Anweisung mit der [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php)-Methode ausgeführt, und anschließend wird die Anweisung mit der [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php)-Methode geschlossen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. 

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_prepare](http://php.net/manual/mysqli.prepare.php)-Methode verwendet, um eine vorbereitete DELETE-Anweisung zu erstellen, und anschließend werden die Parameter für die WHERE-Klausel in der Anweisung mit der [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php)-Methode gebunden. Im Code wird die Anweisung mit der [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php)-Methode ausgeführt, und anschließend wird die Anweisung mit der [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php)-Methode geschlossen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Erstellen einer PHP- und MySQL-Web-App in Azure](../app-service/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)

