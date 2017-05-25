---
title: "Azure SQL-Datenbank-Leistung für Einzeldatenbanken | Microsoft-Dokumentation"
description: "Mit den Informationen in diesem Artikel können Sie ermitteln, welche Dienstebene für Ihre Anwendung am besten geeignet ist. Außerdem werden Möglichkeiten zum Optimieren Ihrer Anwendung empfohlen, um mit Azure SQL-Datenbank das beste Ergebnis zu erzielen."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: dd8d95fa-24b2-4233-b3f1-8e8952a7a22b
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
wms.date: 05/14/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 3663b9005c68fd95158a81b54767a234ad5f37c4
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL-Datenbank und Leistung für Einzeldatenbanken
Azure SQL-Datenbank bietet vier [Dienstebenen](sql-database-service-tiers.md): Basic, Standard, Premium und Premium RS. Auf jeder Dienstebene sind die Ressourcen, die von der SQL-Datenbank genutzt werden können, streng voneinander isoliert, und es wird eine vorhersagbare Leistung für die Dienstebene sichergestellt. In diesem Artikel erhalten Sie nützliche Informationen zum Auswählen der Dienstebene für Ihre Anwendung. Außerdem werden Möglichkeiten zum Optimieren Ihrer Anwendung beschrieben, um mit Azure SQL-Datenbank das beste Ergebnis zu erzielen.

> [!NOTE]
> In diesem Artikel geht es schwerpunktmäßig um die Verbesserung der Leistung für Einzeldatenbanken in Azure SQL-Datenbank. Informationen zur Verbesserung der Leistung für elastische Pools finden Sie unter [Wo sollte ein elastischer Pool verwendet werden?](sql-database-elastic-pool.md). Beachten Sie aber, dass Sie viele Optimierungsempfehlungen in diesem Artikel auf Datenbanken in einem elastischen Pool anwenden und ähnliche Leistungsvorteile erzielen können.
>
>

## <a name="why-service-tiers"></a>Warum werden Dienstebenen verwendet?
Jede Datenbankworkload kann sich zwar unterscheiden, aber der Zweck von Dienstebenen besteht darin, für verschiedene Leistungsebenen eine Vorhersagbarkeit der Leistung zu ermöglichen. Kunden mit höheren Anforderungen an Datenbankressourcen können in einer dedizierteren Computingumgebung arbeiten.

### <a name="common-service-tier-use-cases"></a>Häufige Anwendungsfälle für Dienstebenen
#### <a name="basic"></a>Basic
* **Sie stehen am Anfang der Nutzung von Azure SQL-Datenbank**. Für Anwendungen, die sich in der Entwicklung befinden, sind häufig keine hohen Leistungsebenen erforderlich. Basic-Datenbanken stellen eine ideale Umgebung für die Datenbankentwicklung mit einem niedrigen Preispunkt dar.
* **Sie verfügen über eine Datenbank mit nur einem Benutzer**. Für Anwendungen, bei denen einer Datenbank nur ein Benutzer zugeordnet wird, bestehen in der Regel keine hohen Anforderungen an Parallelität und Leistung. Diese Anwendungen sind Kandidaten für die Dienstebene Basic.

#### <a name="standard"></a>Standard
* **Ihre Datenbank verfügt über mehrere gleichzeitige Anforderungen**. Anwendungen, die für mehr als einen Benutzer gleichzeitig bestimmt sind, benötigen normalerweise höhere Leistungsebenen. Websites mit eher geringem Datenverkehr oder Anwendungen von Abteilungen, für die mehr Ressourcen benötigt werden, sind beispielsweise gute Kandidaten für die Dienstebene Standard.

#### <a name="premium"></a>Premium
Für die meisten Anwendungsfälle der Dienstebene Premium gelten die folgenden Merkmale bzw. Teile dieser Merkmale:

* **Hohe Spitzenlast**. Eine Anwendung, für die zum Durchführen der Vorgänge hohe Werte in Bezug auf CPU, Arbeitsspeicher oder Eingang/Ausgang (I/O) erforderlich sind, wird eine dedizierte, hohe Leistungsebene benötigt. Falls ein Datenbankvorgang über einen längeren Zeitraum mehrere CPU-Kerne nutzt, ist dies beispielsweise ein Kandidat für die Dienstebene Premium.
* **Hohe Zahl von gleichzeitigen Anforderungen**: Einige Datenbankanwendungen verarbeiten viele gleichzeitige Anforderungen, z.B. bei einer Website mit hohem Datenverkehrsaufkommen. Für die Dienstebenen Basic und Standard gelten bei der Anzahl von gleichzeitigen Anforderungen bestimmte Einschränkungen pro Datenbank. Für Anwendungen, die mehr Verbindungen benötigen, muss eine angemessene Reservierungsgröße gewählt werden, um die maximale Anzahl von erforderlichen Anforderungen verarbeiten zu können.
* **Niedrige Latenz**. Für einige Anwendungen muss eine Reaktion der Datenbank in kürzester Zeit garantiert werden. Wenn eine bestimmte gespeicherte Prozedur im Rahmen eines größeren Kundenvorgangs aufgerufen wird, kann unter Umständen die Anforderung bestehen, dass die Rückgabe für diesen Aufruf in 99 Prozent der Fälle innerhalb von maximal 20 Millisekunden erfolgt. Diese Art von Anwendung profitiert von der Dienstebene Premium, da sichergestellt ist, dass genügend erforderliche Rechenleistung verfügbar ist.

* **Premium RS**. Speziell für Kunden mit E/A-intensiven Workloads, die aber nicht die höchsten Verfügbarkeitsgarantien benötigen. Beispiele hierfür sind das Testen von Hochleistungsworkloads oder eine analytische Workload, wo die Datenbank nicht das bevorzugte System ist.

Die Dienstebene, die Sie für Ihre SQL-Datenbank benötigen, richtet sich nach den Spitzenlastanforderungen für die einzelnen Ressourcendimensionen. Bei einigen Anwendungen wird nur ein geringer Anteil einer Ressource genutzt, aber dafür bestehen erhebliche Anforderungen in Bezug auf andere Ressourcen.

