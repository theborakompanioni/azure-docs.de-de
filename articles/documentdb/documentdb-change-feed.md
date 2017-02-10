---
title: "Verwenden der Unterstützung von Änderungsfeeds in Azure DocumentDB | Microsoft-Dokumentation"
description: "Verwenden Sie die Azure DocumentDB-Unterstützung von Änderungsfeeds, um Änderungen in DocumentDB-Dokumenten nachzuverfolgen, eine ereignisbasierte Verarbeitung (wie mit Triggern) durchzuführen und Caches und Analysesysteme auf dem neuesten Stand zu halten."
keywords: "Änderungsfeed"
services: documentdb
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 12/13/2016
ms.author: b-hoedid
translationtype: Human Translation
ms.sourcegitcommit: b22e75264345bc9d155bd1abc1fdb6e978dfad04
ms.openlocfilehash: bafc50750381616ecf30c4e41090f342d82007f9


---
# <a name="working-with-the-change-feed-support-in-azure-documentdb"></a>Verwenden der Unterstützung von Änderungsfeeds in Azure DocumentDB
[Azure DocumentDB](documentdb-introduction.md) ist ein schneller und flexibler NoSQL-Datenbankdienst, der zum Speichern von großen Mengen von Transaktionsdaten und operativen Daten mit einer planbaren Latenz im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge verwendet wird. Dadurch eignet sich die Lösung besonders gut für IoT-, Spiele-, Einzelhandels- und operative Protokollierungsanwendungen. Ein gängiges Entwurfsmuster in diesen Anwendungen ist, Änderungen an DocumentDB-Daten nachzuverfolgen und materialisierte Sichten zu aktualisieren, Echtzeitanalysen auszuführen, Daten in Cold Storage zu archivieren und Benachrichtigungen aufgrund bestimmter Ereignisse basierend auf diesen Änderungen auszulösen. Die DocumentDB-Unterstützung von **Änderungsfeeds** ermöglicht Ihnen das Erstellen effizienter und skalierbarer Lösungen für jedes dieser Muster.

Durch die Unterstützung von Änderungsfeeds bietet DocumentDB eine Liste von Dokumenten in einer DocumentDB-Sammlung, die in der Reihenfolge sortiert ist, in der die Dokumente geändert wurden. Dieser Feed kann verwendet werden, um die Sammlung auf Änderungen an Daten zu überwachen und Aktionen wie die folgenden auszuführen:

* Auslösen eines Aufrufs einer API, wenn ein Dokument eingefügt oder geändert wird
* Ausführen der Verarbeitung in Echtzeit (Datenstrom) bei Aktualisierungen
* Synchronisieren von Daten mit einem Cache, einer Suchmaschine oder einem Data Warehouse

Änderungen in DocumentDB bleiben erhalten und können asynchron verarbeitet werden und über einen oder mehrere Consumer für eine parallele Verarbeitung verteilt werden. Betrachten wir nun die APIs für den Änderungsfeed und wie Sie sie zum Erstellen von skalierbaren Echtzeitanwendungen verwenden können.

![Verwenden des DocumentDB-Änderungsfeeds für Echtzeitanalysen und ereignisgesteuerte Computingszenarien](./media/documentdb-change-feed/changefeed.png)

## <a name="use-cases-and-scenarios"></a>Anwendungsfälle und -szenarien
Ein Änderungsfeed ermöglicht eine effiziente Verarbeitung großer Datasets mit vielen Schreibvorgängen und bietet eine Alternative zum Abfragen des gesamten Datasets, um Änderungen zu identifizieren. Sie können beispielsweise die folgenden Aufgaben effizient ausführen:

* Aktualisieren eines Caches, eines Suchindex oder eines Data Warehouse mit in Azure DocumentDB gespeicherten Daten.
* Implementieren von Datentiering und -archivierung auf Anwendungsebene, d.h. Speichern von „aktiven Daten“ in DocumentDB und Auslagern von „inaktiven Daten“ in [Azure Blob Storage](../storage/storage-introduction.md) oder [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Implementieren von Batchanalysen der Daten unter Verwendung von [Apache Hadoop](documentdb-run-hadoop-with-hdinsight.md).
* Implementieren von [Lambda-Pipelines in Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) mit DocumentDB. DocumentDB bietet eine skalierbare Datenbanklösung, die die Erfassung und Abfrage verarbeiten kann, und implementiert Lambda-Architekturen mit geringen Gesamtkosten. 
* Ausführen von Migrationen zu einem anderen Azure DocumentDB-Konto mit einem anderen Partitionierungsschema ohne Ausfallzeit.

**Lambda-Pipelines mit Azure DocumentDB für die Erfassung und Abfrage:**

![Azure DocumentDB-basierte Lambda-Pipeline für die Erfassung und Abfrage](./media/documentdb-change-feed/lambda.png)

Sie können DocumentDB zum Empfangen und Speichern von Ereignisdaten von Geräten, Sensoren, Infrastrukturen und Anwendungen verwenden und diese Ereignisse in Echtzeit mit [Azure Stream Analytics](documentdb-search-indexer.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md) oder [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md) verarbeiten. 

In Web-Apps und mobilen Apps können Sie Ereignisse wie Änderungen am Profil, an den Voreinstellungen oder am Speicherort des Kunden nachverfolgen, um bestimmte Aktionen wie das Senden von Pushbenachrichtigungen an deren Geräte mit [Azure Functions](../azure-functions/functions-bindings-documentdb.md) oder [App Services](https://azure.microsoft.com/services/app-service/) auszulösen. Wenn Sie DocumentDB zum Erstellen eines Spiels verwenden, können Sie den Änderungsfeed beispielsweise verwenden, um in Echtzeit Bestenlisten anhand der Ergebnisse von abgeschlossenen Spiele zu implementieren.

## <a name="how-change-feed-works-in-azure-documentdb"></a>Funktionsweise von Änderungsfeeds in Azure DocumentDB
DocumentDB bietet die Möglichkeit, Aktualisierungen an einer DocumentDB-Sammlung inkrementell zu lesen. Dieser Änderungsfeed weist die folgenden Eigenschaften auf:

* Änderungen bleiben in DocumentDB erhalten und können asynchron verarbeitet werden.
* Änderungen an Dokumenten in einer Sammlung sind sofort im Änderungsfeed verfügbar.
* Jede Änderung an einem Dokument wird nur einmal im Änderungsfeed angezeigt. Nur die letzte Änderung für ein bestimmtes Dokument ist im Änderungsprotokoll enthalten. Zwischenzeitliche Änderungen sind möglicherweise nicht verfügbar.
* Der Änderungsfeed ist anhand der Reihenfolge der Änderungen innerhalb der einzelnen Partitionsschlüsselwerte sortiert. Es gibt keine festgelegte Reihenfolge über Partitionsschlüsselwerte hinweg.
* Änderungen können für jeden Zeitpunkt synchronisiert werden, d.h., es gibt keine feste Datenaufbewahrungsdauer, in der die Änderungen verfügbar sind.
* Änderungen sind in Abschnitten von Partitionsschlüsselbereichen verfügbar. Auf diese Weise können Änderungen aus umfangreichen Sammlungen parallel von mehreren Consumern/Servern verarbeitet werden.
* Anwendungen können mehrere Änderungsfeeds gleichzeitig für dieselbe Sammlung anfordern.

Der Änderungsfeed in DocumentDB ist standardmäßig für alle Konten aktiviert und verursacht keine zusätzlichen Kosten für Ihr Konto. Sie können Ihren [bereitgestellten Durchsatz](documentdb-request-units.md) in Ihrer Schreibregion oder einer [Leseregion](documentdb-distribute-data-globally.md) zum Lesen aus dem Änderungsfeed verwenden, so wie es bei allen anderen Vorgängen von DocumentDB möglich ist. Der Änderungsfeed enthält Einfüge- und Aktualisierungsvorgänge, die an Dokumenten in der Sammlung vorgenommen wurden. Sie können Löschvorgänge durch ein Kennzeichen für „vorläufiges Löschen“ erfassen, dass den gelöschten Text in Ihren Dokumenten ersetzt. Alternativ können Sie über die [TTL-Funktion](documentdb-time-to-live.md) einen begrenzten Ablaufzeitraum für Ihre Dokumente festlegen, z.B. 24 Stunden. Verwenden Sie den Wert dieser Eigenschaft, um Löschvorgänge zu erfassen. Mit dieser Lösung müssen Sie Änderungen in einem kürzeren Zeitraum als der TTL-Ablaufzeitraum verarbeiten. Der Änderungsfeed ist für jeden Partitionsschlüsselbereich innerhalb der Dokumentsammlung verfügbar. Daher kann er für eine parallele Verarbeitung über einen oder mehrere Consumer verteilt werden. 

![Verteilte Verarbeitung des DocumentDB-Änderungsfeeds](./media/documentdb-change-feed/changefeedvisual.png)

Im folgenden Abschnitt wird der Zugriff auf den Änderungsfeed unter Verwendung der DocumentDB-REST-API und des SDKs beschrieben.

## <a name="working-with-the-rest-api-and-sdk"></a>Arbeiten mit der REST-API und dem SDK
DocumentDB bietet elastische Container für Speicher und Durchsatz, die **Sammlungen** genannt werden. Daten in Sammlungen sind logisch mit [Partitionsschlüsseln](documentdb-partition-data.md) gruppiert, um Skalierbarkeit und eine bessere Leistung zu ermöglichen. DocumentDB bietet verschiedene APIs für den Zugriff auf diese Daten, einschließlich Suchvorgänge anhand der ID (Lesen/Abrufen), Abfragen und Lesefeeds (Scans). Sie können einen Änderungsfeed erhalten, indem Sie zwei neue Anforderungsheader in die `ReadDocumentFeed`-API von DocumentDB aufnehmen. Der Änderungsfeed kann über Partitionsschlüsselbereiche hinweg parallel verarbeitet werden.

### <a name="readdocumentfeed-api"></a>ReadDocumentFeed-API
Betrachten wir nun kurz die Funktionsweise von ReadDocumentFeed. DocumentDB unterstützt über die `ReadDocumentFeed`-API das Lesen eines Feeds von Dokumenten in einer Sammlung. Die folgende Anforderung gibt z.B. eine Seite von Dokumenten in der `serverlogs`-Sammlung zurück. 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/smallcoll HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Die Ergebnisse können mithilfe des `x-ms-max-item-count`-Headers begrenzt werden, und Lesevorgänge können fortgesetzt werden, indem die Anforderung mit einem `x-ms-continuation`-Header, der in der vorherigen Antwort zurückgegeben wurde, erneut übermittelt wird. Bei der Ausführung über einen einzigen Client durchläuft `ReadDocumentFeed` die Ergebnisse aller Partitionen seriell. 

**Serieller Dokumentlesefeed**

![Serielle Ausführung von ReadDocumentFeed in DocumentDB](./media/documentdb-change-feed/readfeedserial.png)

Sie können den Feed von Dokumenten auch mithilfe eines der unterstützten [DocumentDB-SDKs](documentdb-sdk-dotnet.md) abrufen. Der folgende Codeausschnitt zeigt z.B., wie ReadDocumentFeed in .NET ausgeführt wird.

    FeedResponse<dynamic> feedResponse = null;
    do
    {
        feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
    }
    while (feedResponse.ResponseContinuation != null);

> [!NOTE]
> Der Änderungsfeed erfordert SDK-Versionen 1.11.0 und höher (zurzeit in privater Vorschau verfügbar).

### <a name="distributed-execution-of-readdocumentfeed"></a>Verteilte Ausführung von ReadDocumentFeed
Bei Sammlungen, die Terabytes von Daten oder mehr enthalten bzw. viele Aktualisierungen erfassen, ist die serielle Ausführung von Lesefeeds über einen einzelnen Clientcomputer möglicherweise nicht praktikabel. Um diese Big Data-Szenarien zu unterstützen, stellt DocumentDB APIs zum transparenten Verteilen von `ReadDocumentFeed`-Aufrufen über mehrere Leser/Consumer von Clients bereit. 

**Verteilter Dokumentlesefeed**

![Verteilte Ausführung von ReadDocumentFeed in DocumentDB](./media/documentdb-change-feed/readfeedparallel.png)

Für die skalierbare Verarbeitung von inkrementellen Änderungen unterstützt DocumentDB ein Modell mit horizontaler Hochskalierung für die Änderungsfeed-API basierend auf Partitionsschlüsselbereichen.

* Sie können eine Liste der Partitionsschlüsselbereiche für eine Sammlung durch Ausführen eines `ReadPartitionKeyRanges`-Aufrufs abrufen. 
* Für jeden Partitionsschlüsselbereich können Sie `ReadDocumentFeed` ausführen, um die Dokumente mit Partitionsschlüsseln innerhalb dieses Bereichs zu lesen.

### <a name="retrieving-partition-key-ranges-for-a-collection"></a>Abrufen von Partitionsschlüsselbereichen für eine Sammlung
Sie können die Partitionsschlüsselbereiche abrufen, indem Sie die `pkranges`-Ressource innerhalb einer Sammlung anfordern. Die folgende Anforderung ruft z.B. die Liste der Partitionsschlüsselbereiche für die `serverlogs`-Sammlung ab:

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

Diese Anforderung gibt die folgende Antwort mit Metadaten über die Partitionsschlüsselbereiche zurück:

    HTTP/1.1 200 Ok
    Content-Type: application/json
    x-ms-item-count: 25
    x-ms-schemaversion: 1.1
    Date: Tue, 15 Nov 2016 07:26:51 GMT

    {
       "_rid":"qYcAAPEvJBQ=",
       "PartitionKeyRanges":[
          {
             "_rid":"qYcAAPEvJBQCAAAAAAAAUA==",
             "id":"0",
             "_etag":"\"00002800-0000-0000-0000-580ac4ea0000\"",
             "minInclusive":"",
             "maxExclusive":"05C1CFFFFFFFF8",
             "_self":"dbs\/qYcAAA==\/colls\/qYcAAPEvJBQ=\/pkranges\/qYcAAPEvJBQCAAAAAAAAUA==\/",
             "_ts":1477100776
          },
          ...
       ],
       "_count": 25
    }


**Eigenschaften von Partitionsschlüsselbereichen**: Jeder Partitionsschlüsselbereich enthält die in der folgenden Tabelle aufgeführten Metadateneigenschaften:

<table>
    <tr>
        <th>Headername</th>
        <th>Beschreibung</th>
    </tr>
    <tr>
        <td>id</td>
        <td>
            <p>Die ID des Partitionsschlüsselbereichs. Dies ist eine beständige und eindeutige ID innerhalb jeder Sammlung.</p>
            <p>Sie muss im anschließenden Aufruf verwendet werden, um die Änderungen nach Partitionsschlüsselbereich zu lesen.</p>
        </td>
    </tr>
    <tr>
        <td>maxExclusive</td>
        <td>Der maximale Partitionsschlüssel-Hashwert für den Partitionsschlüsselbereich. Zur internen Verwendung.</td>
    </tr>
    <tr>
        <td>minInclusive</td>
        <td>Der minimale Partitionsschlüssel-Hashwert für den Partitionsschlüsselbereich. Zur internen Verwendung.</td>
    </tr>       
</table>

Sie können eines der unterstützten [DocumentDB-SDKs](documentdb-sdk-dotnet.md) verwenden. Der folgende Codeausschnitt zeigt z.B., wie Partitionsschlüsselbereiche in .NET abgerufen werden.

    List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();
    FeedResponse<PartitionKeyRange> response;

    do
    {
        response = await client.ReadPartitionKeyRangeFeedAsync(collection);
        partitionKeyRanges.AddRange(response);
    }
    while (response.ResponseContinuation != null);

DocumentDB unterstützt das Abrufen von Dokumenten pro Partitionsschlüsselbereich durch Festlegen des optionalen `x-ms-documentdb-partitionkeyrangeid`-Headers. 

### <a name="performing-an-incremental-readdocumentfeed"></a>Ausführen eines inkrementellen ReadDocumentFeed
ReadDocumentFeed unterstützt die folgenden Szenarien/Aufgaben für die inkrementelle Verarbeitung von Änderungen in den DocumentDB-Sammlungen:

* Lesen aller Änderungen an Dokumenten von Beginn an, also ab der Sammlungserstellung.
* Lesen aller Änderungen an zukünftigen Aktualisierungen von Dokumenten ab dem aktuellen Zeitpunkt.
* Lesen aller Änderungen an Dokumenten aus einer logischen Version der Sammlung (ETag). Sie können Prüfpunkte für Ihre Consumer basierend auf dem zurückgegebenen ETag aus inkrementellen Lesefeedanforderungen einrichten.

Zu den Änderungen gehören Einfügungen und Aktualisierungen in Dokumenten. Um Löschvorgänge zu erfassen, müssen Sie eine Eigenschaft für „vorläufiges Löschen“ in Ihren Dokumenten verwenden. Sie können aber auch die [integrierte TTL-Eigenschaft](documentdb-time-to-live.md) nutzen, um eine ausstehende Löschung im Änderungsfeed anzugeben.

Die folgende Tabelle enthält die Anforderungs- und Antwortheader für ReadDocumentFeed-Vorgänge.

**Anforderungsheader für einen inkrementellen ReadDocumentFeed**:

<table>
    <tr>
        <th>Headername</th>
        <th>Beschreibung</th>
    </tr>
    <tr>
        <td>A-IM</td>
        <td>Muss auf „Incremental feed“ festgelegt sein oder wird ignoriert</td>
    </tr>
    <tr>
        <td>If-None-Match</td>
        <td>
            <p>Kein Header: Gibt alle Änderungen von Beginn an (Sammlungserstellung) zurück</p>
            <p>"*": Gibt alle neuen Änderungen an Daten in der Sammlung zurück</p>
            <p>&lt;ETag&gt;: Bei Angabe des Sammlung-ETags werden alle Änderungen seit diesem logischen Zeitstempel zurückgegeben</p>
        </td>
    </tr>
    <tr>
        <td>x-ms-documentdb-partitionkeyrangeid</td>
        <td>Die ID des Partitionsschlüsselbereichs zum Lesen von Daten.</td>
    </tr>
</table>

**Antwortheader für einen inkrementellen ReadDocumentFeed**:

<table>
    <tr>
        <th>Headername</th>
        <th>Beschreibung</th>
    </tr>
    <tr>
        <td>etag</td>
        <td>
            <p>Die logische Sequenznummer (LSN) des letzten Dokuments, das in der Antwort zurückgegeben wurde.</p>
            <p>Der inkrementelle ReadDocumentFeed kann durch erneutes Übermitteln dieses Werts in „If-None-Match“ fortgesetzt werden.</p>
        </td>
    </tr>
</table>

Im Folgenden finden Sie eine Beispielanforderung, mit der alle inkrementellen Änderungen in der Sammlung aus der logischen Version/dem ETag `28535` und dem Partitionsschlüsselbereich `16` zurückgegeben werden:

    GET https://mydocumentdb.documents.azure.com/dbs/bigdb/colls/bigcoll/docs HTTP/1.1
    x-ms-max-item-count: 1
    If-None-Match: "28535"
    A-IM: Incremental feed
    x-ms-documentdb-partitionkeyrangeid: 16
    x-ms-date: Tue, 22 Nov 2016 20:43:01 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dzdpL2QQ8TCfiNbW%2fEcT88JHNvWeCgDA8gWeRZ%2btfN5o%3d
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Die Änderungen sind nach Zeit innerhalb jedes Partitionsschlüsselwerts innerhalb des Partitionsschlüsselbereichs sortiert. Es gibt keine festgelegte Reihenfolge über Partitionsschlüsselwerte hinweg. Wenn mehr Ergebnisse vorhanden sind, als auf eine einzelne Seite passen, können Sie die nächste Seite der Ergebnisse lesen, indem Sie die Anforderung mit dem `If-None-Match`-Header mit dem gleichen Wert für `etag` wie in der vorherigen Antwort erneut übermitteln. Wenn mehrere Dokumente in Bezug auf Transaktionen innerhalb einer gespeicherten Prozedur oder eines Triggers aktualisiert wurden, werden sie alle auf der gleichen Antwortseite zurückgegeben.

Das .NET SDK bietet die Hilfsklassen `CreateDocumentChangeFeedQuery` und `ChangeFeedOptions`, um auf Änderungen an einer Sammlung zuzugreifen. Der folgende Codeausschnitt zeigt, wie alle Änderungen seit dem Beginn mit dem .NET SDK von einem einzigen Client abgerufen werden.

    private async Task<Dictionary<string, string>> GetChanges(
        DocumentClient client,
        string collection,
        Dictionary<string, string> checkpoints)
    {
        List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();
        FeedResponse<PartitionKeyRange> pkRangesResponse;

        do
        {
            pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(collection);
            partitionKeyRanges.AddRange(pkRangesResponse);
        }
        while (pkRangesResponse.ResponseContinuation != null);

        foreach (PartitionKeyRange pkRange in partitionKeyRanges)
        {
            string continuation = null;
            checkpoints.TryGetValue(pkRange.Id, out continuation);

            IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
                collection,
                new ChangeFeedOptions
                {
                    PartitionKeyRangeId = pkRange.Id,
                    StartFromBeginning = true,
                    RequestContinuation = continuation,
                    MaxItemCount = 1
                });

            while (query.HasMoreResults)
            {
                FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

                foreach (DeviceReading changedDocument in readChangesResponse)
                {
                    Console.WriteLine(changedDocument.Id);
                }

                checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
            }
        }

        return checkpoints;
    }

Und der folgende Codeausschnitt zeigt, wie die Änderungen in Echtzeit mit DocumentDB mithilfe der Unterstützung von Änderungsfeeds und der vorhergehenden Funktion verarbeitet werden. Der erste Aufruf gibt alle Dokumente in der Sammlung zurück, und der zweite gibt nur die beiden Dokumente zurück, die seit dem letzten Prüfpunkt erstellt wurden.

    // Returns all documents in the collection.
    Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

    await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
    await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

    // Returns only the two documents created above.
    checkpoints = await GetChanges(client, collection, checkpoints);


Sie können den Änderungsfeed auch mithilfe von clientseitiger Logik filtern, um die Ereignisse selektiv zu verarbeiten. Im Folgenden sehen Sie z.B. einen Codeausschnitt, der eine clientseitige LINQ verwendet, um nur Temperaturänderungsereignisse von Gerätesensoren zu verarbeiten.

    FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

    foreach (DeviceReading changedDocument in 
        readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
    {
        // trigger an action, like call an API
    }

Dieser Artikel enthält eine exemplarische Vorgehensweise zur DocumentDB-Unterstützung von Änderungsfeeds. Zudem wird das Nachverfolgen von Änderungen an DocumentDB-Daten mithilfe der DocumentDB-REST-API und/oder des SDKs erläutert. 

## <a name="next-steps"></a>Nächste Schritte
* Probieren Sie die [Codebeispiele für den DocumentDB-Änderungsfeed auf Github](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed) aus.
* Erfahren Sie mehr über [das Ressourcenmodell und die Hierarchie von DocumentDB](documentdb-resources.md).
* Lernen Sie die Codierung mit den [DocumentDB SDKs](documentdb-sdk-dotnet.md) oder der [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) kennen.


<!--HONumber=Dec16_HO2-->


