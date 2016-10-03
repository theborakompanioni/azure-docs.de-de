<properties 
    pageTitle="Kopieren einer Azure SQL-Datenbank mithilfe von Transact-SQL | Microsoft Azure" 
    description="Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von Transact-SQL" 
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="09/19/2016"
	ms.author="sstein"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Kopieren einer Azure SQL-Datenbank mithilfe von Transact-SQL


> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-Portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Die folgenden Schritte veranschaulichen, wie Sie eine SQL-Datenbank mit Transact-SQL auf den gleichen oder einen anderen Server kopieren. Beim Kopieren der Datenbank wird die Anweisung [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) verwendet.

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Azure SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Wenn Sie nicht über SSMS verfügen oder in diesem Artikel beschriebene Funktionen nicht verfügbar sind, können Sie die [aktuelle Version herunterladen](https://msdn.microsoft.com/library/mt238290.aspx).


## Kopieren der SQL-Datenbank

Melden Sie sich an der Datenbank „master“ an, indem Sie die Prinzipalanmeldung auf Serverebene oder die Anmeldung verwenden, mit der die zu kopierende Datenbank erstellt wurde. Anmeldungen, bei denen das Prinzipal auf Serverebene nicht verwendet wird, müssen Mitglieder der Rolle „dbmanager“ sein, um Datenbanken zu kopieren. Weitere Informationen zu Anmeldungen und zum Herstellen einer Verbindung mit dem Server finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).

Starten Sie das Kopieren der Quelldatenbank mit der Anweisung [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx). Durch das Ausführen dieser Anweisung wird der Kopiervorgang für die Datenbank initiiert. Da das Kopieren einer Datenbank ein asynchroner Prozess ist, erfolgt die Rückgabe der CREATE DATABASE-Anweisung, bevor die Datenbank den Kopiervorgang abgeschlossen hat.


### Kopieren einer SQL-Datenbank auf denselben Server

Melden Sie sich an der Datenbank „master“ an, indem Sie die Prinzipalanmeldung auf Serverebene oder die Anmeldung verwenden, mit der die zu kopierende Datenbank erstellt wurde. Anmeldungen, bei denen das Prinzipal auf Serverebene nicht verwendet wird, müssen Mitglieder der Rolle „dbmanager“ sein, um Datenbanken zu kopieren.

Mit diesem Befehl wird „Database1“ in eine neue Datenbank mit dem Namen „Database2“ auf demselben Server kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### Kopieren einer SQL-Datenbank auf einen anderen Server

Melden Sie sich an der Datenbank „master“ des Zielservers an. Dies ist der Azure SQL-Datenbank-Server, auf dem die neue Datenbank erstellt werden soll. Verwenden Sie eine Anmeldung, die den gleichen Namen und das gleiche Kennwort wie der Datenbankbesitzer (DBO) der Quelldatenbank auf dem Azure SQL-Datenbank-Quellserver hat. Die Anmeldung auf dem Zielserver muss auch ein Mitglied der Rolle „dbmanager“ sein, oder es muss sich um die Prinzipalanmeldung auf Serverebene handeln.

Mit diesem Befehl wird „Database1“ auf „Server1“ in eine neue Datenbank mit dem Namen „Database2“ auf „Server2“ kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## Überwachen des Fortschritts des Kopiervorgangs

Überwachen Sie den Kopiervorgang, indem Sie die Ansichten „sys.databases“ und „sys.dm\_database\_copies“ abfragen. Während des Kopiervorgangs wird die Spalte „state\_desc“ der Ansicht „sys.databases“ für die neue Datenbank auf COPYING gesetzt.


- Wenn der Kopiervorgang nicht erfolgreich ist, wird die Spalte „state\_desc“ der Ansicht „sys.databases“ für die neue Datenbank auf SUSPECT gesetzt. Führen Sie in diesem Fall die DROP-Anweisung in der neuen Datenbank aus, und wiederholen Sie den Vorgang später noch einmal.
- Wenn der Kopiervorgang erfolgreich ist, wird die Spalte „state\_desc“ der Ansicht „sys.databases“ für die neue Datenbank auf ONLINE gesetzt. In diesem Fall ist der Kopiervorgang abgeschlossen, und die neue Datenbank ist eine normale Datenbank, die unabhängig von der Quelldatenbank geändert werden kann.

> [AZURE.NOTE] - Wenn Sie den Kopiervorgang während der Ausführung abbrechen möchten, können Sie die Anweisung [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) für die neue Datenbank ausführen. Alternativ dazu kann der Kopiervorgang auch abgebrochen werden, indem die DROP DATABASE-Anweisung in der Quelldatenbank ausgeführt wird.


## Auflösen von Anmeldungen nach Abschluss des Kopiervorgangs

Nachdem die neue Datenbank auf dem Zielserver online ist, können Sie die [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)-Anweisung verwenden, um die Benutzer aus der neuen Datenbank den Anmeldungen auf dem Zielserver zuzuordnen. Informationen zum Auflösen von verwaisten Benutzern finden Sie unter [Problembehandlung bei verwaisten Benutzern](https://msdn.microsoft.com/library/ms175475.aspx). Siehe auch [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).

Für alle Benutzer werden in der neuen Datenbank die Berechtigungen beibehalten, über die sie auch in der Quelldatenbank verfügt haben. Der Benutzer, der das Kopieren der Datenbank initiiert hat, wird zum Datenbankbesitzer der neuen Datenbank und erhält eine neue Sicherheits-ID (SID). Nachdem der Kopiervorgang erfolgreich abgeschlossen wurde und bevor andere Benutzer neu zugeordnet werden, kann nur die Anmeldung, über die der Kopiervorgang initiiert wurde (also der Datenbankbesitzer (DBO)), zum Anmelden an der neuen Datenbank verwendet werden.


## Nächste Schritte

- Einen Überblick über das Kopieren einer Azure SQL-Datenbank finden Sie unter [Kopieren einer Azure SQL-Datenbank](sql-database-copy.md).
- Informationen zum Kopieren einer Datenbank mithilfe des Azure-Portals finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md).
- Informationen zum Kopieren einer Datenbank mithilfe von PowerShell finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md).
- Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).



## Zusätzliche Ressourcen

- [Verwalten von Anmeldungen](sql-database-manage-logins.md)
- [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)
- [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0921_2016-->