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
ms.date: 12/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: ad6fb631f05b1e88e8cbaaca83f9863cfb643269
ms.openlocfilehash: 25725a28686dc3bb33bb400f4882d539cba2b691


---
# <a name="explore-azure-sql-database-tutorials"></a>Entdecken Sie Azure SQL-Datenbank-Tutorials
Über die folgenden Links gelangen Sie zu einer Übersicht über jeden aufgelisteten Featurebereich und zu einem einfachen schrittweisen Tutorial für jeden Bereich. Lösungsbezogene Schnellstarts, die die Verwendung von SQL-Datenbank in einer vollständigen Lösung basierend auf realen Szenarien veranschaulichen, finden Sie unter [Azure SQL Database Solution Quick Starts](sql-database-solution-quick-starts.md)(Lösungsbezogene Schnellstarts für Azure SQL-Datenbank).

## <a name="using-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio
In den folgenden Tutorials lernen Sie, wie Sie SQL Server Management Studio dazu verwenden können, Azure SQL-Datenbank zu verwalten und abzufragen.

> [!IMPORTANT]
> Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mithilfe einer Prinzipalanmeldung auf Serverebene](sql-database-get-started-security.md#connect-to-azure-sql-database-using-sql-server-authentication) |In diesem Tutorial lernen Sie, wie Sie mithilfe einer Prinzipalanmeldung auf SQL-Serverebene eine Verbindung mit Azure SQL-Datenbank herstellen. |
| [Herstellen einer Verbindung mit Azure SQL-Datenbank als Benutzer](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) |In diesem Tutorial lernen Sie, wie Sie mithilfe eines Benutzerkontos auf Datenbankebene eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
|  | |

## <a name="create-servers-databases-and-server-level-firewall-rules"></a>Erstellen von Servern, Datenbanken und Firewallregeln auf Serverebene
In den folgenden Tutorials erstellen Sie Server, Datenbanken und Firewallregeln auf Serverebene, und Sie erfahren, wie Sie eine Verbindung herstellen sowie Server und Datenbanken abfragen.

| Tutorial | Beschreibung |
| --- | --- | --- |
| [Enthält Informationen zu den ersten Schritten mit Azure SQL-Datenbankservern, -Datenbanken und -Firewallregel mit dem Azure-Portal und SQL Server Management Studio.](sql-database-get-started.md) | In diesem Tutorial verwenden Sie das Azure-Portal und SQL Server Management Studio, um einen logischen Server mit Azure SQL-Datenbank zu erstellen, dessen Eigenschaften anzuzeigen, eine Firewall auf Serverebene zu erstellen, eine Verbindung mit der Masterdatenbank herzustellen, eine Beispieldatenbank zu erstellen, deren Eigenschaften anzuzeigen, eine Verbindung mit der Beispieldatenbank herzustellen und eine leere Datenbank zu erstellen. |
| [Erstellen einer SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets](sql-database-get-started-powershell.md) | In diesem Tutorial verwenden Sie PowerShell, um einen logischen Server mit Azure SQL-Datenbank zu erstellen, dessen Eigenschaften anzuzeigen, eine Firewall auf Serverebene zu erstellen, eine Verbindung mit der Masterdatenbank herzustellen, eine Beispieldatenbank zu erstellen, deren Eigenschaften anzuzeigen, eine Verbindung mit der Beispieldatenbank herzustellen und eine leere Datenbank zu erstellen. |
| [Ausprobieren von SQL-Datenbank: Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md)| In diesem Tutorial verwenden Sie C#, um einen SQL-Datenbankserver, eine Firewallregel und eine SQL-Datenbank zu erstellen. Außerdem erstellen Sie eine AD-Anwendung (Active Directory) und den erforderlichen Dienstprinzipal zum Authentifizieren der C#-App. |

## <a name="backup-and-recovery"></a>Sicherung und Wiederherstellung
Im folgenden Tutorial stellen Sie einen bestimmten Zeitpunkt einer Datenbank wieder her, konfigurieren die langfristige Beibehaltung der Sicherung und stellen eine im Azure Recovery Services-Tresor aufbewahrte Datenbank wieder her. 

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Get Started with Backup and Restore for Data Protection and Recovery](sql-database-get-started-backup-recovery.md) (Erste Schritte mit Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung) | In diesem Tutorial stellen Sie über das Azure-Portal einen bestimmten Zeitpunkt einer Datenbank wieder her, konfigurieren die langfristige Beibehaltung der Sicherung und stellen eine im Azure Recovery Services-Tresor aufbewahrte Datenbank wieder her. |


