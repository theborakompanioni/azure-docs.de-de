---
title: "Implementieren einer mehrinstanzenfähigen SaaS-Anwendung mit Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Implementieren Sie eine mehrinstanzenfähige SaaS-Anwendung mit Azure SQL-Datenbank."
services: sql-database
documentationcenter: 
author: AyoOlubeko
manager: jhubbard
editor: monicar
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial-develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/08/2017
ms.author: AyoOlubek
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 80df7b504d13fe1b3be9806eb95e3980d7790970
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="implement-a-multi-tenant-saas-application-using-azure-sql-database"></a>Implementieren einer mehrinstanzenfähigen SaaS-Anwendung mit Azure SQL-Datenbank

Eine mehrinstanzenfähige Anwendung ist eine Anwendung, die in einer Cloudumgebung gehostet wird und die gleichen Dienste für Hunderte oder Tausende von Mandanten bereitstellt, die ihre Daten weder untereinander freigeben noch die Daten anderer Mandanten sehen. Ein Beispiel ist eine SaaS-Anwendung, die Dienste für Mandanten in einer in der Cloud gehosteten Umgebung bereitstellt. Dieses Modell isoliert die Daten für jeden Mandanten und optimiert die Verteilung von Ressourcen für Kosten. 

Dieses Tutorial veranschaulicht das Erstellen einer mehrinstanzenfähigen SaaS-Anwendung mithilfe von Azure SQL-Datenbank.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten einer Datenbankumgebung zur Unterstützung einer mehrinstanzenfähigen SaaS-Anwendung nach dem Datenbank-pro-Mandant-Muster
> * Erstellen eines Mandantenkatalogs
> * Bereitstellen einer Mandantendatenbank und deren Registrierung im Mandantenkatalog
> * Einrichten einer Java-Beispielanwendung 
> * Zugriff auf Mandantendatenbanken mit einer einfachen Java-Konsolenanwendung
> * Löschen eines Mandanten

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten:
* PowerShell und das [aktuelle Azure PowerShell-SDK](http://azure.microsoft.com/downloads/) müssen auf Ihrem Computer installiert sein.

* Die aktuelle Version von [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Bei der Installation von SQL Server Management Studio wird auch die aktuelle Version von „SQLPackage“ installiert. Dies ist ein Befehlszeilenprogramm, mit dem eine Reihe von Datenbankentwicklungsaufgaben automatisiert werden kann.

* [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) und das [aktuelle JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) müssen auf Ihrem Computer installiert sein. 

* [Apache Maven](https://maven.apache.org/download.cgi) muss auf Ihrem Computer installiert sein. Maven dient zum einfacheren Verwalten von Abhängigkeiten sowie Erstellen, Testen und Ausführen Ihres Java-Beispielprojekts.

## <a name="set-up-data-environment"></a>Einrichten der Datenumgebung

Sie werden eine Datenbank pro Mandant bereitstellen. Das Datenbank-pro-Mandant-Modell bietet den höchsten Grad an Isolation zwischen Mandanten bei geringen DevOps-Kosten. Um die Kosten von Cloudressourcen zu optimieren, stellen Sie die Mandantendatenbanken auch in einem elastischen Pool bereit, sodass Sie die Preisleistung für eine Gruppe von Datenbanken optimieren können. Informationen zu anderen Datenbankbereitstellungs-Modellen [finden Sie hier](sql-database-design-patterns-multi-tenancy-saas-applications.md#multitenant-data-models). 

Führen Sie diese Schritte zum Erstellen einer SQL Server-Instanz und eines elastischen Pools aus, der alle Ihre Mandantendatenbanken hostet. 

1. Erstellen Sie Variablen zum Speichern von Werten, die im weiteren Verlauf des Tutorials verwendet werden. Stellen Sie sicher, dass die IP-Adressvariable Ihre IP-Adresse enthält. 
   
   ```PowerShell 
   # Set an admin login and password for your database
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   
   # Create random unique names for logical server and tenants
   $servername = "server-$(Get-Random)"
   $tenant1 = "geolamice"
   $tenant2 = "ranplex"
   
   # Store current client IP address (modify to include your IP address)
   $startIpAddress = 0.0.0.0 
   $endIpAddress = 0.0.0.1
   ```
   
2. Melden Sie sich bei Azure an, und erstellen Sie eine SQL Server-Instanz und elastische Pools. 
   
   ```PowerShell
   # Login to Azure 
   Login-AzureRmAccount
   
   # Create resource group 
   New-AzureRmResourceGroup -Name "myResourceGroup" -Location "northcentralus"
   
   # Create logical SQL Server with firewall rules 
   New-AzureRmSqlServer -ResourceGroupName "myResourceGroup" `
       -ServerName $servername `
       -Location "northcentralus" `
       -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   
   New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
       -ServerName $servername `
       -FirewallRuleName "singleAddress" -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
   
   # Create elastic pool 
   New-AzureRmSqlElasticPool -ResourceGroupName "myResourceGroup"
       -ServerName $servername `
       -ElasticPoolName "myElasticPool" `
       -Edition "Standard" `
       -Dtu 50 `
       -DatabaseDtuMin 10 `
       -DatabaseDtuMax 20
   ```
   
## <a name="create-tenant-catalog"></a>Erstellen des Mandantenkatalogs 

In einer mehrinstanzenfähigen SaaS-Anwendung ist es wichtig zu wissen, wo Informationen über einen Mandanten gespeichert sind. Sie sind häufig in einer Katalogdatenbank gespeichert. Die Katalogdatenbank wird verwendet, um eine Zuordnung zwischen einem Mandanten und einer Datenbank zu speichern, in der die Daten dieses Mandanten gespeichert sind.  Das grundlegende Muster gilt unabhängig davon, ob eine mehrinstanzenfähige oder eine Einzelmandanten-Datenbank verwendet wird.

Führen Sie diese Schritte aus, um eine Katalogdatenbank für die SaaS-Beispielanwendung zu erstellen.

```PowerShell
# Create empty database in pool
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "tenantCatalog" `
    -ElasticPoolName "myElasticPool"

# Create table to track mapping between tenants and their databases
$commandText = "
CREATE TABLE Tenants
(
   TenantId         INT IDENTITY PRIMARY KEY,
   TenantName       NVARCHAR(128) NOT NULL,
   TenantDatabase   NVARCHAR(128) NOT NULL
);

CREATE INDEX IX_TenantName ON Tenants (TenantName);"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant1-and-register-in-tenant-catalog"></a>Bereitstellen der Datenbank für „tenant1“ und Registrierung im Mandantenkatalog 
Stellen Sie mit Powershell eine Datenbank für einen neuen Mandanten „tenant1“ bereit, und registrieren Sie diesen Mandanten im Katalog. 

```PowerShell
# Create empty database in pool for 'tenant1'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into the table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant1');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant1 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register 'tenant1' in the tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant1', '$tenant1');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant2-and-register-in-tenant-catalog"></a>Bereitstellen der Datenbank für „tenant2“ und Registrierung im Mandantenkatalog
Stellen Sie mit Powershell eine Datenbank für einen neuen Mandanten „tenant2“ bereit, und registrieren Sie diesen Mandanten im Katalog. 

```PowerShell
# Create empty database in pool for 'tenant2'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant2 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into the table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant2');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant2 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register tenant 'tenant2' in the tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant2', '$tenant2');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="set-up-sample-java-application"></a>Einrichten einer Java-Beispielanwendung 

1. Erstellen Sie ein Maven-Projekt. Geben Sie in einem Eingabeaufforderungsfenster Folgendes ein:
   
   ```
   mvn archetype:generate -DgroupId=com.microsoft.sqlserver -DartifactId=mssql-jdbc -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
   
2. Fügen Sie diese Abhängigkeit, Sprachebene und Buildoption zur Unterstützung von Manifestdateien in JAR-Dateien der Datei „pom.xml“ hinzu:
   
   ```XML
   <dependency>
         <groupId>com.microsoft.sqlserver</groupId>
         <artifactId>mssql-jdbc</artifactId>
         <version>6.1.0.jre8</version>
   </dependency>
   
   <properties>
         <maven.compiler.source>1.8</maven.compiler.source>
         <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   
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

3. Fügen Sie Folgendes in der Datei „App.java“ hinzu:

   ```java 
   package com.sqldbsamples;
   
   import java.util.Map;
   
   import java.util.HashMap;
   
   import java.io.BufferedReader;
   
   import java.io.InputStreamReader;
   
   import java.sql.Connection;
   
   import java.sql.Statement;
   
   import java.sql.PreparedStatement;
   
   import java.sql.ResultSet;
   
   import java.sql.DriverManager;
   
   public class App {
   
   private static final String SERVER_NAME = "your-server-name";
   
   private static final String CATALOG_DB_NAME = "tenantCatalog";
   
   private static final String USER = "ServerAdmin";
   
   private static final String PASSWORD = "ChangeYourAdminPassword1";
   
   private static final String CATALOG_DB_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, CATALOG_DB_NAME, USER, PASSWORD);
   
   private static final String CMD_LIST = "LIST";

   private static final String CMD_QUERY = "QUERY";

   private static final String CMD_QUIT = "QUIT";
   
   public static void main(String[] args) {
   
   System.out.println("\n############################");
   
   System.out.println("## SAAS DATABASE TUTORIAL ##");
   
   System.out.println("############################\n");
   
   System.out.println("OPTIONS");
   
   System.out.println(" " + CMD_LIST + " - list tenants");
   
   System.out.println(" " + CMD_QUERY + " <NAME> - connect and tenant query tenant <NAME>");
   
   System.out.println(" " + CMD_QUIT + " - quit the application\n");
   
   try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
   
   while(true) {
   
   String[] input = br.readLine().split("\\s");
   
   if (null != input && input.length > 0) {
   
   if (input[0].equalsIgnoreCase(CMD_LIST)) {
   
   listTenants();
   
   } else if (input[0].toLowerCase().startsWith(CMD_QUERY.toLowerCase()) && input.length == 2) {
   
   queryTenant(input[1].trim());
   
   } else if (input[0].equalsIgnoreCase(CMD_QUIT)) {
   
   break;
   
   } else {
   
   System.out.println(" -> Command not supported");
   
   }
   
   }
   
   System.out.println("");
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void listTenants() {
   
   // List all tenants that currently exist in the system
   
   String sql = "SELECT TenantName FROM Tenants";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   Statement stmt = connection.createStatement();
   
   ResultSet resultSet = stmt.executeQuery(sql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void queryTenant(String name) {
   
   // Query the data that was previously inserted into the primary database from the geo replicated database
   
   String url = null;
   
   String sql = "SELECT TenantDatabase FROM Tenants WHERE TenantName = ?";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   PreparedStatement pstmt = connection.prepareStatement(sql)) {
   
   pstmt.setString(1, name);
   
   try (ResultSet resultSet = pstmt.executeQuery()) {
   
   if (resultSet.next()) {
   
   url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, resultSet.getString(1), USER, PASSWORD);
   
   }
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   if (null != url) {
   
   String tenantSql = "SELECT TenantName FROM WhoAmI";
   
   try (Connection connection = DriverManager.getConnection(url);
   
   Statement stmt = connection.createStatement();

   ResultSet resultSet = stmt.executeQuery(tenantSql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> Entry in table WhoAmI in tenant " + name + " is: " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   } else {
   
   System.out.println(" -> Tenant " + name + " not found");
   
   }
   
   }
   
   }
   ```

4. Speichern Sie die Datei.

5. Wechseln Sie zur Befehlskonsole, und führen Sie Folgendes aus:

   ```bash
   mvn package
   ```

6. Führen Sie die Anwendung anschließend folgendermaßen aus: 
   
   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```
   
Bei erfolgreicher Ausführung sieht die Ausgabe wie folgt aus:

```
############################

## SAAS DATABASE TUTORIAL ##

############################

OPTIONS

LIST - list tenants

QUERY <NAME> - connect and tenant query tenant <NAME>

QUIT - quit the application

* List the tenants

* Query tenants you created
```

## <a name="delete-first-tenant"></a>Löschen des ersten Mandanten 
Löschen Sie mit PowerShell die Mandantendatenbank und den Katalogeintrag für den ersten Mandanten.

```PowerShell
# Remove 'tenant1' from catalog 
$commandText = "DELETE FROM Tenants WHERE TenantName = '$tenant1';"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Delete database 
Remove-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1
```

Versuchen Sie, mit der Java-Anwendung eine Verbindung mit „tenant1“ herzustellen. Sie erhalten eine Fehlermeldung, dass der Mandant nicht vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte 
In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Einrichten einer Datenbankumgebung zur Unterstützung einer mehrinstanzenfähigen SaaS-Anwendung nach dem Datenbank-pro-Mandant-Muster
> * Erstellen eines Mandantenkatalogs
> * Bereitstellen einer Mandantendatenbank und deren Registrierung im Mandantenkatalog
> * Einrichten einer Java-Beispielanwendung 
> * Zugriff auf Mandantendatenbanken mit einer einfachen Java-Konsolenanwendung
> * Löschen eines Mandanten

* PowerShell-Beispiele für gängige Aufgaben finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples).

* Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen finden Sie unter [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications).

* Java-Beispiele für allgemeine Azure-Aufgaben finden Sie unter [Java Developer Center](https://azure.microsoft.com/develop/java/).




