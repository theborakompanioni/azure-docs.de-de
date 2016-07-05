<properties
	pageTitle="Kopieren einer Azure SQL-Datenbank | Microsoft Azure"
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



# Kopieren einer Azure SQL-Datenbank

> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-Portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Sie können mithilfe der [automatisierten Sicherungen der Azure SQL-Datenbank](sql-database-automated-backups.md) eine Kopie der SQL-Datenbank erstellen. Beim Kopiervorgang wird das Ende des Transaktionsprotokolls kopiert. Dann werden die vollständigen und differenziellen Sicherungen sowie die Transaktionsprotokollsicherungen, die Teil der automatisierten Sicherungen sind, zum Erstellen einer Datenbank verwendet, die im Hinblick auf Transaktionen konsistent mit der Quelldatenbank zum Zeitpunkt der letzten Transaktionsprotokollsicherung ist.

Sie können die Datenbankkopie auf dem gleichen Server oder auf einem anderen Server erstellen. Die Dienstebene und die Leistungsebene (Tarif) der Datenbankkopie stimmen mit den Ebenen der Quelldatenbank überein. Nachdem der Kopiervorgang abgeschlossen ist, wird die Kopie zu einer voll funktionsfähigen, unabhängigen Datenbank. Anmeldungen, Benutzer und Berechtigungen können unabhängig verwaltet werden.

Wenn Sie eine Datenbank auf denselben logischen Server kopieren, können für beide Datenbanken die gleichen Anmeldedaten verwendet werden. Das Sicherheitsprinzipal, das Sie zum Kopieren der Datenbank verwenden, wird zum Datenbankbesitzer (DBO) der neuen Datenbank. Alle Datenbankbenutzer, ihre Berechtigungen und ihre Sicherheits-IDs (SIDs) werden in die Kopie der Datenbank kopiert.

Wenn Sie eine Datenbank auf einen anderen logischen Server kopieren, wird der Sicherheitsprinzipal auf dem neuen Server zum Datenbankbesitzer für die neue Datenbank. Datenbankbenutzer, die eigenständige Benutzer sind, können in der kopierten Datenbank verwendet werden. Wenn Sie jedoch die Datenbank auf einen neuen Server kopieren, funktionieren Benutzer, die auf Anmeldungen basieren, in der Regel nicht, da die Anmeldungen auf dem neuen Server nicht vorhanden sind. Sollten sie vorhanden sein, stimmen möglicherweise die SIDs nicht überein. Nachdem die neue Datenbank auf dem Zielserver online ist, können Sie die [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)-Anweisung verwenden, um die Benutzer aus der neuen Datenbank den Anmeldungen auf dem Zielserver zuzuordnen. Informationen zum Auflösen von verwaisten Benutzern finden Sie unter [Problembehandlung bei verwaisten Benutzern](https://msdn.microsoft.com/library/ms175475.aspx).


Zum Kopieren einer SQL-Datenbank benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Eine zu kopierende SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).

## Nächste Schritte

- Informationen zum Kopieren einer Datenbank mithilfe des Azure-Portals finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md).
- Informationen zum Kopieren einer Datenbank mithilfe von PowerShell finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md).
- Informationen zum Kopieren einer Datenbank mithilfe von Transact-SQL finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md).
- Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).



## Zusätzliche Ressourcen

- [Verwalten von Anmeldungen](sql-database-manage-logins.md)
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)
- [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0622_2016-->