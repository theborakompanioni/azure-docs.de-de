<properties
	pageTitle="Indexer in Azure Search | Microsoft Azure | Gehosteter Cloudsuchdienst"
	description="Erfahren Sie, wie Sie eine Azure SQL-Datenbank, DocumentDB oder Azure-Speicher per Crawler durchlaufen, um durchsuchbare Daten zu extrahieren und einen Azure Search-Index aufzufüllen."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="jhubbard"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="na"
	ms.workload="search"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.date="08/08/2016"
	ms.author="heidist"/>

# Indexer in Azure Search
> [AZURE.SELECTOR]
- [Übersicht](search-indexer-overview.md)
- [Portal](search-import-data-portal.md)
- [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Blob Storage (Vorschau)](search-howto-indexing-azure-blob-storage.md)
- [Table Storage (Vorschau)](search-howto-indexing-azure-tables.md)

Ein **Indexer** in Azure Search ist ein Crawler, mit dem durchsuchbare Daten und Metadaten aus einer externen Datenquelle extrahiert werden und ein Index basierend auf Feld-zu-Feld-Zuordnungen zwischen dem Index und Ihrer Datenquelle aufgefüllt wird. Dieser Ansatz wird auch als „Pullmodell“ bezeichnet, da der Dienst Daten abruft, ohne dass Sie Code zum Übertragen per Pushvorgang an einen Index schreiben müssen.

Sie können einen Indexer als alleiniges Mittel für die Datenerfassung verwenden, oder Sie können eine Kombination aus Verfahren nutzen, bei denen ein Indexer zum Laden eines Teils der Felder in Ihren Index verwendet wird.

Sie können Indexer bei Bedarf oder nach einem Zeitplan für die regelmäßige Datenaktualisierung ausführen, z.B. alle 15 Minuten. Für häufigere Aktualisierungen ist ein Pushmodell erforderlich, bei dem Daten in Azure Search und Ihrer externen Datenquelle gleichzeitig aktualisiert werden.

## Ansätze zum Erstellen und Verwalten von Indexern

Für allgemein verfügbare Indexer wie Azure SQL oder DocumentDB können Sie Indexer mit diesen Ansätzen erstellen und verwalten:

- [Portal > Assistent „Daten importieren“](search-get-started-portal.md)
- [Dienst-REST-API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

Für Vorschauindexer, z.B. Azure Blob oder Table Storage, ist Code erforderlich. Außerdem werden Vorschau-APIs benötigt, z.B. [Azure Search Preview REST API for indexers](search-api-indexers-2015-02-28-preview.md) (Azure Search Preview-REST-API für Indexer). Portaltools sind normalerweise nicht für Vorschaufeatures verfügbar.

## Grundlegende Konfigurationsschritte

Indexer können Features bereitstellen, die für die Datenquelle eindeutig sind. In dieser Hinsicht variieren einige Aspekte von Indexern oder der Datenquellenkonfiguration nach Indexertyp. Für alle Indexer werden aber die gleiche grundlegende Zusammenstellung und die gleichen Anforderungen verwendet. Die Schritte, die für alle Indexer gelten, sind unten beschrieben.

### Schritt 1: Erstellen eines Index

Mit einem Indexer werden einige Aufgaben in Bezug auf die Datenerfassung automatisiert, aber das Erstellen eines Index gehört nicht dazu. Als Voraussetzung hierfür müssen Sie über einen vordefinierten Index mit Feldern verfügen, die den Feldern in Ihrer externen Datenquelle entsprechen. Weitere Informationen zum Strukturieren eines Index finden Sie unter [Create Index (Azure Search-Dienst REST-API)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### Schritt 2: Erstellen einer Datenquelle

Ein Indexer ruft Daten per Pullvorgang aus einer **Datenquelle** ab, die Informationen enthält, z.B. aus einer Verbindungszeichenfolge. Derzeit werden die folgenden Datenquellen unterstützt:

- [Azure SQL-Datenbank oder SQL Server auf einem virtuellen Azure-Computer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure-Blobspeicher (Vorschau)](search-howto-indexing-azure-blob-storage.md) zum Extrahieren von Text aus PDF, Office-Dokumenten, HTML oder XML
- [Azure Table Storage (Vorschau)](search-howto-indexing-azure-tables.md)

Datenquellen werden unabhängig von den Indexern, die darauf zugreifen, konfiguriert und verwaltet. Dies bedeutet, dass eine Datenquelle von mehreren Indexern verwendet werden kann, um mehr als einen Index gleichzeitig zu laden.

### Schritt 3: Erstellen und Planen des Indexers

Die Indexerdefinition ist ein Konstrukt, bei dem der Index, die Datenquelle und ein Zeitplan angegeben werden. Ein Indexer kann von einem anderen Dienst aus auf eine Datenquelle verweisen, solange diese Datenquelle aus demselben Abonnement stammt. Weitere Informationen zum Strukturieren eines Indexers finden Sie unter [Create Indexer (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn946899.aspx) (Create Indexer (Azure Search REST-API)).

## Nächste Schritte

Nachdem Sie sich über die Grundlagen informiert haben, ist der nächste Schritt das Überprüfen der Anforderungen und Aufgaben für jeden Datenquellentyp.

- [Azure SQL-Datenbank oder SQL Server auf einem virtuellen Azure-Computer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure-Blobspeicher (Vorschau)](search-howto-indexing-azure-blob-storage.md) zum Extrahieren von Text aus PDF, Office-Dokumenten, HTML oder XML
- [Azure Table Storage (Vorschau)](search-howto-indexing-azure-tables.md)
- [Indizieren von CSV-Blobs mit Azure Search-Blobindexer (Vorschau)](search-howto-index-csv-blobs.md)
- [Indizieren von JSON-Blobs mit Azure Search-Blobindexer (Vorschau)](search-howto-index-json-blobs.md)

<!---HONumber=AcomDC_0914_2016-->