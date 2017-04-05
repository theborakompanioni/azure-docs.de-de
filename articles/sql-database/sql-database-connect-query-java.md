---
title: Herstellen einer Verbindung mit Azure SQL-Datenbank mithilfe von Java | Microsoft-Dokumentation
description: Zeigt ein Java-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank.
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: quick start
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 03/27/2017
ms.author: andrela;carlrab;sstein
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: a047d4cdf869fff0d2afaf11f124370c0eae98e4
ms.lasthandoff: 03/30/2017


---
# <a name="azure-sql-database-use-java-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von Java zum Herstellen einer Verbindung und Abfragen von Daten

Verwenden Sie [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server), um eine Verbindung mit einer Azure SQL-Datenbank herzustellen und Abfragen dafür durchzuführen. In diesem Leitfaden wird beschrieben, wie Sie Java zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank verwenden und dann Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen ausführen.

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="configure-development-environment"></a>Konfigurieren der Entwicklungsumgebung

In den folgenden Abschnitten wird beschrieben, wie Sie Ihre vorhandenen Mac OS-, Linux- (Ubuntu) und Windows-Entwicklungsumgebungen für die Arbeit mit Azure SQL-Datenbank konfigurieren.

### <a name="mac-os"></a>**Mac OS**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Java-Projekt erstellen möchten. Geben Sie die folgenden Befehle ein, um **brew** und **Maven** zu installieren. 

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Java-Projekt erstellen möchten. Geben Sie die folgenden Befehle ein, um **Maven** zu installieren. 

```
sudo apt-get install maven
```

### <a name="windows"></a>**Windows**
Installieren Sie [Maven](https://maven.apache.org/download.cgi) unter Verwendung des offiziellen Installers.  

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungszeichenfolge im Azure-Portal ab. Sie verwenden die Verbindungszeichenfolge zum Herstellen einer Verbindung mit der Azure SQL-Datenbank.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie im Bereich **Zusammenfassung** für Ihre Datenbank den vollqualifizierten Servernamen. 

    <img src="./media/sql-database-connect-query-dotnet/server-name.png" alt="server name" style="width: 780px;" />

4. Klicken Sie auf **Datenbankverbindungszeichenfolgen anzeigen**.

5. Überprüfen Sie die vollständige **JDBC**-Verbindungszeichenfolge.

    <img src="./media/sql-database-connect-query-jdbc/jdbc-connection-string.png" alt="JDBC connection string" style="width: 780px;" />

### <a name="create-maven-project"></a>**Erstellen eines Maven-Projekts**
Erstellen Sie über das Terminal ein neues Maven-Projekt. 
```
mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
```

Fügen Sie den **Microsoft JDBC-Treiber für SQL Server** zu den Abhängigkeiten in ***pom.xml*** hinzu. 

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
</dependency>
```

## <a name="select-data"></a>Auswählen von Daten

Verwenden Sie eine [Verbindung](https://docs.microsoft.com/sql/connect/jdbc/working-with-a-connection) mit einer [SELECT](https://msdn.microsoft.com/library/ms189499.aspx)-Transact-SQL-Anweisung, um Daten in Ihrer Azure SQL-Datenbank abzufragen.

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.Statement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "yourserver";
        String dbName = "yourdatabase";
        String user = "yourusername";
        String password = "yourpassword";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                Statement statement = connection.createStatement();
                ResultSet resultSet = statement.executeQuery(selectSql);

                // Print results from select statement
                System.out.println("Top 20 categories:");
                while (resultSet.next())
                {
                    System.out.println(resultSet.getString(1) + " "
                        + resultSet.getString(2));
                }
            }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

## <a name="insert-data"></a>Einfügen von Daten

Verwenden Sie [vorbereitete Anweisungen](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) mit einer [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)-Transact-SQL-Anweisung, um Daten in Ihre Azure SQL-Datenbank einzufügen.

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "yourserver";
        String dbName = "yourdatabase";
        String user = "yourusername";
        String password = "yourpassword";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Insert data example:");
                System.out.println("=========================================");

                // Prepared statement to insert data
                String insertSql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, )" 
                    + " StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

                java.util.Date date = new java.util.Date();
                java.sql.Timestamp sqlTimeStamp = new java.sql.Timestamp(date.getTime());

                PreparedStatement prep = connection.prepareStatement(insertSql);
                prep.setString(1, "BrandNewProduct");
                prep.setInt(2, 200989);
                prep.setString(3, "Blue");
                prep.setDouble(4, 75);
                prep.setDouble(5, 80);
                prep.setTimestamp(6, sqlTimeStamp);

                int count = prep.executeUpdate();
                System.out.println("Inserted: " + count + " row(s)");
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```
## <a name="update-data"></a>Aktualisieren von Daten

Verwenden Sie [vorbereitete Anweisungen](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) mit einer [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)-Transact-SQL-Anweisung, um die Daten in Ihrer Azure SQL-Datenbank zu aktualisieren.

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "yourserver";
        String dbName = "yourdatabase";
        String user = "yourusername";
        String password = "yourpassword";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Update data example:");
                System.out.println("=========================================");

                // Prepared statement to update data
                String updateSql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?";

                PreparedStatement prep = connection.prepareStatement(updateSql);
                prep.setString(1, "500");
                prep.setString(2, "BrandNewProduct");

                int count = prep.executeUpdate();
                System.out.println("Updated: " + count + " row(s)")
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```



## <a name="delete-data"></a>Löschen von Daten

Verwenden Sie [vorbereitete Anweisungen](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) mit einer [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)-Transact-SQL-Anweisung, um die Daten in Ihrer Azure SQL-Datenbank zu aktualisieren.

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "yourserver";
        String dbName = "yourdatabase";
        String user = "yourusername";
        String password = "yourpassword";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Delete data example:");
                System.out.println("=========================================");

                // Prepared statement to delete data
                String deleteSql = "DELETE SalesLT.Product WHERE Name = ?";

                PreparedStatement prep = connection.prepareStatement(deleteSql);
                prep.setString(1, "BrandNewProduct");

                int count = prep.executeUpdate();
                System.out.println("Deleted: " + count + " row(s)");
            }        
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie den Artikel [SQL-Datenbankanwendungsentwicklung – Übersicht](sql-database-develop-overview.md).
* Erkunden Sie das GitHub-Repository für den [Microsoft-JDBC-Treiber für SQL Server](https://github.com/microsoft/mssql-jdbc).
* [Posten Sie Probleme, oder stellen Sie Fragen](https://github.com/microsoft/mssql-jdbc/issues).
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).


