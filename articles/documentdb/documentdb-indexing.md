---
redirect_url: https://azure.microsoft.com/services/documentdb/
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: c1547d9910ce2bbfda79718ba74cdd8e7ca468f5
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017



---
# <a name="automatic-indexing-in-azure-cosmos-db"></a>Automatische Indizierung in Azure Cosmos DB
Dieser Artikel ist ein Auszug aus dem Artikel zur [vom Schema unabhängigen Indizierung mit Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), der bei der [41. internationalen VLDB-Konferenz vom 31.08. bis 04.09.2015](http://www.vldb.org/2015/) vorgestellt wurde. Er bietet eine Einführung in die Funktionsweise der Indizierung in Azure Cosmos DB. 

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

* Wie leitet Azure Cosmos DB das Schema von einem JSON-Dokument ab?
* Wie erstellt Azure Cosmos DB einen Index für verschiedenartige Dokumente?
* Wie führt Azure Cosmos DB die automatische Indizierung in großem Maßstab durch?

## <a id="HowDocumentDBIndexingWorks"></a> So funktioniert die DocumentDB-Indizierung
[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) ist eine speziell für JSON entwickelte Datenbank, die ohne Schema funktioniert. Sie erfordert weder Schema- noch sekundäre Indexdefinitionen, um Daten in großem Maßstab zu indizieren. Dadurch ermöglicht Ihnen Azure Cosmos DB das schnelle Definieren und Durchlaufen von Anwendungsdatenmodellen. Während Sie Dokumente zu einer Sammlung hinzufügen, werden alle Dokumenteigenschaften von Azure Cosmos DB automatisch indiziert und stehen dann für Sie für Abfragen bereit. Dank der automatischen Indizierung können Sie Dokumente mit absolut willkürlichen Schemas speichern, ohne sich Gedanken über Schemas oder sekundäre Indizes machen zu müssen.

Azure Cosmos DB nutzt die Einfachheit von JSON und das Fehlen einer Schemaspezifikation bei diesem Datenaustauschformat, um die Inkongruenz zwischen den Datenbank- und Anwendungsprogrammiermodellen zu beseitigen. Es werden keine Annahmen bezüglich der Dokumente gemacht, und neben instanzspezifischen Werten sind Dokumente mit unterschiedlichen Schemas in einer Azure Cosmos DB-Sammlung zulässig. Im Gegensatz zu anderen Dokumentdatenbanken arbeitet das Datenbankmodul von Azure Cosmos DB direkt auf der Ebene der JSON-Grammatik. Dadurch bleibt sie unabhängig vom Konzept eines Dokumentschemas, und die Grenzen zwischen den Struktur- und Instanzwerten von Dokumenten werden aufgehoben. Dies wiederum ermöglicht die automatische Indizierung von Dokumenten ohne Schema oder sekundäre Indizes.

Bei der Azure Cosmos DB-Indizierung wird die Tatsache genutzt, dass Dokumente für die JSON-Grammatik **als Strukturen dargestellt** werden können. Damit ein JSON-Dokument als Struktur dargestellt werden kann, muss ein Dummy-Stammknoten erstellt werden, der für die restlichen Knoten im Dokument das übergeordnete Element darstellt. Jede Bezeichnung, einschließlich der Arrayindizes in einem JSON-Dokument, wird zu einem Knoten der Struktur. Die nachfolgende Abbildung zeigt ein JSON-Beispieldokument und die entsprechende Strukturdarstellung.

> [!NOTE]
> Da JSON selbstbeschreibend ist, enthält jedes Dokument sowohl ein Schema (Metadaten) als auch Daten. `{"locationId": 5, "city": "Moscow"}` gibt z. B. an, dass zwei Eigenschaften `locationId` und `city` mit einem numerischen und einem Zeichenfolgeneigenschaftswert vorhanden sind. Azure Cosmos DB kann das Schema von Dokumenten ableiten und Dokumente indizieren, wenn sie eingefügt oder ersetzt werden, ohne dass Sie jemals Schemas oder sekundäre Indizes definieren müssen.
> 
> 

**JSON-Dokumente als Strukturen:**

![Dokumente als Strukturen](media/documentdb-indexing/DocumentsAsTrees.png)

Im obigen Beispiel gilt z. B. Folgendes:

* Die JSON-Eigenschaft `{"headquarters": "Belgium"}` im obigen Beispiel entspricht dem Pfad "headquarters/Belgium".
* Das JSON-Array `{"exports": [{"city": “Moscow"}`, `{"city": Athens"}]}` entspricht den Pfaden `/exports/[]/city/Moscow` und `/exports/[]/city/Athens`.

Bei der automatischen Indizierung geschieht Folgendes: (1) Jeder Pfad in einer Dokumentstruktur wird indiziert (es sei denn, der Entwickler hat die Indizierungsrichtlinie explizit zum Ausschließen von bestimmten Pfadmustern konfiguriert). (2) Jede Aktualisierung eines Dokuments in einer Azure Cosmos DB-Sammlung führt zu einer Aktualisierung der Struktur des Indexes (d. h. es werden Knoten hinzugefügt oder entfernt). Eine der wichtigsten Anforderungen bei der automatischen Indizierung von Dokumenten besteht darin, dass die Kosten für die Indizierung und Abfrage eines Dokuments mit tief geschachtelter Struktur (z. B. 10 Ebenen) mit denen für ein flaches JSON-Dokument, das aus Schlüssel-Wert-Paaren besteht und nur eine Ebene tief ist, identisch sein müssen. Sowohl die automatische Indizierung als auch Abfragesubsysteme beruhen daher auf einer normalisierten Pfaddarstellung.

Eine wichtige und logische Konsequenz der einheitlichen Behandlung der Schema- und Instanzwerte im Hinblick auf Pfade ist, dass ein Index der beiden Dokumente – ebenso wie die einzelnen Dokumente – eine Zuordnung zwischen Pfaden beibehält, und die Dokument-IDs, die diesen Pfad enthalten, auch als Struktur dargestellt werden können. Azure Cosmos DB nutzt diese Tatsache, um aus der Vereinigung aller Strukturen, die einzelne Dokumente in der Sammlung darstellen, eine Indexstruktur zu erstellen. Die Indexstruktur in Azure Cosmos DB-Sammlungen wächst im Laufe der Zeit, wenn neue Dokumente hinzugefügt oder aktualisiert werden.

**Azure Cosmos DB-Index als Struktur**:

![Index als Struktur](media/documentdb-indexing/IndexAsTree.png)

Obwohl kein Schema verwendet wird, ermöglichen die SQL- und JavaScript-Abfragesprachen von Azure Cosmos DB relationale Projektionen und Filter, die hierarchische Navigation zwischen Dokumenten, räumliche Vorgänge sowie den Aufruf von vollständig in JavaScript geschriebenen UDFs. Die Abfragelaufzeit von Azure Cosmos DB kann diese Abfragen unterstützen, da sie direkt mit der Indexstrukturdarstellung der Daten arbeiten kann.

Die standardmäßige Indizierungsrichtlinie indiziert automatisch alle Eigenschaften sämtlicher Dokumente und stellt konsistente Abfragen bereit (d. h. der Index wird synchron mit dem Dokumentschreibvorgang aktualisiert). Wie unterstützt Azure Cosmos DB einheitliche Aktualisierungen der Indexstruktur in großem Maßstab? Azure Cosmos DB verwendet für Schreibvorgänge optimierte, nicht sperrende und protokollstrukturierte Verfahren für die Indexwartung. Azure Cosmos DB unterstützt daher eine beständig hohe Menge schneller Schreibvorgänge, während konsistente Abfragen weiterhin verarbeitet werden. 

Die Azure Cosmos DB-Indizierung ist für Speichereffizienz und Mehrinstanzfähigkeit konzipiert. Aus Kostengründen ist der zusätzlich benötigte Speicherplatz für den Index gering und vorhersagbar. Indexaktualisierungen werden zudem innerhalb des Budgets der pro Azure Cosmos DB-Sammlung verfügbaren Systemressourcen ausgeführt.

## <a name="NextSteps"></a> Nächste Schritte
* Herunterladen des Artikels zur [von Schema unabhängigen Indizierung mit Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), der auf der 41. internationalen VLDB-Konferenz vom 31.08. bis 04.09.2015 vorgestellt wurde.
* [Abfragen mit DocumentDB-SQL](documentdb-sql-query.md)
* Weitere Informationen zum Anpassen des Azure Cosmos DB-Indexes finden Sie [hier](documentdb-indexing-policies.md)


