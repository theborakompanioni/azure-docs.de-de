---
title: Herstellen von Verbindungen mit Azure SQL-Datenbank mithilfe von Python | Microsoft-Dokumentation
description: Zeigt ein Python-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank.
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: quick start
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 03/27/2017
ms.author: meetb;carlrab;sstein
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 91e1dcd5b4a7dc62a09c9deb26622dacba1dcaa1
ms.lasthandoff: 03/30/2017


---
# <a name="azure-sql-database-use-python-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von Python zum Herstellen einer Verbindung und Abfragen von Daten

Verwenden Sie [Python](https://python.org), um eine Verbindung mit einer Azure SQL-Datenbank-Instanz herzustellen und Abfragen durchzuführen. In diesem Leitfaden wird beschrieben, wie Sie Python zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank-Instanz verwenden und dann Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen ausführen.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="configure-development-environment"></a>Konfigurieren der Entwicklungsumgebung
### <a name="mac-os"></a>**Mac OS**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Python-Skript erstellen möchten. Geben Sie die folgenden Befehle ein, um **brew**, den **Microsoft ODBC-Treiber für Mac** und **pyodbc** zu installieren. pyodbc verwendet den Microsoft ODBC-Treiber unter Linux zum Herstellen von Verbindungen mit SQL-Datenbank-Instanzen.

``` bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql
sudo pip install pyodbc==3.1.1
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Python-Skript erstellen möchten. Geben Sie die folgenden Befehle ein, um den **Microsoft ODBC-Treiber für Linux** und **pyodbc** zu installieren. pyodbc verwendet den Microsoft ODBC-Treiber unter Linux zum Herstellen von Verbindungen mit SQL-Datenbank-Instanzen.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev
sudo pip install pyodbc==3.1.1
```

### <a name="windows"></a>**Windows**
Installieren Sie den [Microsoft ODBC-Treiber 13.1](https://www.microsoft.com/download/details.aspx?id=53339). pyodbc verwendet den Microsoft ODBC-Treiber unter Linux zum Herstellen von Verbindungen mit SQL-Datenbank-Instanzen. 

Installieren Sie anschließend pyodbc mithilfe von pip.

```cmd
pip install pyodbc==3.1.1
```

Anweisungen zum Aktivieren der Verwendung von pip finden Sie [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows).

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungszeichenfolge im Azure-Portal ab. Sie verwenden die Verbindungszeichenfolge zum Herstellen einer Verbindung mit der Azure SQL-Datenbank.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie im Bereich **Zusammenfassung** für Ihre Datenbank den vollqualifizierten Servernamen. 

    <img src="./media/sql-database-connect-query-dotnet/server-name.png" alt="connection strings" style="width: 780px;" />
   
## <a name="select-data"></a>Auswählen von Daten
Verwenden Sie die [pyodbc.connect](https://mkleehammer.github.io/pyodbc/api-connection.html)-Funktion mit einer [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)-Transact-SQL-Anweisung, um Daten in Ihrer Azure SQL-Datenbank-Instanz abzufragen. Mit der [cursor.execute](https://mkleehammer.github.io/pyodbc/api-cursor.html) -Funktion können Sie ein Resultset aus einer Abfrage einer SQL-Datenbank abrufen. Diese Funktion akzeptiert praktisch jede Abfrage und gibt ein Resultset zurück, das mithilfe von [cursor.fetchone()](https://mkleehammer.github.io/pyodbc/api-cursor.html)durchlaufen werden kann.

```Python
import pyodbc
server = 'yourserver.database.windows.net'
database = 'yourdatabase'
username = 'yourusername'
password = 'yourpassword'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print str(row[0]) + " " + str(row[1])
    row = cursor.fetchone()
```


## <a name="insert-data"></a>Einfügen von Daten
In SQL-Datenbank können die [IDENTITY](https://msdn.microsoft.com/library/ms186775.aspx)-Eigenschaft und das [SEQUENCE](https://msdn.microsoft.com/library/ff878058.aspx)-Objekt zum automatischen Generieren von Werten für [Primärschlüssel](https://msdn.microsoft.com/library/ms179610.aspx) verwendet werden. 

```Python
import pyodbc
server = 'yourserver.database.windows.net'
database = 'yourdatabase'
username = 'yourusername'
password = 'yourpassword'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
with cursor.execute("INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')"): 
    print ('Successfuly Inserted!')
cnxn.commit()
```

## <a name="update-data"></a>Aktualisieren von Daten
Die [cursor.execute](https://mkleehammer.github.io/pyodbc/api-cursor.html)-Funktion kann mit einer [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)-Transact-SQL-Anweisung verwendet werden, um Daten in Ihrer Azure SQL-Datenbank-Instanz zu aktualisieren.

```Python
import pyodbc
server = 'yourserver.database.windows.net'
database = 'yourdatabase'
username = 'yourusername'
password = 'yourpassword'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?"
with cursor.execute(tsql,50,'BrandNewProduct'):
    print ('Successfuly Updated!')
cnxn.commit()

```


## <a name="delete-data"></a>Löschen von Daten
Die [cursor.execute](https://mkleehammer.github.io/pyodbc/api-cursor.html)-Funktion kann mit einer [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)-Transact-SQL-Anweisung verwendet werden, um Daten in Ihrer Azure SQL-Datenbank-Instanz zu löschen.

```Python
import pyodbc
server = 'yourserver.database.windows.net'
database = 'yourdatabase'
username = 'yourusername'
password = 'yourpassword'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "DELETE FROM SalesLT.Product WHERE Name = ?"
with cursor.execute(tsql,'BrandNewProduct'):
    print ('Successfuly Deleted!')
cnxn.commit()
```

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Entwicklung für SQL-Datenbank](sql-database-develop-overview.md).
* Informieren Sie sich über den [Microsoft Python-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/).
* Besuchen Sie das [Python Developer Center](/develop/python/).
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

