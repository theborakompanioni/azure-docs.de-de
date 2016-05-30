<properties
   pageTitle="Problembehandlung | Microsoft Azure"
   description="Problembehandlung bei SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/15/2016"
   ms.author="mausher;sonyama;barbkess"/>

# Problembehandlung
In diesem Thema sind einige der Probleme aufgeführt, die bei der Verwendung von Azure SQL Data Warehouse häufiger auftreten.

## Konnektivität
Häufigere Konnektivitätsprobleme beinhalten Folgendes:

- Es sind keine Firewallregeln festgelegt.
- Es werden nicht unterstützte Tools oder Protokolle verwendet.

### Firewallregeln
Azure SQL-Datenbanken sind durch Firewalls auf Server- und Datenbankebene geschützt, um sicherzustellen, dass nur bekannte IP-Adressen Zugriff auf eine Datenbank haben. Firewalls sind standardmäßig sicher. Sie müssen daher eine IP-Adresse oder einen Adressbereich explizit aktivieren, bevor Sie eine Verbindung herstellen können. Führen Sie die Schritte im Abschnitt [Erstellen einer neuen Azure SQL-Firewall auf Serverebene][] in den [Bereitstellungsanweisungen][] aus, um Ihre Firewall für den Zugriff zu konfigurieren.

### Nicht unterstützte Tools oder Protokolle
SQL Data Warehouse empfiehlt die Verwendung von [Visual Studio 2013 und 2015][] zum Abfragen der Daten. Für die Clientkonnektivität werden [SQL Server Native Client 10/11 (ODBC)][] empfohlen. SQL Server Management Studio (SSMS) wird noch nicht unterstützt, und obwohl es teilweise funktioniert, arbeitet der Strukturbereich „Objekt-Explorer“ nicht mit SQL Data Warehouse. Die Abfrage funktioniert möglicherweise, nachdem einige Fehlermeldungen ignoriert wurden.

## Abfrageleistung

Es gibt einige einfache Methoden, die Sie im Datenbankentwurf durchführen können, um sicherzustellen, dass Sie die optimale Abfrageleistung von SQL Data Warehouse erhalten. Der Artikel [Überwachen Ihres Workloads mit dynamischen Verwaltungssichten][] ist ein guter Ausgangspunkt, wenn Sie nachvollziehen möchten, wie gut Ihre Abfragen arbeiten. Damit eine Abfrage schneller ausgeführt werden kann, müssen Sie Ihren Abfragen manchmal mehr Computeleistungen hinzufügen, um [SQL Data Warehouse horizontal zu skalieren][]. Lesen Sie den Artikel [Verwalten von Statistiken in SQL Data Warehouse][], um mehr über diese Optimierungsmöglichkeiten zu erfahren.

Im folgenden sind einige der häufigsten Ursachen für Leistungsprobleme bei Abfragen aufgeführt.

### Statistiken

[Statistiken][] für Ihre Tabellen erhalten Informationen zum Wertebereich und zur Häufigkeit von Werten in einer Datenbankspalte oder in einer Spaltenkombination. Das Abfragemodul verwendet diese Statistiken, um die Abfrageausführung zu optimieren und die Abfrageleistung zu verbessern. Im Gegensatz zu SQL Server oder SQL DB bietet SQL Data Warehouse keine automatische Erstellung oder kein Update von Statistiken. Statistiken müssen für alle Tabellen manuell verwaltet werden. Weitere Informationen zum Verwalten von Statistiken und zum Identifizieren von Tabellen, die Statistiken benötigen, finden Sie im Artikel [Verwalten von Statistiken in SQL Data Warehouse][].

### Tabellenentwurf

Eine wichtige Entscheidung in SQL Data Warehouse ist [die Auswahl des richtigen Hashverteilungsschlüssels für die Tabelle][] und der [Tabellenentwurf][]. Sie sollten die folgenden Artikel lesen, wenn Sie nach dem ersten Einblick in SQL Data Warehouse die Performance erhöhen möchten.

### Qualität gruppierter Columnstore-Segmente

