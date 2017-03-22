---
title: Laden von Daten in Azure SQL Data Warehouse | Microsoft Docs
description: "Erfahren Sie mehr über die üblichen Szenarien für das Laden von Daten in SQL Data Warehouse. Dazu gehören die Verwendung von PolyBase, Azure Blob Storage, Flatfiles und Datenträgerversand. Sie können auch Drittanbietertools verwenden."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
ms.custom: loading
translationtype: Human Translation
ms.sourcegitcommit: 9366bae67be7d1abf932d07971d2062c7bd29f3c
ms.openlocfilehash: 49825cc7455ae082ef750a2a31abbd2d71693c79
ms.lasthandoff: 02/27/2017


---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Laden von Daten in Azure SQL Data Warehouse
Eine Zusammenfassung der Optionen und Empfehlungen für das Szenario zum Laden von Daten in SQL Data Warehouse.

Der schwierigste Teil beim Laden von Daten ist in der Regel die Daten für das Laden vorzubereiten. Azure vereinfacht das Laden mithilfe von Azure Blob Storage als gemeinsamer Datenspeicher für viele Dienste und mithilfe von Azure Data Factory zum Organisieren der Kommunikation und des Datentransfers zwischen den Azure-Diensten. Diese Prozesse sind in der PolyBase-Technologie integriert, die MPP (Massively Parallel Processing) verwendet, um die Daten parallel vom Azure Blob Storage in SQL Data Warehouse zu laden. 

