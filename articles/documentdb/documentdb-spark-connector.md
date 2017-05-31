---
title: "Herstellen einer Verbindung für Apache Spark mit Azure Cosmos DB | Microsoft-Dokumentation"
description: "In diesem Tutorial wird der Azure Cosmos DB-Spark-Connector beschrieben, mit dem Sie für Apache Spark eine Verbindung mit Azure Cosmos DB herstellen können, um verteilte Aggregationen und Data Science-Vorgänge für das global verteilte mehrinstanzenfähige Datenbanksystem für die Cloud durchzuführen."
keywords: Apache Spark
services: cosmosdb
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: denlee
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 6a5db515bec785aa29b29a3096dc20a72056bd08
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a>Beschleunigen der Big Data-Echtzeitanalyse mit dem Spark-Connector für Azure Cosmos DB

Mit dem Spark-Connector für Azure Cosmos DB kann Cosmos DB als Eingabequelle oder Ausgabesenke für Apache Spark-Aufträge fungieren. Durch die Verbindung von [Spark](http://spark.apache.org/) mit [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) haben Sie die Möglichkeit, plötzlich auftretende Data Science-Probleme schnell zu lösen, indem Daten schnell beibehalten und mit Azure Cosmos DB abgefragt werden können. Der Spark-Connector für Azure Cosmos DB nutzt die nativen verwalteten Indizes von Cosmos DB auf effiziente Weise und ermöglicht den Einsatz von aktualisierbaren Spalten für Analysen und der Pushdown-Prädikatfilterung für sich rasant ändernde global verteilte Daten, z. B. in IoT-, Data Science- und Analyseszenarien. 

## <a name="download"></a>Download

Beginnen Sie, indem Sie den Spark-Connector für Azure Cosmos DB (Vorschau) aus dem GitHub-Repository [azure-documentdb-spark](https://github.com/Azure/azure-documentdb-spark/) herunterladen.

## <a name="connector-components"></a>Connectorkomponenten

Der Connector verwendet die folgenden Komponenten:

* [Azure Cosmos DB](http://documentdb.com) ist das mehrinstanzenfähige [global verteilte Datenbanksystem](documentdb-distribute-data-globally.md) von Microsoft, das speziell für die Cloud konzipiert wurde. Mit Azure Cosmos DB können Kunden sowohl den Durchsatz als auch den Speicher in beliebig vielen geografischen Regionen bereitstellen und flexibel skalieren. Der Dienst verfügt über eine garantierte niedrige Wartezeit (99. Perzentil), eine garantierte hohe Verfügbarkeit von 99,99% und [mehrere gut definierte Konsistenzmodelle](documentdb-consistency-levels.md) für Entwickler.

* [Apache Spark](http://spark.apache.org/), ein leistungsstarkes Open-Source-Verarbeitungsmodul, das für hohe Geschwindigkeit, einfache Nutzung und anspruchsvolle Analysen konzipiert wurde. 

* [Apache Spark für Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Sie können Apache Spark in der Cloud für unternehmenskritische Bereitstellungen mit [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/) bereitstellen.

Offiziell unterstützte Versionen:

| Komponente | Version |
|---------|-------|
|Apache Spark|2.0+|
| Scala| 2.11|
| Azure DocumentDB Java SDK | 1.9.6 |
 
Dieser Artikel enthält Informationen zur Ausführung von einigen einfachen Beispielen mithilfe von Python (per pyDocumentDB) und der Scala-Oberfläche.

Es gibt zwei Ansätze zum Verbinden von Apache Spark mit Azure Cosmos DB:
- Verwenden von pyDocumentDB über das [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python)
- Erstellen eines Java-basierten Spark-Connectors für DocumentDB mit dem [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java)

## <a name="pydocumentdb-implementation"></a>pyDocumentDB-Implementierung 
Mit dem aktuellen [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) können wir Spark wie im folgenden Diagramm dargestellt mit Cosmos DB verbinden:

![Datenfluss von Spark zu Cosmos DB über pyCosmos DB](./media/documentdb-spark-connector/azure-documentdb-spark-pydocumentdb.png)


### <a name="data-flow-of-the-pydocumentdb-implementation"></a>Datenfluss der pyDocumentDB-Implementierung

Der Datenfluss läuft wie folgt ab:

1. Es wird eine Verbindung vom Spark-Masterknoten zum Cosmos DB-Gatewayknoten über pyCosmosDB hergestellt.  Beachten Sie, dass der Benutzer nur Spark- und Cosmos DB-Verbindungen angibt. Die Tatsache, dass eine Verbindung mit den jeweiligen Master- und Gatewayknoten hergestellt wird, ist für den Benutzer transparent.
2. Die Abfrage wird für Cosmos DB (über den Gatewayknoten) durchgeführt, wo die Abfrage dann für die Partitionen der Sammlung auf den Datenknoten erfolgt. Die Antwort auf diese Abfragen wird zurück an den Gatewayknoten gesendet, und das Resultset wird an den Spark-Masterknoten zurückgegeben.
3. Alle nachfolgenden Abfragen (z.B. für einen Spark DataFrame) werden zur Verarbeitung an die Spark-Workerknoten gesendet.

Wichtig ist hierbei, dass die Kommunikation zwischen Spark und Cosmos DB auf den Spark-Masterknoten und die Cosmos DB-Gatewayknoten beschränkt ist.  Wie schnell die Abfragen durchgeführt werden, hängt von der Transportschicht zwischen diesen beiden Knoten ab.

### <a name="installing-pydocumentdb"></a>Installieren von pyDocumentDB
Sie können pyDocumentDB auf Ihrem Treiberknoten installieren, indem Sie **pip** verwenden. Beispiel:

```
pip install pyDocumentDB
```


### <a name="connecting-spark-to-cosmos-db-via-pycosmos-db"></a>Verbinden von Spark mit Cosmos DB über pyCosmos DB 
Aufgrund der Einfachheit des Kommunikationstransports ist die Ausführung einer Abfrage von Spark zu Cosmos DB per pyCosmos DB ebenfalls relativ einfach.

Der folgende Codeausschnitt veranschaulicht, wie Sie pyDocumentDB in einem Spark-Kontext verwenden.

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery 
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys to connect to Cosmos DB 
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==' 
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

Im Codeausschnitt ist Folgendes zu sehen:

* Das DocumentDB Python SDK enthält alle erforderlichen Verbindungsparameter, einschließlich der bevorzugten Standorte (Auswahl des Lesereplikats und der Prioritätsreihenfolge).
*  Importieren Sie die erforderlichen Bibliotheken, und konfigurieren Sie Ihren **masterKey** und **host**, um den Cosmos DB-*Client* zu erstellen (**pydocumentdb.document_client**).


### <a name="executing-spark-queries-via-pydocumentdb"></a>Durchführen von Spark-Abfragen per pyDocumentDB
In den folgenden Beispielen wird die im vorherigen Codeausschnitt erstellte Cosmos DB-Instanz verwendet, indem die angegebenen schreibgeschützten Schlüssel genutzt werden.  Mit dem folgenden Codeausschnitt wird eine Verbindung mit der Sammlung **airports.codes** hergestellt (unter dem DoctorWho-Konto, wie zuvor angegeben) und eine Abfrage zum Extrahieren der Städte mit einem Flughafen im Bundesstaat Washington durchgeführt. 

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Push into list `elements`
elements = list(query)
```

Nachdem die Abfrage mit **query** durchgeführt wurde, ist das Ergebnis ein **query_iterable.QueryIterable**-Element, das in eine Python-Liste konvertiert wird. Eine Python-Liste kann mit dem folgenden Code leicht in einen Spark DataFrame konvertiert werden:

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-documentdb"></a>Gründe für die Verwendung von pyDocumentDB zum Herstellen einer Verbindung für Spark mit DocumentDB
Eine Verbindung für Spark mit Cosmos DB per pyCosmos DB wird normalerweise in den folgenden Szenarien hergestellt:

* Sie möchten Python verwenden.
* Sie geben ein relativ kleines Resultset von DocumentDB an Spark zurück.  Beachten Sie, dass das zugrunde liegende Dataset in DocumentDB recht groß sein kann. Es ist eher so, dass Sie Filter auf Ihre DocumentDB-Quelle anwenden (also Prädikatfilter ausführen).  

## <a name="spark-to-cosmos-db-connector"></a>Spark-Connector für Cosmos DB

Der Spark-Connector für Cosmos DB nutzt das [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java) und verschiebt Daten zwischen den Spark-Workerknoten und Cosmos DB. Dies ist im folgenden Diagramm dargestellt:

![Datenfluss im Spark-Connector für Cosmos DB](./media/documentdb-spark-connector/azure-documentdb-spark-connector.png)

Der Datenfluss läuft wie folgt ab:

1. Es wird eine Verbindung vom Spark-Masterknoten zum Cosmos DB-Gatewayknoten hergestellt, um die Partitionszuordnung zu erhalten.  Beachten Sie, dass der Benutzer nur die Spark- und Cosmos DB-Verbindungen angibt. Die Tatsache, dass eine Verbindung mit den jeweiligen Master- und Gatewayknoten hergestellt wird, ist für den Benutzer transparent.
2. Diese Informationen werden dann wieder für den Spark-Masterknoten bereitgestellt.  An diesem Punkt sollten Sie die Abfrage analysieren können, um zu ermitteln, auf welche Partitionen (und ihre Standorte) Sie in Cosmos DB Zugriff haben müssen.
3. Diese Informationen werden an die Spark-Workerknoten übertragen ...
4. So können für die Spark-Workerknoten direkte Verbindungen mit den Cosmos DB-Partitionen hergestellt werden, um die erforderlichen Daten zu extrahieren und sie dann wieder zurück auf die Spark-Partitionen der Spark-Workerknoten zu übertragen.

Hierbei ist der Hinweis wichtig, dass die Kommunikation zwischen Spark und Cosmos DB erheblich schneller abläuft, weil die Datenverschiebung zwischen den Spark-Workerknoten und den Cosmos DB-Datenknoten (Partitionen) erfolgt.

### <a name="building-the-spark-to-cosmos-db-connector"></a>Erstellen des Spark-Connectors für Cosmos DB
Derzeit wird für das Connectorprojekt Maven verwendet. Sie können Folgendes ausführen, um den Connector ohne Abhängigkeiten zu erstellen:
```
mvn clean package
```
Sie können auch die aktuellen JAR-Versionen im Ordner *releases* herunterladen.

### <a name="including-the-azure-documentdb-spark-jar"></a>Einbinden von Azure DocumentDB Spark-JAR-Code
Vor dem Ausführen von Code müssen Sie den Azure DocumentDB Spark-JAR-Code einbinden.  Bei Verwendung der **Spark-Shell** können Sie den JAR-Code mit der Option **--jars** einbinden.  

```
spark-shell --master $master --jars /$location/azure-documentdb-spark-0.0.1-jar-with-dependencies.jar
```

Gehen Sie wie folgt vor, falls Sie den JAR-Code ohne Abhängigkeiten ausführen möchten:

```
spark-shell --master $master --jars /$location/azure-documentdb-spark-0.0.1.jar,/$location/azure-documentdb-1.9.6.jar
```

Wenn Sie einen Notebook-Dienst wie den Azure HDInsight Jupyter Notebook-Dienst verwenden, können Sie die **spark magic**-Befehle nutzen:

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.9.6.jar","wasb:///example/jars/azure-documentdb-spark-0.0.1.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

Mit dem **jars**-Befehl können Sie die beiden JAR-Elemente einbinden, die für **azure-documentdb-spark** benötigt werden (zusätzlich für das Azure DocumentDB Java SDK). **scala-reflect** wird ausgeschlossen, damit kein Konflikt mit den durchgeführten Livy-Aufrufen entsteht (Jupyter Notebook > Livy > Spark).

### <a name="connecting-spark-to-cosmos-db-using-the-connector"></a>Herstellen einer Verbindung für Spark mit Cosmos DB per Connector
Der Kommunikationstransport ist hierbei etwas komplizierter, aber das Durchführen einer Abfrage von Spark an Cosmos DB ist mit dem Connector deutlich schneller.

Der folgende Codeausschnitt veranschaulicht, wie Sie den Connector in einem Spark-Kontext verwenden.

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.documentdb.spark.schema._
import com.microsoft.azure.documentdb.spark._
import com.microsoft.azure.documentdb.spark.config.Config

// Configure connection to your collection
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US2;",
"Collection" -> "flights_pcoll", 
"SamplingRatio" -> "1.0"))
 
// Create collection connection 
val coll = spark.sqlContext.read.DocumentDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Im Codeausschnitt ist Folgendes zu sehen:

- **azure-documentdb-spark** enthält alle erforderlichen Verbindungsparameter, einschließlich der bevorzugten Standorte (z.B. Auswahl des Lesereplikats und der Prioritätsreihenfolge).
- Importieren Sie einfach die erforderlichen Bibliotheken, und konfigurieren Sie Ihren masterKey und host, um den Cosmos DB-Client zu erstellen.

### <a name="executing-spark-queries-via-the-connector"></a>Durchführen von Spark-Abfragen mit dem Connector

Im folgenden Beispiel wird die im vorherigen Codeausschnitt erstellte Cosmos DB-Instanz verwendet, indem die angegebenen schreibgeschützten Schlüssel genutzt werden. Mit dem folgenden Codeausschnitt wird eine Verbindung mit der Sammlung „DepartureDelays.flights_pcoll“ hergestellt (unter dem DoctorWho-Konto, wie zuvor angegeben) und eine Abfrage zum Extrahieren der Informationen zu Flugverspätungen für Flüge durchgeführt, die in Seattle starten.

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-cosmos-db-connector-implementation"></a>Gründe für die Implementierung des Spark-Connectors für Cosmos DB

Eine Verbindung für Spark mit Cosmos DB mit dem Connector wird normalerweise in den folgenden Szenarien hergestellt:

* Sie möchten Scala verwenden (und aktualisieren, um einen Python-Wrapper einzubinden, wie unter [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-documentdb-spark/issues/3) (Problem 3: Hinzufügen eines Python-Wrappers und von Beispielen) beschrieben).
* Sie müssen eine größere Datenmenge zwischen Apache Spark und Cosmos DB übertragen.

Informationen zum Unterschied bei der Abfrageleistung finden Sie im [Query Test Runs wiki](https://github.com/Azure/azure-documentdb-spark/wiki/Query-Test-Runs) (Wiki zu Abfragetestläufen).

## <a name="distributed-aggregation-example"></a>Beispiel für verteilte Aggregation
Dieser Abschnitt enthält einige Beispiele dafür, wie Sie verteilte Aggregationen und Analysen durchführen können, indem Sie Apache Spark und Azure Cosmos DB gemeinsam verwenden.  Beachten Sie Folgendes: Azure Cosmos DB verfügt bereits über Unterstützung für Aggregationen, wie im Blog zum Thema [Weltweite Aggregate mit Azure Cosmos DB](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/) beschrieben. Hier wird erläutert, wie Sie mit Apache Spark die nächste Stufe erreichen.

Beachten Sie, dass sich diese Aggregationen auf das [Notebook für den Spark-Connector für Cosmos DB](https://github.com/Azure/azure-documentdb-spark/blob/master/samples/notebooks/Spark-to-Cosmos DB_Connector.ipynb) beziehen.

### <a name="connecting-to-flights-sample-data"></a>Herstellen einer Verbindung mit Beispieldaten für Flüge
Für diese Aggregationsbeispiele greifen wir auf einige Flugleistungsdaten zu, die in unserer **DoctorWho**-Cosmos DB-Datenbank gespeichert sind.  Verwenden Sie den folgenden Codeausschnitt, um die Verbindung herzustellen:

```
// Import Spark to Cosmos DB connector
import com.microsoft.azure.documentdb.spark.schema._
import com.microsoft.azure.documentdb.spark._
import com.microsoft.azure.documentdb.spark.config.Config

// Connect to Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US 2;",
"Collection" -> "flights_pcoll", 
"SamplingRatio" -> "1.0"))

// Create collection connection 
val coll = spark.sqlContext.read.DocumentDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Mit diesem Codeausschnitt führen wir außerdem eine grundlegende Abfrage durch, mit der die gewünschten gefilterten Daten von Cosmos DB an Spark übertragen werden (hierbei können von Spark verteilte Aggregatvorgänge durchgeführt werden).  In diesem Fall suchen wir nach Flügen, die in Seattle (SEA) starten.

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

Die folgenden Ergebnisse wurden generiert, indem die Abfragen über den Jupyter Notebook-Dienst durchgeführt wurden.  Beachten Sie, dass alle Codeausschnitte generisch und nicht dienstspezifisch sind.

### <a name="running-limit-and-count-queries"></a>Durchführen von LIMIT- und COUNT-Abfragen
Wie Sie es von SQL/Spark SQL gewohnt sind, beginnen wir mit einer **LIMIT**-Abfrage:

![Spark-LIMIT-Abfrage](./media/documentdb-spark-connector/azure-documentdb-spark-sql-query.png)

Die nächste Abfrage ist eine einfache und schnelle **COUNT**-Abfrage:

![Spark-COUNT-Abfrage](./media/documentdb-spark-connector/azure-documentdb-spark-count-query.png)

### <a name="group-by-query"></a>GROUP BY-Abfrage
Im nächsten Satz können wir nun leicht **GROUP BY**-Abfragen für unsere Cosmos DB-Datenbank durchführen:

```
select destination, sum(delay) as TotalDelays 
from originSEA 
group by destination 
order by sum(delay) desc limit 10
```

![Graph für Spark-GROUP BY-Abfrage](./media/documentdb-spark-connector/azure-documentdb-group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>DISTINCT, ORDER BY-Abfrage
Hier ist eine **DISTINCT, ORDER BY**-Abfrage angegeben:

![Graph für Spark-GROUP BY-Abfrage](./media/documentdb-spark-connector/azure-documentdb-order-by-query.png)

### <a name="continue-the-flight-data-analysis"></a>Fortsetzen der Flugdatenanalyse
Sie können die folgenden Beispielabfragen verwenden, um die Analyse der Flugdaten fortzusetzen:

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a>Top 5 der Ziele (Städte) mit Verspätungen bei Abflug von Seattle
```
select destination, sum(delay) 
from originSEA
where delay < 0 
group by destination 
order by sum(delay) limit 5
```
![Spark-Graph für Top 5 der Verzögerungen](./media/documentdb-spark-connector/azure-documentdb-top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>Berechnen der Medianwerte für Verzögerungen nach Zielstädten bei Abflug von Seattle
```
select destination, percentile_approx(delay, 0.5) as median_delay 
from originSEA
where delay < 0 
group by destination 
order by percentile_approx(delay, 0.5)
```

![Spark-Graph mit Medianwerten für Verzögerungen](./media/documentdb-spark-connector/azure-documentdb-median-delays-graph.png)

## <a name="next-steps"></a>Nächste Schritte

Laden Sie – falls noch nicht geschehen – den Spark-Connector für Cosmos DB aus dem GitHub-Repository [azure-documentdb-spark](https://github.com/Azure/azure-documentdb-spark) herunter, und sehen Sie sich die zusätzlichen Ressourcen im Repository an:

* [Beispiele für verteilte Aggregationen](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)
* [Beispielskripts und Notebooks](https://github.com/Azure/azure-documentdb-spark/tree/master/samples)

Es kann auch ratsam sein, die Informationen unter [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) (Leitfaden zu Apache Spark SQL, DataFrames und Datasets) und den Artikel [Apache Spark für Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) zu lesen.



