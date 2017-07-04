---
title: Herstellen einer Verbindung mit Azure SQL-Datenbank mithilfe von Java | Microsoft-Dokumentation
description: "Zeigt ein Java-Codebeispiel an, das Sie zum Herstellen einer Verbindung mit Azure SQL-Datenbank und Senden von entsprechenden Abfragen verwenden können."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/23/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 63ab7345b70d456d9d1ad23255d9ca7e777e161d
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="azure-sql-database-use-java-to-connect-and-query-data"></a>Azure SQL-Datenbank: Verwenden von Java zum Herstellen einer Verbindung und Abfragen von Daten

In diesem Schnellstart wird veranschaulicht, wie Sie [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) nutzen, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen. Anschließend können Sie Transact-SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank über MacOS-, Windows- und Ubuntu Linux-Plattformen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart werden als Ausgangspunkt die Ressourcen verwendet, die in einem der folgenden Schnellstarts erstellt wurden:

- [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
- [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)
- [Erstellen einer Datenbank – PowerShell](sql-database-get-started-powershell.md)

## <a name="install-java-software"></a>Installieren von Java-Software

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Java-Entwicklung vertraut sind und noch keine Erfahrung mit Azure SQL-Datenbank haben. Falls Sie noch keine Erfahrung mit der Java-Entwicklung haben, navigieren Sie zu [Erstellen einer App mithilfe von SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/), und wählen Sie erst **Java** und dann Ihr Betriebssystem aus.

### <a name="mac-os"></a>**Mac OS**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Java-Projekt erstellen möchten. Installieren Sie **brew** und **Maven**, indem Sie die folgenden Befehle eingeben: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Java-Projekt erstellen möchten. Installieren Sie **Maven**, indem Sie die folgenden Befehle eingeben:

```bash
sudo apt-get install maven
```

### <a name="windows"></a>**Windows**
Installieren Sie [Maven](https://maven.apache.org/download.cgi) unter Verwendung des offiziellen Installers. Verwenden Sie Maven zum einfacheren Verwalten von Abhängigkeiten sowie zum Erstellen, Testen und Ausführen Ihres Java-Projekts. 

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank erforderlich sind. In den weiteren Verfahren benötigen Sie den vollqualifizierten Servernamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Überprüfen Sie auf der Seite **Übersicht** für Ihre Datenbank den vollqualifizierten Servernamen wie in der Abbildung unten dargestellt. Sie können auf den Servernamen zeigen, um die Option **Klicken Sie zum Kopieren** anzuzeigen. 

   ![Servername](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Falls Sie die Anmeldeinformationen für Ihren Server vergessen, können Sie zur Seite des SQL-Datenbankservers navigieren, um den Serveradministrator-Benutzernamen anzuzeigen und ggf. das Kennwort zurückzusetzen.
5. Klicken Sie auf **Datenbankverbindungszeichenfolgen anzeigen**.

6. Überprüfen Sie die vollständige **JDBC**-Verbindungszeichenfolge.

    ![Verbindungszeichenfolge für JDBC](./media/sql-database-connect-query-jdbc/jdbc-connection-string.png)   

### <a name="create-maven-project"></a>**Erstellen eines Maven-Projekts**
Erstellen Sie über das Terminal ein neues Maven-Projekt. 
```bash
mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
```

Fügen Sie den **Microsoft JDBC-Treiber für SQL Server** zu den Abhängigkeiten in ***pom.xml*** hinzu. 

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.0.jre8</version>
</dependency>
```

## <a name="select-data"></a>Auswählen von Daten

Verwenden Sie den folgenden Code, um mit der Transact-SQL-Anweisung [SELECT](https://docs.microsoft.com/sql/connect/jdbc/working-with-a-connection) und der Klasse [connection](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) die 20 wichtigsten Produkte nach Kategorie abzufragen. Ersetzen Sie die Parameter „hostHame“, „dbName“, „user“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

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
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
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
                
                try (Statement statement = connection.createStatement();
                    ResultSet resultSet = statement.executeQuery(selectSql)) {

                        // Print results from select statement
                        System.out.println("Top 20 categories:");
                        while (resultSet.next())
                        {
                            System.out.println(resultSet.getString(1) + " "
                                + resultSet.getString(2));
                        }
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

## <a name="insert-data"></a>Einfügen von Daten

Verwenden Sie den folgenden Code zum Einfügen eines neuen Produkts in die Tabelle „SalesLT.Product“ mithilfe der Klasse [Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) und der Transact-SQL-Anweisung [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Ersetzen Sie die Parameter „hostHame“, „dbName“, „user“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Insert data example:");
                System.out.println("=========================================");

                // Prepared statement to insert data
                String insertSql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, " 
                    + " StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

                java.util.Date date = new java.util.Date();
                java.sql.Timestamp sqlTimeStamp = new java.sql.Timestamp(date.getTime());

                try (PreparedStatement prep = connection.prepareStatement(insertSql)) {
                        prep.setString(1, "BrandNewProduct");
                        prep.setInt(2, 200989);
                        prep.setString(3, "Blue");
                        prep.setDouble(4, 75);
                        prep.setDouble(5, 80);
                        prep.setTimestamp(6, sqlTimeStamp);

                        int count = prep.executeUpdate();
                        System.out.println("Inserted: " + count + " row(s)");
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```
## <a name="update-data"></a>Aktualisieren von Daten

Verwenden Sie den folgenden Code zum Aktualisieren des zuvor hinzugefügten neuen Produkts in Ihrer Azure SQL-Datenbank mithilfe der Klasse [Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) und der Transact-SQL-Anweisung [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Ersetzen Sie die Parameter „hostHame“, „dbName“, „user“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
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

                try (PreparedStatement prep = connection.prepareStatement(updateSql)) {
                        prep.setString(1, "500");
                        prep.setString(2, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Updated: " + count + " row(s)")
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```



## <a name="delete-data"></a>Löschen von Daten

Verwenden Sie den folgenden Code, um mit den [vorbereiteten Anweisungen](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) und der Transact-SQL-Anweisung [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) das neue Produkt zu löschen, das Sie zuvor hinzugefügt haben. Ersetzen Sie die Parameter „hostHame“, „dbName“, „user“ und „password“ durch die Werte, die Sie angegeben haben, als Sie die Datenbank mit den AdventureWorksLT-Beispieldaten erstellt haben. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
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

                try (PreparedStatement prep = connection.prepareStatement(deleteSql)) {
                        prep.setString(1, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Deleted: " + count + " row(s)");
                }
        }       
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Microsoft JDBC Driver für SQL Server](https://github.com/microsoft/mssql-jdbc)
- [Melden von Problemen/Stellen von Fragen](https://github.com/microsoft/mssql-jdbc/issues)


