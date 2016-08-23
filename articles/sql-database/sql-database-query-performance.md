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
   ms.date="08/09/2016"
   ms.author="sstein"/>

# Query Performance Insight für Azure SQL-Datenbank


Das Verwalten und Abstimmen der Leistung von relationalen Datenbanken ist eine anspruchsvolle Aufgabe, die ein erhebliches Maß an Wissen und Zeit erfordert. Dank Query Performance Insight können Sie den Zeitaufwand für die Problembehandlung der Datenbankleistung reduzieren, weil Folgendes ermöglicht wird:

- Tiefere Einblicke in den Verbrauch von Datenbankressourcen (DTU).
- Informationen zu Abfragen mit der höchsten CPU-Auslastung, für die unter Umständen eine Leistungssteigerung erzielt werden kann.
- Die Fähigkeit zum Durchführen eines Drilldowns in die Details einer Abfrage.​

## Voraussetzungen

- Query Performance Insight ist nur unter Azure SQL-Datenbank V12 verfügbar.
- Query Performance Insight erfordert, dass der [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx) für Ihre Datenbank ausgeführt wird. Wenn der Abfragespeicher nicht ausgeführt wird, werden Sie vom Portal zum Aktivieren aufgefordert.

 
## Berechtigungen

Die folgenden Berechtigungen der [rollenbasierten Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) sind für die Verwendung von Query Performance Insight erforderlich:

- Die Berechtigungen **Leser**, **Besitzer**, **Mitwirkender**, **Mitwirkender von SQL DB** oder **Mitwirkender von SQL Server** sind erforderlich, um die Abfragen und Diagramme mit dem höchsten Ressourcenverbrauch anzuzeigen.
- Die Berechtigungen **Besitzer**, **Mitwirkender**, **Mitwirkender von SQL DB** oder **Mitwirkender von SQL Server** sind zum Anzeigen von Abfragetext erforderlich.



## Verwenden von Query Performance Insight

Query Performance Insight ist einfach zu verwenden:

- Überprüfen Sie die Liste mit den Abfragen, die den höchsten Ressourcenverbrauch aufweisen.
- Wählen Sie eine einzelne Abfrage aus, um die Details anzuzeigen.
- Öffnen Sie den [SQL-Datenbankratgeber](sql-database-advisor.md), und überprüfen Sie, ob Empfehlungen verfügbar sind.
- Vergrößern Sie die Ansicht, um ausführlichere Informationen abzurufen.

    ![Leistungsdashboard](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Einige Stunden mit Daten müssen von Query Store für SQL-Datenbank erfasst werden, um Query Performance Insight-Funktionen bereitstellen zu können. Wenn die Datenbank keine Aktivität aufweist oder Query Store während eines bestimmten Zeitraums nicht aktiv war, sind die Diagramme beim Anzeigen dieses Zeitraums leer. Wenn Query Store nicht ausgeführt wird, können Sie die Anwendung jederzeit aktivieren.



## Überprüfen von Abfragen mit der höchster CPU-Auslastung

Gehen Sie im [Portal](http://portal.azure.com) wie folgt vor:

1. Navigieren Sie zu einer SQL-Datenbank, und klicken Sie auf **Einstellungen** > **Leistung** > **Abfragen**.

    ![Query Performance Insight][1]

    Die Ansicht der Abfragen mit höchstem Verbrauch wird geöffnet, und die Abfragen mit der höchsten CPU-Auslastung werden aufgelistet.

1. Klicken Sie an verschiedenen Stellen auf das Diagramm, um die Details anzuzeigen.<br>In der obersten Zeile wird der Gesamtwert für DTU% angezeigt. Die Balken zeigen die Werte für die CPU%-Auslastung der ausgewählten Abfragen während des ausgewählten Intervalls an (bei Auswahl von **Letzte Woche** steht jeder Balken beispielsweise für einen Tag).

    ![Abfragen mit höchstem Verbrauch][2]

    Das untere Raster zeigt zusammengefasste Informationen für die sichtbaren Abfragen.

    -	Abfrage-ID: eindeutiger Bezeichner der Abfrage in der Datenbank.
    -	CPU-Auslastung pro Abfrage während des beobachteten Intervalls (je nach Aggregationsfunktion).
    -	Dauer pro Abfrage (je nach Aggregationsfunktion).
    -	Gesamtzahl der Ausführungen für eine bestimmte Abfrage.


	Sie können einzelne Abfragen aktivieren oder deaktivieren, um sie in das Diagramm einzubeziehen oder davon auszuschließen.


1. Wenn Ihre Daten veraltet sind, klicken Sie auf die Schaltfläche **Aktualisieren**.
1. Optional können Sie auf **Einstellungen** klicken und anpassen, wie die CPU-Auslastungsdaten angezeigt werden, oder einen anderen Zeitraum auswählen.

    ![settings](./media/sql-database-query-performance/settings.png)

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

Diese Meldungen werden in der Regel angezeigt, wenn der Abfragespeicher keine neuen Daten sammeln kann. Zur Behebung dieser Probleme haben Sie einige Optionen:

-	Ändern der Aufbewahrungs- und Erfassungsrichtlinie des Abfragespeichers
-	Erhöhen der Größe des Abfragespeichers
-	Löschen des Abfragespeichers

### Empfohlene Aufbewahrungs- und Erfassungsrichtlinie

Es gibt zwei Arten von Aufbewahrungsrichtlinien:

- Größenbasiert: Bei Festlegung auf AUTO werden Daten automatisch bereinigt, wenn die maximale Größe fast erreicht ist.
- Zeitbasiert: Wenn für den Abfragespeicher der Platz knapp wird, werden Abfrageinformationen gelöscht, die älter als die Standardeinstellung von 30 Tagen sind.

Die Erfassungsrichtlinie sollte festgelegt werden auf:

- **Alle:** Alle Abfragen werden aufgezeichnet. **Alle** ist die Standardoption.
- **Auto:** Seltene Abfragen und Abfragen mit unbedeutender Erstellungs- und Ausführungsdauer werden ignoriert. Die Schwellenwerte für Ausführungszahl, Erstellungs- und Ausführungsdauer werden intern bestimmt.
- **Keine:** Der Abfragespeicher beendet die Erfassung neuer Abfragen.
	
Sie sollten alle Richtlinien auf AUTO und die Bereinigungsrichtlinie auf 30 Tage festlegen:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
    	
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Erhöhen Sie die Größe des Abfragespeichers, indem Sie eine Verbindung mit einer Datenbank herstellen und die folgende Abfrage ausgeben:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Löschen Sie den Abfragespeicher. Löscht alle aktuellen Informationen im Abfragespeicher:

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## Zusammenfassung

Dank Query Performance Insight können Sie die Auswirkungen der Abfragen-Workload und die Wechselwirkung mit dem Verbrauch von Datenbankressourcen besser verstehen. Mit diesem Feature erhalten Sie Informationen zu den Abfragen mit dem höchsten Verbrauch und können die Abfragen ermitteln, die angepasst werden müssen, bevor sie ein Problem verursachen.




## Nächste Schritte

Um weitere Empfehlungen zur Verbesserung der Leistung Ihrer SQL-Datenbank zu erhalten, klicken Sie auf dem Blatt **Query Performance Insight** auf [Datenbankratgeber](sql-database-advisor.md).

![Leistungsratgeber](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png

<!---HONumber=AcomDC_0810_2016-->