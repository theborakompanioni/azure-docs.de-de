   <properties
   pageTitle="Laden von Daten in Azure SQL Data Warehouse | Microsoft Azure"
   description="Erfahren Sie mehr über die üblichen Szenarien für das Laden von Daten in SQL Data Warehouse. Dazu gehören die Verwendung von PolyBase, Azure Blob Storage, Flatfiles und Datenträgerversand. Sie können auch Drittanbietertools verwenden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Laden von Daten in Azure SQL Data Warehouse

Eine Zusammenfassung der Optionen und Empfehlungen für das Szenario zum Laden von Daten in SQL Data Warehouse.

Der schwierigste Teil beim Laden von Daten ist in der Regel die Daten für das Laden vorzubereiten. Azure vereinfacht das Laden mithilfe von Azure Blob Storage als gemeinsamer Datenspeicher für viele Dienste und mithilfe von Azure Data Factory zum Organisieren der Kommunikation und des Datentransfers zwischen den Azure-Diensten. Diese Prozesse sind in der PolyBase-Technologie integriert, die MPP (Massively Parallel Processing) verwendet, um die Daten parallel vom Azure Blob Storage in SQL Data Warehouse zu laden.

Tutorials, in denen Beispieldatenbanken geladen werden, finden Sie unter [Laden von Beispieldaten in SQL Data Warehouse][].

## Laden aus Azure Blob Storage
Der schnellste Weg, Daten in SQL Data Warehouse zu importieren, ist die Verwendung von PolyBase zum Laden von Daten aus Azure Blob Storage. PolyBase verwendet die MPP-Struktur (Massively Parallel Processing) von SQL Data Warehouse, um Daten parallel von Azure Blob Storage zu laden. Sie können T-SQL-Befehle oder eine Azure Data Factory-Pipeline verwenden, um PolyBase zu verwenden.

### 1\. Verwenden von PolyBase und T-SQL

Zusammenfassung des Ladeprozesses:

2. Formatieren Sie die Daten als UTF-8, da UTF-16 von PolyBase derzeit nicht unterstützt wird.
2. Verschieben Sie die Daten in Azure Blob Storage und speichern Sie sie in Textdateien.
3. Konfigurieren Sie externe Objekte in SQL Data Warehouse, um den Speicherort und das Format der Daten zu definieren.
4. Führen Sie einen T-SQL-Befehl aus, um die Daten parallel in eine neue Datenbanktabelle zu laden.

<!-- 5. Schedule and run a loading job. --> 

Ein Tutorial finden Sie unter [Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)][].

### 2\. Verwenden von Azure Data Factory

Für eine einfachere Möglichkeit PolyBase zu verwenden, können Sie eine Azure Data Factory-Pipeline erstellen, die PolyBase zum Laden von Daten aus Azure Blob Storage in SQL Data Warehouse verwendet. Diese ist schnell zu konfigurieren, da Sie die T-SQL-Objekte nicht definieren müssen. Wenn Sie die externe Daten abfragen müssen, ohne sie zu importieren, verwenden Sie T-SQL.

Zusammenfassung des Ladeprozesses:

2. Formatieren Sie die Daten als UTF-8, da UTF-16 von PolyBase derzeit nicht unterstützt wird.
2. Verschieben Sie die Daten in Azure Blob Storage und speichern Sie sie in Textdateien.
3. Erstellen Sie eine Azure Data Factory-Pipeline, um die Daten zu erfassen. Verwenden Sie die PolyBase-Option.
4. Planen Sie die Pipeline und führen Sie sie aus.

Ein Tutorial finden Sie unter [Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse (Azure Data Factory)][].


## Laden aus SQL Server
Zum Laden von Daten aus SQL Server in SQL Data Warehouse können Sie Integration Services (SSIS) verwenden, Flatfiles übertragen oder Laufwerke an Microsoft liefern. Lesen Sie weiter für eine Zusammenfassung der verschiedenen Ladeprozesse und Links zu Tutorials.

Informationen zur Planung einer vollständigen Datenmigration von SQL Server zu SQL Data Warehouse finden Sie unter der [Übersicht zur Migration][].

### Verwenden von Integration Services (SSIS)
Falls Sie zum Laden in den SQL Server bereits Integration Services (SSIS)-Pakete verwenden, können Sie ihre Pakete dahingehend aktualisieren, dass sie SQL Server als Quelle und SQL Data Warehouse als Ziel verwenden. Das geht schnell und einfach und es ist eine gute Wahl, wenn Sie nicht versuchen, die Ladeprozesse zu migrieren, um Daten zu verwenden, die sich bereits in der Cloud befinden. Der Nachteil besteht darin, dass der Ladevorgang langsamer ist als wenn PolyBase verwendet wird, da SSIS den Ladevorgang nicht parallel durchführt.

