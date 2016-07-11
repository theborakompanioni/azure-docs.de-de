<properties
   pageTitle="Problembehandlung bei Azure SQL Data Warehouse | Microsoft Azure"
   description="Problembehandlung bei Azure SQL Data Warehouse."
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
   ms.date="06/28/2016"
   ms.author="sonyama;barbkess"/>

# Problembehandlung bei Azure SQL Data Warehouse
In diesem Thema sind einige der Probleme aufgeführt, die bei der Verwendung von Azure SQL Data Warehouse ggf. auftreten.


##Verbindungsfehler

Nachfolgend finden Sie einige von Kunden häufiger gemeldete Probleme beim Herstellen von Verbindungen sowie passende Lösungsvorschläge.

### CTAIP-Fehler
Dieser Fehler kann auftreten, wenn zwar eine Anmeldung für die SQL Server-Masterdatenbank erstellt wurde, dies jedoch in der SQL Data Warehouse-Datenbank unterlassen wurde. Wenn dieser Fehler auftritt, lesen Sie den [Übersichtsartikel zur Sicherheit][]. In diesem Artikel wird erläutert, wie Sie zunächst eine Anmeldung für die Masterdatenbank erstellen und anschließend in der SQL Data Warehouse-Datenbank einen Benutzer erstellen.

### Firewallregeln
Azure SQL-Datenbanken sind durch Firewalls auf Server- und Datenbankebene geschützt, um sicherzustellen, dass nur bekannte IP-Adressen Zugriff auf eine Datenbank haben. Firewalls sind standardmäßig sicher. Sie müssen daher eine IP-Adresse oder einen Adressbereich explizit aktivieren, bevor Sie eine Verbindung herstellen können. Führen Sie die Schritte im Abschnitt [Erstellen einer neuen Azure SQL-Firewall auf Serverebene][] in den [Bereitstellungsanweisungen][] aus, um Ihre Firewall für den Zugriff zu konfigurieren.

### Nicht unterstützte Tools oder Protokolle
Für SQL Data Warehouse wird empfohlen, [Visual Studio 2013 oder 2015][] zum Abfragen von Daten zu verwenden. Für die Clientkonnektivität werden [SQL Server Native Client 10/11 (ODBC)][] empfohlen. SQL Server Management Studio (SSMS) wird noch nicht unterstützt, und obwohl es teilweise funktioniert, arbeitet der Strukturbereich „Objekt-Explorer“ nicht mit SQL Data Warehouse. Die Abfrage funktioniert möglicherweise, nachdem einige Fehlermeldungen ignoriert wurden.


## Leistungsprobleme

Wenn Sie die Leistung bei Ihren Abfragen verbessern möchten, ist der Artikel [Bewährte Methoden für SQL Data Warehouse][] ein idealer Ausgangspunkt. Zum Beheben von Problemen bei einer bestimmten Abfrage empfiehlt es sich, den Artikel zum [Überwachen von Abfragen][] zu lesen. Mitunter besteht der Weg zum Beschleunigen von Abfragen darin, mehr Computeleistung hinzufügen, indem Sie Ihr [SQL Data Warehouse skalieren][].

## Qualität gruppierter Columnstore-Segmente

Die Qualität gruppierter Columnstore-Segmente ist wichtig für die optimale Abfrageleistung gruppierter Columnstore-Tabellen. Die Segmentqualität kann anhand der Anzahl an Zeilen in einer komprimierten Zeilengruppe gemessen werden. Die folgende Abfrage identifiziert Tabellen mit schlechter Columnstore-Indexsegmentintegrität und generiert das T-SQL zum Neuerstellen des Columnstore-Indexes in diesen Tabellen. Mir der ersten Spalte dieses Abfrageergebnisses erhalten Sie das T-SQL zum Neuerstellen jedes Indexes. Die zweite Spalte bietet eine Empfehlung für die minimale Klasse, die verwendet werden soll, um die Komprimierung zu optimieren.
 
**SCHRITT 1:** Führen Sie diese Abfrage für jede SQL Data Warehouse-Datenbank aus, um alle nicht optimalen gruppierten Columnstore-Indizes zu identifizieren. Wenn keine Zeilen zurückgegeben werden, ist keine weitere Aktion erforderlich.

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
 
**SCHRITT 2:** Erhöhen Sie die Ressourcenklasse eines Benutzers, der über Berechtigungen zum Neuerstellen des Indexes für diese Tabelle verfügt, auf die empfohlene Ressourcenklasse aus der 2. Spalte der obigen Abfrage.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

> [AZURE.NOTE]  Der obige „LoadUser“ sollte ein gültiger Benutzer sein, den Sie zum Ausführen der ALTER INDEX-Anweisung erstellen. Die Ressourcenklasse des Benutzers „db\_owner“ kann nicht geändert werden. Weitere Informationen zu Ressourcenklassen und zum Erstellen eines neuen Benutzers finden Sie in dem unten stehenden Link.

 
**SCHRITT 3:** Melden Sie sich als der Benutzer aus Schritt 2 (z.B. „LoadUser“) an, der nun eine höhere Ressourcenklasse verwendet, und führen Sie die ALTER INDEX-Anweisungen aus, die von der Abfrage in Schritt 1 generiert wurden. Achten Sie darauf, dass dieser Benutzer die ALTER-Berechtigung für die Tabellen hat, die in der Abfrage aus Schritt 1 identifiziert wurden.
 
