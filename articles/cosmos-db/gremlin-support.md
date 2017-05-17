---
title: "Unterstützung für Gremlin in Azure Cosmos DB | Microsoft-Dokumentation"
description: "Informationen über die Gremlin-Sprache, die verfügbaren Funktionen und Schritte in Azure Cosmos DB"
services: cosmosdb
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmosdb
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: db2287782df66ecee0795adb483e253021d0c2cf
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="azure-cosmos-db-gremlin-graph-support"></a>Unterstützung für Gremlin-Diagramme in Azure Cosmos DB
Azure Cosmos DB unterstützt eine [Gremlin]([Gremlin language](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps)) Graph-API zur Erstellung von Diagrammentitäten und zur Durchführung von Diagrammabfragen sowie Traversierungsvorgängen. Mithilfe der Gremlin-Sprache können Sie Diagrammentitäten (Vertices und Edges) erstellen, Eigenschaften innerhalb dieser Entitäten ändern, Abfragen und Traversierungen ausführen und Entitäten löschen. 

Azure Cosmos DB liefert für Unternehmen konzipierte Funktionen für Diagrammdatenbanken. Hierzu zählen globale Verteilungen, unabhängige Skalierungen von Speicher und Durchsatz, vorhersagbare Latenzen im einstelligen Millisekundenbereich, automatische Indizierungen und SLAs von 99,99 %. Da Azure Cosmos DB TinkerPop/Gremlin unterstützt, können Sie mühelos mit einer anderen Diagrammdatenbank geschriebene Anwendungen migrieren, ohne Codeänderungen vorzunehmen. Darüber hinaus wird Azure Cosmos DB aufgrund der Unterstützung für Gremlin nahtlos in TinkerPop-fähige Analyseframeworks wie [Apache Spark GraphX](http://spark.apache.org/graphx/) integriert. 

Dieser Artikel enthält eine kurze exemplarische Vorgehensweise zu Gremlin und listet die Funktionen und Schritte von Gremlin auf, die in der Vorschau der Unterstützung für die Graph-API unterstützt werden.

## <a name="gremlin-by-example"></a>Gremlin anhand eines Beispiels
Anhand eines Beispieldiagramms wird erläutert, wie Abfragen in Gremlin ausgedrückt werden können. Die folgende Abbildung zeigt eine Geschäftsanwendung, die Daten zu Benutzern, Interessen und Geräten in Form eines Diagramms verwaltet.  

![Beispieldatenbank mit Personen, Geräten und Interessen](./media/gremlin-support/sample-graph.png) 

Dieses Diagramm weist folgende Vertex-Typen (in Gremlin als „Bezeichnung“ bezeichnet) auf:

- Personen: Das Diagramm enthält die drei Personen „Robin“, „Thomas“ und „Ben“.
- Interessen: In diesem Beispiel liegen ihre Interessen bei Football-Spielen.
- Geräte: Dies sind die von den Personen verwendeten Geräte.
- Betriebssysteme: Dies sind die Betriebssysteme, die auf den Geräten ausgeführt werden.

Die Beziehungen zwischen diesen Entitäten werden anhand von folgenden Edgetypen/Bezeichnungen dargestellt:

- Kennt: Beispiel: „Thomas kennt Robin.“
- Interessiert an: Um die Interessen der Personen in unserem Diagramm darzustellen, verwenden wir beispielsweise „Ben ist an Football interessiert.“.
- Betriebssystem ausgeführt: Auf dem Laptop wird das Windows-Betriebssystem ausgeführt.
- Verwendet: Dies gibt an, welches Gerät eine Person verwendet. Beispiel: „Robin verwendet ein Motorola-Telefon mit der Seriennummer 77.“

Nun führen wir anhand dieses Diagramms einige Vorgänge mit der [Gremlin-Konsole](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) aus. Sie können diese Vorgänge auch mithilfe von Gremlin-Treibern auf der Plattform Ihrer Wahl (Java, Node.js, Python oder .NET) ausführen.  Bevor wir uns mit den unterstützten Funktionen von Azure Cosmos DB befassen, gehen wir einige Beispiele durch, um uns mit der Syntax vertraut zu machen.

Sehen wir uns zuerst CRUD an. Die folgende Gremlin-Anweisung fügt den Vertex „Thomas“ in das Diagramm ein:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Als Nächstes fügt die folgende Gremlin-Anweisung einen „kennt“-Edge zwischen Thomas und Robin ein.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Die folgende Abfrage gibt die Vertices „Person“ in absteigender Reihenfolge ihrer Vornamen zurück:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Diagramme eignen sich am besten, wenn Sie Fragen wie „Welche Betriebssysteme verwenden Freunde von Thomas?“ beantworten müssen. Um diese Information anhand des Diagramms zu ermitteln, können Sie diese einfache Gremlin-Traversierung ausführen:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Nun sehen wir uns, welche Funktionen Azure Cosmos DB für Gremlin-Entwickler bietet.

## <a name="gremlin-features"></a>Gremlin-Funktionen
TinkerPop ist ein Standard, der eine große Bandbreite an Diagrammtechnologien abdeckt. Daher weist diese Standardterminologie auf, die beschreiben, welche Funktionen von einem Diagrammanbieter bereitgestellt werden. Azure Cosmos DB stellt eine beständige und beschreibbare Diagrammdatenbank mit hoher Parallelität dar, die über mehrere Server oder Cluster hinweg partitioniert werden kann. 

Die folgende Tabelle enthält die TinkerPop-Funktionen, die von Azure Cosmos DB implementiert werden: 

| Kategorie | Implementierung von Azure Cosmos DB |  Hinweise | 
| --- | --- | --- |
| Diagrammfunktionen | Stellen Persistenz und ConcurrentAccess in der Vorschau bereit. Sind zur Unterstützung für Transaktionen ausgelegt. | Computermethoden können über den Spark-Connector implementiert werden. |
| Variablenfunktionen | Unterstützen Boolean-, Integer-, Byte-, Double-, Float-, Long- und String-Datentypen | Unterstützen primitive Typen; ist mit komplexen Typen über Datenmodelle kompatibel |
| Vertex-Funktionen | Unterstützen RemoveVertices MetaProperties, AddVertices MultiProperties, StringIds UserSuppliedIds, AddProperty, RemoveProperty  | Unterstützen die Erstellung, Änderung und Löschung von Vertices |
| Vertex-Eigenschafts-Funktionen | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Unterstützen die Erstellung, Änderung und Löschung von Vertex-Eigenschaften |
| Edgefunktionen | AddEges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Unterstützen die Erstellung, Änderung und Löschung von Edges |
| Edge-Eigenschafts-Funktionen | Properties, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Unterstützen die Erstellung, Änderung und Löschung von Edge-Eigenschaften |

## <a name="gremlin-wire-format-graphson"></a>Gremlin-Sendeformat: GraphSON

Azure Cosmos DB verwendet bei der Rückgabe von Ergebnissen aus Gremlin-Vorgängen das [GraphSON-Format](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format). GraphSON ist das Standardformat von Gremlin zur Darstellung von Vertices, Edges und (ein- und mehrwertigen) Eigenschaften mittels JSON. 

Der folgende Ausschnitt zeigt z.B. eine GraphSON-Darstellung eines Vertex in Azure Cosmos DB. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

Für Vertices werden folgende Eigenschaften von GraphSON verwendet:

| Eigenschaft | Beschreibung |
| --- | --- |
| id | Die ID für den Vertex. Muss eindeutig sein (in Kombination mit dem Wert von „_partition“, falls zutreffend). |
| label | Die Bezeichnung des Vertex. Diese ist optional und dient zur Beschreibung des Entitätstyps. |
| Typ | Dient zur Unterscheidung von Vertices von anderen Dokumenten, die keine Diagramme sind. |
| properties | Sammlung von benutzerdefinierten Eigenschaften in Verbindung mit dem Vertex. Jede Eigenschaft kann mehrere Werte enthalten. |
| _partition (konfigurierbar) | Der Partitionsschlüssel des Vertex. Kann zur Skalierung von Diagrammen auf mehreren Servern verwendet werden. |
| outE | Enthält eine Liste von externen Edges aus einem Vertex. Die Speicherung von Informationen zur Nähe zum Vertex ermöglicht die schnelle Ausführung von Traversierungen. Edges werden basierend auf deren Bezeichnungen gruppiert. |

Zudem enthalten Edges folgende Informationen, die die Navigation in anderen Teilen des Diagramms unterstützen können.

| Eigenschaft | Beschreibung |
| --- | --- |
| id | Die ID für den Edge. Muss eindeutig sein (in Kombination mit dem Wert von „_partition“, falls zutreffend). |
| label | Die Bezeichnung des Edge. Diese Eigenschaft ist optional, und dient zur Beschreibung des Beziehungstyps. |
| inV | Sammlung von benutzerdefinierten Eigenschaften in Verbindung mit dem Edge. Jede Eigenschaft kann mehrere Werte enthalten. |
| properties | Sammlung von benutzerdefinierten Eigenschaften in Verbindung mit dem Edge. Jede Eigenschaft kann mehrere Werte enthalten. |

Jede Eigenschaft kann mehrere Werte in einem Array speichern. 

| Eigenschaft | Beschreibung |
| --- | --- |
| value | Der Wert der Eigenschaft.

## <a name="gremlin-partitioning"></a>Gremlin-Partitionierung

In Azure Cosmos DB werden Diagramme in Containern gespeichert, die in Bezug auf Speicherung und Durchsatz (ausgedrückt in normalisierten Anforderungen pro Sekunde) unabhängig voneinander skaliert werden können. Jeder Container muss eine optionale, jedoch empfohlene Eigenschaft für Partitionsschlüssel definieren, die eine logische Partitionsbegrenzung für die verknüpften Daten festlegt. Jeder Vertex/Edge muss eine `id`-Eigenschaft aufweisen, die für Entitäten innerhalb dieses Partitionsschlüsselwerts eindeutig ist. Ausführliche Informationen finden Sie unter [Partitionierung in Azure Cosmos DB](partition-data.md).

Gremlin-Vorgänge funktionieren nahtlos in Diagrammdaten, die sich über mehrere Partitionen in Azure Cosmos DB erstrecken. Es wird jedoch empfohlen, einen Partitionsschlüssel für die Diagramme auszuwählen, der häufig als Filter in Abfragen verwendet wird, viele unterschiedliche Werte enthält und eine ähnliche Zugriffshäufigkeit für diese Werte aufweist. 

## <a name="gremlin-steps"></a>Gremlin-Schritte
Sehen wir uns nun die Gremlin-Schritte an, die von Azure Cosmos DB unterstützt werden. Eine vollständige Referenz zu Gremlin finden Sie in der [TinkerPop-Referenz](http://tinkerpop.apache.org/docs/current/reference).

| Schritt | Beschreibung | Dokumentation zu TinkerPop 3.2 | Hinweise |
| --- | --- | --- | --- |
| `addE` | Fügt einen Edge zwischen zwei Vertices hinzu | [addE-Schritt](http://tinkerpop.apache.org/docs/current/reference/#addedge-step) | |
| `addV` | Fügt einen Vertex zum Diagramm hinzu | [addV-Schritt](http://tinkerpop.apache.org/docs/current/reference/#addvertex-step) | |
| `as` | Ein Schrittmodulator für die Zuweisung einer Variable zur Ausgabe eines Schritts | [as-Schritt](http://tinkerpop.apache.org/docs/current/reference/#as-step) | |
| `by` | Ein mit `group` und `order` verwendeter Schrittmodulator | [by-Schritt](http://tinkerpop.apache.org/docs/current/reference/#by-step) | |
| `coalesce` | Gibt die erste Traversierung, die ein Ergebnis zurückgibt, zurück | [coalesce-Schritt](http://tinkerpop.apache.org/docs/current/reference/#coalesce-step) | |
| `constant` | Gibt einen konstanten Wert zurück. Wird mit `coalesce` verwendet.| [constant-Schritt](http://tinkerpop.apache.org/docs/current/reference/#constant-step) | |
| `count` | Gibt die Anzahl aus der Traversierung zurück | [count-Schritt](http://tinkerpop.apache.org/docs/current/reference/#count-step) | |
| `dedup` | Gibt die Werte mit entfernten Duplikaten zurück | [dedup-Schritt](http://tinkerpop.apache.org/docs/current/reference/#dedup-step) | |
| `drop` | Löscht die Werte (Vertex/Edge) | [drop-Schritt](http://tinkerpop.apache.org/docs/current/reference/#drop-step) | |
| `fold` | Fungiert als Grenze, die Ergebnisse berechnet und zusammenfasst| [fold-Schritt](http://tinkerpop.apache.org/docs/current/reference/#fold-step) | |
| `group` | Gruppiert die Werte basierend auf den angegebenen Bezeichnungen| [group-Schritt](http://tinkerpop.apache.org/docs/current/reference/#group-step) | |
| `has` | Wird zum Filtern von Eigenschaften, Vertices und Edges verwendet. Unterstützt die Varianten `hasLabel`, `hasId`, `hasNot` und `has`. | [has-Schritt](http://tinkerpop.apache.org/docs/current/reference/#has-step) | |
| `inject` | Fügt Werte in einen Stream ein| [inject-Schritt](http://tinkerpop.apache.org/docs/current/reference/#inject-step) | |
| `is` | Dient zur Ausführung eines Filters mithilfe eines booleschen Ausdrucks | [is-Schritt](http://tinkerpop.apache.org/docs/current/reference/#is-step) | |
| `limit` | Dient zur Beschränkung der Anzahl der Elemente in der Traversierung| [limit-Schritt](http://tinkerpop.apache.org/docs/current/reference/#limit-step) | |
| `local` | Umschließt lokal einen Abschnitt einer Traversierung, ähnlich wie bei einer Unterabfrage | [local-Schritt](http://tinkerpop.apache.org/docs/current/reference/#local-step) | |
| `not` | Dient zur Negierung eines Filters | [not-Schritt](http://tinkerpop.apache.org/docs/current/reference/#not-step) | |
| `optional` | Gibt das Ergebnis der angegebenen Traversierung zurück, wenn diese ein anderes Ergebnis als das vom aufrufenden Element zurückgegebene Ergebnis liefert | [optional-Schritt](http://tinkerpop.apache.org/docs/current/reference/#optional-step) | |
| `or` | Stellt sicher, dass mindestens einer der Traversierungen einen Wert zurückgibt | [or-Schritt](http://tinkerpop.apache.org/docs/current/reference/#or-step) | |
| `order` | Gibt die Ergebnisse in der angegebenen Sortierreihenfolge zurück | [order-Schritt](http://tinkerpop.apache.org/docs/current/reference/#order-step) | |
| `path` | Gibt den vollständigen Pfad der Traversierung zurück | [path-Schritt](http://tinkerpop.apache.org/docs/current/reference/#path-step) | |
| `project` | Projiziert die Eigenschaften als Karte | [project-Schritt](http://tinkerpop.apache.org/docs/current/reference/#project-step) | |
| `properties` | Gibt die Eigenschaften für die angegebenen Bezeichnungen zurück | [properties-Schritt](http://tinkerpop.apache.org/docs/current/reference/#properties-step) | |
| `range` | Filtert auf den angegebenen Wertebereich| [range-Schritt](http://tinkerpop.apache.org/docs/current/reference/#range-step) | |
| `repeat` | Wiederholt den Schritt für die angegebene Anzahl von Versuchen. Wird für die Ausführung als Schleife verwendet. | [repeat-Schritt](http://tinkerpop.apache.org/docs/current/reference/#repeat-step) | |
| `sample` | Wird zum Testen von Ergebnissen aus der Traversierung verwendet | [sample-Schritt](http://tinkerpop.apache.org/docs/current/reference/#sample-step) | |
| `select` | Wird zum Projizieren von Ergebnissen aus der Traversierung verwendet |  [select-Schritt](http://tinkerpop.apache.org/docs/current/reference/#select-step) | |
| `store` | Wird für nicht blockierende Aggregate aus der Traversierung verwendet | [store-Schritt](http://tinkerpop.apache.org/docs/current/reference/#store-step) | |
| `tree` | Aggregiert Pfade aus einem Vertex in einer Struktur | [tree-Schritt](http://tinkerpop.apache.org/docs/current/reference/#tree-step) | |
| `unfold` | Löst einen Iterator als Schritt auf| [unfold-Schritt](http://tinkerpop.apache.org/docs/current/reference/#unfold-step) | |
| `union` | Führt Ergebnisse aus mehreren Traversierungen zusammen| [union-Schritt](http://tinkerpop.apache.org/docs/current/reference/#union-step) | |
| `V` | Enthält die erforderlichen Schritte für Traversierungen zwischen Vertices und Edges: `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV` und `otherV` für | [vertex-Schritte](http://tinkerpop.apache.org/docs/current/reference/#vertex-steps) | |
| `where` | Wird zum Filtern von Ergebnissen aus der Traversierung verwendet. Unterstützt die Operatoren `eq`, `neq`, `lt`, `lte`, `gt`, `gte` und `between`.  | [where-Schritt](http://tinkerpop.apache.org/docs/current/reference/#where-step) | |

Das für Schreibvorgänge optimierte Modul von Azure Cosmos DB unterstützt standardmäßig die automatische Indizierung aller Eigenschaften in Vertices und Edges. Daher werden Abfragen mit Filtern, Bereichsabfragen, Sortierungen oder Aggregate von Eigenschaften über den Index verarbeitet und effizient übermittelt. Weitere Informationen zur Indizierung in Azure Cosmos DB finden Sie in unserem Dokument unter [Schemagnostische Indizierung](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Nächste Schritte
* Erste Schritte zum Erstellen einer Diagrammanwendung [mithilfe unserer SDKs](create-graph-dotnet.md) 
* Informationen über die [Diagrammunterstützung von Azure Cosmos DB](graph-introduction.md)

