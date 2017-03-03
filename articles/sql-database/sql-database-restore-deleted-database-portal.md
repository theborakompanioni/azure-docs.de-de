---
title: "Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal) | Microsoft Docs"
description: "Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal)"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 33b0c9e6-1cd2-4fd9-9b0d-70ecf6e54821
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.date: 10/12/2016
ms.author: sstein
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 8d988aa55d053d28adcf29aeca749a7b18d56ed4
ms.openlocfilehash: 9ea5287884cd7f1eb7314002b3ae98c9259686c3
ms.lasthandoff: 02/16/2017


---
# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal

## <a name="select-the-database-to-restore"></a>Auswählen der wiederherzustellenden Datenbank
So stellen Sie eine gelöschte Datenbank im Azure-Portal wieder her:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **Weitere Dienste** > **SQL-Server**.
2. Wählen Sie den Server aus, der die Datenbank enthält, die Sie wiederherstellen möchten.
3. Scrollen Sie auf dem Serverblatt nach unten zum Bereich **Vorgänge**, und wählen Sie **Gelöschte Datenbanken** aus: ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
4. Wählen Sie die Datenbank aus, die Sie wiederherstellen möchten.
5. Geben Sie einen Datenbanknamen an, und klicken Sie auf **OK**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)


