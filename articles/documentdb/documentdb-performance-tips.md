---
title: "Tipps zur Leistungssteigerung für DocumentDB | Microsoft Docs"
description: Machen Sie sich mit Clientkonfigurationsoptionen zur Verbesserung der Leistung von Azure DocumentDB-Datenbanken vertraut.
keywords: 'So wird&quot;s gemacht: Verbessern der Datenbankleistung'
services: documentdb
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 94ff155e-f9bc-488f-8c7a-5e7037091bb9
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 2d833a559b72569983340972ba3b905b9e42e61d
ms.openlocfilehash: 5b4efb2d6dedb43436745f5e8055cae44e4a58ac


---
# <a name="performance-tips-for-documentdb"></a>Tipps zur Leistungssteigerung für DocumentDB
Azure DocumentDB ist eine schnelle und flexible verteilte Datenbank mit nahtloser Skalierung, garantierter Latenz und garantiertem Durchsatz. Die Skalierung Ihrer Datenbank mit DocumentDB erfordert keine aufwendigen Änderungen an der Architektur und keine Erstellung von komplexem Code. Zentrales Hoch- und Herunterskalieren ist ebenso problemlos möglich wie das Aufrufen einer einzelnen API oder [SDK-Methode](documentdb-performance-levels.md#changing-performance-levels-using-the-net-sdk). Da der Zugriff auf DocumentDB jedoch über Netzwerkaufrufe erfolgt, können Sie clientseitige Optimierungen vornehmen, um eine optimale Leistung zu erzielen.

Im Anschluss finden Sie einige Optionen zur Optimierung der Datenbankleistung:

## <a name="networking"></a>Netzwerk
<a id="direct-connection"></a>

1. **Verbindungsrichtlinie: Verwenden des direkten Verbindungsmodus**

    Die Art der Verbindungsherstellung zwischen einem Client und Azure DocumentDB hat erhebliche Auswirkungen auf die Leistung – insbesondere im Hinblick auf die clientseitige Latenz. Für die Konfiguration der Verbindungsrichtlinie stehen zwei wichtige Konfigurationseinstellungen zur Verfügung: der Verbindungs*modus* und das [Verbindungs*protokoll*](#connection-protocol).  Folgende Modi sind verfügbar:

   1. Gatewaymodus (Standard)
   2. Direkter Modus

      Der Gatewaymodus wird auf allen SDK-Plattformen unterstützt und ist als Standardoption konfiguriert.  Wenn Ihre Anwendung in einem Unternehmensnetzwerk mit strengen Firewalleinschränkungen ausgeführt wird, ist der Gatewaymodus die beste Wahl, da er den HTTPS-Standardport und einen einzelnen Endpunkt verwendet. Im Gatewaymodus ist jedoch jeweils ein zusätzlicher Netzwerkhop erforderlich, wenn Daten in DocumentDB geschrieben oder daraus gelesen werden, was sich negativ auf die Leistung auswirkt.   Aus diesem Grund bietet der direkte Modus die bessere Leistung, da weniger Netzwerkhops erforderlich sind.
2. **Verbindungsrichtlinie: Verwenden des TCP-Protokolls**

    Bei Verwendung des direkten Modus stehen zwei Protokolloptionen zur Verfügung:

   * TCP
   * HTTPS

     DocumentDB bietet ein einfaches und offenes RESTful-Programmiermodell über HTTPS. Darüber hinaus ist ein effizientes TCP-Protokoll vorhanden, das ebenfalls über ein RESTful-Kommunikationsmodell verfügt und über das .NET-Client-SDK verfügbar ist. Sowohl Direct TCP als auch HTTPS nutzen SSL für die erste Authentifizierung und Verschlüsselung des Datenverkehrs. Die beste Leistung erzielen Sie mit dem TCP-Protokoll.

     Bei der Verwendung von TCP im Gatewaymodus ist TCP-Port 443 der DocumentDB-Port und 10250 der MongoDB-API-Port. Wenn Sie TCP im direkten Modus verwenden, müssen Sie zusätzlich zu den Gatewayports sicherstellen, dass der Portbereich zwischen 10000 und 20000 offen ist, da DocumentDB dynamische TCP-Ports verwendet. Wenn diese Ports nicht geöffnet sind und Sie versuchen, TCP zu verwenden, wird der Fehler „503 - Dienst nicht verfügbar“ angezeigt.

     Der Verbindungsmodus wird im Zuge der Erstellung der DocumentClient-Instanz mit dem ConnectionPolicy-Parameter konfiguriert. Bei Verwendung des direkten Modus kann das Protokoll auch innerhalb des ConnectionPolicy-Parameters festgelegt werden.

         var serviceEndpoint = new Uri("https://contoso.documents.net");
         var authKey = new "your authKey from Azure Mngt Portal";
         DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
         new ConnectionPolicy
         {

             ConnectionMode = ConnectionMode.Direct,
             ConnectionProtocol = Protocol.Tcp
         });

     Da TCP nur im direkten Modus unterstützt wird, wird bei Verwendung des Gatewaymodus immer das HTTPS-Protokoll für die Kommunikation mit dem Gateway verwendet, und der Protokollwert des ConnectionPolicy-Parameters wird ignoriert.

     ![Abbildung der DocumentDB-Verbindungsrichtlinie](./media/documentdb-performance-tips/azure-documentdb-connection-policy.png)
3. **Aufrufen von OpenAsync, um die Startlatenz bei der ersten Anforderung zu vermeiden**

    Aufgrund des erforderlichen Abrufs der Adressroutingtabelle tritt bei der ersten Anforderung standardmäßig eine höhere Latenz auf. Um diese Startlatenz bei der ersten Anforderung zu vermeiden, empfiehlt es sich, während der Initialisierung einmal „OpenAsync()“ aufzurufen:

        await client.OpenAsync();
   <a id="same-region"></a>
4. **Platzieren der Clients in der gleichen Azure-Region**

    Platzieren Sie nach Möglichkeit sämtliche Anwendungen, die DocumentDB aufrufen, in der gleichen Region wie die DocumentDB-Datenbank. DocumentDB-Aufrufe aus derselben Azure-Region werden normalerweise innerhalb von ca. 1 bis 2 ms abgeschlossen, während die Latenz zwischen der Ost- und Westküste der USA mehr als 50 ms beträgt. Diese Latenz variiert ggf. von Anforderung zu Anforderung und ist abhängig von der Route, die die Anforderung zwischen dem Client und der Grenze des Azure-Datencenters nimmt. Die geringste Latenz erzielen Sie, wenn sich die aufrufende Anwendung in der gleichen Azure-Region wie der bereitgestellte DocumentDB-Endpunkt befindet. Eine Liste mit den verfügbaren Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services).

    ![Abbildung der DocumentDB-Verbindungsrichtlinie](./media/documentdb-performance-tips/azure-documentdb-same-region.png)
   <a id="increase-threads"></a>
