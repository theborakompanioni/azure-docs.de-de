---
title: "Herstellen einer Verbindung mit Azure-Datenbank für MySQL per C++ | Microsoft-Dokumentation"
description: "Diese Schnellstartanleitung enthält ein C++-Codebeispiel, mit dem Sie eine Verbindung mit den Daten aus Azure-Datenbank für MySQL herstellen und Daten abfragen können."
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: C++
ms.topic: hero-article
ms.date: 08/03/2017
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: 63388b83b913d95136140fa4c56af0dbebbdad81
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a>Azure-Datenbank für MySQL: Verwenden von Connector/C++ zum Herstellen einer Verbindung und zum Abfragen von Daten
In dieser Schnellstartanleitung erfahren Sie, wie Sie mit einer C++-Anwendung eine Verbindung mit einer Azure-Datenbank für MySQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der C++-Entwicklung vertraut sind und noch keine Erfahrung mit Azure-Datenbank für MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Außerdem benötigen Sie Folgendes:
- Installieren von [.NET Framework](https://www.microsoft.com/net/download)
- Installieren von [Visual Studio](https://www.visualstudio.com/downloads/)
- Installieren von [MySQL-Connector/C++](https://dev.mysql.com/downloads/connector/cpp/) 
- Installieren von [Boost](http://www.boost.org/)

## <a name="install-visual-studio-and-net"></a>Installieren von Visual Studio Code und .NET
Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Entwicklung per .NET vertraut sind.

### <a name="windows"></a>**Windows**
1. Installieren Sie Visual Studio 2017 Community. Hierbei handelt es sich um eine vollwertige, kostenlose und erweiterbare IDE zum Erstellen von modernen Anwendungen für Android, iOS und Windows sowie zum Erstellen von Web- und Datenbankanwendungen und Clouddiensten. Sie können entweder das vollständige .NET Framework oder nur .NET Core installieren. Die Codeausschnitte in der Schnellstartanleitung können für beides verwendet werden. Falls Visual Studio bereits auf Ihrem Computer installiert ist, können Sie die nächsten beiden Schritte überspringen.
   - Laden Sie das [Visual Studio 2017-Installationsprogramm](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) herunter. 
   - Führen Sie das Installationsprogramm aus, und befolgen Sie die Installationsanweisungen, um die Installation abzuschließen.

### <a name="configure-visual-studio"></a>**Konfigurieren von Visual Studio**
1. Fügen Sie in Visual Studio unter „Projekteigenschaft“ > „Konfigurationseigenschaften“ > „C/C++“ > „Linker“ > „Allgemein“ > „Zusätzliche Bibliotheksverzeichnisse“ das Verzeichnis „lib\opt“ (also „C:\Programme (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt“) des C++-Connectors hinzu.
2. Gehen Sie in Visual Studio unter „Projekteigenschaft“ > „Konfigurationseigenschaften“ > „C/C++“ > „Allgemein“ > „Zusätzliche Includeverzeichnisse“ wie folgt vor:
   - Fügen Sie das Verzeichnis „include/“ des C++-Connectors (also „C:\Programme (x86)\MySQL\MySQL Connector C++ 1.1.9\include“\) hinzu.
   - Fügen Sie das Stammverzeichnis der Boost-Bibliothek hinzu (also „C:\boost_1_64_0“\).
3. Fügen Sie in Visual Studio unter „Projekteigenschaft“ > „Konfigurationseigenschaften“ > „C/C++“ > „Linker“ > „Eingabe“ > „Zusätzliche Abhängigkeiten“ dem Textfeld die Bibliothek „mysqlcppconn.lib“ hinzu.
4. Kopieren Sie die Datei „mysqlcppconn.dll“ aus dem Bibliotheksordner des C++-Connectors in Schritt 3 in dasselbe Verzeichnis wie die ausführbare Datei der Anwendung, oder fügen Sie sie der Umgebungsvariablen hinzu, damit sie von Ihrer Anwendung gefunden werden kann.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen**, und suchen Sie nach dem soeben erstellten Server, z.B. **myserver4demo**.
3. Klicken Sie auf den Servernamen.
4. Wählen Sie die Seite mit den **Eigenschaften** des Servers aus. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators**.
 ![Servername für Azure-Datenbank für MySQL](./media/connect-cpp/1_server-properties-name-login.png)
5. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite **Übersicht** navigieren, um den Serveradministrator-Anmeldenamen anzuzeigen und ggf. das Kennwort zurückzusetzen.

## <a name="connect-create-table-and-insert-data"></a>Herstellen der Verbindung, Erstellen der Tabelle und Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und die Daten zu laden, indem Sie die SQL-Anweisungen **CREATE TABLE** und **INSERT INTO** verwenden. Im Code wird die sql::Driver-Klasse mit der connect()-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend werden im Code die Methoden „createStatement()“ und „execute()“ verwendet, um die Datenbankbefehle auszuführen. 

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a>Lesen von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer **SELECT**-SQL-Anweisung zu verbinden und zu lesen. Im Code wird die sql::Driver-Klasse mit der connect()-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend werden im Code die Methoden „prepareStatement()“ und „executeQuery()“ verwendet, um die Auswahlbefehle auszuführen. Zum Schluss wird im Code „next()“ verwendet, um zu den Datensätzen in den Ergebnissen zu gelangen. Danach werden im Code „getInt()“ und „getString()“ verwendet, um die Werte im Datensatz zu analysieren.

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **UPDATE**-SQL-Anweisung zu verbinden und zu lesen. Im Code wird die sql::Driver-Klasse mit der connect()-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend werden im Code die Methoden „prepareStatement()“ und „executeQuery()“ verwendet, um die Aktualisierungsbefehle auszuführen. 

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer **DELETE**-SQL-Anweisung zu verbinden und zu lesen. Im Code wird die sql::Driver-Klasse mit der connect()-Methode verwendet, um eine Verbindung mit MySQL herzustellen. Anschließend werden im Code die Methoden „prepareStatement()“ und „executeQuery()“ verwendet, um die Löschbefehle auszuführen.

Ersetzen Sie die Parameter „Host“, „DBName“, „User“ und „Password“ durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md)

