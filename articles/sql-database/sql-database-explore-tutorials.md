---        
title: Kennenlernen der Azure SQL-Datenbank-Tutorials | Microsoft Docs
description: "Erfahren Sie mehr über die Features und Funktionalität von SQL-Datenbank"
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 04c0fd7f-d260-4e43-a4f0-41cdcd5e3786
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 02/08/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 60bcd02d24e2084b9020ce56ef6a9f8268c6b1b5
ms.openlocfilehash: 23d4c00f9e4c719e2124ad3c8733269712a796f0

        
---
 
# <a name="explore-azure-sql-database-tutorials"></a>Entdecken Sie Azure SQL-Datenbank-Tutorials
Über Links in den folgenden Tabellen gelangen Sie zu einer Übersicht über jeden aufgelisteten Featurebereich und zu einem einfachen Tutorial für jeden Bereich. Lösungsbezogene Schnellstarts, die die Verwendung von SQL-Datenbank in einer vollständigen Lösung basierend auf realen Szenarien veranschaulichen, finden Sie unter [Azure SQL Database Solution Quick Starts](sql-database-solution-quick-starts.md)(Lösungsbezogene Schnellstarts für Azure SQL-Datenbank).

## <a name="create-servers-databases-and-server-level-firewall-rules"></a>Erstellen von Servern, Datenbanken und Firewallregeln auf Serverebene
In den folgenden Tutorials erstellen Sie Server, Datenbanken und Firewallregeln auf Serverebene und erfahren, wie Sie eine Verbindung herstellen sowie Server und Datenbanken abfragen.

| Tutorial: | Beschreibung |
| --- | --- | 
| [Schnellstarttutorial: Ihre erste Azure SQL-Datenbank](sql-database-get-started.md) | Nach Abschluss dieses Schnellstarttutorials verfügen Sie über eine Beispieldatenbank und eine leere Datenbank, die in einer Azure-Ressourcengruppe ausgeführt wird und einem logischen Server zugeordnet ist. Sie verfügen außerdem über zwei Firewallregeln auf Serverebene, die so konfiguriert sind, dass sich der Prinzipal auf Serverebene am Server über zwei angegebene IP-Adressen anmelden kann. Zum Schluss lernen Sie, wie eine Datenbank im Azure-Portal abgefragt wird und wie mithilfe von SQL Server Management Studio eine Verbindung hergestellt und Abfragen durchgeführt werden. |
| [Tutorial: Bereitstellen und Verwenden einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-powershell.md) | Nach Abschluss dieses Tutorials verfügen Sie über eine Beispieldatenbank und eine leere Datenbank, die in einer Azure-Ressourcengruppe ausgeführt wird und einem logischen Server zugeordnet ist. Außerdem verfügen Sie über eine Firewallregel auf Serverebene, die so konfiguriert ist, dass sich der Prinzipal auf Serverebene am Server über eine angegebene IP-Adresse (oder einen IP-Adressbereich) anmelden kann. |
| [Ausprobieren von SQL-Datenbank: Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md)| In diesem Tutorial verwenden Sie C#, um einen SQL-Datenbankserver, eine Firewallregel und eine SQL-Datenbank zu erstellen. Außerdem erstellen Sie eine AD-Anwendung (Active Directory) und den erforderlichen Dienstprinzipal zum Authentifizieren der C#-App. |
|  | |

## <a name="backups-long-term-retention-and-database-recovery"></a>Sicherungen, langfristige Archivierung und Datenbankwiederherstellung
In den folgenden Tutorials erfahren Sie mehr über [Datenbanksicherungen](sql-database-automated-backups.md), die [langfristige Archivierung](sql-database-long-term-retention.md) und [die Datenbankwiederherstellung mithilfe von Sicherungen](sql-database-recovery-using-backups.md).

| Tutorial | Beschreibung |
| --- | --- | 
| [Sichern und Wiederherstellen über das Azure-Portal](sql-database-get-started-backup-recovery.md) | In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal Sicherungen anzeigen, eine zeitpunktabhängige Wiederherstellung durchführen, die langfristige Archivierung von Sicherungen konfigurieren und im Azure Recovery Services-Tresor eine Wiederherstellung aus einer Sicherung ausführen.
| [Sichern und Wiederherstellen mithilfe von PowerShell](sql-database-get-started-backup-recovery-powershell.md) | In diesem Tutorial erfahren Sie, wie Sie über das PowerShell Sicherungen anzeigen, eine zeitpunktabhängige Wiederherstellung durchführen, die langfristige Archivierung von Sicherungen konfigurieren und im Azure Recovery Services-Tresor eine Wiederherstellung aus einer Sicherung ausführen.
|  | |

## <a name="sharded-databases"></a>Sharddatenbanken
In den folgenden Tutorials erfahren mehr über [Horizontales Skalieren von Datenbanken mit dem Shardzuordnungs-Manager](sql-database-elastic-scale-shard-map-management.md).

| Tutorial | Beschreibung |
| --- | --- | 
| [Erstellen einer Anwendung mit horizontaler Partitionierung](sql-database-elastic-scale-get-started.md) |In diesem Tutorial erfahren Sie, wie Sie die Funktionen der Tools für elastische Datenbanken mithilfe einer einfachen horizontal partitionierten Anwendung verwenden. |
| [Bereitstellen eines Split-Merge-Diensts](sql-database-elastic-scale-configure-deploy-split-and-merge.md) |In diesem Tutorial erfahren Sie, wie Sie Daten zwischen zwei Datenbanken mit horizontaler Partitionierung verschieben. |
|  | |