Die Qualität gruppierter Columnstore-Segmente ist wichtig für die optimale Abfrageleistung gruppierter Columnstore-Tabellen. Die Segmentqualität kann anhand der Anzahl an Zeilen in einer komprimierten Zeilengruppe gemessen werden. Die folgende Abfrage identifiziert Tabellen mit schlechter Columnstore-Indexsegmentintegrität und generiert das T-SQL zum Neuerstellen des Columnstore-Indexes in diesen Tabellen. Mir der ersten Spalte dieses Abfrageergebnisses erhalten Sie das T-SQL zum Neuerstellen jedes Indexes. Die zweite Spalte bietet eine Empfehlung für die minimale Klasse, die verwendet werden soll, um die Komprimierung zu optimieren.
 
**SCHRITT 1:** Führen Sie diese Abfrage für jede SQL Data Warehouse-Datenbank aus, um alle nicht optimalen gruppierten Columnstore-Indizes zu identifizieren. Wenn keine Zeilen zurückgegeben werden, hatte diese Regression keine Auswirkung auf Sie, und keine weitere Aktion ist erforderlich.

```sql
SELECT 
     'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;' AS [T-SQL to Rebuild Index]
    ,CASE WHEN n.nbr_nodes < 3 THEN 'xlargerc' WHEN n.nbr_nodes BETWEEN 4 AND 6 THEN 'largerc' ELSE 'mediumrc' END AS [Resource Class Recommendation]
    ,s.name AS [Schema Name]
    ,t.name AS [Table Name]
    ,AVG(CASE WHEN rg.State = 3 THEN rg.Total_rows ELSE NULL END) AS [Ave Rows in Compressed Row Groups]
FROM 
    sys.pdw_nodes_column_store_row_groups rg
    JOIN sys.pdw_nodes_tables pt 
        ON rg.object_id = pt.object_id AND rg.pdw_node_id = pt.pdw_node_id AND pt.distribution_id = rg.distribution_id
    JOIN sys.pdw_table_mappings tm 
        ON pt.name = tm.physical_name
    INNER JOIN sys.tables t 
        ON tm.object_id = t.object_id
INNER JOIN sys.schemas s
    ON t.schema_id = s.schema_id
CROSS JOIN (SELECT COUNT(*) nbr_nodes  FROM sys.dm_pdw_nodes WHERE type = 'compute') n
GROUP BY 
    n.nbr_nodes, s.name, t.name
HAVING 
    AVG(CASE WHEN rg.State = 3 THEN rg.Total_rows ELSE NULL END) < 100000 OR
    AVG(CASE WHEN rg.State = 0 THEN rg.Total_rows ELSE NULL END) < 100000

ORDER BY 
    s.name, t.name
```
 
**SCHRITT 2:** Erhöhen Sie die Ressourcenklasse eines Benutzers mit Berechtigungen zum Neuerstellen des Indexes für diese Tabelle auf die empfohlene Ressourcenklasse aus der 2. Spalte der obigen Abfrage.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

> [AZURE.NOTE]  Der obige „LoadUser“ sollte ein gültiger Benutzer sein, den Sie zum Ausführen der ALTER INDEX-Anweisung erstellen. Die Ressourcenklasse des Benutzers „db\_owner“ kann nicht geändert werden. Weitere Informationen zu Ressourcenklassen und zum Erstellen eines neuen Benutzers finden Sie in dem unten stehenden Link.

 
**SCHRITT 3:** Melden Sie sich als der Benutzer aus Schritt 2 (z.B. „LoadUser“) an, der nun eine höhere Ressourcenklasse verwendet, und Ausführen der ALTER INDEX-Anweisungen, die von der Abfrage in Schritt 1 generiert wurden. Achten Sie darauf, dass dieser Benutzer die ALTER-Berechtigung für die Tabellen hat, die in der Abfrage aus Schritt 1 identifiziert wurden.
 
**SCHRITT 4:** Führen Sie die Abfrage aus Schritt 1 erneut aus. Wenn die Indizes effizient erstellt wurden, sollten keine Zeilen von dieser Abfrage zurückgegeben. Wenn keine Zeilen zurückgegeben werden, sind Sie fertig. Wenn Sie mehrere SQL Data Warehouse-Datenbanken haben, möchten Sie diesen Vorgang sicher für jede Ihrer Datenbanken wiederholen. Wenn Zeilen zurückgegeben werden, fahren Sie mit Schritt 5 fort.
 
