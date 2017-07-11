---
title: "Herstellen einer Verbindung mit Azure-Datenbank für PostgreSQL per Ruby | Microsoft-Dokumentation"
description: "Dieser Schnellstart enthält ein Ruby-Codebeispiel, das Sie nutzen können, um zu den Daten von Azure-Datenbank für PostgreSQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: ruby
ms.topic: hero-article
ms.date: 06/30/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: a04188aed40a46e8cb64dfeb1230fb9b3c6ab679
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---

<a id="azure-database-for-postgresql-use-ruby-to-connect-and-query-data" class="xliff"></a>

# Azure-Datenbank für PostgreSQL: Verwenden von Ruby zum Herstellen von Verbindungen mit Daten und Durchführen von Abfragen
Dieser Schnellstart zeigt, wie Sie mit einer [Ruby](https://www.ruby-lang.org)-Anwendung eine Verbindung mit einer Azure-Datenbank für PostgreSQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. In diesem Artikel wird davon ausgegangen, dass Sie mit der Entwicklung unter Verwendung der Sprache Ruby vertraut sind, aber noch keine Erfahrung mit Azure-Datenbank für PostgreSQL haben.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen einer Datenbank – Portal](quickstart-create-server-database-portal.md)
- [Erstellen einer Datenbank – Azure CLI](quickstart-create-server-database-azure-cli.md)

<a id="install-ruby" class="xliff"></a>

## Installieren von Ruby
Installieren Sie Ruby auf Ihrem eigenen Computer. 

<a id="windows" class="xliff"></a>

### Windows
- Laden Sie die aktuelle Version von [Ruby](http://rubyinstaller.org/downloads/) herunter, und installieren Sie sie.
- Aktivieren Sie auf dem Abschlussbildschirm des MSI-Installers das Kontrollkästchen „Run 'ridk install' to install MSYS2 and development toolchain“ („ridk install“ ausführen, um MSYS2 und die Entwicklungstoolkette zu installieren). Klicken Sie anschließend auf **Finish** (Fertig stellen), um den nächsten Installer zu starten.
- Der RubyInstaller2-Installer für Windows wird gestartet. Geben Sie „2“ ein, um das MSYS2-Repositoryupdate zu installieren. Schließen Sie das Befehlsfenster, nachdem der Vorgang abgeschlossen wurde und die Installationseingabeaufforderung wieder angezeigt wird.
- Starten Sie über das Menü „Start“ eine neue Eingabeaufforderung (cmd).
- Testen Sie die Ruby-Installation per `ruby -v`, um die installierte Version anzuzeigen.
- Testen Sie die Gem-Installation per `gem -v`, um die installierte Version anzuzeigen.
- Erstellen Sie das PostgreSQL-Modul für Ruby, indem Sie Gem mit dem Befehl `gem install pg` ausführen.

<a id="macos" class="xliff"></a>

### macOS
- Installieren Sie Ruby per Homebrew, indem Sie den Befehl `brew install ruby` ausführen. Weitere Installationsoptionen finden Sie in der [Installationsdokumentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew) zu Ruby.
- Testen Sie die Ruby-Installation per `ruby -v`, um die installierte Version anzuzeigen.
- Testen Sie die Gem-Installation per `gem -v`, um die installierte Version anzuzeigen.
- Erstellen Sie das PostgreSQL-Modul für Ruby, indem Sie Gem mit dem Befehl `gem install pg` ausführen.

<a id="linux-ubuntu" class="xliff"></a>

### Linux (Ubuntu)
- Installieren Sie Ruby, indem Sie den Befehl `sudo apt-get install ruby-full` ausführen. Weitere Installationsoptionen finden Sie in der [Installationsdokumentation](https://www.ruby-lang.org/en/documentation/installation/) zu Ruby.
- Testen Sie die Ruby-Installation per `ruby -v`, um die installierte Version anzuzeigen.
- Installieren Sie die neuesten Updates für Gem, indem Sie den Befehl `sudo gem update --system` ausführen.
- Testen Sie die Gem-Installation per `gem -v`, um die installierte Version anzuzeigen.
- Installieren Sie gcc, make und andere Buildtools, indem Sie den Befehl `sudo apt-get install build-essential` ausführen.
- Installieren Sie die PostgreSQL-Bibliotheken, indem Sie den Befehl `sudo apt-get install libpq-dev` ausführen.
- Erstellen Sie das Ruby-pg-Modul mithilfe von Gem, indem Sie den Befehl `sudo gem install pg` ausführen.

<a id="run-ruby-code" class="xliff"></a>

## Ausführen von Ruby-Code 
- Speichern Sie den Code in einer Textdatei, und speichern Sie die Datei in einem Projektordner mit der Dateierweiterung „.rb“, z.B. `C:\rubypostgres\read.rb` oder `/home/username/rubypostgres/read.rb`.
- Starten Sie zum Ausführen des Codes die Eingabeaufforderung oder die Bash-Shell. Wechseln Sie mit `cd rubypostgres` in Ihren Projektordner, und geben Sie dann den Befehl `ruby read.rb` ein, um die Anwendung auszuführen.

<a id="get-connection-information" class="xliff"></a>

## Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure-Datenbank für PostgreSQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem erstellten Server, z.B. **mypgserver-20170401**.
3. Klicken Sie auf den Servernamen **mypgserver-20170401**.
4. Wählen Sie die Seite **Übersicht** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Azure-Datenbank für PostgreSQL – Anmeldename des Serveradministrators](./media/connect-ruby/1-connection-string.png)
5. Falls Sie die Informationen für die Serveranmeldung vergessen haben, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen. Setzen Sie das Kennwort zurück, falls dies erforderlich ist.

<a id="connect-and-create-a-table" class="xliff"></a>

## Herstellen einer Verbindung und Erstellen einer Tabelle
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und eine Tabelle zu erstellen, indem Sie eine **CREATE TABLE**-SQL-Anweisung gefolgt von **INSERT INTO**-SQL-Anweisungen zum Hinzufügen von Zeilen zur Tabelle nutzen.

Der Code verwendet ein [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um die Befehle DROP, CREATE TABLE und INSERT INTO auszuführen. Der Code führt mit der [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

<a id="read-data" class="xliff"></a>

## Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. 

Der Code verwendet ein [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um den SELECT-Befehl auszuführen, und die Ergebnisse werden in einem Resultset vorgehalten. Die Resultset-Sammlung wird mit der Schleife `resultSet.each do` durchlaufen, wobei sich die aktuellen Zeilenwerte in der Variablen `row` befinden. Der Code führt mit der [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

<a id="update-data" class="xliff"></a>

## Aktualisieren von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten per **UPDATE**-SQL-Anweisung zu aktualisieren.

Der Code verwendet ein [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um den UPDATE-Befehl auszuführen. Der Code führt mit der [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


<a id="delete-data" class="xliff"></a>

## Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. 

Der Code verwendet ein [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection)-Objekt mit dem Konstruktor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize), um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen. Anschließend wird die [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method)-Methode aufgerufen, um den UPDATE-Befehl auszuführen. Der Code führt mit der [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error)-Klasse eine Überprüfung auf Fehler durch. Anschließend wird die [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)-Methode aufgerufen, um die Verbindung vor dem Beenden zu schließen.

Ersetzen Sie die Zeichenfolgen `host`, `database`, `user` und `password` durch Ihre eigenen Werte. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

