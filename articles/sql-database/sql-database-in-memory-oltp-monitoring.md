---
title: "Überwachen von XTP-In-Memory-Speicher | Microsoft Docs"
description: "Einschätzen und Überwachen der XTP-In-Memory-Speicherverwendung und -kapazität; Beheben des Kapazitätsfehlers 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jodebrui
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: e953b60493c5a7c7a7ad74533471bd321d42abef
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="monitor-in-memory-oltp-storage"></a>Überwachen des In-Memory-OLTP-Speichers
Bei der Verwendung von [In-Memory-OLTP](sql-database-in-memory.md)befinden sich die Daten der speicheroptimierten Tabellen und Tabellenvariablen im In-Memory-OLTP-Speicher. Jeder Premium-Tarif weist eine maximale In-Memory-OLTP-Speichergröße auf, die in „Ressourceneinschränkungen für Azure SQL-Datenbank“ in den Abschnitten zu den [Ressourceneinschränkungen für einzelne Datenbanken](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) und [Ressourceneinschränkungen für elastische Pools](sql-database-resource-limits.md#elastic-pool-change-storage-size) dokumentiert ist. Wenn dieses Limit überschritten wird, treten bei Einfüge- und Aktualisierungsvorgängen möglicherweise Fehler auf (Fehlercode: 41823). An diesem Punkt müssen Sie entweder Daten löschen, um Speicherplatz freizugeben, oder ein Upgrade der Leistungsstufe Ihrer Datenbank durchführen.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Bestimmen, ob genügend In-Memory-Speicherplatz für die Daten vorhanden ist
Bestimmen Sie die Speicherkapazitäten der verschiedenen Premium-Dienstebenen. Siehe Abschnitte zu den [Ressourceneinschränkungen für einzelne Datenbanken](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) und [Ressourceneinschränkungen für elastische Pools](sql-database-resource-limits.md#elastic-pool-change-storage-size) in „Ressourceneinschränkungen für Azure SQL-Datenbank“.

Das Einschätzen des Speicherbedarfs für eine speicheroptimierte Tabelle funktioniert für SQL Server genauso wie in Azure SQL-Datenbank. Lesen Sie die Informationen hierzu in [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Beachten Sie, dass sowohl die Tabellen- und Tabellenvariablenzeilen als auch die Indizes in die maximale Größe für Benutzerdaten eingerechnet werden. Darüber hinaus benötigt ALTER TABLE genügend Platz, um eine neue Version der gesamten Tabelle und ihrer Indizes zu erstellen.

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung
Sie können die Nutzung von In-Memory-Speicher als Prozentsatz der Speicherkapazität für Ihre Leistungsstufe im [Azure-Portal](https://portal.azure.com/) überwachen: 

1. Suchen Sie auf dem Blatt „Datenbank“ das Feld „Ressourcenverwendung“, und klicken Sie auf „Bearbeiten“.
2. Wählen Sie die Metrik `In-Memory OLTP Storage percentage` aus.
3. Um eine Warnung hinzuzufügen, klicken Sie auf das Feld „Ressourcenverwendung“, um das Blatt „Metrik“ zu öffnen, und klicken Sie dann auf „Warnung hinzufügen“.

Oder verwenden Sie folgende Abfrage, um die In-Memory-Speicherverwendung anzuzeigen:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Korrigieren von Fehlern aufgrund von fehlendem Speicherplatz – Fehler 41823
Wenn nicht genügend Speicherplatz zur Verfügung steht, treten bei den Vorgängen INSERT, UPDATE und CREATE Fehler mit Fehlercode 41823 auf.

Der Fehlercode 41823 weist darauf hin, dass die speicheroptimierten Tabellen und Tabellenvariablen die maximale Speichergröße überschritten haben.

Beheben Sie den Fehler mit einer der folgenden Methoden:

* Löschen Sie Daten aus den speicheroptimierten Tabellen – Sie können die Daten beispielsweise in herkömmliche datenträgerbasierte Tabellen auslagern.
* Führen Sie ein Upgrade auf eine Dienstebene durch, die genügend In-Memory-Speicher für die Daten bietet, die Sie in In-Memory-Tabellen speichern müssen.

## <a name="next-steps"></a>Nächste Schritte
Eine Anleitung zur Überwachung finden Sie unter [Überwachen von Azure SQL-Datenbank mit dynamischen Verwaltungssichten](sql-database-monitoring-with-dmvs.md).

