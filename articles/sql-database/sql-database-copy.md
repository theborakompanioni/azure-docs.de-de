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
ms.custom: migrate and move
ms.devlang: NA
ms.date: 10/24/2016
ms.author: sstein; sashan
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 21be71a1b4c79ecec8af02d08f65c41128c5ef73
ms.openlocfilehash: 265adbccee6764322148fad23f31aa1e279dfc9b


---
# <a name="copy-an-azure-sql-database"></a>Kopieren einer Azure SQL-Datenbank

Sie können mithilfe des [Features für aktive Georeplikation](sql-database-geo-replication-overview.md) eine Kopie der SQL-Datenbank erstellen. Im Gegensatz zur Georeplikation wird der Replikationslink aber geschlossen, sobald die Seedingphase beendet ist. Daher ist die Datenbankkopie eine Momentaufnahme der Quelldatenbank zum Zeitpunkt der Kopieranforderung.  

Sie können die Datenbankkopie auf dem gleichen Server oder auf einem anderen Server erstellen. Dienst- und Leistungsebene (Tarif) der Datenbankkopie stimmen standardmäßig mit den Ebenen der Quelldatenbank überein. Wenn Sie die API verwenden, können Sie eine andere Leistungsebene innerhalb der gleichen Dienstebene (Edition) auswählen. Nachdem der Kopiervorgang abgeschlossen ist, wird die Kopie zu einer voll funktionsfähigen, unabhängigen Datenbank. Zu diesem Zeitpunkt können Sie ein Upgrade oder Downgrade auf jede beliebige Edition durchführen. Anmeldungen, Benutzer und Berechtigungen können unabhängig verwaltet werden.  

Wenn Sie eine Datenbank auf denselben logischen Server kopieren, können für beide Datenbanken die gleichen Anmeldedaten verwendet werden. Das Sicherheitsprinzipal, das Sie zum Kopieren der Datenbank verwenden, wird zum Datenbankbesitzer (DBO) der neuen Datenbank. Alle Datenbankbenutzer, ihre Berechtigungen und ihre Sicherheits-IDs (SIDs) werden in die Kopie der Datenbank kopiert.  

Wenn Sie eine Datenbank auf einen anderen logischen Server kopieren, wird der Sicherheitsprinzipal auf dem neuen Server zum Datenbankbesitzer für die neue Datenbank. Wenn Sie [eigenständige Datenbankbenutzer](sql-database-manage-logins.md) für den Datenzugriff verwenden, stellen Sie sicher, dass sowohl für die primäre als auch für die sekundäre Datenbank immer die gleichen Anmeldeinformationen verwendet werden, sodass Sie nach Abschluss des Kopiervorgang mit diesen Anmeldeinformationen sofort auf die Datenbank zugreifen können. Wenn Sie [Azure Active Directory](../active-directory/active-directory-whatis.md) verwenden, müssen Sie die Anmeldeinformationen in der Kopie überhaupt nicht verwalten. Wenn Sie jedoch die Datenbank auf einen neuen Server kopieren, funktioniert der anmeldungsbasierte Zugriff ggf. nicht, da die Anmeldungen auf dem neuen Server nicht vorhanden sind. Informationen zum Verwalten von Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). 

Zum Kopieren einer SQL-Datenbank benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, klicken Sie einfach oben auf dieser Seite auf den Link **Kostenloses Konto** klicken. Lesen Sie anschließend den Artikel weiter.
* Eine zu kopierende SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Kopieren einer Datenbank mithilfe des Azure-Portals finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md) .
* Informationen zum Kopieren einer Datenbank mithilfe von PowerShell finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md) .
* Informationen zum Kopieren einer Datenbank mithilfe von Transact-SQL finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md) .
* Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md) .

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Verwalten von Anmeldungen](sql-database-manage-logins.md)
* [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)
* [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md)
* [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)




<!--HONumber=Feb17_HO2-->


