---
title: "Replikation und Failover Ihrer Azure SQL-Datenbank-Lösung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre Azure SQL-Datenbank-Instanz und die Anwendung für ein Failover zu einer replizierten Datenbank und zum Testen des Failovers konfigurieren."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jstrauss
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial-failover
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/18/2017
ms.author: sashan
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 7e5b82da402ab9d20410746c2b6c6b3aaab5b754
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="replicate-and-failover-an-azure-sql-database-solution"></a>Replikation und Failover einer Azure SQL-Datenbank-Lösung

In diesem Tutorial konfigurieren Sie eine Azure SQL-Datenbank-Instanz und Anwendung für das Failover zu einer Remoteregion und testen dann Ihren Failoverplan. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten:

- Die neueste Version von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Bei der Installation von SSMS wird auch die neueste Version von „SQLPackage“ installiert. Dies ist ein Befehlszeilenprogramm, mit dem eine Reihe von Datenbankentwicklungsaufgaben automatisiert werden kann. 
- Eine zu migrierende Azure-Datenbank. Dieses Tutorial verwendet die AdventureWorksLT-Beispieldatenbank mit dem Namen „**mySampleDatabase“ aus einem der folgenden Schnellstarts:

   - [Erstellen einer Datenbank – Portal](sql-database-get-started-portal.md)
   - [Erstellen einer Datenbank – CLI](sql-database-get-started-cli.md)

## <a name="create-azure-active-directory-users-optional"></a>Erstellen eines Azure Active Directory-Benutzers (optional)

In diesem Schritt erstellen oder identifizieren Sie Azure Active Directory-Benutzer, um sie als Benutzer dem logischen Server Ihrer Azure SQL-Datenbank-Instanz und der Beispieldatenbank hinzufügen.
- Wenn Ihr Abonnement Teil einer Azure Active Directory-Unternehmensumgebung mit vorhandenen Benutzerkonten ist, identifizieren Sie 3 Benutzerkonten zur Verwendung als Active Directory-Administrator, Anwendungsadministrator und Anwendungsbenutzer für dieses Tutorial, und fahren Sie fort mit Schritt 3: Erstellen von SQL-Datenbank-Anmeldenamen und -Benutzern. 
- Wenn Ihr Abonnement nicht Teil einer Azure Active Directory-Unternehmensumgebung ist, oder wenn es Teil einer Azure Active Directory-Unternehmensumgebung ohne vorhandene Benutzerkonten ist (und Sie über die Berechtigung zum Erstellen neuer Azure Active Directory-Benutzerkonten verfügen):

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **Weitere Dienste**.
3. Geben Sie im Filtertextfeld **Azure** ein, und wählen Sie dann **Azure Active Directory**.
4. Klicken Sie im Bereich **Schnelle Aufgaben** auf der Seite **Azure Active Directory** auf **Benutzer hinzufügen**.
5. Erstellen Sie im Formular **Benutzer** den folgenden Benutzer.
   - Name: **ad-admin**
   - Benutzername: **ad-admin@yourdomain** (Yopu4708)
6. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie das Kennwort für dieses Benutzerkonto zur späteren Verwendung.
7. Klicken Sie auf **Erstellen**.
8. Wiederholen Sie die 3 vorherigen Schritte, um die folgenden 2 neuen Benutzer zu erstellen.
   - Name: **app-admin**
   - Benutzername: **app-admin@yourdomain** (Buju4319)
   - Name: **app-user**
   - Benutzername: **app-user@yourdomain** (Nonu4001).

9. Öffnen Sie ein neues Browserfenster, und melden Sie sich beim Azure-Portal mit dem neu erstellten Konto **ad-admin** an.
10. Geben Sie auf der Seite **Ändern Sie Ihr Kennwort** das vom System generierte Kennwort in das Feld **Aktuelles Kennwort** ein. 
11. Geben Sie in die Felder **Neues Kennwort** und **Kennwort bestätigen** ein Kennwort Ihrer Wahl ein.
12. Klicken Sie auf **Kennwort ändern und anmelden**.

## <a name="configure-sql-database-integration-with-azure-active-directory"></a>Konfigurieren der SQL-Datenbank-Integration mit Azure Active Directory

