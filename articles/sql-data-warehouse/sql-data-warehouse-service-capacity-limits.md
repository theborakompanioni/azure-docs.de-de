<properties
   pageTitle="SQL Data Warehouse – Kapazitätsgrenzen | Microsoft Azure"
   description="Maximale Werte für Verbindungen, Datenbanken, Tabellen und Abfragen für SQL Data Warehouse."
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
   ms.date="07/11/2016"
   ms.author="sonyama;barbkess;jrj"/>

# Kapazitätsgrenzen von SQL Data Warehouse

Die folgenden Tabellen erhalten die maximalen Werte, die für verschiedene Komponenten von Azure SQL Data Warehouse zulässig sind.


## Workloadverwaltung

| Kategorie | Beschreibung | Maximum |
| :------------------ | :------------------------------------------- | :----------------- |
| [Data Warehouse-Einheiten (DWU)][]| Compute, Arbeitsspeicher und E/A-Ressourcen | 6000 |
| Datenbankverbindung | Gleichzeitig geöffnete Sitzungen | 1\.024<br/><br/>Wir unterstützen maximal 1.024 aktive Verbindungen, wobei jede Verbindung gleichzeitig Anforderungen an eine SQL Data Warehouse-Datenbank übermitteln kann. Beachten Sie, dass die Anzahl der Abfragen, die tatsächlich gleichzeitig ausgeführt werden können, begrenzt ist. Wenn der Grenzwert überschritten wird, gelangt die Anforderung in eine interne Warteschlange, in der sie auf die Verarbeitung wartet.|
| Datenbankverbindung | Maximaler Arbeitsspeicher für vorbereitete Anweisungen | 20 MB |
| [Workloadverwaltung][] | Maximale Anzahl gleichzeitiger Abfragen | 32<br/><br/> SQL Data Warehouse führt standardmäßig bis zu 32 gleichzeitige Abfragen durch und reiht verbleibende Abfragen in die Warteschlange ein.<br/><br/>Der Grad an Parallelität kann sich verringern, wenn Benutzer einer höheren Ressourcenklasse zugewiesen werden. Einige Abfragen, wie DMV-Abfragen, dürfen immer ausgeführt werden.|


## Datenbankobjekte

