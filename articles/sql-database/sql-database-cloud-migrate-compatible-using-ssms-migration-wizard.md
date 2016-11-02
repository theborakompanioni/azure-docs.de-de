<properties
   pageTitle="Migrieren einer SQL Server-Datenbank zu einer SQL-Datenbank mit dem Datenbankassistenten zum Bereitstellen einer Datenbank unter Microsoft Azure | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Assistent für Microsoft Azure-Datenbank"
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

# Migrieren von SQL Server-Datenbank auf SQL-Datenbank mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank


> [AZURE.SELECTOR]
- [SSMS-Migrations-Assistent](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exportieren in eine BACPAC-Datei](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importieren aus einer BACPAC-Datei](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure in SQL Server Management Studio wird eine [kompatible SQL Server-Datenbank](sql-database-cloud-migrate.md) direkt in Ihren Azure SQL-Datenbankserver migriert.

## Verwenden des Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank

> [AZURE.NOTE] Die folgenden Schritte setzen voraus, dass Sie über einen [bereitgestellten SQL-Datenbankserver](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/) verfügen.

1. Stellen Sie sicher, dass Sie über die neueste Version von SQL Server Management Studio verfügen. Neue Versionen von Management Studio werden monatlich aktualisiert, damit sie mit Updates des Azure-Portals synchron sind.

    > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Öffnen Sie Management Studio und stellen Sie eine Verbindung mit Ihrer SQL Server-Datenbank her, die in den Objekt-Explorer migriert werden soll.
3. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, zeigen Sie auf **Aufgaben**, und klicken Sie auf **Datenbank für Microsoft Azure SQL-Datenbank bereitstellen…**

	!["Bereitstellen in Azure" aus dem Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.	Klicken Sie im Bereitstellungs-Assistenten auf **Weiter**, und klicken Sie dann auf **Verbinden**, um die Verbindung mit Ihrem SQL-Datenbank-Server zu konfigurieren.

	!["Bereitstellen in Azure" aus dem Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. Geben Sie im Dialogfeld „Verbindung mit Server herstellen“ Ihre Verbindungsinformationen ein, um die Verbindung mit Ihrem SQL-Datenbankserver herzustellen.

	!["Bereitstellen in Azure" aus dem Menü "Aufgaben"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.	Geben Sie Folgendes für die [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei an, die dieser Assistent während der Migration erstellt:

 - Den **neuen Datenbanknamen**
 - Die **Edition von Microsoft Azure SQL-Datenbank** ([Dienstebene](sql-database-service-tiers.md))
 - Die **maximale Datenbankgröße**
 - Das **Dienstziel** (Leistungsstufe)
 - Den **temporären Dateinamen**

    ![Exporteinstellungen](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.	Schließen Sie den Assistenten ab. Je nach Größe und Komplexität der Datenbank kann die Bereitstellung wenige Minuten bis viele Stunden dauern. Wenn dieser Assistent Kompatibilitätsprobleme erkennt, werden auf dem Bildschirm Fehler angezeigt, und die Migration wird nicht fortgesetzt. Eine Anleitung zum Beheben von Problemen mit der Datenbankkompatibilität finden Sie unter [Beheben von Problemen mit der Datenbankkompatibilität](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.	Stellen Sie mit dem Objekt-Explorer eine Verbindung mit der migrierten Datenbank auf Ihrem Azure SQL-Datenbankserver her.
8.	Zeigen Sie über das Azure-Portal Ihre Datenbank und deren Eigenschaften an.

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0831_2016-->