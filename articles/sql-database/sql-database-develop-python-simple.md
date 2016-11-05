---
title: Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Python | Microsoft Docs
description: Zeigt ein Python-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank.
services: sql-database
documentationcenter: ''
author: meet-bhagdev
manager: jhubbard
editor: ''

ms.service: sql-database
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 10/05/2016
ms.author: meetb

---
# <a name="connect-to-sql-database-by-using-python"></a>Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Python
[!INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird gezeigt, wie Sie mithilfe von Python eine Verbindung mit einer Azure SQL-Datenbank herstellen und Abfragen ausführen. Dieses Beispiel kann auf Windows-, Ubuntu Linux- oder Mac-Plattformen ausgeführt werden.

## <a name="step-1:-create-a-sql-database"></a>Schritt 1: Erstellen einer SQL-Datenbank
Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank.  Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage**befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**. Nachdem Sie Ihre Datenbank erstellt haben, stellen Sie sicher, dass Sie den Zugriff auf Ihre IP-Adresse aktivieren. Aktivieren Sie dazu die Firewallregeln, wie auf der [Seite mit den ersten Schritten](sql-database-get-started.md) beschrieben.

## <a name="step-2:-configure-development-environment"></a>Schritt 2: Konfigurieren der Entwicklungsumgebung
### <a name="**mac-os**"></a>**Mac OS**
### <a name="install-the-required-modules"></a>Installieren der erforderlichen Module
Öffnen Sie das Terminal, und installieren Sie folgende Komponenten:

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="**linux-(ubuntu)**"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Python-Skript erstellen möchten. Geben Sie die folgenden Befehle ein, um **FreeTDS** und **pymssql** zu installieren. pymssql verwendet FreeTDS, um eine Verbindung mit den SQL-Databank-Instanzen herzustellen.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1

### <a name="**windows**"></a>**Windows**
Installieren Sie pymssql von [**hier**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Stellen Sie sicher, dass Sie die richtige WHL-Datei auswählen. Wählen Sie z.B. bei Verwendung von Python 2.7 auf einem 64-Bit-Computer die Datei „pymssql‑2.1.1‑cp27‑none‑win_amd64.whl“ aus. Speichern Sie die WHL-Datei nach dem Herunterladen im Ordner „C:/Python27“.

Installieren Sie jetzt den pymssql-Treiber mithilfe des pip-Befehls von der Befehlszeile. Wechseln Sie mit dem Befehl „cd“ zum Verzeichnis „C:/Python27“, und führen Sie Folgendes aus.

    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Anweisungen zum Aktivieren der Verwendung von pip finden Sie [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows).

## <a name="step-3:-run-sample-code"></a>Schritt 3: Ausführen von Beispielcode
Erstellen Sie eine Datei namens **sql_sample.py**, und fügen Sie den folgenden Code in die Datei ein. Sie können dies mithilfe dieser Befehle von der Befehlszeile ausführen:

    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Herstellen von Verbindungen mit der SQL-Datenbank
Die [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) -Funktion dient zum Herstellen einer Verbindung mit der SQL-Datenbank.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Ausführen von SQL-SELECT-Anweisungen
Mit der [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) -Funktion können Sie ein Resultset aus einer Abfrage einer SQL-Datenbank abrufen. Diese Funktion akzeptiert praktisch jede Abfrage und gibt ein Resultset zurück, das mithilfe von [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone)durchlaufen werden kann.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row,-pass-parameters,-and-retrieve-the-generated-primary-key"></a>Einfügen von Zeilen, Übergeben von Parametern und Abrufen von generierten Primärschlüsseln
In SQL-Datenbank können die [IDENTITY](https://msdn.microsoft.com/library/ms186775.aspx)-Eigenschaft und das [SEQUENCE](https://msdn.microsoft.com/library/ff878058.aspx)-Objekt zum automatischen Generieren von Werten für [Primärschlüssel](https://msdn.microsoft.com/library/ms179610.aspx) verwendet werden. 

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transaktionen
Dieses Codebeispiel veranschaulicht die Verwendung von Transaktionen für folgende Aufgaben:

* Starten von Transaktionen
* Einfügen einer Zeile mit Daten
* Durchführen von Rollbacks für Transaktionen, um Einfügungen rückgängig zu machen 

Fügen Sie folgenden Code in „sql_sample.py“ ein.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft Python-Treiber für SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Besuchen Sie das [Python Developer Center](/develop/python/).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)

<!--HONumber=Oct16_HO2-->


