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
   ms.date="04/20/2016"
   ms.author="mausher;sonyama;barbkess"/>

# Problembehandlung
In diesem Thema sind einige der Probleme aufgeführt, die bei der Verwendung von Azure SQL Data Warehouse häufiger auftreten.

## Konnektivität
Die Herstellung einer Verbindung mit Azure SQL Data Warehouse kann aus mehreren Gründen fehlschlagen:

- Es sind keine Firewallregeln festgelegt.
- Es werden nicht unterstützte Tools oder Protokolle verwendet.

### Firewallregeln
Azure SQL-Datenbanken sind durch Firewalls auf Server- und Datenbankebene geschützt, um sicherzustellen, dass nur bekannte IP-Adressen Zugriff auf Datenbanken haben. Die Firewalls sind standardmäßig sicher. Das bedeutet, dass Sie den Zugriff für Ihre IP-Adresse konfigurieren müssen, damit Sie eine Verbindung herstellen können.

Um Ihre Firewall für den Zugriff zu konfigurieren, führen Sie die im Abschnitt [Konfigurieren des Serverfirewall-Zugriffs für Ihre Client-IP-Adresse][] auf der Seite [Erstellen eines SQL Data Warehouse][] beschriebenen Schritte aus.

### Nicht unterstützte Tools oder Protokolle
SQL Data Warehouse unterstützt [Visual Studio 2013/2015][] als Entwicklungsumgebungen und [SQL Server Native Client 10/11 (ODBC)][] für Clientverbindungen.

Weitere Informationen finden Sie auf der Seite [Verbinden][].

## Abfrageleistung

### Statistiken

[Statistiken][] sind Objekte, die Informationen zum Wertebereich und zur Häufigkeit von Werten in einer Datenbankspalte enthalten. Das Abfragemodul verwendet diese Statistiken, um die Abfrageausführung zu optimieren und die Abfrageleistung zu verbessern. Im Gegensatz zu SQL Server oder SQL DB ermöglicht SQL Data Warehouse keine automatische Erstellung oder automatische Aktualisierung von Statistiken. Statistiken müssen für alle Tabellen manuell verwaltet werden.

Sie können die folgende Abfrage verwenden, um den Zeitpunkt zu ermitteln, zu dem die Statistiken für alle Tabellen zuletzt aktualisiert wurden.

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

### Qualität gruppierter Columnstore-Segmente

Die Qualität gruppierter Columnstore-Segmente ist wichtig für die optimale Abfrageleistung gruppierter Columnstore-Tabellen. Die Segmentqualität kann anhand der Anzahl an Zeilen in einer komprimierten Zeilengruppe gemessen werden. Die folgende Abfrage identifiziert Tabellen mit schlechter Columnstore-Indexsegmentintegrität und generiert das T-SQL zum Neuerstellen des Columnstore-Indexes in diesen Tabellen. Mir der ersten Spalte dieses Abfrageergebnisses erhalten Sie das T-SQL zum Neuerstellen jedes Indexes. Die zweite Spalte bietet eine Empfehlung für die minimale Klasse, die verwendet werden soll, um die Komprimierung zu optimieren.
 
**Schritt 1:** Führen Sie diese Abfrage für jede SQL Data Warehouse-Datenbank aus, um alle nicht optimalen gruppierten Columnstore-Indizes zu identifizieren. Wenn keine Zeilen zurückgegeben werden, hatte diese Regression keine Auswirkung auf Sie, und keine weitere Aktion ist erforderlich.

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
    AVG(CASE WHEN rg.State = 3 THEN rg.Total_rows ELSE NULL END) < 100000
ORDER BY 
    s.name, t.name