## <a name="elastic-pools"></a>Elastische Pools
In den folgenden Tutorials erfahren Sie mehr zur Verwendung von [elastischen Pools](sql-database-elastic-pool.md) zum Verwalten der Leistungsziele für mehrere Datenbanken mit sehr unterschiedlichen und unvorhersehbaren Nutzungsmustern.

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Erstellen eines elastischen Pools](sql-database-elastic-pool-create-portal.md) |In diesem Tutorial erfahren Sie, wie Sie einen skalierbaren Pool aus Azure SQL-Datenbanken erstellen. |
| [Überwachen einer elastischen Datenbank](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) |In diesem Tutorial erfahren Sie, wie Sie eine einzelne elastische Datenbank auf potenzielle Probleme überwachen können. |
| [Eine Warnung zu einer Poolressource hinzufügen](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) |In diesem Tutorial erfahren Sie, wie Sie Regeln zu den Ressourcen hinzufügen, die E-Mails an Personen verschicken, oder Warnzeichenfolgen zu einem URL-Endpunkt, wenn die Ressource einen Verwendungsschwellenwert erreicht, den Sie eingerichtet haben. |
| [Verschieben einer Datenbank in einen elastischen Pool](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) |In diesem Tutorial erfahren Sie, wie Sie eine Datenbank in einen elastischen Pool verschieben. |
| [Verschieben einer Datenbank aus einem elastischen Pool](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) |In diesem Tutorial erfahren Sie, wie Sie eine Datenbank aus einem elastischen Pool verschieben. |
| [Ändern Sie die Leistungseinstellungen eines Pools](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) |In diesem Lernprogramm erfahren Sie, wie Sie die Leistungs- und Speicherbegrenzungen für einen Pool anpassen. |
|  | |

## <a name="elastic-database-jobs"></a>Aufträge für die elastische Datenbank
In den folgenden Tutorials erfahren Sie mehr zur Verwendung von [Aufträgen für elastische Datenbanken](sql-database-elastic-jobs-overview.md).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) |In diesem Tutorial erfahren Sie, wie Sie die Funktionen der Tools für elastische Datenbanken mithilfe einer einfachen horizontal partitionierten Anwendung verwenden. |
| [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) |In diesem Tutorial erfahren Sie, wie Sie Aufträge zum Verwalten einer Gruppe aufeinander bezogener Datenbanken erstellen und verwalten können. |
|  | |

## <a name="elastic-queries"></a>Elastische Abfragen
In den folgenden Tutorials erfahren Sie mehr zur Ausführung von [elastischen Abfragen](sql-database-elastic-query-overview.md).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Abfragen über eine horizontal partitionierte Datenbank (Sharddatenbank)](sql-database-elastic-query-getting-started.md) |In diesem Tutorial erfahren Sie, wie Sie Berichte für alle Datenbanken in einer horizontal partitionierten Datenbank (Sharddatenbank) mithilfe von [elastischen Abfragen](sql-database-elastic-query-overview.md) |
| [Abfragen über eine vertikal partitionierte Datenbank](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) |In diesem Tutorial erfahren Sie, wie Sie Berichte für alle Datenbanken in einer vertikalen Datenbank mithilfe von [elastischen Abfragen](sql-database-elastic-query-overview.md) |
| [Migrieren einer vorhandenen Datenbank zum horizontalen Skalieren](sql-database-elastic-convert-to-use-elastic-tools.md) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank horizontal skalieren (Shard). |
|  | |

