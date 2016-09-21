<properties
	pageTitle="SQL-Datenbank: Was ist eine DTU? | Microsoft Azure"
	description="Es wird beschrieben, was eine Azure SQL-Datenbanktransaktionseinheit ist."
	keywords="Datenbankoptionen, Datenbankleistung"
	services="sql-database"
	documentationCenter=""
	authors="CarlRabeler"
	manager="jhubbard"
	editor="CarlRabeler"/>

<tags
	ms.service="sql-database"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="NA"
	ms.date="09/06/2016"
	ms.author="carlrab"/>

# Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)

In diesem Artikel werden Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs) beschrieben. Außerdem wird erläutert, was passiert, wenn die Obergrenze für DTUs oder eDTUs erreicht ist.

## Was sind Datenbanktransaktionseinheiten (DTUs)?

Eine DTU ist eine Maßeinheit für die Ressourcen, die für eine eigenständige Azure SQL-Datenbank auf einer bestimmten Leistungsebene der [Dienstebene einer eigenständigen Datenbank](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) garantiert verfügbar sind. Eine DTU ist eine gemischte Kennzahl aus CPU, Arbeitsspeicher und Daten-E/A sowie Transaktionsprotokoll-E/A. Das Verhältnis wird anhand einer OLTP-Benchmark-Workload ermittelt, die für echte OLTP-Workloads typisch ist. Eine Verdoppelung der DTUs durch das Erhöhen der Leistungsebene einer Datenbank ist gleichbedeutend mit der Verdoppelung des Ressourcensatzes, der für die Datenbank verfügbar ist. Beispiel: Eine Premium-P11-Datenbank mit 1.750 DTUs bietet eine 350 mal höhere DTU-Computeleistung als eine Basic-Datenbank mit fünf DTUs. Informationen zur Methodik hinter der OLTP-Benchmark-Workload, die zum Ermitteln der DTU-Mischung verwendet wird, finden Sie unter [Übersicht über das Durchführen von Vergleichstests für Azure SQL-Datenbank](sql-database-benchmark-overview.md).