**Schritt 5:** Wenn Zeilen zurückgegeben werden, wenn Sie die Abfrage aus Schritt 1 erneut ausführen, haben Sie möglicherweise Tabellen mit besonders breiten Zeilen, die große Speichermengen zur optimalen Erstellung der gruppierten Columnstore-Indizes benötigen. Wenn dies der Fall ist, wiederholen Sie diesen Prozess für diese Tabelle mithilfe der xlargerc-Klasse. Um die Ressourcenklasse zu ändern, wiederholen Sie Schritt 2 mit „xlargerc“. Wiederholen Sie dann Schritt 3 für die Tabellen, deren Indizes immer noch nicht optimal sind. Wenn Sie DW100 - DW300 verwenden und bereits „xlargerc“ verwendet haben, können Sie wahlweise entweder die Indizes beibehalten oder vorübergehend die DWUs erhöhen, um mehr Arbeitsspeicher für diesen Vorgang bereitzustellen.
 
**ABSCHLIESSENDE SCHRITTE:** Die oben angegebene Ressourcenklasse ist die empfohlene minimale Ressourcenklasse zum Erstellen von Columnstore-Indizes in höchster Qualität. Sie sollten diese Einstellung für den Benutzer beibehalten, der die Daten lädt. Aber wenn Sie die Änderung aus Schritt 2 rückgängig machen möchten, verwenden Sie den folgenden Befehl.

```sql
EXEC sp_droprolemember 'smallrc', 'LoadUser'
```

Als Richtwert zur Festlegung der minimalen Ressourcenklasse für das Laden in eine CCI-Tabelle gilt die Verwendung von „xlargerc“ für DW100 bis DW300, „largerc“ für DW400 bis DW600 und „mediumrc“ für alles ab DW1000. Dieser Leitfaden ist für die meisten Workloads ideal. Das Ziel ist, jedem Indexerstellungsvorgang 400 MB oder mehr Arbeitsspeicher zu gewähren. Allerdings wird eine Größe nicht allen gerecht. Der zum Optimieren eines Columnstore-Indexes benötigte Speicherplatz ist von den geladenen Daten abhängig, die in erster Linie von der Zeilengröße beeinflusst werden. Tabellen mit geringen Zeilenbreiten benötigen weniger Arbeitsspeicher, breitere Zeilen benötigen mehr. Wenn Sie experimentieren möchten, können Sie die Abfrage aus Schritt 1 verwenden, um zu sehen, ob Sie optimalere Columnstore-Indizes bei kleineren Speicherbelegungen erhalten. Minimal wünschen Sie durchschnittlich mehr als 100.000 Zeilen pro Zeilengruppe. Über 500K ist noch besser. Maximal sehen Sie 1 Million Zeilen pro Zeilengruppe. Ausführliche Informationen zum Verwalten von Ressourcenklassen und Parallelität erhalten Sie über den unten stehenden Link.


## Nächste Schritte
Weitere Informationen zur Optimierung Ihrer SQL Data Warehouse-Lösung finden Sie im Artikel [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse horizontal zu skalieren]: ./sql-data-warehouse-overview-scalability.md
[Tabellenentwurf]: ./sql-data-warehouse-develop-table-design.md
[die Auswahl des richtigen Hashverteilungsschlüssels für die Tabelle]: ./sql-data-warehouse-develop-hash-distribution-key
[development overview]: ./sql-data-warehouse-overview-develop.md
[Überwachen Ihres Workloads mit dynamischen Verwaltungssichten]: ./sql-data-warehouse-manage-monitor.md
[Verwalten von Statistiken in SQL Data Warehouse]: ./sql-data-warehouse-develop-statistics.md
[Bereitstellungsanweisungen]: ./sql-data-warehouse-get-started-provision.md
[Erstellen einer neuen Azure SQL-Firewall auf Serverebene]: ./sql-data-warehouse-get-started-provision.md/#create-a-new-azure-sql-server-level-firewall
[Visual Studio 2013 und 2015]: ./sql-data-warehouse-get-started-connect.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Verwalten von Statistiken in SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Statistiken]: ./sql-data-warehouse-develop-statistics.md

<!--MSDN references-->
[SQL Server Native Client 10/11 (ODBC)]: https://msdn.microsoft.com/library/ms131415.aspx

<!--Other web references-->

<!---HONumber=AcomDC_0518_2016-->