## <a name="service-tier-capabilities-and-limits"></a>Funktionen und Beschränkungen von Dienstebenen
Jeder Dienstebene und Leistungsebene sind verschiedene Beschränkungen und Leistungsmerkmale zugeordnet. In dieser Tabelle sind diese Merkmale für eine Einzeldatenbank beschrieben.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> Kunden mit den Leistungsstufen P11 und P15 können bis zu 4TB Speicher ohne Aufpreis nutzen. Die 4 TB-Option ist derzeit in den folgenden Regionen verfügbar: USA, Osten 2; USA, Westen, USA Reg-Bez. Virginia; Europa, Westen; Deutschland, Mitte; Asien, Südosten; Japan, Osten; Australien, Osten; Kanada, Mitte und Kanada, Osten.
>

### <a name="maximum-in-memory-oltp-storage"></a>Maximaler In-Memory-OLTP-Speicher
Sie können die **sys.dm_db_resource_stats**-Ansicht verwenden, um die Verwendung des Azure-In-Memory-Speichers zu überwachen. Weitere Informationen zur Überwachung finden Sie unter [Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md).

### <a name="maximum-concurrent-requests"></a>Maximale Anzahl von gleichzeitigen Anforderungen
Um die Anzahl gleichzeitiger Anforderungen anzuzeigen, führen Sie in der SQL-Datenbank diese Transact-SQL-Abfrage aus:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Ändern Sie diese Abfrage zum Analysieren der Workload einer lokalen SQL Server-Datenbank, um nach der entsprechenden Datenbank zu filtern, die analysiert werden soll. Wenn Sie beispielsweise über eine lokale Datenbank mit dem Namen „MyDatabase“ verfügen, wird mit dieser Transact-SQL-Abfrage die Anzahl von gleichzeitigen Abfragen in dieser Datenbank zurückgegeben:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dies ist nur eine Momentaufnahme zu einem bestimmten Zeitpunkt. Um ein besseres Verständnis Ihrer Workload und der Anforderungen an gleichzeitige Anforderungen zu entwickeln, müssten Sie im Laufe der Zeit viele Beispiele sammeln.

### <a name="maximum-concurrent-logins"></a>Maximale Anzahl von gleichzeitigen Anmeldungen
Sie können Ihre Benutzer- und Anmeldungsmuster analysieren, um Informationen zur Häufigkeit der Anmeldungen zu erhalten. Sie können auch reale Auslastungen in einer Testumgebung ausführen, um sicherzustellen, dass diese oder andere in diesem Artikel beschriebene Grenzwerte nicht erreicht oder überschritten werden. Es gibt keine einzelne Abfrage oder dynamische Verwaltungssicht (DMV), mit der Sie die Anzahl von gleichzeitigen Anmeldungen oder den Verlauf anzeigen können.

Der Dienst authentifiziert jede Anmeldung, wenn mehrere Clients die gleiche Verbindungszeichenfolge verwenden. Wenn zehn Benutzer gleichzeitig mit dem gleichen Benutzernamen und Kennwort mit einer Datenbank verbunden sind, wären dies also zehn gleichzeitige Anmeldungen. Diese Obergrenze gilt nur für die Dauer der Anmeldung und Authentifizierung. Wenn sich die gleichen zehn Benutzer nacheinander an der Datenbank anmelden, wäre die Anzahl von gleichzeitigen Anmeldungen niemals höher als 1.

> [!NOTE]
> Diese Beschränkung gilt derzeit nicht für Datenbanken in elastischen Pools.
>
>

### <a name="maximum-sessions"></a>Maximale Anzahl von Sitzungen
Um die Anzahl aktueller aktiver Sitzungen anzuzeigen, führen Sie in der SQL-Datenbank diese Transact-SQL-Abfrage aus:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Ändern Sie die Abfrage beim Analysieren einer lokalen SQL Server-Workload, um sie auf eine bestimmte Datenbank auszurichten. Diese Abfrage ist zum Ermitteln der möglichen Sitzungsanforderungen für die Datenbank hilfreich, wenn Sie eine Verschiebung zur Azure SQL-Datenbank erwägen.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Diese Abfragen geben wieder eine Anzahl zu einem bestimmten Zeitpunkt zurück. Wenn Sie im Laufe der Zeit mehrere Beispielwerte sammeln, können Sie sich am besten über Ihre Sitzungsnutzung informieren.

Für die SQL-Datenbankanalyse können Sie Verlaufsstatistiken für Sitzungen abrufen, indem Sie [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) abfragen und die Spalte **active_session_count** überprüfen.

## <a name="monitor-resource-use"></a>Überwachen der Ressourcennutzung

Sie können die Ressourcennutzung mithilfe von [Query Performance Insight für SQL-Datenbank](sql-database-query-performance.md) und [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx) überwachen.

