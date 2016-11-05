---
title: Wann sollten NoSQL und SQL verwendet werden? | Microsoft Docs
description: Informieren Sie sich über die Vorteile, die die Verwendung nicht relationaler NoSQL-Lösungen gegenüber SQL-Lösungen bietet. Finden Sie heraus, ob sich Ihr Szenario am besten mit einem Microsoft Azure NoSQL-Dienst oder mit einer SQL Server-Lösung umsetzen lässt.
keywords: nosql vs sql, when to use NoSQL, sql vs nosql
services: documentdb
documentationcenter: ''
author: mimig1
manager: jhubbard
editor: ''

ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/24/2016
ms.author: mimig

---
# NoSQL im Vergleich zu SQL
SQL Server und relationale Datenbanken (RDBMS) werden seit über 20 Jahren erfolgreich eingesetzt. Da jedoch immer größere Datenmengen und immer unterschiedlichere Datentypen mit hoher Geschwindigkeit verarbeitet werden müssen, haben sich die Datenspeicheranforderungen für Anwendungsentwickler geändert. Um diese neuen Anforderungen zu erfüllen, erfreuen sich NoSQL-Datenbanken immer größerer Beliebtheit, mit denen unstrukturierte und heterogene Daten im jeweils erforderlichen Umfang gespeichert werden können.

Bei NoSQL handelt es sich um eine Datenbankkategorie, die sich erheblich von SQL-Datenbanken unterscheidet. NoSQL wird häufig im Zusammenhang mit „Nicht SQL“-Datenverwaltungssystemen oder einem Ansatz an die Datenverwaltung verwendet, der „nicht nur SQL“ beinhaltet. Die NoSQL-Kategorie umfasst eine Reihe von Technologien, darunter u. a. Dokumentdatenbanken, Schlüsselwertspeicher, spaltenbasierte Speicher und Diagrammdatenbanken, die häufig bei Gaming-Apps, sozialen Apps und IoT-Apps eingesetzt werden.

![NoSQL im Vergleich zu SQL: gängige Szenarien und Datenmodelle](./media/documentdb-nosql-vs-sql/nosql-vs-sql-overview.png)

In diesem Artikel werden die Unterschiede zwischen NoSQL und SQL erläutert. Außerdem werden die NoSQL- und SQL-Angebote von Microsoft vorgestellt.

## Wann sollte NoSQL verwendet werden?
Angenommen, Sie entwickeln eine neue Social Engagement-Website. Auf dieser Website können Benutzer Beiträge erstellen und Bilder, Videos oder Musik hinzufügen. Andere Benutzer können die Beiträge kommentieren und Punkte (Likes) vergeben, um die Beiträge zu bewerten. Die Landing Page zeigt einen Feed mit Beiträgen, die Benutzer freigeben und mit denen Benutzer interagieren können.

Wie würden Sie diese Daten speichern? Wenn Sie mit SQL vertraut sind, beginnen Sie vielleicht in etwa wie folgt:

![NoSQL im Vergleich zu SQL: relationales Datenmodell für eine Social Engagement-Website](./media/documentdb-nosql-vs-sql/nosql-vs-sql-social.png)

So weit, so gut. Machen Sie sich nun einmal Gedanken über die Struktur eines einzelnen Beitrags, und wie Sie ihn anzeigen können. Wenn Sie den Beitrag und die zugehörigen Bilder, Audio- und Videoinhalte, Kommentare, Punkte und Benutzerinformationen auf einer Website oder in einer Anwendung anzeigen möchten, müssten Sie eine Abfrage mit acht Tabellenjoins ausführen, um die Inhalte abzurufen. Stellen Sie sich jetzt einen Stream aus Beiträgen vor, die dynamisch geladen und auf dem Bildschirm angezeigt werden. Dabei wird schnell klar, dass Tausende von Abfragen und eine Vielzahl von Joins erforderlich sind, um die Aufgabe auszuführen.

