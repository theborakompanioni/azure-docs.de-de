<properties
pageTitle="Indizieren von Azure Blob Storage mit Azure Search"
description="Erfahren Sie, wie Sie Azure Blob Storage indizieren und Text aus Dokumenten mit Azure Search extrahieren."
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
ms.date="08/16/2016"
ms.author="eugenesh" />

# Indizieren von Dokumenten in Azure Blob Storage mit Azure Search

Dieser Artikel beschreibt, wie Sie Azure Search zum Indizieren von Dokumenten (z.B. PDF- oder Microsoft Office-Dokumente und verschiedene andere gängige Formate) verwenden, die in Azure-Blobspeicher gespeichert sind. Mit dem neuen Azure Search-Blobindexer verläuft dieser Prozess schnell und reibungslos.

> [AZURE.IMPORTANT] Diese Funktion befindet sich derzeit in der Vorschauphase. Sie ist nur im Rahmen der REST-API unter der Version **2015-02-28-Preview** verfügbar. Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.

## Unterstützte Dokumentformate

Der Blobindexer kann Text aus den folgenden Dokumentformaten extrahieren:

- PDF
- Microsoft Office-Formate: DOCX/DOC, XLSX/XLS, PPTX/PPT, MSG (Outlook-E-Mails)
- HTML
- XML
- ZIP
- EML
- Nur-Text-Dateien
- JSON (Details finden Sie unter [Indizierung von JSON-Blobs](search-howto-index-json-blobs.md))
- CSV (Details finden Sie unter [Indizierung von CSV-Blobs](search-howto-index-csv-blobs.md))

## Einrichten der Blobindizierung

Zum Einrichten und Konfigurieren des Indexers für Azure Blob Storage können Sie die Azure Search-REST-API nutzen, um **Indexer** und **Datenquellen** wie in [diesem Artikel](https://msdn.microsoft.com/library/azure/dn946891.aspx) beschrieben zu erstellen und zu verwalten. Sie können auch die [Version 2.0-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) des .NET SDK verwenden. In Zukunft wird die Blobindizierung vom Azure-Portal unterstützt.

Führen Sie die folgenden drei Schritte aus, um einen Indexer einzurichten: Erstellen einer Datenquelle, Erstellen eines Index, Konfigurieren des Indexers.

### Schritt 1: Erstellen einer Datenquelle

Eine Datenquelle gibt an, welche Daten indiziert werden müssen. Sie legt außerdem die Anmeldeinformationen für den Zugriff auf die Daten sowie die Richtlinien zur Aktivierung von Azure Search fest, um Änderungen an den Daten effizient identifizieren zu können (z.B. neue, geänderte oder gelöschte Zeilen). Eine Datenquelle kann von mehreren Indexern in demselben Abonnement verwendet werden.

Für die Blobindizierung muss die Datenquelle über die folgenden erforderlichen Eigenschaften verfügen:

- **name** ist der eindeutige Name der Datenquelle im Suchdienst.

- **type** muss `azureblob` lauten.

- Mit **credentials** wird die Speicherkonto-Verbindungszeichenfolge als `credentials.connectionString`-Parameter angegeben. Sie erhalten die Verbindungszeichenfolge über das Azure-Portal. Navigieren Sie zum Blatt mit dem gewünschten Speicherkonto, wählen Sie **Einstellungen** > **Schlüssel**, und verwenden Sie den Wert „Primäre Verbindungszeichenfolge“ oder „Sekundäre Verbindungszeichenfolge“. Da die Verbindungszeichenfolge an ein Speicherkonto gebunden ist, wird durch das Angeben der Verbindungszeichenfolge implizit das Speicherkonto identifiziert, das die Daten bereitstellt.

- Mit **container** wird ein Container in Ihrem Speicherkonto angegeben. Standardmäßig können alle Blobs im Container abgerufen werden. Wenn Sie nur Blobs in einem bestimmten virtuellen Verzeichnis indizieren möchten, können Sie dieses Verzeichnis mit dem optionalen **query**-Parameter angeben.

Das folgende Beispiel veranschaulicht eine Datenquellendefinition:

	POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	    "name" : "blob-datasource",
	    "type" : "azureblob",
	    "credentials" : { "connectionString" : "<my storage connection string>" },
	    "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
	}   

