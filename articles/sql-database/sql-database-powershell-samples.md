---
title: "Azure PowerShell-Beispiele für SQL-Datenbank | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispiele – Skripts zum Erstellen und Verwalten von Azure SQL-Datenbank-Servern, Pools für elastische Datenbanken, Datenbanken und Firewalls."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 03/07/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: abcccabbfc56102c97ef2408fb812755e21b467c
ms.lasthandoff: 03/22/2017

---

# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure PowerShell-Beispiele für Azure SQL-Datenbank

Die folgende Tabelle enthält Links zu Azure PowerShell-Beispielskripts für Azure SQL-Datenbank.

| |  |
|---|---|
|**Erstellen einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Create a single SQL database and configure a firewall rule using PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) (Erstellen einer einzelnen SQL-Datenbank und Konfigurieren einer Firewallregel mit PowerShell) | Erstellt eine einzelne Azure SQL-Datenbank und konfiguriert eine Firewallregel auf Serverebene. |
| [Create elastic pools and move databases between pools and out of a pool using PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) (Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools und aus einem Pool mit PowerShell) | Erstellt Pools für elastische Datenbanken, verschiebt in einem Pool zusammengefasste Datenbanken und ändert Leistungsstufen.|
|**Konfigurieren von Georeplikation und Failover**||
| [Configure Active Geo-Replication for a single Azure SQL database using PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) (Konfigurieren der aktiven Georeplikation für eine einzelne Azure SQL-Datenbank mit PowerShell)| Konfiguriert die aktive Georeplikation für eine einzelne Azure SQL-Datenbank und führt ein Failover zum sekundären Replikat aus. |
| [Configure Active Geo-Replication for a pooled Azure SQL database using PowerShell](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md) (Konfigurieren der aktiven Georeplikation für eine in einem Pool enthaltene Azure SQL-Datenbank mit PowerShell)| Konfiguriert die aktive Georeplikation für eine Azure SQL-Datenbank in einem Pool für elastische Datenbanken und führt ein Failover zum sekundären Replikat aus. |
|**Skalieren einer einzelnen Datenbank und eines Pools für elastische Datenbanken**||
| [Monitor and scale a single SQL database using PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md) (Überwachen und Skalieren einer einzelnen SQL-Datenbank mit PowerShell) | Überwacht die Leistungsmetriken einer Azure SQL-Datenbank, skaliert sie auf eine höhere Leistungsstufe und erstellt eine Warnungsregel auf einer der Leistungsmetriken. |
| [Monitor and scale a SQL Database elastic pool using PowerShell](scripts/sql-database-monitor-and-scale-pool-powershell.md) (Überwachen und Skalieren eines Pools für elastische SQL-Datenbanken mit PowerShell) | Überwacht die Leistungsmetriken eines Pools für elastische Datenbanken, skaliert sie auf eine höhere Leistungsstufe und erstellt eine Warnungsregel auf einer der Leistungsmetriken.  |
| **Überwachung und Bedrohungserkennung** |
| [Configure SQL Database auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md) (Konfigurieren der Überwachung und Bedrohungserkennung für SQL-Datenbank mit PowerShell)| Konfiguriert Richtlinien zur Überwachung und Bedrohungserkennung für eine Azure SQL-Datenbank. |
| **Wiederherstellen, Kopieren und Importieren einer Datenbank**||
| [Restore a SQL database using PowerShell (Wiederherstellen einer SQL-Datenbank mit PowerShell)](scripts/sql-database-restore-database-powershell.md)| Stellt eine Azure SQL-Datenbank auf Basis einer georedundanten Sicherung und eine gelöschte Azure SQL-Datenbank gemäß der letzten Sicherung wieder her. |
| [Copy a SQL database to a new server using PowerShell](scripts/sql-database-copy-database-to-new-server-powershell.md) (Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell)| Erstellt eine Kopie einer vorhandenen Azure SQL-Datenbank auf einem neuen Azure SQL-Server. |
| [Import from a bacpac into a SQL database using PowerShell](scripts/sql-database-import-from-bacpac-powershell.md) (Importieren aus einer BACPAC-Datei in eine SQL-Datenbank mit PowerShell)| Importiert eine Datenbank aus einer BACPAC-Datei auf einen Azure SQL-Server. |
|||

