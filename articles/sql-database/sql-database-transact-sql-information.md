---
title: "Nicht unterstützte T-SQL-Funktionen in Azure SQL-Datenbank | Microsoft Docs"
description: "Transact-SQL-Anweisungen, die in Azure SQL-Datenbank nicht vollständig unterstützt werden"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/22/2017
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: a08d9f2ef29002f10473b0e041737c9c607f3ca0
ms.openlocfilehash: 7d6de93c99141248ea970ea668fb0b2191267b62
ms.lasthandoff: 03/01/2017


---
# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL-Datenbank – Abweichungen bei Transact-SQL   
Die meisten Transact-SQL-Funktionen, von denen Anwendungen abhängen, werden in Microsoft SQL Server und Azure SQL-Datenbank unterstützt. Die zentralen SQL-Komponenten wie z.B. Datentypen, Operatoren, Zeichenfolgen-, Arithmetik-, logische und Cursorfunktionen usw., funktionieren ohne Unterschiede zu SQL Server.

# <a name="why-some-transact-sql-is-not-supported"></a>Warum einige Transact-SQL-Features nicht unterstützt werden
Azure SQL-Datenbank ist so konzipiert, dass Funktionen von den Abhängigkeiten von der master-Datenbank und dem Betriebssystem isoliert werden. Daher sind viele Aktivitäten auf Serverebene für die SQL-Datenbank nicht geeignet. Transact-SQL-Anweisungen sind in der Regel nicht verfügbar, wenn sie Optionen auf Serverebene, Betriebssystemkomponenten oder bestimmte Dateisystemkonfigurationen konfigurieren. Wenn Funktionen erforderlich sind, die sich außerhalb der Benutzerdatenbank befinden, dann ist häufig eine andere geeignete Alternative zur SQL-Datenbank bzw. aus einem anderen Azure-Feature oder -Dienst verfügbar. 

Beispielsweise gilt Always On nicht für Azure SQL-Datenbank, da die hohe Verfügbarkeit im Dienst integriert ist, d.h., jede Datenbank ist hoch verfügbar. Aus diesem Grund werden alle Transact-SQL-Anweisungen, die sich auf Verfügbarkeitsgruppen beziehen, nicht von der SQL-Datenbank unterstützt, und die dynamischen Verwaltungssichten, die sich auf Always On beziehen, werden nicht unterstützt.  

Eine Liste der Funktionen, die von der SQL-Datenbank unterstützt bzw. nicht unterstützt werden, finden Sie unter [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md).


## <a name="transact-sql-syntax-partially-supported-in-sql-database"></a>Transact-SQL-Syntax, die teilweise in der SQL-Datenbank unterstützt wird
SQL-Datenbank unterstützt einige, aber nicht alle Argumente, die in den entsprechenden SQL Server 2016-Transact-SQL-Anweisungen zum Verwalten von Datenbanken und Anmeldungen enthalten sind. Beispielsweise ist die Anweisung `CREATE DATABASE` in Azure SQL-Datenbank verfügbar, jedoch werden nicht alle in SQL Server unterstützten Optionen in Azure SQL-Datenbank unterstützt (und umgekehrt). In den verlinkten Syntaxthemen finden Sie Einzelheiten zu den unterstützten Bereichen jeder Anweisung.