1. Klicken Sie im linken Menü auf **Weitere Dienste**, geben Sie im Filtertextfeld **sql** ein, und wählen Sie dann **SQL Server**.
2. Klicken Sie auf der Seite **SQL Server** auf Ihre SQL-Datenbankserver-Instanz.
3. Klicken Sie Im Bereich „Essentials“ der Seite **Übersicht** für den Server unter **Active Directory-Administrator** auf **Nicht konfiguriert**.
4. Klicken Sie auf der Seite **Active Directory-Administrator** auf **Administrator festlegen**.
5. Wählen Sie das Azure Active Directory-Konto **ad-admin** (oder ein anderes bereits vorhandenes Konto, z.B. Ihr eigenes Konto) als Serveradministrator für die Azure SQL-Datenbankserver-Instanz aus.
6. Klicken Sie auf **Auswählen**.
7. Klicken Sie auf **Speichern**.

## <a name="create-users-with-permissions-for-your-database"></a>Erstellen von Benutzern mit Berechtigungen für Ihre Datenbank erstellen

<TO DO: need to change script to grant app-user sufficient permissions to perform operation in java app>

Verwenden Sie SQL Server Management Studio zum Herstellen einer Verbindung mit Ihrer Datenbank und zum Erstellen von Benutzerkonten. Diese Benutzerkonten werden automatisch auf den sekundären Server repliziert. Sie müssen möglicherweise eine Firewallregel konfigurieren, wenn Sie eine Verbindung mit einem Client unter einer IP-Adresse herstellen, für die Sie noch keine Firewall konfiguriert haben. Die entsprechenden Schritte finden Sie unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](sql-database-get-started-portal.md).

1. Öffnen Sie SQL Server Management Studio.
2. Ändern Sie den **Authentifizierungs**-Modus in **Active Directory-Kennwortauthentifizierung**.
3. Stellen Sie mit dem neu erstellten Azure Active Directory-Serveradministratorkonto eine Verbindung mit Ihrem Server her. 
4. Blenden Sie im Objekt-Explorer **Systemdatenbanken** ein, klicken Sie mit der rechten Maustaste auf **mySampleDatabase**, und klicken Sie dann auf **Neue Abfrage**.
5. Führen Sie im Abfragefenster die folgende Abfrage aus, um Benutzerkonten in Ihrer Datenbank zu erstellen, wobei Sie den beiden Administratorkonten **db_owner**-Berechtigungen erteilen. Ersetzen Sie den Platzhalter für den Domänennamen durch Ihre Domäne.

   ```tsql
   --Create Azure AD user account
   CREATE USER [app-admin@<yourdomain>] FROM EXTERNAL PROVIDER;
   --Add Azure AD user to db_owner role
   ALTER ROLE db_owner ADD MEMBER [app-admin@<yourdomain>]; 
   --Create additional Azure AD user account
   CREATE USER [app-user@<yourdomain>] FROM EXTERNAL PROVIDER;
   --Create SQL user account
   CREATE USER app_admin WITH PASSWORD = 'MyStrongPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'MyStrongPassword1';
   ```

## <a name="create-database-level-firewall"></a>Erstellen einer Firewall auf Datenbankebene

Verwenden Sie SQL Server Management Studio, um eine Firewallregel auf Datenbankebene für die Datenbank zu erstellen. Diese Firewallregel auf Datenbankebene wird automatisch auf den sekundären Server repliziert. Zu Testzwecken können Sie eine Firewallregel für alle IP-Adressen (0.0.0.0 und 255.255.255.255) erstellen, eine Firewallregel für die einzelnen IP-Adressen erstellen, mit denen Sie die Server-Firewallregel erstellt haben, oder Sie können eine oder mehrere Firewallregeln für die IP-Adressen der Computer konfigurieren, die Sie zum Testen dieses Tutorials verwenden möchten.  

- Ersetzen Sie im geöffneten Abfragefenster die vorherige Abfrage durch die folgende Abfrage, wobei Sie die IP-Adressen durch die entsprechenden IP-Adressen für Ihre Umgebung ersetzen. 

   ```tsql
   -- Create database-level firewall setting for your publich IP address
   EXECUTE sp_set_database_firewall_rule N'mySampleDatabase','0.0.0.1','0.0.0.1';
   ```  

