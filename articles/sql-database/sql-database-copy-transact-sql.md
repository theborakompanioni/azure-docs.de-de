---
title: Kopieren einer Azure SQL-Datenbank mithilfe von Transact-SQL | Microsoft Docs
description: Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von Transact-SQL
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 40ea3718-33f8-41af-90cb-3aa15059365e
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 02/07/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 60bcd02d24e2084b9020ce56ef6a9f8268c6b1b5
ms.openlocfilehash: 3ade1f2850b2a67f68e8a4a7f519b7dc7ba1de10


---
# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopieren einer Azure SQL-Datenbank mithilfe von Transact-SQL

Die folgenden Schritte veranschaulichen, wie Sie eine SQL-Datenbank mit Transact-SQL auf den gleichen oder einen anderen Server kopieren. Beim Kopieren der Datenbank wird die Anweisung [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) verwendet. 

> [!NOTE]
> Sie können eine SQL-Datenbank auch über das [Azure-Portal](sql-database-copy-portal.md) oder [PowerShell](sql-database-copy-powershell.md) kopieren.
>

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, klicken Sie einfach oben auf dieser Seite auf den Link **Kostenloses Konto** klicken. Lesen Sie anschließend den Artikel weiter.
* Azure SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).
* [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Wenn Sie nicht über SSMS verfügen oder in diesem Artikel beschriebene Funktionen nicht verfügbar sind, können Sie die [aktuelle Version herunterladen](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="copy-your-sql-database"></a>Kopieren der SQL-Datenbank
Melden Sie sich an der Datenbank „master“ an, indem Sie die Prinzipalanmeldung auf Serverebene oder die Anmeldung verwenden, mit der die zu kopierende Datenbank erstellt wurde. Anmeldungen, bei denen der Prinzipal auf Serverebene nicht verwendet wird, müssen Mitglieder der Rolle „dbmanager“ sein, um Datenbanken zu kopieren. Weitere Informationen zu Anmeldungen und zum Herstellen einer Verbindung mit dem Server finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).

Starten Sie das Kopieren der Quelldatenbank mit der Anweisung [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) . Durch das Ausführen dieser Anweisung wird der Kopiervorgang für die Datenbank initiiert. Da das Kopieren einer Datenbank ein asynchroner Prozess ist, erfolgt die Rückgabe der CREATE DATABASE-Anweisung, bevor die Datenbank den Kopiervorgang abgeschlossen hat.

### <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf denselben Server
Melden Sie sich an der Datenbank „master“ an, indem Sie die Prinzipalanmeldung auf Serverebene oder die Anmeldung verwenden, mit der die zu kopierende Datenbank erstellt wurde. Anmeldungen, bei denen der Prinzipal auf Serverebene nicht verwendet wird, müssen Mitglieder der Rolle „dbmanager“ sein, um Datenbanken zu kopieren.

Mit diesem Befehl wird „Database1“ in eine neue Datenbank mit dem Namen „Database2“ auf demselben Server kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einen anderen Server
Melden Sie sich an der Datenbank „master“ des Zielservers an. Dies ist der Azure SQL-Datenbank-Server, auf dem die neue Datenbank erstellt werden soll. Verwenden Sie eine Anmeldung, die den gleichen Namen und das gleiche Kennwort wie der Datenbankbesitzer (DBO) der Quelldatenbank auf dem Azure SQL-Datenbank-Quellserver hat. Die Anmeldung auf dem Zielserver muss auch ein Mitglied der Rolle „dbmanager“ sein, oder es muss sich um die Prinzipalanmeldung auf Serverebene handeln.

Mit diesem Befehl wird „Database1“ auf „Server1“ in eine neue Datenbank mit dem Namen „Database2“ auf „Server2“ kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


## <a name="monitor-the-progress-of-the-copy-operation"></a>Überwachen des Fortschritts des Kopiervorgangs
Überwachen Sie den Kopiervorgang, indem Sie die Ansichten „sys.databases“ und „sys.dm_database_copies“ abfragen. Während des Kopiervorgangs wird die Spalte „state_desc“ der Ansicht „sys.databases“ für die neue Datenbank auf COPYING gesetzt.

* Wenn der Kopiervorgang nicht erfolgreich ist, wird die Spalte „state_desc“ der Ansicht „sys.databases“ für die neue Datenbank auf SUSPECT gesetzt. Führen Sie in diesem Fall die DROP-Anweisung in der neuen Datenbank aus, und wiederholen Sie den Vorgang später noch einmal.
* Wenn der Kopiervorgang erfolgreich ist, wird die Spalte „state_desc“ der Ansicht „sys.databases“ für die neue Datenbank auf ONLINE gesetzt. In diesem Fall ist der Kopiervorgang abgeschlossen, und die neue Datenbank ist eine normale Datenbank, die unabhängig von der Quelldatenbank geändert werden kann.

> [!NOTE]
> * Wenn Sie den Kopiervorgang während der Ausführung abbrechen möchten, führen Sie die Anweisung [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) für die neue Datenbank aus. Alternativ dazu kann der Kopiervorgang auch abgebrochen werden, indem die DROP DATABASE-Anweisung in der Quelldatenbank ausgeführt wird.
> 
> 

## <a name="resolve-logins-after-the-copy-operation-completes"></a>Auflösen von Anmeldungen nach Abschluss des Kopiervorgangs
Nachdem die neue Datenbank auf dem Zielserver online ist, können Sie die [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) -Anweisung verwenden, um die Benutzer aus der neuen Datenbank den Anmeldungen auf dem Zielserver zuzuordnen. Informationen zum Auflösen von verwaisten Benutzern finden Sie unter [Problembehandlung bei verwaisten Benutzern](https://msdn.microsoft.com/library/ms175475.aspx). Siehe auch [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).

Für alle Benutzer werden in der neuen Datenbank die Berechtigungen beibehalten, über die sie auch in der Quelldatenbank verfügt haben. Der Benutzer, der das Kopieren der Datenbank initiiert hat, wird zum Datenbankbesitzer der neuen Datenbank und erhält eine neue Sicherheits-ID (SID). Nachdem der Kopiervorgang erfolgreich abgeschlossen wurde und bevor andere Benutzer neu zugeordnet werden, kann nur die Anmeldung, über die der Kopiervorgang initiiert wurde (also der Datenbankbesitzer (DBO)), zum Anmelden an der neuen Datenbank verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
* [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)



<!--HONumber=Feb17_HO2-->