**SCHRITT 4:** Führen Sie die Abfrage aus Schritt 1 erneut aus. Wenn die Indizes effizient erstellt wurden, sollten keine Zeilen von dieser Abfrage zurückgegeben. Wenn keine Zeilen zurückgegeben werden, sind Sie fertig. Wenn Sie mehrere SQL Data Warehouse-Datenbanken haben, möchten Sie diesen Vorgang sicher für jede Ihrer Datenbanken wiederholen. Wenn Zeilen zurückgegeben werden, fahren Sie mit Schritt 5 fort.
 
**Schritt 5:** Wenn beim erneuten Ausführen der Abfrage aus Schritt 1 Zeilen zurückgegeben werden, enthalten Ihre Tabellen möglicherweise besonders breite Zeilen. Für diese werden große Mengen an Speicher benötigt, damit die gruppierten Columnstore-Indizes optimal erstellt werden können. Wenn dies der Fall ist, wiederholen Sie diesen Prozess für diese Tabelle mithilfe der xlargerc-Klasse. Um die Ressourcenklasse zu ändern, wiederholen Sie Schritt 2 mit „xlargerc“. Wiederholen Sie dann Schritt 3 für die Tabellen, deren Indizes immer noch nicht optimal sind. Wenn Sie DW100 - DW300 verwenden und bereits „xlargerc“ verwendet haben, können Sie wahlweise entweder die Indizes beibehalten oder vorübergehend die DWUs erhöhen, um mehr Arbeitsspeicher für diesen Vorgang bereitzustellen.
 
**ABSCHLIESSENDE SCHRITTE:** Die oben angegebene Ressourcenklasse ist die empfohlene minimale Ressourcenklasse zum Erstellen von Columnstore-Indizes in höchster Qualität. Sie sollten diese Einstellung für den Benutzer beibehalten, der die Daten lädt. Aber wenn Sie die Änderung aus Schritt 2 rückgängig machen möchten, verwenden Sie den folgenden Befehl.

```sql
EXEC sp_droprolemember 'smallrc', 'LoadUser'
```

Als Richtwert zur Festlegung der minimalen Ressourcenklasse für das Laden in eine CCI-Tabelle gilt die Verwendung von „xlargerc“ für DW100 bis DW300, „largerc“ für DW400 bis DW600 und „mediumrc“ für alles ab DW1000. Dieser Leitfaden ist für die meisten Workloads ideal. Das Ziel ist, jedem Indexerstellungsvorgang 400 MB oder mehr Arbeitsspeicher zu gewähren. Allerdings wird eine Größe nicht allen gerecht. Der zum Optimieren eines Columnstore-Indexes benötigte Speicherplatz ist von den geladenen Daten abhängig, die in erster Linie von der Zeilengröße beeinflusst werden. Tabellen mit geringen Zeilenbreiten benötigen weniger Arbeitsspeicher, breitere Zeilen benötigen mehr. Wenn Sie experimentieren möchten, können Sie die Abfrage aus Schritt 1 verwenden, um zu sehen, ob Sie optimalere Columnstore-Indizes bei kleineren Speicherbelegungen erhalten. Minimal wünschen Sie durchschnittlich mehr als 100.000 Zeilen pro Zeilengruppe. Über 500K ist noch besser. Maximal sehen Sie 1 Million Zeilen pro Zeilengruppe. Ausführliche Informationen zum Verwalten von Ressourcenklassen und Parallelität erhalten Sie über den unten stehenden Link.


## Nächste Schritte

Wenn Sie bisher keine Lösung für Ihr Problem gefunden haben, können Sie folgende Ressourcen nutzen:

- [Blogs]
- [Funktionsanfragen]
- [Videos]
- [CAT Team-Blogs]
- [Erstellen eines Supporttickets]
- [MSDN-Forum]
- [Stack Overflow-Forum]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Übersichtsartikel zur Sicherheit]: ./sql-data-warehouse-overview-manage-security.md
[Erstellen eines Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[SQL Data Warehouse skalieren]: ./sql-data-warehouse-manage-compute-overview.md
[Überwachen von Abfragen]: ./sql-data-warehouse-manage-monitor.md
[Bereitstellungsanweisungen]: ./sql-data-warehouse-get-started-provision.md
[Erstellen einer neuen Azure SQL-Firewall auf Serverebene]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Visual Studio 2013 oder 2015]: ./sql-data-warehouse-get-started-connect.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[SQL Server Native Client 10/11 (ODBC)]: https://msdn.microsoft.com/library/ms131415.aspx

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum]: https://social.msdn.microsoft.com/Forums/de-DE/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

<!---HONumber=AcomDC_0629_2016-->