5. **Erhöhen der Anzahl von Threads/Aufgaben**

    Da DocumentDB-Aufrufe über das Netzwerk abgewickelt werden, müssen Sie ggf. den Parallelitätsgrad Ihrer Anforderungen variieren, um die Wartezeit für die Clientanwendung zwischen Anforderungen auf ein Minimum zu reduzieren. Erstellen Sie beispielsweise bei Verwendung der [Task Parallel Library](https://msdn.microsoft.com//library/dd460717.aspx) von .NET mehrere hundert Aufgaben für Lese- und Schreibvorgänge in DocumentDB.

## <a name="sdk-usage"></a>SDK-Verwendung
1. **Installieren des neuesten SDKs**

    Die DocumentDB-SDKs werden kontinuierlich verbessert, um die bestmögliche Leistung zu erzielen. Informationen zum neuesten SDK und zu den Verbesserungen finden Sie auf den Seiten unter [DocumentDB SDK](documentdb-sdk-dotnet.md) .
2. **Verwenden eines Singleton-DocumentDB-Clients für die Lebensdauer der Anwendung**

    Beachten Sie, dass bei Verwendung des direkten Modus jede DocumentClient-Instanz threadsicher ist und über eine effiziente Verbindungsverwaltung und Adressenzwischenspeicherung verfügt. Zur Ermöglichung einer effizienten Verbindungsverwaltung und einer besseren DocumentClient-Leistung empfiehlt es sich, für die Lebensdauer der Anwendung pro Anwendungsdomäne eine einzelne DocumentClient-Instanz zu verwenden.
   <a id="max-connection"></a>
3. **Erhöhen von „System.Net MaxConnections“ pro Host**

    Anforderungen an DocumentDB erfolgen standardmäßig über HTTPS/REST und unterliegen dem Standardverbindungslimit pro Hostname oder IP-Adresse. Unter Umständen müssen Sie einen höheren MaxConnections-Wert festlegen (100 - 1.000), damit die Clientbibliothek mehrere Verbindungen zu DocumentDB gleichzeitig nutzen kann. Ab .NET SDK 1.8.0 ist der Standardwert für [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) auf 50 festgelegt. Zur Erhöhung dieses Werts können Sie [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/en-us/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) auf einen höheren Wert festlegen.  
4. **Optimieren von parallelen Abfragen für partitionierte Sammlungen**

     Ab Version 1.9.0 des DocumentDB .NET SDKs werden parallele Abfragen unterstützt, mit denen Sie eine partitionierte Sammlung parallel abfragen können. (Weitere Informationen finden Sie unter [Arbeiten mit den SDKs](documentdb-partition-data.md#working-with-the-sdks) und in den dazugehörigen [Codebeispielen](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs).) Parallele Abfragen sind darauf ausgelegt, Latenz und Durchsatz im Vergleich mit seriellen Abfragen zu verbessern. Parallele Abfragen verfügen über zwei Parameter, die Benutzer optimieren können, um sie an ihre Anforderungen anzupassen: (a) MaxDegreeOfParallelism zum Steuern der maximalen Anzahl von Partitionen, die parallel abgefragt werden können, und (b) MaxBufferedItemCount zum Steuern der Anzahl von vorab abgerufenen Ergebnissen.

    (a) ***Optimieren von MaxDegreeOfParallelism\:***
    Bei parallelen Abfragen werden mehrere Partitionen parallel abgefragt. Die Daten einer individuell partitionierten Sammlung werden in Bezug auf die Abfrage aber seriell abgerufen. Wenn Sie MaxDegreeOfParallelism also auf die Anzahl von Partitionen festlegen, ist die Wahrscheinlichkeit am höchsten, dass die bestmögliche Leistung für die Abfrage erzielt wird (vorausgesetzt, alle anderen Systembedingungen bleiben unverändert). Falls Ihnen die Anzahl von Partitionen nicht bekannt ist, können Sie MaxDegreeOfParallelism auf einen hohen Wert festlegen. Das System wählt für MaxDegreeOfParallelism dann den minimalen Wert aus (Anzahl von Partitionen, Benutzereingabe).

    Es ist wichtig zu beachten, dass sich für parallele Abfragen die größten Vorteile ergeben, wenn die Daten in Bezug auf die Abfrage gleichmäßig auf alle Partitionen verteilt werden. Wenn die partitionierte Auflistung so partitioniert ist, dass sich alle Daten bzw. die meisten Daten, die von einer Abfrage zurückgegeben werden, auf einigen wenigen Partitionen befinden (schlimmstenfalls nur auf einer Partition), können aufgrund dieser Partitionierung Engpässe bei der Leistung auftreten.

    (b) ***Optimieren von Tuning MaxBufferedItemCount\:***
    Die parallele Abfrage ist so konzipiert, dass Ergebnisse vorab abgerufen werden, während der Client den aktuellen Batch mit Ergebnissen verarbeitet. Diese Art des Abrufs führt zu einer Verbesserung der Latenz einer Abfrage. MaxBufferedItemCount ist der Parameter zum Begrenzen der Menge von vorab abgerufenen Ergebnissen. Wenn Sie MaxBufferedItemCount auf die erwartete Anzahl von zurückgegebenen Ergebnissen (oder eine höhere Anzahl) festlegen, ist der Vorteil durch das vorherige Abrufen für die Abfrage am größten.

    Beachten Sie, dass das vorherige Abrufen unabhängig von MaxDegreeOfParallelism funktioniert und dass nur ein Puffer für die Daten aller Partitionen vorhanden ist.  
5. **Aktivieren der serverseitigen Garbage Collection**

    In einigen Fällen kann es hilfreich sein, die Häufigkeit zu verringern, mit der die Garbage Collection ausgeführt wird. Legen Sie [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) in .NET auf „True“ fest.
6. **Implementieren eines Backoffs in RetryAfter-Intervallen**

    Es empfiehlt sich, die Last während Leistungstests so lange erhöhen, bis eine geringe Menge von Anforderungen gedrosselt wird. Wenn es sich um eine gedrosselte Anwendung handelt, sollte die Clientanwendung diese Drosselung für das vom Server angegebene Wiederholungsintervall aussetzen. Durch das Aussetzen wird die geringstmögliche Wartezeit zwischen den Wiederholungsversuchen gewährleistet. Wiederholungsrichtlinien werden ab Version 1.8.0 von DocumentDB ([.NET](documentdb-sdk-dotnet.md) und [Java](documentdb-sdk-java.md)) bzw. ab Version 1.9.0 ([Node.js](documentdb-sdk-node.md) und [Python](documentdb-sdk-python.md)) und in allen unterstützten Versionen der [.NET Core](documentdb-sdk-dotnet-core.md)-SDKs unterstützt. Weitere Informationen finden Sie unter [Überschreiten von Grenzwerten für den reservierten Durchsatz](documentdb-request-units.md#RequestRateTooLarge) sowie unter [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
7. **Horizontales Hochskalieren Ihrer Clientworkload**

    Wenn Sie auf einem hohen Durchsatzniveau testen (> 50.000 RU/s), kann sich die Clientanwendung als Engpass erweisen, da der Computer die CPU- oder Netzwerknutzung deckelt. Wenn dieser Punkt erreicht wird, können Sie das DocumentDB-Konto weiter auslasten, indem Sie Ihre Clientanwendungen auf mehrere Server horizontal hochskalieren.
8. **Zwischenspeichern von Dokument-URIs zur Verringerung der Latenz bei Lesevorgängen**

    Zur Erzielung einer optimalen Leseleistung sollten Dokument-URIs möglichst immer zwischengespeichert werden.
   <a id="tune-page-size"></a>
9. **Optimieren der Seitengröße für Abfragen/Lesefeeds, um die Leistung zu verbessern**

    Wenn mehrere Dokumente mithilfe der Lesefeedfunktion (ReadDocumentFeedAsync) gleichzeitig gelesen werden oder eine DocumentDB-SQL-Abfrage ausgegeben wird, werden die Ergebnisse bei der Rückgabe segmentiert, falls das Resultset zu groß ist. Ergebnisse werden standardmäßig in Blöcken mit je 100 Elementen oder 1 MB zurückgegeben (je nachdem, welcher Grenzwert zuerst erreicht wird).

    Mithilfe des Anforderungsheaders „x-ms-max-item-count“ können Sie die Seitengröße auf bis zu 1.000 erhöhen und so die Anzahl von Netzwerkroundtrips verringern, die zum Abrufen aller entsprechenden Ergebnisse erforderlich sind. Falls nur einige wenige Ergebnisse angezeigt werden müssen (etwa, wenn von der Benutzeroberfläche oder Anwendungs-API lediglich zehn Ergebnisse zurückgegeben werden), können Sie die Seitengröße auch auf 10 verringern, um den durch Lese- und Abfragevorgänge beanspruchten Durchsatz zu reduzieren.

    Die Seitengröße kann auch mithilfe der verfügbaren DocumentDB-SDKs festgelegt werden.  Beispiel:

        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
10. **Erhöhen der Anzahl von Threads/Aufgaben**

    Siehe [Erhöhen der Anzahl von Threads/Aufgaben](#increase-threads) im Abschnitt „Netzwerk“.

## <a name="indexing-policy"></a>Indizierungsrichtlinien
1. **Verwenden der verzögerten Indizierung zur Beschleunigung der Erfassung zu Spitzenzeiten**

    DocumentDB ermöglicht die Angabe einer Indizierungsrichtlinie (auf Sammlungsebene), sodass Sie wählen können, ob Dokumente in einer Sammlung automatisch indiziert werden sollen.  Darüber hinaus können Sie zwischen synchronen (konsistenten) und asynchronen (verzögerten) Indexaktualisierungen wählen. Der Index wird bei jedem Einfügen, Ersetzen oder Löschen eines Dokuments der Sammlung standardmäßig synchron aktualisiert. Im synchronen Modus können die Abfragen die gleiche [Konsistenzebene](documentdb-consistency-levels.md) wie bei Dokumentlesevorgängen verwenden – ganz ohne Verzögerung durch den Aufholbedarf des Index.

    Die verzögerte Indizierung empfiehlt sich unter Umständen für Szenarien, in denen Daten schubweise geschrieben werden und sich der Arbeitsaufwand für die Indizierung von Inhalten über einen längeren Zeitraum bezahlt machen soll. Mit der verzögerten Indizierung können Sie den bereitgestellten Durchsatz effektiv nutzen und Schreibanforderungen zu Spitzenzeiten mit minimaler Latenz verarbeiten. Wichtig: Bei aktivierter verzögerter Indizierung sind Abfrageergebnisse unabhängig von der für das DocumentDB-Konto konfigurierten Konsistenzebene letztendlich konsistent.

    Daher fällt im konsistenten Indizierungsmodus („IndexingPolicy.IndexingMode“ auf „Consistent“ festgelegt) pro Schreibvorgang die höchste Belastung für Anforderungseinheiten an, während im verzögerten Indizierungsmodus („IndexingPolicy.IndexingMode“ auf „Lazy“ festgelegt) sowie bei deaktivierter Indizierung („IndexingPolicy.Automatic“ auf „False“ festgelegt) zum Zeitpunkt des Schreibvorgangs keinerlei Indizierungskosten anfallen.
2. **Beschleunigen von Schreibvorgängen durch Ausschließen nicht verwendeter Pfade von der Indizierung**

    Die Indizierungsrichtlinie von DocumentDB ermöglicht auch die Verwendung von Indizierungspfaden („IndexingPolicy.IncludedPaths“ und „IndexingPolicy.ExcludedPaths“), um anzugeben, welche Dokumentpfade in die Indizierung ein- bzw. von der Indizierung ausgeschlossen werden sollen.  Die Verwendung von Indizierungspfaden kann in Szenarien, in denen die Abfragemuster im Voraus bekannt sind, die Leistung bei Schreibvorgängen verbessern und den Indexspeicher verringern, da die Indizierungskosten direkt damit zusammenhängen, wie viele individuelle Pfade indiziert werden. Der folgende Code zeigt beispielsweise, wie Sie mithilfe des Platzhalterzeichens „*“ einen gesamten Abschnitt von Dokumenten (eine Unterstruktur) von der Indizierung ausschließen.

        var collection = new DocumentCollection { Id = "excludedPathCollection" };
        collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
        collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
        collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);

    Weitere Informationen finden Sie unter [Indizierungsrichtlinien für DocumentDB](documentdb-indexing-policies.md).

## <a name="throughput"></a>Durchsatz
<a id="measure-rus"></a>

1. **Messen und Optimieren (Senken) der Anzahl von Anforderungseinheiten pro Sekunde**

    DocumentDB bietet vielfältige Datenbankvorgänge (einschließlich relationaler und hierarchischer Abfragen mit UDFs, gespeicherter Prozeduren und Trigger), die alle in den Dokumenten innerhalb einer Datenbanksammlung ausgeführt werden. Die Kosten im Zusammenhang mit diesen Vorgängen variieren basierend auf dem CPU-, E/A- und Speicheraufwand, der für den jeweiligen Vorgang erforderlich ist. Anstelle sich Gedanken über Hardwareressourcen und deren Verwaltung zu machen, können Sie sich eine Anforderungseinheit (RU) als alleinige Maßeinheit für die Ressourcen vorstellen, die für das Durchführen der verschiedenen Datenbankvorgänge und das Ausführen einer Anwendungsanforderung erforderlich sind.

    [Anforderungseinheiten](documentdb-request-units.md) werden für jedes Datenbankkonto basierend auf der Anzahl von erworbenen Kapazitätseinheiten bereitgestellt. Der Verbrauch von Anforderungseinheiten wird als Rate pro Sekunde bemessen. Anwendungen, die die bereitgestellte Anforderungseinheitenrate für das dazugehörige Konto übersteigen, werden begrenzt, bis die Rate unter das reservierte Niveau für das Konto fällt. Wenn die Anwendung einen höheren Durchsatz benötigt, können Sie zusätzliche Kapazitätseinheiten erwerben.

    Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten für einen Vorgang verbraucht werden. Die Anzahl von Prädikaten, die Art der Prädikate, die Anzahl von UDFs und die Größe des Quelldatasets beeinflussen die Kosten von Abfragevorgängen.

    Untersuchen Sie zum Ermitteln des Indizierungsaufwands für einen beliebigen Vorgang (Erstellen, Aktualisieren oder Löschen) den Header „x-ms-request-charge“ (oder die entsprechende RequestCharge-Eigenschaft in „ResourceResponse<T>“ oder „FeedResponse<T>“ im .NET SDK), um die Anzahl von Anforderungseinheiten zu ermitteln, die von diesen Vorgängen genutzt werden.

        // Measure the performance (request units) of writes
        ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
        Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
        // Measure the performance (request units) of queries
        IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
        while (queryable.HasMoreResults)
             {
                  FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
                  Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
             }

    Bei der in diesem Header zurückgegebenen Anforderungsbelastung handelt es sich um einen Bruchteil Ihres bereitgestellten Durchsatzes (2.000 RUs/Sekunde). Falls die obige Abfrage also beispielsweise 1000 Dokumente mit einer Größe von 1 KB zurückgibt, fallen für den Vorgang Kosten in Höhe von 1000 an. Somit werden vom Server innerhalb einer Sekunde nur zwei solcher Anforderungen berücksichtigt, und weitere Anforderungen werden gedrosselt. Weitere Informationen finden Sie unter [Anforderungseinheiten in DocumentDB](documentdb-request-units.md) sowie unter dem [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner).
2. **Behandeln von Ratenbeschränkungen/zu hohen Anforderungsraten**

    Wenn ein Client versucht, den für ein Konto reservierten Durchsatz zu überschreiten, wird die Serverleistung nicht beeinträchtigt, und es wird kein über die reservierte Kapazität hinausgehender Durchsatz in Anspruch genommen. Der Server beendet die Anforderung vorab mit „RequestRateTooLarge“ (HTTP-Statuscode: 429) und gibt den Header „x-ms-retry-after-ms“ zurück. Darin ist die Zeitspanne (in Millisekunden) angegeben, die der Benutzer warten muss, bis ein neuer Anforderungsversuch unternommen werden kann.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    Alle SDKs fangen diese Antwort implizit ab, berücksichtigen den vom Server angegebenen Header vom Typ „retry-after“ und wiederholen die Anforderung. Wenn nicht mehrere Clients gleichzeitig auf Ihr Konto zugreifen, wird die nächste Wiederholung erfolgreich ausgeführt.

    Falls mehrere Clients kumulativ und kontinuierlich die Anforderungsrate überschreiten, reicht die intern vom Client festgelegte Standardanzahl von neun Wiederholungen unter Umständen nicht aus. In diesem Fall löst der Client für die Anwendung eine DocumentClientException mit dem Statuscode 429 aus. Die standardmäßige Wiederholungsanzahl kann durch Festlegen der Wiederholungsoptionen (RetryOptions) für die ConnectionPolicy-Instanz geändert werden. Die DocumentClientException mit dem Statuscode 429 wird standardmäßig nach einer kumulierten Wartezeit von 30 Sekunden zurückgegeben, wenn die Anforderung weiterhin die Anforderungsrate übersteigt. Dies gilt auch, wenn die aktuelle Wiederholungsanzahl unter der maximalen Wiederholungsanzahl liegt – ganz gleich, ob es sich dabei um den Standardwert (9) oder um einen benutzerdefinierten Wert handelt.

    Das automatisierte Wiederholungsverhalten trägt zwar bei den meisten Anwendungen zur Verbesserung der Resilienz und Nutzbarkeit bei, kann bei Leistungsbenchmarks aber auch hinderlich sein (insbesondere beim Ermitteln der Latenz).  Die Wartezeit für den Client nimmt stark zu, wenn das Experiment die Serverdrosselung erreicht und damit die automatische Wiederholung durch das Client-SDK auslöst. Ermitteln Sie zur Vermeidung von Latenzspitzenwerten bei Leistungsexperimenten die von den einzelnen Vorgängen zurückgegebene Belastung, und stellen Sie sicher, dass die Anforderungen die reservierte Anforderungsrate nicht überschreiten. Weitere Informationen finden Sie unter [Anforderungseinheiten in DocumentDB](documentdb-request-units.md).
3. **Konzipieren für kleinere Dokumente und höheren Durchsatz**

    Die Anforderungsbelastung (also die Kosten für die Anforderungsverarbeitung) eines Vorgangs hängt direkt mit der Größe des Dokuments zusammen. Vorgänge für große Dokumente sind teurer als Vorgänge für kleine Dateien.

## <a name="next-steps"></a>Nächste Schritte
Eine Beispielanwendung zur Evaluierung von DocumentDB für Hochleistungsszenarien auf einigen Clientcomputern finden Sie unter [Leistungs- und Skalierungstests mit Azure DocumentDB](documentdb-performance-testing.md).

Weitere Informationen zur Gestaltung einer auf Skalierung und hohe Leistung ausgelegten Anwendung finden Sie unter [Partitionieren und Skalieren von Daten in DocumentDB](documentdb-partition-data.md).



<!--HONumber=Nov16_HO3-->


