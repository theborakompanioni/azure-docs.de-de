<properties
   pageTitle="Verwenden des Abfragespeichers in Azure SQL-Datenbank"
   description="Erfahren Sie, wie Sie den Abfragespeicher in Azure SQL-Datenbank verwenden."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="05/25/2016"
   ms.author="carlrab"/>

# Verwenden des Abfragespeichers in Azure SQL-Datenbank 

Der Abfragespeicher in Azure ist eine vollständig verwaltete Datenbankfunktion, mit der fortlaufend ausführliche Verlaufsinformationen zu allen Abfragen gesammelt und dargestellt werden. Sie können sich den Abfragespeicher in etwa wie den Flugdatenschreiber eines Flugzeugs vorstellen, mit dem die Problembehandlung der Abfrageleistung sowohl für Cloudkunden als auch für lokale Kunden erheblich vereinfacht wird. In diesem Artikel werden spezielle Aspekte der Abfragespeichernutzung in Azure beschrieben. Mit diesen vorab erfassten Abfragedaten können Benutzer Leistungsprobleme schnell diagnostizieren und lösen und haben mehr Zeit, sich auf ihr Geschäft zu konzentrieren.

Der Abfragespeicher ist in Azure SQL-Datenbank seit November 2015 [global verfügbar](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/). Der Abfragespeicher ist die Grundlage für die Leistungsanalyse und Optimierungsfunktionen, z.B. [SQL Database Advisor and Performance Dashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/) (SQL-Datenbank-Ratgeber und Leistungsdashboard). Zum Zeitpunkt der Veröffentlichung dieses Artikels wird der Abfragespeicher in mehr als 200.000 Benutzerdatenbanken in Azure ausgeführt und sammelt schon seit mehreren Monaten ununterbrochen Informationen zu Abfragen.

> [AZURE.IMPORTANT] Microsoft führt gerade die Aktivierung des Abfragespeichers für alle Azure SQL-Datenbanken durch (vorhandene und neue Datenbanken). Dieser Aktivierungsprozess umfasst derzeit nur Singleton-Datenbanken, soll aber in naher Zukunft auf elastische Pooldatenbanken erweitert werden. Aktivieren Sie den Abfragespeicher bei der Verwendung von elastischen Pools nur für eine kleine Teilmenge der Datenbanken. Die Aktivierung des Abfragespeichers für alle Datenbanken in einem elastischen Pool kann zu einer übermäßigen Ressourcennutzung führen und bewirken, dass das System nicht mehr schnell reagiert.

## Abfragespeicher als verwaltete Funktion in Azure SQL-Datenbank

Der Betrieb des Abfragespeichers in Azure SQL-Datenbank basiert auf diesen beiden grundlegenden Prinzipien:

- Keine sichtbaren Auswirkungen auf die Workload von Kunden
- Abfragen werden ständig überwacht, es sei denn, es ergeben sich Auswirkungen auf die Workload von Kunden

Für die Auswirkungen auf die Workload des Kunden gelten zwei Aspekte:

