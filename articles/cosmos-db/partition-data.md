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
ms.date: 08/29/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: f2ac16c2f514aaa7e3f90fdf0d0b6d2912ef8485
ms.openlocfilehash: 6f272136d535dddd9c8213293841ace203c042a1
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---

# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partitionieren und Skalieren in Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ist ein weltweit verteilter Datenbankdienst mit mehreren Modellen, der Ihnen helfen soll, eine schnelle und vorhersagbare Leistung zu erzielen. Er wird nahtlos zusammen mit Ihrer Anwendung skaliert, während diese wächst. Dieser Artikel bietet eine Übersicht darüber, wie die Partitionierung für alle Datenmodelle in Azure Cosmos DB funktioniert. Außerdem wird beschrieben, wie Sie Azure Cosmos DB-Container zum effektiven Skalieren Ihrer Anwendungen konfigurieren können.

Partitionierung und Partitionsschlüssel werden im folgenden Azure Friday-Video mit Scott Hanselman und Shireesh Thota (Azure Cosmos DB Principal Engineering Manager) behandelt:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Partitionierung in Azure Cosmos DB
In Azure Cosmos DB können Sie schemalose Daten jeder Größe innerhalb von Millisekunden speichern und abfragen. Azure Cosmos DB stellt Container für das Speichern von Daten bereit, die als *Sammlungen* (für Dokumente), *Graphen* oder *Tabellen* bezeichnet werden. Container sind logische Ressourcen und können eine oder mehrere physische Partitionen bzw. einen oder mehrere Server umfassen. Die Anzahl der Partitionen wird von Azure Cosmos DB auf Basis der Speichergröße und des bereitgestellten Durchsatzes des Containers bestimmt. Jede Partition in Azure Cosmos DB verfügt über eine festgelegte Menge an zugeordnetem SSD-gestütztem Speicher und wird für eine hohe Verfügbarkeit repliziert. Die Partitionsverwaltung erfolgt vollständig über Azure Cosmos DB, Sie müssen also weder komplexen Code schreiben noch Ihre Partitionen verwalten. Für Azure Cosmos DB-Container gelten keine Obergrenzen hinsichtlich Speicherplatz und Durchsatz. 

![Ressourcenpartitionierung](./media/introduction/azure-cosmos-db-partitioning.png) 

Die Partitionierung ist für Ihre Anwendung transparent. Azure Cosmos DB unterstützt schnelle Lese- und Schreibvorgänge, Abfragen, Transaktionslogik, Konsistenzebenen und eine präzise Zugriffssteuerung über Methoden- bzw. API-Aufrufe einer einzigen Containerressource. Der Dienst übernimmt die Verteilung von Daten über Partitionen sowie das Routing von Abfrage-Anforderungen an die richtige Partition. 

Wie funktioniert die Partitionierung? Jedes Element muss zur eindeutigen Identifizierung über einen Partitionsschlüssel und einen Zeilenschlüssel verfügen. Der Partitionsschlüssel fungiert als logische Partition für Ihre Daten und bietet Azure Cosmos DB eine natürliche Grenze für die Verteilung von Daten über Partitionen hinweg. So funktioniert die Partitionierung in Azure Cosmos DB:

* Sie stellen einen Azure Cosmos DB-Container mit einem Durchsatz von `T` Anforderungen/Sek. bereit.
* Im Hintergrund stellt Azure Cosmos DB die Partitionen bereit, die zum Verarbeiten von `T` Anforderungen/Sek. erforderlich sind. Wenn `T` höher ist als der maximale Durchsatz pro Partition `t`, stellt Azure Cosmos DB `N` = `T/t` Partitionen bereit.
* Azure Cosmos DB ordnet den Schlüsselbereich der Partitionsschlüsselhashes den `N` Partitionen gleichmäßig zu. Daher hostet jede (physische) Partition 1-N Partitionsschlüsselwerte (logische Partitionen).
* Wenn eine physische Partition `p` ihren Speichergrenzwert erreicht, teilt Azure Cosmos DB `p` nahtlos in zwei neue Partitionen (`p1` und `p2`) auf. Die Werte werden ungefähr zur Hälfte der Schlüssel auf die Partitionen aufgeteilt. Dieser Aufteilungsvorgang ist für Ihre Anwendung nicht sichtbar.
* Ähnliches gilt, wenn Sie einen höheren Durchsatz als `t*N` bereitstellen: Azure Cosmos DB teilt mindestens eine Ihrer Partitionen auf, um den höheren Durchsatz zu unterstützen.