| Kategorie | Beschreibung | Maximum |
| :---------------- | :------------------------------------------- | :----------------- |
| Datenbank | Max. Größe | 240 TB komprimiert auf dem Datenträger<br/><br/>Dieser Speicherplatz ist unabhängig von „tempdb“ oder vom Protokollspeicherplatz und daher für permanente Tabellen reserviert. Die gruppierte Columnstore-Komprimierung wird auf fünffach geschätzt. Dies bedeutet, dass die nicht komprimierte Größe der Datenbank auf ungefähr 1 PB zunehmen kann, wenn alle Tabellen gruppierte Columnstore-Tabellen sind (der standardmäßige Tabellentyp).|
| Tabelle | Max. Größe | 60 TB komprimiert auf dem Datenträger |
| Tabelle | Tabellen pro Datenbank | 2 Milliarden |
| Tabelle | Spalten pro Tabelle | 1024 Spalten |
| Tabelle | Bytes pro Spalte | 8000 Bytes |
| Tabelle | Bytes pro Zeile, definierte Größe | 8\.060 Bytes<br/><br/>Die Anzahl von Bytes pro Zeile wird auf die gleiche Weise wie bei SQL Server mit aktivierter Seitenkomprimierung berechnet. Wie SQL Server auch, unterstützt SQL Data Warehouse die Speicherung von Zeilenüberlaufsdaten, sodass Spalten variabler Länge aus der Zeile verschoben werden können. Im Hauptdatensatz für Spalten variabler Länge, die aus einer Zeile verschoben wurden, wird nur ein 24-Byte-Stamm gespeichert. Weitere Informationen finden Sie unter [Zeilenüberlauf bei Daten über 8 KB][] in der SQL Server-Onlinedokumentation.<br/><br/>Eine Liste der SQL Data Warehouse-Datentypgrößen finden Sie unter [CREATE TABLE (Azure SQL Data Warehouse)][]. |
| Table | Partitionen pro Tabelle | 15\.000<br/><br/>Für eine hohe Leistung empfehlen wir das Minimieren der Anzahl der benötigten Partitionen, während Ihre Geschäftsfunktionen weiterhin unterstützt werden. Mit einer steigenden Anzahl von Partitionen wächst der Verarbeitungsaufwand für Datendefinitionssprache (DDL)- und Datenbearbeitungssprache (DML)-Vorgänge, was zu Leistungseinbußen führt.|
| Tabelle | Zeichen pro Partitionsbegrenzungswert.| 4000 |
| Index | Nicht gruppierte Indizes pro Tabelle. | 999<br/><br/>Gilt nur für Rowstore-Tabellen.|
| Index | Gruppierte Indizes pro Tabelle. | 1<br><br/>Gilt für Rowstore- und Columnstore-Tabellen.|
| Index | Zeilen in einer Rowgroup eines Columnstore-Indexes | 1\.024<br/><br/>Jeder Columnstore-Index wird in Form von mehreren Columnstore-Indizes implementiert. Beachten Sie, dass beim Einfügen von 1.024 Zeilen in einen SQL Data Warehouse-Columnstore-Index die Zeilen nicht alle in dieselbe Rowgroup übertragen werden.|
| Index | Parallele Erstellung gruppierter Columnstore-Indizes. | 32<br/><br/>Gilt, wenn alle gruppierten Columnstore-Indizes jeweils für verschiedene Tabellen erstellt werden. Pro Tabelle ist nur das Erstellen eines gruppierten Columnstore-Index zulässig. Zusätzliche Anforderungen werden in eine Warteschlange gestellt.|
| Index | Größe des Indexschlüssels. | 900 Bytes<br/><br/>Gilt nur für Rowstore-Indizes.<br/><br/>Indizes für „varchar“-Spalten mit einer maximalen Größe von mehr als 900 Bytes können erstellt werden, wenn die vorhandenen Daten in den Spalten bei der Indexerstellung nicht größer als 900 Bytes sind. Anschließende auf die Spalten angewendete INSERT- oder UPDATE-Anweisungen, die bewirken, dass die Gesamtgröße 900 Bytes überschreitet, haben allerdings keinen Erfolg.|
| Index | Schlüsselspalten pro Index. | 16<br/><br/>Gilt nur für Rowstore-Indizes. Gruppierte Columnstore-Indizes enthalten alle Spalten.|
| Statistiken | Größe der kombinierten Spaltenwerte. | 900 Bytes. |
| Statistiken | Spalten pro Statistikobjekt. | 32 |
| Statistiken | Für Spalten pro Tabelle erstellte Statistiken. | 30\.000 |
| Gespeicherte Prozeduren | Maximale Schachtelungsebenen. | 8 |
| Sicht | Spalten pro Sicht | 1024 |


## Lädt

| Kategorie | Beschreibung | Maximum |
| :---------------- | :------------------------------------------- | :----------------- |
| PolyBase-Auslastung | Bytes pro Zeile | 32\.768<br/><br/>Der Ladevorgang in PolyBase ist auf das Laden von Zeilen beschränkt, die kleiner als 32.000 sind und nicht in Spalten des Typs VARCHR(MAX) NVARCHAR(MAX) oder VARBINARY(MAX) geladen werden können. Diese Beschränkung existiert zwar noch, wird aber in Kürze entfernt.<br/><br/>


## Abfragen

