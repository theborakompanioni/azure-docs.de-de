<properties
   pageTitle="Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank vor der Migration zu SQL-Datenbank | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure, SSDT"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank mithilfe von SQL Server Data Tools für Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Aktualisierungsratgeber](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In diesem Artikel erfahren Sie, wie Sie mit SQL Server Data Tools für Visual Studio vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank ermitteln und beheben.

## Verwenden von SQL Server Data Tools für Visual Studio

Verwenden Sie SQL Server Data Tools für Visual Studio (SSDT), um das Datenbankschema zur Analyse in ein Visual Studio-Datenbankprojekt zu importieren. Für die Analyse muss SQL-Datenbank V12 als Zielplattform angegeben und das Projekt erstellt werden. Wenn der Erstellungsvorgang erfolgreich ist, ist die Datenbank kompatibel. Wenn der Buildvorgang fehlschlägt, können Sie die Fehler in SSDT (oder einem anderen Tool, das in diesem Thema erläutert wird) beheben. Nachdem das Projekt erfolgreich erstellt wurde, können Sie es in einer Kopie der Quelldatenbank wieder veröffentlichen. Dann können Sie die Daten mit dem Datenvergleichsfeature in SSDT aus der Quelldatenbank in die Azure SQL V12-kompatible Datenbank kopieren. Sie können diese aktualisierte Datenbank dann migrieren. Um diese Option verwenden zu können, laden Sie die [neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx) herunter.

  ![Diagramm der VSSSDT-Migration](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Wenn nur das Schema migriert werden muss, kann das Schema direkt in Visual Studio für die Azure SQL-Datenbank veröffentlicht werden. Diese Methode kann verwendet werden, wenn das Datenbankschema mehr Änderungen erfordert, als der Migrations-Assistent allein verarbeiten kann.

## Ermitteln von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio
   
1.	Öffnen Sie den **Objekt-Explorer von SQL Server** in Visual Studio. Verwenden Sie **SQL Server hinzufügen**, um eine Verbindung mit der SQL Server-Instanz herzustellen, die die zu migrierende Datenbank enthält. Suchen Sie die Datenbank im Objekt-Explorer, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Neues Projekt erstellen...** aus.
    
	![Neues Projekt](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)
   
2.	Legen Sie die Importeinstellungen auf **Import application-scoped objects only** fest. Deaktivieren Sie die Optionen zum Importieren des Folgenden: Anmeldungsverweise, Berechtigungen und Datenbankeinstellungen.

    ![alt text](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)

3.	Klicken Sie auf **Starten**, um die Datenbank zu importieren und das Projekt zu erstellen, das eine T-SQL-Skriptdatei für jedes Objekt in der Datenbank enthält. Die Skriptdateien werden in Ordnern innerhalb des Projekts geschachtelt.

    ![alt text](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)

4.	Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf das Datenbankprojekt, und wählen Sie „Eigenschaften“ aus. Konfigurieren Sie auf der Seite **Projekteinstellungen** die Zielplattform „Microsoft Azure SQL Database V12“.
    
    ![alt text](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)
    
5.	Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Erstellen**, um das Projekt zu erstellen.
    
	![alt text](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)
    
6.	Die **Fehlerliste** zeigt die einzelnen Inkompatibilitäten an. In diesem Fall ist der Benutzername „NT AUTHORITY\\NETWORK SERVICE“ nicht kompatibel. Da er nicht kompatibel ist, können Sie ihn kommentieren oder entfernen (und die Auswirkungen des Entfernens dieser Anmeldung und Rolle aus der Datenbanklösung behandeln).
    
	![alt text](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)
    
## Beheben von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio

1.	Doppelklicken Sie auf das erste Skript, um es in einem Abfragefenster zu öffnen, auszukommentieren und dann auszuführen. ![alt text](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.	Wiederholen Sie diesen Vorgang für jedes Skript mit Inkompatibilitäten mit, bis kein Fehler mehr vorhanden ist. ![alt text](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.	Wenn die Datenbank fehlerfrei ist, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen**. Eine Kopie der Quelldatenbank wird erstellt und veröffentlicht (Sie sollten, zumindest zu Beginn, eine Kopie verwenden).
 - Vor der Veröffentlichung sollten Sie in Abhängigkeit von der Quell-SQL Server-Version (älter als SQL Server 2014) die Zielplattform für das Projekt zum Aktivieren der Bereitstellung zurücksetzen.
 - Wenn Sie eine ältere SQL Server-Datenbank migrieren, führen Sie keine Features in das Projekt ein, die nicht von der Quell-SQL Server-Instanz unterstützt werden, es sei denn, Sie migrieren die Datenbank zu einer neueren Version von SQL Server.

    	![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
    	![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
    	
4.	Klicken Sie im SQL Server-Objekt-Explorer mit der rechten Maustaste auf Ihre Quelldatenbank, und klicken Sie auf **Datenvergleich**. Der Vergleich des Projekts mit der ursprünglichen Datenbank hilft Ihnen zu verstehen, welche Änderungen vom Assistenten vorgenommen wurden. Wählen Sie Ihre Azure SQL-Version V12 der Datenbank aus, und klicken Sie dann auf **Fertig stellen**.
    
	![alt text](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)
    
	![alt text](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)

5.	Überprüfen Sie die erkannten Unterschiede, und klicken Sie dann auf **Ziel aktualisieren**, um Daten aus der Quelldatenbank in die Azure SQL V12-Datenbank zu migrieren.
    
	![alt text](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)
    
6.	Wählen Sie eine Bereitstellungsmethode. Informationen finden Sie unter [Migrieren einer kompatiblen SQL Server-Datenbank zu SQL-Datenbank](sql-database-cloud-migrate.md).

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0831_2016-->