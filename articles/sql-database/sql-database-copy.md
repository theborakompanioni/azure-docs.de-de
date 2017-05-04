---
title: Kopieren einer Azure SQL-Datenbank | Microsoft Docs
description: Erstellen der Kopie einer Azure SQL-Datenbank
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 5aaf6bcd-3839-49b5-8c77-cbdf786e359b
ms.service: sql-database
ms.custom: move data
ms.devlang: NA
ms.date: 04/05/2017
ms.author: sashan;carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: af953e16103951fe2fd283898b3c466a1ebe91fd
ms.lasthandoff: 04/27/2017


---
# <a name="copy-an-azure-sql-database"></a>Kopieren einer Azure SQL-Datenbank

Azure SQL-Datenbank bietet mehrere Methoden, eine hinsichtlich Transaktionen konsistente Kopie einer vorhandenen Azure SQL-Datenbank auf demselben Server oder auf einem anderen Server zu erstellen. Sie können eine SQL-Datenbank über das Azure-Portal, über PowerShell oder über T-SQL kopieren. 

## <a name="overview"></a>Übersicht

Eine Datenbankkopie ist eine Momentaufnahme der Quelldatenbank zum Zeitpunkt der Kopieranforderung. Sie können denselben Server oder einen anderen Server, dessen Dienstebene und Leistungsstufe oder eine andere Leistungsstufe innerhalb desselben Diensttarifs (Edition) auswählen. Nachdem der Kopiervorgang abgeschlossen ist, wird die Kopie zu einer voll funktionsfähigen, unabhängigen Datenbank. Zu diesem Zeitpunkt können Sie ein Upgrade oder Downgrade auf jede beliebige Edition durchführen. Anmeldungen, Benutzer und Berechtigungen können unabhängig verwaltet werden.  

## <a name="logins-in-the-database-copy"></a>Anmeldungen in der Datenbankkopie

Wenn Sie eine Datenbank auf denselben logischen Server kopieren, können für beide Datenbanken die gleichen Anmeldedaten verwendet werden. Der Sicherheitsprinzipal, den Sie zum Kopieren der Datenbank verwenden, wird zum Datenbankbesitzer der neuen Datenbank. Alle Datenbankbenutzer, ihre Berechtigungen und ihre Sicherheits-IDs (SIDs) werden in die Kopie der Datenbank kopiert.  

Wenn Sie eine Datenbank auf einen anderen logischen Server kopieren, wird der Sicherheitsprinzipal auf dem neuen Server zum Datenbankbesitzer für die neue Datenbank. Wenn Sie [eigenständige Datenbankbenutzer](sql-database-manage-logins.md) für den Datenzugriff verwenden, stellen Sie sicher, dass sowohl für die primäre als auch für die sekundäre Datenbank immer die gleichen Anmeldeinformationen verwendet werden, sodass Sie nach Abschluss des Kopiervorgang mit diesen Anmeldeinformationen sofort auf die Datenbank zugreifen können. 

Wenn Sie [Azure Active Directory](../active-directory/active-directory-whatis.md) verwenden, müssen Sie die Anmeldeinformationen in der Kopie überhaupt nicht verwalten. Wenn Sie jedoch die Datenbank auf einen neuen Server kopieren, funktioniert der anmeldungsbasierte Zugriff eventuell nicht, da die Anmeldungen auf dem neuen Server nicht vorhanden sind. Informationen zum Verwalten von Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). 

