<properties 
   pageTitle="Query Performance Insight für Azure SQL-Datenbank" 
   description="Mit der Überwachung der Abfrageleistung werden für eine Azure SQL-Datenbank die Abfragen mit der höchsten CPU-Auslastung identifiziert." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="04/28/2016"
   ms.author="sstein"/>

# Query Performance Insight für Azure SQL-Datenbank


Das Verwalten und Abstimmen der Leistung von relationalen Datenbanken ist eine anspruchsvolle Aufgabe, die ein erhebliches Maß an Wissen und Zeit erfordert. Dank Query Performance Insight können Sie den Zeitaufwand für die Problembehandlung der Datenbankleistung reduzieren, weil Folgendes ermöglicht wird:

- Tiefere Einblicke in den Verbrauch von Datenbankressourcen (DTU). 
- Informationen zu Abfragen mit der höchsten CPU-Auslastung, für die unter Umständen eine Leistungssteigerung erzielt werden kann. 
- Die Fähigkeit zum Durchführen eines Drilldowns in die Details einer Abfrage.​

## Voraussetzungen

- Query Performance Insight ist nur unter Azure SQL-Datenbank V12 verfügbar.
- Query Performance Insight erfordert, dass der [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx) für Ihre Datenbank ausgeführt wird. Das Portal fordert Sie auf, den Abfragespeicher zu aktivieren, falls noch nicht erfolgt.

 
## Berechtigungen

Die folgenden Berechtigungen der [rollenbasierten Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) sind für die Verwendung von Query Performance Insight erforderlich:

- Die Berechtigungen **Leser**, **Besitzer**, **Mitwirkender**, **Mitwirkender von SQL DB** oder **Mitwirkender von SQL Server** sind erforderlich, um die Abfragen und Diagramme mit dem höchsten Ressourcenverbrauch anzuzeigen. 
- Die Berechtigungen **Besitzer**, **Mitwirkender**, **Mitwirkender von SQL DB** oder **Mitwirkender von SQL Server** sind zum Anzeigen von Abfragetext erforderlich.



## Verwenden von Query Performance Insight

Query Performance Insight ist einfach zu verwenden:

- Überprüfen Sie die Liste mit den Abfragen, die den höchsten Ressourcenverbrauch aufweisen. 
- Wählen Sie eine einzelne Abfrage aus, um die Details anzuzeigen.
- Öffnen Sie den [Leistungsratgeber](sql-database-index-advisor.md), und überprüfen Sie, ob Empfehlungen verfügbar sind.
- Vergrößern Sie die Ansicht, um ausführlichere Informationen abzurufen.
- 
    ![Leistungsdashboard](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Einige Stunden mit Daten müssen von Query Store für SQL-Datenbank erfasst werden, um Query Performance Insight-Funktionen bereitstellen zu können. Wenn die Datenbank keine Aktivität aufweist oder Query Store während eines bestimmten Zeitraums nicht aktiv war, sind die Diagramme beim Anzeigen dieses Zeitraums leer. Sie können Query Store jederzeit aktivieren, wenn die Anwendung nicht ausgeführt wird.



## Überprüfen von Abfragen mit der höchster CPU-Auslastung

Gehen Sie im [Portal](http://portal.azure.com) wie folgt vor:

1. Navigieren Sie zu einer SQL-Datenbank, und klicken Sie auf **Alle Einstellungen** > **Leistung** > **Abfragen**. 

    ![Query Performance Insight][1]

    Die Ansicht der Abfragen mit höchstem Verbrauch wird geöffnet, und die Abfragen mit der höchsten CPU-Auslastung werden aufgelistet.

1. Klicken Sie an verschiedenen Stellen auf das Diagramm, um die Details anzuzeigen.<br>In der obersten Zeile wird der Gesamtwert für DTU% für die Datenbank angezeigt, und die Balken zeigen die Werte für die CPU%-Auslastung der ausgewählten Abfragen während des ausgewählten Intervalls an (wenn z.B. **Letzte Woche** ausgewählt ist, entspricht jeder ausgewählte Balken 1 Tag).

    ![Abfragen mit höchstem Verbrauch][2]

    Das untere Raster zeigt zusammengefasste Informationen für die sichtbaren Abfragen.

    -	Durchschnittliche CPU-Nutzung pro Abfrage während des beobachteten Intervalls. 
    -	Gesamtdauer pro Abfrage.
    -	Gesamtzahl der Ausführungen für eine bestimmte Abfrage.


	Sie können einzelne Abfragen aktivieren oder deaktivieren, um sie in das Diagramm einzubeziehen oder davon auszuschließen.


1. Wenn Ihre Daten veraltet sind, klicken Sie auf die Schaltfläche **Aktualisieren**.
1. Optional können Sie auf **Einstellungen** klicken und anpassen, wie die CPU-Auslastungsdaten angezeigt werden, oder einen anderen Zeitraum auswählen.

    ![Einstellungen](./media/sql-database-query-performance/settings.png)

## Anzeigen von Details einzelner Abfragen

So zeigen Sie die Details von Abfragen an

1. Klicken Sie in der Liste mit den Abfragen mit der höchsten Auslastung auf eine beliebige Abfrage.

    ![Details](./media/sql-database-query-performance/details.png)

4. Die Detailansicht wird geöffnet, und die CPU-Auslastung der Abfragen wird in Abhängigkeit von der Zeit aufgeführt.
3. Klicken Sie an verschiedenen Stellen auf das Diagramm, um die Details anzuzeigen.<br>In der obersten Zeile wird der Gesamtwert für DTU% angezeigt, und die Balken zeigen die Werte für die CPU%-Auslastung der ausgewählten Abfragen an.
4. Überprüfen Sie die Daten, um ausführliche Metriken wie Dauer, Anzahl der Ausführungen und Ressourcennutzung in Prozent für jedes Intervall anzuzeigen, in dem die Abfrage ausgeführt wurde.
    
    ![Abfragedetails][3]

1. Optional können Sie auf **Einstellungen** klicken und anpassen, wie die CPU-Auslastungsdaten angezeigt werden, oder einen anderen Zeitraum auswählen.


## 	Optimierung der Abfragespeicherkonfiguration für Query Performance Insight

Während der Verwendung von Query Performance Insight können die folgenden Abfragespeichermeldungen angezeigt werden:

- „Der Abfragespeicher hat seine Kapazität erreicht und sammelt keine neuen Daten.“
- „Der Abfragespeicher für diese Datenbank ist im schreibgeschützten Modus und sammelt keine Performance Insight-Daten.“
- „Die Abfragespeicherparameter für Query Performance Insight sind nicht optimal festgelegt.“

Diese Meldungen werden in der Regel angezeigt, wenn der Abfragespeicher keine neuen Daten sammeln kann. Zum Beheben dieses Problems stehen Ihnen mehrere Optionen zur Verfügung:

-	Ändern der Aufbewahrungs- und Erfassungsrichtlinie des Abfragespeichers
-	Erhöhen der Größe des Abfragespeichers 
-	Löschen des Abfragespeichers

### Empfohlene Aufbewahrungs- und Erfassungsrichtlinie

Es gibt zwei Arten von Aufbewahrungsrichtlinien:

- Größenbasiert – bei Festlegung auf AUTO werden Daten automatisch bereinigt, wenn die maximale Größe fast erreicht ist.
- Zeitbasiert – standardmäßig legen wir ein Limit von 30 Tagen fest, d. h., wenn der Speicherplatz im Abfragespeicher zur Neige geht, werden Abfragedaten gelöscht, die älter als 30 Tage sind. 

Die Erfassungsrichtlinie sollte festgelegt werden auf:

- **Alle**: Alle Abfragen werden aufgezeichnet. Dies ist die Standardoption.
- **Auto**: Seltene Abfragen und Abfragen mit unbedeutender Erstellungs- und Ausführungsdauer werden ignoriert. Die Schwellenwerte für Ausführungszahl, Erstellungs- und Ausführungsdauer werden intern bestimmt.
- **Keine**: Der Abfragespeicher beendet die Erfassung neuer Abfragen.
	
Sie sollten alle Richtlinien auf AUTO setzen und die Bereinigungsrichtlinie auf 30 Tage:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
    	
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Erhöhen Sie die Größe des Abfragespeichers. Hierzu könnten Sie eine Verbindung mit einer Datenbank herstellen und folgende Abfrage ausführen:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Löschen Sie den Abfragespeicher. Beachten Sie, dass dadurch alle aktuellen Informationen im Abfragespeicher gelöscht werden:

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## Zusammenfassung

Dank Query Performance Insight können Sie die Auswirkungen der Abfragen-Workload und die Wechselwirkung mit dem Verbrauch von Datenbankressourcen besser verstehen. Mit diesem Feature erhalten Sie Informationen zu den Abfragen mit dem höchsten Verbrauch und können die Abfragen ermitteln, die angepasst werden müssen, bevor sie ein Problem verursachen. Klicken Sie in einer Datenbank auf **Query Performance Insight**, um die Abfragen mit der höchsten Ressourcenauslastung (CPU) anzuzeigen.




## Nächste Schritte

Um weitere Empfehlungen zur Verbesserung der Leistung Ihrer SQL-Datenbank zu erhalten, klicken Sie auf dem Blatt **Query Performance Insight** auf [Leistungsratgeber](sql-database-index-advisor.md).

![Leistungsratgeber](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png

<!---HONumber=AcomDC_0504_2016-->