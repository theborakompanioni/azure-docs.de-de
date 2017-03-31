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
ms.author: sashan
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: e64abfd5581c02b609707f7fa712962c024b293b
ms.lasthandoff: 03/23/2017


---
# <a name="copy-an-azure-sql-database"></a>Kopieren einer Azure SQL-Datenbank

Sie können die Datenbankkopie auf dem gleichen Server oder auf einem anderen Server erstellen. Die Datenbankkopie ist eine Momentaufnahme der Quelldatenbank zum Zeitpunkt der Kopieranforderung. Dienst- und Leistungsebene (Tarif) der Datenbankkopie stimmen standardmäßig mit den Ebenen der Quelldatenbank überein. Wenn Sie die API verwenden, können Sie eine andere Leistungsebene innerhalb der gleichen Dienstebene (Edition) auswählen. Nachdem der Kopiervorgang abgeschlossen ist, wird die Kopie zu einer voll funktionsfähigen, unabhängigen Datenbank. Zu diesem Zeitpunkt können Sie ein Upgrade oder Downgrade auf jede beliebige Edition durchführen. Anmeldungen, Benutzer und Berechtigungen können unabhängig verwaltet werden.  

Wenn Sie eine Datenbank auf denselben logischen Server kopieren, können für beide Datenbanken die gleichen Anmeldedaten verwendet werden. Das Sicherheitsprinzipal, das Sie zum Kopieren der Datenbank verwenden, wird zum Datenbankbesitzer (DBO) der neuen Datenbank. Alle Datenbankbenutzer, ihre Berechtigungen und ihre Sicherheits-IDs (SIDs) werden in die Kopie der Datenbank kopiert.  

Wenn Sie eine Datenbank auf einen anderen logischen Server kopieren, wird der Sicherheitsprinzipal auf dem neuen Server zum Datenbankbesitzer für die neue Datenbank. Wenn Sie [eigenständige Datenbankbenutzer](sql-database-manage-logins.md) für den Datenzugriff verwenden, stellen Sie sicher, dass sowohl für die primäre als auch für die sekundäre Datenbank immer die gleichen Anmeldeinformationen verwendet werden, sodass Sie nach Abschluss des Kopiervorgang mit diesen Anmeldeinformationen sofort auf die Datenbank zugreifen können. Wenn Sie [Azure Active Directory](../active-directory/active-directory-whatis.md) verwenden, müssen Sie die Anmeldeinformationen in der Kopie überhaupt nicht verwalten. Wenn Sie jedoch die Datenbank auf einen neuen Server kopieren, funktioniert der anmeldungsbasierte Zugriff ggf. nicht, da die Anmeldungen auf dem neuen Server nicht vorhanden sind. Informationen zum Verwalten von Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). 

Sie können eine SQL-Datenbank über das [Azure-Portal](sql-database-copy-portal.md), mithilfe von [PowerShell](scripts/sql-database-copy-database-to-new-server-powershell.md) oder über [T-SQL](sql-database-copy-transact-sql.md) kopieren. 

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md) .
* Informationen zu Anmeldungen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).
* Informationen zum Exportieren einer Datenbank finden Sie unter [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md).