- ***Verfügbarkeit:*** Die [SLA für SQL-Datenbank](https://azure.microsoft.com/support/legal/sla/sql-database/v1_0/) wird nicht reduziert, wenn der Abfragespeicher ausgeführt wird.
- ***Leistung:*** Der durchschnittliche Mehraufwand, der durch den Abfragespeicher anfällt, liegt normalerweise im Bereich von 1 bis 2 %.

Für den Abfragespeicher in Azure werden eingeschränkte Ressourcen verwendet (CPU, Arbeitsspeicher, Datenträger-E/A, Größe auf Datenträger usw.). Es werden verschiedene Systemeinschränkungen beachtet, damit die reguläre Workload möglichst wenig beeinträchtigt wird:

- ***Statische Einschränkungen:*** Einschränkungen aufgrund der Ressourcenkapazität einer bestimmten Dienstebene (Basic, Standard, Premium, Elastischer Pool).
- ***Dynamische Einschränkungen:*** Einschränkungen, die sich durch den aktuellen Verbrauch einer Workload ergeben (also die verfügbaren Ressourcen).

Zur Sicherstellung eines unterbrechungsfreien und zuverlässigen Betriebs verfügt Azure SQL-Datenbank für den Abfragespeicher über eine permanente Überwachungsinfrastruktur, mit der wichtige Betriebsdaten aus jeder Datenbank gesammelt werden. Auf diese Weise werden ständig mehrere technische KPIs überwacht, um für einen zuverlässigen Betrieb zu sorgen:

- Anzahl von Ausnahmen und automatischen Problemlösungen
- Anzahl von Datenbanken mit dem Status READ\_ONLY und Dauer des Status READ\_ONLY
- Top-Datenbanken mit einem über dem Grenzwert befindlichen Arbeitsspeicherverbrauch für den Abfragespeicher
- Top-Datenbanken nach Häufigkeit und Dauer der automatischen Bereinigung
- Top-Datenbanken nach Dauer des Ladens von Daten in den Arbeitsspeicher (während der Initialisierung)
- Top-Datenbanken nach Dauer der Datenleerung auf Datenträger

Azure SQL-Datenbank nutzt die gesammelten Daten für folgende Zwecke:

- ***Erlernen von Verwendungsmustern für eine große Zahl von Datenbanken und Verbessern der Zuverlässigkeit und Qualität von Funktionen:*** Der Abfragespeicher wird mit jedem Update von Azure SQL-Datenbank verbessert. 
- ***Lösen oder Minimieren von Problemen, die vom Abfragespeicher verursacht werden:*** Azure SQL-Datenbank kann innerhalb kurzer Zeit (weniger als eine Stunde) Probleme erkennen und minimieren oder lösen, die sich erheblich auf die Workload von Kunden auswirken. Probleme werden am häufigsten behandelt, indem der Abfragespeicher vorübergehend deaktiviert wird (***OFF***).

Von Zeit zu Zeit werden mit Updates des Abfragespeichers Änderungen an den Standardeinstellungen vorgenommen, die auf interne und in seltenen Fällen auch auf externe Konfigurationen für Kunden angewendet werden. Aus diesem Grund kann sich die Nutzung des Abfragespeichers unter Azure SQL-Datenbank für Kunden gegenüber lokalen Umgebungen unterscheiden, da von der Azure-Plattform automatische Aktionen durchgeführt werden:

- Der Status des Abfragespeichers kann in ***OFF*** geändert werden, um Probleme zu lösen, und dann wieder in ***ON***, nachdem das Problem behoben wurde.
- Die Konfiguration des Abfragespeichers kann geändert werden, um einen zuverlässigen Betrieb sicherzustellen. Hierbei gibt es folgende Möglichkeiten:
    - Individuelle Datenbankänderungen, mit denen Probleme aufgrund von Instabilität oder Unzuverlässigkeit behoben werden.
    - Globale Einführung optimaler Konfigurationsänderungen, aus denen sich Vorteile für alle Datenbanken ergeben, die den Abfragespeicher nutzen.

Die Deaktivierung des Abfragespeichers (***OFF***) ist eine automatische Aktion für individuelle Datenbanken. Sie wird durchgeführt, wenn sich ein Produktverhalten negativ auf Benutzerdatenbanken auswirkt, für die der Erkennungsmechanismus eine Warnung ausgelöst hat. Für eine betroffene Datenbank bleibt der Abfragespeicher deaktiviert (***OFF***), bis eine neue Version mit der verbesserten Implementierung des Abfragespeichers verfügbar ist. Wenn der Übergang in den Status ***OFF*** erfolgt, wird der Kunde per E-Mail informiert. Dem Kunden wird empfohlen, den Abfragespeicher erst wieder zu aktivieren, nachdem eine neue Version bereitgestellt wurde. Nach dem Rollout einer neuen Version aktiviert die Infrastruktur von Azure SQL-Datenbank den Abfragespeicher automatisch für alle Datenbanken, für die er auf ***OFF*** festgelegt wurde.

In selteneren Fällen werden von Azure SQL-Datenbank unter Umständen neue Konfigurationsstandardeinstellungen für alle Benutzerdatenbanken erzwungen, die im Hinblick auf den zuverlässigen Betrieb und die fortlaufende Datensammlung optimiert sind.

## Optimale Konfiguration des Abfragespeichers

Dieser Abschnitt beschreibt die optimalen Standardeinstellungen der Konfiguration, mit denen der zuverlässige Betrieb des Abfragespeichers und der abhängigen Features sichergestellt wird, z.B. [SQL-Datenbank-Ratgeber und Leistungsdashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Die Standardkonfiguration ist für die fortlaufende Datensammlung optimiert. Dies bedeutet, dass möglichst wenig Zeit im Status OFF bzw. READ\_ONLY verbracht wird.

| Konfiguration | Beschreibung | Standard | Kommentar |
| ------------- | ----------- | ------- | ------- |
| MAX\_STORAGE\_SIZE\_MB | Gibt die Beschränkung für den Datenspeicherplatz an, den der Abfragespeicher in der Kundendatenbank belegt. | 100 | Für neue Datenbanken erzwungen |
| INTERVAL\_LENGTH\_MINUTES | Definiert die Größe des Zeitfensters, in dem gesammelte Laufzeitstatistiken für Abfragepläne aggregiert und dauerhaft gespeichert werden. Jeder aktive Abfrageplan verfügt über eine Zeile für einen Zeitraum mit dieser Konfiguration. | 60 | Für neue Datenbanken erzwungen |
| STALE\_QUERY\_THRESHOLD\_DAYS | Zeitbasierte Bereinigungsrichtlinie, mit der der Aufbewahrungszeitraum für dauerhaft gespeicherte Laufzeitstatistiken und inaktive Abfragen gesteuert wird. | 30 | Für neue Datenbanken und Datenbanken mit vorheriger Standardeinstellung erzwungen (367) |
| SIZE\_BASED\_CLEANUP\_MODE | Gibt an, ob die automatische Datenbereinigung durchgeführt wird, wenn der Datenumfang des Abfragespeichers in Kürze den Grenzwert erreicht. | AUTO | Für alle Datenbanken erzwungen |
| QUERY\_CAPTURE\_MODE | Gibt an, ob alle Abfragen nachverfolgt werden oder nur eine Teilmenge der Abfragen nachverfolgt wird. | AUTO | Für alle Datenbanken erzwungen |
| FLUSH\_INTERVAL\_SECONDS | Gibt an, wie lange gesammelte Laufzeitstatistiken maximal im Arbeitsspeicher aufbewahrt werden, bevor die Leerung auf Datenträger erfolgt. | 900 | Für neue Datenbanken erzwungen |
||||||

> [AZURE.IMPORTANT] Diese Standardeinstellungen werden in der letzten Phase der Abfragespeicheraktivierung automatisch auf alle Azure SQL-Datenbanken angewendet (siehe wichtigen Hinweis oben). Nach diesem Optimierungsschritt werden die von Kunden festgelegten Konfigurationswerte für Azure SQL-Datenbank nicht mehr geändert, es sei denn, sie wirken sich negativ auf die primäre Workload oder den zuverlässigen Betrieb des Abfragespeichers aus.

Falls Sie weiterhin Ihre benutzerdefinierten Einstellungen nutzen möchten, helfen Ihnen die Informationen unter [ALTER DATABASE SET-Optionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx) weiter, um die Konfiguration wieder in den vorherigen Zustand zu versetzen. Lesen Sie den Artikel [Bewährte Methoden für den Abfragespeicher](https://msdn.microsoft.com/library/mt604821.aspx), um zu erfahren, wie Sie die optimalen Konfigurationsparameter auswählen.

## Nächste Schritte

[Einblicke in die SQL-Datenbankleistung](sql-database-performance.md)

## Weitere Ressourcen

Weitere Informationen finden Sie in den folgenden Artikeln:

- [A flight data recorder for your database (Ein Flugdatenschreiber für Ihre Datenbank)](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Überwachen der Leistung mit dem Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)

- [Query Store Usage Scenarios (Verwendungsszenarien für den Abfragespeicher)](https://msdn.microsoft.com/library/mt614796.aspx)

- [Überwachen der Leistung mit dem Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)

<!---HONumber=AcomDC_0615_2016-->