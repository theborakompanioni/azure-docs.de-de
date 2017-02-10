---
title: "Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Azure SQL-Datenbank | Microsoft Docs"
description: "Kurzübersicht über das Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Datenbank"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 12/07/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 6d729f89322b67fe75d2c0ad001e70b42ccedb7e
ms.openlocfilehash: ae35dd15c386bbbf4e77a143d245efcd1f44f238


---
# <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-an-azure-sql-database"></a>Anzeigen des ältesten Wiederherstellungspunkts aus den vom Dienst generierten Sicherungen einer Azure SQL-Datenbank

In diesem Thema erfahren Sie, wie Sie den ältesten Wiederherstellungspunkt aus den vom Dienst generierten Sicherungen einer Azure SQL-Datenbank anzeigen.

## <a name="view-the-oldest-restore-point-using-the-azure-portal"></a>Anzeigen des ältesten Wiederherstellungspunkts über das Azure-Portal

1. Öffnen Sie das Blatt **SQL-Datenbank** für Ihre Datenbank.

    ![Blatt für neue Beispieldatenbank](./media/sql-database-get-started/new-sample-db-blade.png)

2. Klicken Sie auf der Symbolleiste auf **Wiederherstellen**.

    ![„Wiederherstellen“ auf der Symbolleiste](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

3. Überprüfen Sie auf dem Blatt „Wiederherstellen“ den ältesten Wiederherstellungspunkt.

    ![Ältester Wiederherstellungspunkt](./media/sql-database-get-started-backup-recovery/oldest-restore-point.png)

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](: https://azure.microsoft.com/en-us/documentation/articles/)sql-database-automated-backups.MD.
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
- Weitere Informationen zum Wiederherstellen von Daten aus Sicherungen finden Sie im Artikel zur [Wiederherstellung aus einer Sicherung](sql-database-recovery-using-backups.md).


<!--HONumber=Dec16_HO2-->


