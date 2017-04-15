---
title: Tools zum Verwalten und Entwickeln mit Azure SQL-Datenbank | Microsoft-Dokumentation
description: "In diesem Artikel werden die Tools und Optionen zum Verwalten und Entwickeln für Azure SQL-Datenbank vorgestellt"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 37767380-975f-4dee-a28d-80bc2036dda3
ms.service: sql-database
ms.custom: manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 9b0a3a314e576db2133e5c63fada43bb11a4e520
ms.lasthandoff: 03/04/2017


---
# <a name="overview-tools-to-manage--develop-with-azure-sql-database"></a>Übersicht: Tools zum Verwalten und Entwickeln mit Azure SQL-Datenbank
In diesem Thema werden die Tools für die Verwaltung und Entwicklung mit Azure SQL-Datenbank vorgestellt.

> [!IMPORTANT]
> In dieser Dokumentation finden Sie Schnellstartvorlagen, Beispiele und Anleitungen, die darstellen, wie Sie mit Azure SQL-Datenbank mithilfe der in den folgenden Abschnitten vorgestellten Tools verwalten und entwickeln können. Verwenden Sie den Navigationsbereich auf der linken Seite und das Filterfeld, um nach bestimmten Inhalten für das Azure-Portal, PowerShell und T-SQL zu suchen.
>

## <a name="azure-portal"></a>Azure-Portal
Das [Azure-Portal](https://portal.azure.com) ist eine webbasierte Anwendung, in der Sie Datenbanken und logische Server erstellen, aktualisieren und löschen sowie Datenbankaktivitäten überwachen können. Dieses Tool eignet sich hervorragend, wenn Sie gerade erst mit der Verwendung von Azure begonnen haben, nur wenige Datenbanken verwalten oder bestimmte Aktionen schnell ausführen müssen.

## <a name="sql-server-management-studio-and-transact-sql"></a>SQL Server Management Studio und Transact-SQL
SQL Server Management Studio (SSMS) ist ein Clienttool, das auf Ihrem Computer für die Verwaltung Ihrer Datenbank in der Cloud mithilfe von Transact-SQL ausgeführt wird. Viele Datenbankadministratoren sind vertraut mit SSMS, das mit Azure SQL-Datenbanken eingesetzt werden kann. [Laden Sie die neueste Version von SSMS herunter](https://msdn.microsoft.com/library/mt238290) , und verwenden Sie bei der Arbeit mit Azure SQL-Datenbank stets die neueste Version. 

> [!IMPORTANT]
> Verwenden Sie immer die aktuelle Version von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290).
>  

## <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools in Visual Studio
SQL Server Data Tools (SSDT) ist ein Clienttool, das auf Ihrem Computer für die Entwicklung Ihrer Datenbank in der Cloud ausgeführt wird. Wenn Sie ein Anwendungsentwickler sind, der mit Visual Studio oder anderen integrierten Entwicklungsumgebungen (IDEs) vertraut ist, sollten Sie [SSDT in Visual Studio ausprobieren](https://msdn.microsoft.com/library/mt204009.aspx).  

> [!IMPORTANT]
> Verwenden Sie immer die neueste Version von [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx), um immer mit den Updates von Microsoft Azure und SQL-Datenbank synchronisiert zu sein.
>  
## <a name="powershell"></a>PowerShell
Sie können PowerShell zur Verwaltung von Datenbanken und elastischen Pools sowie zur Automatisierung von Azure-Ressourcenbereitstellungen verwenden. Microsoft empfiehlt, dieses Tool zur Verwaltung einer großen Anzahl von Datenbanken und zur Automatisierung von Bereitstellungs- und Ressourcenänderungen in einer Produktionsumgebung zu verwenden.

## <a name="elastic-database-tools"></a>Tools für elastische Datenbanken
Verwenden Sie Tools für elastische Datenbanken, um folgende Aktionen auszuführen: 

* Ausführen eines T-SQL-Skripts für einen Satz von Datenbanken mithilfe eines [elastischen Auftrags](sql-database-elastic-jobs-overview.md)
* Verschieben von Datenbanken aus einem mehrinstanzenfähigen Modell zu einem Modell mit einem einzelnen Mandanten mithilfe des [Split-Merge-Tools](sql-database-elastic-scale-overview-split-and-merge.md)
* Verwalten von Datenbanken in einem Modell mit einem einzelnen Mandanten oder einem mehrinstanzenfähigen Modell mithilfe der [Clientbibliothek für die elastische Skalierung](sql-database-elastic-database-client-library.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
* [Azure Automation](https://azure.microsoft.com/documentation/services/automation/)
* [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)