Die jeweilige Semantik für Partitionsschlüssel ist je nach Semantik der verschiedenen APIs geringfügig unterschiedlich, wie in der folgenden Tabelle dargestellt:

| API | Partitionsschlüssel | Zeilenschlüssel |
| --- | --- | --- |
| Azure Cosmos DB | Benutzerdefinierter Partitionsschlüsselpfad | `id` (feststehend) | 
| MongoDB | Gemeinsam verwendeter Schlüssel  | `_id` (feststehend) | 
| Graph | Benutzerdefinierte Partitionsschlüsseleigenschaft | `id` (feststehend) | 
| Table | `PartitionKey` (feststehend) | `RowKey` (feststehend) | 

Azure Cosmos DB arbeitet mit hashbasierter Partitionierung. Wenn Sie ein Element schreiben, erstellt Azure Cosmos DB einen Hashwert für den Partitionsschlüsselwert und verwendet das Hashergebnis, um zu ermitteln, in welcher Partition das Element gespeichert werden soll. Azure Cosmos DB speichert alle Elemente mit dem gleichen Partitionsschlüssel in der gleichen physischen Partition. Die Auswahl des Partitionsschlüssels ist eine wichtige Entscheidung, die Sie zur Entwurfszeit treffen müssen. Sie müssen einen Eigenschaftsnamen auswählen, der eine Vielzahl von Werten zulässt und gleichmäßige Zugriffsmuster aufweist.

> [!NOTE]
> Es hat sich bewährt, einen Partitionsschlüssel mit vielen unterschiedlichen Werten (mindestens Hunderte oder Tausende) auszuwählen.
>

Azure Cosmos DB-Container können *feststehend* oder *unbegrenzt* erstellt werden. Container mit fester Größe weisen eine Obergrenze von 10 GB und 10.000 RUs/Sek. (Request Units, Anforderungseinheiten) auf. Bei einigen APIs kann der Partitionsschlüssel für Container mit fester Größe weggelassen werden. Um einen unbegrenzten Container zu erstellen, müssen Sie einen Mindestdurchsatz von 2.500 RU/s angeben.

## <a name="partitioning-and-provisioned-throughput"></a>Partitionierung und bereitgestellter Durchsatz
Azure Cosmos DB ist auf vorhersagbare Leistung ausgelegt. Wenn Sie einen Container erstellen, reservieren Sie Durchsatz hinsichtlich der *[Anforderungseinheiten](request-units.md) (Request Units, RUs) pro Sekunde*. Jeder Anforderung wird eine Gebühr für Anforderungseinheiten zugewiesen, die sich proportional zur vom Vorgang genutzten Menge an Systemressourcen wie CPU, Arbeitsspeicher und E/A verhält. Der Lesevorgang eines Dokuments der Größe 1 KB mit einer Sitzungskonsistenz beansprucht eine Anforderungseinheit. Ein Lesevorgang entspricht einer RU, unabhängig von der Anzahl der gespeicherten Elemente oder der Anzahl gleichzeitiger Anforderungen, die parallel ausgeführt werden. Größere Elemente erfordern mehr Anforderungseinheiten. Wenn Sie die Größe Ihrer Entitäten sowie die von Ihrer Anwendung benötigte Anzahl an Lesevorgängen kennen, können Sie Ihrer Anwendung für den Lesevorgang exakt den benötigten Durchsatz bereitstellen. 

> [!NOTE]
> Um den vollständigen Durchsatz des Containers zu erreichen, müssen Sie einen Partitionsschlüssel auswählen, der Ihnen ermöglicht, Anforderungen gleichmäßig zwischen einigen unterschiedlichen Partitionsschlüsselwerten zu verteilen.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="work-with-the-azure-cosmos-db-apis"></a>Arbeiten mit den Azure Cosmos DB-APIs
Sie können das Azure-Portal oder die Azure-Befehlszeilenschnittstelle verwenden, um Container zu erstellen und jederzeit zu skalieren. Dieser Abschnitt zeigt, wie Sie Container erstellen und den Durchsatz sowie die Partitionsschlüsseldefinition in jeder der unterstützten APIs angeben.

