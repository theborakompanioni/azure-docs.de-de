---
title: Indizierung der JSON-Blobs mit Azure Search-Blobindexer
description: Indizierung der JSON-Blobs mit Azure Search-Blobindexer
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: c4a9e57cda4ba5b4db742c1a37686a802f58212f
ms.lasthandoff: 04/11/2017

---

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indizierung der JSON-Blobs mit Azure Search-Blobindexer
Dieser Artikel beschreibt die Konfiguration des Azure Search-Blobindexers, um strukturierte Inhalte aus Blobs zu extrahieren, die JSON enthalten.

## <a name="scenarios"></a>Szenarios
In der Standardeinstellung analysiert der [Azure Search-Blob-Indexer](search-howto-indexing-azure-blob-storage.md) JSON-Blobs als einen einzelnen Textblock. Häufig möchten Sie die Struktur Ihrer JSON-Dokumente beibehalten. Möglicherweise möchten Sie das folgende JSON-Dokument

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

in einem Azure Search-Dokument mit den Feldern „text“, „datePublished“ und „tags“ analysieren.

Wenn Ihre Blobs ein **Array aus JSON-Objekten**enthalten, können Sie alternativ dazu auch jedes Element des Arrays in ein separates Azure Search-Dokument umwandeln. Als Beispiel ein Blob mit folgender JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Damit können Sie Ihren Azure Search-Index mit drei separaten Dokumenten auffüllen, die jeweils die Felder „id“ und „text“ aufweisen.

> [!IMPORTANT]
> Die Funktion zur Analyse von JSON-Arrays befindet sich derzeit in der Vorschauphase. Sie ist nur im Rahmen der REST-API unter der Version **2015-02-28-Preview**verfügbar. Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.
>
>

## <a name="setting-up-json-indexing"></a>Einrichten der JSON-Indizierung
Das Indizieren von JSON-Blobs ist mit der normalen Dokumentextrahierung vergleichbar. Erstellen Sie zunächst die Datenquelle wie gewohnt: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Anschließend erstellen Sie den Zielsuchindex, sofern nicht bereits vorhanden. 

Schließlich erstellen Sie einen Indexer und legen den `parsingMode`-Parameter auf `json` (zum Indizieren jedes Blob als einzelnes Dokument) oder `jsonArray` (wenn die Blobs JSON-Arrays enthalten und jedes Element eines Arrays als separates Dokument behandelt werden soll) fest:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Verwenden Sie bei Bedarf **Feldzuordnungen** , um die Eigenschaften des JSON-Quelldokuments auszuwählen, die zum Auffüllen des Zielsuchindexes verwendet werden. Dies wird im nächsten Abschnitt näher beschrieben.

> [!IMPORTANT]
> Bei Verwendung der Modi `json` oder `jsonArray` geht Azure Search davon aus, dass alle Blobs in Ihrer Datenquelle JSON enthalten. Wenn Sie eine Kombination von JSON- und Nicht-JSON-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-to-build-search-documents"></a>Verwenden von Feldzuordnungen zum Erstellen von Suchdokumenten
Derzeit kann Azure Search JSON-Dokumente nicht willkürlich direkt indizieren, da nur primitive Datentypen, Zeichenfolgenarrays und GeoJSON-Punkte unterstützt werden. Sie können jedoch mit **Feldzuordnungen** Teile des JSON-Dokuments auswählen und diese in die Felder der obersten Ebene des Suchdokuments „heben“. Informationen zu den Grundlagen von Feldzuordnungen finden Sie unter [Durch Azure Search-Indexerfeldzuordnungen werden die Unterschiede zwischen Datenquellen und Suchindizes überbrückt](search-indexer-field-mappings.md).

Kommen wir nun zu unserem JSON-Beispieldokument zurück:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Angenommen, Sie haben einen Suchindex mit den folgenden Feldern: `text` vom Typ `Edm.String`, `date` vom Typ `Edm.DateTimeOffset`und `tags` vom Typ `Collection(Edm.String)`. Um das JSON-Objekt in der gewünschten Form zuzuordnen, verwenden Sie die folgenden Feldzuordnungen:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Die Quellenfeldnamen in den Zuordnungen werden mit der [JSON-Zeiger](http://tools.ietf.org/html/rfc6901) -Notation angegeben. Sie beginnen mit einem Schrägstrich, um auf das Stammverzeichnis des JSON-Dokuments zu verweisen, und wählen dann mittels eines schrägstrichgetrennten Weiterleitungspfads den Pfad zur gewünschten Eigenschaft (auf beliebiger Schachtelungsebene) aus.

Sie können auch mit einem nullbasierten Index auf einzelne Arrayelemente verweisen. Um z. B. das erste Element des Arrays „tags“ aus dem obigen Beispiel auszuwählen, verwenden Sie eine Feldzuordnung wie folgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Wenn ein Quellfeldname in einer Feldzuordnung auf eine Eigenschaft verweist, die nicht in JSON vorhanden ist, wird die Zuordnung ohne Fehler übersprungen. Dies geschieht, damit Dokumente mit einem anderen Schema unterstützt werden können (ein häufiger Anwendungsfall). Da keine Überprüfung erfolgt, müssen Sie darauf achten, Tippfehler in Ihrer Feldzuordnungspezifikation zu vermeiden.
>
>

Wenn Ihre JSON-Dokumente nur einfache Eigenschaften der obersten Ebene enthalten, benötigen Sie möglicherweise überhaupt keine Feldzuordnungen. Wenn das JSON-Objekt z.B. wie folgt aussieht, werden die Eigenschaften der obersten Ebene „text“, „datePublished“ und „tags“ direkt den entsprechenden Feldern im Suchindex zugeordnet:

    {
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Dies ist eine vollständige Indexer-Nutzlast mit Feldzuordnungen:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="indexing-nested-json-arrays"></a>Indizieren von geschachtelten JSON-Arrays
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

Dann verwenden Sie diese Konfiguration, um das in der `level2`-Eigenschaft enthaltene Array zu indizieren:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns bei der Verbesserung von Azure Search
Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/) mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.

