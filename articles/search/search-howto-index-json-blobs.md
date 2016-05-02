<properties
pageTitle="Indizierung der JSON-Blobs mit Azure Search-Blobindexer"
description="Erfahren Sie, wie Sie JSON-Blobs mit Azure Search indizieren können."
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
ms.date="04/20/2016"
ms.author="eugenesh" />

# Indizierung der JSON-Blobs mit Azure Search-Blobindexer 

In der Standardeinstellung analysiert der [Azure Search-Blob-Indexer](search-howto-indexing-azure-blob-storage.md) JSON-Blobs als ein einzelnes Segment des Texts. Häufig möchten Sie die Struktur Ihrer JSON-Dokumente beibehalten. Möglicherweise möchten Sie das folgende JSON-Dokument

	{ 
		"article" : {
	 		"text" : "A hopefully useful article explaining how to parse JSON blobs",
	        "datePublished" : "2016-04-13" 
			"tags" : [ "search", "storage", "howto" ]    
	    }
	}

nach den Feldern „text“, „datePublished“ und „tags“ im Suchindex analysieren.

Dieser Artikel beschreibt die Vorgehensweise beim Konfigurieren von Azure Search-Blob-Indexer für die JSON-Analyse. Viel Spaß beim Indizieren!

> [AZURE.IMPORTANT] Diese Funktion befindet sich derzeit in der Vorschauphase. Sie ist nur im Rahmen der REST-API unter der Version **2015-02-28-Preview** verfügbar. Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.

## Einrichten der JSON-Indizierung

Um JSON-Blobs zu indizieren, verwenden Sie den Blobindexer im Modus „JSON-Analyse“. Aktivieren Sie die `useJsonParser`-Konfigurationseinstellung in der Eigenschaft `parameters` der Indexerdefinition:

	{
	  "name" : "my-json-indexer",
	  ... other indexer properties
	  "parameters" : { "configuration" : { "useJsonParser" : true } }
	}

Verwenden Sie bei Bedarf **Feldzuordnungen**, um die Eigenschaften des JSON-Quelldokuments auszuwählen, die zum Auffüllen des Suchindexes verwendet werden. Dies wird weiter unten ausführlicher beschrieben.

> [AZURE.IMPORTANT] Bei Verwendung des JSON-Analysemodus betrachtet Azure Search alle Blobs in Ihrer Datenquelle als JSON-Blobs. Wenn Sie eine Mischung aus JSON- und Nicht-JSON-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf [unserer UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search).

## Verwenden von Feldzuordnungen zum Erstellen von Suchdokumenten 

Derzeit kann Azure Search JSON-Dokumente nicht willkürlich direkt indizieren, da nur primitive Datentypen, Zeichenfolgenarrays und GeoJSON-Punkte unterstützt werden. Sie können jedoch mit **Feldzuordnungen** Teile des JSON-Dokuments auswählen und diese in die Felder der obersten Ebene des Suchdokuments „heben“. Weitere Informationen zu den Grundlagen von Feldzuordnungen finden Sie unter [Anpassen von Azure Search-Indexern](search-indexers-customization.md).

Kommen wir nun zu unserem JSON-Beispieldokument zurück:

    { 
		"article" : {
	 		"text" : "A hopefully useful article explaining how to parse JSON blobs",
	        "datePublished" : "2016-04-13" 
			"tags" : [ "search", "storage", "howto" ]    
	    }
	}

Angenommen, Sie haben einen Suchindex mit den folgenden Feldern: `text` vom Typ „Edm.String“, `date` vom Typ „Edm.DateTimeOffset“ und `tags` vom Typ „Collection(Edm.String)“. Um das JSON-Objekt in der gewünschten Form zuzuordnen, verwenden Sie die folgenden Feldzuordnungen:

	"fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
  	]

Die Quellenfeldnamen in den Zuordnungen werden mit der [JSON-Zeiger](http://tools.ietf.org/html/rfc6901)-Notation angegeben. Sie beginnen mit einem Schrägstrich, um auf das Stammverzeichnis des JSON-Dokuments zu verweisen, und geben dann mittels eines schrägstrichgetrennten Weiterleitungspfads den Pfad zur gewünschten Eigenschaft (auf beliebiger Schachtelungsebene) an.

Sie können auch mit einem nullbasierten Index auf einzelne Arrayelemente verweisen. Um z. B. das erste Element des Arrays „tags“ aus dem obigen Beispiel auszuwählen, verwenden Sie eine Feldzuordnung wie folgt:

	{ "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Wenn ein Quellfeldname in einer Feldzuordnung auf eine Eigenschaft verweist, die nicht in JSON vorhanden ist, wird die Zuordnung ohne Fehler übersprungen. Dies geschieht, damit Dokumente mit einem anderen Schema unterstützt werden können (ein häufiger Anwendungsfall). Da keine Überprüfung erfolgt, müssen Sie darauf achten, Tippfehler in Ihrer Feldzuordnungspezifikation zu vermeiden.

Wenn Ihre JSON-Dokumente nur einfache Eigenschaften der obersten Ebene enthalten, benötigen Sie möglicherweise überhaupt keine Feldzuordnungen. Wenn das JSON-Objekt z. B. wie folgt aussieht, werden die Eigenschaften der obersten Ebene „text“, „datePublished“ und „tags“ direkt den entsprechenden Feldern im Suchindex zugeordnet:
 
	{ 
	   "text" : "A hopefully useful article explaining how to parse JSON blobs",
	   "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
 	}

> [AZURE.NOTE] Azure Search unterstützt derzeit nur die Analyse eines einzigen JSON-Blobs in einem einzigen Suchdokument. Wenn Ihre Blobs JSON-Arrays enthalten, die Sie in mehreren Suchdokumenten analysieren möchten, stimmen Sie für [diese UserVoice-Empfehlung](https://feedback.azure.com/forums/263029-azure-search/suggestions/13431384-parse-blob-containing-a-json-array-into-multiple-d), um uns zu helfen, diese Arbeit zu priorisieren.

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
	    "container" : { "name" : "my-container", "query" : "optional, my-folder" }
	}   

Indexer:

	POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  "name" : "my-json-indexer",
	  "dataSourceName" : "my-blob-datasource",
	  "targetIndexName" : "my-target-index",
	  "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
  	  ]
	}

## Helfen Sie uns bei der Verbesserung von Azure Search

Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/) mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.

<!---HONumber=AcomDC_0420_2016-->