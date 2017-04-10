---
title: "Auflösen von T-SQL-Unterschieden bei Migration zur Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Transact-SQL-Anweisungen, die in Azure SQL-Datenbank nicht vollständig unterstützt werden"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/17/2017
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 9716975994d1ab3b7cf63efc6480efcd48eb67bb
ms.lasthandoff: 03/18/2017


---
# <a name="resolving-transact-sql-differences-during-migration-to-sql-database"></a>Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank   
Beim [Migrieren der Datenbank](sql-database-cloud-migrate.md) von SQL Server zu Azure SQL-Server stellen Sie möglicherweise fest, dass die Datenbank einige Umstrukturierungen erfordert, bevor SQL Server migriert werden kann. Dieses Thema enthält Anleitungen, die Ihnen sowohl bei der Durchführung dieser Umstrukturierung als auch beim Verstehen der Gründe helfen, warum diese Umstrukturierung notwendig ist. Verwenden Sie zum Erkennen von Kompatibilitätsproblemen den [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Übersicht
Die meisten Transact-SQL-Funktionen, die von Anwendungen verwendet werden, werden in Microsoft SQL Server und in der Azure SQL-Datenbank unterstützt. Die zentralen SQL-Komponenten wie z.B. Datentypen, Operatoren, Zeichenfolgen-, Arithmetik-, logische und Cursorfunktionen funktionieren in SQL Server und in der SQL-Datenbank gleich. Es gibt jedoch einige T-SQL-Unterschiede zwischen Elementen der Datendefinitionssprache (Data Definition Language, DDL) und der Datenbearbeitungssprache (Data Manipulation Language, DML), die dazu führen, dass T-SQL-Anweisungen und -Abfragen nur teilweise unterstützt werden (mehr dazu weiter unten in diesem Thema).

Darüber hinaus werden einige Features und Syntax überhaupt nicht unterstützt, da die Azure SQL-Datenbank entwickelt wurde, um Features von Abhängigkeiten auf Masterdatenbank und Betriebssystem zu isolieren. An sich sind die meisten Aktivitäten auf Serverebene für die SQL-Datenbank nicht geeignet. T-SQL-Anweisungen und -Optionen sind nicht verfügbar, wenn sie Optionen auf Serverebene, Betriebssystemkomponenten oder bestimmte Dateisystemkonfigurationen konfigurieren. Wenn solche Funktionen erforderlich sind, dann ist häufig eine andere geeignete Alternative von der SQL-Datenbank oder aus einem anderen Azure-Feature oder -Dienst verfügbar. 

Zum Beispiel ist hohe Verfügbarkeit in Azure integriert, sodass Konfigurieren von Always On nicht erforderlich ist (obwohl Sie möglicherweise aktive Georeplikation für schnellere Wiederherstellung im Notfall konfigurieren möchten). Deshalb werden alle T-SQL-Anweisungen, die sich auf Verfügbarkeitsgruppen beziehen, nicht von der SQL-Datenbank unterstützt, und die dynamischen Verwaltungssichten, die sich auf Always On beziehen, werden auch nicht unterstützt.

Eine Liste der Funktionen, die von der SQL-Datenbank unterstützt bzw. nicht unterstützt werden, finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md). Die Liste auf dieser Seite ergänzt dieses Thema zu Richtlinien und Funktionen und konzentriert sich auf Transact-SQL-Anweisungen.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Transact-SQL-Syntaxanweisungen mit partiellen Unterschieden
Die zentralen DDL-Anweisungen sind verfügbar, aber einige DDL-Anweisungen haben Erweiterungen im Zusammenhang mit Datenträger-Platzierung und nicht unterstützten Funktionen. 