Außerdem lässt sich die Nutzung über diese beiden Ansichten überwachen:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Sie können die Sicht [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in jeder SQL-Datenbank verwenden. Die Sicht **sys.dm_db_resource_stats** enthält Daten zur Ressourcennutzung in der letzten Zeit relativ zur Dienstebene. Durchschnittliche Prozentsätze für CPU, Dateneingang/-ausgang, Protokollschreibvorgänge und Arbeitsspeicher werden alle 15 Sekunden aufgezeichnet und eine Stunde lang aufbewahrt.

Da diese Ansicht eine detailliertere Darstellung der Ressourcennutzung ist, sollten Sie für alle Analysen des aktuellen Zustands oder für die Problembehandlung zuerst **sys.dm_db_resource_stats** verwenden. Mit dieser Abfrage wird beispielsweise die durchschnittliche und maximale Ressourcennutzung für die aktuelle Datenbank in der letzten Stunde angezeigt:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Beispiele für andere Abfragen finden Sie unter [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats
Die Sicht [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in der **master**-Datenbank enthält zusätzliche Informationen, die zur Überwachung der Leistung Ihrer SQL-Datenbank innerhalb der jeweiligen Dienst- und Leistungsebene hilfreich sind. Die Daten werden alle fünf Minuten gesammelt und c.a. 14 Tage lang aufbewahrt. Diese Sicht ist für eine längere Verlaufsanalyse der Ressourcennutzung Ihrer SQL-Datenbank hilfreich.

Der folgende Graph zeigt die CPU-Ressourcennutzung für eine Premium-Datenbank mit der Leistungsebene P2 für jede Stunde einer Woche. Dieser Graph beginnt mit einem Montag und zeigt fünf Arbeitstage und dann das Wochenende, an dem die Anwendung deutlich weniger gefragt ist.

![Ressourcennutzung für SQL-Datenbank](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Die Daten verdeutlichen, dass diese Datenbank derzeit über eine CPU-Spitzenlast von etwas mehr als 50 Prozent CPU-Auslastung relativ zur Leistungsebene P2 verfügt (Dienstagmittag). Falls die CPU der entscheidende Faktor im Ressourcenprofil der Anwendung ist, entscheiden Sie sich ggf. für die Leistungsebene P2, um die Bewältigung der Workload stets sicherstellen zu können. Wenn eine Anwendung im Laufe der Zeit voraussichtlich größer wird, ist die Verwendung eines zusätzlichen Ressourcenpuffers ratsam, damit für die Anwendung nie der Grenzwert für die Leistungsebene erreicht wird. Wenn Sie die Leistungsebene erhöhen, können Sie für den Kunden sichtbare Fehler vermeiden. Diese können ggf. auftreten, wenn eine Datenbank nicht über genügend Leistung zum effektiven Verarbeiten von Anforderungen verfügt, vor allem in latenzanfälligen Umgebungen. Ein Beispiel ist eine Datenbank für eine Anwendung, mit der Webseiten basierend auf den Ergebnissen von Datenbankaufrufen farbig gestaltet werden.

Andere Anwendungstypen können denselben Graphen unter Umständen anders interpretieren. Wenn eine Anwendung beispielsweise jeden Tag versucht, Gehaltsabrechnungsdaten zu verarbeiten, und dasselbe Diagramm gilt, wird diese Art von „Batchauftrag“-Modell bei Leistungsebene P1 ggf. zufriedenstellend ausgeführt. Die Leistungsebene P1 verfügt über 100 DTUs, während Leistungsebene P2 über 200 DTUs verfügt. Die Leistungsebene P1 bietet also die Hälfte der Leistung von Leistungsebene P2. Eine Nutzung von 50 Prozent CPU-Auslastung bei P2 entspricht also 100 Prozent CPU-Auslastung bei P1. Wenn die Anwendung nicht über Timeouts verfügt, spielt es unter Umständen keine Rolle, ob ein Auftrag 2 oder 2,5 Stunden dauert, solange er noch am selben Tag abgeschlossen wird. Für eine Anwendung in dieser Kategorie reicht wahrscheinlich die Leistungsebene P1 aus. Sie können die Tatsache nutzen, dass es am Tag Zeiten gibt, in denen die Ressourcennutzung niedriger ist. Dies bedeutet, dass „Spitzen“ ggf. in einen der Zeiträume später am Tag verlagert werden können. Die Leistungsebene P1 ist für diese Art von Anwendung ggf. gut geeignet (und spart Kosten), solange die Aufträge am selben Tag abgeschlossen werden können.

Azure SQL-Datenbank macht die verbrauchten Ressourceninformationen für jede aktive Datenbank in der Sicht **sys.resource_stats** der **master**-Datenbank jedes Servers verfügbar. Die Daten in der Tabelle werden zu Intervallen von fünf Minuten zusammengefasst. Bei den Dienstebenen Basic, Standard, Premium und Premium RS kann es länger als fünf Minuten dauern, bis sie in der Tabelle angezeigt werden. Diese Daten sind also besser für Verlaufsanalysen als für Analysen nahezu in Echtzeit geeignet. Fragen Sie die Sicht **sys.resource_stats** ab, um den kürzlichen Verlauf einer Datenbank anzuzeigen und zu überprüfen, ob die gewählte Reservierung zur gewünschten Leistung zur richtigen Zeit geführt hat.

> [!NOTE]
> Sie müssen mit der **master**-Datenbank Ihres logischen SQL-Datenbankservers verbunden sein, um **sys.resource_stats** in den folgenden Beispielen abzufragen.
>
>

Dieses Beispiel veranschaulicht, wie die Daten in dieser Sicht verfügbar gemacht werden:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Katalogsicht „sys.resource_stats“](./media/sql-database-performance-guidance/sys_resource_stats.png)

Das nächste Beispiel enthält unterschiedliche Möglichkeiten zum Einsatz der **sys.resource_stats**-Katalogsicht, um Informationen zur Nutzung von Ressourcen durch Ihre SQL-Datenbank zu erhalten:

1. Sie können diese Abfrage ausführen, um die Ressourcennutzung der letzten Woche für die Datenbank „userdb1“ anzuzeigen:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. Um auszuwerten, wie gut Ihre Workload zur Leistungsebene passt, müssen Sie die einzelnen Aspekte der Ressourcenmetriken untersuchen: CPU, Lesevorgänge, Schreibvorgänge, Anzahl von Workern und Anzahl von Sitzungen. Hier wird von einer überarbeiteten Abfrage **sys.resource_stats** verwendet, um den Durchschnitt und die Höchstwerte dieser Ressourcenmetriken zu melden:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Mit diesen Informationen zu den Durchschnitts- und Höchstwerten der Ressourcenmetriken können Sie bewerten, wie gut Ihre Workload zur gewählten Leistungsebene passt. Normalerweise erhalten Sie mit den Durchschnittswerten aus **sys.resource_stats** eine gute Grundlage gegenüber der Zielgröße. Dies sollte Ihre primäre Messlatte sein. Beispielsweise können Sie die Standard-Dienstebene mit der Leistungsebene S2 verwenden. Die durchschnittlichen Nutzungsprozentsätze für CPU- und I/O-Lese- und -Schreibvorgänge liegen unter 40 Prozent, die durchschnittliche Anzahl von Workern unter 50 und die durchschnittliche Anzahl von Sitzungen unter 200. Für diese Workload ist unter Umständen die Leistungsebene S1 geeignet. Es ist leicht zu erkennen, ob Ihre Datenbank die Grenzen für Worker und Sitzungen einhält. Um zu ermitteln, ob eine Datenbank in Bezug auf CPU, Lesevorgänge und Schreibvorgänge zu einer niedrigeren Leistungsebene passt, dividieren Sie die DTU-Anzahl der niedrigeren Leistungsebene durch die DTU-Anzahl Ihrer aktuellen Leistungsebene und multiplizieren das Ergebnis dann mit 100:

    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**

    Das Ergebnis ist der relative Leistungsunterschied zwischen den beiden Leistungsebenen als Prozentsatz. Wenn die Ressourcennutzung diese Menge nicht überschreitet, kann für Ihre Workload ggf. die niedrigere Leistungsstufe geeignet sein. Sie sollten sich aber alle Bereiche der Ressourcennutzung ansehen und den Prozentsatz dafür ermitteln, wie oft Ihre Datenbankworkload in die niedrigere Leistungsebene passt. Mit der folgenden Abfrage wird der Prozentsatz für die Eignung pro Ressourcendimension basierend auf dem in diesem Beispiel berechneten Schwellenwert von 40% ausgegeben:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    In Abhängigkeit Ihres Servicelevelziels (Service Level Objective, SLO) für die Datenbank können Sie entscheiden, ob Ihre Workload in die niedrigere Leistungsebene passt. Wenn der SLO-Wert für die Datenbankworkload 99,9 Prozent beträgt und die obige Abfrage höhere Werte als 99,9 Prozent für alle drei Ressourcendimensionen zurückgibt, ist die Wahrscheinlichkeit hoch, dass Ihre Workload für die niedrigere Leistungsebene geeignet ist.

    Wenn Sie sich den Prozentsatz für die Eignung ansehen, erhalten Sie auch Informationen dazu, ob Sie zur nächsthöheren Leistungsebene wechseln müssen, um das Servicelevelziel zu erreichen. „userdb1“ weist beispielsweise die folgende CPU-Auslastung für die letzte Woche auf:

   | Durchschnittlicher CPU-Prozentwert | Maximaler CPU-Prozentwert |
   | --- | --- |
   | 24,5 |100,00 |

    Der durchschnittliche CPU-Wert beträgt ca. ein Viertel der Obergrenze der Leistungsebene. Dies würde also gut zur Leistungsebene der Datenbank passen. Der Höchstwert zeigt aber, dass die Datenbank die Obergrenze der Leistungsebene erreicht. Müssen Sie also zur nächsthöheren Leistungsebene wechseln? Prüfen Sie, wie häufig Ihre Workload 100 Prozent erreicht, und vergleichen Sie dies dann mit Ihrem Servicelevelziel für die Datenbankworkload.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Wenn diese Abfrage für eine der drei Ressourcendimensionen einen Wert unterhalb von 99,9 Prozent zurückgibt, können Sie entweder die Umstellung auf die nächsthöhere Leistungsebene erwägen oder Verfahren zur Anwendungsoptimierung nutzen, um die Last für die SQL-Datenbank zu reduzieren.
4. Bei dieser Übung wird auch die projizierte zukünftige Zunahme der Workload berücksichtigt.

## <a name="tune-your-application"></a>Optimieren der Anwendung
Beim herkömmlichen lokalen Einsatz von SQL Server ist die anfängliche Kapazitätsplanung häufig von der Ausführung einer Anwendung in der Produktion getrennt. Zuerst werden die Hardware und die Produktlizenzen gekauft, und anschließend wird die Leistungsoptimierung durchgeführt. Wenn Sie Azure SQL-Datenbank verwenden, ist es ratsam, den Prozess zur Ausführung und Optimierung einer Anwendung einzubinden. Beim Modell mit der Bezahlung für bedarfsabhängige Kapazität können Sie Ihre Anwendung so optimieren, dass die jeweils benötigten minimalen Ressourcen genutzt werden, anstatt eine Hardware-Überbereitstellung basierend auf dem geschätzten zukünftigen Wachstum einer Anwendung durchzuführen. Diese Schätzungen stimmen häufig nicht. Einige Kunden entscheiden sich aber unter Umständen gegen die Optimierung einer Anwendung und stattdessen für die Überbereitstellung von Hardwareressourcen. Dieser Ansatz kann vorteilhaft sein, wenn Sie für eine wichtige Anwendung während eines Zeitraums mit hoher Auslastung keine Änderungen durchführen möchten. Durch das Optimieren einer Anwendung können aber Ressourcenanforderungen minimiert und monatliche Rechnungen reduziert werden, wenn Sie die Dienstebenen in Azure SQL-Datenbank verwenden.

### <a name="application-characteristics"></a>Anwendungsmerkmale
Azure SQL-Datenbank-Dienstebenen sind zwar für die Verbesserung der Leistungsstabilität und Vorhersagbarkeit für eine Anwendung ausgelegt, aber es gibt einige bewährte Methoden, mit denen Sie Ihre Anwendung so optimieren können, dass die Ressourcen einer Leistungsebene besser genutzt werden. Für viele Anwendungen können zwar erhebliche Leistungsgewinne erzielt werden, indem einfach auf eine höhere Leistungs- oder Dienstebene umgestellt wird, aber für einige Anwendungen ist eine weitere Optimierung erforderlich, um von einer höheren Dienstebene zu profitieren. Sie können zur Steigerung der Leistung eine zusätzliche Optimierung von Anwendungen erwägen, die über die folgenden Merkmale verfügen:

* **Anwendungen mit langsamer Leistung aufgrund von vielen Einzelaufrufen**: Bei Anwendungen mit vielen Einzelaufrufen kommt es zu einer übermäßig hohen Zahl von Datenzugriffen und somit zu erhöhter Netzwerklatenz. Sie müssen diese Arten von Anwendungen ggf. ändern, um die Anzahl von Datenzugriffsvorgängen auf die SQL-Datenbank zu reduzieren. Beispielsweise können Sie die Anwendungsleistung verbessern, indem Sie Verfahren wie das Zusammenfassen von Ad-hoc-Abfragen in Batches oder das Verschieben der Abfragen in gespeicherte Prozeduren verwenden. Weitere Informationen finden Sie unter [Batch-Abfragen](#batch-queries).
* **Datenbanken mit einer intensiven Workload, die nicht von einem gesamten einzelnen Computer unterstützt werden können**. Datenbanken, die die Ressourcen der höchsten Premium-Leistungsebene überschreiten, können vom horizontalen Hochskalieren der Workload profitieren. Weitere Informationen finden Sie unter [Datenbankübergreifendes Sharding](#cross-database-sharding) und [Funktionale Partitionierung](#functional-partitioning).
* **Anwendungen mit suboptimalen Abfragen**: Anwendungen, vor allem auf der Datenzugriffsschicht, die über unzureichend optimierte Abfragen verfügen, profitieren ggf. nicht von einer höheren Leistungsebene. Dies umfasst auch Abfragen, die keine WHERE-Klausel aufweisen, bei denen Indizes fehlen oder die über veraltete Statistiken verfügen. Diese Anwendungen profitieren von Standardverfahren zur Optimierung der Abfrageleistung. Weitere Informationen finden Sie unter [Fehlende Indizes](#missing-indexes) und [Abfrageoptimierung/Abfragehinweise](#query-tuning-and-hinting).
* **Anwendungen mit suboptimalem Datenzugriffsdesign**. Anwendungen, die über inhärente Parallelitätsprobleme in Bezug auf den Datenzugriff verfügen, z.B. „Deadlocking“, profitieren ggf. nicht von der Wahl einer höheren Leistungsebene. Erwägen Sie die Reduzierung von Roundtrips für die Azure SQL-Datenbank, indem Daten auf Clientseite mit dem Azure-Cachedienst oder einer anderen Cachingtechnologie zwischengespeichert werden. Weitere Informationen finden Sie unter [Zwischenspeicherung auf Anwendungsebene](#application-tier-caching).

## <a name="tuning-techniques"></a>Optimierungsverfahren
In diesem Abschnitt werden einige Verfahren beschrieben, mit denen Sie Azure SQL-Datenbank so optimieren können, dass Sie für Ihre Anwendung die beste Leistung erzielen und für die Ausführung die kleinstmögliche Leistungsebene wählen können. Einige Verfahren dieser Art sind mit herkömmlichen bewährten Methoden zum Optimieren von SQL Server identisch, aber die anderen Verfahren gelten speziell für Azure SQL-Datenbank. In einigen Fällen können Sie die verbrauchten Ressourcen für eine Datenbank untersuchen, um Bereiche zu ermitteln, in denen eine weitere Optimierung möglich ist und herkömmliche SQL Server-Verfahren so erweitert werden können, dass sie auch für Azure SQL-Datenbank funktionieren.

### <a name="azure-portal-tools"></a>Tools des Azure-Portals
Mit den folgenden Tools im Azure-Portal können Sie Leistungsprobleme der SQL-Datenbank analysieren und beheben:

* [Query Performance Insight](sql-database-query-performance.md)
* [SQL-Datenbankratgeber](sql-database-advisor.md)

Das Azure-Portal enthält weitere Informationen zu diesen beiden Tools und ihrer Verwendung. Es ist ratsam, zuerst die Tools im Azure-Portal auszuprobieren, um Probleme effizient diagnostizieren und beheben zu können. Es wird empfohlen, in besonderen Fällen die als Nächstes beschriebenen Ansätze für die manuelle Optimierung beim Fehlen von Indizes und für die Abfragenoptimierung zu nutzen.

### <a name="missing-indexes"></a>Fehlende Indizes
Ein häufiges Problem in Bezug auf die OLTP-Datenbankleistung ist der physische Datenbankentwurf. Datenbankschemas werden häufig entworfen und bereitgestellt, ohne dass Skalierungstests durchgeführt werden (entweder in Bezug auf die Auslastung oder das Datenvolumen). Leider kann es passieren, dass die Leistung eines Abfrageplans auf niedriger Ebene akzeptabel ist, dann jedoch erheblich nachlässt, wenn Datenvolumina auf Produktionsebene verarbeitet werden müssen. Die häufigste Ursache dieses Problems ist das Fehlen geeigneter Indizes für Filter oder andere Einschränkungen in einer Abfrage. Das Fehlen von Indizes manifestiert sich häufig als Tabellenscan, wenn eine Indexsuche ausreichend wäre.

In diesem Beispiel wird für den ausgewählten Abfrageplan ein Scan genutzt, obwohl eine Suche ausreichen würde:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Abfrageplan mit fehlenden Indizes](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL-Datenbank unterstützt Sie beim Finden und Beheben von häufigen Zuständen mit fehlenden Indizes. Mit in Azure SQL-Datenbank integrierten DMVs wird die Abfragenkompilierung daraufhin untersucht, ob ein Index die geschätzten Kosten zum Ausführen einer Abfrage erheblich reduzieren würde. Während der Abfrageausführung wird mit SQL-Datenbank nachverfolgt, wie häufig jeder Abfrageplan ausgeführt wird. Außerdem wird die geschätzte Differenz zwischen dem ausgeführten Abfrageplan und dem imaginären Abfrageplan mit dem Index nachverfolgt. Sie können diese DMVs verwenden, um schnell abzuschätzen, welche Änderungen am physischen Datenbankdesign zu einer Verbesserung der Workload-Gesamtkosten für eine Datenbank und der tatsächlichen Workload führen können.

Sie können die folgende Abfrage verwenden, um eventuell fehlende Indizes zu ermitteln:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

In diesem Beispiel hat die Abfrage zu folgender Empfehlung geführt:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Nach der Erstellung wählt dieselbe SELECT-Anweisung einen anderen Plan, bei der anstelle eines Scans eine Suche verwendet und der Plan dann effizienter ausgeführt wird:

![Abfrageplan mit korrigierten Indizes](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Die wichtigste Erkenntnis besteht darin, dass die E/A-Kapazität eines freigegebenen Warensystems mit mehr Einschränkungen als ein dedizierter Servercomputer versehen ist. Es ist wichtig, unnötige E/A-Vorgänge zu reduzieren, um das System innerhalb des DTU-Werts jeder Leistungsebene der Azure SQL-Datenbank-Dienstebenen bestmöglich zu nutzen. Die Wahl des richtigen Designs der physischen Datenbank kann die Latenz für einzelne Abfragen und den Durchsatz gleichzeitiger Anforderungen pro Skalierungseinheit erheblich verbessern und die erforderlichen Kosten zur Erfüllung der Abfrage reduzieren. Weitere Informationen zu den fehlenden Index-DMVs finden Sie unter [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Abfrageoptimierung/Abfragehinweise
Der Abfrageoptimierer in Azure SQL-Datenbank ähnelt dem herkömmlichen SQL Server-Abfrageoptimierer. Die meisten bewährten Methoden zum Optimieren von Abfragen und Verstehen der Einschränkungen des Argumentationsmodells für den Abfrageoptimierer gelten auch für Azure SQL-Datenbank. Wenn Sie in Azure SQL-Datenbank Abfragen optimieren, kommen Sie ggf. in den Genuss des zusätzlichen Vorteils, die aggregierten Ressourcenanforderungen reduzieren zu können. Ihre Anwendung kann unter Umständen zu geringeren Kosten als eine nicht optimierte gleichwertige Anwendung ausgeführt werden, weil eine niedrigere Leistungsebene gewählt werden kann.

Ein Beispiel, das in SQL Server häufig vorkommt und auch für Azure SQL-Datenbank gilt, ist das Ermitteln (Englisch: Sniffing) von Parametern durch den Abfrageoptimierer. Während der Kompilierung wertet der Abfrageoptimierer den aktuellen Wert eines Parameters aus, um zu bestimmen, ob ein besserer Abfrageplan generiert werden kann. Diese Strategie kann zwar häufig zu einem Abfrageplan führen, der deutlich schneller als ein ohne bekannte Parameterwerte kompilierter Plan ist, aber derzeit funktioniert dies in SQL Server und Azure SQL-Datenbank nicht fehlerfrei. Es kann vorkommen, dass der Parameter nicht ermittelt wird oder dass der Parameter zwar ermittelt wird, der generierte Plan für den vollständigen Satz mit Parameterwerten in einer Workload aber suboptimal ist. Microsoft bindet Abfragehinweise (Direktiven) ein, damit Sie Ihre Absichten besser angeben und das Standardverhalten der Parameterermittlung außer Kraft setzen können. Häufig können mit der Verwendung von Hinweisen Fälle behoben werden, in denen das Standardverhalten von SQL Server oder Azure SQL-Datenbank für eine bestimmte Kundenworkload nicht perfekt ist.

Im nächsten Beispiel wird veranschaulicht, wie der Abfrageprozessor einen Plan generieren kann, der sowohl für Leistungs- als auch für Ressourcenanforderungen suboptimal ist. Dieses Beispiel verdeutlicht auch Folgendes: Wenn Sie einen Abfragehinweis verwenden, können Sie die Abfragelaufzeit und die Ressourcenanforderungen für Ihre SQL-Datenbank reduzieren:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Mit dem Einrichtungscode wird eine Tabelle mit einer „schiefen“ Datenverteilung erstellt. Der optimale Abfrageplan variiert in Abhängigkeit davon, welcher Parameter ausgewählt wird. Leider führt das Cachingverhalten des Plans nicht immer zu einer Neukompilierung der Abfrage, die auf dem häufigsten Parameterwert basiert. Es ist also möglich, dass ein suboptimaler Plan auch dann zwischengespeichert und für viele Werte verwendet wird, wenn ein anderer Plan generell eine bessere Wahl wäre. Anschließend erstellt der Abfrageplan zwei gespeicherte Prozeduren, die identisch sind, aber mit der Ausnahme, dass eine Prozedur einen speziellen Abfragehinweis aufweist.

**Beispiel (Teil 1)**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Beispiel (Teil 2)**

(Es ist ratsam, mindestens zehn Minuten zu warten, bevor Sie mit Teil 2 des Beispiels beginnen, damit die Ergebnisse in den resultierenden Telemetriedaten unterschiedlich sind.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

In jedem Teil dieses Beispiels wird versucht, eine parametrisierte Einfügeanweisung 1.000-mal auszuführen (zum Generieren einer ausreichenden Last für einen Testdatensatz). Beim Ausführen von gespeicherten Prozeduren untersucht der Abfrageprozessor den Parameterwert, der während der ersten Kompilierung an die Prozedur übergeben wird („Parameterermittlung“). Der Prozessor speichert den sich ergebenden Plan auch dann zwischen und verwendet ihn für spätere Aufrufe, wenn der Parameterwert anders ist. Der optimale Plan wird unter Umständen nicht immer verwendet. In einigen Fällen müssen Sie den Optimierer zur Auswahl eines Plans leiten, der für den Durchschnittsfall besser geeignet ist, anstatt der spezielle Fall, für den die Abfrage kompiliert wurde. In diesem Beispiel wird vom anfänglichen Plan ein „Scanplan“ generiert. Dieser liest alle Zeilen, um die einzelnen Werte zu ermitteln, die mit dem Parameter übereinstimmen:

![Abfragenoptimierung mit einem Scanplan](./media/sql-database-performance-guidance/query_tuning_1.png)

Da die Prozedur mit dem Wert 1 ausgeführt wurde, war der sich ergebende Plan für Wert 1 optimal, aber für alle anderen Werte der Tabelle suboptimal. Dies ist wahrscheinlich nicht das gewünschte Ergebnis, wenn Sie jeden Plan zufällig auswählen, da der Plan langsamer ist und mehr Ressourcen benötigt.

Wenn Sie den Test mit Festlegung von `SET STATISTICS IO` auf `ON` ausführen, werden die Schritte des logischen Scans in diesem Beispiel im Hintergrund ausgeführt. Sie sehen, dass vom Plan 1.148 Lesevorgänge ausgeführt werden (was nicht effizient ist, wenn im Normalfall nur eine Zeile zurückgegeben werden soll):

![Abfragenoptimierung per logischem Scan](./media/sql-database-performance-guidance/query_tuning_2.png)

Im zweiten Teil des Beispiels wird ein Abfragehinweis verwendet, um den Optimierer anzuweisen, während des Kompilierungsprozesses einen bestimmten Wert zu verwenden. In diesem Fall wird erzwungen, dass der Abfrageprozessor den Wert ignoriert, der als Parameter übergeben wird, und stattdessen `UNKNOWN`annimmt. Bezieht sich auf einen Wert, der in der Tabelle für die durchschnittliche Häufigkeit angegeben ist (Datenschiefe wird ignoriert). Der sich ergebende Plan ist ein suchbasierter Plan, der im Durchschnitt schneller und ressourcenschonender als der Plan aus Teil 1 dieses Beispiels ist:

![Abfragenoptimierung per Abfragehinweis](./media/sql-database-performance-guidance/query_tuning_3.png)

Sie sehen das Ergebnis in der Tabelle **sys.resource_stats** (zwischen der Ausführung des Tests und dem Füllen der Tabelle mit den Daten liegt eine Verzögerung). In diesem Beispiel wurde Teil 1 während des Zeitfensters 22:25:00 und Teil 2 um 22:35:00 ausgeführt. Im früheren Zeitfenster wurden in diesem Zeitraum mehr Ressourcen als im späteren Zeitfenster verwendet (aufgrund von Verbesserungen der Planungseffizienz).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Abfragenoptimierung – Beispielergebnisse](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> Das Volumen ist in diesem Beispiel zwar absichtlich nur sehr klein, aber die Auswirkungen von suboptimalen Parametern können beträchtlich sein, besonders für größere Datenbanken. Der Unterschied kann für die langsame und die schnelle Variante in Extremfällen zwischen dem Sekundenbereich und dem Stundenbereich liegen.
>
>

Sie können **sys.resource_stats** überprüfen, um zu ermitteln, ob die Ressource für einen Test mehr oder weniger Ressourcen als für einen anderen Test verwendet. Beim Vergleichen von Daten sollten Sie Tests zeitlich ausreichend trennen, damit sie sich in der Ansicht **sys.resource_stats** nicht in demselben 5-Minuten-Fenster bewegen. Ziel dieser Übung ist die Minimierung der Gesamtmenge an verwendeten Ressourcen, und nicht die Minimierung der Ressourcen bei Spitzenlast. Im Allgemeinen führt eine Optimierung eines Codeabschnitts in Bezug auf die Latenz auch zu einem verringerten Ressourcenverbrauch. Stellen Sie sicher, dass die an einer Anwendung vorgenommenen Änderungen auch notwendig sind und dass die Änderungen keine negativen Auswirkungen auf die Benutzerfreundlichkeit haben, wenn Benutzer in der Anwendung Abfragehinweise verwenden.

Wenn eine Workload eine Gruppe von sich wiederholenden Abfragen enthält, ist es häufig sinnvoll, den Optimierungsgrad Ihrer Planentscheidungen zu erfassen und zu überprüfen. Dies hat nämlich Auswirkungen auf die Mindestgrößeneinheit für Ressourcen, die zum Hosten der Datenbank erforderlich ist. Untersuchen Sie die Pläne nach der ersten Überprüfung gelegentlich erneut, um sicherzugehen, dass sie nicht veraltet sind. Weitere Informationen finden Sie unter [Abfragehinweise (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Datenbankübergreifendes Sharding
Da Azure SQL-Datenbank auf normaler Hardware ausgeführt wird, gelten für eine Einzeldatenbank niedrigere Kapazitätsgrenzen als für eine herkömmliche lokale SQL Server-Installation. Einige Kunden verwenden das Sharding-Verfahren (also das horizontale Partitionieren), um Datenbankvorgänge auf mehrere Datenbanken zu verteilen, wenn diese nicht in die Grenzen für eine Einzeldatenbank in Azure SQL-Datenbank passen. Die meisten Kunden, die Sharding-Verfahren für Azure SQL-Datenbank verwenden, teilen ihre Daten einer Dimension auf mehrere Datenbanken auf. Bei diesem Ansatz muss verinnerlicht werden, dass von OLTP-Anwendungen häufig Transaktionen durchgeführt werden, die nur für eine Zeile oder eine kleine Gruppe von Zeilen im Schema gelten.

> [!NOTE]
> SQL-Datenbank verfügt jetzt über eine Bibliothek als Hilfe für das Sharding. Weitere Informationen finden Sie unter [Übersicht über die Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md).
>
>

Wenn eine Datenbank beispielsweise Kundennamen, Bestellungen und Bestelldetails enthält (wie in der herkömmlichen Northwind-Beispieldatenbank von SQL Server), können Sie diese Daten auf mehrere Datenbanken aufteilen. Hierzu wird ein Kunde mit den zugehörigen Bestellinformationen und -details gruppiert. Sie können dafür sorgen, dass die Daten des Kunden in einer Einzeldatenbank verbleiben. Bei dieser Anwendung würden unterschiedliche Kunden auf verschiedene Datenbanken verteilt werden, um eine effektive Verteilung der Last auf mehrere Datenbanken zu erreichen. Beim Sharding können Kunden nicht nur die Erreichung der Obergrenze bei der Datenbankgröße vermeiden, sondern für Azure SQL-Datenbank auch die Verarbeitung von Workloads ermöglichen, die die Obergrenzen der unterschiedlichen Leistungsebenen deutlich überschreiten, solange jede einzelne Datenbank in die DTU passt.

Auch wenn beim Datenbanksharding die aggregierte Ressourcenkapazität für eine Lösung nicht reduziert wird, ist dieses Verfahren äußerst effektiv zur Unterstützung sehr großer Lösungen, die auf mehrere Datenbanken verteilt sind. Jede Datenbank kann mit einer anderen Leistungsebene ausgeführt werden, um sehr große, „effektive“ Datenbanken mit hohen Ressourcenanforderungen zu unterstützen.

### <a name="functional-partitioning"></a>Funktionale Partitionierung
Es kommt häufig vor, dass SQL Server-Benutzer viele Funktionen in einer Einzeldatenbank kombinieren. Wenn eine Anwendung beispielsweise Logik zum Verwalten des Bestands für einen Store enthält, kann die Datenbank Logik für die Bereiche Bestand, Nachverfolgung von Bestellungen, gespeicherte Prozeduren und indizierte/materialisierte Sichten zum Verwalten der Berichterstellung für den Monatsabschluss enthalten. Dieses Verfahren vereinfacht die Verwaltung der Datenbank für Vorgänge wie die Sicherung. Es erfordert aber auch, dass Sie die Größe der Hardware so bemessen, dass die Spitzenlast über alle Funktionen einer Anwendung hinweg bewältigt werden kann.

Wenn Sie eine Architektur mit horizontaler Hochskalierung in Azure SQL-Datenbank verwenden, ist es vorteilhaft, die Funktionen einer Anwendung auf unterschiedliche Datenbanken zu verteilen. Mit diesem Verfahren wird jede Anwendung unabhängig von anderen Anwendungen skaliert. Wenn die Auslastung einer Anwendung steigt (und somit auch die Auslastung der Datenbank), kann der Administrator unabhängige Leistungsebenen für jede Funktion in einer Anwendung wählen. Unter Berücksichtigung der Obergrenze kann eine Anwendung bei dieser Architektur eine Größe erreichen, die von einem einzelnen normalen Computer nicht mehr bewältigt werden kann, da die Last auf mehrere Computer verteilt wird.

### <a name="batch-queries"></a>Batch-Abfragen
Für Anwendungen, bei denen mit sehr häufigen Ad-hoc-Abfragen mit hohem Volumen auf Daten zugegriffen wird, wird ein Großteil der Reaktionszeit für die Netzwerkkommunikation zwischen der Anwendungsebene und der Azure SQL-Datenbankebene verbraucht. Auch wenn sowohl die Anwendung als auch Azure SQL-Datenbank in demselben Rechenzentrum angeordnet sind, kann die Netzwerklatenz zwischen den beiden Komponenten durch eine hohe Anzahl von Datenzugriffsvorgängen verstärkt werden. Um die Netzwerkroundtrips für die Datenzugriffsvorgänge zu reduzieren, können Sie die Option zum Zusammenfassen von Ad-hoc-Abfragen in Batches oder zum Kompilieren als gespeicherte Prozeduren verwenden. Wenn Sie Ad-hoc-Abfragen zu einem Batch zusammenfassen, können Sie mehrere Abfragen in einem großen Batch innerhalb eines Vorgangs an Azure SQL-Datenbank senden. Wenn Sie Ad-hoc-Abfragen in einer gespeicherten Prozedur kompilieren, können Sie das gleiche Ergebnis wie beim Zusammenfassen zu Batches erzielen. Ein weiterer Vorteil der Verwendung einer gespeicherten Prozedur ist die Erhöhung der Wahrscheinlichkeit, dass die Abfragepläne in Azure SQL-Datenbank zwischengespeichert werden, sodass Sie die gespeicherte Prozedur erneut verwenden können.

Einige Anwendungen sind mit vielen Schreibvorgängen verbunden. In einigen Fällen ist es auch möglich, die E/A-Gesamtlast einer Datenbank zu reduzieren, indem geprüft wird, wie Schreibvorgänge zu Batches zusammengefasst werden können. Dies ist häufig so einfach wie die Verwendung von expliziten Transaktionen anstelle von Transaktionen mit automatischem Commit innerhalb von gespeicherten Prozeduren und Ad-hoc-Batches. Eine Auswertung unterschiedlicher Verfahren, die verwendet werden können, finden Sie unter [Stapelverarbeitungsverfahren für SQL-Datenbankanwendungen in Azure](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimentieren Sie mit Ihrer eigenen Workload, um das richtige Modell für die Erstellung von Batches zu ermitteln. Achten Sie hierbei darauf, dass ein Modell ggf. etwas andere Konsistenzgarantien in Bezug auf Transaktionen aufweisen kann. Für die Ermittlung der richtigen Workload, bei der die Ressourcenverwendung reduziert wird, ist die richtige Kombination aus Konsistenz und Abstrichen bei der Leistung erforderlich.

### <a name="application-tier-caching"></a>Zwischenspeichern auf Anwendungsebene
Einige Datenbankanwendungen verfügen über Workloads mit einer hohen Zahl von Lesevorgängen. Cachingschichten können dazu beitragen, die Auslastung für die Datenbank zu reduzieren und ggf. die Leistungsebene zu reduzieren, die zum Unterstützen einer Datenbank mit Azure SQL-Datenbank erforderlich ist. Wenn Sie [Azure Redis Cache](https://azure.microsoft.com/services/cache/) verwenden und über Workloads mit vielen Lesevorgängen verfügen, können Sie Daten einmalig lesen (oder je nach Konfiguration ggf. einmal pro Computer auf Anwendungsebene) und dann außerhalb von Azure SQL-Datenbank speichern. Dies ist eine Möglichkeit zur Reduzierung der Datenbanklast (CPU- und Lesevorgang-E/A), aber es kommt zu einer Auswirkung auf die Transaktionskonsistenz, da die aus dem Cache ausgelesenen Daten gegenüber den Daten in der Datenbank ggf. nicht mehr synchron sind. Es gibt zwar viele Anwendungen, bei denen ein gewisser Inkonsistenzgrad akzeptabel ist, aber dies gilt nicht für alle Workloads. Sie sollten sich daher vollständig mit den Anwendungsanforderungen vertraut machen, bevor Sie eine Cachingstrategie auf Anwendungsebene implementieren.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Dienstebenen finden Sie unter [SQL-Datenbankoptionen und -leistung](sql-database-service-tiers.md)
* Weitere Informationen zu elastischen Pools finden Sie unter [Was ist ein elastischer Azure-Pool?](sql-database-elastic-pool.md).
* Informationen zur Leistung und zu elastischen Pools finden Sie unter [Wann ein elastischer Pool in Frage kommt](sql-database-elastic-pool.md).

