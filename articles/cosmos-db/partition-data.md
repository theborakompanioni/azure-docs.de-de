---
title: Partitionierung und horizontale Skalierung in Azure Cosmos DB | Microsoft-Dokumentation
description: "Erfahren Sie, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung auswählen."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: 5f501bdb0a3c478a436d729dfe045ad8e39bd3bb
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


---

# <a name="how-to-partition-and-scale-in-azure-cosmos-db"></a>Partitionieren und Skalieren in Azure Cosmos DB

[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ist ein globaler, verteilter Datenbankdienst, der mehrere Modelle unterstützt und dafür konzipiert wurde, dass Sie eine schnelle, vorhersagbare Leistung für Ihre Anwendung erzielen und diese während des Wachstums nahtlos skalieren können. Dieser Artikel erläutert, wie die Partitionierung für all die verschiedenen Datenmodelle in Azure Cosmos DB funktioniert und wie Sie Azure Cosmos DB-Container konfigurieren können, um Ihre Anwendungen effektiv zu skalieren.

Partitionierung und Partitionsschlüssel werden auch im folgenden Azure Friday-Video mit Scott Hanselman und Shireesh Thota (Cosmos DB Principal Engineering Manager) behandelt.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Partitionierung in Azure Cosmos DB
In Azure Cosmos DB können Sie schemalose Daten jeder Größe innerhalb von Millisekunden speichern und abfragen. Cosmos DB stellt Container für das Speichern von Daten bereit, die als **Sammlungen (für Dokumente), Diagramme oder Tabellen** bezeichnet werden. Container sind logische Ressourcen und können eine oder mehrere physische Partitionen bzw. einen oder mehrere Server umfassen. Die Anzahl der Partitionen wird von Cosmos DB auf Basis der Speichergröße und des bereitgestellten Durchsatzes des Containers bestimmt. Jede Partition in Cosmos DB verfügt über eine festgelegte Menge an zugeordnetem SSD-gestütztem Speicher und wird für eine hohe Verfügbarkeit repliziert. Die Partitionsverwaltung erfolgt vollständig über Azure Cosmos DB, Sie müssen also weder komplexen Code schreiben noch Ihre Partitionen verwalten. Für Cosmos DB-Container gelten keine Obergrenzen hinsichtlich Speicherplatz und Durchsatz. 

![Horizontal](./media/introduction/azure-cosmos-db-partitioning.png) 

Die Partitionierung ist für Ihre Anwendung transparent. Cosmos DB unterstützt schnelle Lese- und Schreibvorgänge, Abfragen, Transaktionslogik, Konsistenzebenen und eine präzise Zugriffssteuerung über Methoden- bzw. API-Aufrufe einer einzigen Containerressource. Der Dienst übernimmt die Verteilung von Daten über Partitionen sowie das Routing von Abfrage-Anforderungen an die richtige Partition. 

Wie funktioniert die Partitionierung? Jedes Element muss zur eindeutigen Identifizierung über einen Partitionsschlüssel und einen Zeilenschlüssel verfügen. Der Partitionsschlüssel fungiert als logische Partition für Ihre Daten und bietet Cosmos DB stellt eine natürliche Grenze für die Verteilung von Daten über Partitionen hinweg. So funktioniert die Partitionierung in Azure Cosmos DB:

* Sie stellen einen Cosmos DB-Container mit einem Durchsatz von `T` Anforderungen/Sek. bereit.
* Im Hintergrund stellt Cosmos DB die Partitionen bereit, die zum Verarbeiten von `T` Anforderungen/Sek. erforderlich sind. Wenn `T` höher ist als der maximale Durchsatz pro Partition `t`, stellt Cosmos DB `N` = `T/t` Partitionen bereit.
* Cosmos DB ordnet den Schlüsselbereich der Partitionsschlüsselhashes gleichmäßig zu den `N` Partitionen zu. Daher hostet jede (physische) Partition 1-N Partitionsschlüsselwerte (logische Partitionen).
* Wenn eine physische Partition `p` das Speicherlimit erreicht, teilt Cosmos DB `p` nahtlos in zwei neue Partitionen `p1` und `p2` auf und verteilt die Werte ungefähr gemäß der Hälfte der Schlüssel auf beide Partitionen. Dieser Aufteilungsvorgang ist für Ihre Anwendung nicht sichtbar.
* Ähnliches gilt, wenn Sie mehr als `t*N` Durchsatz bereitstellen: Cosmos DB teilt mindestens eine Ihrer Partitionen auf, um den höheren Durchsatz zu unterstützen.

Die jeweilige Semantik für Partitionsschlüssel ist je nach Semantik der verschiedenen APIs geringfügig unterschiedlich, wie in der folgenden Tabelle dargestellt:

| API | Partitionsschlüssel | Zeilenschlüssel |
| --- | --- | --- |
| DocumentDB | Benutzerdefinierter Partitionsschlüsselpfad | `id` (feststehend) | 
| MongoDB | Benutzerdefinierter Shardschlüssel  | `_id` (feststehend) | 
| Grafik | Benutzerdefinierte Partitionsschlüsseleigenschaft | `id` (feststehend) | 
| Table | `PartitionKey` (feststehend) | `RowKey` (feststehend) | 

Cosmos DB arbeitete mit hashbasierter Partitionierung. Wenn Sie ein Element schreiben, erstellt Cosmos DB einen Hashwert für den Partitionsschlüsselwert und verwendet das Hashergebnis, um zu ermitteln, in welcher Partition das Element gespeichert werden soll. Cosmos DB speichert alle Elemente mit dem gleichen Partitionsschlüssel in der gleichen physischen Partition. Die Auswahl des Partitionsschlüssels ist eine wichtige Entscheidung, die Sie zur Entwurfszeit treffen müssen. Sie müssen einen Eigenschaftsnamen auswählen, der eine Vielzahl von Werten zulässt und gleichmäßige Zugriffsmuster aufweist.

> [!NOTE]
> Es hat sich bewährt, einen Partitionsschlüssel mit vielen unterschiedlichen Werten (mindestens mehrere Hundert oder mehrere Tausend) auszuwählen.
>

Azure Cosmos DB-Container können „festgelegt“ oder „unbegrenzt“ erstellt werden. Container mit fester Größe weisen eine Obergrenze von 10 GB und 10.000 RUs/Sek. (Request Units, Anforderungseinheiten) auf. Bei einigen APIs kann der Partitionsschlüssel für Container mit fester Größe weggelassen werden. Um einen unbegrenzten Container zu erstellen, müssen Sie einen Mindestdurchsatz von 2.500 RU/Sek. angeben.

## <a name="partitioning-and-provisioned-throughput"></a>Partitionierung und bereitgestellter Durchsatz
Cosmos DB ist auf vorhersagbare Leistung ausgelegt. Wenn Sie einen Container erstellen, reservieren Sie Durchsatz in Form von **[Anforderungseinheiten](request-units.md) pro Sekunde mit einem möglichen zusätzlichen RU-Wert pro Minute**. Jeder Anforderung wird eine Gebühr für Anforderungseinheiten zugewiesen, die sich proportional zur vom Vorgang genutzten Menge an Systemressourcen wie CPU, Arbeitsspeicher und E/A verhält. Der Lesevorgang eines 1 KB großen Dokuments mit Sitzungskonsistenz beansprucht eine Anforderungseinheit. Ein Lesevorgang entspricht einer RU, unabhängig von der Anzahl der gespeicherten Elemente oder der Anzahl gleichzeitiger Anforderungen, die parallel ausgeführt werden. Größere Elemente erfordern mehr Anforderungseinheiten. Wenn Sie die Größe Ihrer Entitäten sowie die von Ihrer Anwendung benötigte Anzahl an Lesevorgängen kennen, können Sie Ihrer Anwendung für den Lesevorgang exakt den benötigten Durchsatz bereitstellen. 

> [!NOTE]
> Um den vollständigen Durchsatz des Containers zu erreichen, müssen Sie einen Partitionsschlüssel auswählen, der Ihnen ermöglicht, Anforderungen gleichmäßig zwischen einigen unterschiedlichen Partitionsschlüsselwerten zu verteilen.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-the-azure-cosmos-db-apis"></a>Arbeiten mit den Azure Cosmos DB-APIs
Sie können das Azure-Portal oder die Azure-Befehlszeilenschnittstelle verwenden, um Container zu erstellen und jederzeit zu skalieren. Dieser Abschnitt zeigt, wie Sie Container erstellen und den Durchsatz sowie die Partitionsschlüsseldefinition in jeder der unterstützten APIs angeben.

### <a name="documentdb-api"></a>DocumentDB-API
Das folgende Beispiel zeigt, wie Sie mit der DocumentDB-API einen Container (bzw. eine Sammlung) erstellen. Weitere Informationen erhalten Sie unter [Partitionieren mit der DocumentDB-API](partition-data.md).

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Sie können ein Element (Dokument) mit der `GET`-Methode in der REST-API oder mithilfe von `ReadDocumentAsync` in einem der SDKs lesen.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>MongoDB-API
Bei der MongoDB-API können Sie eine Sammlung mit Shards mithilfe Ihrer bevorzugten Tools, Treiber oder SDKs erstellen. In diesem Beispiel verwenden wir die Mongo Shell zum Erstellen der Sammlung.

In der Mongo Shell:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Ergebnisse:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Tabellen-API

Bei der Tabellen-API geben Sie den Durchsatz für Tabellen in der appSettings-Konfiguration für Ihre Anwendung an:

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

Dann können Sie mit dem Azure Table Storage-SDK die Tabelle erstellen. Der Partitionsschlüssel wird implizit als `PartitionKey`-Wert erstellt. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

Mit folgendem Codeausschnitt können Sie eine einzelne Entität abrufen:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Weitere Informationen finden Sie unter [Entwickeln mit der Tabellen-API](tutorial-develop-table-dotnet.md).

### <a name="graph-api"></a>Graph-API

Bei der Graph-API müssen Sie das Azure-Portal oder die Azure-Befehlszeilenschnittstelle verwenden, um Container zu erstellen. Da Azure Cosmos DB mehrere Modelle unterstützt, können Sie alternativ dazu auch eines der anderen Modelle verwenden, um Ihren Graph-Container zu erstellen und zu skalieren.

Mit dem Partitionsschlüssel und der Partitions-ID in Gremlin können Sie jeden beliebigen Scheitelpunkt- oder Randwert lesen. Bei einem Graphen mit der Region („USA) als Partitionsschlüssel und „Seattle“ als Zeilenschlüssel können Sie einen Scheitelpunkt beispielsweise mit folgender Syntax suchen:

```
g.V(['USA', 'Seattle'])
```

Das Gleiche gilt für Randwerte: Sie können mithilfe des Partitions- und des Zeilenschlüssels auf einen Randwert verweisen.

```
g.E(['USA', 'I5'])
```

Weitere Informationen finden Sie unter [Gremlin-Unterstützung für Cosmos DB](gremlin-support.md).


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a>Entwerfen für Partitionierung
Um bei Azure Cosmos DB eine effektive Skalierung zu erzielen, müssen Sie beim Erstellen Ihres Containers einen sinnvollen Partitionsschlüssel auswählen. Bei der Auswahl des Partitionsschlüssels müssen zwei wichtige Überlegungen angestellt werden:

* **Grenzen für Abfragen und Transaktionen**: Sie sollten den Partitionsschlüssel so wählen, dass ein Gleichgewicht zwischen der reibungslosen Ausführbarkeit von Transaktionen und der Verteilung Ihrer Entitäten auf mehrere Partitionsschlüssel gegeben ist, um eine skalierbare Lösung zu ermöglichen. An einem Ende der Skala könnten Sie denselben Partitionsschlüssel für alle Elemente festlegen, dies könnte aber die Skalierbarkeit Ihrer Lösung beeinträchtigen. Am anderen Ende der Skala könnten Sie jedem Dokument einen eindeutigen Partitionsschlüssel zuweisen. Dadurch würden Sie zwar eine hohe Skalierbarkeit erzielen, könnten aber keine dokumentübergreifenden Transaktionen über gespeicherte Prozeduren und Trigger verwenden. Ein idealer Partitionsschlüssel ermöglicht die Verwendung von effizienten Abfragen und verfügt über eine ausreichende Kardinalität, um sicherzustellen, dass Ihre Lösung skalierbar ist. 
* **Keine Speicher- und Leistungsengpässe**: Es ist wichtig, eine Eigenschaft auszuwählen, die eine gleichmäßige Verteilung von Schreibvorgängen auf verschiedene unterschiedliche Werte ermöglicht. Anforderungen an den gleichen Partitionsschlüssel dürfen den Durchsatz einer einzelnen Partition nicht überschreiten und werden gedrosselt. Daher ist es wichtig, einen Partitionsschlüssel auszuwählen, der nicht zu „Hotspots“ innerhalb Ihrer Anwendung führt. Da alle Daten für einen einzelnen Partitionsschlüssel innerhalb einer Partition gespeichert werden müssen, sollten Sie möglichst keine Partitionsschlüssel verwenden, die große Mengen an Daten für den gleichen Wert besitzen. 

Im Folgenden sehen wir uns einige Szenarien aus dem echten Leben sowie sinnvolle Partitionsschlüssel für jedes Szenario an:
* Wenn Sie ein Benutzerprofil-Back-End implementieren, ist die Benutzer-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie IoT-Daten speichern, z.B. zum Gerätezustand, ist eine Geräte-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie DocumentDB für die Protokollierung von Zeitreihendaten verwenden, ist der Hostname oder die Prozess-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie über eine mehrinstanzenfähige Architektur verfügen, ist die Mandanten-ID eine gute Wahl für Partitionsschlüssel.

In einigen Fällen – z.B. bei IoT und Benutzerprofilen – kann der Partitionsschlüssel mit Ihrer ID (Dokumentenschlüssel) identisch sein. In anderen Fällen, wie etwa bei den Zeitreihendaten, kann sich der Partitionsschlüssel von der ID unterscheiden.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionierung und Protokollierung/Zeitreihendaten
Gängige Anwendungsfälle von Cosmos DB sind Protokollierung und Telemetrie. Es ist wichtig, einen guten Partitionsschlüssel auswählen, da Sie möglicherweise große Datenmengen lesen oder schreiben müssen. Die Auswahl hängt von Ihren Lese- und Schreibraten und den zu erwartenden Abfragen ab. Im Folgenden finden Sie einige Tipps zum Auswählen eines guten Partitionsschlüssels.

* Wenn Ihr Anwendungsfall nur wenige Schreibvorgänge umfasst, die sich über längere Zeit ansammeln, und Sie Abfragen anhand von Zeitstempelbereichen und anderen Filtern durchführen müssen, ist ein Rollup des Zeitstempels, z.B. mit dem Datum als Partitionsschlüssel, ein guter Ansatz. Dadurch können Sie Abfragen über alle Daten für ein Datum von einer einzigen Partition ausführen. 
* Wenn Ihre Workload sehr viele Schreibvorgänge umfasst – der häufigere Fall –, sollten Sie einen nicht auf timestamp basierenden Partitionsschlüssel verwenden, sodass Cosmos DB die Schreibvorgänge gleichmäßig auf verschiedene Partitionen verteilen kann. Hierbei sind Hostname, Prozess-ID, Aktivitäts-ID oder eine andere Eigenschaft mit hoher Kardinalität eine gute Wahl. 
* Eine dritte Möglichkeit ist ein Hybridansatz mit mehreren Containern – einem für jeden Tag des Monats –, bei dem der Partitionsschlüssel eine differenzierte Eigenschaft wie z.B. der Hostname ist. Dies hat den Vorteil, dass Sie unterschiedliche Durchsätze entsprechend dem Zeitfenster festlegen können: Beispielsweise kann der Container für den aktuellen Monat mit höherem Durchsatz bereitgestellt werden, da er für Lese- und Schreibvorgänge verwendet wird, während in vorherigen Monaten ein geringerer Durchsatz ausreichend war, da nur Lesevorgänge ausgeführt wurden.

### <a name="partitioning-and-multi-tenancy"></a>Partitionierung und Mehrinstanzenfähigkeit
Wenn Sie mit Cosmos DB eine mehrinstanzenfähige Anwendung implementieren, gibt es zwei gängige Muster: „ein Partitionsschlüssel pro Mandant“ und „ein Container pro Mandant“. Hier sind ihre Vor- und Nachteile auflistet:

* Ein Partitionsschlüssel pro Mandant: In diesem Modell sind Mandanten innerhalb eines einzelnen Containers verbunden. Abfragen und Einfügungen für Elemente innerhalb eines einzelnen Mandanten können jedoch für eine einzelne Partition ausgeführt werden. Sie können auch Transaktionslogik über alle Elemente innerhalb eines Mandanten hinweg implementieren. Da mehrere Mandanten einen Container gemeinsam nutzen, können Sie Kosten für Speicher und Durchsatz sparen, indem Sie Ressourcen für Mandanten in einem einzigen Container zusammenfassen, anstatt für jeden Mandanten zusätzlichen Spielraum bereitzustellen. Der Nachteil darin besteht, dass Sie über keine Leistungsisolation pro Mandant verfügen. Im Vergleich zu gezielten Steigerungen für Mandanten werden Erhöhungen von Leistung/Durchsatz auf den gesamten Container angewendet.
* Ein Container pro Mandant: Für jeden Mandanten ist ein getrennter Container vorhanden. In diesem Modell können Sie die Leistung pro Mandant reservieren. Mit dem neuen Preismodell für die Bereitstellung von Cosmos DB ist dieses Modell kosteneffektiver für mehrinstanzenfähige Anwendungen mit wenigen Mandanten.

Sie können auch einen kombinierten/abgestuften Ansatz verwenden, in dem kleine Mandanten zusammengefasst und größere Mandanten in einen eigenen Container migriert werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Übersicht über Konzepte und bewährte Methoden für die Partitionierung mit einer beliebigen Azure Cosmos DB-API erhalten. 

* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).