- CREATE- und ALTER DATABASE-Anweisungen haben mehr als drei Dutzend Optionen. Die Anweisungen enthalten Ablage von Dateien, FILESTREAM und Service Broker-Optionen, die nur für SQL Server gelten. Dies ist möglicherweise unwichtig, wenn Sie Datenbanken erstellen, bevor Sie migrieren, aber wenn Sie T-SQL-Code migrieren, der Datenbanken erstellt, sollten Sie [CREATE DATABASE (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn268335.aspx) mit der SQL Server-Syntax in [CREATE DATABASE (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) vergleichen, um sicherzustellen, dass alle Optionen, die Sie verwenden, unterstützt werden. CREATE DATABASE für Azure SQL-Datenbank verfügt auch über Optionen für Dienstziel und elastische Skalierung, die nur für SQL-Datenbank gelten.
- Die CREATE- und ALTER TABLE-Anweisungen verfügen über FileTable-Optionen, die nicht auf der SQL-Datenbank verwendet werden können, weil FILESTREAM nicht unterstützt wird.
- CREATE- und ALTER LOGIN-Anweisungen werden unterstützt, aber die SQL-Datenbank bietet nicht alle Optionen. Um die Portabilität der Datenbank zu verbessern, unterstützt die SQL-Datenbank nach Möglichkeit die Verwendung von eigenständigen Datenbankbenutzern anstelle von Anmeldungen. Weitere Informationen finden Sie unter [CREATE/ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx) und [Steuern und Gewähren von Datenbankzugriff](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>Transact-SQL-Syntax, die nicht in der SQL-Datenbank unterstützt wird   
Zusätzlich zu Transact-SQL-Anweisungen, die sich auf die nicht unterstützen Features beziehen, die unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md) beschrieben sind, werden die folgenden Anweisungen und Gruppen von Anweisungen nicht unterstützt. Wenn die zu migrierende Datenbank eine der folgenden Funktionen verwendet, müssen Sie deshalb T-SQL so umstrukturieren, dass diese T-SQL-Funktionen und -Anweisungen beseitigt werden.

- Sortierung von Systemobjekten
- Verbindungsbezogen: Endpunktanweisungen, `ORIGINAL_DB_NAME`. SQL-Datenbank unterstützt keine Windows-Authentifizierung, jedoch die ähnliche Azure Active Directory-Authentifizierung. Einige Authentifizierungstypen erfordern die neueste Version von SSMS. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
- Plattformübergreifende Datenbankabfragen mit drei oder vier Teilnamen. (Schreibgeschützte datenbankübergreifende Abfragen werden durch die Verwendung einer [Abfrage von elastischen Datenbanken](sql-database-elastic-query-overview.md)unterstützt.)
- Datenbankübergreifende Besitzverkettung, `TRUSTWORTHY`-Einstellung
- `DATABASEPROPERTY` Verwenden Sie stattdessen `DATABASEPROPERTYEX`.
- `EXECUTE AS LOGIN` Verwenden Sie stattdessen „EXECUTE AS USER“.
- Die Verschlüsselung wird mit Ausnahme der erweiterbaren Schlüsselverwaltung unterstützt.
- Ereignisse: Ereignisse, Ereignisbenachrichtigungen, Abfragebenachrichtigungen
- Ablage von Dateien: Syntax im Zusammenhang mit der Ablage von Datenbankdateien, Größe und Datenbankdateien, die automatisch von Microsoft Azure verwaltet werden.
- Hohe Verfügbarkeit: Syntax im Zusammenhang mit hoher Verfügbarkeit, die über Ihr Microsoft Azure-Konto verwaltet wird. Dies schließt die Syntax für die Sicherung, Wiederherstellung, für Always On, die Datenbankspiegelung, den Protokollversand und Wiederherstellungsmodi ein.
- Protokollleser: Syntax, die vom Protokollleser abhängig ist, der nicht in der SQL-Datenbank verfügbar ist: Pushreplikation, Erfassung geänderter Daten. Die SQL-Datenbank kann ein Abonnent eines Pushreplikationsartikels sein.
- Funktionen: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Globale temporäre Tabellen
- Hardware: Syntax im Zusammenhang mit hardwarebezogenen Servereinstellungen: Arbeitsspeicher, Worker-Threads, CPU-Affinität, Ablaufverfolgungskennzeichen usw. Verwenden Sie stattdessen Dienstebenen.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE` und vierteilige Namen
- .NET Framework: CLR-Integration in SQL Server
- Semantische Suche
- Serveranmeldeinformationen: Verwenden Sie stattdessen [database scoped credentials (datenbankbezogene Anmeldeinformationen)](https://msdn.microsoft.com/library/mt270260.aspx).
- Elemente auf Serverebene: Serverrollen `IS_SRVROLEMEMBER`, `sys.login_token`. `GRANT`, `REVOKE` und `DENY` der Berechtigungen auf Serverebene sind nicht verfügbar, obwohl einige durch Berechtigungen auf Datenbankebene ersetzt wurden. Einige nützliche DMVs auf Serverebene verfügen über entsprechende DMVs auf Datenbankebene.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure`-Optionen und `RECONFIGURE`. Einige Optionen sind verfügbar mit [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server-Agent: Syntax, die auf dem SQL Server-Agent oder der MSDB-Datenbank beruht: Warnungen, Operatoren, zentrale Verwaltungsserver. Verwenden Sie stattdessen Skripterstellung, z.B. Azure PowerShell.
- SQL Server-Überwachung: Verwenden Sie stattdessen die SQL-Datenbank-Überwachung.
- SQL Server-Ablaufverfolgung
- Ablaufverfolgungskennzeichen: Einige Ablaufverfolgungskennzeichen-Elemente wurden in Kompatibilitätsmodi verschoben.
- Transact-SQL-Debugging
- Trigger: Auf Server begrenzt oder Anmeldetrigger
- `USE`-Anweisung: Sie müssen eine neue Verbindung mit der neuen Datenbank herstellen, um den Datenbankkontext in eine andere Datenbank zu ändern.

## <a name="full-transact-sql-reference"></a>Vollständige Transact-SQL-Referenz
Weitere Informationen zu Transact-SQL-Grammatik und -Syntax sowie Beispiele finden Sie unter [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx) in der SQL Server-Onlinedokumentation. 

### <a name="about-the-applies-to-tags"></a>Informationen zu Tags vom Typ "Gilt für"
Die Transact-SQL-Referenz umfasst Themen zu SQL Server-Versionen ab 2008. Unter der Themenüberschrift befindet sich ein Symbol, in dem die vier SQL Server-Plattformen aufgelistet sind und die Anwendbarkeit angezeigt wird. Beispielsweise wurden Verfügbarkeitsgruppen in SQL Server 2012 eingeführt. Das Thema [CREATE AVAILABILTY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) gibt an, dass die Anweisung für **SQL Server gilt (beginnend mit 2012)**. Die Anweisung gilt nicht für SQL Server 2008, SQL Server 2008 R2, Azure SQL-Datenbank, Azure SQL Data Warehouse oder Parallel Data Warehouse.

In einigen Fällen kann der allgemeine Gegenstand eines Themas in einem Produkt verwendet werden, es liegen jedoch kleine Unterschiede im Hinblick auf die verschiedenen Produkte vor. Die Unterschiede werden dann im Thema entsprechend angegeben. In einigen Fällen kann der allgemeine Gegenstand eines Themas in einem Produkt verwendet werden, es liegen jedoch kleine Unterschiede im Hinblick auf die verschiedenen Produkte vor. Die Unterschiede werden dann im Thema entsprechend angegeben. Das Thema CREATE TRIGGER ist zum Beispiel in der SQL-Datenbank verfügbar. Aber die Option **ALL SERVER** für Trigger auf Serverebene gibt an, dass Trigger auf Serverebene in der SQL-Datenbank nicht verwendet werden können. Verwenden Sie stattdessen Trigger auf Datenbankebene.

## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Funktionen, die von der SQL-Datenbank unterstützt bzw. nicht unterstützt werden, finden Sie unter [Funktionen von Azure SQL-Datenbank](sql-database-features.md). Die Liste auf dieser Seite ergänzt dieses Thema zu Richtlinien und Funktionen und konzentriert sich auf Transact-SQL-Anweisungen.


