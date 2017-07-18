---
title: "Herstellen einer Verbindung mit Azure-Datenbank für PostgreSQL per Python | Microsoft-Dokumentation"
description: "Enthält ein Python-Codebeispiel, das Sie nutzen können, um zu den Daten von Azure-Datenbank für PostgreSQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: python
ms.topic: hero-article
ms.date: 07/07/2017
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 3cd090b02887857a68271f021e3580e05660d1dc
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="azure-database-for-postgresql-use-python-to-connect-and-query-data"></a>Azure-Datenbank für PostgreSQL: Verwenden von Python zum Herstellen von Verbindungen mit Daten und Durchführen von Abfragen
In diesem Schnellstart wird veranschaulicht, wie Sie [Python](https://python.org) nutzen, um eine Verbindung mit einer Azure-Datenbank für PostgreSQL herzustellen. Anschließend können Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank auf MacOS-, Ubuntu Linux- und Windows-Plattformen verwenden. Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Python-Entwicklung vertraut sind und noch keine Erfahrung mit Azure-Datenbank für PostgreSQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen einer Datenbank – Portal](quickstart-create-server-database-portal.md)
- [Erstellen einer Datenbank – CLI](quickstart-create-server-database-azure-cli.md)

Außerdem benötigen Sie:
- Installation von [Python](https://www.python.org/downloads/)
- Installation des [pip](https://pip.pypa.io/en/stable/installing/)-Pakets (pip ist bereits installiert, wenn Sie Binärdateien der Version Python 2 (mindestens 2.7.9) oder Python 3 (mindestens 3.4) verwenden, die Sie von [python.org](https://python.org) heruntergeladen haben, aber Sie müssen pip aktualisieren.)

## <a name="install-the-python-connection-libraries-for-postgresql"></a>Installieren der Python-Verbindungsbibliotheken für PostgreSQL
Installieren Sie das [psycopg2](http://initd.org/psycopg/docs/install.html)-Paket, mit dem Sie eine Verbindung für die Datenbank herstellen und diese abfragen können. psycopg2 ist [unter PyPI](https://pypi.python.org/pypi/psycopg2/) in Form von [wheel](http://pythonwheels.com/)-Paketen für die gängigen Plattformen (Linux, OSX, Windows) verfügbar. Sie können „pip install“ also verwenden, um die Binärversion des Moduls einschließlich aller Abhängigkeiten abzurufen:

```cmd
pip install psycopg2
```
Stellen Sie sicher, dass Sie eine aktuelle Version von pip verwenden (ein Upgrade ist beispielsweise mit `pip install -U pip` möglich).

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure-Datenbank für PostgreSQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem Server **mypgserver-20170401**, den Sie soeben erstellt haben.
3. Klicken Sie auf den Servernamen **mypgserver-20170401**.
4. Wählen Sie die Seite **Übersicht** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Azure-Datenbank für PostgreSQL – Anmeldename des Serveradministrators](./media/connect-python/1-connection-string.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

## <a name="how-to-run-python-code"></a>Ausführen von Python-Code
- Erstellen Sie mit Ihrem bevorzugten Text-Editor eine neue Datei namens „postgres.py“, und speichern Sie sie in einem Projektordner. Kopieren Sie eines der weiter unten gezeigten Codebeispiele, fügen Sie es in die Textdatei ein, und speichern Sie sie. Wählen Sie die UTF-8-Codierung aus, wenn Sie die Datei unter dem Windows-Betriebssystem speichern. 
- Starten Sie zum Ausführen des Codes die Eingabeaufforderung oder die Bash-Shell. Wechseln Sie in das Verzeichnis Ihres Projektordners (beispielsweise `cd postgresql`). Geben Sie anschließend den Python-Befehl ein, gefolgt vom Namen der Datei (beispielsweise `python postgresql.py`).

> [!NOTE]
> Ab Version 3 von Python wird beim Ausführen der weiter unten bereitgestellten Codeblöcke möglicherweise der Fehler `SyntaxError: Missing parentheses in call to 'print'` angezeigt. Ersetzen Sie in diesem Fall jeden Aufruf des Befehls `print "string"` durch einen Funktionsaufruf mit Klammern (beispielsweise `print("string")`).

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um die Verbindung herzustellen und die Daten zu laden, indem Sie die Funktion [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) mit der **INSERT**-SQL-Anweisung nutzen. Die Funktion [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) wird verwendet, um die SQL-Abfrage für die PostgreSQL-Datenbank auszuführen. Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die eingefügten Daten mit der Funktion [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) per **SELECT**-SQL-Anweisung zu lesen. Diese Funktion akzeptiert eine Abfrage und gibt ein Resultset zurück, das mithilfe von [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall) durchlaufen werden kann. Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Bestandszeile, die Sie zuvor mit der Funktion [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) eingefügt haben, per **UPDATE**-SQL-Anweisung zu aktualisieren. Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um ein Bestandselement, das Sie zuvor mit der Funktion [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) eingefügt haben, per **DELETE**-SQL-Anweisung zu löschen. Ersetzen Sie die Parameter „host“, „dbname“, „user“ und „password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

