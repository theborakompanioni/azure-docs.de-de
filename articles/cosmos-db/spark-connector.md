---
title: "Herstellen einer Verbindung für Apache Spark mit Azure Cosmos DB | Microsoft-Dokumentation"
description: "In diesem Tutorial wird der Azure Cosmos DB-Spark-Connector beschrieben, mit dem Sie für Apache Spark eine Verbindung mit Azure Cosmos DB herstellen können, um verteilte Aggregationen und Data Science-Vorgänge für das global verteilte mehrinstanzenfähige Datenbanksystem von Microsoft für die Cloud durchzuführen."
keywords: Apache Spark
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 8ecbb478c81cde25bbd0d1c9ee07ae02b07f8cc7
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---

# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a>Beschleunigen der Big Data-Echtzeitanalyse mit dem Spark-Connector für Azure Cosmos DB

Mit dem Spark-Connector für Azure Cosmos DB kann Azure Cosmos DB als Eingabequelle oder Ausgabesenke für Apache Spark-Aufträge fungieren. Durch die Verbindung von [Spark](http://spark.apache.org/) mit [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) haben Sie die Möglichkeit, plötzlich auftretende Data Science-Probleme rasch zu lösen, indem Sie Azure Cosmos DB verwenden, um Daten schnell zu speichern und abzufragen. Der Spark-Connector für Azure Cosmos DB nutzt die nativen verwalteten Azure Cosmos DB-Indizes effizient. Die Indizes aktivieren aktualisierbare Spalten, wenn Sie Analysen durchführen und Pushdown-Prädikatfilter für sich schnell ändernde global verteilte Daten verwenden. Dies kann das Internet der Dinge (IoT), Data Science und Analyseszenarien umfassen.

Informationen zum Arbeiten mit Spark GraphX und den Gremlin-Graph-APIs von Azure Cosmos DB finden Sie unter [Durchführen von Graphanalysen mithilfe von Spark und Apache TinkerPop Gremlin](spark-connector-graph.md).

## <a name="download"></a>Herunterladen

Beginnen Sie, indem Sie den Spark-Connector für Azure Cosmos DB (Vorschauversion) aus dem GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) herunterladen.

## <a name="connector-components"></a>Connectorkomponenten

Der Connector verwendet die folgenden Komponenten:

* Mit [Azure Cosmos DB](http://documentdb.com) können Kunden sowohl den Durchsatz als auch den Speicher in beliebig vielen geografischen Regionen bereitstellen und flexibel skalieren. Der Dienst bietet Folgendes:
   * Eine sofort einsatzfähige [globale Verteilung](distribute-data-globally.md) und horizontale Skalierung
   * Garantiert einstellige Latenzzeiten im 99. Perzentil
   * [Mehrere, klar definierte Konsistenzmodelle](consistency-levels.md)
   * Garantierte hohe Verfügbarkeit mit Multi-Homing-Funktionen
   * Alle Funktionen werden durch branchenführende, umfassende [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db) (Service Level Agreements, SLAs) unterstützt.

* [Apache Spark](http://spark.apache.org/) ist ein leistungsstarkes Open-Source-Verarbeitungsmodul, das für hohe Geschwindigkeit, einfache Nutzung und anspruchsvolle Analysen konzipiert wurde.

* [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md), sodass Sie Apache Spark in der Cloud mithilfe von [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/) für unternehmenskritische Bereitstellungen bereitstellen können.

Offiziell unterstützte Versionen:

| Komponente | Version |
|---------|-------|
|Apache Spark|2.0+|
| Scala| 2.11|
| Azure DocumentDB Java SDK | 1.10.0 |

Dieser Artikel enthält Informationen zur Ausführung von einigen einfachen Beispielen mithilfe von Python (per pyDocumentDB) und den Scala-Oberflächen.

Es gibt zwei Ansätze zum Verbinden von Apache Spark mit Azure Cosmos DB:
- Verwenden von pyDocumentDB über das [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python)
- Erstellen eines Java-basierten Spark-Connectors für Azure Cosmos DB mit dem [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java)

## <a name="pydocumentdb-implementation"></a>pyDocumentDB-Implementierung
Mit dem aktuellen [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) können Sie Spark wie im folgenden Diagramm dargestellt mit Azure Cosmos DB verbinden:

![Datenfluss von Spark zu Azure Cosmos DB über pyDocumentDB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-the-pydocumentdb-implementation"></a>Datenfluss der pyDocumentDB-Implementierung

Der Datenfluss läuft wie folgt ab:

1. Der Spark-Masterknoten wird über pyDocumentDB mit dem Azure Cosmos DB-Gatewayknoten verbunden. Benutzer geben nur die Spark- und Azure Cosmos DB-Verbindungen an. Verbindungen mit den entsprechenden Master- und Gatewayknoten sind für den Benutzer transparent.
2. Der Gatewayknoten führt die Abfrage für Azure Cosmos DB durch, wo die Abfrage dann für die Partitionen der Sammlung auf den Datenknoten erfolgt. Die Antwort auf diese Abfragen wird zurück an den Gatewayknoten gesendet, und das Resultset wird an den Spark-Masterknoten zurückgegeben.
3. Nachfolgende Abfragen (z.B. für einen Spark DataFrame) werden zur Verarbeitung an die Spark-Workerknoten gesendet.

Die Kommunikation zwischen Spark und Azure Cosmos DB ist auf den Spark-Masterknoten und die Azure Cosmos DB-Gatewayknoten beschränkt.  Wie schnell die Abfragen durchgeführt werden, hängt von der Transportschicht zwischen diesen beiden Knoten ab.

### <a name="install-pydocumentdb"></a>Installieren von pyDocumentDB
Sie können pyDocumentDB auf Ihrem Treiberknoten installieren, indem Sie **pip** verwenden. Beispiel:

```
pip install pyDocumentDB
```


### <a name="connect-spark-to-azure-cosmos-db-via-pydocumentdb"></a>Verbinden von Spark mit Azure Cosmos DB über pyDocumentDB
Aufgrund der Einfachheit des Kommunikationstransports ist die Ausführung einer Abfrage von Spark zu Azure Cosmos DB per pyDocumentDB relativ einfach.

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


# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

Im Codeausschnitt ist Folgendes zu sehen:

* Das Azure Cosmos DB Python SDK (`pyDocumentDB`) enthält alle erforderlichen Verbindungsparameter. Durch den Parameter für den bevorzugten Standort wird beispielsweise die Lesereplikat- und Prioritätsreihenfolge bestimmt.
*  Importieren Sie die erforderlichen Bibliotheken, und konfigurieren Sie **masterKey** und **host**, um den Azure Cosmos DB-*Client* zu erstellen (**pydocumentdb.document_client**).


### <a name="execute-spark-queries-via-pydocumentdb"></a>Ausführen von Spark-Abfragen per pyDocumentDB
In den folgenden Beispielen wird die im vorherigen Codeausschnitt erstellte Azure Cosmos DB-Instanz verwendet, indem die angegebenen schreibgeschützten Schlüssel verwendet werden. Mit dem folgenden Codeausschnitt wird eine Verbindung mit der Sammlung **airports.codes** hergestellt (unter dem DoctorWho-Konto, wie zuvor angegeben) und eine Abfrage zum Extrahieren der Städte mit einem Flughafen im Bundesstaat Washington durchgeführt.

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
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

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-azure-cosmos-db"></a>Gründe für die Verwendung von pyDocumentDB zum Herstellen einer Spark-Verbindung mit Azure Cosmos DB
Eine Spark-Verbindung mit Azure Cosmos DB per pyDocumentDB wird normalerweise in den folgenden Szenarien hergestellt:

* Sie möchten Python verwenden.
* Sie geben ein relativ kleines Resultset von Azure Cosmos DB an Spark zurück. Beachten Sie, dass das zugrunde liegende Dataset in Azure Cosmos DB recht groß sein kann. Sie wenden Filter auf Ihre Azure Cosmos DB-Quelle an (führen also Prädikatfilter aus).  

## <a name="spark-to-azure-cosmos-db-connector"></a>Spark-Connector für Azure Cosmos DB

Der Spark-Connector für Azure Cosmos DB nutzt das [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) und verschiebt Daten zwischen den Spark-Workerknoten und Azure Cosmos DB. Dies ist im folgenden Diagramm dargestellt:

![Datenfluss im Spark-Connector für Azure Cosmos DB](./media/spark-connector/spark-connector.png)

Der Datenfluss läuft wie folgt ab:

1. Es wird eine Verbindung zwischen Spark-Masterknoten und Azure Cosmos DB-Gatewayknoten hergestellt, um die Partitionszuordnung zu erhalten. Benutzer geben nur die Spark- und Azure Cosmos DB-Verbindungen an. Verbindungen mit den entsprechenden Master- und Gatewayknoten sind für den Benutzer transparent.
2. Diese Informationen werden dann wieder für den Spark-Masterknoten bereitgestellt.  An diesem Punkt sollten Sie die Abfrage analysieren können, um zu ermitteln, auf welche Partitionen (und ihre Standorte) Sie in Azure Cosmos DB Zugriff haben müssen.
3. Diese Informationen werden an die Spark-Workerknoten übertragen.
4. Für die Spark-Workerknoten werden direkte Verbindungen mit den Azure Cosmos DB-Partitionen hergestellt, um die Daten zu extrahieren und sie dann wieder zurück auf die Spark-Partitionen der Spark-Workerknoten zu übertragen.

Die Kommunikation zwischen Spark und Azure Cosmos DB verläuft erheblich schneller, da die Datenverschiebung zwischen den Spark-Workerknoten und den Azure Cosmos DB-Datenknoten (Partitionen) erfolgt.

### <a name="build-the-spark-to-azure-cosmos-db-connector"></a>Erstellen des Spark-Connectors für Azure Cosmos DB
Derzeit wird für das Connectorprojekt Maven verwendet. Sie können Folgendes ausführen, um den Connector ohne Abhängigkeiten zu erstellen:
```
mvn clean package
```
Sie können auch die aktuellen JAR-Versionen im Ordner *releases* herunterladen.

### <a name="include-the-azure-cosmos-db-spark-jar"></a>Einbinden von Azure Cosmos DB Spark-JAR
Bevor Sie Code ausführen, müssen Sie Azure Cosmos DB Spark-JAR einbinden.  Bei Verwendung der **Spark-Shell** können Sie den JAR-Code mit der Option **--jars** einbinden.  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

Verwenden Sie den folgenden Code, wenn Sie den JAR-Code ohne Abhängigkeiten ausführen möchten:

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

Wenn Sie einen Notebook-Dienst wie den Azure HDInsight Jupyter Notebook-Dienst verwenden, können Sie die **spark magic**-Befehle nutzen:

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

Mit dem **jars**-Befehl können Sie die beiden JAR-Elemente einbinden, die für **azure-cosmosdb-spark** benötigt werden (zusätzlich für das Azure DocumentDB Java SDK). **scala-reflect** wird ausgeschlossen, damit kein Konflikt mit den Livy-Aufrufen entsteht (Jupyter Notebook > Livy > Spark).

### <a name="connect-spark-to-azure-cosmos-db-using-the-connector"></a>Herstellen einer Spark-Verbindung mit Azure Cosmos DB per Connector
Der Kommunikationstransport ist hierbei etwas komplizierter, aber das Ausführen einer Abfrage von Spark an Azure Cosmos DB ist mit dem Connector deutlich schneller.

Der folgende Codeausschnitt veranschaulicht, wie Sie den Connector in einem Spark-Kontext verwenden.

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Im Codeausschnitt ist Folgendes zu sehen:

- **azure-cosmosdb-spark** enthält alle erforderlichen Verbindungsparameter, die die bevorzugten Standorte enthalten. Sie können beispielsweise die Lesereplikat- und Prioritätsreihenfolge auswählen.
- Importieren Sie einfach die erforderlichen Bibliotheken, und konfigurieren Sie „masterKey“ und „host“, um den Azure Cosmos DB-Client zu erstellen.

### <a name="execute-spark-queries-via-the-connector"></a>Ausführen von Spark-Abfragen mit dem Connector

Im folgenden Beispiel wird die im vorherigen Codeausschnitt erstellte Azure Cosmos DB-Instanz verwendet, indem die angegebenen schreibgeschützten Schlüssel verwendet werden. Mit dem folgenden Codeausschnitt wird eine Verbindung mit der Sammlung „DepartureDelays.flights_pcoll“ hergestellt (unter dem DoctorWho-Konto, wie zuvor angegeben) und eine Abfrage zum Extrahieren der Informationen zu Flugverspätungen für Flüge durchgeführt, die in Seattle starten.

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-azure-cosmos-db-connector-implementation"></a>Gründe für die Implementierung des Spark-Connectors für Azure Cosmos DB

Eine Spark-Verbindung mit Azure Cosmos DB mithilfe des Connectors wird normalerweise in den folgenden Szenarien hergestellt:

* Sie möchten Scala verwenden und aktualisieren, um einen Python-Wrapper einzubinden, wie unter [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-cosmosdb-spark/issues/3) (Problem 3: Hinzufügen eines Python-Wrappers und von Beispielen) beschrieben.
* Sie müssen eine größere Datenmenge zwischen Apache Spark und Azure Cosmos DB übertragen.

Informationen zum Unterschied bei der Abfrageleistung finden Sie im [Query Test Runs wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs) (Wiki zu Abfragetestläufen).

## <a name="distributed-aggregation-example"></a>Beispiel für verteilte Aggregation
Dieser Abschnitt enthält einige Beispiele dafür, wie Sie verteilte Aggregationen und Analysen durchführen können, indem Sie Apache Spark und Azure Cosmos DB gemeinsam verwenden. Azure Cosmos DB verfügt bereits über Unterstützung für Aggregationen, wie im Blog [Planet scale aggregates with Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/) (Weltweite Aggregate mit Azure Cosmos DB) beschrieben. Hier wird erläutert, wie Sie mit Apache Spark noch mehr erreichen.

Beachten Sie, dass sich diese Aggregationen auf das [Notebook für den Spark-Connector für Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb) beziehen.

### <a name="connect-to-flights-sample-data"></a>Herstellen einer Verbindung mit Beispieldaten für Flüge
Für diese Aggregationsbeispiele wird auf einige Flugleistungsdaten zugegriffen, die in der **DoctorWho**-Azure Cosmos DB-Datenbank gespeichert sind. Verwenden Sie den folgenden Codeausschnitt, um die Verbindung herzustellen:

```
// Import Spark to Azure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect to Azure Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US 2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Mit diesem Codeausschnitt führen wir außerdem eine grundlegende Abfrage durch, mit der die gewünschten gefilterten Daten von Azure Cosmos DB an Spark übertragen werden. Hierbei können von Spark verteilte Aggregatvorgänge durchgeführt werden. In diesem Fall suchen wir nach Flügen, die in Seattle (SEA) starten.

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

Die folgenden Ergebnisse wurden generiert, indem die Abfragen über den Jupyter Notebook-Dienst durchgeführt wurden.  Beachten Sie, dass alle Codeausschnitte generisch und nicht dienstspezifisch sind.

### <a name="running-limit-and-count-queries"></a>Durchführen von LIMIT- und COUNT-Abfragen
Wie Sie es von SQL/Spark SQL gewohnt sind, beginnen wir mit einer **LIMIT**-Abfrage:

![Spark-LIMIT-Abfrage](./media/spark-connector/spark-sql-query.png)

Die nächste Abfrage ist eine einfache und schnelle **COUNT**-Abfrage:

![Spark-COUNT-Abfrage](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a>GROUP BY-Abfrage
Im nächsten Satz können wir nun leicht **GROUP BY**-Abfragen für unsere Azure Cosmos DB-Datenbank ausführen:

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Graph für Spark-GROUP BY-Abfrage](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>DISTINCT, ORDER BY-Abfrage
Hier ist eine **DISTINCT, ORDER BY**-Abfrage angegeben:

![Graph für Spark-GROUP BY-Abfrage](./media/spark-connector/order-by-query.png)

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
![Spark-Graph für Top 5 der Verzögerungen](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>Berechnen der Medianwerte für Verzögerungen nach Zielstädten bei Abflug von Seattle
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark-Graph mit Medianwerten für Verzögerungen](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a>Nächste Schritte

Laden Sie – falls noch nicht geschehen – den Spark-Connector für Azure Cosmos DB aus dem GitHub-Repository [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) herunter, und sehen Sie sich die zusätzlichen Ressourcen im Repository an:

* [Beispiele für verteilte Aggregationen](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Beispielskripts und Notebooks](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Es kann auch ratsam sein, die Informationen unter [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) (Leitfaden zu Apache Spark SQL, DataFrames und Datasets) und den Artikel [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) zu lesen.