## <a name="create-a-failover-group"></a>Erstellen einer Failovergruppe 

Wählen Sie eine Failoverregion, erstellen Sie in dieser Region einen leeren Server, und erstellen Sie dann eine Failovergruppe zwischen Ihrem vorhandenen Server und dem neuen leeren Server.

1. Füllen Sie die Variablen.

   ```powershell
   $secpasswd = ConvertTo-SecureString "yourstrongpassword" -AsPlainText -Force
   $mycreds = New-Object System.Management.Automation.PSCredential (“ServerAdmin”, $secpasswd)
   $myresourcegroup = "<your resource group>"
   $mylocation = "<resource group location>"
   $myserver = "<your existing server>"
   $mydatabase = "<your existing database>"
   $mydrlocation = "<your disaster recovery location>"
   $mydrserver = "<your disaster recovery server>"
   $myfailovergroup = "<your failover group"
   ```

2. Erstellen Sie einen leeren Sicherungsserver in Ihrer Failoverregion.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroup -Location $mydrlocation -ServerName $mydrserver -ServerVersion "12.0" -SqlAdministratorCredentials $mycreds
   ```

3. Erstellen Sie eine Failovergruppe.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup –ResourceGroupName $myresourcegroup -ServerName "$myserver" -PartnerServerName $mydrserver  –FailoverGroupName $myfailovergroupname –FailoverPolicy "Automatic" -GracePeriodWithDataLossHours 2
   ```

4. Hinzufügen Ihrer Datenbank zur Failovergruppe

   ```powershell
   $mydrserver | Add-AzureRMSqlDatabaseToFailoverGroup –FailoverGroupName $myfailovergroup  -Database $mydatabase
   ```

## <a name="add-empty-backup-server-to-domain"></a>Hinzufügen des leeren Sicherungsservers zur Domäne

1. Klicken Sie im Azure-Portal im linken Menü auf **Weitere Dienste**, geben Sie im Filtertextfeld **sql** ein, und wählen Sie dann **SQL Server**.
2. Klicken Sie auf der Seite **SQL Server** auf Ihren neuen SQL-Datenbank-Notfallwiederherstellungsserver.
3. Klicken Sie Im Bereich „Essentials“ der Seite **Übersicht** für den Server unter **Active Directory-Administrator** auf **Nicht konfiguriert**.
4. Klicken Sie auf der Seite **Active Directory-Administrator** auf **Administrator festlegen**.
5. Wählen Sie das Azure Active Directory-Konto **ad-admin** (oder ein anderes bereits vorhandenes Konto, z.B. Ihr eigenes Konto) als Serveradministrator für die neue Azure SQL-Datenbankserver-Instanz zur Notfallwiederherstellung aus.
6. Klicken Sie auf **Auswählen**.
7. Klicken Sie auf **Speichern**.

## <a name="prepare-client-tier"></a>Vorbereiten der Clientebene

1. Erstellen Sie das TM-Profil mit Failoverprofil AWProfile.
2. Konfigurieren der Überwachung

   ```powershell
   $profile = New-AzureRmTrafficManagerProfile -Name AWProfile -ResourceGroupName MYRG -TrafficRoutingMethod Failover -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
   $webapp1 = Get-AzureRMWebApp -Name WebappUSWest
   Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
   $webapp2 = Get-AzureRMWebApp -Name WebappUSEast
   Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
   Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
   ```

## <a name="deploy-java-application-and-connect-to-database"></a>Bereitstellen der Java-Anwendung und Herstellen einer Verbindung mit der Datenbank

<In progress> Siehe [Azure SQL-Datenbank: Verwenden von Java zum Herstellen einer Verbindung und Abfragen von Daten](sql-database-connect-query-java.md).

<TO DO: change user to app-user>