Weitere Informationen über die API zum Erstellen einer Datenquelle finden Sie unter [Datenquelle erstellen](search-api-indexers-2015-02-28-preview.md#create-data-source).

### Schritt 2: Erstellen eines Index 

Mit dem Index werden die Felder in einem Dokument, Attribute und andere Konstrukte für die Suchoberfläche angegeben.

Stellen Sie bei der Blobindizierung sicher, dass der Index über ein durchsuchbares Feld `content` zum Speichern des Blobs verfügt.

	POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
	Content-Type: application/json
	api-key: [admin key]

	{
  		"name" : "my-target-index",
  		"fields": [
    		{ "name": "id", "type": "Edm.String", "key": true, "searchable": false },
    		{ "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
  		]
	}

Weitere Informationen zur API zum Erstellen eines Index finden Sie unter [Create Index (Azure Search-Dienst REST-API)](https://msdn.microsoft.com/library/dn798941.aspx).

### Schritt 3: Erstellen eines Indexers 

Ein Indexer verbindet Datenquellen mit Zielsuchindizes und stellt Zeitplaninformationen bereit, sodass Sie die Datenaktualisierung automatisieren können. Nachdem der Index und die Datenquelle erstellt wurden, ist es relativ einfach, einen Indexer zu erstellen, mit dem auf die Datenquelle und einen Zielindex verwiesen wird. Zum Beispiel:

	POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  "name" : "blob-indexer",
	  "dataSourceName" : "blob-datasource",
	  "targetIndexName" : "my-target-index",
	  "schedule" : { "interval" : "PT2H" }
	}

Dieser Indexer wird alle zwei Stunden ausgeführt (das Planungsintervall wird auf „PT2H“ festgelegt). Um einen Indexer alle 30 Minuten auszuführen, legen Sie das Intervall auf „PT30M“ fest. Das kürzeste unterstützte Intervall ist 5 Minuten. Der Zeitplan ist optional. Falls Sie ihn weglassen, wird ein Indexer nur einmal bei seiner Erstellung ausgeführt. Allerdings können Sie ein Indexer bei Bedarf jederzeit ausführen.

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer).


## Prozess der Dokumentextrahierung

Azure Search indiziert jedes Dokument (Blob) wie folgt:

- Der gesamte Inhalt des Dokuments wird in ein Zeichenfolgenfeld mit dem Namen `content` extrahiert. Beachten Sie, dass derzeit keine Unterstützung zum Extrahieren mehrerer Dokumente aus einem einzelnen Blob vorhanden ist:
	- Beispielsweise wird eine CSV-Datei als einzelnes Dokument indiziert. Stimmen Sie für [diesen UserVoice-Vorschlag](https://feedback.azure.com/forums/263029-azure-search/suggestions/13865325-please-treat-each-line-in-a-csv-file-as-a-separate), wenn Sie jede Zeile in einer CSV-Datei als separates Dokument behandeln müssen.
	- Ein Verbunddokument oder eingebettetes Dokument (z. B. ein ZIP-Archiv oder ein Word-Dokument mit eingebetteter Outlook-E-Mail mit PDF-Anhang) wird ebenfalls als einzelnes Dokument indiziert.

- Falls für das Blob vom Benutzer angegebene Metadateneigenschaften vorhanden sind, werden diese „Wort für Wort“ extrahiert. Die Metadateneigenschaften können auch verwendet werden, um bestimmte Aspekte des Prozesses der Dokumentextrahierung zu steuern. Weitere Informationen finden Sie unter [Verwenden von benutzerdefinierten Metadaten zum Steuern der Dokumentextrahierung](#CustomMetadataControl).

- Standardmäßige Blob-Metadateneigenschaften werden in die folgenden Felder extrahiert:

	- **metadata\_storage\_name** (Edm.String): Der Dateiname des Blobs. Für ein Blob mit dem Namen „/my-container/my-folder/subfolder/resume.pdf“ lautet der Wert dieses Felds beispielsweise `resume.pdf`.

	- **metadata\_storage\_path** (Edm.String): Der vollständige URI des Blobs, einschließlich Speicherkonto. Beispiel: `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

	- **metadata\_storage\_content\_type** (Edm.String): Inhaltstyp, der mit dem Code angegeben wird, den Sie zum Hochladen des Blobs verwendet haben. Beispiel: `application/octet-stream`.

	- **metadata\_storage\_last\_modified** (Edm.DateTimeOffset): Zeitstempel der letzten Änderung für das Blob. Von Azure Search wird dieser Zeitstempel zum Identifizieren geänderter Blobs verwendet, um die erneute Indizierung aller Elemente nach der ersten Indizierung zu vermeiden.

	- **metadata\_storage\_size** (Edm.Int64): Blob-Größe in Byte.

	- **metadata\_storage\_content\_md5** (Edm.String): MD5-Hash des Blob-Inhalts, falls verfügbar.

- Die Metadateneigenschaften der einzelnen Dokumentformate werden in die [hier](#ContentSpecificMetadata) aufgeführten Felder extrahiert.

Es ist nicht erforderlich, Felder für alle obigen Eigenschaften in Ihrem Suchindex zu definieren. Erfassen Sie einfach die Eigenschaften, die Sie für Ihre Anwendung benötigen.

> [AZURE.NOTE] Die Feldnamen im vorhandenen Index unterscheiden sich von den Feldnamen, die während der Dokumentextrahierung generiert werden. Sie können **Feldzuordnungen** verwenden, um die von Azure Search bereitgestellten Eigenschaftennamen den Feldnamen in Ihrem Suchindex zuzuordnen. Weiter unten finden Sie ein Beispiel für die Verwendung von Feldzuordnungen.

## Auswählen des Dokumentschlüsselfelds und Behandeln unterschiedlicher Feldnamen

In Azure Search wird ein Dokument mit dem Dokumentschlüssel eindeutig identifiziert. Jeder Suchindex muss über genau ein Schlüsselfeld vom Typ „Edm.String“ verfügen. Das Schlüsselfeld ist für jedes Dokument erforderlich, das dem Index hinzugefügt wird (es ist auch das einzige erforderliche Feld).
   
Sie sollten sorgfältig abwägen, welches extrahierte Feld Sie dem Schlüsselfeld für Ihren Index zuordnen. Die Kandidaten lauten:

- **metadata\_storage\_name**: Dies ist ggf. ein passender Kandidat. Beachten Sie aber, dass 1) die Namen unter Umständen nicht eindeutig sind, falls Blobs mit dem gleichen Namen in unterschiedlichen Ordnern enthalten sind, und 2) der Name Zeichen enthalten kann, die in Dokumentschlüsseln ungültig sind, z. B. Bindestriche. Als Lösung für ungültige Zeichen können Sie die Option `base64EncodeKeys` in den Indexereigenschaften aktivieren. Denken Sie in diesem Fall daran, die Dokumentschlüssel zu codieren, wenn Sie sie in API-Aufrufen übergeben, z. B. bei einem Lookup. (Unter .NET können Sie hierfür beispielsweise die [UrlTokenEncode-Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) verwenden.)

- **metadata\_storage\_path**: Die Verwendung des vollständigen Pfads sorgt für Eindeutigkeit, aber der Pfad enthält auf jeden Fall `/`-Zeichen, die [in einem Dokumentschlüssel ungültig sind](https://msdn.microsoft.com/library/azure/dn857353.aspx). Wie oben auch, haben Sie hierbei die Möglichkeit, die Schlüssel mit der Option `base64EncodeKeys` zu codieren.

- Falls keine der Optionen für Sie geeignet ist, können Sie sehr flexibel vorgehen und den Blobs eine benutzerdefinierte Metadateneigenschaft hinzufügen. Bei dieser Option ist es aber erforderlich, dass diese Metadateneigenschaft im Rahmen des Blob-Uploadvorgangs allen Blobs hinzugefügt wird. Da der Schlüssel eine erforderliche Eigenschaft ist, tritt für alle Blobs, die nicht über diese Eigenschaft verfügen, beim Indizieren ein Fehler auf.

> [AZURE.IMPORTANT] Wenn keine explizite Zuordnung für das Schlüsselfeld im Index vorhanden ist, verwendet Azure Search automatisch `metadata_storage_path` (zweite Option oben) als Schlüssel und aktiviert die Base64-Codierung der Schlüssel.

Für dieses Beispiel wählen wir das Feld `metadata_storage_name` als Dokumentschlüssel aus. Nehmen wir außerdem an, dass Ihr Index über ein Schlüsselfeld mit dem Namen `key` und ein Feld `fileSize` zum Speichern der Dokumentgröße verfügt. Um alles wie gewünscht einzurichten, geben Sie die folgenden Feldzuordnungen an, wenn Sie den Indexer erstellen oder aktualisieren:

	"fieldMappings" : [
	  { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key" },
	  { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
	]

Hier wird beschrieben, wie Sie Feldzuordnungen hinzufügen und die Base64-Codierung von Schlüsseln für einen Indexer aktivieren, um alle Elemente zu verknüpfen:

	PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  "dataSourceName" : " blob-datasource ",
	  "targetIndexName" : "my-target-index",
	  "schedule" : { "interval" : "PT2H" },
	  "fieldMappings" : [
	    { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key" },
	    { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
	  ],
	  "parameters" : { "base64EncodeKeys": true }
	}

> [AZURE.NOTE] Weitere Informationen zu Feldzuordnungen finden Sie in [diesem Artikel](search-indexer-field-mappings.md).

## Inkrementelle Indizierung und Erkennung von Löschungen

Wenn Sie einen Blob-Indexer zur Ausführung nach einem Zeitplan einrichten, werden nur die geänderten Blobs neu indiziert. Dies wird anhand des `LastModified`-Zeitstempels der Blobs ermittelt.

> [AZURE.NOTE] Sie müssen keine Richtlinie zum Erkennen von Änderungen angeben. Die inkrementelle Indizierung wird für Sie automatisch indiziert.

Um anzugeben, dass bestimmte Dokumente aus dem Index entfernt werden müssen, sollten Sie die Strategie „Vorläufiges Löschen“ verwenden. Anstatt die entsprechenden Blobs zu löschen, fügen Sie eine benutzerdefinierte Metadateneigenschaft hinzu, um anzugeben, dass die gelöscht sind. Außerdem richten Sie für die Datenquelle eine Richtlinie zur Erkennung des vorläufigen Löschens ein.

> [AZURE.WARNING] Wenn Sie keine Richtlinie zum Erkennen von Löschungen verwenden, sondern nur die Blobs löschen, werden entsprechende Dokumente nicht aus dem Suchindex entfernt.

Bei der unten angegebenen Richtlinie wird ein Blob beispielsweise als gelöscht angesehen, wenn es über die Metadateneigenschaft `IsDeleted` mit dem Wert `true` verfügt:

	PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
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

<a name="ContentSpecificMetadata"></a>
## Inhaltstypspezifische Metadateneigenschaften

In der folgenden Tabelle sind die Verarbeitungsschritte für jedes Dokumentformat zusammengefasst, und es werden die Metadateneigenschaften beschrieben, die von Azure Search extrahiert werden.

Dokumentformat/Inhaltstyp | Inhaltstypspezifische Metadateneigenschaften | Verarbeitungsdetails
-------------------------------|-------------------------------------------|-------------------
HTML (`text/html`) | `metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` | Entfernen von HTML-Markup und Extrahieren von Text
PDF (`application/pdf`) | `metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title`| Extrahieren von Text, z. B. eingebettete Dokumente (mit Ausnahme von Bildern)
DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Extrahieren von Text, z. B. eingebettete Dokumente
DOC (application/msword) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Extrahieren von Text, z. B. eingebettete Dokumente
XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Extrahieren von Text, z. B. eingebettete Dokumente
XLS (application/vnd.ms-excel) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Extrahieren von Text, z. B. eingebettete Dokumente
PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Extrahieren von Text, z. B. eingebettete Dokumente
PPT (application/vnd.ms-powerpoint) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Extrahieren von Text, z. B. eingebettete Dokumente
MSG (application/vnd.ms-outlook) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` | Extrahieren von Text, einschließlich Anlagen
ZIP (application/zip) | `metadata_content_type` | Extrahieren von Text aus allen Dokumenten im Archiv
XML (application/xml) | `metadata_content_type`</br>`metadata_content_encoding`</br> | Entfernen von XML-Markup und Extrahieren von Text
JSON (application/json) | `metadata_content_type`</br>`metadata_content_encoding` | Extrahieren von Text<br/>HINWEIS: Wenn Sie mehrere Felder des Dokuments aus einem JSON-Blob extrahieren möchten, helfen Ihnen die detaillierten Informationen unter [Indizierung der JSON-Blobs](search-howto-index-json-blobs.md) weiter.
EML (message/rfc822) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` | Extrahieren von Text, einschließlich Anlagen
Nur-Text (text/plain) | `metadata_content_type`</br>`metadata_content_encoding`</br> | 

<a name="CustomMetadataControl"></a>
## Verwenden von benutzerdefinierten Metadaten zum Steuern der Dokumentextrahierung

Sie können einem Blob Metadateneigenschaften hinzufügen, um bestimmte Aspekte der Blobindizierung und des Prozesses der Dokumentextrahierung zu steuern. Derzeit werden die folgenden Eigenschaften unterstützt:

Eigenschaftenname | Eigenschaftswert | Erklärung
--------------|----------------|------------
AzureSearch\_Skip | „true“ | Weist den Blobindexer an, das Blob vollständig zu überspringen. Es wird nicht versucht, Metadaten oder Inhalt zu extrahieren. Dies ist nützlich, wenn Sie bestimmte Inhaltstypen überspringen möchten oder wenn ein bestimmtes Blob wiederholt fehlschlägt und den Indizierungsprozess unterbricht.
AzureSearch\_SkipContent | „true“ | Weist den Blobindexer an, nur die Metadaten zu indizieren und das Extrahieren von Inhalten des Blobs zu überspringen. Dies ist hilfreich, wenn der Blobinhalt nicht weiter relevant ist, Sie aber dennoch die an das Blob angefügten Metadaten indizieren möchten.

<a name="IndexerParametersConfigurationControl"></a>
## Verwenden von Indexerparametern zum Steuern des Extrahierens von Dokumenten

Mehrere Konfigurationsparameter stehen für den Indexer zur Verfügung, mit denen gesteuert werden kann, welche Blobs und welche Teile des Inhalts und der Metadaten eines Blobs indiziert werden.

### Indizieren von Blobs mit bestimmten Dateierweiterungen

Sie können nur die Blobs mit den Dateinamenerweiterungen indizieren, die Sie über den `indexedFileNameExtensions`-Konfigurationsparameter des Indexers angeben. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einem vorangestellten Punkt). Um beispielsweise nur die PDF- und DOCX-Blobs zu indizieren, gehen Sie folgendermaßen vor:

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
	}

### Ausschließen von Blobs mit bestimmten Dateierweiterungen von der Indizierung

Mithilfe des `excludedFileNameExtensions`-Konfigurationsparameters können Sie verhindern, dass Blobs mit bestimmten Dateinamenerweiterungen indiziert werden. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einem vorangestellten Punkt). Um beispielsweise alle Blobs mit Ausnahme von Blobs mit den Erweiterungen PNG und JPEG zu indizieren, gehen Sie folgendermaßen vor:

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
	}

Wenn sowohl der `indexedFileNameExtensions`- als auch der `excludedFileNameExtensions`-Parameter vorhanden ist, untersucht Azure Search zunächst `indexedFileNameExtensions` und danach `excludedFileNameExtensions`. Das heißt, wenn die gleiche Dateierweiterung in beiden Listen vorhanden ist, wird sie von der Indizierung ausgeschlossen.

### Indizieren von Speichermetadaten

Mithilfe der Konfigurationseigenschaft `indexStorageMetadataOnly` können Sie nur die Speichermetadaten indizieren und den Prozess der Dokumentextrahierung vollständig überspringen. Dies ist hilfreich, wenn Sie weder den Inhalt des Dokuments noch die inhaltstypspezifischen Metadateneigenschaften benötigen. Legen Sie dazu die `indexStorageMetadataOnly`-Eigenschaft auf `true` fest:

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "indexStorageMetadataOnly" : true } }
	}

### Indizieren von Speicher- und Inhaltstyp-Metadaten und Überspringen der Inhaltsextrahierung

Falls Sie alle Metadaten extrahieren, aber die Inhaltsextraktion für alle Blobs überspringen möchten, können Sie dieses Verhalten durch die Indexerkonfiguration anfordern, anstatt jedem einzelnen Blob `AzureSearch_SkipContent`-Metadaten hinzuzufügen. Legen Sie zu diesem Zweck die `skipContent`-Konfigurationseigenschaft des Indexers auf `true` fest:

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "skipContent" : true } }
	}

## Helfen Sie uns bei der Verbesserung von Azure Search

Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/) mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.

<!---HONumber=AcomDC_0817_2016-->