### <a name="azure-cosmos-db-api"></a>Azure Cosmos DB-API
Das folgende Beispiel zeigt, wie Sie mit der Azure Cosmos DB-API einen Container (bzw. eine Sammlung) erstellen. 

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

### <a name="table-api"></a>Tabelle-API

Bei der Tabellen-API geben Sie den Durchsatz für Tabellen in der appSettings-Konfiguration für Ihre Anwendung an.

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

Mit der Graph-API müssen Sie das Azure-Portal oder die Azure-Befehlszeilenschnittstelle verwenden, um Container zu erstellen. Da Azure Cosmos DB mehrere Modelle unterstützt, können Sie alternativ dazu auch eines der anderen Modelle verwenden, um Ihren Graph-Container zu erstellen und zu skalieren.

Mit dem Partitionsschlüssel und der Partitions-ID in Gremlin können Sie jeden beliebigen Scheitelpunkt- oder Randwert lesen. Bei einem Graphen mit der Region „USA“ als Partitionsschlüssel und „Seattle“ als Zeilenschlüssel können Sie einen Scheitelpunkt beispielsweise mit folgender Syntax suchen:

```
g.V(['USA', 'Seattle'])
```

Sie können mithilfe des Partitions- und des Zeilenschlüssels auf einen Randwert verweisen.

```
g.E(['USA', 'I5'])
```

Weitere Informationen finden Sie unter [Gremlin-Unterstützung für Azure Cosmos DB](gremlin-support.md).


<a name="designing-for-partitioning"></a>
## <a name="design-for-partitioning"></a>Entwerfen für Partitionierung
Um bei Azure Cosmos DB eine effektive Skalierung zu erzielen, müssen Sie beim Erstellen Ihres Containers einen sinnvollen Partitionsschlüssel auswählen. Bei der Auswahl des Partitionsschlüssels müssen zwei wichtige Überlegungen angestellt werden:

* **Die Grenze für Abfragen und Transaktionen**. Sie sollten Ihren Partitionsschlüssel so wählen, dass Transaktionen vorgenommen werden können und zugleich die Skalierbarkeit der Lösung gegeben ist (durch Verteilung Ihrer Entitäten auf mehrere Partitionsschlüssel). An einem Ende der Skala könnten Sie denselben Partitionsschlüssel für alle Elemente festlegen, dies könnte aber die Skalierbarkeit Ihrer Lösung beeinträchtigen. Andererseits können Sie jedem Element einen eindeutigen Partitionsschlüssel zuweisen. Diese Auswahl ist hochgradig skalierbar, verhindert aber, dass Sie dokumentübergreifende Transaktionen über gespeicherte Prozeduren und Trigger verwenden können. Ein idealer Partitionsschlüssel ermöglicht die Verwendung von effizienten Abfragen und verfügt über eine ausreichende Kardinalität, um sicherzustellen, dass Ihre Lösung skalierbar ist. 
* **Keine Speicher- und Leistungsengpässe**. Es ist wichtig, eine Eigenschaft auszuwählen, die eine gleichmäßige Verteilung von Schreibvorgängen auf verschiedene unterschiedliche Werte ermöglicht. Anforderungen an den gleichen Partitionsschlüssel dürfen den Durchsatz einer einzelnen Partition nicht überschreiten und werden gedrosselt. Daher ist es wichtig, einen Partitionsschlüssel auszuwählen, der nicht zu „Hotspots“ innerhalb Ihrer Anwendung führt. Da alle Daten für einen einzelnen Partitionsschlüssel innerhalb einer Partition gespeichert werden müssen, sollten Sie möglichst keine Partitionsschlüssel verwenden, die große Mengen an Daten für den gleichen Wert besitzen. 

Im Folgenden sehen wir uns einige Szenarien aus dem echten Leben sowie sinnvolle Partitionsschlüssel für jedes Szenario an:
* Wenn Sie ein Benutzerprofil-Back-End implementieren, ist die Benutzer-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie IoT-Daten speichern, z.B. zum Gerätezustand, ist eine Geräte-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie Azure Cosmos DB für die Protokollierung von Zeitreihendaten verwenden, ist der Hostname oder die Prozess-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie über eine mehrinstanzenfähige Architektur verfügen, ist die Mandanten-ID eine gute Wahl für Partitionsschlüssel.

