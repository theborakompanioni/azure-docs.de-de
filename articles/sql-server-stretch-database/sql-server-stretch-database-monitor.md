---
title: "Überwachen und Behandeln von Problemen der Datenmigration (Stretch Database) | Microsoft Docs"
description: "Erfahren Sie Einzelheiten über das Überwachen des Status der Datenmigration."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 2341d446-9909-4694-8bb8-d288582daf54
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 4efcb1b30f002d7baecaa46f9994dfbc9ad42dee


---
# <a name="monitor-and-troubleshoot-data-migration-stretch-database"></a>Überwachen und Behandeln von Problemen der Datenmigration (Stretch-Datenbank)
Wählen Sie **Aufgaben | Stretch | Überwachung** für eine Datenbank in SQL Server Management Studio aus, um die Datenmigration im Stretch Database Monitor zu überwachen.

## <a name="check-the-status-of-data-migration-in-the-stretch-database-monitor"></a>Prüfen des Status der Datenmigration in Stretch Database Monitor
Wählen Sie **Aufgaben | Stretch | Überwachung** für eine Datenbank in SQL Server Management Studio aus, um Stretch Database Monitor zu öffnen und die Datenmigration zu überwachen.

* Der obere Teil der Monitors zeigt allgemeine Informationen über die Stretch\-fähigen SQL Server-Datenbank und die Azure-Remotedatenbank an.
* Im unteren Teil der Monitors wird der Status der Datenmigration für jede Stretch\-fähige Tabelle in der Datenbank angezeigt.

![Stretch Database Monitor][StretchMonitorImage1]

## <a name="a-namemigrationacheck-the-status-of-data-migration-in-a-dynamic-management-view"></a><a name="Migration"></a>Prüfen des Status der Datenmigration in einer dynamischen Verwaltungsansicht
Öffnen Sie die dynamische Verwaltungssicht **sys.dm\_db\_rda\_migration\_status**, um zu ermitteln, wie viele Batches und Zeilen mit Daten migriert wurden. Weitere Informationen finden Sie unter [sys.dm_db_rda_migration_status (Transact-SQL)](https://msdn.microsoft.com/library/dn935017.aspx).

## <a name="a-namefirewallatroubleshoot-data-migration"></a><a name="Firewall"></a>Beheben von Problemen bei der Datenmigration
**Zeilen aus meiner Stretch-aktivierten Tabelle werden nicht zu Azure migriert. Was ist das Problem?**

Es gibt mehrere Probleme, die eine Migration beeinflussen können. Überprüfen Sie die folgenden Punkte.

* Überprüfen Sie die Netzwerkkonnektivität für den SQL Server-Computer.
* Stellen Sie sicher, dass die Azure-Firewall Ihren SQL Server nicht daran hindert, eine Verbindung mit dem Remoteendpunkt herzustellen.
* Suchen Sie in der dynamischen Verwaltungsansicht **sys.dm\_db\_rda\_migration\_status** nach dem Status des aktuellen Batches. Wenn ein Fehler aufgetreten ist, überprüfen Sie die Werte error\_number, error\_state und error\_severity für den Batch.
  
  * Weitere Informationen über die Ansicht finden Sie unter [sys.dm_db_rda_migration_status (Transact-SQL)](https://msdn.microsoft.com/library/dn935017.aspx).
  * Weitere Informationen zum Inhalt einer SQL Server-Fehlermeldung finden Sie unter [sys.messages (Transact-SQL)](https://msdn.microsoft.com/library/ms187382.aspx).

**Die Azure-Firewall blockiert Verbindungen von meinem lokalen Server.**

Möglicherweise müssen Sie in den Einstellungen der Azure-Firewall des Azure-Servers eine Regel hinzufügen, um SQL Server zu erlauben, mit dem Azure-Remoteserver zu kommunizieren.

## <a name="see-also"></a>Weitere Informationen
[Verwalten von Stretch Database und Behandeln von Problemen](sql-server-stretch-database-manage.md)

<!--Image references-->
[StretchMonitorImage1]: ./media/sql-server-stretch-database-monitor/StretchDBMonitor.png



<!--HONumber=Dec16_HO2-->


