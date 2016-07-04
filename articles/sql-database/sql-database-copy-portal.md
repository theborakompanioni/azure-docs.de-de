<properties
	pageTitle="Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals | Microsoft Azure"
	description="Erstellen der Kopie einer Azure SQL-Datenbank"
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="06/16/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>



# Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals

> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-Portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Die folgenden Schritte veranschaulichen, wie Sie eine SQL-Datenbank über das [Azure-Portal](https://portal.azure.com) auf den gleichen oder einen anderen Server kopieren.

Zum Kopieren einer SQL-Datenbank benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Eine zu kopierende SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).


## Kopieren der SQL-Datenbank

Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie kopieren möchten:

1.	Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2.	Navigieren Sie zu der Datenbank, die Sie kopieren möchten: Durchsuchen > SQL-Datenbanken.
3.	Klicken Sie auf dem Blatt „SQL-Datenbank“ auf **Kopieren**, um das Blatt **Kopieren** zu öffnen:

    ![Datenbank kopieren][1]

1.  Geben Sie einen Namen für die Kopie der Datenbank ein. Es wird ein Standardname angegeben, den Sie bei Bedarf ändern können.
2.  Wählen Sie einen **Zielserver** aus. Auf dem Zielserver wird die Kopie der Datenbank erstellt. Sie können einen neuen Server erstellen oder in der Liste einen vorhandenen Server auswählen.
3.  Klicken Sie auf **OK**, um den Kopiervorgang zu starten.

    ![Datenbankname und Server][2]


## Überwachen des Fortschritts des Kopiervorgangs

- Klicken Sie nach dem Starten des Kopiervorgangs auf die Portalbenachrichtigung, um Details anzuzeigen.

    ![Benachrichtigung][3]
 
    ![Überwachen][4]


## Sicherstellen, dass die Datenbank auf dem Server aktiv ist

- Klicken Sie auf **DURCHSUCHEN** > **SQL-Datenbanken**, und überprüfen Sie, ob die neue Datenbank **online** ist.


## Auflösen von Anmeldungen

Informationen zum Auflösen von Anmeldungen, nachdem der Kopiervorgang abgeschlossen wurde, finden Sie unter [Auflösen von Anmeldungen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes).


## Nächste Schritte

- Einen Überblick über das Kopieren einer Azure SQL-Datenbank finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md).
- Informationen zum Kopieren einer Datenbank mithilfe von PowerShell finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md).
- Informationen zum Kopieren einer Datenbank mithilfe von Transact-SQL finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md).
- Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).



## Zusätzliche Ressourcen

- [Verwalten von Anmeldungen](sql-database-manage-logins.md)
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)
- [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

<!---HONumber=AcomDC_0622_2016-->