- Datenbanken: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx)/[ALTER_DATABASE](https://msdn.microsoft.com/library/mt574871.aspx)   
- Anmeldungen: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)   

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>Transact-SQL-Syntax, die nicht in der SQL-Datenbank unterstützt wird   
Zusätzlich zu Transact-SQL-Anweisungen, die sich auf die nicht unterstützen Features beziehen, die unter [Azure SQL Database features (Features der Azure SQL-Datenbank)](sql-database-features.md) beschrieben sind, werden die folgenden Anweisungen und Gruppen von Anweisungen nicht unterstützt.
- Sortierung von Systemobjekten
- Verbindungsbezogen: Endpunktanweisungen, `ORIGINAL_DB_NAME`. SQL-Datenbank unterstützt keine Windows-Authentifizierung, jedoch die ähnliche Azure Active Directory-Authentifizierung. Einige Authentifizierungstypen erfordern die neueste Version von SSMS. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
- Plattformübergreifende Datenbankabfragen mit drei oder vier Teilnamen. (Schreibgeschützte datenbankübergreifende Abfragen werden durch die Verwendung einer [Abfrage von elastischen Datenbanken](sql-database-elastic-query-overview.md)unterstützt.)
- Datenbankübergreifende Besitzverkettung, `TRUSTWORTHY`-Einstellung
- `DATABASEPROPERTY` Verwenden Sie stattdessen `DATABASEPROPERTYEX`.
- `EXECUTE AS LOGIN` Verwenden Sie stattdessen „EXECUTE AS USER“.
- Die Verschlüsselung wird mit Ausnahme der erweiterbaren Schlüsselverwaltung unterstützt.
- Ereignisse: Ereignisse, Ereignisbenachrichtigungen, Abfragebenachrichtigungen
- Syntax im Zusammenhang mit der Ablage von Datenbankdateien, Größe und Datenbankdateien, die automatisch von Microsoft Azure verwaltet werden.
- Syntax im Zusammenhang mit hoher Verfügbarkeit, die über Ihr Microsoft Azure-Konto verwaltet wird. Dies schließt die Syntax für die Sicherung, Wiederherstellung, für Always On, die Datenbankspiegelung, den Protokollversand und Wiederherstellungsmodi ein.
- Syntax, die vom Protokollleser abhängig ist, der nicht in der SQL-Datenbank verfügbar ist: Pushreplikation, Erfassung geänderter Daten. Die SQL-Datenbank kann ein Abonnent eines Pushreplikationsartikels sein.
- Syntax, die auf dem SQL Server-Agent oder der MSDB-Datenbank beruht: Warnungen, Operatoren, zentrale Verwaltungsserver. Verwenden Sie stattdessen Skripterstellung, z.B. Azure PowerShell.
- Funktionen: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Globale temporäre Tabellen
- Syntax im Zusammenhang mit hardwarebezogenen Servereinstellungen: Arbeitsspeicher, Worker-Threads, CPU-Affinität, Ablaufverfolgungskennzeichen usw. Verwenden Sie stattdessen Dienstebenen.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE` und vierteilige Namen
- .NET Framework: [CLR-Integration in SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Semantische Suche
- Serveranmeldeinformationen. Verwenden Sie stattdessen datenbankbezogene Anmeldeinformationen.
- Elemente auf Serverebene: Serverrollen `IS_SRVROLEMEMBER`, `sys.login_token`. `GRANT`, `REVOKE` und `DENY` der Berechtigungen auf Serverebene sind nicht verfügbar, obwohl einige durch Berechtigungen auf Datenbankebene ersetzt wurden. Einige nützliche DMVs auf Serverebene verfügen über entsprechende DMVs auf Datenbankebene.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure`-Optionen und `RECONFIGURE`. Einige Optionen sind verfügbar mit [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server-Überwachung. Verwenden Sie stattdessen die SQL-Datenbank-Überwachung.
- SQL Server-Ablaufverfolgung
- Ablaufverfolgungskennzeichen. Einige Ablaufverfolgungskennzeichen-Elemente wurden in Kompatibilitätsmodi verschoben.
- Transact-SQL-Debugging
- Trigger: Auf Server begrenzt oder Anmeldetrigger
- `USE`-Anweisung: Sie müssen eine neue Verbindung mit der neuen Datenbank herstellen, um den Datenbankkontext in eine andere Datenbank zu ändern.

## <a name="full-transact-sql-reference"></a>Vollständige Transact-SQL-Referenz
Weitere Informationen zu Transact-SQL-Grammatik und -Syntax sowie Beispiele finden Sie unter [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx) in der SQL Server-Onlinedokumentation. 

### <a name="about-the-applies-to-tags"></a>Informationen zu Tags vom Typ "Gilt für"
Die Transact-SQL-Referenz umfasst Themen zu SQL Server-Versionen ab 2008. Unter der Themenüberschrift befindet sich ein Symbol, in dem die vier SQL Server-Plattformen aufgelistet sind und die Anwendbarkeit angezeigt wird. Beispielsweise wurden Verfügbarkeitsgruppen in SQL Server 2012 eingeführt. Das Thema [CREATE AVAILABILTY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) gibt an, dass die Anweisung für **SQL Server gilt (beginnend mit 2012)**. Die Anweisung gilt nicht für SQL Server 2008, SQL Server 2008 R2, Azure SQL-Datenbank, Azure SQL Data Warehouse oder Parallel Data Warehouse.

In einigen Fällen kann der allgemeine Gegenstand eines Themas in einem Produkt verwendet werden, es liegen jedoch kleine Unterschiede im Hinblick auf die verschiedenen Produkte vor. Die Unterschiede werden dann im Thema entsprechend angegeben.

