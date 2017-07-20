---
title: "Implementieren einer geografisch verteilten Azure SQL-Datenbanklösung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre Azure SQL-Datenbank-Instanz und die Anwendung für ein Failover zu einer replizierten Datenbank und zum Testen des Failovers konfigurieren."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/26/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 9f53f318e20dac9248906bdbe898ba4dacb286ac
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---

# <a name="implement-a-geo-distributed-database"></a>Implementieren einer geografisch verteilten Datenbank

In diesem Tutorial konfigurieren Sie eine Azure SQL-Datenbank-Instanz und Anwendung für das Failover zu einer Remoteregion und testen dann Ihren Failoverplan. Folgendes wird vermittelt: 

> [!div class="checklist"]
> * Erstellen von Datenbankbenutzern und Gewähren von Berechtigungen
> * Einrichten einer Firewallregel auf Datenbankebene
> * Erstellen einer [Georeplikations-Failovergruppe](sql-database-geo-replication-overview.md)
> * Erstellen und Kompilieren einer Java-Anwendung zum Abfragen einer Azure SQL-Datenbank
> * Durchführen eines Notfallwiederherstellungsverfahrens

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

- Sie müssen das neueste [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs) installiert haben. 
- Eine Azure SQL-Datenbank muss installiert sein. Dieses Tutorial verwendet die AdventureWorksLT-Beispieldatenbank mit dem Namen **mySampleDatabase** aus einem der folgenden Schnellstarts:

   - [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
   - [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)
   - [Erstellen einer Datenbank – PowerShell](sql-database-get-started-powershell.md)

- Sie müssen eine Methode zum Ausführen von SQL-Skripts für Ihre Datenbank bestimmt haben, bei der eines der folgenden Abfragetools verwendet werden kann:
   - Der Abfrage-Editor im [Azure-Portal](https://portal.azure.com). Weitere Informationen zum Verwenden des Abfrage-Editors im Azure-Portal finden Sie unter [Verbinden und Abfragen mit dem Abfrage-Editor](sql-database-get-started-portal.md#query-the-sql-database).
   - Die neueste Version von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), einer integrierten Umgebung für die Verwaltung beliebiger SQL-Infrastrukturen – von SQL Server bis SQL-Datenbank für Microsoft Windows.
   - Die neueste Version von [Visual Studio Code](https://code.visualstudio.com/docs), eines grafischer Code-Editors für Linux, macOS und Windows, der Erweiterungen unterstützt, z.B. die [mssql-Erweiterung](https://aka.ms/mssql-marketplace) zum Abfragen von Microsoft SQL Server, Azure SQL-Datenbank und SQL Data Warehouse. Weitere Informationen zum Verwenden dieses Tools mit Azure SQL-Datenbank finden Sie unter [Verbinden und Abfragen mit Visual Studio-Code](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Erstellen von Datenbankbenutzern und Gewähren von Berechtigungen

Stellen Sie eine Verbindung mit der Datenbank her, und erstellen Sie mithilfe eines der folgenden Abfragetools Benutzerkonten:

- Abfrage-Editor im Azure-Portal
- SQL Server Management Studio
- Visual Studio Code

Diese Benutzerkonten werden automatisch auf den sekundären Server repliziert (und laufend synchronisiert gehalten). Zum Verwenden von SQL Server Management Studio oder Visual Studio Code müssen Sie möglicherweise eine Firewallregel konfigurieren, wenn Sie eine Verbindung mit einem Client unter einer IP-Adresse herstellen, für die Sie noch keine Firewall konfiguriert haben. Die Schritte hierzu werden ausführlich unter [Erstellen einer Firewallregel auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) beschrieben.

- Führen Sie in einem Abfragefenster die folgende Abfrage aus, um zwei Benutzerkonten in der Datenbank zu erstellen. Das Skript gewährt dem **app_admin**-Konto **db_owner**-Berechtigungen und dem **app_user**-Konto **SELECT**- und **UPDATE**-Berechtigungen. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a>Erstellen einer Firewall auf Datenbankebene

Erstellen Sie eine [Firewallregel auf Datenbankebene](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) für Ihre SQL-Datenbank. Diese Firewallregel auf Datenbankebene wird automatisch auf den sekundären Server repliziert, den Sie in diesem Tutorial erstellen. Verwenden Sie zur Vereinfachung (in diesem Tutorial) die öffentliche IP-Adresse des Computers, auf dem Sie die Schritte des Tutorials ausführen. Eine Beschreibung, wie Sie die IP-Adresse für die Firewallregel auf Serverebene für den aktuellen Computer bestimmen, finden Sie unter [Erstellen einer Firewall auf Serverebene](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- Ersetzen Sie im geöffneten Abfragefenster die vorherige Abfrage durch die folgende Abfrage, wobei Sie die IP-Adressen durch die entsprechenden IP-Adressen für Ihre Umgebung ersetzen.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Erstellen einer Auto-Failovergruppe mit aktiver Georeplikation 

Erstellen Sie mit Azure PowerShell eine [Auto-Failovergruppe mit aktiver Georeplikation](sql-database-geo-replication-overview.md) zwischen dem vorhandenen Azure SQL-Server und dem neuen leeren Azure SQL Server in einer Azure-Region, und fügen Sie anschließend der Failovergruppe die Beispieldatenbank hinzu.

> [!IMPORTANT]
> Für diese Cmdlets wird Azure PowerShell 4.0 benötigt. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Füllen Sie die Variablen der PowerShell-Skripts mit den Werten für den vorhandenen Server und die Beispieldatenbank, und geben Sie einen global eindeutigen Namen für den Namen der Failovergruppe an.

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. Erstellen Sie einen leeren Sicherungsserver in Ihrer Failoverregion.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Erstellen Sie eine Failovergruppe zwischen den beiden Servern.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. Fügen Sie der Failovergruppe Ihre Datenbank hinzu.

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a>Installieren von Java-Software

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit der Java-Entwicklung vertraut sind und noch keine Erfahrung mit Azure SQL-Datenbank haben. 

### <a name="mac-os"></a>**Mac OS**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Java-Projekt erstellen möchten. Installieren Sie **brew** und **Maven**, indem Sie die folgenden Befehle eingeben: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Eine ausführliche Anleitung zum Installieren und Konfigurieren einer Java- und Maven-Umgebung finden Sie unter [Erstellen einer App mit SQL Server](https://www.microsoft.com/sql-server/developer-get-started/); wählen Sie dort **Java** und anschließend **MacOS** aus, und befolgen Sie dann die ausführlichen Anweisungen zum Konfigurieren von Java und Maven in Schritt 1.2 und 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öffnen Sie das Terminal, und navigieren Sie zu einem Verzeichnis, in dem Sie Ihr Java-Projekt erstellen möchten. Installieren Sie **Maven**, indem Sie die folgenden Befehle eingeben:

```bash
sudo apt-get install maven
```

Eine ausführliche Anleitung zum Installieren und Konfigurieren einer Java- und Maven-Umgebung finden Sie unter [Erstellen einer App mit SQL Server](https://www.microsoft.com/sql-server/developer-get-started/); wählen Sie dort **Java** und anschließend **Ubuntu** aus, und befolgen Sie dann die ausführlichen Anweisungen zum Konfigurieren von Java und Maven in Schritt 1.2, 1.3 und 1.4.

### <a name="windows"></a>**Windows**
Installieren Sie [Maven](https://maven.apache.org/download.cgi) unter Verwendung des offiziellen Installers. Verwenden Sie Maven zum einfacheren Verwalten von Abhängigkeiten sowie zum Erstellen, Testen und Ausführen Ihres Java-Projekts. Eine ausführliche Anleitung zum Installieren und Konfigurieren einer Java- und Maven-Umgebung finden Sie unter [Erstellen einer App mit SQL Server](https://www.microsoft.com/sql-server/developer-get-started/); wählen Sie dort **Java** und anschließend „Windows“ aus, und befolgen Sie dann die ausführlichen Anweisungen zum Konfigurieren von Java und Maven in Schritt 1.2 und 1.3.

## <a name="create-sqldbsample-project"></a>Erstellen eines SqlDbSample-Projekts

1. Erstellen Sie in einer Befehlskonsole (wie Bash) eine Maven-Projekt. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Geben Sie **Y** ein, und drücken Sie die **EINGABETASTE**.
3. Wechseln Sie das Verzeichnis und in Ihr neu erstelltes Projekt.

   ```bash
   cd SqlDbSamples
   ```

4. Öffnen Sie in Ihrem bevorzugten Editor die Datei „pom.xml“ im Projektordner. 

5. Fügen Sie dem Maven-Projekt die Abhängigkeit „Microsoft JDBC Driver for SQL Server“ hinzu, indem Sie Ihren bevorzugten Text-Editor öffnen, die folgenden Zeilen kopieren und in die Datei „pom.xml“ einfügen. Überschreiben Sie nicht die vorhandenen Werte, mit denen die Datei bereits gefüllt ist. Die JDBC-Abhängigkeit muss innerhalb des größeren Abschnitts „dependencies“ ( ) eingefügt werden.   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Geben Sie die Java-Version an, für die das Projekt kompiliert werden soll, indem Sie in der Datei „pom.xml“ nach dem Abschnitt „dependencies“ den folgenden Abschnitt „properties“ hinzufügen. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Fügen Sie in der Datei „pom.xml“ nach dem Abschnitt „properties“ den folgenden Abschnitt „build“ hinzu, um Manifestdateien in JARs zu unterstützen.       

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```
8. Speichern und schließen Sie die Datei „pom.xml“.
9. Öffnen Sie die Datei „App.java“ („C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java“), und ersetzen Sie ihren Inhalt durch den folgenden Inhalt. Ersetzen Sie den Namen der Failovergruppe durch den Namen für Ihre Failovergruppe. Wenn Sie die Werte für Datenbankname, Benutzer oder Kennwort geändert haben, müssen Sie auch diese Werte ändern.

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data that was previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
        
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. Speichern und schließen Sie die Datei „App.java“.

## <a name="compile-and-run-the-sqldbsample-project"></a>Kompilieren und Ausführen des SqlDbSample-Projekts

1. Führen Sie in der Befehlskonsole den folgenden Befehl aus.

   ```bash
   mvn package
   ```
2. Führen Sie anschließend den folgenden Befehl aus, um die Anwendung auszuführen (die Ausführung dauert ca. 1 Stunde, es sei denn, sie wird manuell beendet):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Durchführen eines Notfallwiederherstellungsverfahrens

1. Rufen Sie das manuelle Failover der Failovergruppe auf. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Beobachten Sie die Anwendungsergebnisse während des Failovers. Einige Einfügevorgänge schlagen während der Aktualisierung des DNS-Caches fehl.     

3. Ermitteln Sie, welche Rolle Ihr Notfallwiederherstellungsserver spielt.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Failback.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Beobachten Sie die Anwendungsergebnisse während des Failbacks. Einige Einfügevorgänge schlagen während der Aktualisierung des DNS-Caches fehl.     

6. Ermitteln Sie, welche Rolle Ihr Notfallwiederherstellungsserver spielt.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a>Nächste Schritte 

Weitere Informationen finden Sie unter [Aktive Georeplikation und Failovergruppen](sql-database-geo-replication-overview.md).

