<properties 
	pageTitle="DocumentDB-Skalierung und -Leistung | Microsoft Azure" 
	description="Erfahren Sie, wie Sie Skalierungs- und Leistungstests mit Azure DocumentDB durchführen."
	keywords="Leistungstests"
	services="documentdb" 
	authors="arramac" 
	manager="jhubbard" 
	editor="" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/20/2016" 
	ms.author="arramac"/>

# Leistungs- und Skalierungstests mit Azure DocumentDB
Leistungs- und Skalierungstests sind ein wichtiger Schritt bei der Entwicklung von Anwendungen. Die Datenbankebene hat bei vielen Anwendungen einen erheblichen Einfluss auf die allgemeine Leistung und Skalierbarkeit und ist daher eine wichtige Komponente von Leistungstests. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) wurde speziell für die elastische Skalierung und vorhersagbare Leistung entwickelt und eignet sich deshalb hervorragend für Anwendungen, die eine leistungsstarke Datenbankebene benötigen.

Dieser Artikel ist eine Referenz für Entwickler, die Leistungstestsammlungen für ihre DocumentDB-Workloads implementieren oder DocumentDB für leistungsstarke Anwendungsszenarien auswerten. Er konzentriert sich hauptsächlich auf isolierte Leistungstests der Datenbank, enthält jedoch auch bewährte Methoden für Produktionsanwendungen.

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

- Wo kann ich eine Beispiel-.NET-Clientanwendung für Leistungstests von Azure DocumentDB finden?
- Welche Schlüsselfaktoren beeinflussen die End-to-End-Leistung von Anfragen, die an Azure DocumentDB gesendet werden? 
- Wie erreiche ich über meine Clientanwendung hohe Durchsätze mit Azure DocumentDB?

Sie können das Projekt aus dem [DocumentDB-Beispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark) herunterladen, um mit dem Programmieren loszulegen.

## Entscheidende Clientkonfigurationsoptionen
DocumentDB ist eine schnelle und flexibel verteilte Datenbank, die problemlos mit einer garantierten Latenzzeit und Durchsatz skaliert. Sie müssen keine tief greifenden Änderungen an der Architektur vornehmen oder komplexen Code schreiben, um Ihre Datenbankebene mit DocumentDB zu skalieren. Zentrales Hoch- und Herunterskalieren ist ebenso einfach wie das Aufrufen einer einzigen API oder SDK-Methode. Beim Testen der Skalierung ist es wichtig zu beachten, dass der Zugriff auf DocumentDB über Netzwerkaufrufe erfolgt. Wenn Sie eine eigenständige Clientanwendung schreiben, um die Leistung von DocumentDB zu testen, müssen Sie sie entsprechend konfigurieren, damit sie den Auswirkungen entgegenwirkt, die die Netzwerklatenz auf Ihre Leistungsmessungen hat.

Ziehen Sie die folgenden Clientkonfigurationsoptionen in Betracht, um die beste End-to-End-Leistung mit DocumentDB zu gewährleisten:

- **Die Anzahl der Threads/Aufgaben erhöhen:** Da DocumentDB über das Netzwerk aufgerufen wird, müssen Sie möglicherweise den Parallelitätsgrad Ihrer Anfragen variieren, sodass eine geringe Wartezeit für die Clientanwendung zwischen Anfragen anfällt. Wenn Sie beispielsweise die [Task Parallel Library](https://msdn.microsoft.com//library/dd460717.aspx) von .NET verwenden, erstellen Sie Aufgaben in 100er Größenordnungen, die Lese- und Schreibvorgänge in DocumentDB ausführen.
- **Innerhalb derselben Azure-Region testen:** Sofern dies möglich ist, sollten Sie den Test auf einem virtuellen Computer oder einem App Service ausführen, der in derselben Azure-Region bereitgestellt wird. DocumentDB-Aufrufe aus derselben Azure-Region werden schätzungsweise innerhalb von 1-2ms abgeschlossen, während die Latenz zwischen der Ost- und Westküste der USA mehr als 50 ms beträgt.
- **System.Net MaxConnections pro Host erhöhen**: DocumentDB wird standardmäßig über HTTPS/REST angefordert. Diese Anfragen unterliegen den standardmäßigen Verbindungsbeschränkungen pro Hostname oder IP-Adresse. Möglicherweise müssen Sie einen höheren Wert für die Beschränkungen festlegen (100-1.000), damit die Clientbibliothek mehrere Verbindungen zu DocumentDB gleichzeitig nutzen kann. In .NET ist das der Wert [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).
- **Die serverseitige automatische Speicherbereinigung aktivieren:** Die Reduzierung der Häufigkeit, mit der die automatische Speicherbereinigung ausgeführt wird, kann in einigen Fällen hilfreich sein. Legen Sie [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) in .NET auf TRUE fest.
- **Direkte Konnektivität über das TCP-Protokoll verwenden:** Verwenden Sie [direkte Konnektivität](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionmode.aspx) über das [TCP-Protokoll](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.protocol.aspx), um eine optimale Leistung zu erzielen. 
- **Backoff in RetryAfter-Intervallen implementieren**: Sie sollten die Last während der Leistungstests so lange erhöhen, bis eine kleine Menge von Anforderungen gedrosselt wird. Wenn es sich um eine gedrosselte Anwendung handelt, sollte die Clientanwendung diese Drosselung für das vom Server angegebene Wiederholungsintervall aussetzen. Dadurch wird die geringstmögliche Wartezeit zwischen den Wiederholungsversuchen gewährleistet. Weitere Informationen finden Sie unter [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
- **Die Arbeitslast Ihres Clients horizontal hochskalieren:** Wenn Sie auf einem hohen Durchsatzniveau testen (>50.000 RU/s), kann die Clientanwendung als Engpass fungieren, weil der Computer die CPU- oder Netzwerkauslastung kappt. Wenn dieser Punkt erreicht wird, können Sie das DocumentDB-Konto weiter auslasten, indem Sie Ihre Clientanwendungen auf mehrere Server horizontal hochskalieren.

## Erste Schritte
Die schnellste Einstiegsmethode ist das nachstehende .NET-Beispiel zu kompilieren und auszuführen, wie in den folgenden Schritten beschrieben. Sie können auch den Quellcode prüfen und Konfigurationen implementieren, die Ihren eigenen Clientanwendungen ähneln.

**Schritt 1:** Sie können das Projekt aus dem [DocumentDB-Beispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark) herunterladen oder das GitHub-Repository durchsuchen.

**Schritt 2:** Ändern Sie die Einstellungen für „EndpointUrl“, „AuthorizationKey“, „CollectionThroughput“ und „DocumentTemplate“ (optional) in der Datei „App.config“.

> [AZURE.NOTE] Bevor Sie Sammlungen mit hohem Durchsatz bereitstellen, besuchen Sie bitte die [Preisseite](https://azure.microsoft.com/pricing/details/documentdb/), um die Kosten pro Sammlung einzuschätzen. DocumentDB rechnet Speicher und Durchsatz unabhängig voneinander auf Stundenbasis ab, sodass Sie Kosten sparen, indem Sie den Durchsatz Ihrer DocumentDB-Sammlungen nach dem Testen löschen oder verringern.

**Schritt 3:** Kompilieren Sie die Konsolen-App über die Befehlszeile, und führen Sie sie von dort aus. Ihnen sollte eine Ausgabe wie die folgende angezeigt werden:

	Summary:
	---------------------------------------------------------------------
	Endpoint: https://docdb-scale-demo.documents.azure.com:443/
	Collection : db.testdata at 50000 request units per second
	Document Template*: Player.json
	Degree of parallelism*: 500
	---------------------------------------------------------------------

	DocumentDBBenchmark starting...
	Creating database db
	Creating collection testdata
	Creating metric collection metrics
	Retrying after sleeping for 00:03:34.1720000
	Starting Inserts with 500 tasks
	Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
	Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
	Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
	Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
	Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
	Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
	Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
	Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
	Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
	Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
	Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
	Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
	Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
	Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
	Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
	Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
	Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
	Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
	Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
	Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
	Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
	Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

	Summary:
	---------------------------------------------------------------------
	Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
	---------------------------------------------------------------------
	DocumentDBBenchmark completed successfully.


**Schritt 4 (falls erforderlich):** Der vom Tool gemeldete Durchsatz (RU/s) sollte identisch oder höher sein als der bereitgestellte Durchsatz der Sammlung. Wenn dies nicht der Fall ist, kann die Erhöhung von „DegreeOfParallelism“ in kleinen Schritten dabei helfen, den Grenzwert zu erreichen. Wenn der Durchsatz Ihrer Clientanwendung seinen Höchstwert erreicht, hilft Ihnen das Starten mehrerer App-Instanzen auf demselben oder auf verschiedenen Computern dabei, den bereitgestellten Grenzwert auf den verschiedenen Instanzen zu erreichen. Wenn Sie Hilfe zu diesem Schritt benötigen, erreichen Sie uns über [Ask DocumentDB](askdocdb@microsoft.com) (Fragen an DocumentDB) oder über ein Support-Ticket.

Wenn die App ausgeführt wird, können Sie verschiedene [Indizierungsrichtlinien](documentdb-indexing-policies.md) und [Konsistenzebenen](documentdb-consistency-levels.md) ausprobieren, um ihre Auswirkungen auf Durchsatz und Latenzzeit zu verstehen. Sie können auch den Quellcode prüfen und Konfigurationen implementieren, die Ihren eigenen Testsammlungen oder Produktionsanwendungen ähneln.

## Zusammenfassung
In diesem Artikel wurde erläutert, wie Sie auf DocumentDB Leistungs- und Skalierungstests mithilfe einer .NET-Konsolenanwendung ausführen können. Außerdem wurden die wichtigsten Konfigurationsoptionen für die beste Leistung von Azure DocumentDB besprochen. Weitere Informationen zu der Arbeit mit DocumentDB finden Sie unter den folgenden Links.

* [Beispiel zu Leistungstests in DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Serverseitige Partitionierung in DocumentDB](documentdb-partition-data.md)
* [DocumentDB-Sammlungen und -Leistungsstufen](documentdb-performance-levels.md)
* [DocumentDB .NET SDK-Dokumentation auf MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET-Beispiele](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB-Blog zu Leistungstipps](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)

<!---HONumber=AcomDC_0525_2016-->