| Kategorie | Beschreibung | Maximum |
| :---------------- | :------------------------------------------- | :----------------- |
| Abfrage | In Warteschlange gestellte Abfragen von Benutzertabellen. | 1000 |
| Abfrage | Gleichzeitige Abfragen von Systemsichten. | 100 |
| Abfrage | In Warteschlange gestellte Abfragen von Systemsichten | 1000 |
| Abfrage | Maximale Parameter | 2098 |
| Batch | Maximale Größe | 65\.536*4096 |
| SELECT-Ergebnisse | Spalten pro Zeile | 4\.096<br/><br/>Das Ergebnis einer SELECT-Anweisung kann nie mehr als 4.096 Spalten pro Zeile enthalten. Es gibt keine Garantie, dass Sie stets über 4096 verfügen. Wenn der Abfrageplan eine temporäre Tabelle erfordert, gilt möglicherweise der Maximalwert von 1024 Spalten pro Tabelle.|
| SELECT | Geschachtelte Unterabfragen | 32<br/><br/>In einer SELECT-Anweisung sind maximal 32 geschachtelte Unterabfragen zulässig. Es gibt keine Garantie, dass Sie stets über 32 verfügen. Ein JOIN-Befehl kann z. B. eine Unterabfrage in den Abfrageplan einführen. Die Anzahl der Unterabfragen kann auch durch den verfügbaren Speicher eingeschränkt werden.|
| SELECT | Spalten pro JOIN | 1024 Spalten<br/><br/>Für einen JOIN sind maximal 1024 Spalten zulässig. Es gibt keine Garantie, dass Sie stets über 1024 verfügen. Wenn der JOIN-Plan eine temporäre Tabelle mit mehr Spalten als das JOIN-Ergebnis erfordert, gilt die Grenze von 1024 für die temporäre Tabelle. |
| SELECT | Bytes pro GROUP BY-Spalten. | 8060<br/><br/>Die Maximalgröße von Spalten in der GROUP BY-Klausel beträgt 8060 Bytes.|
| SELECT | Bytes pro ORDER BY-Spalten | 8060<br/><br/>Die Maximalgröße von Spalten in der ORDER BY-Klausel beträgt 8060 Bytes.|
| Bezeichner und Konstanten pro Anweisung | Anzahl der Bezeichner und Konstanten, auf die verwiesen wird. | 65\.535<br/><br/>SQL Data Warehouse beschränkt die Anzahl von Bezeichnern und Konstanten, die in einem einzelnen Ausdruck einer Abfrage enthalten sein können. Dieser Grenzwert ist 65.535. Das Überschreiten dieses Werts führt zum SQL Server-Fehler 8632. Weitere Informationen finden Sie unter [Interner Fehler: ein Ausdrucksdienstelimit wurde erreicht.][].|


## Metadaten

| Systemsicht | Maximale Anzahl von Zeilen |
| :--------------------------------- | :------------|
| sys.dm\_pdw\_component\_health\_alerts | 10\.000 |
| sys.dm\_pdw\_dms\_cores | 100 |
| sys.dm\_pdw\_dms\_workers | Gesamtanzahl der DMS-Worker für die letzten 1000 SQL-Anforderungen. |
| sys.dm\_pdw\_errors | 10\.000 |
| sys.dm\_pdw\_exec\_requests | 10\.000 |
| sys.dm\_pdw\_exec\_sessions | 10\.000 |
| sys.dm\_pdw\_request\_steps | Die Gesamtzahl der Schritte für die letzten 1000 SQL-Anforderungen, die in „sys.dm\_pdw\_exec\_requests“ gespeichert sind. |
| sys.dm\_pdw\_os\_event\_logs | 10\.000 |
| sys.dm\_pdw\_sql\_requests | Die letzten 1000 SQL-Anforderungen, die in „sys.dm\_pdw\_exec\_requests“ gespeichert sind. |


## Nächste Schritte
Weitere Referenzinformationen finden Sie unter [SQL Data Warehouse-Referenz – Übersicht][].

<!--Image references-->

<!--Article references-->
[Data Warehouse-Einheiten (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[SQL Data Warehouse-Referenz – Übersicht]: ./sql-data-warehouse-overview-reference.md
[Workloadverwaltung]: ./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->
[Zeilenüberlauf bei Daten über 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[CREATE TABLE (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Interner Fehler: ein Ausdrucksdienstelimit wurde erreicht.]: https://support.microsoft.com/kb/913050

<!---HONumber=AcomDC_0713_2016-->