Nachdem der Kopiervorgang erfolgreich abgeschlossen wurde und bevor andere Benutzer neu zugeordnet werden, kann nur die Anmeldung, über die der Kopiervorgang initiiert wurde (der Datenbankbesitzer), zum Anmelden bei der neuen Datenbank verwendet werden. Informationen zum Auflösen von Anmeldungen, nachdem der Kopiervorgang abgeschlossen wurde, finden Sie unter [Auflösen von Anmeldungen](sql-database-copy.md#resolve-logins.md).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Kopieren einer Datenbank über das Azure-Portal

Um eine Datenbank über das Azure-Portal zu kopieren, öffnen Sie die Seite für Ihre Datenbank, und klicken Sie auf **Kopieren**. 

   ![Datenbankkopie](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Kopieren einer Datenbank mit PowerShell

Verwenden Sie zum Kopieren einer Datenbank mit PowerShell das Cmdlet [`New-AzureRmSqlDatabaseCopy`](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy). 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Ein vollständiges Beispielskript finden Sie unter [Kopieren einer Datenbank auf einen neuen Server](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Kopieren einer Datenbank mit Transact-SQL

Melden Sie sich mit der Prinzipalanmeldung auf Serverebene oder der Anmeldung, mit der die zu kopierende Datenbank erstellt wurde, bei der Datenbank master an. Damit der Datenbankkopiervorgang funktioniert, müssen Anmeldungen, bei denen der Prinzipal auf Serverebene nicht verwendet wird, Mitglieder der Rolle „dbmanager“ sein. Weitere Informationen zu Anmeldungen und zum Herstellen einer Verbindung mit dem Server finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).

Starten Sie das Kopieren der Quelldatenbank mit der Anweisung [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) . Durch das Ausführen dieser Anweisung wird der Kopiervorgang für die Datenbank initiiert. Da das Kopieren einer Datenbank ein asynchroner Prozess ist, erfolgt die Rückgabe der CREATE DATABASE-Anweisung, bevor die Datenbank den Kopiervorgang abgeschlossen hat.

### <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf denselben Server
Melden Sie sich mit der Prinzipalanmeldung auf Serverebene oder der Anmeldung, mit der die zu kopierende Datenbank erstellt wurde, bei der Datenbank master an. Damit der Datenbankkopiervorgang funktioniert, müssen Anmeldungen, bei denen der Prinzipal auf Serverebene nicht verwendet wird, Mitglieder der Rolle „dbmanager“ sein.

Mit diesem Befehl wird Database1 in eine neue Datenbank mit dem Namen „Database2“ auf demselben Server kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einen anderen Server

Melden Sie sich bei der Datenbank master auf dem Zielserver an. Dies ist der SQL-Datenbank-Server, auf dem die neue Datenbank erstellt werden soll. Verwenden Sie eine Anmeldung, die den gleichen Namen und das gleiche Kennwort wie der Datenbankbesitzer der Quelldatenbank auf dem SQL-Datenbank-Quellserver hat. Die Anmeldung auf dem Zielserver muss auch ein Mitglied der Rolle „dbmanager“ sein, oder es muss sich um die Prinzipalanmeldung auf Serverebene handeln.

Mit diesem Befehl wird Database1 auf server1 in eine neue Datenbank mit dem Namen „Database2“ auf server2 kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-the-progress-of-the-copying-operation"></a>Überwachen des Fortschritts des Kopiervorgangs

Überwachen Sie den Kopiervorgang, indem Sie die Ansichten „sys.databases“ und „sys.dm_database_copies“ abfragen. Während des Kopiervorgangs wird die Spalte **state_desc** der Ansicht „sys.databases“ für die neue Datenbank auf **COPYING** gesetzt.

* Wenn beim Kopieren ein Fehler auftritt, wird die Spalte **state_desc** der Ansicht „sys.databases“ für die neue Datenbank auf **SUSPECT** gesetzt. Führen Sie die DROP-Anweisung in der neuen Datenbank aus, und wiederholen Sie den Vorgang später noch einmal.
* Wenn der Kopiervorgang erfolgreich ist, wird die Spalte **state_desc** der Ansicht „sys.databases“ für die neue Datenbank auf **ONLINE** gesetzt. Der Kopiervorgang ist abgeschlossen, und die neue Datenbank ist eine normale Datenbank, die unabhängig von der Quelldatenbank geändert werden kann.

> [!NOTE]
> Wenn Sie den Kopiervorgang während der Ausführung abbrechen möchten, führen Sie die Anweisung [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) für die neue Datenbank aus. Alternativ dazu kann der Kopiervorgang auch abgebrochen werden, indem die DROP DATABASE-Anweisung in der Quelldatenbank ausgeführt wird.
> 

## <a name="resolve-logins"></a>Auflösen von Anmeldungen

Nachdem die neue Datenbank auf dem Zielserver online ist, können Sie die [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) -Anweisung verwenden, um die Benutzer aus der neuen Datenbank den Anmeldungen auf dem Zielserver zuzuordnen. Informationen zum Auflösen von verwaisten Benutzern finden Sie unter [Problembehandlung bei verwaisten Benutzern](https://msdn.microsoft.com/library/ms175475.aspx). Siehe auch [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).

Für alle Benutzer werden in der neuen Datenbank die Berechtigungen beibehalten, über die sie auch in der Quelldatenbank verfügt haben. Der Benutzer, der das Kopieren der Datenbank initiiert hat, wird zum Datenbankbesitzer der neuen Datenbank und erhält eine neue Sicherheits-ID (SID). Nachdem der Kopiervorgang erfolgreich abgeschlossen wurde und bevor andere Benutzer neu zugeordnet werden, kann nur die Anmeldung, über die der Kopiervorgang initiiert wurde (der Datenbankbesitzer), zum Anmelden bei der neuen Datenbank verwendet werden.

Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Anmeldungen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md) und [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
* Informationen zum Exportieren einer Datenbank finden Sie unter [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md).

