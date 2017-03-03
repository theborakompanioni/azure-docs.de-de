---
title: Herstellen von Verbindungen mit SQL-Datenbank mithilfe von PHP unter Windows | Microsoft Docs
description: "Enthält ein PHP-Beispielprogramm, das eine Verbindung von einem Windows-Client mit Azure SQL-Datenbank herstellt, sowie Links zu den auf dem Client erforderlichen Softwarekomponenten."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: development
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 02/13/2017
ms.author: meetb
translationtype: Human Translation
ms.sourcegitcommit: 94fa09526683582bc017213d0ad9455f31cb22ae
ms.openlocfilehash: fba66e9d41daa2df34fbb3ffd8c92e664eaa560e
ms.lasthandoff: 02/21/2017


---

# <a name="connect-to-sql-database-by-using-php"></a>Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP
[!INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird gezeigt, wie Sie mithilfe von PHP eine Verbindung mit einer Azure SQL-Datenbank-Instanz herstellen und Abfragen ausführen. Dieses Beispiel kann unter Windows oder Linux ausgeführt werden. 


## <a name="step-1-create-a-sql-database"></a>Schritt 1: Erstellen einer SQL-Datenbank
Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank.  Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage**befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**. Nachdem Sie Ihre Datenbank erstellt haben, stellen Sie sicher, dass Sie den Zugriff auf Ihre IP-Adresse aktivieren. Aktivieren Sie dazu die Firewallregeln, wie auf der [Seite mit den ersten Schritten](sql-database-get-started.md) beschrieben.

## <a name="step-2-configure-development-environment"></a>Schritt 2: Konfigurieren der Entwicklungsumgebung

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Python-Skript erstellen möchten. Geben Sie die folgenden Befehle ein, um den **Microsoft ODBC-Treiber für Linux**, **pdo_sqlsrv** und **sqlsrv** zu installieren. Der Microsoft-PHP-Treiber für SQL Server verwendet den Microsoft-ODBC-Treiber unter Linux zum Herstellen von Verbindungen mit SQL-Datenbank-Instanzen.

```
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql unixodbc-dev gcc g++ build-essential
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

## <a name="step-3-run-sample-code"></a>Schritt 3: Ausführen von Beispielcode
Erstellen Sie eine Datei mit dem Namen **sql_sample.php**, und fügen Sie den folgenden Code in die Datei ein. Sie können dies mithilfe dieser Befehle von der Befehlszeile ausführen:

```
php sql_sample.php
```

### <a name="connect-to-your-sql-database"></a>Herstellen von Verbindungen mit der SQL-Datenbank
Die [sqlsrv_connect](http://php.net/manual/en/function.sqlsrv-connect.php)-Funktion dient zum Herstellen einer Verbindung mit SQL-Datenbank.

```
<?php
$serverName = "yourserver.database.windows.net";
$connectionOptions = array(
    "Database" => "yourdatabase",
    "Uid" => "yourusername",
    "PWD" => "yourpassword"
    );
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if($conn)
    echo "Connected!"
?>
```

### <a name="execute-an-sql-select-statement"></a>Ausführen von SQL-SELECT-Anweisungen
Mit der [sqlsrv_query](http://php.net/manual/en/function.sqlsrv-query.php)-Funktion können Sie ein Resultset aus einer Abfrage einer SQL-Datenbank abrufen. 

```
<?php
$serverName = "yourserver.database.windows.net";
$connectionOptions = array(
    "Database" => "yourdatabase",
    "Uid" => "yourusername",
    "PWD" => "yourpassword"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if($conn)
    echo "Connected!"
$tsql = "SELECT [CompanyName] FROM SalesLT.Customer";  
$getProducts = sqlsrv_query($conn, $tsql);  
if ($getProducts == FALSE)  
    die(FormatErrors(sqlsrv_errors()));  
$productCount = 0;  
while($row = sqlsrv_fetch_array($getProducts, SQLSRV_FETCH_ASSOC))  
{  
    echo($row['CompanyName']);  
    echo("<br/>");  
    $productCount++;  
}  
sqlsrv_free_stmt($getProducts);  
sqlsrv_close($conn);    
function FormatErrors( $errors )
{
    /* Display errors. */
    echo "Error information: ";
    
    foreach ( $errors as $error )
    {
        echo "SQLSTATE: ".$error['SQLSTATE']."";
        echo "Code: ".$error['code']."";
        echo "Message: ".$error['message']."";
    }
}
?>
```

### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Einfügen von Zeilen, Übergeben von Parametern und Abrufen von generierten Primärschlüsseln
In SQL-Datenbank können die [IDENTITY](https://msdn.microsoft.com/library/ms186775.aspx)-Eigenschaft und das [SEQUENCE](https://msdn.microsoft.com/library/ff878058.aspx)-Objekt zum automatischen Generieren von Werten für [Primärschlüssel](https://msdn.microsoft.com/library/ms179610.aspx) verwendet werden. 


```
<?php
$serverName = "yourserver.database.windows.net";
$connectionOptions = array(
    "Database" => "yourdatabase",
    "Uid" => "yourusername",
    "PWD" => "yourpassword"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if($conn)
    echo "Connected!"
$tsql = "INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server 1', 'SQL Server 2', 0, 0, getdate())";  
//Insert query  
$insertReview = sqlsrv_query($conn, $tsql);  
if($insertReview == FALSE)  
    die(FormatErrors( sqlsrv_errors()));  
echo "Product Key inserted is :";  
while($row = sqlsrv_fetch_array($insertReview, SQLSRV_FETCH_ASSOC))  
{     
    echo($row['ProductID']);  
}  
sqlsrv_free_stmt($insertReview);  
sqlsrv_close($conn);  
function FormatErrors( $errors )
{
    /* Display errors. */
    echo "Error information: ";
    foreach ( $errors as $error )
    {
        echo "SQLSTATE: ".$error['SQLSTATE']."";
        echo "Code: ".$error['code']."";
        echo "Message: ".$error['message']."";
    }
}
?>
```


## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft PHP-Treiber für SQL Server](https://github.com/Microsoft/msphpsql/)
* [Probleme/Fragen](https://github.com/Microsoft/msphpsql/issues)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)

