---
title: "Herstellen einer Verbindung mit Azure-Datenbank für MySQL per Ruby | Microsoft-Dokumentation"
description: "Dieser Schnellstart enthält mehrere Ruby-Codebeispiele, die Sie nutzen können, um zu den Daten von Azure-Datenbank für MySQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 07/13/2017
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: e54f1dccbae060c52f48bfeb277c045b99a91715
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---

# <a name="azure-database-for-mysql-use-ruby-to-connect-and-query-data"></a>Azure-Datenbank für MySQL: Verwenden von Ruby zum Verbinden und Abfragen von Daten
In diesem Schnellstart wird gezeigt, wie Sie eine Verbindung mit einer Azure-Datenbank für MySQL per [Ruby](https://www.ruby-lang.org)-Anwendung und [mysql2](https://rubygems.org/gems/mysql2)-Gem auf Windows-, Ubuntu Linux- und Mac-Plattformen herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. In diesem Artikel wird davon ausgegangen, dass Sie mit der Entwicklung per Ruby vertraut sind, aber noch keine Erfahrung mit Azure-Datenbank für MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a>Installieren von Ruby
Installieren Sie Ruby, Gem und die MySQL2-Bibliothek auf Ihrem eigenen Computer. 

### <a name="windows"></a>Windows
1. Laden Sie Version 2.3 von [Ruby](http://rubyinstaller.org/downloads/) herunter, und installieren Sie sie.
2. Starten Sie über das Menü „Start“ eine neue Eingabeaufforderung (cmd).
3. Wechseln Sie in das Ruby-Verzeichnis für Version 2.3. `cd c:\Ruby23-x64\bin`
4. Überprüfen Sie die Ruby-Installation, indem Sie den Befehl `ruby -v` ausführen, um die installierte Version anzuzeigen.
5. Überprüfen Sie die Gem-Installation, indem Sie den Befehl `gem -v` ausführen, um die installierte Version anzuzeigen.
6. Erstellen Sie das Mysql2-Modul für Ruby, indem Sie Gem mit dem Befehl `gem install mysql2` ausführen.

### <a name="macos"></a>macOS
1. Installieren Sie Ruby per Homebrew, indem Sie den Befehl `brew install ruby` ausführen. Weitere Installationsoptionen finden Sie in der [Installationsdokumentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew) zu Ruby.
2. Überprüfen Sie die Ruby-Installation, indem Sie den Befehl `ruby -v` ausführen, um die installierte Version anzuzeigen.
3. Überprüfen Sie die Gem-Installation, indem Sie den Befehl `gem -v` ausführen, um die installierte Version anzuzeigen.
4. Erstellen Sie das Mysql2-Modul für Ruby, indem Sie Gem mit dem Befehl `gem install mysql2` ausführen.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Installieren Sie Ruby, indem Sie den Befehl `sudo apt-get install ruby-full` ausführen. Weitere Installationsoptionen finden Sie in der [Installationsdokumentation](https://www.ruby-lang.org/en/documentation/installation/) zu Ruby.
2. Überprüfen Sie die Ruby-Installation, indem Sie den Befehl `ruby -v` ausführen, um die installierte Version anzuzeigen.
3. Installieren Sie die neuesten Updates für Gem, indem Sie den Befehl `sudo gem update --system` ausführen.
4. Überprüfen Sie die Gem-Installation, indem Sie den Befehl `gem -v` ausführen, um die installierte Version anzuzeigen.
5. Installieren Sie gcc, make und andere Buildtools, indem Sie den Befehl `sudo apt-get install build-essential` ausführen.
6. Installieren Sie die Entwicklerbibliotheken für den MySQL-Client, indem Sie den Befehl `sudo apt-get install libmysqlclient-dev` ausführen.
7. Erstellen Sie das mysql2-Modul für Ruby, indem Sie Gem mit dem Befehl `sudo gem install mysql2` ausführen.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem soeben erstellten Server (beispielsweise **myserver4demo**).
3. Klicken Sie auf den Servernamen **myserver4demo**.
4. Wählen Sie die Seite mit den **Eigenschaften** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Azure-Datenbank für MySQL – Anmeldename des Serveradministrators](./media/connect-ruby/1_server-properties-name-login.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

## <a name="run-ruby-code"></a>Ausführen von Ruby-Code 
1. Fügen Sie den Ruby-Code aus den Abschnitten unten in Textdateien ein, und speichern Sie die Dateien in einem Projektordner mit der Dateierweiterung „.rb“, z.B. `C:\rubymysql\createtable.rb` oder `/home/username/rubymysql/createtable.rb`.
2. Starten Sie zum Ausführen des Codes die Eingabeaufforderung oder die Bash-Shell. Wechseln Sie mit `cd rubymysql` in das Verzeichnis Ihres Projektordners.
3. Geben Sie anschließend den Ruby-Befehl gefolgt vom Dateinamen ein, z.B. `ruby createtable.rb`, um die Anwendung auszuführen.
4. Unter Windows müssen Sie unter Umständen den vollständigen Pfad verwenden, um die Ruby-Anwendung zu starten, z.B. `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`, wenn sich die Ruby-Anwendung nicht unter dem Pfad der Umgebungsvariablen befindet.

## <a name="connect-and-create-a-table"></a>Herstellen einer Verbindung und Erstellen einer Tabelle
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und eine Tabelle zu erstellen, indem Sie eine **CREATE TABLE**-SQL-Anweisung gefolgt von **INSERT INTO**-SQL-Anweisungen zum Hinzufügen von Zeilen zur Tabelle nutzen.

Im Code wird eine .new()-Methode der [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-Klasse verwendet, um eine Verbindung mit der Azure-Datenbank für MySQL herzustellen. Anschließend wird mehrfach die [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage)-Methode aufgerufen, um die Befehle DROP, CREATE TABLE und INSERT INTO auszuführen. Anschließend wird die [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `username` und `password` durch Ihre eigenen Werte. 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. 

Im Code wird eine .new()-Methode der [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-Klasse verwendet, um eine Verbindung mit der Azure-Datenbank für MySQL herzustellen. Anschließend wird die [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage)-Methode aufgerufen, um die SELECT-Befehle auszuführen. Anschließend wird die [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `username` und `password` durch Ihre eigenen Werte. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten per **UPDATE**-SQL-Anweisung zu aktualisieren.

Im Code wird eine .new()-Methode der [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-Klasse verwendet, um eine Verbindung mit der Azure-Datenbank für MySQL herzustellen. Anschließend wird die [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage)-Methode aufgerufen, um die UPDATE-Befehle auszuführen. Anschließend wird die [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `username` und `password` durch Ihre eigenen Werte. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. 

Im Code wird eine .new()-Methode der [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-Klasse verwendet, um eine Verbindung mit der Azure-Datenbank für MySQL herzustellen. Anschließend wird die [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage)-Methode aufgerufen, um die DELETE-Befehle auszuführen. Anschließend wird die [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `username` und `password` durch Ihre eigenen Werte. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./concepts-migrate-import-export.md)

