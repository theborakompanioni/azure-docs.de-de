<properties
   pageTitle="Verwenden des Abfragespeichers in Azure SQL-Datenbank"
   description="Erfahren Sie, wie Sie den Abfragespeicher in Azure SQL-Datenbank verwenden."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# Verwenden des Abfragespeichers in Azure SQL-Datenbank 

Der Abfragespeicher in Azure ist eine vollständig verwaltete Datenbankfunktion, mit der fortlaufend ausführliche Verlaufsinformationen zu allen Abfragen gesammelt und dargestellt werden. Sie können sich den Abfragespeicher in etwa wie den Flugdatenschreiber eines Flugzeugs vorstellen, mit dem die Problembehandlung der Abfrageleistung sowohl für Cloudkunden als auch für lokale Kunden erheblich vereinfacht wird. In diesem Artikel werden spezielle Aspekte der Abfragespeichernutzung in Azure beschrieben. Mit diesen vorab erfassten Abfragedaten können Sie Leistungsprobleme schnell diagnostizieren und lösen und haben mehr Zeit, sich auf das Geschäft zu konzentrieren.

Der Abfragespeicher ist in Azure SQL-Datenbank seit November 2015 [global verfügbar](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/). Der Abfragespeicher ist die Grundlage für die Leistungsanalyse und Optimierungsfunktionen, z.B. [SQL Database Advisor and Performance Dashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/) (SQL-Datenbank-Ratgeber und Leistungsdashboard). Zum Zeitpunkt der Veröffentlichung dieses Artikels wird der Abfragespeicher in mehr als 200.000 Benutzerdatenbanken in Azure ausgeführt und sammelt seit mehreren Monaten ununterbrochen Informationen zu Abfragen.

> [AZURE.IMPORTANT] Microsoft führt gerade die Aktivierung des Abfragespeichers für alle Azure SQL-Datenbanken durch (vorhandene und neue Datenbanken).

## Optimale Konfiguration des Abfragespeichers

Dieser Abschnitt beschreibt die optimalen Standardeinstellungen der Konfiguration, mit denen der zuverlässige Betrieb des Abfragespeichers und der abhängigen Features sichergestellt wird, z.B. [SQL-Datenbank-Ratgeber und Leistungsdashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Die Standardkonfiguration ist für die fortlaufende Datensammlung optimiert. Dies bedeutet, dass möglichst wenig Zeit im Status OFF bzw. READ\_ONLY verbracht wird.

| Konfiguration | Beschreibung | Standard | Kommentar |
| ------------- | ----------- | ------- | ------- |
| MAX\_STORAGE\_SIZE\_MB | Gibt das Limit für den Datenspeicherplatz an, den der Abfragespeicher in der Kundendatenbank belegt. | 100 | Für neue Datenbanken erzwungen |
| INTERVAL\_LENGTH\_MINUTES | Definiert die Größe des Zeitfensters, in dem gesammelte Laufzeitstatistiken für Abfragepläne aggregiert und dauerhaft gespeichert werden. Jeder aktive Abfrageplan verfügt über maximal eine Zeile für einen mit dieser Konfiguration definierten Zeitraum. | 60 | Für neue Datenbanken erzwungen |
| STALE\_QUERY\_THRESHOLD\_DAYS | Zeitbasierte Bereinigungsrichtlinie, mit der der Aufbewahrungszeitraum für dauerhaft gespeicherte Laufzeitstatistiken und inaktive Abfragen gesteuert wird. | 30 | Für neue Datenbanken und Datenbanken mit vorheriger Standardeinstellung erzwungen (367) |
| SIZE\_BASED\_CLEANUP\_MODE | Gibt an, ob die automatische Datenbereinigung durchgeführt wird, wenn der Datenumfang des Abfragespeichers sich dem Grenzwert nähert. | AUTO | Für alle Datenbanken erzwungen |
| QUERY\_CAPTURE\_MODE | Gibt an, ob die Gesamtmenge aller Abfragen oder nur eine Teilmenge der Abfragen nachverfolgt wird. | AUTO | Für alle Datenbanken erzwungen |
| FLUSH\_INTERVAL\_SECONDS | Gibt an, wie lange gesammelte Laufzeitstatistiken maximal im Arbeitsspeicher aufbewahrt werden, bevor eine Leerung auf einen Datenträger erfolgt. | 900 | Für neue Datenbanken erzwungen |
||||||

> [AZURE.IMPORTANT] Diese Standardeinstellungen werden in der letzten Phase der Abfragespeicheraktivierung automatisch auf alle Azure SQL-Datenbanken angewendet (siehe wichtigen Hinweis oben). Nach diesem Optimierungsschritt werden die von Kunden festgelegten Konfigurationswerte für Azure SQL-Datenbank nicht mehr geändert, es sei denn, sie wirken sich negativ auf die primäre Workload oder den zuverlässigen Betrieb des Abfragespeichers aus.

Wenn Sie weiterhin Ihre benutzerdefinierten Einstellungen nutzen möchten, helfen Ihnen die Informationen unter [ALTER DATABASE SET-Optionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx) weiter, um die Konfiguration wieder in den vorherigen Zustand zu versetzen. Lesen Sie den Artikel [Bewährte Methoden für den Abfragespeicher](https://msdn.microsoft.com/library/mt604821.aspx), um zu erfahren, wie Sie die optimalen Konfigurationsparameter auswählen.

## Nächste Schritte

[Einblicke in die SQL-Datenbankleistung](sql-database-performance.md)

## Zusätzliche Ressourcen

Weitere Informationen finden Sie in den folgenden Artikeln:

- [A flight data recorder for your database (Ein Flugdatenschreiber für Ihre Datenbank)](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database)

- [Überwachen der Leistung mit dem Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)

- [Query Store Usage Scenarios (Verwendungsszenarien für den Abfragespeicher)](https://msdn.microsoft.com/library/mt614796.aspx)

- [Überwachen der Leistung mit dem Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)

<!---HONumber=AcomDC_0817_2016-->