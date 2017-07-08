---
title: "Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB | Microsoft-Dokumentation"
description: "Verwenden Sie die Azure Cosmos DB-Unterstützung von Änderungsfeeds, um Änderungen in Dokumenten nachzuverfolgen, eine ereignisbasierte Verarbeitung (wie mit Triggern) durchzuführen und Caches und Analysesysteme auf dem neuesten Stand zu halten."
keywords: "Änderungsfeed"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 03/23/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: d04d1240fb353a973953b2a90eadc65705219edb
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB
[Azure Cosmos DB](../cosmos-db/introduction.md) ist ein schneller und flexibler, weltweit replizierter Datenbankdienst, der zum Speichern von großen Mengen von Transaktionsdaten und operativen Daten mit einer planbaren Latenz im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge verwendet wird. Dadurch eignet sich die Lösung besonders gut für IoT-, Spiele-, Einzelhandels- und operative Protokollierungsanwendungen. Ein gängiges Entwurfsmuster in diesen Anwendungen ist, Änderungen an Azure Cosmos DB-Daten nachzuverfolgen und materialisierte Sichten zu aktualisieren, Echtzeitanalysen auszuführen, Daten in Cold Storage zu archivieren und Benachrichtigungen aufgrund bestimmter Ereignisse basierend auf diesen Änderungen auszulösen. Die **Unterstützung von Änderungsfeeds** in Azure Cosmos DB ermöglicht Ihnen das Erstellen effizienter und skalierbarer Lösungen für jedes dieser Muster.

Durch die Unterstützung von Änderungsfeeds bietet Azure Cosmos DB eine Liste von Dokumenten in einer Azure Cosmos DB-Sammlung, die in der Reihenfolge sortiert ist, in der die Dokumente geändert wurden. Dieser Feed kann verwendet werden, um die Sammlung auf Änderungen an Daten zu überwachen und Aktionen wie die folgenden auszuführen:

* Auslösen eines Aufrufs einer API, wenn ein Dokument eingefügt oder geändert wird
* Ausführen der Verarbeitung in Echtzeit (Datenstrom) bei Aktualisierungen
* Synchronisieren von Daten mit einem Cache, einer Suchmaschine oder einem Data Warehouse

Änderungen in Azure Cosmos DB bleiben erhalten und können asynchron verarbeitet werden und über einen oder mehrere Consumer für eine parallele Verarbeitung verteilt werden. Betrachten wir nun die APIs für den Änderungsfeed und wie Sie sie zum Erstellen von skalierbaren Echtzeitanwendungen verwenden können. In diesem Artikel wird die Arbeit mit räumlichen Daten mit der DocumentDB-API für Azure Cosmos DB veranschaulicht. 

![Verwenden des Azure Cosmos DB-Änderungsfeeds für Echtzeitanalysen und ereignisgesteuerte Computingszenarien](./media/change-feed/changefeed.png)

## <a name="use-cases-and-scenarios"></a>Anwendungsfälle und -szenarien
Ein Änderungsfeed ermöglicht eine effiziente Verarbeitung großer Datasets mit vielen Schreibvorgängen und bietet eine Alternative zum Abfragen des gesamten Datasets, um Änderungen zu identifizieren. Sie können beispielsweise die folgenden Aufgaben effizient ausführen:

* Aktualisieren eines Caches, eines Suchindex oder eines Data Warehouse mit in Azure Cosmos DB gespeicherten Daten.
* Implementieren von Datentiering und -archivierung auf Anwendungsebene, d. h. Speichern von „aktiven Daten“ in Azure Cosmos DB und Auslagern von „inaktiven Daten“ in [Azure Blob Storage](../storage/storage-introduction.md) oder [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Implementieren von Batchanalysen der Daten unter Verwendung von [Apache Hadoop](run-hadoop-with-hdinsight.md).
* Implementieren von [Lambda-Pipelines in Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) mit Azure Cosmos DB. Azure Cosmos DB bietet eine skalierbare Datenbanklösung, die die Erfassung und Abfrage verarbeiten kann, und implementiert Lambda-Architekturen mit geringen Gesamtkosten. 
* Ausführen von Migrationen zu einem anderen Azure Cosmos DB-Konto mit einem anderen Partitionierungsschema ohne Ausfallzeit.

**Lambda-Pipelines mit Azure Cosmos DB für die Erfassung und Abfrage:**

![Azure Cosmos DB-basierte Lambda-Pipeline für die Erfassung und Abfrage](./media/change-feed/lambda.png)

Sie können Azure Cosmos DB zum Empfangen und Speichern von Ereignisdaten von Geräten, Sensoren, Infrastrukturen und Anwendungen verwenden und diese Ereignisse in Echtzeit mit [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md) oder [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md) verarbeiten. 

In Web-Apps und mobilen Apps können Sie Ereignisse wie Änderungen am Profil, an den Voreinstellungen oder am Speicherort des Kunden nachverfolgen, um bestimmte Aktionen wie das Senden von Pushbenachrichtigungen an deren Geräte mit [Azure Functions](../azure-functions/functions-bindings-documentdb.md) oder [App Services](https://azure.microsoft.com/services/app-service/) auszulösen. Wenn Sie Azure Cosmos DB zum Erstellen eines Spiels verwenden, können Sie den Änderungsfeed beispielsweise verwenden, um in Echtzeit Bestenlisten anhand der Ergebnisse von abgeschlossenen Spiele zu implementieren.

## <a name="how-change-feed-works-in-azure-cosmos-db"></a>Funktionsweise von Änderungsfeeds in Azure Cosmos DB
Azure Cosmos DB bietet die Möglichkeit, Aktualisierungen an einer Azure Cosmos DB-Sammlung inkrementell zu lesen. Dieser Änderungsfeed weist die folgenden Eigenschaften auf:

* Änderungen bleiben in Azure Cosmos DB erhalten und können asynchron verarbeitet werden.
* Änderungen an Dokumenten in einer Sammlung sind sofort im Änderungsfeed verfügbar.
* Jede Änderung an einem Dokument wird nur einmal im Änderungsfeed angezeigt. Nur die letzte Änderung für ein bestimmtes Dokument ist im Änderungsprotokoll enthalten. Zwischenzeitliche Änderungen sind möglicherweise nicht verfügbar.
* Der Änderungsfeed ist anhand der Reihenfolge der Änderungen innerhalb der einzelnen Partitionsschlüsselwerte sortiert. Es gibt keine festgelegte Reihenfolge über Partitionsschlüsselwerte hinweg.
* Änderungen können für jeden Zeitpunkt synchronisiert werden, d.h., es gibt keine feste Datenaufbewahrungsdauer, in der die Änderungen verfügbar sind.
* Änderungen sind in Abschnitten von Partitionsschlüsselbereichen verfügbar. Auf diese Weise können Änderungen aus umfangreichen Sammlungen parallel von mehreren Consumern/Servern verarbeitet werden.
* Anwendungen können mehrere Änderungsfeeds gleichzeitig für dieselbe Sammlung anfordern.

Der Änderungsfeed in Azure Cosmos DB ist standardmäßig für alle Konten aktiviert und verursacht keine zusätzlichen Kosten für Ihr Konto. Sie können Ihren [bereitgestellten Durchsatz](request-units.md) in Ihrer Schreibregion oder einer [Leseregion](distribute-data-globally.md) zum Lesen aus dem Änderungsfeed verwenden, so wie es bei allen anderen Vorgängen von Azure Cosmos DB möglich ist. Der Änderungsfeed enthält Einfüge- und Aktualisierungsvorgänge, die an Dokumenten in der Sammlung vorgenommen wurden. Sie können Löschvorgänge durch ein Kennzeichen für „vorläufiges Löschen“ erfassen, dass den gelöschten Text in Ihren Dokumenten ersetzt. Alternativ können Sie über die [TTL-Funktion](time-to-live.md) einen begrenzten Ablaufzeitraum für Ihre Dokumente festlegen, z.B. 24 Stunden. Verwenden Sie den Wert dieser Eigenschaft, um Löschvorgänge zu erfassen. Mit dieser Lösung müssen Sie Änderungen in einem kürzeren Zeitraum als der TTL-Ablaufzeitraum verarbeiten. Der Änderungsfeed ist für jeden Partitionsschlüsselbereich innerhalb der Dokumentsammlung verfügbar. Daher kann er für eine parallele Verarbeitung über einen oder mehrere Consumer verteilt werden. 

![Verteilte Verarbeitung des Azure Cosmos DB-Änderungsfeeds](./media/change-feed/changefeedvisual.png)

Im folgenden Abschnitt wird der Zugriff auf den Änderungsfeed unter Verwendung der Azure Cosmos DB-REST-API und des SDKs beschrieben. Für .NET-Anwendungen empfiehlt sich die Verwendung der [Change Feed Processor-Bibliothek]() zum Verarbeiten von Ereignissen aus dem Änderungsfeed.

## <a id="rest-apis"></a>Arbeiten mit der REST-API und dem SDK
Azure Cosmos DB bietet elastische Container für Speicher und Durchsatz, die **Sammlungen** genannt werden. Daten in Sammlungen sind logisch mit [Partitionsschlüsseln](partition-data.md) gruppiert, um Skalierbarkeit und eine bessere Leistung zu ermöglichen. Azure Cosmos DB bietet verschiedene APIs für den Zugriff auf diese Daten, einschließlich Suchvorgänge anhand der ID (Lesen/Abrufen), Abfragen und Lesefeeds (Scans). Sie können einen Änderungsfeed erhalten, indem Sie zwei neue Anforderungsheader in die DocumentDB-`ReadDocumentFeed`-API aufnehmen. Der Änderungsfeed kann über Partitionsschlüsselbereiche hinweg parallel verarbeitet werden.

### <a name="readdocumentfeed-api"></a>ReadDocumentFeed-API
Betrachten wir nun kurz die Funktionsweise von ReadDocumentFeed. Azure Cosmos DB unterstützt über die `ReadDocumentFeed`-API das Lesen eines Feeds von Dokumenten in einer Sammlung. Die folgende Anforderung gibt z.B. eine Seite von Dokumenten in der `serverlogs`-Sammlung zurück. 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
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

Sie können den Feed von Dokumenten auch mithilfe eines der unterstützten [Azure Cosmos DB-SDKs](documentdb-sdk-dotnet.md) abrufen. Der folgende Codeausschnitt zeigt z.B., wie ReadDocumentFeed in .NET ausgeführt wird.

    FeedResponse<dynamic> feedResponse = null;
    do
    {
        feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
    }
    while (feedResponse.ResponseContinuation != null);

### <a name="distributed-execution-of-readdocumentfeed"></a>Verteilte Ausführung von ReadDocumentFeed
Bei Sammlungen, die Terabytes von Daten oder mehr enthalten bzw. viele Aktualisierungen erfassen, ist die serielle Ausführung von Lesefeeds über einen einzelnen Clientcomputer möglicherweise nicht praktikabel. Um diese Big Data-Szenarien zu unterstützen, stellt Azure Cosmos DB APIs zum transparenten Verteilen von `ReadDocumentFeed`-Aufrufen über mehrere Leser/Consumer von Clients bereit. 

**Verteilter Dokumentlesefeed**

Für die skalierbare Verarbeitung von inkrementellen Änderungen unterstützt Azure Cosmos DB ein Modell mit horizontaler Hochskalierung für die Änderungsfeed-API basierend auf Partitionsschlüsselbereichen.

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

Sie können eines der unterstützten [Azure Cosmos DB-SDKs](documentdb-sdk-dotnet.md) verwenden. Der folgende Codeausschnitt zeigt z.B., wie Partitionsschlüsselbereiche in .NET abgerufen werden.

    string pkRangesResponseContinuation = null;
    List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

    do
    {
        FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
            collectionUri, 
            new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

        partitionKeyRanges.AddRange(pkRangesResponse);
        pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    }
    while (pkRangesResponseContinuation != null);

Azure Cosmos DB unterstützt das Abrufen von Dokumenten pro Partitionsschlüsselbereich durch Festlegen des optionalen `x-ms-documentdb-partitionkeyrangeid`-Headers. 

### <a name="performing-an-incremental-readdocumentfeed"></a>Ausführen eines inkrementellen ReadDocumentFeed
ReadDocumentFeed unterstützt die folgenden Szenarien/Aufgaben für die inkrementelle Verarbeitung von Änderungen in den Azure Cosmos DB-Sammlungen:

* Lesen aller Änderungen an Dokumenten von Beginn an, also ab der Sammlungserstellung.
* Lesen aller Änderungen an zukünftigen Aktualisierungen von Dokumenten ab dem aktuellen Zeitpunkt.
* Lesen aller Änderungen an Dokumenten aus einer logischen Version der Sammlung (ETag). Sie können Prüfpunkte für Ihre Consumer basierend auf dem zurückgegebenen ETag aus inkrementellen Lesefeedanforderungen einrichten.

Zu den Änderungen gehören Einfügungen und Aktualisierungen in Dokumenten. Um Löschvorgänge zu erfassen, müssen Sie eine Eigenschaft für „vorläufiges Löschen“ in Ihren Dokumenten verwenden. Sie können aber auch die [integrierte TTL-Eigenschaft](time-to-live.md) nutzen, um eine ausstehende Löschung im Änderungsfeed anzugeben.

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

Die Änderungen sind nach Zeit innerhalb jedes Partitionsschlüsselwerts innerhalb des Partitionsschlüsselbereichs sortiert. Es gibt keine festgelegte Reihenfolge über Partitionsschlüsselwerte hinweg. Wenn mehr Ergebnisse vorhanden sind, als auf eine einzelne Seite passen, können Sie die nächste Seite der Ergebnisse lesen, indem Sie die Anforderung mit dem `If-None-Match`-Header mit dem gleichen Wert für `etag` wie in der vorherigen Antwort erneut übermitteln. Wenn mehrere Dokumente in Bezug auf Transaktionen innerhalb einer gespeicherten Prozedur oder eines Triggers eingefügt oder aktualisiert wurden, werden sie alle auf der gleichen Antwortseite zurückgegeben.

> [!NOTE]
> Mit einem Änderungsfeed werden auf einer Seite möglicherweise mehr Elemente zurückgegeben als in `x-ms-max-item-count` angegeben wurde, falls mehrere Dokumente innerhalb einer gespeicherten Prozedur oder eines Triggers eingefügt oder aktualisiert wurden. 

Das .NET SDK enthält die Hilfsklassen [CreateDocumentChangeFeedQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) und [ChangeFeedOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.changefeedoptions.aspx) für den Zugriff auf die an einer Sammlung vorgenommenen Änderungen. Der folgende Codeausschnitt zeigt, wie alle Änderungen seit dem Beginn mit dem .NET SDK von einem einzigen Client abgerufen werden.

    private async Task<Dictionary<string, string>> GetChanges(
        DocumentClient client,
        string collection,
        Dictionary<string, string> checkpoints)
    {
        string pkRangesResponseContinuation = null;
        List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

        do
        {
            FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
                collectionUri, 
                new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

            partitionKeyRanges.AddRange(pkRangesResponse);
            pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
        }
        while (pkRangesResponseContinuation != null);

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

Und der folgende Codeausschnitt zeigt, wie die Änderungen in Echtzeit mit Azure Cosmos DB mithilfe der Unterstützung von Änderungsfeeds und der vorhergehenden Funktion verarbeitet werden. Der erste Aufruf gibt alle Dokumente in der Sammlung zurück, und der zweite gibt nur die beiden Dokumente zurück, die seit dem letzten Prüfpunkt erstellt wurden.

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

## <a id="change-feed-processor"></a>Change Feed Processor-Bibliothek
Die [Change Feed Processor-Bibliothek von Azure Cosmos DB](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/ChangeFeedProcessor) kann zum Verteilen der Ereignisverarbeitung vom Änderungsfeed auf mehrere Consumer verwendet werden. Sie sollten diese Implementierung verwenden, wenn Sie Leser für Änderungsfeeds auf der .NET-Plattform erstellen. Mit der `ChangeFeedProcessorHost`-Klasse wird eine threadsichere Laufzeitumgebung mit mehreren Prozessen für Ereignisprozessorimplementierungen bereitgestellt, die auch die Erstellung von Prüfpunkten und die Leaseverwaltung für Partitionen ermöglicht.

Zur Verwendung der [`ChangeFeedProcessorHost`](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/ChangeFeedProcessor/DocumentDB.ChangeFeedProcessor/ChangeFeedEventHost.cs)-Klasse können Sie [`IChangeFeedObserver`](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/ChangeFeedProcessor/DocumentDB.ChangeFeedProcessor/IChangeFeedObserver.cs) implementieren. Diese Schnittstelle enthält drei Methoden:

* OpenAsync
* CloseAsync
* ProcessEventsAsync

Instanziieren Sie zum Starten der Ereignisverarbeitung die ChangeFeedProcessorHost-Klasse, und geben Sie die entsprechenden Parameter für Ihre Azure Cosmos DB-Sammlung an. Rufen Sie anschließend `RegisterObserverAsync` auf, um Ihre `IChangeFeedObserver`-Implementierung bei der Runtime zu registrieren. An diesem Punkt versucht der Host, einen Lease für jeden Partitionsschlüsselbereich in der Azure Cosmos DB-Sammlung abzurufen, indem ein „gieriger“ Algorithmus verwendet wird. Diese Leases gelten für einen bestimmten Zeitraum und müssen anschließend erneuert werden. Wenn neue Knoten (hier: Workerinstanzen) in den Onlinezustand versetzt werden, geben sie Leasereservierungen heraus. Im Laufe der Zeit wird die Arbeitsauslastung dann auf die Knoten verteilt, da jeder Knoten versucht, mehr Leases zu erlangen.

![Verwenden des Change Feed Processor-Hosts von Azure Cosmos DB](./media/change-feed/changefeedprocessor.png)

Im Laufe der Zeit wird somit ein Gleichgewicht erreicht. Diese dynamische Funktion ermöglicht, dass die CPU-basierte automatische Skalierung sowohl beim zentralen Herunterskalieren als auch beim zentralen Hochskalieren auf Consumer angewendet wird. Wenn Änderungen in Azure Cosmos DB schneller verfügbar sind, als sie von den Consumern verarbeitet werden können, kann der CPU-Anstieg auf den Consumern verwendet werden, um eine automatische Skalierung in Bezug auf die Anzahl der Workerinstanzen auszulösen.

Die `ChangeFeedProcessorHost`-Klasse implementiert zudem unter Verwendung einer separaten Azure Cosmos DB-Sammlung von Leases ein Verfahren für die Prüfpunktausführung. Bei diesem Verfahren wird der Offset pro Partition gespeichert, damit jeder Consumer ermitteln kann, wie der letzte Prüfpunkt des vorherigen Consumers lautete. Da Partitionen per Lease zwischen Knoten wechseln, ist dies das Synchronisierungsverfahren, das die Auslastungsverteilung ermöglicht.


Dies ist ein Codeausschnitt für einen einfachen Change Feed Processor-Host, der Änderungen an der Konsole ausgibt:

```cs
    class DocumentFeedObserver : IChangeFeedObserver
    {
        private static int s_totalDocs = 0;
        public Task OpenAsync(ChangeFeedObserverContext context)
        {
            Console.WriteLine("Worker opened, {0}", context.PartitionKeyRangeId);
            return Task.CompletedTask;  // Requires targeting .NET 4.6+.
        }
        public Task CloseAsync(ChangeFeedObserverContext context, ChangeFeedObserverCloseReason reason)
        {
            Console.WriteLine("Worker closed, {0}", context.PartitionKeyRangeId);
            return Task.CompletedTask;
        }
        public Task ProcessEventsAsync(IReadOnlyList<Document> docs, ChangeFeedObserverContext context)
        {
            Console.WriteLine("Change feed: total {0} doc(s)", Interlocked.Add(ref s_totalDocs, docs.Count));
            return Task.CompletedTask;
        }
    }
```

Der folgende Codeausschnitt zeigt, wie Sie einen neuen Host registrieren, um auf Änderungen an einer Azure Cosmos DB-Sammlung zu lauschen. Hier konfigurieren wir eine separate Sammlung, um die Leases für Partitionen mehrerer Consumer zu verwalten:

```cs
    string hostName = Guid.NewGuid().ToString();
    DocumentCollectionInfo documentCollectionLocation = new DocumentCollectionInfo
    {
        Uri = new Uri("https://YOUR_SERVICE.documents.azure.com:443/"),
        MasterKey = "YOUR_SECRET_KEY==",
        DatabaseName = "db1",
        CollectionName = "documents"
    };

    DocumentCollectionInfo leaseCollectionLocation = new DocumentCollectionInfo
    {
        Uri = new Uri("https://YOUR_SERVICE.documents.azure.com:443/"),
        MasterKey = "YOUR_SECRET_KEY==",
        DatabaseName = "db1",
        CollectionName = "leases"
    };

    ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation);
    await host.RegisterObserverAsync<DocumentFeedObserver>();
```

Dieser Artikel enthält eine exemplarische Vorgehensweise zur Azure Cosmos DB-Unterstützung von Änderungsfeeds. Zudem wird das Nachverfolgen von Änderungen an Azure Cosmos DB-Daten mithilfe der REST-API und/oder des SDKs erläutert. 

## <a name="next-steps"></a>Nächste Schritte
* Probieren Sie die [Codebeispiele für den Azure Cosmos DB-Änderungsfeed auf GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed) aus.
* Lernen Sie die Codierung mit den [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md) oder der [REST-API](/rest/api/documentdb/) kennen.

