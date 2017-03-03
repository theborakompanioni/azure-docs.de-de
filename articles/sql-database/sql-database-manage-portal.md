---
title: "Verwalten von Azure SQL-Datenbanken über das Azure-Portal | Microsoft Docs"
description: "Kurzübersicht über die Verwendung des Azure-Portals zum Verwalten einer relationalen Datenbank mithilfe des Azure-Portals in der Cloud"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3a56e9de-c21a-40ba-9a35-958172cb4e5b
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 01/10/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: ae230c012a17eb73c8993a32197c844c6abaa2a4
ms.openlocfilehash: 130f2341eca570a982ab5c22ba429f6b48006f90
ms.lasthandoff: 02/17/2017


---
# <a name="manage-azure-sql-databases-using-the-azure-portal"></a>Verwalten von Azure SQL-Datenbanken über das Azure-Portal

Das [Azure-Portal](https://portal.azure.com/) ermöglicht das Erstellen, Überwachen und Verwalten von Azure SQL-Datenbanken und -Servern. Dieser Artikel enthält eine kurze Beschreibung und Links zu den Details der gängigen Aufgaben.

> [!TIP]
> Das [Tutorial zu den ersten Schritten](sql-database-get-started.md) veranschaulicht, wie Sie einen Server und eine serverbasierte Firewall erstellen, Servereigenschaften anzeigen, eine Verbindung mithilfe von SQL Server Management Studio herstellen, die Masterdatenbank abfragen, eine Beispieldatenbank und eine leere Datenbank erstellen, Datenbankeigenschaften abfragen, und die Beispieldatenbank abfragen.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Anzeigen Ihrer Azure SQL-Datenbanken, -Server und -Pools
Zum Anzeigen der verfügbaren Dienste für die SQL-Datenbank klicken Sie auf **More services** (Weitere Dienste) und geben im Suchfeld **SQL** ein:

![SQL-Datenbank](./media/sql-database-manage-portal/sql-services.png)

## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Wie kann ich Azure SQL-Datenbanken erstellen oder anzeigen?
Zum Öffnen des Blatts **SQL-Datenbanken** klicken Sie auf **SQL-Datenbanken** und klicken anschließend auf den Server, mit dem Sie arbeiten möchten, oder klicken Sie auf **+ Hinzufügen**, um eine SQL-Datenbank zu erstellen. Details finden Sie unter [Erstellen, Herstellen der Verbindung und Abfragen Ihrer ersten Azure SQL-Datenbank-Instanz im Azure-Portal und Verwendung von SSMS](sql-database-get-started.md).

![SQL-Datenbanken](./media/sql-database-manage-portal/sql-databases.png)

## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Wie kann ich Azure SQL-Server erstellen oder anzeigen?
Zum Öffnen des Blatts **SQL Server** klicken Sie auf **SQL Server** und klicken anschließend auf den Server, mit dem Sie arbeiten möchten, oder klicken Sie auf **+Hinzufügen**, um einen SQL-Server zu erstellen. Details finden Sie unter [Erstellen, Herstellen der Verbindung und Abfragen Ihrer ersten Azure SQL-Datenbank-Instanz im Azure-Portal und Verwendung von SSMS](sql-database-get-started.md).

![SQL-Server](./media/sql-database-manage-portal/sql-servers.png)

## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Wie kann ich elastische SQL-Pools erstellen oder anzeigen?
Zum Öffnen des Blatts **Elastische SQL-Pools** klicken Sie auf **Elastische SQL-Pools** und klicken anschließend auf den Pool, mit dem Sie arbeiten möchten, oder klicken Sie auf **+ Hinzufügen**, um einen Pool zu erstellen. Details finden Sie unter [Erstellen eines elastischen Pools mit dem Azure-Portal](sql-database-elastic-pool-manage-portal.md).

![Elastische SQL-Pools](./media/sql-database-manage-portal/elastic-pools.png)

## <a name="how-do-i-update-or-view-sql-database-settings"></a>Wie kann ich SQL-Datenbankeinstellungen aktualisieren oder anzeigen?
Zum Anzeigen oder Aktualisieren Ihrer Datenbankeinstellungen klicken Sie auf dem Blatt „SQL-Datenbank“ auf die gewünschte Einstellung:

![Einstellungen der SQL-Datenbank](./media/sql-database-manage-portal/settings.png)

## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Wie finde ich den vollqualifizierten Servernamen einer SQL-Datenbank?
Zum Anzeigen des Servernamens Ihrer Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Übersicht** und notieren den Servernamen:

![Einstellungen der SQL-Datenbank](./media/sql-database-manage-portal/server-name.png)

## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Wie verwalte ich Firewallregeln zum Steuern des Zugriffs auf meinen SQL-Server und die Datenbank?
Zum Anzeigen, Erstellen oder Aktualisieren von Firewallregeln klicken Sie auf dem Blatt **SQL-Datenbank** auf **Serverfirewall festlegen**. Details finden Sie unter [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).

![Firewallregeln](./media/sql-database-manage-portal/sql-database-firewall.png)

## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Wie ändere ich die Dienstebene oder Leistungsstufe meiner SQL-Datenbank?
Zum Aktualisieren der Dienstebene oder Leistungsstufe einer SQL-Datenbank klicken Sie auf dem Blatt **Tarif (DTUs skalieren)** auf **SQL-Datenbank**. Details finden Sie unter [Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank](sql-database-service-tiers.md).

![Tarife](./media/sql-database-manage-portal/pricing-tier.png)

## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Wie konfiguriere ich die Überwachung und Bedrohungserkennung für eine SQL-Datenbank?
Zum Konfigurieren der Überwachung und Bedrohungserkennung für eine SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Überwachung und Bedrohungserkennung**. Details finden Sie unter [Datenbanküberwachung](sql-database-auditing.md) und [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection.md).

## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Wie konfiguriere ich die dynamische Datenmaskierung für eine SQL-Datenbank?
Zum Konfigurieren der dynamischen Datenmaskierung für eine SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Dynamische Datenmaskierung**. Details finden Sie unter [Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbanken](sql-database-dynamic-data-masking-get-started.md).

## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Wie konfiguriere ich Transparent Data Encryption (TDE) für eine SQL­Datenbank?
Zum Konfigurieren der dynamischen Datenverschlüsselung klicken Sie auf dem Blatt **SQL-Datenbank** auf **Transparent Data Encryption**. Details finden Sie unter [Aktivieren von TDE in einer Datenbank mithilfe des Portals](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Wie kann ich die maximale Größe einer SQL-Datenbank anzeigen oder ändern?
Zum Anzeigen oder Ändern der Größe einer SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Datenbankgröße**. Aktualisieren Sie die maximale Größe einer Datenbank durch Ändern der Dienstebene oder Leistungsstufe. Details finden Sie unter [Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank](sql-database-service-tiers.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Wie kann ich die Leistung einer SQL-Datenbank überwachen und verbessern?
Zum Überwachen und Verbessern der Leistungsmerkmale einer SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Leistungsübersicht**. Details finden Sie unter [Einblicke in die SQL-Datenbankleistung](sql-database-performance.md).

## <a name="how-do-i-configure-geo-replication"></a>Wie konfiguriere ich die Georeplikation?
Zum Einrichten der Georeplikation für eine SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Georeplikation**. Details finden Sie unter [Konfigurieren der Georeplikation für Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md).

## <a name="how-do-i-fail-over-to-a-geo-replicated-sql-database"></a>Wie führe ich ein Failover auf eine georeplizierte sekundäre Datenbank durch?
Für ein Failover auf eine georeplizierte sekundäre Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Georeplikation** und anschließend auf **Failover**. Details finden Sie unter [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-failover-portal.md).

## <a name="how-do-i-copy-a-sql-database"></a>Wie kopiere ich eine SQL-Datenbank?
Zum Kopieren einer SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Kopieren**. Details finden Sie unter [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md).

![Einstellungen der SQL-Datenbank](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Wie archiviere ich eine Azure SQL-Datenbank in eine BACPAC-Datei?
Zum Erstellen einer BACPAC-Datei einer SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Exportieren**. Details finden Sie unter [Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals](sql-database-export.md).

![SQL-­Datenbankexport](./media/sql-database-manage-portal/sql-database-export.png)

## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Wie stelle ich einen früheren Zustand einer SQL-Datenbank wieder her?
Zum Wiederherstellen einer SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Wiederherstellen**. Details finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-point-in-time-restore-portal.md).

![Einstellungen der SQL-Datenbank](./media/sql-database-manage-portal/sql-database-restore.png)

## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Wie erstelle ich eine Azure SQL-Datenbank aus einer BACPAC-Datei?
Zum Erstellen einer SQL-Datenbank aus einer BACPAC-Datei klicken Sie auf dem Blatt **SQL-Server** auf **Datenbank importieren**. Details finden Sie unter [Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank](sql-database-import-portal.md).

![Datenbank importieren](./media/sql-database-manage-portal/server-commands.png)

## <a name="how-do-i-restore-a-deleted-sql-database"></a>Wie stelle ich eine gelöschte SQL-Datenbank wieder her?
Zum Wiederherstellen einer gelöschten SQL­Datenbank klicken Sie auf dem Blatt **SQL Server** (SQL-Server, der die gelöschte Datenbank enthalten hat) auf **Gelöschte Datenbanken**. Details finden Sie unter [Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Wie lösche ich eine SQL-Datenbank?
Zum Löschen einer SQL-Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Löschen**. 

![Einstellungen der SQL-Datenbank](./media/sql-database-manage-portal/sql-database-delete.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [SQL-Datenbank](sql-database-technical-overview.md)
* [Überwachen und Verwalten eines elastischen Pools über das Azure-Portal](sql-database-elastic-pool-manage-portal.md)


