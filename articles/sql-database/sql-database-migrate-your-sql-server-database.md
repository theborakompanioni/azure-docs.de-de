---
title: Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine SQL Server-Datenbank zu Azure SQL-Datenbank migrieren.
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/27/2017
ms.author: janeng
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 375d3ea0230e7d3fd0fc02ca7e0b8a7a76c24a27
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---

# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank

Das Verschieben Ihrer SQL Server-Datenbank zu Azure SQL-Datenbank ist ein dreiteiliger Prozess: Sie bereiten die Datenbank zunächst vor und exportieren und importieren sie dann. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten einer Datenbank in einer SQL Server-Instanz für die Migration zu Azure SQL-Datenbank per [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)
> * Exportieren der Datenbank in eine BACPAC-Datei
> * Importieren der BACPAC-Datei in eine Azure SQL-Datenbank

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

- Die neueste Version von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) muss installiert sein. Bei der Installation von SSMS wird auch die neueste Version von „SQLPackage“ installiert. Dies ist ein Befehlszeilenprogramm, mit dem eine Reihe von Datenbankentwicklungsaufgaben automatisiert werden kann. 
- Der [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) muss installiert sein.
- Sie haben die zu migrierende Datenbank angegeben, und haben Zugriff auf diese. In diesem Tutorial wird die [SQL Server 2008-OLTP-Datenbank „AdventureWorks2008R2“](https://msftdbprodsamples.codeplex.com/releases/view/59211) für eine Instanz von SQL Server 2008R2 oder höher verwendet, Sie können aber jede Datenbank Ihrer Wahl verwenden. Um Kompatibilitätsprobleme zu beheben, verwenden Sie [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).

## <a name="prepare-for-migration"></a>Vorbereiten der Migration

Sie können nun die Schritte zum Vorbereiten auf die Migration ausführen. Führen Sie die folgenden Schritte aus, um mit dem  **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)**  eine Bewertung (Assessment) vorzunehmen, ob Ihre Datenbank für die Migration zu Azure SQL-Datenbank geeignet ist.