Tutorials, in denen Beispieldatenbanken geladen werden, finden Sie unter [Laden von Beispieldatenbanken][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Laden aus Azure Blob Storage
Der schnellste Weg, Daten in SQL Data Warehouse zu importieren, ist die Verwendung von PolyBase zum Laden von Daten aus Azure Blob Storage. PolyBase verwendet die MPP-Struktur (Massively Parallel Processing) von SQL Data Warehouse, um Daten parallel von Azure Blob Storage zu laden. Sie können T-SQL-Befehle oder eine Azure Data Factory-Pipeline verwenden, um PolyBase zu verwenden.

### <a name="1-use-polybase-and-t-sql"></a>1. Verwenden von PolyBase und T-SQL
Zusammenfassung des Ladeprozesses:

1. Verschieben Sie die Daten nach Azure Blob Storage oder Azure Data Lake Store, und speichern Sie sie in Textdateien.
2. Konfigurieren Sie externe Objekte in SQL Data Warehouse, um den Speicherort und das Format der Daten zu definieren.
3. Führen Sie einen T-SQL-Befehl aus, um die Daten parallel in eine neue Datenbanktabelle zu laden.

<!-- 5. Schedule and run a loading job. --> 

Ein Tutorial finden Sie unter [Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Verwenden von Azure Data Factory
Für eine einfachere Möglichkeit PolyBase zu verwenden, können Sie eine Azure Data Factory-Pipeline erstellen, die PolyBase zum Laden von Daten aus Azure Blob Storage in SQL Data Warehouse verwendet. Diese ist schnell zu konfigurieren, da Sie die T-SQL-Objekte nicht definieren müssen. Wenn Sie die externe Daten abfragen müssen, ohne sie zu importieren, verwenden Sie T-SQL. 

Zusammenfassung des Ladeprozesses:

1. Verschieben Sie die Daten in Azure Blob Storage und speichern Sie sie in Textdateien. Azure Data Factory unterstützt derzeit keine Verbindung zwischen ADLS und PolyBase.
2. Erstellen Sie eine Azure Data Factory-Pipeline, um die Daten zu erfassen. Verwenden Sie die PolyBase-Option.
4. Planen Sie die Pipeline und führen Sie sie aus.

Ein Tutorial finden Sie unter [Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>Laden aus SQL Server
Zum Laden von Daten aus SQL Server in SQL Data Warehouse können Sie Integration Services (SSIS) verwenden, Flatfiles übertragen oder Laufwerke an Microsoft liefern. Lesen Sie weiter für eine Zusammenfassung der verschiedenen Ladeprozesse und Links zu Tutorials.

Informationen zur Planung einer vollständigen Datenmigration von SQL Server zu SQL Data Warehouse finden Sie unter der [Übersicht zur Migration][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Verwenden von Integration Services (SSIS)
Falls Sie zum Laden in den SQL Server bereits Integration Services (SSIS)-Pakete verwenden, können Sie ihre Pakete dahingehend aktualisieren, dass sie SQL Server als Quelle und SQL Data Warehouse als Ziel verwenden. Das geht schnell und einfach und es ist eine gute Wahl, wenn Sie nicht versuchen, die Ladeprozesse zu migrieren, um Daten zu verwenden, die sich bereits in der Cloud befinden. Der Nachteil besteht darin, dass der Ladevorgang langsamer ist als wenn PolyBase verwendet wird, da SSIS den Ladevorgang nicht parallel durchführt.

Zusammenfassung des Ladeprozesses:

1. Überarbeiten Sie Ihr Integration Services-Paket dahingehend, dass es auf die SQL Server-Instanz für die Quelle und auf die SQL Data Warehouse-Datenbank für das Ziel verweist.
2. Migrieren Sie das Schema nach SQL Data Warehouse, falls es sich noch nicht dort befinden sollte.
3. Ändern Sie die Zuordnung in den Paketen und verwenden Sie nur die Datentypen, die von SQL Data Warehouse unterstützt werden.
4. Planen Sie das Paket und führen Sie es aus.

Ein Tutorial finden Sie unter [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Verwenden von AZCopy (empfohlen für < 10 TB Daten)
Wenn Ihre Datengröße < 10 TB entspricht, können Sie die Daten aus SQL Server in Flatfiles exportieren, Dateien in Azure Blob Storage kopieren und anschließend PolyBase zum Laden der Daten in SQL Data Warehouse verwenden.

Zusammenfassung des Ladeprozesses:

1. Verwenden Sie das Befehlszeilen-Hilfsprogramm BCP zum Exportieren von Daten aus SQL Server in Flatfiles.
2. Verwenden Sie das Befehlszeilen-Hilfsprogramm AZCopy zum Kopieren von Daten aus Flatfiles in Azure Blob Storage.
3. Verwenden Sie PolyBase zum Laden in SQL Data Warehouse.

Ein Tutorial finden Sie unter [Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>Verwenden von BCP
Wenn Sie eine kleine Menge Daten haben, können Sie BCP zum direkten Laden in Azure SQL Data Warehouse verwenden.

Zusammenfassung des Ladeprozesses:

1. Verwenden Sie das Befehlszeilen-Hilfsprogramm BCP zum Exportieren von Daten aus SQL Server in Flatfiles.
2. Verwenden Sie BCP zum Laden von Daten aus Flatfiles direkt in SQL Data Warehouse.

Ein Tutorial finden Sie unter [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Verwenden von Import/Export (empfohlen für Datengrößen > 10 TB Daten)
Wenn Ihre Datengröße 10 TB überschreitet und Sie diese nach Azure verschieben möchten, empfehlen wir die Verwendung unseres Datenträgerversanddiensts [Import/Export][Import/Export]. 

Zusammenfassung des Ladeprozesses:

1. Verwenden Sie das Befehlszeilen-Hilfsdienstprogramm BCP zum Exportieren von Daten aus SQL Server in Flatfiles oder auf übertragbare Laufwerke.
2. Versenden Sie die Laufwerke an Microsoft.
3. Microsoft lädt die Daten in SQL Data Warehouse.

## <a name="load-from-hdinsight"></a>Laden aus HDInsight
SQL Data Warehouse unterstützt das Laden von Daten aus HDInsight über PolyBase. Der Prozess ist identisch mit dem Laden von Daten aus Azure Blob Storage. Dabei wird mithilfe von PolyBase eine Verbindung mit HDInsight hergestellt, um Daten zu laden. 

### <a name="1-use-polybase-and-t-sql"></a>1. Verwenden von PolyBase und T-SQL
Zusammenfassung des Ladeprozesses:

1. Verschieben Sie Ihre Daten in HDInsight, und speichern Sie sie in Textdateien oder im ORC- oder Parquet-Format.
2. Konfigurieren Sie externe Objekte in SQL Data Warehouse, um den Speicherort und das Format der Daten zu definieren.
3. Führen Sie einen T-SQL-Befehl aus, um die Daten parallel in eine neue Datenbanktabelle zu laden.

Ein Tutorial finden Sie unter [Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Recommendations
Viele unserer Partner stellen Ladelösungen bereit. Weitere Informationen finden Sie in der Liste mit unseren [Lösungspartnern][solution partners]. 

Wenn Ihre Daten aus einer nicht-relationalen Quelle stammen und Sie diese in SQL Data Warehouse laden möchten, müssen Sie sie vor dem Laden in Zeilen und Spalten umwandeln. Die transformierten Daten müssen nicht in einer Datenbank gespeichert werden, sie können in Textdateien gespeichert werden.

Erstellen Sie Statistiken für die neu geladenen Daten. Azure SQL Data Warehouse bietet noch keine Unterstützung für die automatische Erstellung oder die automatische Aktualisierung von Statistiken.  Für die beste Leistung bei Abfragen, ist es wichtig, dass Sie die Statistiken für alle Spalten aller Tabellen nach dem ersten Laden oder nach allen wesentlichen Datenänderungen erstellen.  Weitere Informationen finden Sie unter [Statistiken][Statistics].

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage to SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server to Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server to Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/

