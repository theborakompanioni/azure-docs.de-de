---
title: Indizieren von Azure Blob Storage mit Azure Search
description: Erfahren Sie, wie Sie Azure Blob Storage indizieren und Text aus Dokumenten mit Azure Search extrahieren.
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 2a5968f4-6768-4e16-84d0-8b995592f36a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/15/2017
ms.author: eugenesh
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: e14da5fa10533d922a6263e8f52a53c0eaa23393
ms.lasthandoff: 04/25/2017

---

# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Indizieren von Dokumenten in Azure Blob Storage mit Azure Search
Dieser Artikel beschreibt, wie Sie Azure Search zum Indizieren von Dokumenten (z.B. PDF- oder Microsoft Office-Dokumente und verschiedene andere gängige Formate) verwenden, die in Azure-Blobspeicher gespeichert sind. Zunächst werden grundlegende Informationen zu Einrichten und Konfigurieren eines Blobindexers erläutert. Anschließend folgt eine ausführlichere Betrachtung der Verhaltensweisen und Szenarien, die Ihnen voraussichtlich begegnen.

## <a name="supported-document-formats"></a>Unterstützte Dokumentformate
Der Blobindexer kann Text aus den folgenden Dokumentformaten extrahieren:

* PDF
* Microsoft Office-Formate: DOCX/DOC, XLSX/XLS, PPTX/PPT, MSG (Outlook-E-Mails)  
* HTML
* XML
* ZIP
* EML
* Nur-Text-Dateien  
* JSON (siehe Vorschaufeature [Indizierung der JSON-Blobs](search-howto-index-json-blobs.md))
* CSV (siehe Vorschaufeature [Indizierung der CSV-Blobs](search-howto-index-csv-blobs.md))

> [!IMPORTANT]
> Die Unterstützung für CSV- und JSON-Arrays befindet sich derzeit in der Vorschauphase. Diese Formate sind nur mit der Version **2015-02-28-Preview** der REST-API oder der Version 2.x-preview des .NET SDKs verfügbar. Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.
>
>

## <a name="setting-up-blob-indexing"></a>Einrichten der Blobindizierung
Sie können einen Azure Blob Storage-Indexer über folgende Elemente einrichten:

* [Azure-Portal](https://ms.portal.azure.com)
* Azure Search [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Mit dem Azure Search [.NET SDK](https://aka.ms/search-sdk)

> [!NOTE]
> Einige Features (z.B. Feldzuordnungen) sind im Portal noch nicht verfügbar und müssen programmgesteuert verwendet werden.
>
>

Hier wird der Ablauf unter Verwendung der REST-API veranschaulicht.

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle
Eine Datenquelle gibt an, welche Daten indiziert werden müssen. Sie legt außerdem die Anmeldeinformationen für den Zugriff auf die Daten sowie die Richtlinien fest, mit denen Änderungen an den Daten effizient identifiziert werden können (z.B. neue, geänderte oder gelöschte Zeilen). Eine Datenquelle kann von mehreren Indexern im selben Suchdienst verwendet werden.

Für die Blobindizierung muss die Datenquelle über die folgenden erforderlichen Eigenschaften verfügen:

* **name** ist der eindeutige Name der Datenquelle im Suchdienst.
* **type** muss `azureblob` lauten.
* Mit **credentials** wird die Speicherkonto-Verbindungszeichenfolge als `credentials.connectionString`-Parameter angegeben. Details finden Sie weiter unten unter [Angeben von Anmeldeinformationen](#Credentials).
* Mit **container** wird ein Container in Ihrem Speicherkonto angegeben. Standardmäßig können alle Blobs im Container abgerufen werden. Wenn Sie nur Blobs in einem bestimmten virtuellen Verzeichnis indizieren möchten, können Sie dieses Verzeichnis mit dem optionalen **query**-Parameter angeben.

So erstellen Sie eine Datenquelle:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Weitere Informationen über die API zum Erstellen einer Datenquelle finden Sie unter [Datenquelle erstellen](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Angeben von Anmeldeinformationen ####

Sie haben folgende Möglichkeiten zum Angeben der Anmeldeinformationen für den Blobcontainer:

- **Verbindungszeichenfolge für den Vollzugriff auf ein Speicherkonto**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`. Sie können die Verbindungszeichenfolge über das Azure-Portal abrufen, indem Sie auf dem Blatt des Speicherkontos zu „Einstellungen“ > „Schlüssel“ (für klassische Speicherkonten) oder zu „Einstellungen“ > „Zugriffsschlüssel“ (für Azure Resource Manager-Speicherkonten) navigieren.
- Verbindungszeichenfolge für eine **Shared Access Signature (SAS) für ein Speicherkonto**: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl`. Die SAS muss über Listen- und Leseberechtigungen für Container und Objekte (in diesem Fall Blobs) verfügen.
-  **Shared Access Signature des Containers**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl`. Die SAS muss über Listen- und Leseberechtigungen für den Container verfügen.

Weitere Informationen zu Shared Access Signatures von Speichern finden Sie unter [Verwenden von Shared Access Signatures (SAS)](../storage/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Bei Verwendung von SAS-Anmeldeinformationen müssen Sie die Anmeldedaten für die Datenquellen in regelmäßigen Abständen mit erneuerten Signaturen aktualisieren, um den Ablauf zu verhindern. Falls SAS-Anmeldedaten ablaufen, tritt beim Indexer ein Fehler mit ungefähr folgender Fehlermeldung auf: `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Schritt 2: Erstellen eines Index
Mit dem Index werden die Felder in einem Dokument, Attribute und andere Konstrukte für die Suchoberfläche angegeben.

Hier sehen Sie, wie Sie einen Index mit einem durchsuchbaren `content`-Feld zum Speichern des aus Blobs extrahierten Texts erstellen:   

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Weitere Informationen zum Erstellen von Indizes finden Sie unter [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) (Index erstellen).

### <a name="step-3-create-an-indexer"></a>Schritt 3: Erstellen eines Indexers
Ein Indexer verbindet eine Datenquelle mit einem Zielsuchindex und stellt einen Zeitplan zur Automatisierung der Datenaktualisierung bereit.

Nach der Erstellung von Index und Datenquelle können Sie den Indexer erstellen:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Dieser Indexer wird alle zwei Stunden ausgeführt (das Planungsintervall wird auf „PT2H“ festgelegt). Um einen Indexer alle 30 Minuten auszuführen, legen Sie das Intervall auf „PT30M“ fest. Das kürzeste unterstützte Intervall beträgt fünf Minuten. Der Zeitplan ist optional. Ohne Zeitplan wird ein Indexer nur einmal bei seiner Erstellung ausgeführt. Allerdings können Sie ein Indexer bei Bedarf jederzeit ausführen.   

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Indexer erstellen](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="how-azure-search-indexes-blobs"></a>Wie indiziert Azure Search Blobs?

Je nach [Indexer-Konfiguration](#PartsOfBlobToIndex), kann der Blobindexer Metadaten und Textinhalt, Speicher- und Inhaltsmetadaten oder nur Speichermetadaten indizieren (nützlich, wenn Sie nur die Metadaten interessieren und den Inhalt des Blobs nicht indizieren müssen). Der Indexer extrahiert standardmäßig sowohl die Metadaten als auch den Inhalt.

> [!NOTE]
> Die Blobs mit strukturiertem Inhalt wie JSON oder CSV werden standardmäßig als ein einzelnes Textsegment indiziert. Wenn Sie JSON- und CSV-Blobs in einem strukturierten Verfahren indizieren möchten, finden Sie unter [Indizierung der JSON-Blobs](search-howto-index-json-blobs.md) und [Indizierung der CSV-Blobs](search-howto-index-csv-blobs.md) Vorschaufeatures.
> 
> Ein Verbunddokument oder eingebettetes Dokument (z.B. ein ZIP-Archiv oder ein Word-Dokument mit eingebetteter Outlook-E-Mail mit Anhängen) wird ebenfalls als einzelnes Dokument indiziert.

* Der Textinhalt des Dokuments wird in ein Zeichenfolgefeld mit dem Namen `content` extrahiert.

> [!NOTE]
> Azure Search beschränkt die Menge des Texts, der extrahiert wird, in Abhängigkeit vom Tarif: 32.000 Zeichen für den Free-Tarif, 64.000 für Basic und 4 Millionen für Standard, Standard S2 und Standard S3. Für gekürzte Dokumente wird eine Warnung in die Statusantwort des Indexers einbezogen.  

* Falls für das Blob vom Benutzer angegebene Metadateneigenschaften vorhanden sind, werden diese „Wort für Wort“ extrahiert.
* Standardmäßige Blob-Metadateneigenschaften werden in die folgenden Felder extrahiert:

  * **metadata\_storage\_name** (Edm.String): der Dateiname des Blobs. Für ein Blob mit dem Namen „/my-container/my-folder/subfolder/resume.pdf“ lautet der Wert dieses Felds beispielsweise `resume.pdf`.
  * **metadata\_storage\_path** (Edm.String): der vollständige URI des Blobs, einschließlich Speicherkonto. Beispiel: `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **metadata\_storage\_content\_type** (Edm.String): Inhaltstyp, der mit dem Code angegeben wird, den Sie zum Hochladen des Blobs verwendet haben. Beispiel: `application/octet-stream`.
  * **metadata\_storage\_last\_modified** (Edm.DateTimeOffset): Zeitstempel der letzten Änderung für das Blob. In Azure Search wird dieser Zeitstempel zum Identifizieren geänderter Blobs verwendet, um die erneute Indizierung aller Elemente nach der ersten Indizierung zu vermeiden.
  * **metadata\_storage\_size** (Edm.Int64): Blob-Größe in Byte.
  * **metadata\_storage\_content\_md5** (Edm.String): MD5-Hash des Blob-Inhalts, falls verfügbar.
* Die Metadateneigenschaften der einzelnen Dokumentformate werden in die [hier](#ContentSpecificMetadata) aufgeführten Felder extrahiert.

Es ist nicht erforderlich, Felder für alle obigen Eigenschaften in Ihrem Suchindex zu definieren. Erfassen Sie einfach die Eigenschaften, die Sie für Ihre Anwendung benötigen.

> [!NOTE]
> Die Feldnamen im vorhandenen Index unterscheiden sich von den Feldnamen, die während der Dokumentextrahierung generiert werden. Sie können **Feldzuordnungen** verwenden, um die von Azure Search bereitgestellten Eigenschaftennamen den Feldnamen in Ihrem Suchindex zuzuordnen. Weiter unten finden Sie ein Beispiel für die Verwendung von Feldzuordnungen.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Definieren von Dokumentschlüsseln und Feldzuordnungen
In Azure Search wird ein Dokument mit dem Dokumentschlüssel eindeutig identifiziert. Jeder Suchindex muss über genau ein Schlüsselfeld vom Typ „Edm.String“ verfügen. Das Schlüsselfeld ist für jedes Dokument erforderlich, das dem Index hinzugefügt wird (es ist auch das einzige erforderliche Feld).  

Sie sollten sorgfältig abwägen, welches extrahierte Feld Sie dem Schlüsselfeld für Ihren Index zuordnen. Die Kandidaten lauten:

* **metadata\_storage\_name**: Dies ist gegebenenfalls ein passender Kandidat. Beachten Sie aber, dass 1) die Namen unter Umständen nicht eindeutig sind, falls Blobs mit dem gleichen Namen in unterschiedlichen Ordnern enthalten sind, und 2) der Name Zeichen enthalten kann, die in Dokumentschlüsseln ungültig sind, z.B. Bindestriche. Als Lösung für ungültige Zeichen können Sie die `base64Encode`-[Feldzuordnungsfunktion](search-indexer-field-mappings.md#base64EncodeFunction) verwenden. Denken Sie in diesem Fall daran, die Dokumentschlüssel zu codieren, wenn Sie sie in API-Aufrufen übergeben, z.B. bei einem Lookup. (Unter .NET können Sie hierfür beispielsweise die [UrlTokenEncode-Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) verwenden.)
* **metadata\_storage\_path**: Die Verwendung des vollständigen Pfads sorgt für Eindeutigkeit, aber der Pfad enthält auf jeden Fall `/`-Zeichen, die [in einem Dokumentschlüssel ungültig sind](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Wie oben auch, haben Sie hierbei die Möglichkeit, die Schlüssel mit der `base64Encode`-[Funktion](search-indexer-field-mappings.md#base64EncodeFunction) zu codieren.
* Falls keine der Optionen oben für Sie geeignet ist, können Sie den Blobs eine benutzerdefinierte Metadateneigenschaft hinzufügen. Bei dieser Option ist es aber erforderlich, dass diese Metadateneigenschaft im Rahmen des Blob-Uploadvorgangs allen Blobs hinzugefügt wird. Da der Schlüssel eine erforderliche Eigenschaft ist, tritt für alle Blobs, die nicht über diese Eigenschaft verfügen, beim Indizieren ein Fehler auf.

> [!IMPORTANT]
> Wenn keine ausdrückliche Zuordnung für das Schlüsselfeld im Index vorhanden ist, wird in Azure Search automatisch `metadata_storage_path` als Schlüssel verwendet, und die Schlüsselwerte werden Base64-codiert (zweite Option oben).
>
>

Für dieses Beispiel wählen wir das Feld `metadata_storage_name` als Dokumentschlüssel aus. Nehmen wir außerdem an, dass Ihr Index über ein Schlüsselfeld mit dem Namen `key` und ein Feld `fileSize` zum Speichern der Dokumentgröße verfügt. Um alles wie gewünscht einzurichten, geben Sie die folgenden Feldzuordnungen an, wenn Sie den Indexer erstellen oder aktualisieren:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Hier wird beschrieben, wie Sie Feldzuordnungen hinzufügen und die Base64-Codierung von Schlüsseln für einen Indexer aktivieren, um alle Elemente zu verknüpfen:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> Weitere Informationen zu Feldzuordnungen finden Sie in [diesem Artikel](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Steuern, welche Blobs indiziert werden
Sie können steuern, welche Blobs indiziert und welche übersprungen werden.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Indizieren von Blobs mit bestimmten Dateierweiterungen
Sie können nur die Blobs mit den Dateinamenerweiterungen indizieren, die Sie über den `indexedFileNameExtensions`-Konfigurationsparameter des Indexers angeben. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einem vorangestellten Punkt). Um beispielsweise nur die PDF- und DOCX-Blobs zu indizieren, gehen Sie folgendermaßen vor:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Ausschließen von Blobs mit bestimmten Dateierweiterungen
Mithilfe des `excludedFileNameExtensions`-Konfigurationsparameters können Sie verhindern, dass Blobs mit bestimmten Dateinamenerweiterungen indiziert werden. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einem vorangestellten Punkt). Um beispielsweise alle Blobs mit Ausnahme von Blobs mit den Erweiterungen PNG und JPEG zu indizieren, gehen Sie folgendermaßen vor:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Wenn sowohl der `indexedFileNameExtensions`- als auch der `excludedFileNameExtensions`-Parameter vorhanden ist, untersucht Azure Search zunächst `indexedFileNameExtensions` und danach `excludedFileNameExtensions`. Das heißt, wenn die gleiche Dateierweiterung in beiden Listen vorhanden ist, wird sie von der Indizierung ausgeschlossen.

### <a name="dealing-with-unsupported-content-types"></a>Umgang mit nicht unterstützten Inhaltstypen

Der Blobindexer wird standardmäßig beendet, sobald ein Blob mit einem nicht unterstützten Inhaltstyp (z.B. ein Bild) gefunden wird. Natürlich können Sie den Parameter `excludedFileNameExtensions` nutzen, um bestimmte Inhaltstypen zu überspringen. Allerdings müssen Sie möglicherweise Blobs indizieren, ohne im Voraus alle möglichen Inhaltstypen zu kennen. Legen Sie zum Fortsetzen der Indizierung beim Auftreten eines nicht unterstützten Inhaltstyps den Konfigurationsparameter `failOnUnsupportedContentType` auf `false` fest:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

### <a name="ignoring-parsing-errors"></a>Ignorieren von Analysefehlern

Die Dokumentextraktionslogik von Azure Search ist nicht perfekt und wird in manchen Fällen die Analyse von Dokumenten mit einem unterstützten Inhaltstyp wie DOCX- oder PDF-Dateien nicht erfolgreich durchführen können. Wenn Sie den Indizierungsprozess in einem solchen Fall nicht unterbrechen möchten, legen Sie die Konfigurationsparameter `maxFailedItems` und `maxFailedItemsPerBatch` auf sinnvolle Werte fest. Beispiel:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Steuern, welche Teile des Blobs indiziert werden

Sie können mithilfe des Konfigurationsparameters `dataToExtract` steuern, welche Teile der Blobs indiziert werden. Die folgenden Werte sind möglich:

* `storageMetadata` ‒ Gibt an, dass nur die [standardmäßigen Blob-Eigenschaften und benutzerspezifischen Metadaten](../storage/storage-properties-metadata.md) indiziert werden
* `allMetadata` ‒ Gibt an, dass Speichermetadaten und die [inhaltstypspezifischen Metadaten](#ContentSpecificMetadata), die aus dem Blobinhalt extrahiert wurden, indiziert werden
* `contentAndMetadata` ‒ Gibt an, dass alle Metadaten und Textinhalte, die aus dem Blob extrahiert wurden, indiziert werden Dies ist der Standardwert.

Verwenden Sie z.B. Folgendes, um nur die Speichermetadaten zu indizieren:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Verwenden von Blob-Metadaten, um zu steuern, wie Blobs indiziert werden

Die oben beschriebenen Konfigurationsparameter werden auf alle Blobs angewendet. In manchen Fällen möchten Sie möglicherweise steuern, wie *einzelne Blobs* indiziert werden. Dies ist möglich, indem Sie die folgenden Eigenschaften und Werte der Blob-Metadaten hinzufügen:

| Eigenschaftenname | Eigenschaftswert | Erklärung |
| --- | --- | --- |
| AzureSearch_Skip |„true“ |Weist den Blobindexer an, das Blob vollständig zu überspringen. Weder die Metadaten- noch die Inhaltsextraktion werden versucht. Dies ist nützlich, wenn ein bestimmtes Blob wiederholt fehlschlägt und den Indizierungsprozess unterbricht. |
| AzureSearch_SkipContent |„true“ |Dies entspricht der [oben](#PartsOfBlobToIndex) beschriebenen Einstellung `"dataToExtract" : "allMetadata"` zu einem bestimmten Blob. |

## <a name="incremental-indexing-and-deletion-detection"></a>Inkrementelle Indizierung und Erkennung von Löschungen
Wenn Sie einen Blob-Indexer zur Ausführung nach einem Zeitplan einrichten, werden nur die geänderten Blobs neu indiziert. Dies wird anhand des `LastModified`-Zeitstempels der Blobs ermittelt.

> [!NOTE]
> Sie müssen keine Richtlinie zum Erkennen von Änderungen angeben. Die inkrementelle Indizierung wird für Sie automatisch indiziert.

Damit das Löschen von Dokumenten unterstützt wird, sollten Sie die Strategie des „vorläufigen Löschens“ anwenden. Wenn Sie die Blobs direkt löschen, werden die entsprechenden Dokumente nicht aus dem Suchindex entfernt. Führen Sie stattdessen die folgenden Schritte aus:  

1. Fügen Sie dem Blob eine benutzerdefinierte Metadateneigenschaft hinzu, um in Azure Search anzugeben, dass es logisch gelöscht wird.
2. Konfigurieren Sie für die Datenquelle eine Richtlinie zur Erkennung des vorläufigen Löschens.
3. Nachdem das Blob mit dem Indexer verarbeitet wurde (wie durch die API für den Indexerstatus gezeigt), können Sie das Blob physisch löschen.

Bei der folgenden Richtlinie wird ein Blob beispielsweise als gelöscht angesehen, wenn es über die Metadateneigenschaft `IsDeleted` mit dem Wert `true` verfügt:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

## <a name="indexing-large-datasets"></a>Indizieren großer Datasets

Das Indizieren von Blobs kann sehr zeitaufwändig sein. In Fällen, in denen Sie Millionen von Blobs zu indizieren haben, können Sie durch das Partitionieren der Daten und Verwenden mehrerer Indexer zur parallelen Datenverarbeitung die Indizierung beschleunigen. Sie können es folgendermaßen einrichten:

- Partitionieren Sie Ihre Daten in mehrere Blobcontainer oder virtuelle Ordner.
- Richten Sie mehrere Azure Search-Datenquellen ein, eine pro Container oder Ordner. Verwenden Sie den Parameter `query`, um auf einen Blob-Ordner zu verweisen:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Erstellen Sie einen entsprechenden Indexer für jede Datenquelle. Alle Indexer können auf den gleichen Zielsuchindex zeigen.  

## <a name="indexing-documents-along-with-related-data"></a>Indizieren von Dokumenten zusammen mit den zugehörigen Daten

Ihren Dokumenten sind möglicherweise Metadaten zugeordnet – z.B. die Abteilung, die das Dokument erstellt hat –, die als strukturierte Daten an einem der folgenden Speicherorte gespeichert sind.
-   In einem separaten Datenspeicher, z.B. SQL-Datenbank oder DocumentDB
-   Als benutzerdefinierte Metadaten in Azure Blob Storage direkt an jedes Dokument angefügt (Weitere Informationen finden Sie unter [Festlegen und Abrufen von Eigenschaften und Metadaten für Blob-Ressourcen](https://docs.microsoft.com/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources).)

Sie können die Dokumente zusammen mit ihren Metadaten indizieren, indem Sie den gleichen eindeutigen Schlüsselwert jedem Dokument und seinen Metadaten zuweisen und für jeden Indexer die Aktion `mergeOrUpload` angeben. Eine ausführliche Beschreibung dieser Lösung finden Sie im externen Artikel [Combine documents with other data in Azure Search](http://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html) (Zusammenführen von Dokumenten mit anderen Daten in Azure Search).

<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Inhaltstypspezifische Metadateneigenschaften
In der folgenden Tabelle sind die Verarbeitungsschritte für jedes Dokumentformat zusammengefasst, und es werden die Metadateneigenschaften beschrieben, die von Azure Search extrahiert werden.

| Dokumentformat/Inhaltstyp | Inhaltstypspezifische Metadateneigenschaften | Verarbeitungsdetails |
| --- | --- | --- |
| HTML (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Entfernen von HTML-Markup und Extrahieren von Text |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Extrahieren von Text, z. B. eingebettete Dokumente (mit Ausnahme von Bildern) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahieren von Text, z. B. eingebettete Dokumente |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahieren von Text, z. B. eingebettete Dokumente |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahieren von Text, z. B. eingebettete Dokumente |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahieren von Text, z. B. eingebettete Dokumente |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extrahieren von Text, z. B. eingebettete Dokumente |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extrahieren von Text, z. B. eingebettete Dokumente |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Extrahieren von Text, einschließlich Anlagen |
| ZIP (application/zip) |`metadata_content_type` |Extrahieren von Text aus allen Dokumenten im Archiv |
| XML (application/xml) |`metadata_content_type`</br>`metadata_content_encoding`</br> |Entfernen von XML-Markup und Extrahieren von Text |
| JSON (application/json) |`metadata_content_type`</br>`metadata_content_encoding` |Extrahieren von Text<br/>HINWEIS: Wenn Sie mehrere Felder des Dokuments aus einem JSON-Blob extrahieren möchten, helfen Ihnen die ausführlichen Informationen unter [Indizierung der JSON-Blobs](search-howto-index-json-blobs.md) weiter. |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Extrahieren von Text, einschließlich Anlagen |
| Nur-Text (text/plain) |`metadata_content_type`</br>`metadata_content_encoding`</br> | |

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns bei der Verbesserung von Azure Search
Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/) mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.