![Einführung in SQL-Datenbank: Einzeldatenbank-DTUs nach Tarif und Stufe](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Sie können jederzeit [Dienstebenen ändern](sql-database-scale-up.md) und die Ausfallzeiten für Ihre Anwendung dabei gering halten (im Durchschnitt meist unter vier Sekunden). Für viele Unternehmen und Apps genügt es, wenn Datenbanken erstellt werden können und sich die Leistung von Einzeldatenbanken nach oben oder unten anpassen lässt, insbesondere, wenn die Nutzungsmuster relativ vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es jedoch schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. Für dieses Szenario verwenden Sie einen elastischen Pool mit einer bestimmten Anzahl von eDTUs.

## Was sind elastische Datenbanktransaktionseinheiten (eDTUs)?

Eine eDTU ist eine Maßeinheit für den Satz mit den Ressourcen (DTUs), die in einer Gruppe mit Datenbanken auf einem Azure SQL-Server gemeinsam genutzt werden können. Dies wird als [elastischer Pool](sql-database-elastic-pool.png) bezeichnet. Elastische Pools stellen eine einfache und kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken mit unterschiedlichsten und unvorhersehbaren Nutzungsmustern dar. Weitere Informationen finden Sie im Abschnitt [Dienstebenen und Leistung für elastische Pools in eDTUs](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus).

![Einführung in die SQL-Datenbank: eDTUs nach Tarif und Stufe](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Ein Pool erhält eine festgelegte Anzahl von eDTUs zu einem festen Preis. Im Pool können einzelne Datenbanken die automatische Skalierung innerhalb der angegebenen Parameter flexibel automatisch skalieren. Bei hoher Auslastung kann eine Datenbank mehr eDTUs nutzen, um die Anforderungen zu erfüllen. Datenbanken verbrauchen bei geringerer Auslastung weniger Ressourcen und ohne Auslastung gar keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool und nicht nur für einzelne Datenbanken vereinfachen Sie Ihre Verwaltungsaufgaben. Außerdem verfügen Sie über ein vorhersagbares Budget für den Pool.

Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen oder Auswirkungen auf die Datenbanken im elastischen Pool kommt. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Sie können dem Pool Datenbanken hinzufügen oder Datenbanken aus dem Pool entfernen. Wenn die Ressourcen für eine Datenbank voraussichtlich nicht ausgeschöpft werden, sollten Sie sie entfernen.

## Wie kann ich die Anzahl von DTUs bestimmen, die für meine Workload erforderlich sind?

Wenn Sie die vorhandene Workload eines lokalen oder virtuellen SQL Server-Computers zu Azure SQL-Datenbank migrieren möchten, können Sie den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) nutzen, um die ungefähre Anzahl von benötigten DTUs zu berechnen. Für eine vorhandene Azure SQL-Datenbank-Workload können Sie [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md) verwenden, um den Ressourcenverbrauch der Datenbank (DTUs) zu verstehen und Erkenntnisse zur Optimierung der Workload zu gewinnen. Sie können auch die DMV [sys.dm\_db\_ resource\_stats](https://msdn.microsoft.com/library/dn800981.aspx) verwenden, um die Informationen zum Ressourcenverbrauch der letzten Stunde zu erhalten. Alternativ dazu ist es möglich, die Katalogansicht [sys.resource\_stats](http://msdn.microsoft.com/library/dn269979.aspx) abzufragen, um die gleichen Daten für die letzten 14 Tage abzufragen. Da hierfür Durchschnittswerte für jeweils fünf Minuten genutzt werden, ist die Genauigkeit aber geringer.

## Wie kann ermittelt werden, ob sich durch einen elastischen Pool mit Ressourcen Vorteile für mich ergeben?

Pools eignen sich gut für eine große Anzahl von Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine einzelne Datenbank wird dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet. Die SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbankserver und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md).

## Was passiert, wenn die Obergrenze der DTUs erreicht ist?

Leistungsstufen werden kalibriert und gesteuert, um die erforderlichen Ressourcen zum Ausführen der Workload Ihrer Datenbank bis zu den maximalen Grenzwerten bereitzustellen, die für die ausgewählte Dienstebene/Leistungsstufe zulässig sind. Wenn die Workload die Grenzwerte für CPU, Daten-E/A bzw. Protokoll-E/A erreicht, erhalten Sie die Ressourcen auch weiterhin auf der maximal zulässigen Ebene. Es treten aber wahrscheinlich erhöhte Wartezeiten für Ihre Abfragen auf. Diese Höchstgrenzen führen nicht zu Fehlern, sondern nur zu einer Verlangsamung Ihrer Workload. Wenn die Verlangsamung aber zu schwerwiegend ist, tritt ein Timeout für die Abfragen auf. Wenn Sie den Grenzwert für die maximal zulässigen gleichzeitigen Benutzersitzungen/-anforderungen (Arbeitsthreads) erreichen, erhalten Sie explizite Fehler. Informationen zu den Grenzwerten für andere Ressourcen als CPU, Arbeitsspeicher, Daten-E/A und Transaktionsprotokoll-E/A finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md).

## Nächste Schritte

- Unter [Dienstebene](sql-database-service-tiers.md) finden Sie Informationen zu den DTUs und eDTUs, die für eigenständige Datenbanken und elastische Pools verfügbar sind.
- Informationen zu den Grenzwerten für andere Ressourcen als CPU, Arbeitsspeicher, Daten-E/A und Transaktionsprotokoll-E/A finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md).
- Informationen zum Verbrauch (DTUs) finden Sie unter [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md).
- Informationen zur Methodik hinter der OLTP-Benchmark-Workload, die zum Ermitteln der DTU-Mischung verwendet wird, finden Sie unter [Übersicht über das Durchführen von Vergleichstests für Azure SQL-Datenbank](sql-database-benchmark-overview.md).

<!---HONumber=AcomDC_0914_2016-->