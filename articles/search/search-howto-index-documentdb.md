---
title: "Indizieren einer DocumentDB-Datenquelle für Azure Search | Microsoft-Dokumentation"
description: Dieser Artikel veranschaulicht das Erstellen eines Azure Search-Indexers mit DocumentDB als Datenquelle.
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: documentdb
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 02/08/2017
ms.author: eugenesh
translationtype: Human Translation
ms.sourcegitcommit: d19a85e127b548e5f8979358879e8b9354934904
ms.openlocfilehash: ca09ac90dfcf125291bc0b312b16e28160a18527


---
# <a name="connecting-documentdb-with-azure-search-using-indexers"></a>Herstellen einer Verbindung zwischen DocumentDB und Azure Search unter Verwendung von Indexern

Wenn Sie eine großartige Suchumgebung für Ihre DocumentDB-Daten realisieren möchten, können Sie einen Azure Search-Indexer konfigurieren und ausführen, der Daten extrahiert und in einen Azure Search-Index abruft. In diesem Artikel erfahren Sie, wie Sie Azure DocumentDB mit Azure Search integrieren, ohne dass Sie Code zum Beibehalten der Indizierungsinfrastruktur schreiben müssen.

Um dies einzurichten, benötigen Sie einen [Azure Search-Dienst](search-create-service-portal.md) sowie einen Indexer, Index und eine Datenquelle. Sie können diese Objekte mithilfe einer der folgenden Methoden erstellen: [Portal](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search) oder [REST-API](/rest/api/searchservice/) für alle Nicht-.NET Framework-Sprachen. 

Wenn Sie sich für das Portal entscheiden, begleitet Sie der [Datenimport-Assistent](search-import-data-portal.md) durch die Erstellung aller dieser Objekte. Ein Standardindex kann in der Regel für Sie generiert werden.

> [!NOTE]
> Starten Sie über das DocumentDB-Dashboard den **Datenimport-Assistenten**, um die Indizierung für diese Datenquelle zu vereinfachen. Navigieren Sie im linken Navigationsbereich zu **Sammlungen** > **Azure Search hinzufügen**, um mit dem Vorgang zu beginnen.

## <a name="a-idconceptsaazure-search-indexer-concepts"></a><a id="Concepts"></a>Azure Search Indexer-Konzepte
Azure Search unterstützt die Erstellung und Verwaltung von Datenquellen (einschließlich DocumentDB) und von Indexern, die gegenläufig zu diesen Datenquellen fungieren.

Eine **Datenquelle** gibt die zu indizierenden Daten, Anmeldeinformationen und Richtlinien für das Bestimmen von Änderungen in den Daten an (z.B. geänderte oder gelöschte Dokumente in Ihrer Sammlung). Die Datenquelle wird als unabhängige Ressource definiert, sodass sie von mehreren Indexern verwendet werden kann.

Ein **Indexer** beschreibt, wie die Daten von der Datenquelle in einen Zielsuchindex fließen. Sie sollten jeweils einen Indexer pro Kombination aus Zielindex und Datenquelle erstellen. Während mehrere Indexer zwar in denselben Index schreiben können, kann ein Indexer nur in einen einzigen Index schreiben. Ein Indexer wird für Folgendes verwendet:

* Eine einmalige Kopie der Daten zum Auffüllen eines Indexes ausführen.
* Einen Index mit Änderungen an der Datenquelle nach einem Zeitplan synchronisieren. Der Zeitplan ist Teil der Indexer-Definition.
* Bedarfs-Updates für einen Index je nach Notwendigkeit abrufen.

## <a name="a-idcreatedatasourceastep-1-create-a-data-source"></a><a id="CreateDataSource"></a>Schritt 1: Erstellen einer Datenquelle
Rufen Sie eine HTTP POST-Anforderung zum Erstellen einer neuen Datenquelle im Azure Search-Dienst einschließlich der folgenden Anforderungsheader auf.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

