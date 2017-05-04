---
title: Herstellen von Verbindungen mit Azure SQL-Datenbank mithilfe von Python | Microsoft-Dokumentation
description: "Zeigt ein Python-Codebeispiel an, das Sie zum Herstellen einer Verbindung mit Azure SQL-Datenbank und Senden von entsprechenden Abfragen verwenden können."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: quick start connect
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/17/2017
ms.author: meetb;carlrab;sstein
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: f055f1eb2c6f1c0fa9f032f033929299e224de2e
ms.lasthandoff: 04/20/2017


---
# <a name="azure-sql-database-use-python-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von Python zum Herstellen einer Verbindung und Abfragen von Daten

 In diesem Schnellstart wird veranschaulicht, wie Sie [Python](https://python.org) nutzen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen. Anschließend können Sie Transact-SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank auf Mac OS-, Windows- und Ubuntu Linux-Plattformen verwenden.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="install-the-python-and-database-communication-libraries"></a>Installieren von Python und Datenbank-Kommunikationsbibliotheken

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Python-Entwicklung vertraut sind und noch keine Erfahrung mit Azure SQL-Datenbank haben. Falls Sie noch keine Erfahrung mit der Python-Entwicklung haben, navigieren Sie zu [Erstellen einer App mithilfe von SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/), und wählen Sie erst **Python** und dann Ihr Betriebssystem aus.

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
Installieren Sie den [Microsoft ODBC-Treiber 13.1](https://www.microsoft.com/download/details.aspx?id=53339) (aktualisieren Sie den Treiber bei Aufforderung). pyodbc verwendet den Microsoft ODBC-Treiber unter Linux zum Herstellen von Verbindungen mit SQL-Datenbank-Instanzen. 

Installieren Sie anschließend **pyodbc** mithilfe von pip.

```cmd
pip install pyodbc==3.1.1
```

Anweisungen zum Aktivieren der Verwendung von pip finden Sie [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows).

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der Abbildung unten dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen. 

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbankserver vergessen haben, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.     
   
## <a name="select-data"></a>Auswählen von Daten

Verwenden Sie den folgenden Code, um mit der Transact-SQL-Anweisung [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) und der Funktion [pyodbc.connect]((https://github.com/mkleehammer/pyodbc/wiki)) die 20 wichtigsten Produkte nach Kategorie abzufragen. Mit der [cursor.execute](https://mkleehammer.github.io/pyodbc/api-cursor.html)-Funktion können Sie ein Resultset aus einer Abfrage einer SQL-Datenbank-Instanz abrufen. Diese Funktion akzeptiert jede Abfrage und gibt ein Resultset zurück, das mithilfe von [cursor.fetchone()](https://mkleehammer.github.io/pyodbc/api-cursor.html) durchlaufen werden kann. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
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
Verwenden Sie den folgenden Code, um mit der Transact-SQL-Anweisung [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) und der Funktion [cursor.execute](https://mkleehammer.github.io/pyodbc/api-cursor.html) ein neues Produkt in die Tabelle „SalesLT.Product“ einzufügen. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
with cursor.execute("INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')"): 
    print ('Successfuly Inserted!')
cnxn.commit()
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um das neue zuvor hinzugefügte Produkt mit der Funktion [cursor.execute](https://mkleehammer.github.io/pyodbc/api-cursor.html) und der Transact-SQL-Anweisung [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) zu aktualisieren. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?"
with cursor.execute(tsql,50,'BrandNewProduct'):
    print ('Successfuly Updated!')
cnxn.commit()

```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um das neue zuvor hinzugefügte Produkt mit der Funktion [cursor.execute](https://mkleehammer.github.io/pyodbc/api-cursor.html) und der Transact-SQL-Anweisung [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) zu löschen. Ersetzen Sie die Parameter „server“, „username“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "DELETE FROM SalesLT.Product WHERE Name = ?"
with cursor.execute(tsql,'BrandNewProduct'):
    print ('Successfuly Deleted!')
cnxn.commit()
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über den [Microsoft Python-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/).
- Besuchen Sie das [Python Developer Center](/develop/python/).
- Weitere Informationen zum Herstellen einer Verbindung und Durchführen von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](sql-database-connect-query-dotnet.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit PHP finden Sie unter [Verbinden und Abfragen mit PHP](sql-database-connect-query-php.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js](sql-database-connect-query-nodejs.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Java finden Sie unter [Verbinden und Abfragen mit Java](sql-database-connect-query-java.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Ruby finden Sie unter [Verbinden und Abfragen mit Ruby](sql-database-connect-query-ruby.md).