1. Öffnen Sie den **Data Migration Assistant**. Sie können DMA auf jedem Computer ausführen, der eine Verbindung mit der SQL Server-Instanz hat, die die Datenbank enthält, die Sie migrieren möchten. Sie müssen DMA nicht auf dem Computer installieren, der die SQL Server-Instanz hostet.

     ![Öffnen des Data Migration Assistant](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Klicken Sie im linken Menü auf **+ Neu**, um ein **Assessment**-Projekt zu erstellen. Geben Sie einen Namen in das Formularfeld **Project name** ein (für alle anderen Felder sollten Sie deren Standardwerte übernehmen), und klicken Sie auf **Create**. Die Seite **Options** wird geöffnet.

     ![Neues Data Migration Assistant-Projekt](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. Klicken Sie auf der Seite **Options** auf **Weiter**. Die Seite **Select sources** wird geöffnet.

     ![Optionen für neue Datenmigration](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. Geben Sie auf der Seite **Select sources** den Namen der SQL Server-Instanz ein, die den Server enthält, den Sie migrieren möchten. Ändern Sie ggf. die anderen Werte auf dieser Seite. Klicken Sie auf **Verbinden**.

     ![Quellen für neue Datenmigration auswählen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. Aktivieren Sie auf der Seite **Select sources** im Bereich **Add sources** die Kontrollkästchen für die Datenbanken, die auf Kompatibilität geprüft werden sollen. Klicken Sie auf **Hinzufügen**.

     ![Quellen für neue Datenmigration auswählen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. Klicken Sie auf **Start Assessment**.

     ![Bewertung für neue Datenmigration starten](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. Wenn die Bewertung abgeschlossen ist, prüfen Sie zunächst, ob die Datenbank ausreichend kompatibel ist, um migriert werden zu können. Suchen Sie nach dem Häkchen in einem grünen Kreis.

     ![Neue Datenmigration – Bewertungsergebnisse – kompatibel](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. Überprüfen Sie die Ergebnisse. Die angezeigten Ergebnisse für die **SQL Server-Featureübereinstimmung** sind die zu prüfenden Standardergebnisse. Insbesondere sollten Sie die Informationen über nicht unterstützte und teilweise unterstützte Features sowie die bereitgestellten Informationen zu empfohlenen Aktionen überprüfen. 

     ![Bewertung für neue Datenmigration – Featureübereinstimmung](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. Überprüfen Sie die **Kompatibilitätsprobleme**, indem Sie oben links auf die entsprechende Option klicken. Überprüfen Sie speziell die Informationen über Migrationsblocker, Verhaltensänderungen und veraltete Features für jeden Kompatibilitätsgrad. Überprüfen Sie für die Datenbank „AdventureWorks2008R2“ die Änderungen an der Volltextsuche seit SQL Server 2008 und die Änderungen an SERVERPROPERTY('LCID') seit SQL Server 2000. Für ausführliche Informationen zu diesen Änderungen werden entsprechende Links bereitgestellt. Viele Suchoptionen und -einstellungen für die Volltextsuche wurden geändert. 

   > [!IMPORTANT] 
   > Nachdem Sie Ihre Datenbank zu Azure SQL-Datenbank migriert haben, können Sie wählen, die Datenbank mit deren aktuellem Kompatibilitätsgrad (Ebene 100 für die Datenbank „AdventureWorks2008R2“) oder mit einem höheren Grad auszuführen. Weitere Informationen zu den Auswirkungen und Optionen für das Ausführen einer Datenbank mit einem bestimmten Kompatibilitätsgrad finden Sie unter [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lesen Sie auch [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql). Dort finden Sie Informationen zu weiteren Einstellungen auf Datenbankebene, die sich auf Kompatibilitätsgrade beziehen.
   >

10. Klicken Sie optional auf **Export report**, um den Bericht als JSON-Datei zu speichern.
11. Schließen Sie den Data Migration Assistant.

## <a name="export-to-bacpac-file"></a>Exportieren in eine BACPAC-Datei 

Eine BACPAC-Datei ist eine ZIP-Datei mit der Erweiterung BACPAC. Sie enthält die Metadaten und Daten aus einer SQL Server-Datenbank. Eine BACPAC-Datei kann in Azure Blob Storage oder im lokalen Speicher zur Archivierung oder Migration (wie von SQL Server zu Azure SQL-Datenbank) gespeichert werden. Damit ein Export hinsichtlich der Transaktionen konsistent ist, müssen Sie sicherstellen, dass während des Exports keine Schreibaktivitäten stattfinden.

Führen Sie die folgenden Schritte aus, um die Datenbank „AdventureWorks2008R2“ mit dem Befehlszeilenprogramm „SQLPackage“ in lokalen Speicher zu exportieren.

1. Öffnen Sie eine Windows-Eingabeaufforderung, und navigieren Sie zu dem Ordner, in dem sich die **130**-Version von „SQLPackage“ befindet, z.B. **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.

2. Führen Sie den folgenden SQLPackage-Befehl an der Eingabeaufforderung aus, um die Datenbank **AdventureWorks2008R2** von **localhost** in **AdventureWorks2008R2.bacpac** zu exportieren. Ändern Sie diese Werte entsprechend Ihrer Umgebung.

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![Exportieren mit SQLPackage](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

Sobald die Ausführung abgeschlossen ist, wird die generierte BACPAC-Datei in dem Verzeichnis gespeichert, in dem sich die ausführbare Datei „sqlpackage“ befindet. In diesem Beispiel ist das „C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin“. 

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an. Melden Sie sich von dem Computer an, auf dem Sie das Befehlszeilenprogramm „SQLPackage“ ausführen. Dies vereinfacht die Erstellung der Firewallregel in Schritt 5.

## <a name="create-a-sql-server-logical-server"></a>Erstellen eines logischen SQL Servers

Ein [logischer SQL Server](sql-database-features.md) fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. Führen Sie die folgenden Schritte aus, um einen logischen SQL Server zu erstellen, der die migrierte Adventure Works-OLTP-SQL Server-Datenbank enthalten soll. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Geben Sie im Suchfenster **sql server** auf der Seite **Neu** ein, und wählen Sie in der gefilterten Liste den Eintrag **SQL database (logical server)** (SQL-Datenbank (logischer Server) aus.

    ![Auswählen des logischen Servers](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. Klicken Sie auf der Seite **Alles** auf **SQL server (logical server)**, und klicken Sie dann auf der Seite **SQL Server (logical server)** auf **Create**.

    ![Erstellen des logischen Servers](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. Geben Sie die folgenden Informationen in das Formular „SQL server (logical server)“ ein, wie in der obigen Abbildung dargestellt:     

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servername** | Eingeben eines global eindeutigen Namens | Gültige Servernamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). | 
   | **Serveradministratoranmeldung** | Eingeben eines gültigen Namens | Gültige Anmeldenamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner). |
   | **Kennwort** | Eingeben eines gültigen Kennworts | Ihr Kennwort muss mindestens acht Zeichen umfassen und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen. |
   | **Abonnement** | Auswählen eines Abonnements | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Gruppe wie z.B. **myResourceGroup**. |  Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Standort** | Eingeben eines gültigen Speicherorts für den neuen Server | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |

   ![Ausgefülltes Formular für Erstellen eines logischen Servers](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. Klicken Sie auf **Erstellen**, um den logischen Server bereitzustellen. Die Bereitstellung dauert einige Minuten. 

> [!IMPORTANT]
> Merken Sie sich den Servernamen, die Serveradministratoranmeldung und das Kennwort. Sie benötigen sie später in diesem Tutorial.
>

## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine [Firewall auf Serverebene](sql-database-firewall-configure.md), um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die folgenden Schritte aus, um eine SQL-Datenbank-Firewallregel auf Serverebene für die IP-Adresse des Computers zu erstellen, auf dem Sie das Befehlszeilenprogramm „SQLPackage“ ausführen. Dadurch kann SQLPackage über die Azure SQL-Datenbank-Firewall eine Verbindung mit dem logischen SQL Datenbank-Server herstellen. 

1. Klicken Sie im linken Menü auf **Alle Ressourcen**, und klicken Sie auf der Seite **Alle Ressourcen** auf den neuen Server. Die Übersichtsseite für Ihren Server wird geöffnet und enthält Optionen für die weitere Konfiguration.

     ![Übersicht über logischen Server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Klicken Sie auf der Übersichtsseite im linken Menü unter **Einstellungen** auf **Firewall**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

3. Klicken Sie auf der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um die IP-Adresse des Computers hinzuzufügen, den Sie derzeit verwenden. Klicken Sie dann auf **Speichern**. Eine Firewallregel auf Serverebene wird für diese IP-Adresse erstellt.

     ![Festlegen der Serverfirewallregel](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. Klicken Sie auf **OK**.

Nun können Sie über diese IP-Adresse Verbindungen mit allen Datenbanken auf diesem Server herstellen. Verwenden Sie hierzu SQL Server Management Studio oder ein anderes Tool Ihrer Wahl sowie das zuvor erstellte Serveradministratorkonto.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

## <a name="import-a-bacpac-file-to-azure-sql-database"></a>Importieren einer BACPAC-Datei in Azure SQL-Datenbank 

Die neuesten Versionen des Befehlszeilenprogramms „SQLPackage“ unterstützen das Erstellen einer Azure SQL-Datenbank auf einer angegebenen [Dienstebene und Leistungsstufe](sql-database-service-tiers.md). Damit beim Importieren die beste Leistung erzielt wird, wählen Sie eine hohe Dienstebene und Leistungsstufe aus. Nach dem Import können Sie zentral herunterskalieren, wenn die Dienstebene und die Leistungsstufe höher sind, als dies derzeit erforderlich ist.

Führen Sie die folgenden Schritte aus, um die Datenbank „AdventureWorks2008R2“ mit dem Befehlszeilenprogramm „SQLPackage“ in Azure SQL-Datenbank zu importieren. Sie können SQL Server Management Studio zwar für diese Aufgabe verwenden, SQLPackage wird jedoch für die meisten Produktionsumgebungen bevorzugt, damit eine maximale Flexibilität und eine gute Leistung sichergestellt ist. Weitere Informationen dazu finden Sie unter [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) (Migrieren von SQL Server zu Azure SQL-Datenbank mithilfe von BACPAC-Dateien).

- Führen Sie den folgenden SQLPackage-Befehl an der Eingabeaufforderung aus, um die Datenbank **AdventureWorks2008R2** aus dem lokalen Speicher in den logischen SQL Server zu importieren, den Sie zuvor in eine neue Datenbank mit der Dienstebene **Premium** und dem Dienstziel **P6** erstellt haben. Ersetzen Sie die Werte in spitzen Klammern durch die entsprechenden Werte für Ihren logischen SQL Server, und geben Sie einen Namen für die neue Datenbank ein (ersetzen Sie auch die spitzen Klammern). Sie können auch die Werte für die Datenbankedition und das Dienstziel durch die Werte Ihrer Umgebung ersetzen. In diesem Tutorial hat die migrierte Datenbank den Namen **myMigratedDatabase**.

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![Importieren mit SQLPackage](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> Ein logischer SQL Server lauscht auf Port 1433. Wenn Sie versuchen, durch eine Unternehmensfirewall eine Verbindung mit einem logischen SQL Server herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit der Vorgang erfolgreich ist.
>

## <a name="connect-using-sql-server-management-studio-ssms"></a>Herstellen einer Verbindung mithilfe von SQL Server Management Studio (SSMS)

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit Ihrem Azure SQL-Datenbankserver und der neu migrierten Datenbank herzustellen, die in diesem Tutorial den Namen **myMigratedDatabase** hat. Wenn Sie SQL Server Management Studio nicht auf dem Computer ausführen, der mit dem identisch ist, auf dem Sie „SQLPackage“ ausgeführt haben, erstellen Sie eine Firewallregel für diesen Computer (siehe die Schritte im vorherigen Verfahren).

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:
   - **Servertyp**: Geben Sie das Datenbankmodul ein.
   - **Servername**: Geben Sie den vollqualifizierten Servernamen ein, z.B. **mynewserver20170403.database.windows.net**.
   - **Authentifizierung**: Geben Sie die SQL Server-Authentifizierung an.
   - **Anmeldung**: Geben Sie Ihr Serveradministratorkonto ein.
   - **Kennwort**: Geben Sie das Kennwort für Ihr Serveradministratorkonto ein.
 
    ![Verbinden mit SSMS](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. Klicken Sie auf **Verbinden**. Die Objekt-Explorer-Fenster wird geöffnet. 

4. Erweitern Sie im Objekt-Explorer die Option **Datenbanken** und anschließend den Eintrag **myMigratedDatabase**, um die Objekte in der Beispieldatenbank anzuzeigen.

## <a name="change-database-properties"></a>Ändern von Datenbankeigenschaften

Sie können die Dienstebene, die Leistungsstufe und den Kompatibilitätsgrad über SQL Server Management Studio ändern. Es empfiehlt sich, dass Sie während des Importierens eine Datenbank mit einer höheren Leistungsstufe importieren, um die bestmögliche Leistung zu erreichen. Skalieren Sie diese nach dem abgeschlossenen Import jedoch herunter, um Geld zu sparen, bis Sie die importierte Datenbank aktiv verwenden können. Durch Ändern des Kompatibilitätsgrads erreichen Sie eine bessere Leistung und erhalten Zugriff auf die neuesten Funktionen des Azure SQL-Datenbankdiensts. Wenn Sie eine ältere Datenbank migrieren, bleibt ihr Datenbankkompatibilitätsgrad auf der niedrigsten Ebene, die mit der zu importierenden Datenbank kompatibel ist. Weitere Informationen finden Sie unter [Improved query performance with compatibility Level 130 in Azure SQL Database](sql-database-compatibility-level-query-performance-130.md) (Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank).

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **myMigratedDatabase**, und klicken Sie auf **Neue Abfrage**. Ein Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.

2. Führen Sie folgenden Befehl aus, um die auf die Dienstebene auf **Standard** und die Leistungsstufe auf **S1** festzulegen.

    ```
    ALTER DATABASE myMigratedDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

    ![Ändern der Dienstebene](./media/sql-database-migrate-your-sql-server-database/service-tier.png)

3. Führen Sie den folgenden Befehl aus, um den Datenbankkompatibilitätsgrad in **130** zu ändern.

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Ändern des Kompatibilitätsgrads](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a>Nächste Schritte 
In diesem Tutorial haben Sie Ihre Datenbank vorbereitet, exportiert und importiert. Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Vorbereiten der Datenbank einer SQL Server-Instanz für die Migration zu Azure SQL-Datenbank
> * Exportieren der Datenbank in eine BACPAC-Datei
> * Importieren der BACPAC-Datei in eine Azure SQL-Datenbank

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Datenbank schützen.

> [!div class="nextstepaction"]
> [Schützen der Azure SQL-Datenbank](sql-database-security-tutorial.md)