## <a name="elastic-database-jobs"></a>Aufträge für die elastische Datenbank
In den folgenden Tutorials erfahren Sie mehr zur Verwendung von [Aufträgen für elastische Datenbanken](sql-database-elastic-jobs-overview.md).

| Tutorial | Beschreibung |
| --- | --- | 
| [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) |In diesem Tutorial erfahren Sie, wie Sie Aufträge zum Verwalten einer Gruppe aufeinander bezogener Datenbanken erstellen und verwalten können. |
|  | |

## <a name="elastic-queries"></a>Elastische Abfragen
In den folgenden Tutorials erfahren Sie mehr zur Ausführung [elastischer Abfragen](sql-database-elastic-query-overview.md).

| Tutorial | Beschreibung |
| --- | --- | 
| [Erstellen elastischer Abfragen](sql-database-elastic-query-getting-started-vertical.md) | In diesem Tutorial erfahren Sie, wie Sie T-SQL-Abfragen ausführen, die mithilfe eines einzelnen Verbindungspunkts mehrere Datenbanken umfassen. |
| [Erstellen von Berichten für horizontal hochskalierte Clouddatenbanken](sql-database-elastic-query-getting-started.md) |In diesem Tutorial erfahren Sie, wie Sie Berichte für alle Datenbanken in einer horizontal partitionierten Datenbank erstellen. |
| [Ausführen von Abfragen über Clouddatenbanken mit unterschiedlichen Schemas hinweg](sql-database-elastic-query-vertical-partitioning.md) | In diesem Tutorial erfahren Sie, wie Sie T-SQL-Abfragen ausführen, die mehrere Datenbanken mit unterschiedlichen Schemas umfassen. |
| [Erstellen von Berichten für horizontal hochskalierte Clouddatenbanken](sql-database-elastic-query-horizontal-partitioning.md) |In diesem Tutorial erfahren Sie, wie Sie Berichte erstellen, die alle Datenbanken in einer Datenbank mit horizontaler Partitionierung umfassen. |
|  | |

## <a name="database-authentication-and-authorization"></a>Datenbankauthentifizierung und Autorisierung
In den folgenden Tutorials erfahren Sie mehr über das [Erstellen und Verwalten von Anmeldungen und Benutzern](sql-database-manage-logins.md).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [SQL-Authentifizierung und -Autorisierung](sql-database-control-access-sql-authentication-get-started.md) | In diesem Tutorial erhalten Sie Informationen dazu, wie Sie Anmeldungen und Benutzer mithilfe der SQL Server-Authentifizierung erstellen, sie Rollen hinzufügen und ihnen Berechtigungen erteilen. |
| [Azure AD-Authentifizierung und -Autorisierung](sql-database-control-access-aad-authentication-get-started.md) | In diesem Tutorial erfahren Sie mehr über das Erstellen von Anmeldungen und Benutzern mithilfe der Azure Active Directory-Authentifizierung. |
|  | |

## <a name="secure-and-protect-data"></a>Sichern und Schützen von Daten
In den folgenden Tutorials erfahren Sie mehr zur [Sicherung von Daten in einer Azure SQL-Datenbank](sql-database-security-overview.md).

| Tutorial | Beschreibung |
| --- | --- | 
| [Schützen sensibler Daten mithilfe von Always Encrypted ](sql-database-always-encrypted-azure-key-vault.md) |In diesem Tutorial verwenden Sie den Always Encrypted-Assistenten, um sensible Daten in einer Azure SQL-Datenbank zu schützen. |
|  | |

## <a name="develop"></a>Entwickeln
In den folgenden Tutorials erfahren Sie mehr über die Anwendungs- und Datenbankentwicklung.

| Tutorial | Beschreibung |
| --- | --- | 
| [Erstellen eines Berichts mit Excel](sql-database-connect-excel.md) |In diesem Tutorial erfahren Sie, wie Sie für Excel eine Verbindung mit einer SQL-Datenbank in der Cloud herstellen, damit Sie Daten importieren und Tabellen und Diagramme basierend auf den Werten in der Datenbank erstellen können. |
| [Erstellen einer App mithilfe von SQL Server](https://www.microsoft.com/sql-server/developer-get-started/) |In diesem Tutorial erfahren Sie, wie Sie eine Anwendung mithilfe von SQL Server erstellen. |
| [Verwenden von Entity Framework mit elastischen Tools](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) |In diesem Tutorial erfahren Sie, welche Änderungen in einer Entity Framework-Anwendung erforderlich sind, damit diese die Funktionen der Tools für elastische Datenbanken nutzen kann. |
| [Einführen von In-Memory OLTP](sql-database-in-memory-oltp-migration.md) | In diesem Tutorial erfahren Sie, wie Sie [In-Memory OLTP](sql-database-in-memory.md) zum Verbessern der Transaktionsverarbeitungsleistung nutzen. |
|  | |

## <a name="data-sync"></a>Datensynchronisierung
In diesem Tutorial erfahren Sie mehr über die [Datensynchronisierung](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Tutorial | Beschreibung |
| --- | --- | 
| [Erste Schritte mit Azure SQL-Datensynchronisierung (Vorschauversion)](sql-database-get-started-sql-data-sync.md) |In diesem Tutorial lernen Sie die Grundlagen der Azure SQL-Datensynchronisierung mit dem klassischen Azure-Portal kennen. |
|  | |

## <a name="next-steps"></a>Nächste Schritte
[Explore Azure SQL Database Solution Quick Starts (Entdecken Sie die lösungsbezogenen Schnellstarts für Azure SQL-Datenbank)](sql-database-solution-quick-starts.md)



<!--HONumber=Feb17_HO2-->


