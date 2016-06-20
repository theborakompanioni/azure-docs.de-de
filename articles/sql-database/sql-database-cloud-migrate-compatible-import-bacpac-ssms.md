<properties
   pageTitle="Migrieren einer SQL Server-Datenbank zu einer Azure SQL-Datenbank | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankbereitstellung, Datenbankmigration, Datenbank importieren, Datenbank exportieren, Migrations-Assistent"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="06/07/2016"
   ms.author="carlrab"/>

# Importieren aus BACPAC zur SQL-Datenbank mithilfe von SSMS

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure-Portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

In diesem Artikel wird beschrieben, wie Sie mithilfe des Assistenten „Datenebenenanwendung exportieren“ in SQL Server Management Studio aus einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4)-Datei zur SQL-Datenbank importieren.

> [AZURE.NOTE] Bei den folgenden Schritten wird davon ausgegangen, dass Sie Ihre logische SQL Azure-Instanz bereits bereitgestellt haben und dass Ihnen die Verbindungsinformationen vorliegen.

1. Stellen Sie sicher, dass Sie über die neueste Version von SQL Server Management Studio verfügen. Neue Versionen von Management Studio werden monatlich aktualisiert, damit sie mit Updates des Azure-Portals synchron sind.

	 > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Stellen Sie eine Verbindung mit dem Azure SQL-Datenbank-Server her. Klicken Sie mit der rechten Maustaste auf den Ordner **Datenbanken**, und klicken Sie auf **Datenebenenanwendung importieren...**.

    ![Menüelement für das Importieren von Datenebenenanwendungen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.	Importieren Sie mit dem Import-Assistenten die BACPAC-Datei von Ihrem lokalen Datenträger, oder wählen Sie das Azure-Speicherkonto und den Container aus, in das bzw. den Sie die BACPAC-Datei hochgeladen haben, um die neue Datenbank in Azure SQL-Datenbank zu erstellen.

    ![Importieren von Einstellungen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

    ***Wichtig:*** Verwenden Sie beim Importieren einer BACPAC-Datei aus Azure-Blobspeicher den Standardspeicher. Das Importieren einer BACPAC-Datei aus dem Premiumspeicher wird nicht unterstützt.

4.	Geben Sie den **neuen Datenbanknamen** für die Datenbank in Azure SQL-Datenbank an, und legen Sie **Edition von Microsoft Azure SQL-Datenbank** (Dienstebene), **Maximale Datenbankgröße** und **Serviceziel** (Leistungsebene) fest.

    ![Datenbankeinstellungen](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.	Klicken Sie auf **Weiter** und dann auf **Fertig stellen**, um die BACPAC-Datei in eine neue Datenbank auf dem Azure SQL-Datenbank-Server zu importieren.

6. Stellen Sie mit dem Objekt-Explorer eine Verbindung mit der migrierten Datenbank auf Ihrem Azure SQL-Datenbank-Server her.

6.	Zeigen Sie über das Azure-Portal Ihre Datenbank und deren Eigenschaften an.

## Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## Weitere Informationen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Teilweise oder vollständig unterstützte Transact-SQL-Funktionen](sql-database-transact-sql-information.md)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Migrieren von Nicht-SQL Server-Datenbanken mithilfe des SQL Server-Migrations-Assistenten)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0608_2016-->