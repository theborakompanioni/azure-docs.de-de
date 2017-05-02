---
title: Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine SQL Server-Datenbank zu Azure SQL-Datenbank migrieren.
services: sql-database
documentationcenter: 
author: janeng
manager: jstrauss
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial-migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/04/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 842ca29e46aefbd58638ac642f000ef39c1202d1
ms.lasthandoff: 04/13/2017


---

# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank

In diesem Tutorial verwenden Sie den Microsoft Data Migration Assistant, um eine vorhandene SQL Server-Datenbank zu Azure SQL-Datenbank zu migrieren, und Sie durchlaufen die erforderlichen Schritte von der Vorbereitung für die Migration zum Durchführen der tatsächlichen Datenmigration und zum Herstellen der Verbindung mit der migrierten Datenbank, nachdem die Migration abgeschlossen ist. 

> [!IMPORTANT]
> Um Kompatibilitätsprobleme zu beheben, verwenden Sie [Visual Studio Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt). 
>

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten:

- Die neueste Version von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Bei der Installation von SSMS wird auch die neueste Version von „SQLPackage“ installiert. Dies ist ein Befehlszeilenprogramm, mit dem eine Reihe von Datenbankentwicklungsaufgaben automatisiert werden kann. 
- Den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Eine zu migrierende Datenbank. In diesem Tutorial wird die [SQL Server 2008-OLTP-Datenbank „AdventureWorks2008R2“](https://msftdbprodsamples.codeplex.com/releases/view/59211) für eine Instanz von SQL Server 2008R2 oder höher verwendet, Sie können aber jede Datenbank Ihrer Wahl verwenden. 

## <a name="step-1---prepare-for-migration"></a>Schritt 1: Vorbereiten auf die Migration

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

8. Überprüfen Sie die Ergebnisse. Beginnen Sie mit **SQL Server feature parity** (Featureübereinstimmung). Insbesondere sollten Sie die Informationen über nicht unterstützte und teilweise unterstützte Features sowie die bereitgestellten Informationen zu empfohlenen Aktionen überprüfen. 

     ![Bewertung für neue Datenmigration – Featureübereinstimmung](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. Klicken Sie auf **Compatibility issues**. Überprüfen Sie speziell die Informationen über Migrationsblocker, Verhaltensänderungen und veraltete Features für jeden Kompatibilitätsgrad. Überprüfen Sie für die Datenbank „AdventureWorks2008R2“ die Änderungen an der Volltextsuche seit SQL Server 2008 und die Änderungen an SERVERPROPERTY('LCID') seit SQL Server 2000. Für ausführliche Informationen zu diesen Änderungen werden entsprechende Links bereitgestellt. Viele Suchoptionen und -einstellungen für die Volltextsuche wurden geändert. 

   > [!IMPORTANT] 
   > Nachdem Sie Ihre Datenbank zu Azure SQL-Datenbank migriert haben, können Sie wählen, die Datenbank mit deren aktuellem Kompatibilitätsgrad (Ebene 100 für die Datenbank „AdventureWorks2008R2“) oder mit einem höheren Grad auszuführen. Weitere Informationen zu den Auswirkungen und Optionen für das Ausführen einer Datenbank mit einem bestimmten Kompatibilitätsgrad finden Sie unter [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lesen Sie auch [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql). Dort finden Sie Informationen zu weiteren Einstellungen auf Datenbankebene, die sich auf Kompatibilitätsgrade beziehen.
   >

10. Klicken Sie optional auf **Export report**, um den Bericht als JSON-Datei zu speichern.
11. Schließen Sie den Data Migration Assistant.

## <a name="step-2---export-to-bacpac-file"></a>Schritt 2: Exportieren in eine BACPAC-Datei 

Eine BACPAC-Datei ist eine ZIP-Datei mit der Erweiterung BACPAC. Sie enthält die Metadaten und Daten aus einer SQL Server-Datenbank. Eine BACPAC-Datei kann in Azure Blob Storage oder im lokalen Speicher zur Archivierung oder Migration (wie von SQL Server zu Azure SQL-Datenbank) gespeichert werden. Damit ein Export hinsichtlich der Transaktionen konsistent ist, müssen Sie sicherstellen, dass während des Exports keine Schreibaktivitäten stattfinden.

Führen Sie die folgenden Schritte aus, um die Datenbank „AdventureWorks2008R2“ mit dem Befehlszeilenprogramm „SQLPackage“ in lokalen Speicher zu exportieren.

1. Öffnen Sie eine Windows-Eingabeaufforderung, und navigieren Sie zu dem Ordner, in dem sich die **130**-Version von „SQLPackage“ befindet, z.B. **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.

2. Führen Sie den folgenden SQLPackage-Befehl an der Eingabeaufforderung aus, um die Datenbank **AdventureWorks2008R2** von **localhost** in **AdventureWorks2008R2.bacpac** zu exportieren. Ändern Sie diese Werte entsprechend Ihrer Umgebung.

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![Exportieren mit SQLPackage](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

Sobald die Ausführung abgeschlossen ist, wird die generierte BCPAC-Datei in dem Verzeichnis gespeichert, in dem sich die ausführbare Datei „sqlpackage“ befindet. In diesem Beispiel ist das „C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin“. 

## <a name="step-3-log-in-to-the-azure-portal"></a>Schritt 3: Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an. Melden Sie sich von dem Computer an, auf dem Sie das Befehlszeilenprogramm „SQLPackage“ ausführen. Dies vereinfacht die Erstellung der Firewallregel in Schritt 5.

## <a name="step-4-create-a-sql-database-logical-server"></a>Schritt 4: Erstellen eines logischen SQL-Datenbankservers

Ein [logischer Azure SQL-Datenbankserver](sql-database-features.md) fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. Führen Sie die folgenden Schritte aus, um einen logischen SQL-Datenbank-Server zu erstellen, der die migrierte Adventure Works-OLTP-SQL Server-Datenbank enthalten soll. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Geben Sie **server** im Suchfenster auf der Seite **New** ein, und wählen Sie in der gefilterten Liste den Eintrag **SQL database (logical server)** aus.

    ![Auswählen des logischen Servers](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. Klicken Sie auf der Seite **Alles** auf **SQL server (logical server)**, und klicken Sie dann auf der Seite **SQL Server (logical server)** auf **Create**.

    ![Erstellen des logischen Servers](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. Geben Sie die folgenden Informationen in das Formular „SQL server (logical server)“ ein, wie in der obigen Abbildung dargestellt:     

   - Servername: Geben Sie einen global eindeutigen Servernamen ein.
   - Serveradministratoranmeldung: Geben Sie einen Namen für die Serveradministratoranmeldung ein.
   - Kennwort: Geben Sie das gewünschte Kennwort ein.
   - Ressourcengruppe: Wählen Sie **Erstellen** aus, und geben Sie **myResourceGroup** an.
   - Speicherort: Wählen Sie den Standort des Rechenzentrums aus.

    ![Ausgefülltes Formular für Erstellen eines logischen Servers](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. Klicken Sie auf **Erstellen**, um den logischen Server bereitzustellen. Die Bereitstellung dauert einige Minuten. 

## <a name="step-5-create-a-server-level-firewall-rule"></a>Schritt 5: Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine [Firewall auf Serverebene](sql-database-firewall-configure.md), um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die folgenden Schritte aus, um eine SQL-Datenbank-Firewallregel auf Serverebene für die IP-Adresse des Computers zu erstellen, auf dem Sie das Befehlszeilenprogramm „SQLPackage“ ausführen. Damit wird es „SQLPackage“ ermöglicht, über die Azure SQL-Datenbank-Firewall eine Verbindung mit dem logischen SQL-Datenbankserver herzustellen. 

1. Klicken Sie im linken Menü auf **Alle Ressourcen**, und klicken Sie auf der Seite **Alle Ressourcen** auf den neuen Server. Die Übersichtsseite für Ihren Server wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170403.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

     ![Übersicht über logischen Server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Klicken Sie auf der Übersichtsseite im linken Menü unter **Einstellungen** auf **Firewall**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

3. Klicken Sie auf der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um die IP-Adresse des Computers hinzuzufügen, den Sie derzeit verwenden. Klicken Sie dann auf **Speichern**. Eine Firewallregel auf Serverebene wird für diese IP-Adresse erstellt.

     ![Festlegen der Serverfirewallregel](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. Klicken Sie auf **OK**.

Nun können Sie über diese IP-Adresse Verbindungen mit allen Datenbanken auf diesem Server herstellen. Verwenden Sie hierzu SQL Server Management Studio oder ein anderes Tool Ihrer Wahl sowie das zuvor erstellte Serveradministratorkonto.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

## <a name="step-6---import-bacpac-file-to-azure-sql-database"></a>Schritt 6: Importieren der BACPAC-Datei in Azure SQL-Datenbank 

Die neuesten Versionen des Befehlszeilenprogramms „SQLPackage“ unterstützen das Erstellen einer Azure SQL-Datenbank auf einer angegebenen [Dienstebene und Leistungsstufe](sql-database-service-tiers.md). Damit beim Importieren die beste Leistung erzielt wird, wählen Sie eine hohe Dienstebene und Leistungsstufe aus. Nach dem Import können Sie zentral herunterskalieren, wenn die Dienstebene und die Leistungsstufe höher sind, als dies derzeit erforderlich ist.

Führen Sie die folgenden Schritte aus, um die Datenbank „AdventureWorks2008R2“ mit dem Befehlszeilenprogramm „SQLPackage“ in Azure SQL-Datenbank zu importieren.

- Führen Sie den folgenden SQLPackage-Befehl an der Eingabeaufforderung aus, um die Datenbank **AdventureWorks2008R2** aus dem lokalen Speicher in dem logischen Azure SQL-Datenbankserver zu importieren, den Sie zuvor mit dem Datenbanknamen **myMigratedDatabase**, der Dienstebene **Premium** und dem Dienstziel **P6** erstellt haben. Ändern Sie jeden dieser drei Werte entsprechend Ihrer Umgebung.

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![Importieren mit SQLPackage](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> Ein logischer Azure SQL-Datenbankserver lauscht über Port 1433. Wenn Sie versuchen, durch eine Unternehmensfirewall eine Verbindung mit einem logischen Azure SQL-Datenbankserver herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit der Vorgang erfolgreich ist.
>

## <a name="step-7---connect-using-sql-server-management-studio-ssms"></a>Schritt 7: Herstellen einer Verbindung mithilfe von SQL Server Management Studio (SSMS)

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit Ihrem Azure SQL-Datenbankserver und der neu migrierten Datenbank herzustellen. Wenn Sie SQL Server Management Studio nicht auf dem Computer ausführen, der mit dem identisch ist, auf dem Sie „SQLPackage“ ausgeführt haben, erstellen Sie eine Firewallregel für diesen Computer (siehe die Schritte im vorherigen Verfahren).

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

## <a name="step-8---change-database-properties"></a>Schritt 8: Ändern von Datenbankeigenschaften

Sie können die Dienstebene, die Leistungsstufe und den Kompatibilitätsgrad über SQL Server Management Studio ändern.

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

- Eine Übersicht über Migration finden Sie unter [Datenbankmigration](sql-database-cloud-migrate.md).
- Eine Zusammenstellung zu Transact-SQL-Unterschieden finden Sie unter [Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank](sql-database-transact-sql-information.md).
- Informationen zum Herstellen einer Verbindung mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](sql-database-connect-query-dotnet.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit PHP finden Sie unter [Verbinden und Abfragen mit PHP](sql-database-connect-query-php.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js](sql-database-connect-query-nodejs.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Java finden Sie unter [Verbinden und Abfragen mit Java](sql-database-connect-query-java.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Python finden Sie unter [Verbinden und Abfragen mit Python](sql-database-connect-query-python.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Ruby finden Sie unter [Verbinden und Abfragen mit Ruby](sql-database-connect-query-ruby.md).


