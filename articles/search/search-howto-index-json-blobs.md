<properties
pageTitle="Indizierung der JSON-Blobs mit Azure Search-Blobindexer"
description="Indizierung der JSON-Blobs mit Azure Search-Blobindexer"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# Indizierung der JSON-Blobs mit Azure Search-Blobindexer 

Dieser Artikel beschreibt die Konfiguration des Azure Search-Blobindexers, um strukturierte Inhalte aus Blobs zu extrahieren, die JSON enthalten.

## Szenarios

In der Standardeinstellung analysiert der [Azure Search-Blob-Indexer](search-howto-indexing-azure-blob-storage.md) JSON-Blobs als einen einzelnen Textblock. Häufig möchten Sie die Struktur Ihrer JSON-Dokumente beibehalten. Möglicherweise möchten Sie das folgende JSON-Dokument

	{ 
		"article" : {
	 		"text" : "A hopefully useful article explaining how to parse JSON blobs",
	        "datePublished" : "2016-04-13" 
			"tags" : [ "search", "storage", "howto" ]    
	    }
	}

in einem Azure Search-Dokument mit den Feldern „text“, „datePublished“ und „tags“ analysieren.

Wenn Ihre Blobs ein **Array aus JSON-Objekten** enthalten, können Sie alternativ dazu auch jedes Element des Arrays in ein separates Azure Search-Dokument umwandeln. Als Beispiel ein Blob mit folgender JSON:

	[
		{ "id" : "1", "text" : "example 1" },
		{ "id" : "2", "text" : "example 2" },
		{ "id" : "3", "text" : "example 3" }
	]

Damit können Sie Ihren Azure Search-Index mit 3 separaten Dokumenten auffüllen, die jeweils die Felder „id“ und „text“ aufweisen.

> [AZURE.IMPORTANT] Diese Funktion befindet sich derzeit in der Vorschauphase. Sie ist nur im Rahmen der REST-API unter der Version **2015-02-28-Preview** verfügbar. Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.

## Einrichten der JSON-Indizierung

Um JSON-Blobs zu indizieren, legen Sie den `parsingMode`-Konfigurationsparameter auf `json` (um jedes Blob als ein einzelnes Dokument zu indizieren) oder auf `jsonArray` fest (wenn Ihre Blobs ein JSON-Array enthalten):

	{
	  "name" : "my-json-indexer",
	  ... other indexer properties
	  "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
	}

Verwenden Sie bei Bedarf **Feldzuordnungen**, um die Eigenschaften des JSON-Quelldokuments auszuwählen, die zum Auffüllen des Zielsuchindexes verwendet werden. Dies wird weiter unten ausführlicher beschrieben.

> [AZURE.IMPORTANT] Bei Verwendung der Modi `json` oder `jsonArray` betrachtet Azure Search alle Blobs in Ihrer Datenquelle als JSON-Blobs. Wenn Sie eine Mischung aus JSON- und Nicht-JSON-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf [unserer UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search).

## Verwenden von Feldzuordnungen zum Erstellen von Suchdokumenten 

Derzeit kann Azure Search JSON-Dokumente nicht willkürlich direkt indizieren, da nur primitive Datentypen, Zeichenfolgenarrays und GeoJSON-Punkte unterstützt werden. Sie können jedoch mit **Feldzuordnungen** Teile des JSON-Dokuments auswählen und diese in die Felder der obersten Ebene des Suchdokuments „heben“. Informationen zu den Grundlagen von Feldzuordnungen finden Sie unter [Durch Azure Search-Indexerfeldzuordnungen werden die Unterschiede zwischen Datenquellen und Suchindizes überbrückt](search-indexer-field-mappings.md).

Kommen wir nun zu unserem JSON-Beispieldokument zurück:

    { 
		"article" : {
	 		"text" : "A hopefully useful article explaining how to parse JSON blobs",
	        "datePublished" : "2016-04-13" 
			"tags" : [ "search", "storage", "howto" ]    
	    }
	}

Angenommen, Sie haben einen Suchindex mit den folgenden Feldern: `text` vom Typ Edm.String, `date` vom Typ Edm.DateTimeOffset und `tags` vom Typ Collection(Edm.String). Um das JSON-Objekt in der gewünschten Form zuzuordnen, verwenden Sie die folgenden Feldzuordnungen:

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

## Indizieren von geschachtelten JSON-Arrays

Was geschieht, wenn Sie ein Array aus JSON-Objekten indizieren möchten, dieses Array aber irgendwo im Dokument geschachtelt ist? Sie können mithilfe der Konfigurationseigenschaft `documentRoot` auswählen, welche Eigenschaft das Array enthält. Angenommen, Ihre Blobs sehen folgendermaßen aus:

	{ 
		"level1" : {
			"level2" : [
				{ "id" : "1", "text" : "Use the documentRoot property" }, 
				{ "id" : "2", "text" : "to pluck the array you want to index" },
				{ "id" : "3", "text" : "even if it's nested inside the document" }  
			]
		}
	} 

Dann verwenden Sie diese Konfiguration, um das in der Eigenschaft „level2“ enthaltene Array zu indizieren:

	{
		"name" : "my-json-array-indexer",
		... other indexer properties
		"parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
	}


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

<!---HONumber=AcomDC_0727_2016-->