---
title: "Herstellen einer Verbindung mit Azure-Datenbank für MySQL per C# | Microsoft-Dokumentation"
description: "Dieser Schnellstart enthält ein C#-Codebeispiel (.NET), das Sie nutzen können, um zu den Daten von Azure-Datenbank für MySQL eine Verbindung herzustellen und Abfragen dafür durchzuführen."
services: MySQL
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: MySQL-database
ms.custom: mvc
ms.devlang: csharp
ms.topic: hero-article
ms.date: 07/10/2017
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: ffe3ae320a61031cf314cc1d70e0c093b033f85c
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="azure-database-for-mysql-use-net-c-to-connect-and-query-data"></a>Azure-Datenbank für MySQL: Verwenden von .NET (C#) zum Verbinden und Abfragen von Daten
Dieser Schnellstart zeigt, wie Sie mit einer C#-Anwendung eine Verbindung mit einer Azure-Datenbank für MySQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der C#-Entwicklung vertraut sind und noch keine Erfahrung mit Azure-Datenbank für MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installieren von [.NET Framework](https://www.microsoft.com/net/download)
- Installieren von [Visual Studio](https://www.visualstudio.com/downloads/)
- Installieren des [ODBC-Treibers für MySQL](https://dev.mysql.com/downloads/connector/odbc/) 

## <a name="install-visual-studio-and-net"></a>Installieren von Visual Studio Code und .NET
Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Entwicklung per .NET vertraut sind.

### <a name="windows-net-framework-and-net-core"></a>**Windows .NET Framework und .NET Core**
Visual Studio 2017 Community ist eine vollwertige, kostenlose und erweiterbare IDE zum Erstellen von modernen Anwendungen für Android, iOS und Windows sowie zum Erstellen von Web- und Datenbankanwendungen und Clouddiensten. Sie können entweder das vollständige .NET Framework oder nur .NET Core installieren. Die Codeausschnitte im Schnellstart funktionieren für beide Szenarien. Falls Visual Studio bereits auf Ihrem Computer installiert ist, überspringen Sie die nächsten Schritte.

1. Laden Sie das [Visual Studio 2017-Installationsprogramm](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) herunter. 
2. Führen Sie das Installationsprogramm aus, und befolgen Sie die Installationsanweisungen, um die Installation abzuschließen.

### <a name="mac-os"></a>**Mac OS**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr .NET Core-Projekt erstellen möchten. Geben Sie die folgenden Befehle ein, um **brew**, **OpenSSL** und **.NET Core** zu installieren. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

Installieren Sie .NET Core unter Mac OS. Laden Sie das [offizielle Installationsprogramm](https://go.microsoft.com/fwlink/?linkid=843444) herunter. Dieses Installationsprogramm installiert die Tools und platziert sie in Ihrem Pfad, sodass Sie .NET über die Konsole ausführen können.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr .NET Core-Projekt erstellen möchten. Geben Sie die folgenden Befehle ein, um **.NET Core** zu installieren.

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```


## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem soeben erstellten Server, z.B. **myserver4demo**.
3. Klicken Sie auf den Servernamen.
4. Wählen Sie die Seite mit den **Eigenschaften** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-csharp/1_server-properties-name-login.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten zu laden, indem Sie die SQL-Anweisungen **CREATE TABLE** und **INSERT INTO** verwenden. Im Code wird die ODBC-Klasse mit der [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend wird im Code die [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx)-Methode verwendet, die CommandText-Eigenschaft festgelegt und die [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx)-Methode aufgerufen, um die Datenbankbefehle auszuführen. 

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;

namespace driver
{
    class MySQLCreate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "DROP TABLE IF EXISTS inventory;";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished dropping table (if existed)");

            command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished creating table");

            command.CommandText =
                String.Format(
                    @"INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                    INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                    INSERT INTO inventory (name, quantity) VALUES ({4}, {5});",
                    "\'banana\'", 150,
                    "\'orange\'", 154,
                    "\'apple\'", 100
                    );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }

    }
}

```

## <a name="read-data"></a>Lesen von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. Im Code wird die ODBC-Klasse mit der [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend werden im Code die Methoden [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) und [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) verwendet, um die Datenbankbefehle auszuführen. Als Nächstes wird im Code [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) verwendet, um auf die Datensätze in den Ergebnissen zuzugreifen. Mithilfe von GetInt32 und GetString werden im Code dann die Werte im Datensatz analysiert.

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;


namespace driver
{
    class MySQLRead
    {

        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "SELECT * FROM inventory;";

            var reader = command.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(
                    string.Format(
                        "Reading from table=({0}, {1}, {2})",
                        reader.GetInt32(0).ToString(),
                        reader.GetString(1),
                        reader.GetInt32(2).ToString()
                        )
                    );
            }

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}


```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu verbinden und zu lesen. Im Code wird die ODBC-Klasse mit der [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend wird im Code die [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx)-Methode verwendet, die CommandText-Eigenschaft festgelegt und die [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx)-Methode aufgerufen, um die Datenbankbefehle auszuführen.

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLUpdate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("UPDATE inventory SET quantity = {0} WHERE name = {1};",
                200,
                "\'banana\'"
                );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}



```


## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. 

Im Code wird die ODBC-Klasse mit der [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx)-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend wird im Code die [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx)-Methode verwendet, die CommandText-Eigenschaft festgelegt und die [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx)-Methode aufgerufen, um die Datenbankbefehle auszuführen.

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLDelete
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
                String.Format("DELETE FROM inventory WHERE name = {0};",
                    "\'orange\'");
            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}

```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md)

