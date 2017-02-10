---
title: "Übersicht: Verwaltungstools für SQL-Datenbank | Microsoft Docs"
description: Vergleicht die Tools und Optionen zum Verwalten von Azure SQL-Datenbank
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 37767380-975f-4dee-a28d-80bc2036dda3
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: 915292a191da82af9e24e89460d46dc61a062b18


---
# <a name="overview-management-tools-for-sql-database"></a>Übersicht: Verwaltungstools für SQL-Datenbank
In diesem Thema werden Tools und Optionen für die Verwaltung von Azure SQL-Datenbanken untersucht und verglichen, damit Sie das Tool auswählen können, das für den Auftrag, für Ihr Unternehmen und für Sie selbst am besten geeignet ist. Die Auswahl des richtigen Tools hängt von der Anzahl der Datenbanken, den Aufgaben und der Ausführungshäufigkeit einer Aufgabe ab.

## <a name="azure-portal"></a>Azure-Portal
Das [Azure-Portal](https://portal.azure.com) ist eine webbasierte Anwendung, in der Sie Datenbanken und logische Server erstellen, aktualisieren und löschen sowie Datenbankaktivitäten überwachen können. Dieses Tool eignet sich hervorragend, wenn Sie gerade erst mit der Verwendung von Azure begonnen haben, nur wenige Datenbanken verwalten oder bestimmte Aktionen schnell ausführen müssen.

Weitere Informationen zum Verwenden des Portals finden Sie unter [Verwalten von SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio und SQL Server Data Tools in Visual Studio
SQL Server Management Studio (SSMS) und SQL Server Data Tools (SSDT) sind Clienttools, die auf Ihrem Computer ausgeführt werden und mit denen Sie Ihre Datenbank in der Cloud verwalten und entwickeln können. Wenn Sie ein Anwendungsentwickler sind, der mit Visual Studio oder anderen integrierten Entwicklungsumgebungen (IDEs) vertraut ist, sollten Sie [SSDT in Visual Studio ausprobieren](https://msdn.microsoft.com/library/mt204009.aspx). Viele Datenbankadministratoren sind vertraut mit SSMS, das mit Azure SQL-Datenbanken eingesetzt werden kann. [Laden Sie die neueste Version von SSMS herunter](https://msdn.microsoft.com/library/mt238290) , und verwenden Sie bei der Arbeit mit Azure SQL-Datenbank stets die neueste Version. Weitere Informationen zum Verwalten von Azure SQL-Datenbanken mit SSMS finden Sie unter [Verwalten von SQL-Datenbanken mit SSMS](sql-database-manage-azure-ssms.md).

> [!IMPORTANT]
> Verwenden Sie immer die neueste Version von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) und den [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx), damit Ihr System mit Updates von Microsoft Azure und SQL-Datenbank synchronisiert wird.
>  

## <a name="powershell"></a>PowerShell
Sie können PowerShell zur Verwaltung von Datenbanken und elastischen Pools sowie zur Automatisierung von Azure-Ressourcenbereitstellungen verwenden. Microsoft empfiehlt, dieses Tool zur Verwaltung einer großen Anzahl von Datenbanken und zur Automatisierung von Bereitstellungs- und Ressourcenänderungen in einer Produktionsumgebung zu verwenden.

Weitere Informationen finden Sie unter [Verwalten von SQL-Datenbank mit PowerShell](sql-database-manage-powershell.md).

## <a name="elastic-database-tools"></a>Tools für elastische Datenbanken
Verwenden Sie Tools für elastische Datenbanken, um folgende Aktionen auszuführen: 

* Ausführen eines T-SQL-Skripts für einen Satz von Datenbanken mithilfe eines [elastischen Auftrags](sql-database-elastic-jobs-overview.md)
* Verschieben von Datenbanken aus einem mehrinstanzenfähigen Modell zu einem Modell mit einem einzelnen Mandanten mithilfe des [Split-Merge-Tools](sql-database-elastic-scale-overview-split-and-merge.md)
* Verwalten von Datenbanken in einem Modell mit einem einzelnen Mandanten oder einem mehrinstanzenfähigen Modell mithilfe der [Clientbibliothek für die elastische Skalierung](sql-database-elastic-database-client-library.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
* [Azure Automation](https://azure.microsoft.com/documentation/services/automation/)
* [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)




<!--HONumber=Dec16_HO2-->


