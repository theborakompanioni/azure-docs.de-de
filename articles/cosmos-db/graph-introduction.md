---
title: "Einführung in die Graph-APIs von Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie anhand der Graph-Abfragesprache Gremlin von Apache TinkerPop umfangreiche Diagramme mit niedrigen Latenzen durch Azure Cosmos DB speichern, abfragen und traversieren können."
services: cosmos-db
author: dennyglee
documentationcenter: 
ms.assetid: b916644c-4f28-4964-95fe-681faa6d6e08
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/21/2017
ms.author: denlee
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 4ee52c2665d5856cb0022365edf0485458b629fe
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017

---
# <a name="introduction-to-azure-cosmos-db-graph-api"></a>Einführung in die Graph-API von Azure Cosmos DB

[Azure Cosmos DB](introduction.md) ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet [sofort einsetzbare globale Verteilung](distribute-data-globally.md), [flexible Skalierung von Durchsatz und Speicher](partition-data.md) weltweit, Latenzzeiten im einstelligen Millisekundenbereich beim 99. Perzentil, [fünf wohldefinierte Konsistenzebenen](consistency-levels.md) sowie garantierte hohe Verfügbarkeit, gestützt durch [branchenführende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db/) (SLAs, Service Level Agreements). Azure Cosmos DB [indiziert automatisch Daten](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), sodass Sie sich nicht mit der Schema- und Indexverwaltung befassen müssen. Es unterstützt mehrere Datenmodelle – Dokumente, Schlüsselwerte, Diagramme und spaltenorientierte Datenmodelle.

![Gremlin, Diagramm und Azure Cosmos DB](./media/graph-introduction/graph-gremlin.png)

Die Graph-API von Azure Cosmos DB bietet:

- Graph-Modellierung
- Traversal-APIs
- Sofort einsatzbereite globale Verteilung
- Flexible Skalierung von Speicher und Durchsatz mit Leselatenzen von weniger als 10 ms und weniger als 15 ms beim 99. Perzentil
- Automatische Indizierung mit sofortiger Abfrageverfügbarkeit
- Einstellbare Konsistenzebenen
- Umfassende SLAs, einschließlich einer Verfügbarkeit von 99,99 %