Sie könnten nun eine relationale Lösung wie SQL Server einsetzen, um die Daten zu speichern. Allerdings gibt es eine weitere Möglichkeit, mit der sich diese Vorgänge vereinfachen lassen: NoSQL. Indem Sie den Beitrag in ein JSON-Dokument (siehe unten) umwandeln und in DocumentDB speichern, unserem Azure NoSQL-Dokumentdatenbank-Dienst, können Sie die Leistung steigern und den gesamten Beitrag mit einer einzigen Abfrage und ohne Joins abrufen. Dieser Ansatz ist einfacher und ermöglicht es Ihnen, eine höhere Leistung zu erzielen.

    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"The first video"},
            {"url":"http://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

Außerdem lassen sich diese Daten basierend auf der Beitrags-ID partitionieren, sodass die Daten horizontal hochskaliert werden können und Sie sich die NoSQL-Skalierung zunutze machen können. Mit NoSQL-Systemen können Entwickler zudem flexiblere Konsistenzeinstellungen wählen und hoch verfügbare Apps anbieten. Und schließlich bietet diese Lösung den Vorteil, dass Entwickler auf Datenebene kein Schema definieren und verwalten müssen, wodurch eine schnellere Iteration möglich ist.

Diese Lösung kann anschließend mit weiteren Azure-Diensten erweitert werden:

* [Azure Search](https://azure.microsoft.com/services/search/) kann von Benutzern über die Web-App verwendet werden, um nach Beiträgen zu suchen.
* [Azure App Services](https://azure.microsoft.com/services/app-service/) kann zum Hosten von Anwendungen und Hintergrundprozessen eingesetzt werden.
* [Azure Blob Storage](https://azure.microsoft.com/services/storage/) kann zum Speichern der vollständigen Benutzerprofile, einschließlich der zugehörigen Bilder, verwendet werden.
* [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) kann zum Speichern enorm großer Datenmengen (z. B. Anmeldeinformationen) sowie von Daten zum Analysieren der Nutzung eingesetzt werden.
* Mit [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) können Sie wichtige Einblicke und Feedback zum Prozess erhalten, um die richtigen Inhalte für die richtigen Benutzer bereitzustellen.

Diese Social Engagement-Website ist nur ein Szenario, in dem eine NoSQL-Datenbank das ideale Datenmodell ist. Weitere Informationen zu diesem Szenario und zum Erstellen eines Datenmodells für DocumentDB in Anwendungen für soziale Medien finden Sie unter [Going social with DocumentDB (DocumentDB für soziale Medien)](documentdb-social-media-apps.md).

## NoSQL und SQL im Vergleich
In der folgenden Tabelle sind die wesentlichen Unterschiede zwischen NoSQL und SQL aufgeführt.

![NoSQL im Vergleich zu SQL: für welche Szenarien eignet sich NoSQL, für welche SQL? SQL und NoSQL im Vergleich](./media/documentdb-nosql-vs-sql/nosql-vs-sql-comparison.png)

Wenn sich Ihre Anforderungen am besten mit einer NoSQL-Datenbank erfüllen lassen, fahren Sie mit dem nächsten Abschnitt fort, um mehr über die verfügbaren NoSQL-Dienste von Azure zu erfahren. Wenn eine SQL-Datenbank die beste Lösung für Ihre Anforderungen ist, fahren Sie mit [Welche SQL-Angebote gibt es von Microsoft?](#what-are-the-microsoft-sql-offerings) fort.

## Welche NoSQL-Angebote gibt es von Microsoft Azure?
Azure bietet vier vollständig verwaltete NoSQL-Dienste:

* [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)
* [Azure Table Storage](https://azure.microsoft.com/services/storage/)
* [Azure HBase als Teil von HDInsight](https://azure.microsoft.com/services/hdinsight/)
* [Azure Redis Cache](https://azure.microsoft.com/services/cache/)

Nachfolgend sind die wichtigsten Unterscheidungsmerkmale der einzelnen Dienste dargestellt. Mit welchem Dienst lassen sich die Anforderungen Ihrer Anwendung am besten erfüllen?

![NoSQL im Vergleich zu SQL: wann sollten die NoSQL-Angebote von Microsoft Azure (einschließlich DocumentDB, Table Storage, HBase als Teil von HDInsight und Redis Cache) verwendet werden?](./media/documentdb-nosql-vs-sql/nosql-vs-sql-documentdb-storage-hbase-hdinsight-redis-cache.png)

Wenn Ihre Anwendungsanforderungen mit mehreren Diensten erfüllt werden können, finden Sie in den folgenden Themen weitere Informationen:

* [DocumentDB-Lernpfad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) und [DocumentDB use cases (DocumentDB-Anwendungsfälle)](documentdb-use-cases.md)
* [Get started with Azure table storage (Erste Schritte mit Azure Table Storage)](../storage/storage-dotnet-how-to-use-tables.md)
* [What is HBase in HDInsight (Was ist HBase in HDInsight?)](../hdinsight/hdinsight-hbase-overview.md)
* [Redis Cache-Lernpfad](https://azure.microsoft.com/documentation/learning-paths/redis-cache/)

Fahren Sie dann mit [Nächste Schritte](#next-steps) fort, um Informationen zur kostenlosen Testversion zu erhalten.

## Welche SQL-Angebote gibt es von Microsoft?
Microsoft verfügt über fünf SQL-Angebote:

* [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)
* [SQL Server auf virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/)
* [SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-2016/)
* [Azure SQL Data Warehouse (Vorschau)](https://azure.microsoft.com/services/sql-data-warehouse/)
* [Analytics Platform System (lokales Gerät)](https://www.microsoft.com/de-DE/server-cloud/products/analytics-platform-system/)

Wenn Sie sich für SQL Server auf einem virtuellen Computer oder für SQL-Datenbank interessieren, finden Sie unter [Choose a cloud SQL Server option: Azure SQL (PaaS) Database or SQL Server on Azure VMs (IaaS) (Auswählen einer SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server in Azure Virtual Machines (IaaS))](../sql-database/sql-database-paas-vs-sql-server-iaas.md) weitere Informationen zu den Unterschieden zwischen den beiden Angeboten.

Wenn SQL eine geeignete Lösung ist, finden Sie unter [SQL Server](https://www.microsoft.com/server-cloud/products/) weitere Informationen zu den Vorteilen der Microsoft SQL-Produkte und -Dienste.

Fahren Sie dann mit [Nächste Schritte](#next-steps) fort, um unsere Angebote kostenlos zu testen.

## Nächste Schritte
Wir möchten Sie einladen, mehr über unsere SQL- und NoSQL-Produkte zu erfahren und diese kostenlos zu testen.

* Sie können sich bei allen Azure-Diensten für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/pricing/free-trial/) registrieren, um eine Gutschrift in Höhe von 200 US-Dollar zu erhalten, die Sie für die Nutzung der Azure-Dienste ausgeben können.
  
  * [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)
  * [Azure HBase als Teil von HDInsight](https://azure.microsoft.com/services/hdinsight/)
  * [Azure Redis Cache](https://azure.microsoft.com/services/cache/)
  * [Azure SQL Data Warehouse (Vorschau)](https://azure.microsoft.com/services/sql-data-warehouse/)
  * [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)
  * [Azure Table Storage](https://azure.microsoft.com/services/storage/)
* Sie können eine [Testversion von SQL Server 2016 auf einem virtuellen Computer](https://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2016ctp33evaluationwindowsserver2012r2/) bereitstellen oder eine [Testversion von SQL Server](https://www.microsoft.com/de-DE/evalcenter/evaluate-sql-server-2016) herunterladen.
  
  * [SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-2016/)
  * [SQL Server auf virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/)

<!---HONumber=AcomDC_0727_2016-->