```
 
**Schritt 2:** Erhöhen Sie die Ressourcenklasse eines Benutzers mit Berechtigungen zum Neuerstellen des Indexes für diese Tabelle auf die empfohlene Ressourcenklasse aus der 2. Spalte der obigen Abfrage.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

> [AZURE.NOTE]  Der obige „LoadUser“ sollte ein gültiger Benutzer sein, den Sie zum Ausführen der ALTER INDEX-Anweisung erstellen. Die Ressourcenklasse des Benutzers „db\_owner“ kann nicht geändert werden. Weitere Informationen zu Ressourcenklassen und zum Erstellen eines neuen Benutzers finden Sie in dem unten stehenden Link.

 
**Schritt 3:** Anmeldung als Benutzer aus Schritt 2 (z.B. „LoadUser“), der nun eine höhere Ressourcenklasse verwendet, und Ausführen der ALTER INDEX-Anweisungen, die von der Abfrage in Schritt 1 generiert wurden. Achten Sie darauf, dass dieser Benutzer die ALTER-Berechtigung für die Tabellen hat, die in der Abfrage aus Schritt 1 identifiziert wurden.
 
**Schritt 4:** Führen Sie die Abfrage aus Schritt 1 erneut aus. Wenn die Indizes effizient erstellt wurden, sollten keine Zeilen von dieser Abfrage zurückgegeben. Wenn keine Zeilen zurückgegeben werden, sind Sie fertig. Wenn Sie mehrere SQL Data Warehouse-Datenbanken haben, möchten Sie diesen Vorgang sicher für jede Ihrer Datenbanken wiederholen. Wenn Zeilen zurückgegeben werden, fahren Sie mit Schritt 5 fort.
 
**Schritt 5:** Wenn Zeilen zurückgegeben werden, wenn Sie die Abfrage aus Schritt 1 erneut ausführen, haben Sie möglicherweise Tabellen mit besonders breiten Zeilen, die große Speichermengen zur optimalen Erstellung der gruppierten Columnstore-Indizes benötigen. Wenn dies der Fall ist, wiederholen Sie diesen Prozess für diese Tabelle mithilfe der xlargerc-Klasse. Um die Ressourcenklasse zu ändern, wiederholen Sie Schritt 2 mit „xlargerc“. Wiederholen Sie dann Schritt 3 für die Tabellen, deren Indizes immer noch nicht optimal sind. Wenn Sie DW100 - DW300 verwenden und bereits „xlargerc“ verwendet haben, können Sie wahlweise entweder die Indizes beibehalten oder vorübergehend die DWUs erhöhen, um mehr Arbeitsspeicher für diesen Vorgang bereitzustellen.
 
**ABSCHLIEßENDE SCHRITTE:** Die oben angegebene Ressourcenklasse ist die empfohlene minimale Ressourcenklasse zum Erstellen von Columnstore-Indizes in höchster Qualität. Sie sollten diese Einstellung für den Benutzer beibehalten, der die Daten lädt. Aber wenn Sie die Änderung aus Schritt 2 rückgängig machen möchten, verwenden Sie den folgenden Befehl.

```sql
EXEC sp_droprolemember 'smallrc', 'LoadUser'
```


Der Leitfaden für die minimale Ressourcenklasse zum Laden in eine CCI-Tabelle ist die Verwendung von „xlargerc“ für DW100-DW300, „largerc“ für DW400-DW600 und „mediumrc“ für alles ab DW1000. Dieser Leitfaden ist für die meisten Workloads ideal. Das Ziel ist, jedem Indexerstellungsvorgang 400 MB oder mehr Arbeitsspeicher zu gewähren. Allerdings wird eine Größe nicht allen gerecht. Der zum Optimieren eines Columnstore-Indexes benötigte Speicherplatz ist von den geladenen Daten abhängig, die in erster Linie von der Zeilengröße beeinflusst werden. Tabellen mit geringen Zeilenbreiten benötigen weniger Arbeitsspeicher, breitere Zeilen benötigen mehr. Wenn Sie experimentieren möchten, können Sie die Abfrage aus Schritt 1 verwenden, um zu sehen, ob Sie optimalere Columnstore-Indizes bei kleineren Speicherbelegungen erhalten. Minimal wünschen Sie durchschnittlich mehr als 100.000 Zeilen pro Zeilengruppe. Über 500K ist noch besser. Maximal sehen Sie 1 Million Zeilen pro Zeilengruppe. Ausführliche Informationen zum Verwalten von Ressourcenklassen und Parallelität erhalten Sie über den unten stehenden Link.


### Zentrale Leistungskonzepte

Informationen zu zusätzlichen zentralen Leistung- und Skalierbarkeitskonzepten finden Sie in den folgenden Artikeln:

- [Leistung und Skalierbarkeit][]
- [Parallelitätsmodell][]
- [Entwerfen von Tabellen][]
- [Auswählen eines Hashverteilungsschlüssels für die Tabelle][]

## Nächste Schritte
Anleitungen zum Erstellen Ihrer SQL Data Warehouse-Lösung finden Sie im Artikel [Entwicklungsübersicht][].

<!--Image references-->

<!--Article references-->
[Leistung und Skalierbarkeit]: sql-data-warehouse-performance-scale.md
[Parallelitätsmodell]: sql-data-warehouse-develop-concurrency.md
[Entwerfen von Tabellen]: sql-data-warehouse-develop-table-design.md
[Auswählen eines Hashverteilungsschlüssels für die Tabelle]: sql-data-warehouse-develop-hash-distribution-key
[Entwicklungsübersicht]: sql-data-warehouse-overview-develop.md
[Erstellen eines SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Konfigurieren des Serverfirewall-Zugriffs für Ihre Client-IP-Adresse]: sql-data-warehouse-get-started-provision.md/#step-4-configure-server-firewall-access-for-your-client-ip
[Visual Studio 2013/2015]: sql-data-warehouse-get-started-connect.md
[Verbinden]: sql-data-warehouse-get-started-connect.md
[Statistiken]: sql-data-warehouse-develop-statistics.md

<!--MSDN references-->
[SQL Server Native Client 10/11 (ODBC)]: https://msdn.microsoft.com/library/ms131415.aspx

<!--Other web references-->

<!---HONumber=AcomDC_0427_2016-->