Zusammenfassung des Ladeprozesses:

1. Überarbeiten Sie Ihr Integration Services-Paket dahingehend, dass es auf die SQL Server-Instanz für die Quelle und auf die SQL Data Warehouse-Datenbank für das Ziel verweist.
2. Migrieren Sie das Schema nach SQL Data Warehouse, falls es sich noch nicht dort befinden sollte.
3. Ändern Sie die Zuordnung in den Paketen und verwenden Sie nur die Datentypen, die von SQL Data Warehouse unterstützt werden.
3. Planen Sie das Paket und führen Sie es aus.

Ein Tutorial finden Sie unter [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)][].

### Verwenden von AZCopy (empfohlen für < 10 TB Daten)
Wenn Ihre Datengröße < 10 TB entspricht, können Sie die Daten aus SQL Server in Flatfiles exportieren, Dateien in Azure Blob Storage kopieren und anschließend PolyBase zum Laden der Daten in SQL Data Warehouse verwenden.

Zusammenfassung des Ladeprozesses:

1. Verwenden Sie das Befehlszeilen-Hilfsprogramm BCP zum Exportieren von Daten aus SQL Server in Flatfiles.
2. Verwenden Sie das Befehlszeilen-Hilfsprogramm AZCopy zum Kopieren von Daten aus Flatfiles in Azure Blob Storage.
3. Verwenden Sie PolyBase zum Laden in SQL Data Warehouse.

Ein Tutorial finden Sie unter [Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)][].

### Verwenden von BCP
Wenn Sie eine kleine Menge Daten haben, können Sie BCP zum direkten Laden in Azure SQL Data Warehouse verwenden.

Zusammenfassung des Ladeprozesses:
1. Verwenden Sie das Befehlszeilen-Hilfsprogramm BCP zum Exportieren von Daten aus SQL Server in Flatfiles.
2. Verwenden Sie BCP zum Laden von Daten aus Flatfiles direkt in SQL Data Warehouse.

Ein Tutorial finden Sie unter [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (bcp)][].


### Verwenden von Import/Export (empfohlen für Datengrößen > 10 TB Daten)
Wenn Ihre Datengröße > 10 TB entspricht, und Sie diese in Azure verschieben möchten, empfehlen wir die Verwendung unseres Datenträgerversanddiensts [Import/Export][].

Zusammenfassung des Ladeprozesses:
2. Verwenden Sie das Befehlszeilen-Hilfsdienstprogramm BCP zum Exportieren von Daten aus SQL Server in Flatfiles oder auf übertragbare Laufwerke.
3. Versenden Sie die Laufwerke an Microsoft.
4. Microsoft lädt die Daten in SQL Data Warehouse.


## Recommendations

Viele unserer Partner stellen Ladelösungen bereit. Sehen Sie sich die Liste mit unseren [Lösungspartnern][] an, um mehr zu erfahren.

Wenn Ihre Daten aus einer nicht-relationalen Quelle stammen und Sie diese in SQL Data Warehouse laden möchten, müssen Sie sie vor dem Laden in Zeilen und Spalten umwandeln. Die transformierten Daten müssen nicht in einer Datenbank gespeichert werden, sie können in Textdateien gespeichert werden.

Erstellen Sie Statistiken für die neu geladenen Daten. Azure SQL Data Warehouse bietet noch keine Unterstützung für die automatische Erstellung oder die automatische Aktualisierung von Statistiken. Für die beste Leistung bei Abfragen, ist es wichtig, dass Sie die Statistiken für alle Spalten aller Tabellen nach dem ersten Laden oder nach allen wesentlichen Datenänderungen erstellen. Weitere Informationen finden Sie unter [Statistiken][].


## Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][].

<!--Image references-->

<!--Article references-->
[Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Laden von Daten aus SQL Server in Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Laden von Beispieldaten in SQL Data Warehouse]: ./sql-data-warehouse-load-sample-databases.md
[Übersicht zur Migration]: ./sql-data-warehouse-overview-migrate.md
[Lösungspartnern]: ./sql-data-warehouse-integrate-solution-partners.md
[Entwicklungsübersicht]: ./sql-data-warehouse-overview-develop.md
[Statistiken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/

<!---HONumber=AcomDC_0706_2016-->