`api-version` ist erforderlich. Die gültigen Werte sind `2015-02-28` oder eine höhere Version. Unter [API-Versionen in Azure Search](search-api-versions.md) finden Sie alle unterstützten Such-API-Versionen.

Der Anforderungstext umfasst die Datenquellendefinition, welche die folgenden Felder enthalten sollte:

* **Name**: Wählen Sie einen beliebigen Namen für Ihre DocumentDB-Datenbank.
* **Typ**: Verwenden Sie `documentdb`.
* **Anmeldeinformationen**:
  
  * **ConnectionString**: Erforderlich. Geben Sie die Verbindungsinformationen zur Azure DocumentDB-Datenbank im folgenden Format an: `AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`
* **Container**:
  
  * **Name**: Erforderlich. Geben Sie die ID der zu indizierenden DocumentDB-Sammlung an.
  * **Abfrage**: Optional. Sie können eine Abfrage spezifizieren, um ein beliebiges JSON-Dokument in ein Flatfile-Schema zu reduzieren, welches Azure Search indizieren kann.
* **dataChangeDetectionPolicy**: Optional. Siehe [Richtlinie zur Erkennung von Datenänderungen](#DataChangeDetectionPolicy) weiter unten.
* **dataDeletionDetectionPolicy**: Optional. Siehe [Richtlinie zur Erkennung von Datenlöschungen](#DataDeletionDetectionPolicy) weiter unten.

Nachfolgend sehen Sie ein [Beispiel für einen Anforderungstext](#CreateDataSourceExample).

### <a name="a-iddatachangedetectionpolicyacapturing-changed-documents"></a><a id="DataChangeDetectionPolicy"></a>Erfassen von geänderten Dokumenten
Die Richtlinie zum Erkennen von Datenänderungen dient einer effizienten Identifizierung geänderter Datenelemente. Derzeit ist die einzige unterstützte Richtlinie die `High Water Mark`-Richtlinie, die die `_ts` zuletzt modifizierte Zeitstempeleigenschaft verwendet, welche von DocumentDB bereitgestellt wird. Dies wird wie folgt spezifiziert:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Sie müssen `_ts` außerdem in der Projektion und der `WHERE`-Klausel für Ihre Abfrage hinzufügen. Beispiel:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark

### <a name="a-iddatadeletiondetectionpolicyacapturing-deleted-documents"></a><a id="DataDeletionDetectionPolicy"></a>Erfassen von gelöschten Dokumenten
Wenn Zeilen aus der Quelltabelle gelöscht werden, sollten Sie diese Zeilen auch aus dem Suchindex löschen. Die Richtlinie zum Erkennen von Datenlöschungen dient einer effizienten Identifizierung gelöschter Datenelemente. Zurzeit ist `Soft Delete` die einzige unterstützte Richtlinie (die Löschung wird durch ein bestimmtes Kennzeichen markiert). Diese wird folgendermaßen festgelegt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [!NOTE]
> Sie müssen die Eigenschaft „softDeleteColumnName“ in Ihre SELECT-Klausel einbeziehen, wenn Sie eine benutzerdefinierte Projektion verwenden.
> 
> 

### <a name="a-idleveagingqueriesaleveraging-queries"></a><a id="LeveagingQueries"></a>Verwenden von Abfragen
Neben dem Erfassen geänderter und gelöschter Dokumente können mit einer DocumentDB-Abfrage auch geschachtelte Eigenschaften vereinfacht, Arrays entladen, JSON-Eigenschaften geplant und die zu indizierenden Daten gefiltert werden. Das Bearbeiten der zu indizierenden Daten kann die Leistung des Azure Search-Indexers verbessern.

Beispieldokument:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }


Abfrage zur Vereinfachung:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark
    
    
Abfrage zur Projektion:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark


Abfrage zur Arrayentladung:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark
    
    
Abfrage zur Filterung:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark


### <a name="a-idcreatedatasourceexamplearequest-body-example"></a><a id="CreateDataSourceExample"></a>Beispiel für Anforderungstext
Anhand des folgenden Beispiels wird eine Datenquelle mit einer benutzerdefinierten Abfrage und Richtlinienhinweisen erstellt:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

### <a name="response"></a>Antwort
Sie erhalten die Antwort "HTTP 201 Created", wenn die Datenquelle erfolgreich erstellt wurde.

## <a name="a-idcreateindexastep-2-create-an-index"></a><a id="CreateIndex"></a>Schritt 2: Erstellen eines Index
Erstellen Sie einen Azure Search-Zielindex, wenn Sie bislang noch über keinen verfügen. Sie können dies auf der [Azure-Portal-Benutzeroberfläche](search-create-index-portal.md) tun oder dazu die [REST-API zum Erstellen von Indizes](/rest/api/searchservice/create-index) oder die [Index-Klasse](/dotnet/api/microsoft.azure.search.models.index) verwenden.

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Stellen Sie sicher, dass das Schema des Ziel-Indexes mit dem Schema der JSON-Quelldokumente oder mit der Ausgabe Ihrer benutzerdefinierten Abfrageprojektion kompatibel ist.

> [!NOTE]
> Für partitionierte Sammlungen ist der Standarddokumentschlüssel die DocumentDB-Eigenschaft `_rid`, die in Azure Search in `rid` umbenannt wird. Darüber hinaus enthalten die `_rid`-Werte von DocumentDB Zeichen, die in Azure Search-Schlüsseln ungültig sind. Deshalb sind die `_rid`-Werte Base64-codiert.
> 
> 

### <a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Abbildung A: Zuordnung zwischen JSON-Datentypen und Azure Search-Datentypen
| JSON-DATENTYP | KOMPATIBLE ZIEL-INDEX-FELDTYPEN |
| --- | --- |
| Bool |Edm.Boolean, Edm.String |
| Zahlen, die wie Ganzzahlen aussehen |Edm.Int32, Edm.Int64, Edm.String |
| Zahlen, die wie Gleitkommas aussehen |Edm.Double, Edm.String |
| String |Edm.String |
| Arrays primitiver Typen, z. B. "a", "b", "c" |Collection(Edm.String) |
| Zeichenfolgen, die wie Datumsangaben aussehen |Edm.DateTimeOffset, Edm.String |
| GeoJSON-Objekte z. B. { „Typ“: „Punkt“, „Koordinaten“: [ long, lat ] } |Edm.GeographyPoint |
| Andere JSON-Objekte |N/V |

### <a name="a-idcreateindexexamplearequest-body-example"></a><a id="CreateIndexExample"></a>Beispiel für Anforderungstext
Anhand des folgenden Beispiels wird ein Index mit einer ID und einem Beschreibungsfeld erstellt:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

### <a name="response"></a>Antwort
Sie erhalten die Antwort "HTTP 201 Created", wenn der Index erfolgreich erstellt wurde.

## <a name="a-idcreateindexerastep-3-create-an-indexer"></a><a id="CreateIndexer"></a>Schritt 3: Erstellen eines Indexers
Sie können einen neuen Indexer innerhalb des Azure-Suchdiensts mithilfe einer HTTP POST-Anforderung mit den folgenden Headern erstellen.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Der Anforderungstext umfasst die Indexerdefinition, welche die folgenden Felder enthalten sollte:

* **Name**: Erforderlich. Der Name des Indexers.
* **dataSourceName**: Erforderlich. Der Name einer vorhandenen Datenquelle.
* **targetIndexName**: Erforderlich. Der Name eines vorhandenen Indexes.
* **Zeitplan**: Optional. Siehe [Indizierungszeitplan](#IndexingSchedule) weiter unten.

### <a name="a-idindexingschedulearunning-indexers-on-a-schedule"></a><a id="IndexingSchedule"></a>Ausführen von Indexern nach einem Zeitplan
Ein Indexer kann optional einen Zeitplan angeben. Wenn ein Zeitplan vorliegt, wird der Indexer regelmäßig gemäß Zeitplan ausgeführt. Der Zeitplan besitzt die folgenden Attribute:

* **Intervall**: Erforderlich. Ein Zeitdauerwert, der ein Intervall oder den Zeitraum für Indexer-Ausführungen angibt. Das kleinste zulässige Intervall beträgt 5 Minuten. Das längste ist ein Tag. Es muss als XSD-Wert „dayTimeDuration“ formatiert sein (eine eingeschränkte Teilmenge eines [ISO 8601-Zeitwerts](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration)). Das Muster hierfür lautet wie folgt: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` = alle 15 Minuten, `PT2H` = alle 2 Stunden.
* **startTime**: Erforderlich. Ein UTC-DateTime-Wert, der angibt, wann die Ausführung des Indexers beginnen soll.

### <a name="a-idcreateindexerexamplearequest-body-example"></a><a id="CreateIndexerExample"></a>Beispiel für Anforderungstext
Anhand des folgenden Beispiels wird ein Indexer erstellt. Dieser kopiert Daten aus der Sammlung, die von der `myDocDbDataSource`-Datenquelle referenziert wird, nach einem Zeitplan in den `mySearchIndex`-Index. Der Zeitplan beginnt am 1. Jan. 2015 UTC und wird stündlich ausgeführt.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

### <a name="response"></a>Antwort
Sie erhalten die Antwort "HTTP 201 Created", wenn der Indexer erfolgreich erstellt wurde.

## <a name="a-idrunindexerastep-4-run-an-indexer"></a><a id="RunIndexer"></a>Schritt 4: Ausführen ein Indexers
Zusätzlich zur Ausführung in regelmäßigen Abständen nach einem Zeitplan kann ein Indexer bei Bedarf auch aufgerufen werden, indem die folgende HTTP POST-Anforderung ausgegeben wird:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

### <a name="response"></a>Antwort
Sie erhalten die Antwort "HTTP 202 Accepted", wenn der Indexer erfolgreich aufgerufen wurde.

## <a name="a-namegetindexerstatusastep-5-get-indexer-status"></a><a name="GetIndexerStatus"></a>Schritt 5: Abrufen des Indexerstatus
Sie können eine HTTP GET-Anforderung aufrufen, um den aktuellen Status und Ausführungsverlauf eines Indexers abzurufen:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

### <a name="response"></a>Antwort
Sie erhalten die Antwort „HTTP 200 OK“, die zusammen mit einem Antworttext zurückgegeben wird. Dieser enthält Informationen zum allgemeinen Integritätsstatus des Indexers, letzten Aufruf des Indexers sowie den Verlauf der jüngsten Aufrufe des Indexers (sofern vorhanden).

Die Antwort sollte etwa wie folgt aussehen:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Der Ausführungsverlauf enthält bis zu 50 der jüngsten abgeschlossenen Ausführungen. Diese sind in umgekehrter chronologischer Reihenfolge sortiert (somit ist die neueste Ausführung als Erstes in der Antwort aufgelistet).

## <a name="a-namenextstepsanext-steps"></a><a name="NextSteps"></a>Nächste Schritte
Glückwunsch! Sie wissen nun, wie Azure DocumentDB mit Azure Search unter Verwendung des Indexers für DocumentDB integriert wird.

* Weitere Informationen zu Azure DocumentDB finden Sie auf der [Seite zum DocumentDB-Dienst](https://azure.microsoft.com/services/documentdb/).
* Weitere Informationen zu Azure Search finden Sie auf der [Seite des Search-Diensts](https://azure.microsoft.com/services/search/).


<!--HONumber=Feb17_HO2-->


