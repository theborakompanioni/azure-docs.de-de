---
title: "Herstellen einer Verbindung mit Azure-Datenbank für PostgreSQL mit der Sprache Go | Microsoft-Dokumentation"
description: "Dieser Schnellstart enthält ein Beispiel für die Programmiersprache Go, das Sie nutzen können, um zu den Daten von Azure-Datenbank für PostgreSQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: go
ms.topic: hero-article
ms.date: 06/29/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: f3e7e57993b5253a895640854672da431c00854b
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---

<a id="azure-database-for-postgresql-use-go-language-to-connect-and-query-data" class="xliff"></a>

# Azure-Datenbank für PostgreSQL: Verwenden der Sprache Go zum Herstellen von Verbindungen mit Daten und Durchführen von Abfragen
Dieser Schnellstart zeigt, wie Sie mit Code, der in der Sprache [Go](https://golang.org/) geschrieben ist, eine Verbindung mit einer Azure-Datenbank für PostgreSQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. In diesem Artikel wird davon ausgegangen, dass Sie mit der Entwicklung unter Verwendung der Sprache Go vertraut sind, aber noch keine Erfahrung mit Azure-Datenbank für PostgreSQL haben.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen einer Datenbank – Portal](quickstart-create-server-database-portal.md)
- [Erstellen einer Datenbank – Azure CLI](quickstart-create-server-database-azure-cli.md)

<a id="install-go-and-pq" class="xliff"></a>

## Installieren von Go und pq
- Laden Sie Go gemäß der [Installationsanleitung](https://golang.org/doc/install) für Ihre Plattform herunter, und führen Sie die Installation durch.
- Erstellen Sie einen Ordner für Ihr Projekt, z.B. „C:\Postgresql\“. Wechseln Sie in der Befehlszeile zum Projektordner, z.B. mit `cd C:\Postgres\`.
- Laden Sie den [Pure Go Postgres-Treiber (pq)](https://github.com/lib/pq) in Ihren Projektordner herunter, indem Sie den Befehl `go get github.com/lib/pq` eingeben, während Sie sich in demselben Verzeichnis befinden.

<a id="build-and-run-go-code" class="xliff"></a>

## Erstellen und Ausführen von Go-Code 
- Speichern Sie den Code in einer Textdatei mit der Erweiterung „*.go“, und speichern Sie die Datei dann in Ihrem Projektordner, z.B. `C:\postgres\read.go`.
- Wechseln Sie zum Ausführen des Codes zu Ihrem Projektordner (`cd postgres`), und geben Sie dann den Befehl `go run read.go` ein, um die Anwendung zu kompilieren und auszuführen.
- Verwenden Sie `go build read.go`, um aus dem Code eine native Anwendung zu erstellen, und starten Sie anschließend „read.exe“, um die Anwendung auszuführen.

<a id="get-connection-information" class="xliff"></a>

## Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure-Datenbank für PostgreSQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem erstellten Server, z.B. **mypgserver-20170401**.
3. Klicken Sie auf den Servernamen **mypgserver-20170401**.
4. Wählen Sie die Seite **Übersicht** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Azure-Datenbank für PostgreSQL – Anmeldename des Serveradministrators](./media/connect-go/1-connection-string.png)
5. Falls Sie die Informationen für die Serveranmeldung vergessen haben, können Sie zur Seite **Übersicht** navigieren und den Serveradministrator-Anmeldenamen anzeigen. Setzen Sie das Kennwort zurück, falls dies erforderlich ist.

<a id="connect-and-create-a-table" class="xliff"></a>

## Herstellen einer Verbindung und Erstellen einer Tabelle
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und eine Tabelle zu erstellen, indem Sie eine **CREATE TABLE**-SQL-Anweisung gefolgt von **INSERT INTO**-SQL-Anweisungen zum Hinzufügen von Zeilen zur Tabelle nutzen.

Der Code importiert drei Pakete: das [sql-Paket](https://golang.org/pkg/database/sql/), das [pq-Paket](http://godoc.org/github.com/lib/pq) als Treiber für die Kommunikation mit dem Postgres-Server und das [fmt-Paket](https://golang.org/pkg/fmt/) für die Ein- und Ausgabe in der Befehlszeile.

Mit dem Code wird die [sql.Open()](http://godoc.org/github.com/lib/pq#Open)-Methode aufgerufen, um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen, und die Verbindung wird mit der [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping)-Methode überprüft. Ein [Datenbankhandle](https://golang.org/pkg/database/sql/#DB) wird durchgängig für den Verbindungspool des Datenbankservers eingesetzt. Im Code wird die [Exec()](https://golang.org/pkg/database/sql/#DB.Exec)-Methode mehrfach aufgerufen, um mehrere SQL-Befehle auszuführen. Es wird jedes Mal eine benutzerdefinierte checkError()-Methode verwendet, um das Auftreten eines Fehlers zu prüfen, und der Vorgang wird beendet, falls dies der Fall ist.


Ersetzen Sie die Parameter `HOST`, `DATABASE`, `USER` und `PASSWORD` durch Ihre eigenen Werte. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed)")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table")

    // Insert some data into table.
    sql_statement := "INSERT INTO inventory (name, quantity) VALUES ($1, $2);"
    _, err = db.Exec(sql_statement, "banana", 150)
    checkError(err)
    _, err = db.Exec(sql_statement, "orange", 154)
    checkError(err)
    _, err = db.Exec(sql_statement, "apple", 100)
    checkError(err)
    fmt.Println("Inserted 3 rows of data")
}
```

<a id="read-data" class="xliff"></a>

## Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. 

Der Code importiert drei Pakete: das [sql-Paket](https://golang.org/pkg/database/sql/), das [pq-Paket](http://godoc.org/github.com/lib/pq) als Treiber für die Kommunikation mit dem Postgres-Server und das [fmt-Paket](https://golang.org/pkg/fmt/) für die Ein- und Ausgabe in der Befehlszeile.

Mit dem Code wird die [sql.Open()](http://godoc.org/github.com/lib/pq#Open)-Methode aufgerufen, um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen, und die Verbindung wird mit der [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping)-Methode überprüft. Ein [Datenbankhandle](https://golang.org/pkg/database/sql/#DB) wird durchgängig für den Verbindungspool des Datenbankservers eingesetzt. Die SELECT-Abfrage wird ausgeführt, indem die [db.Query()](https://golang.org/pkg/database/sql/#DB.Query)-Methode aufgerufen wird, und die sich ergebenden Zeilen werden in einer Variablen vom Typ [rows](https://golang.org/pkg/database/sql/#Rows) (Zeilen) vorgehalten. Der Code liest die Spaltendatenwerte in der aktuellen Zeile mit der [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan)-Methode und führt eine Schleife für die Zeilen durch, indem der Iterator [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) verwendet wird, bis keine weiteren Zeilen mehr vorhanden sind. Die Spaltenwerte jeder Zeile werden auf der Konsole ausgegeben. Es wird jedes Mal eine benutzerdefinierte checkError()-Methode verwendet, um das Auftreten eines Fehlers zu prüfen, und der Vorgang wird beendet, falls dies der Fall ist.

Ersetzen Sie die Parameter `HOST`, `DATABASE`, `USER` und `PASSWORD` durch Ihre eigenen Werte. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Read rows from table.
    var id int
    var name string
    var quantity int

    sql_statement := "SELECT * from inventory;"
    rows, err := db.Query(sql_statement)
    checkError(err)

    for rows.Next() {
        switch err := rows.Scan(&id, &name, &quantity); err {
        case sql.ErrNoRows:
            fmt.Println("No rows were returned")
        case nil:
            fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
        default:
            checkError(err)
        }
    }
}
```

<a id="update-data" class="xliff"></a>

## Aktualisieren von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten per **UPDATE**-SQL-Anweisung zu aktualisieren.

Der Code importiert drei Pakete: das [sql-Paket](https://golang.org/pkg/database/sql/), das [pq-Paket](http://godoc.org/github.com/lib/pq) als Treiber für die Kommunikation mit dem Postgres-Server und das [fmt-Paket](https://golang.org/pkg/fmt/) für die Ein- und Ausgabe in der Befehlszeile.

Mit dem Code wird die [sql.Open()](http://godoc.org/github.com/lib/pq#Open)-Methode aufgerufen, um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen, und die Verbindung wird mit der [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping)-Methode überprüft. Ein [Datenbankhandle](https://golang.org/pkg/database/sql/#DB) wird durchgängig für den Verbindungspool des Datenbankservers eingesetzt. Im Code wird die [Exec()](https://golang.org/pkg/database/sql/#DB.Exec)-Methode aufgerufen, um die SQL-Anweisung auszuführen, mit der die Tabelle aktualisiert wird. Es wird eine benutzerdefinierte checkError()-Methode verwendet, um das Auftreten eines Fehlers zu prüfen, und der Vorgang wird beendet, falls dies der Fall ist.

Ersetzen Sie die Parameter `HOST`, `DATABASE`, `USER` und `PASSWORD` durch Ihre eigenen Werte. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

<a id="delete-data" class="xliff"></a>

## Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. 

Der Code importiert drei Pakete: das [sql-Paket](https://golang.org/pkg/database/sql/), das [pq-Paket](http://godoc.org/github.com/lib/pq) als Treiber für die Kommunikation mit dem Postgres-Server und das [fmt-Paket](https://golang.org/pkg/fmt/) für die Ein- und Ausgabe in der Befehlszeile.

Mit dem Code wird die [sql.Open()](http://godoc.org/github.com/lib/pq#Open)-Methode aufgerufen, um eine Verbindung mit Azure-Datenbank für PostgreSQL herzustellen, und die Verbindung wird mit der [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping)-Methode überprüft. Ein [Datenbankhandle](https://golang.org/pkg/database/sql/#DB) wird durchgängig für den Verbindungspool des Datenbankservers eingesetzt. Im Code wird die [Exec()](https://golang.org/pkg/database/sql/#DB.Exec)-Methode aufgerufen, um die SQL-Anweisung auszuführen, mit der die Tabelle aktualisiert wird. Es wird eine benutzerdefinierte checkError()-Methode verwendet, um das Auftreten eines Fehlers zu prüfen, und der Vorgang wird beendet, falls dies der Fall ist.

Ersetzen Sie die Parameter `HOST`, `DATABASE`, `USER` und `PASSWORD` durch Ihre eigenen Werte. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection to database")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)

