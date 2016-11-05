---
title: Nicht unterstützte T-SQL-Funktionen in Azure SQL-Datenbank | Microsoft Docs
description: Transact-SQL-Anweisungen, die in Azure SQL-Datenbank nicht vollständig unterstützt werden
services: sql-database
documentationcenter: ''
author: BYHAM
manager: jhubbard
editor: ''
tags: ''

ms.service: sql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/30/2016
ms.author: rick.byham@microsoft.com

---
# Azure SQL-Datenbank – Abweichungen bei Transact-SQL
Die meisten Transact-SQL-Funktionen, von denen Anwendungen abhängen, werden in Microsoft SQL Server und Azure SQL-Datenbank unterstützt. Hier eine unvollständige Liste der für Anwendungen unterstützten Funktionen:

* Datentypen
* Operatoren
* Zeichenfolgen-, arithmetische, logische und Cursorfunktionen

Azure SQL-Datenbank ist jedoch so konzipiert, dass Funktionen von der Abhängigkeit von der **master**-Datenbank isoliert werden. Daher sind viele Aktivitäten auf Serverebene für SQL-Datenbank nicht geeignet und werden nicht unterstützt. Funktionen, die in SQL Server veraltet sind, werden im Allgemeinen in Azure SQL-Datenbank nicht unterstützt.

> [!NOTE]
> In diesem Thema werden die Features behandelt, die mit Azure SQL-Datenbank bei einem Upgrade auf die aktuelle Version Azure SQL-Datenbank V12 verfügbar sind. Weitere Informationen zu V12 finden Sie unter [SQL-Datenbank V12 – Neuerungen](sql-database-v12-whats-new.md).
> 
> 

In den folgenden Abschnitten werden die Features aufgelistet, die teilweise unterstützt werden, und die Features, die vollständig unterstützt werden.

## In Azure SQL-Datenbank V12 teilweise unterstützte Funktionen
Azure SQL-Datenbank V12 unterstützt einige, aber nicht alle Argumente, die in den entsprechenden SQL Server 2016-Transact-SQL-Anweisungen vorhanden sind. Beispielsweise steht die CREATE PROCEDURE-Anweisung zur Verfügung, die Optionen von CREATE PROCEDURE dagegen nicht. In den verlinkten Syntaxthemen finden Sie Einzelheiten zu den unterstützten Bereichen jeder Anweisung.