1. Installieren Sie Java 8.
2. Installieren Sie Maven.
3. Erstellen Sie ein Maven-Projekt.
4. Fügen Sie der Datei „pom.xml“ folgenden Code hinzu: 

   - Abhängigkeit

      ```java
      <dependency>
         <groupId>com.microsoft.sqlserver</groupId>
         <artifactId>mssql-jdbc</artifactId>
         <version>6.1.0.jre8</version>
       </dependency>
      ```
   - Sprachebene

      ```java
      <properties>
         <maven.compiler.source>1.8</maven.compiler.source>
         <maven.compiler.target>1.8</maven.compiler.target>
      </properties>
      ```

   - Buildoption zur Unterstützung von Manifestdateien in JAR-Dateien

      ```java
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
5. Fügen Sie Folgendes in der Datei „App.java“ hinzu:

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

      private static final String PRIMARY_HOST_HAME = "your_primary_server_name";
      private static final String SECONDARY_HOST_NAME = "your_secondary_server_name";
      private static final String PRIMARY_HOST_HAME = "janengsampleserver";
      private static final String SECONDARY_HOST_NAME = PRIMARY_HOST_HAME;
    
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "ServerAdmin";
      private static final String PASSWORD = "ChangeYourAdminPassword1";

      private static final String PRIMARY_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", PRIMARY_HOST_HAME, DB_NAME, USER, PASSWORD);
      private static final String SECONDARY_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SECONDARY_HOST_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         // todo: get the max id from the table and initialize INSERT COUNTER with it so that the code will always run (avoid duplicate keys)  
         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1h
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

      try (Connection connection = DriverManager.getConnection(PRIMARY_URL); 
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

      try (Connection connection = DriverManager.getConnection(SECONDARY_URL); 
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
        
      try (Connection connection = DriverManager.getConnection(SECONDARY_URL); 
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
6. Speichern Sie die Datei.

## <a name="compile-and-run"></a>Kompilieren und Ausführen

1. Wechseln Sie zur Konsole und führen Sie Folgendes aus:

   ```java
   mvn package
   ```
2. Führen Sie nach Abschluss die Anwendung folgendermaßen aus (die Ausführung dauert ungefähr 1 Stunde, es sei denn, sie wird manuell beendet):

   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

   Bei erfolgreicher Ausführung sieht die Ausgabe wie folgt aus:

   #######################################
   ## <a name="geo-distributed-database-tutorial"></a>GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful

## <a name="perform-dr-drill"></a>Ausführen der DR-Übung

1. Rufen Sie ein manuelles Failover von FG mit einem erzwungenen Failover auf. Wenn Datenverlust während der Übung nicht akzeptabel ist, sollten Sie -AllowDataLoss entfernen.

   ```powershell
   $fg | Switch-AzureRMSqlDatabaseFailoverGroup -AllowDataLoss
   ```

2.    Deaktivieren Sie den primären Endpunkt im TM-Profil (um das Endpunktfailover auszulösen). 

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name webapp1ep -Type AzureEndpoints -ProfileName $profile.Name -ResourceGroupName MYRG -Force
   ```

3.    Führen Sie die Anwendung erneut aus.


## <a name="relocate-application-to-primary-region"></a>Verschieben der Anwendung in die primäre Region

1.    Rufen Sie ein manuelles freundliches Failover von FG auf. Geben Sie -AllowDataLoss nicht an.

   ```powershell
   $fg | Switch-AzureRMSqlDatabaseFailoverGroup 
   ```

2.    Deaktivieren Sie den sekundären Endpunkt (webapp2ep) im TM-Profil (um das Endpunktfailover auszulösen). 

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name webapp2ep -Type AzureEndpoints -ProfileName $profile.Name -ResourceGroupName MYRG -Force
   ```

3.    Führen Sie die Anwendung erneut aus.

## <a name="troubleshoot-failover"></a>Problembehandlung bei einem Failover 

Finden Sie heraus, welche Region jetzt primär ist, um sicherzustellen, dass das Failover durchgeführt wurde. „Role“ würde anzeigen, ob sie primär oder sekundär ist.

   ```powershell
   $fg = Get-AzureRMSqlDatabaseFailoverGroup -ResourceGroupName "myrg" -ServerName "AWserver" 
   print $fg.role
   ```

## <a name="next-steps"></a>Nächste Schritte 

- Bald in Ihrem Kino
