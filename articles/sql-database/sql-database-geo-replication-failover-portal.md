---
title: "Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal | Microsoft Docs"
description: "Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: a9d184a4-09e0-4f41-b364-40425f68f430
ms.service: sql-database
ms.custom: business continuity; how to
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 11/22/2016
ms.author: sashan;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 09c2332589b1170b411c6f45f4109fb8048887e2
ms.openlocfilehash: 7354f020d0a3a80775adf6d3b1e519ac6c24fdf6


---
# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-geo-replication-failover-portal.md)
> * [PowerShell](sql-database-geo-replication-failover-powershell.md)
> * [T-SQL](sql-database-geo-replication-failover-transact-sql.md)
> 
> 

In diesem Artikel wird beschrieben, wie Sie mit dem [Azure-Portal](http://portal.azure.com)ein Failover zu einer sekundären SQL-Datenbank konfigurieren. Informationen zum Konfigurieren der Georeplikation finden Sie unter [Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md).

## <a name="initiate-a-failover"></a>Initiieren eines Failovers
Für die sekundäre Datenbank kann ein Wechsel durchgeführt werden, bei dem sie zur primären Datenbank wird.  

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zur primären Datenbank in der Georeplikationspartnerschaft.
2. Wählen Sie auf dem Blatt „SQL-Datenbank“ **Alle Einstellungen** > **Georeplikation**.
3. Wählen Sie in der Liste **SEKUNDÄRE DATENBANKEN** die Datenbank aus, die zur neuen primären Datenbank werden soll. Klicken Sie anschließend auf **Failover**.
   
    ![Failover][2]
4. Klicken Sie auf **Ja** , um das Failover zu beginnen.

Durch den Befehl wird die sekundäre Datenbank sofort in die primäre Rolle geändert. 

Es gibt einen kurzer Zeitraum, in dem beide Datenbanken während des Rollenwechsels (ca. 0 bis 25 Sekunden) nicht verfügbar sind. Wenn die primäre Datenbank über mehrere sekundäre Datenbanken verfügt, werden die anderen sekundären Datenbanken durch den Befehl automatisch neu konfiguriert, sodass sie eine Verbindung mit der neuen primären Datenbank herstellen. Unter normalen Umständen dauert der gesamte Vorgang nicht länger als 1 Minute. 

> [!NOTE]
> Wenn die primäre Datenbank bei Ausgabe des Befehls online ist und gerade Commits für Transaktionen ausführt, können einige Daten verloren gehen.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Stellen Sie nach dem Failover sicher, dass die Authentifizierungsanforderungen für Ihren Server und Ihre Datenbank auf der neuen primären konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
* Informationen zur Wiederherstellung nach einem Ausfall mithilfe der aktiven Georeplikation, einschließlich der vor und nach der Wiederherstellung erforderlichen Schritte und der Ausführung eines Notfallwiederherstellungsverfahrens, finden Sie unter [Notfallwiederherstellungsverfahren](sql-database-disaster-recovery.md)
* Den Blogpost von Sasha Nosov über die aktive Georeplikation finden Sie unter [Spotlight on new Geo-Replication capabilities](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
* Informationen zum Vorbereiten von Cloudanwendungen für die aktive Georeplikation finden Sie unter [Entwerfen von Cloudanwendungen zum Sicherstellen der Geschäftskontinuität mithilfe der Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* Informationen zur Verwendung der aktiven Georeplikation mit elastischen Datenbankpools finden Sie unter [Strategien zur Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
* Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)

<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png



<!--HONumber=Dec16_HO1-->