* Datenbanken: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx)/[ALTER](https://msdn.microsoft.com/library/ms174269.aspx)
* Dynamische Verwaltungssichten stehen im Allgemeinen für verfügbare Features zur Verfügung.
* Funktionen: [CREATE](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER FUNCTION](https://msdn.microsoft.com/library/ms186967.aspx)
* [KILL](https://msdn.microsoft.com/library/ms173730.aspx)
* Anmeldungen: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)
* Gespeicherte Prozeduren: [CREATE](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
* Tabellen: [CREATE](https://msdn.microsoft.com/library/dn305849.aspx)/[ALTER](https://msdn.microsoft.com/library/ms190273.aspx)
* Typen (benutzerdefiniert): [CREATE TYPE](https://msdn.microsoft.com/library/ms175007.aspx)
* Benutzer: [CREATE](https://msdn.microsoft.com/library/ms173463.aspx)/[ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)
* Sichten: [CREATE](https://msdn.microsoft.com/library/ms187956.aspx)/[ALTER VIEW](https://msdn.microsoft.com/library/ms173846.aspx)

## In SQL-Datenbank nicht unterstützte Funktionen
* Sortierung von Systemobjekten
* Verbindungsbezogen: Endpunktanweisungen, ORIGINAL\_DB\_NAME. SQL-Datenbank unterstützt keine Windows-Authentifizierung, jedoch die ähnliche Azure Active Directory-Authentifizierung. Einige Authentifizierungstypen erfordern die neueste Version von SSMS. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
* Plattformübergreifende Datenbankabfragen mit drei oder vier Teilnamen. (Schreibgeschützte datenbankübergreifende Abfragen werden durch die Verwendung einer [Abfrage von elastischen Datenbanken](sql-database-elastic-query-overview.md) unterstützt.)
* Datenbankübergreifende Besitzverkettung, TRUSTWORTHY-Einstellung
* Datensammler
* Datenbankdiagramme
* Datenbank-E-Mail
* DATABASEPROPERTY (verwenden Sie stattdessen DATABASEPROPERTYEX)
* EXECUTE AS-Anmeldungen
* Verschlüsselung: erweiterbare Schlüsselverwaltung
* Ereignisse: Ereignisse, Ereignisbenachrichtigungen, Abfragebenachrichtigungen
* Features im Zusammenhang mit der Ablage von Datenbankdateien, Größe und Datenbankdateien, die automatisch von Microsoft Azure verwaltet werden.
* Features im Zusammenhang mit hoher Verfügbarkeit, die über Ihr Microsoft Azure-Konto verwaltet werden: Sicherung, Wiederherstellung, Always On, Datenbankspiegelung, Protokollversand, Wiederherstellungsmodi. Weitere Informationen finden Sie unter "Sichern und Wiederherstellen der Azure SQL-Datenbank".
* Features, die vom in der SQL-Datenbank ausgeführten Protokollleser abhängig sind: Pushreplikation, Erfassung geänderter Daten.
* Features, die vom SQL Server-Agent oder der MSDB-Datenbank abhängen: Aufträge, Warnungen, Operatoren, richtlinienbasierte Verwaltung, Datenbank-E-Mail, zentrale Verwaltungsserver.
* FILESTREAM
* Funktionen: fn\_get\_sql, fn\_virtualfilestats, fn\_virtualservernodes
* Globale temporäre Tabellen
* Hardwarebezogene Servereinstellungen: Arbeitsspeicher, Worker-Threads, CPU-Affinität, Ablaufverfolgungskennzeichen usw. Verwenden Sie stattdessen Dienstebenen.
* HAS\_DBACCESS
* KILL STATS JOB
* Verknüpfte Server, OPENQUERY, OPENROWSET, OPENDATASOURCE, BULK INSERT, Namen mit vier Teilen
* Master-/Zielserver
* .NET Framework: [CLR-Integration in SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
* Ressourcenkontrolle
* Semantische Suche
* Serveranmeldeinformationen. Verwenden Sie stattdessen datenbankbezogene Anmeldeinformationen.
* Elemente auf Serverebene: Serverrollen, IS\_SRVROLEMEMBER, sys.login\_token. Berechtigungen auf Serverebene sind nicht verfügbar, obwohl einige durch Berechtigungen auf Datenbankebene ersetzt wurden. Einige dynamische Verwaltungssichten auf Serverebene sind nicht verfügbar, obwohl einige durch dynamische Verwaltungssichten auf Datenbankebene ersetzt wurden.
* Serverloses Express: localdb, Benutzerinstanzen
* Service Broker
* SET REMOTE\_PROC\_TRANSACTIONS
* SHUTDOWN
* sp\_addmessage
* sp\_configure-Optionen und RECONFIGURE. Einige Optionen sind verfügbar mit [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
* sp\_helpuser
* sp\_migrate\_user\_to\_contained
* SQL Server-Überwachung. Verwenden Sie stattdessen die SQL-Datenbank-Überwachung.
* SQL Server Profiler
* SQL Server-Ablaufverfolgung
* Ablaufverfolgungskennzeichen. Einige Ablaufverfolgungskennzeichen-Elemente wurden in Kompatibilitätsmodi verschoben.
* Transact-SQL-Debugging
* Trigger: Auf Server begrenzt oder Anmeldetrigger
* USE-Anweisung: Sie müssen eine neue Verbindung mit der neuen Datenbank herstellen, um den Datenbankkontext in eine andere Datenbank zu ändern.

## Vollständige Transact-SQL-Referenz
Weitere Informationen zu Transact-SQL-Grammatik und -Syntax sowie Beispiele finden Sie unter [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx) in der SQL Server-Onlinedokumentation.

### Informationen zu Tags vom Typ "Gilt für"
Die Transact-SQL-Referenz umfasst Themen zu SQL Server-Versionen ab 2008. Unter der Themenüberschrift befindet sich ein Symbol, in dem die vier SQL Server-Plattformen aufgelistet sind und die Anwendbarkeit angezeigt wird. Beispielsweise wurden Verfügbarkeitsgruppen in SQL Server 2012 eingeführt. Das Thema [CREATE AVAILABILTY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) gibt an, dass die Anweisung für ** SQL Server gilt (beginnend mit 2012). Die Anweisung gilt nicht für SQL Server 2008, SQL Server 2008 R2, Azure SQL-Datenbank, Azure SQL Data Warehouse oder Parallel Data Warehouse.

In einigen Fällen kann der allgemeine Gegenstand eines Themas in einem Produkt verwendet werden, es liegen jedoch kleine Unterschiede im Hinblick auf die verschiedenen Produkte vor. Die Unterschiede werden dann im Thema entsprechend angegeben.

<!---HONumber=AcomDC_0831_2016-->