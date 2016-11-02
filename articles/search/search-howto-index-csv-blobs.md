<properties
pageTitle="Indizierung von CSV-Blobs mit Azure Search-Blobindexer | Microsoft Azure"
description="Erfahren Sie, wie Sie CSV-Blobs mit Azure Search indizieren können."
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/12/2016"
ms.author="eugenesh" />

# Indizierung von CSV-Blobs mit Azure Search-Blobindexer 

In der Standardeinstellung analysiert der [Azure Search-Blobindexer](search-howto-indexing-azure-blob-storage.md) durch Trennzeichen getrennte Blobs als ein einzelnes Textsegment. Bei Blobs mit CSV-Daten sollen die einzelnen Zeilen im Blob jedoch häufig als separates Dokument behandelt werden. Beispiel:

	id, datePublished, tags
	1, 2016-01-12, "azure-search,azure,cloud" 
	2, 2016-07-07, "cloud,mobile" 

Es wird empfohlen, diesen durch Trennzeichen getrennten Text als zwei Dokumente zu analysieren. Dabei sollte jedes Dokument die Felder „id“, „datePublished“ und „tags“ enthalten.

In diesem Artikel erfahren Sie, wie Sie CSV-Blobs mit einem Azure Search-Blobindexer analysieren.

> [AZURE.IMPORTANT] Diese Funktion befindet sich derzeit in der Vorschauphase. Sie ist nur im Rahmen der REST-API unter der Version **2015-02-28-Preview** verfügbar. Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.

## Einrichten der CSV-Indizierung

Erstellen oder aktualisieren Sie zum Indizieren von CSV-Blobs eine Indexerdefinition mit dem `delimitedText`-Analysemodus:

	{
	  "name" : "my-csv-indexer",
	  ... other indexer properties
	  "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
	}

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Erstellen eines Indexers](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders` gibt an, dass die erste (nicht leere) Zeile der einzelnen Blobs Header enthält. Wenn Blobs am Anfang keine Headerzeile enthalten, sollten die Header in der Indexerkonfiguration angegeben werden:

	"parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Derzeit wird nur die UTF-8-Codierung unterstützt. Darüber hinaus wird nur das Kommazeichen `','` als Trennzeichen unterstützt. Wenn Sie Unterstützung für andere Codierungen oder Trennzeichen benötigen, informieren Sie uns auf [unserer UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Bei Verwendung des Analysemodus für durch Trennzeichen getrennten Text betrachtet Azure Search alle Blobs in Ihrer Datenquelle als CSV-Blobs. Wenn Sie eine Mischung aus CSV- und Nicht-CSV-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf [unserer UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search).

## Beispiele für Anforderungen

Dies alles wird an vollständigen Nutzlastbeispielen demonstriert.

Datenquelle:

	POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	    "name" : "my-blob-datasource",
	    "type" : "azureblob",
	    "credentials" : { "connectionString" : "<my storage connection string>" },
	    "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
	}   

Indexer:

	POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  "name" : "my-csv-indexer",
	  "dataSourceName" : "my-blob-datasource",
	  "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
	}

## Helfen Sie uns bei der Verbesserung von Azure Search

Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/) mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.

<!---HONumber=AcomDC_0713_2016-->