In einigen Fällen – z.B. bei IoT und Benutzerprofilen – kann der Partitionsschlüssel mit Ihrer ID (Dokumentenschlüssel) identisch sein. In anderen Fällen, wie etwa bei den Zeitreihendaten, kann sich der Partitionsschlüssel von der ID unterscheiden.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionierung und Protokollierung/Zeitreihendaten
Gängige Anwendungsfälle von Azure Cosmos DB sind Protokollierung und Telemetrie. Es ist wichtig, einen guten Partitionsschlüssel auswählen, da Sie möglicherweise große Datenmengen lesen oder schreiben müssen. Die Auswahl hängt von Ihren Lese- und Schreibraten und den zu erwartenden Abfragen ab. Im Folgenden finden Sie einige Tipps zum Auswählen eines guten Partitionsschlüssels:

* Wenn Ihr Anwendungsfall eine kleine Rate von Schreibvorgängen umfasst, die über einen langen Zeitraum anfallen, und Sie Abfragen nach Zeitabschnitten von Zeitstempeln und anderen Filtern durchführen müssen, verwenden Sie einen Rollup des Zeitstempels. Ein guter Ansatz ist beispielsweise die Verwendung des Datums als Partitionsschlüssel. Dadurch können Sie Abfragen über alle Daten für ein Datum von einer einzigen Partition ausführen. 
* Wenn Ihre Workload hoch ist (was häufiger der Fall ist), verwenden Sie einen Partitionsschlüssel, der nicht auf dem Zeitstempel basiert. Bei diesem Ansatz kann Azure Cosmos DB die Schreibvorgänge gleichmäßig auf die verschiedenen Partitionen verteilen. Hierbei sind Hostname, Prozess-ID, Aktivitäts-ID oder eine andere Eigenschaft mit hoher Kardinalität eine gute Wahl. 
* Eine weitere Möglichkeit ist ein Hybridansatz mit mehreren Containern – einem für jeden Tag des Monats –, bei dem der Partitionsschlüssel eine differenzierte Eigenschaft wie z.B. der Hostname ist. Dieser Ansatz hat den Vorteil, dass Sie unterschiedliche Durchsatzraten basierend auf dem Zeitfenster festlegen können. Der Container für den aktuellen Monat wird z.B. mit einem höheren Durchsatz bereitgestellt, da er für Lese- und Schreibvorgänge verwendet wird. Frühere Monate werden mit einem niedrigeren Durchsatz bereitgestellt, da sie nur Lesevorgängen dienen.

### <a name="partitioning-and-multitenancy"></a>Partitionierung und Mehrinstanzenfähigkeit
Wenn Sie mit Azure Cosmos DB eine mehrinstanzenfähige Anwendung implementieren, gibt es zwei gängige Muster: „ein Partitionsschlüssel pro Mandant“ und „ein Container pro Mandant“. Hier sind ihre Vor- und Nachteile auflistet:

* **Ein Partitionsschlüssel pro Mandant**. Bei diesem Modell sind Mandanten innerhalb eines einzelnen Containers verbunden. Abfragen und Einfügungen für Elemente innerhalb eines einzelnen Mandanten können jedoch für eine einzelne Partition ausgeführt werden. Sie können auch Transaktionslogik über alle Elemente innerhalb eines Mandanten hinweg implementieren. Da mehrere Mandanten einen Container gemeinsam nutzen, können Sie Kosten für Speicher und Durchsatz sparen, indem Sie Ressourcen für Mandanten in einem einzigen Container zusammenfassen, anstatt für jeden Mandanten zusätzlichen Spielraum bereitzustellen. Der Nachteil darin besteht, dass Sie über keine Leistungsisolation pro Mandant verfügen. Im Vergleich zu gezielten Steigerungen für Mandanten werden Erhöhungen von Leistung oder Durchsatz auf den gesamten Container angewendet.
* **Ein Container pro Mandant**. Bei diesem Modell hat jeder Mandant einen eigenen Container, und Sie können pro Mandant Leistung reservieren. Mit dem neuen Preismodell für die Bereitstellung von Azure Cosmos DB ist dieses Modell kosteneffizienter für mehrinstanzenfähige Anwendungen mit wenigen Mandanten.

Sie können auch einen kombinierten/abgestuften Ansatz verwenden, in dem kleine Mandanten zusammengefasst und größere Mandanten in einen eigenen Container migriert werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Übersicht über Konzepte und bewährte Methoden für die Partitionierung mit einer beliebigen Azure Cosmos DB-API erhalten. 

* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).