## <a name="performance-optimization"></a>Leistungsoptimierung
In den folgenden Tutorials erfahren Sie, wie Sie die [Leistung eigenständiger Datenbanken](sql-database-performance-guidance.md) optimieren. Informationen zum Optimieren der Leistung von mehreren Datenbanken finden Sie unter [Elastische Pools](#elastic-pools).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Ändern der Dienstebene und Leistungsstufe Ihrer Datenbank](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) |In diesem Tutorial erfahren Sie, wie Sie die Leistung einer Azure SQL-Datenbank mithilfe von Dienstebenen zentral hoch- bzw. herunterskalieren können. |
| [SQL Datenbank-Ratgeber – Query Performance Insight](sql-database-performance.md#performance-overview) |In diesem Tutorial erfahren Sie, wie Sie Query Performance Insight des SQL-Datenbank-Ratgebers öffnen und verwenden. |
| [SQL-Datenbank-Ratgeber – Anzeigen von Empfehlungen](sql-database-advisor.md) |In diesem Tutorial erfahren Sie, wie Sie Empfehlungen des SQL-Datenbank-Ratgebers zur Leistung anzeigen und anwenden. |
| [Überprüfen von Abfragen mit der höchster CPU-Auslastung](sql-database-query-performance.md#review-top-cpu-consuming-queries) |In diesem Tutorial erfahren Sie, wie Sie Query Performance Insight des SQL-Datenbank-Ratgebers verwenden, um Abfragen mit der höchsten CPU-Auslastung zu überprüfen. |
| [Anzeigen von Details einzelner Abfragen](sql-database-query-performance.md#viewing-individual-query-details) |In diesem Tutorial erfahren Sie, wie Sie Query Performance Insight des SQL-Datenbank-Ratgebers verwenden, um Details zu der Leistung einzelner Abfragen anzuzeigen. |
|  | |

## <a name="sql-database-migration-and-archive"></a>SQL-Datenbank-Migration und -Archiv
In den folgenden Tutorials erfahren Sie, wie Sie [eine vorhandene SQL Server-Datenbank zu Azure SQL-Datenbank migrieren](sql-database-cloud-migrate.md).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Ermitteln von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) |In diesem Tutorial lernen Sie, wie Sie SQL Server Data Tools für Visual Studio zum Ermitteln der Azure SQL-Datenbank-Kompatibilität verwenden. |
| [Beheben von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) |In diesem Tutorial lernen Sie, wie Sie SQL Server Data Tools für Visual Studio zum Beheben von Azure SQL-Datenbank-Kompatibilitätsproblemen verwenden. |
| [Ermitteln der SQL-Datenbankkompatibilität mithilfe von SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) |In diesem Tutorial lernen Sie, wie Sie das Befehlszeilenprogramm „SQLPackage.exe“ zum Ermitteln der Azure SQL-Datenbank-Kompatibilität verwenden. |
| [Ermitteln der SQL-Datenbankkompatibilität mithilfe von SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |In diesem Tutorial lernen Sie, wie Sie SQL Server Management Studio zum Ermitteln der Azure SQL-Datenbank-Kompatibilität verwenden. |
| [Migrieren von SQL Server-Datenbank auf SQL-Datenbank mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) |In diesem Tutorial erfahren Sie, wie Sie eine kompatible SQL Server-Datenbank mithilfe des Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank in SQL Server Management Studio zu Azure SQL-Datenbank migrieren. |
| [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei per SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) |In diesem Tutorial erfahren Sie, wie Sie eine kompatible SQL Server-Datenbank mithilfe des Assistenten zum Exportieren einer Datenebenenanwendung in SQL Server Management Studio in eine BACPAC-Datei exportieren. |
| [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei per SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) |In diesem Tutorial erfahren Sie, wie Sie eine kompatible SQL Server-Datenbank mithilfe des Befehlszeilenprogramms „SQLPackage.exe“ in eine BACPAC-Datei exportieren. |
| [Importieren aus BACPAC zur SQL-Datenbank mithilfe von SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) |In diesem Tutorial erfahren Sie, wie Sie eine Datenbank mithilfe des Assistenten zum Exportieren einer Datenebenenanwendung in SQL Server Management Studio aus einer BACPAC-Datei in Azure SQL-Datenbank importieren. |
| [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe von SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) |In diesem Tutorial erfahren Sie, wie Sie eine Datenbank mithilfe des Befehlszeilenprogramms „SQLPackage.exe“ von einer BACPAC-Datei in Azure SQL-Datenbank importieren. |
| [Importieren einer BACPAC-Datei in Azure SQL-Datenbank über das Azure-Portal](sql-database-import.md) |In diesem Tutorial erfahren Sie, wie Sie eine Datenbank mithilfe des Azure-Portals von einer BACPAC-Datei, die in einem Azure-Blob gespeichert ist, in Azure SQL-Datenbank importieren. |
| [Importieren einer BACPAC-Datei nach Azure SQL-­Datenbank mithilfe von PowerShell](sql-database-import-powershell.md) |In diesem Tutorial erfahren Sie, wie Sie eine Datenbank mithilfe von PowerShell von einer BACPAC-Datei in Azure SQL-Datenbank importieren. |
| [Archivieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-export.md#export-your-database) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank mithilfe des Azure-Portals in eine BACPAC-Datei archivieren. |
| [Archivieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-export-powershell.md) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank mit PowerShell in einer BACPAC-Datei archivieren. |
| [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy.md) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank mithilfe des Azure-Portals kopieren. |
| [Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank mithilfe von PowerShell kopieren. |
| [Kopieren einer Azure SQL-Datenbank mithilfe von Transact-SQL](sql-database-copy-transact-sql.md#copy-your-sql-database) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank mithilfe von Transact-SQL kopieren. |
|  | |

## <a name="develop"></a>Entwickeln
In den folgenden Tutorials erfahren Sie mehr über die [SQL-Datenbankentwicklung](sql-database-develop-overview.md) und die [Verwendung von Verbindungsbibliotheken](sql-database-libraries.md).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von .NET (C#)](sql-database-develop-dotnet-simple.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von C# eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
| [Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Java](sql-database-develop-java-simple.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von Java eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
| [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Node.js](sql-database-develop-nodejs-simple.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von Node.js eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
| [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP](sql-database-develop-php-simple.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von PHP eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
| [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Python](sql-database-develop-python-simple.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von Python eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
| [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Ruby](sql-database-develop-ruby-simple.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von Ruby eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
|  | |

## <a name="database-authentication-and-authorization"></a>Datenbankauthentifizierung und Autorisierung
In den folgenden Tutorials erfahren Sie mehr über das [Erstellen und Verwalten von Anmeldungen und Benutzern](sql-database-manage-logins.md).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Erstellen einer Azure SQL-Datenbank-Firewallregel auf Serverebene mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md) |In diesem Tutorial erfahren Sie, wie Sie eine SQL-Datenbank-Firewall auf Serverebene mithilfe des Azure-Portals konfigurieren. |
| [Erstellen einer Firewallregel auf Datenbankebene mithilfe von Transact-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von Transact-SQL eine Firewallregel auf Datenbankebene erstellen. |
| [Verwalten von Firewallregeln auf Serverebene mithilfe von Transact-SQL](sql-database-configure-firewall-settings-tsql.md#server-level-firewall-rules) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank-Firewall auf Serverebene mithilfe von Transact-SQL verwalten. |
| [Verwalten von Firewallregeln auf Serverebene mithilfe von PowerShell](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-by-using-powershell) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank-Firewall auf Serverebene mithilfe von PowerShell verwalten. |
| [Verwalten von Firewallregeln auf Serverebene mithilfe der REST-API](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-rest-api) |In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank-Firewall auf Serverebene mithilfe der REST-API verwalten. |
| [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mithilfe einer Prinzipalanmeldung auf Serverebene](sql-database-get-started-security.md#connect-to-azure-sql-database-using-sql-server-authentication) |In diesem Tutorial lernen Sie, wie Sie mithilfe einer Prinzipalanmeldung auf Serverebene eine Verbindung mit Azure SQL-Datenbank herstellen. |
| [Einer Anmeldung Datenbankzugriff gewähren](sql-database-manage-logins.md#granting-database-access-to-a-login() |In diesem Tutorial erfahren Sie, wie Sie einer Anmeldung auf Serverebene Datenbankzugriff gewähren. |
| [Erstellen neuer Datenbankbenutzer mit SSMS](sql-database-get-started-security.md#create-new-database-user-using-ssms) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von SSMS einen neuen Datenbankbenutzer in einer vorhandenen Datenbank erstellen. |
| [Erteilen von „db_owner“-Berechtigungen für neue Datenbankbenutzer](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) |In diesem Tutorial erfahren Sie, wie Sie einem vorhandenen Datenbankbenutzer „db_owner“-Berechtigungen zuweisen. |
| [Herstellen einer Verbindung mit Azure SQL-Datenbank als Benutzer](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) |In diesem Tutorial erfahren Sie, wie Sie mithilfe eines Benutzerkontos auf Datenbankebene eine Verbindung mit einer Azure SQL-Datenbank herstellen. |
|  | |

## <a name="secure-and-protect-data"></a>Sichern und Schützen von Daten
In den folgenden Tutorials erfahren Sie mehr zur [Sicherung von Azure SQL-Datenbank-Daten](sql-database-security.md).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Aktivieren der Bedrohungserkennung für Ihre Datenbank mithilfe des Azure-Portals](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) |In diesem Tutorial erfahren Sie, wie Sie die Bedrohungserkennung für Ihre Datenbank im Azure-Portal einrichten. |
| [Sichern sensibler Daten mithilfe von Always Encrypted ](sql-database-always-encrypted-azure-key-vault.md) |In diesem Tutorial verwenden Sie den Always Encrypted-Assistenten, um sensible Daten in einer Azure SQL-Datenbank zu sichern. |
| [Sichern sensibler Daten mithilfe von Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/dn948096.aspx) |In diesem Tutorial erfahren Sie, wie Sie Transparent Data Encryption verwenden können, um sensible Daten zu sichern. |
| [Verschlüsseln einer Spalte mit Daten](https://msdn.microsoft.com/library/ms179331.aspx) |In diesem Tutorial erfahren Sie, wie Sie eine Spalte mit Daten mithilfe von Transact-SQL verschlüsseln. |
| [Einrichten der dynamischen Datenmaskierung](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal) |In diesem Tutorial erfahren Sie, wie Sie die dynamische Datenmaskierung für Ihre Azure SQL-Datenbank einrichten. |
|  | |

## <a name="business-continuity"></a>Geschäftskontinuität
In den folgenden Tutorials erfahren Sie mehr über die Verwendung der [Geowiederherstellung und der aktiven Georeplikation](sql-database-business-continuity.md) zum Wiederherstellen nach Fehlern, für die Geschäftskontinuität und für das horizontale Skalieren von Abfragen.

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-point-in-time-restore.md) |In diesem Tutorial erfahren Sie, wie Sie den Zustand Ihrer Datenbank zu einem früheren Zeitpunkt mithilfe des Azure-Portals wiederherstellen können. |
| [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](sql-database-point-in-time-restore.md) |In diesem Tutorial erfahren Sie, wie Sie den Zustand Ihrer Datenbank zu einem früheren Zeitpunkt mithilfe von PowerShell wiederherstellen können. |
| [Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md) |In diesem Tutorial erfahren Sie, wie Sie eine gelöschte Datenbank mithilfe des Azure-Portals wiederherstellen. |
| [Wiederherstellen einer gelöschten Azure SQL-Datenbank mithilfe von PowerShell](sql-database-restore-deleted-database-powershell.md) |In diesem Tutorial erfahren Sie, wie Sie eine gelöschte Datenbank mithilfe von PowerShell wiederherstellen. |
| [Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md) |In diesem Tutorial erfahren Sie, wie Sie die aktive Georeplikation mithilfe des Azure-Portals konfigurieren. |
| [Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-powershell.md) |In diesem Tutorial erfahren Sie, wie Sie die aktive Georeplikation mithilfe von PowerShell konfigurieren. |
| [Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-transact-sql.md) |In diesem Tutorial erfahren Sie, wie Sie die aktive Georeplikation mithilfe von Transact-SQL konfigurieren. |
| [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-failover-portal.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals ein Failover auf ein georepliziertes sekundäres Replikat ausführen. |
| [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-failover-powershell.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von PowerShell ein Failover auf ein georepliziertes sekundäres Replikat ausführen. |
| [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) |In diesem Tutorial erfahren Sie, wie Sie mithilfe von Transact-SQL ein Failover auf ein georepliziertes sekundäres Replikat ausführen. |
|  | |

## <a name="data-sync"></a>Datensynchronisierung
In diesem Tutorial erfahren Sie mehr über die [Datensynchronisierung](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Lernprogramm: | Beschreibung |
| --- | --- | --- |
| [Erste Schritte mit Azure SQL-Datensynchronisierung (Vorschauversion)](sql-database-get-started-sql-data-sync.md) |In diesem Tutorial lernen Sie die Grundlagen der Azure SQL-Datensynchronisierung mit dem klassischen Azure-Portal kennen. |
|  | |

## <a name="next-steps"></a>Nächste Schritte
[Explore Azure SQL Database Solution Quick Starts (Entdecken Sie die lösungsbezogenen Schnellstarts für Azure SQL-Datenbank)](sql-database-solution-quick-starts.md)



<!--HONumber=Dec16_HO3-->


