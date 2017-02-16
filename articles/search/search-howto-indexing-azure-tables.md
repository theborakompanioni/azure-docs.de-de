---
title: Indizieren von Azure Table Storage mit Azure Search
description: Erfahren Sie, wie in Azure Tables gespeicherte Daten mit Azure Search indiziert werden.
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 1cc27411-d0cc-40ed-8aed-c7cb9ab402b9
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/15/2016
ms.author: eugenesh
translationtype: Human Translation
ms.sourcegitcommit: 714045750ab16364ecd1095f1f346d3da1d4c4a5
ms.openlocfilehash: 4bfcf719cb071a28421c64dbb4d6c132f45ba9f9

---

# <a name="indexing-azure-table-storage-with-azure-search"></a>Indizieren von Azure Table Storage mit Azure Search
In diesem Artikel wird beschrieben, wie Sie Azure Search zum Indizieren von Daten verwenden, die in Azure Table Storage gespeichert sind. Mit dem neuen Azure Search-Indexer für Tabellen verläuft dieser Prozess schnell und reibungslos.

## <a name="setting-up-azure-table-indexing"></a>Einrichten der Azure-Tabellenindizierung
Für die Einrichtung und Konfiguration eines Azure-Indexers für Tabellen können Sie die Azure Search-REST-API nutzen, um **Indexer** und **Datenquellen** wie unter [Indizierungsvorgänge](https://msdn.microsoft.com/library/azure/dn946891.aspx) beschrieben zu erstellen und zu verwalten. Sie können auch die [Version 2.0-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) des .NET SDK verwenden. In Zukunft wird die Tabellenindizierung vom Azure-Portal unterstützt.

Eine Datenquelle gibt an, welche Daten indiziert werden müssen. Sie legt außerdem die Anmeldeinformationen für den Zugriff auf die Daten sowie die Richtlinien zur Aktivierung von Azure Search fest, um Änderungen an den Daten effizient identifizieren zu können (z. B. neue, geänderte oder gelöschte Zeilen).

Mit einem Indexer werden Daten aus einer Datenquelle gelesen und in einen Zielsuchindex geladen.

So richten Sie die Tabellenindizierung ein:

1. Erstellen einer Datenquelle
   * Legen Sie den `type`-Parameter auf `azuretable` fest.
   * Übergeben Sie die Verbindungszeichenfolge des Speicherkontos als `credentials.connectionString`-Parameter. Sie können die Verbindungszeichenfolge über das Azure-Portal abrufen, indem Sie auf dem Blatt des Speicherkontos zu **Einstellungen** > **Schlüssel** (für klassische Speicherkonten) oder zu **Einstellungen** > **Zugriffsschlüssel** (für ARM-Speicherkonten) navigieren. Beachten Sie, dass Azure Search momentan keine Shared Access Signature-Anmeldeinformationen unterstützt. Wenn Sie SAS verwenden möchten, stimmen Sie für [diesen UserVoice-Vorschlag](https://feedback.azure.com/forums/263029-azure-search/suggestions/12368244-support-shared-access-signature-for-blob-datasourc).
   * Geben Sie den Tabellennamen mit dem Parameter `container.name` an.
   * Geben Sie optional eine Abfrage mit dem Parameter `container.query` an. Verwenden Sie nach Möglichkeit einen Filter für PartitionKey, um die beste Leistung zu erzielen. Alle anderen Abfragen führen zu einem vollständigen Tabellenscan, wodurch bei großen Tabellen die Leistung beeinträchtigt werden kann.
2. Erstellen Sie einen Suchindex mit dem Schema, das den Spalten in der Tabelle entspricht, die Sie indizieren möchten.
3. Erstellen Sie den Indexer, indem Sie die Datenquelle mit dem Suchindex verbinden.

### <a name="create-data-source"></a>Erstellen der Datenquelle
    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Weitere Informationen über die API zum Erstellen einer Datenquelle finden Sie unter [Datenquelle erstellen](https://msdn.microsoft.com/library/azure/dn946876.aspx).

### <a name="create-index"></a>Erstellen des Index
    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Weitere Informationen zur API zum Erstellen eines Index finden Sie unter [Create Index](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Erstellen eines Indexers
Zuletzt erstellen Sie den Indexer, der auf die Datenquelle und den Zielindex verweist. Beispiel:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Indexer erstellen](https://msdn.microsoft.com/library/azure/dn946899.aspx).

Das ist schon alles. Viel Spaß beim Indizieren!

## <a name="dealing-with-different-field-names"></a>Behandeln von unterschiedlichen Feldnamen
Die Feldnamen in Ihrem vorhandenen Index unterscheiden sich häufig von den Eigenschaftennamen in Ihrer Tabelle. Sie können **Feldzuordnungen** verwenden, um die Eigenschaftennamen der Tabelle den Feldnamen in Ihrem Suchindex zuzuordnen. Weitere Informationen zu Feldzuordnungen finden Sie unter [Durch Azure Search-Indexerfeldzuordnungen werden die Unterschiede zwischen Datenquellen und Suchindizes überbrückt](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Behandeln von Dokumentschlüsseln
In Azure Search wird ein Dokument mit dem Dokumentschlüssel eindeutig identifiziert. Jeder Suchindex muss über genau ein Schlüsselfeld vom Typ `Edm.String`verfügen. Das Schlüsselfeld ist für jedes Dokument erforderlich, das dem Index hinzugefügt wird (es ist gleichzeitig das einzige erforderliche Feld).

Da Tabellenzeilen über einen Verbundschlüssel verfügen, generiert Azure Search ein synthetisches Feld mit dem Namen `Key` , bei dem es sich um eine Verkettung von Partitionsschlüssel- und Zeilenschlüsselwerten handelt. Wenn der Partitionsschlüssel einer Zeile beispielsweise `PK1` lautet, und der Zeilenschlüssel den Wert `RK1` hat, hat das Feld `Key` den Wert `PK1RK1`.

> [!NOTE]
> Der Wert von `Key` kann unter Umständen Zeichen enthalten, die in Dokumentschlüsseln ungültig sind, z.B. Bindestriche. Ungültige Zeichen können Sie mit der `base64Encode`-[Feldzuordnungsfunktion](search-indexer-field-mappings.md#base64EncodeFunction) behandeln. Verwenden Sie in diesem Fall auch die URL-sichere Base64-Codierung beim Übergeben von Dokumentschlüsseln in API-Aufrufen (z.B. Suche).
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Inkrementelle Indizierung und Erkennung von Löschungen
Wenn Sie für einen Tabellenindexer die Ausführung nach einem Zeitplan einrichten, werden nur neue oder aktualisierte Zeilen neu indiziert. Dies wird durch den `Timestamp`-Wert einer Zeile bestimmt. Sie müssen keine Richtlinie zum Erkennen von Änderungen angeben. Die inkrementelle Indizierung wird für Sie automatisch aktiviert.

Um anzugeben, dass bestimmte Dokumente aus dem Index entfernt werden müssen, können Sie die Strategie „Vorläufiges Löschen“ verwenden. Anstatt eine Zeile zu löschen, fügen Sie eine Eigenschaft hinzu, um anzugeben, dass sie gelöscht wurde. Außerdem richten Sie für die Datenquelle eine Richtlinie zur Erkennung des vorläufigen Löschens ein. Bei der unten angegebenen Richtlinie wird eine Zeile beispielsweise als gelöscht angesehen, wenn sie über die `IsDeleted`-Eigenschaft mit dem Wert `"true"` verfügt:

    PUT https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns bei der Verbesserung von Azure Search
Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/)mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.



<!--HONumber=Dec16_HO3-->