Zum Abfragen von Azure Cosmos DB können Sie die [Apache TinkerPop](http://tinkerpop.apache.org)-Graph-Traversalsprache [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) oder andere mit TinkerPop kompatible Diagrammsysteme wie [Apache Spark GraphX](spark-connector-graph.md) verwenden.

Dieser Artikel enthält eine Übersicht über die Graph-API von Azure Cosmos DB und erläutert, wie Sie diese zum Speichern von umfangreichen Diagrammen mit Milliarden von Vertices und Edges verwenden können. Sie können die Diagramme mit einer Latenz im Millisekundenbereich abfragen und die Diagrammstruktur und das Schema entwickeln.

## <a name="graph-database"></a>Diagrammdatenbank
Daten sind in der Praxis naturgemäß vernetzt. Bei der konventionellen Datenmodellierung liegt der Schwerpunkt auf Entitäten. Bei vielen Anwendungen besteht auch die Notwendigkeit, sowohl Entitäten als auch Beziehungen natürlich zu modellieren.

Ein [Diagramm](http://mathworld.wolfram.com/Graph.html) ist eine Struktur aus [Vertices](http://mathworld.wolfram.com/GraphVertex.html) und [Edges](http://mathworld.wolfram.com/GraphEdge.html). Sowohl Vertices als auch Edges können eine beliebige Anzahl von Eigenschaften aufweisen. Als Vertices werden diskrete Objekte wie etwa eine Person, ein Ort oder ein Ereignis bezeichnet. Edges bezeichnen Beziehungen zwischen Vertices. Beispielsweise könnte eine Person eine andere Person kennen, an einem Ereignis beteiligt sein und sich vor Kurzem an einem Ort befunden haben. Eigenschaften geben Informationen zu den Vertices und Edges an. Zu Eigenschaften zählen beispielsweise Name und Alter eines Vertex sowie ein Edge, der einen Zeitstempel und/oder eine Gewichtung aufweist. Dieses Modell wird offiziell als [Eigenschaftsdiagramm](http://tinkerpop.apache.org/docs/current/reference/#intro) bezeichnet. Azure Cosmos DB unterstützt das Eigenschaftsdiagrammmodell.

Das folgende Beispieldiagramm stellt beispielhaft die Beziehungen zwischen Personen, Mobilgeräten, Interessen und Betriebssystemen dar.

![Beispieldatenbank mit Personen, Geräten und Interessen](./media/graph-introduction/sample-graph.png)

Diagramme sind nützlich, um sich einen Überblick über eine große Bandbreite an Datasets in Wissenschaft, Technologie und Wirtschaft zu verschaffen. Mithilfe von Diagrammdatenbanken können Diagramme natürlich modelliert und gespeichert werden, sodass sie sich in zahlreichen Szenarien nützlich einsetzen lassen. Bei Diagrammdatenbanken handelt es sich in der Regel um NoSQL-Datenbanken, da diese Anwendungsfälle häufig auch Schemaflexibilität und schnelle Iterationen erfordern.

Diagramme bieten eine neuartige und leistungsstarke Datenmodellierungsmethode. Dies allein ist jedoch kein ausreichender Grund für die Verwendung einer Diagrammdatenbank. Bei vielen Anwendungsfällen und -mustern im Zusammenhang mit Diagrammtraversierungen übertreffen Diagramme konventionelle SQL- und NoSQL-Datenbanken um ein Vielfaches. Dieser Leistungsunterschied macht sich umso deutlicher bemerkbar, wenn mehr als eine Beziehung (z.B. Freunde von Freunden) traversiert wird.

Sie können die von Diagrammdatenbanken bereitgestellte Funktion für schnelle Traversierungen mit Diagrammalgorithmen (z.B. Tiefensuche, Breitensuche, Dijkstra-Algorithmus) kombinieren, um Probleme in verschiedenen Domänen wie sozialen Netzwerken, Inhaltsverwaltung, Geodaten und Empfehlungen zu beheben.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Weltweite Diagramme mit Azure Cosmos DB
Azure Cosmos DB ist eine vollständig verwaltete Diagrammdatenbank, die eine globale Verteilung, flexible Skalierung von Speicher und Durchsatz, automatische Indizierung und Abfrage sowie einstellbare Konsistenzebenen bietet und den TinkerPop-Standard unterstützt.  

![Architektur von Azure Cosmos DB-Diagrammen](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB bietet im Vergleich zu anderen Diagrammdatenbanken auf dem Markt folgende einzigartige Features:

* Flexibel skalierbarer Durchsatz und Speicher

 Diagramme müssen in der Praxis über die Kapazität eines einzelnen Servers hinweg skaliert werden. Mit Azure Cosmos DB können Sie Ihre Diagramme nahtlos über mehrere Server hinweg skalieren. Zudem können Sie den Durchsatz Ihres Diagramms unabhängig basierend auf Ihren Zugriffsmustern skalieren. Azure Cosmos DB unterstützt Diagrammdatenbanken, die auf eine nahezu unbegrenzte Speichergröße und einen nahezu unbegrenzten bereitgestellten Durchsatz skaliert werden können.

* Replikation in mehreren Regionen

 Azure Cosmos DB repliziert Ihre Diagrammdaten transparent in allen Regionen, die Sie Ihrem Konto zugeordnet haben. Die Replikation ermöglicht es Ihnen, Anwendungen zu entwickeln, die globalen Zugriff auf Daten erfordern. In den Bereichen Konsistenz, Verfügbarkeit und Leistung und den entsprechenden Garantien gibt es dabei gewisse Nachteile. Azure Cosmos DB bietet ein transparentes regionales Failover mit Multi-Homing-APIs. Sie können Durchsatz und Speicher weltweit flexibel skalieren.

* Schnelle Abfragen und Traversierungen mit der vertrauten Gremlin-Syntax

 Speichern Sie heterogene Vertices und Edges, und führen Sie Abfragen dieser Dokumente über eine vertraute Gremlin-Syntax durch. Azure Cosmos DB nutzt eine sperrfreie, protokollstrukturierte Indizierungstechnologie für die gleichzeitige Ausführung zahlreicher Vorgänge, um sämtliche Inhalte automatisch zu indizieren. Diese Funktion ermöglicht umfassende Echtzeitabfragen und -traversierungen, ohne Schemahinweise, sekundäre Indizes oder Ansichten festlegen zu müssen. Mehr erfahren Sie unter [Abfragediagramme mithilfe von Gremlin](gremlin-support.md).

* Vollständige Verwaltung

 Sie müssen sich nicht mehr mit der Verwaltung von Datenbanken und Rechenressourcen befassen. Dank des vollständig verwalteten Microsoft Azure-Diensts bleiben Ihnen die Verwaltung virtueller Computer, die Bereitstellung und Konfiguration von Software, die Skalierung oder komplexe Datentarifupgrades erspart. Alle Diagramme werden automatisch gesichert und vor regionalen Ausfällen geschützt. Sie können einfach Azure Cosmos DB-Konten hinzufügen und nach Bedarf Kapazitäten bereitstellen. Dies ermöglicht es Ihnen, sich auf Ihre Anwendung zu konzentrieren, ohne sich mit dem Betrieb und der Verwaltung der Datenbank aufhalten zu müssen.

* Automatische Indizierung

 Alle Eigenschaften in Knoten und Edges im Diagramm werden von Azure Cosmos DB automatisch indiziert, ohne dass ein Schema oder die Erstellung sekundärer Indizes erwartet oder gefordert wird.

* Kompatibilität mit Apache TinkerPop

 Azure Cosmos DB unterstützt systemintern den Open-Source-Standard Apache TinkerPop und kann in andere TinkerPop-fähige Diagrammsysteme integriert werden. So können Sie mühelos Migrationen von einer anderen Diagrammdatenbank wie Titan oder Neo4j durchführen, oder Azure Cosmos DB mit Diagrammanalyseframeworks wie [Apache Spark GraphX](spark-connector-graph.md) verwenden.

* Einstellbare Konsistenzebenen

 Die Konsistenz kann über fünf klar definierte Ebenen abgestimmt werden, um für ein ausgewogenes Verhältnis zwischen Konsistenz und Leistung zu sorgen. Für Abfragen und Lesevorgänge bietet Azure Cosmos DB fünf verschiedene Konsistenzebenen – „stark“, „begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „letztlich“. Mit diesen granularen, wohldefinierten Konsistenzebenen können fundierte Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz geschlossen werden. Weitere Informationen finden Sie unter [Verwenden von Konsistenzebenen zum Maximieren der Verfügbarkeit und Leistung in DocumentDB](consistency-levels.md).

Azure Cosmos DB bietet zudem die Möglichkeit, mehrere Modelle wie Dokumente und Diagramme in denselben Containern bzw. Datenbanken zu verwenden. Sie können eine Dokumentsammlung verwenden, um Diagrammdaten zusammen mit Dokumenten zu speichern. Sowohl mit SQL-Abfragen über JSON als auch mit Gremlin Abfragen können Sie dieselben Daten wie ein Diagramm abfragen.

## <a name="getting-started"></a>Erste Schritte
Sie können die Azure-Befehlszeilenschnittstelle (CLI), Azure PowerShell oder das Azure-Portal mit Unterstützung für die Graph-API verwenden, um Azure Cosmos DB-Konten zu erstellen. Nach der Erstellung von Konten wird im Azure-Portal ein Dienstendpunkt wie `https://<youraccount>.graphs.azure.com` bereitgestellt, der ein WebSocket-Front-End für Gremlin bietet. Um eine Verbindung zu diesem Endpunkt herzustellen und Anwendungen in Java, Node.js oder einem beliebigen Gremlin-Clienttreiber zu erstellen, können Sie Ihre TinkerPop-kompatiblen Tools wie die [Gremin-Konsole](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) konfigurieren.

In der folgenden Tabelle werden gängige Gremlin-Treiber aufgeführt, die Sie für Azure Cosmos DB verwenden können:

| Herunterladen | Dokumentation |
| --- | --- |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) |[Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) |
| [Node.js](https://www.npmjs.com/package/gremlin) |[Gremlin-JavaScript auf Github](https://github.com/jbmusso/gremlin-javascript) |
| [Gremlin-Konsole](https://tinkerpop.apache.org/downloads.html) |[TinkerPop-Dokumente](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |

Azure Cosmos DB bietet neben den [Azure Cosmos DB-SDKs](documentdb-sdk-dotnet.md) über NuGet auch eine .NET-Bibliothek mit Gremlin-Erweiterungsmethoden. Diese Bibliothek stellt einen In-Process-Gremlin-Server zur Verfügung, mit dem eine direkte Verbindung zu DocumentDB-Datenpartitionen hergestellt werden kann.

| Herunterladen | Dokumentation |
| --- | --- |
| [.NET](https://www.nuget.org/packages/Microsoft.Azure.Graphs/) |[Microsoft.Azure.Graphs](https://msdn.microsoft.com/library/azure/dn948556.aspx) |

Durch Verwendung des [Azure Cosmos DB-Emulators](local-emulator.md) können Sie Entwicklungsarbeiten und Tests lokal mithilfe der Graph-API durchführen, ohne ein Azure-Abonnement zu erstellen oder sonstige Kosten zu verursachen. Wenn Sie mit der Funktion der Anwendung im Emulator zufrieden sind, können Sie zur Verwendung eines Azure Cosmos DB-Kontos in der Cloud wechseln.

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Szenarien für die Diagrammunterstützung von Azure Cosmos DB
Im Folgenden werden einige Szenarien vorgestellt, in denen die Diagrammunterstützung von Azure Cosmos DB verwendet werden kann:

* Soziale Netzwerke

 Durch die Kombination von Daten über Ihre Kunden und deren Interaktionen mit anderen Personen können Sie individuelle Erlebnisse schaffen, das Kundenverhalten vorhersagen oder Personen mit ähnlichen Interessen vernetzen. Azure Cosmos DB kann zum Verwalten sozialer Netzwerke sowie zum Nachverfolgen von Kundenpräferenzen und -daten verwendet werden.

* Empfehlungsmodule

 Dieses Szenario kommt häufig im Einzelhandel vor. Durch die Kombination von Informationen zu Produkten, Benutzern und Benutzerinteraktionen (z.B. Einkäufe, Surfverhalten oder Bewertungen eines Artikels) können Sie benutzerdefinierte Empfehlungen erstellen. Die geringe Latenz, flexible Skalierung und native Diagrammunterstützung von Azure Cosmos DB sind für die Modellierung solcher Interaktionen ideal.

* Geodaten

 Zahlreiche Anwendungen in den Bereichen Telekommunikation, Logistik und Reiseplanung müssen einen bestimmten Ort in einer bestimmten Region oder die kürzeste bzw. optimale Route zwischen zwei Orten finden. Azure Cosmos DB ist die ideale Lösung für derartige Probleme.

* Internet der Dinge

 Durch die Modellierung von Netzwerken und Verbindungen zwischen IoT-Geräten als Diagramme können Sie sich einen besseren Überblick über den Status Ihrer Geräte und Ressourcen verschaffen und herausfinden, inwiefern sich Änderungen an einem Teil des Netzwerks möglicherweise auf andere Teile auswirken können.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Diagrammunterstützung in Azure Cosmos DB finden Sie durch folgende Ressourcen:

* Erste Schritte durch das [Tutorial zum Azure Cosmos DB-Diagramm](create-graph-dotnet.md)
* Informationen zum [Abfragen von Diagrammen in Azure Cosmos DB mittels Gremlin](gremlin-support.md)

