---
title: "Azure Cosmos DB: Durchführen von Graphanalysen mithilfe von Spark und Apache TinkerPop Gremlin | Microsoft-Dokumentation"
description: "Bietet Anweisungen zum Einrichten und Ausführen von Graphanalysen und Parallelberechnung mit Azure Cosmos DB und Spark GraphX"
services: cosmos-db
documentationcenter: 
author: khdang
manager: shireest
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: gremlin
ms.topic: hero-article
ms.date: 05/21/2017
ms.author: khdang
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 7a95953fadb3089f60fd55973fdb3410012655af
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="azure-cosmos-db-perform-graph-analytics-using-spark-and-apache-tinkerpop-gremlin"></a>Azure Cosmos DB: Durchführen von Graphanalysen mithilfe von Spark und Apache TinkerPop Gremlin

[Azure Cosmos DB](introduction.md) ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Sie können Dokument-, Schlüssel/Wert- und Graphdatenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB zugrunde liegen. Azure Cosmos DB unterstützt OLTP-Graphworkloads in [Gremlin von Apache TinkerPop](graph-introduction.md).

[Spark](http://spark.apache.org/) ein Projekt der Apache Software Foundation mit Schwerpunkt auf allgemeiner OLAP-Datenverarbeitung. Spark bietet ein verteiltes In-Memory-/datenträgerbasiertes Hybrid-Computingmodell, das dem MapReduce-Modell von Hadoop ähnlich ist. Sie können Apache Spark in der Cloud mit [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/) bereitstellen. Spark umfasst die [GraphX-Bibliothek](http://spark.apache.org/graphx/) für Graphen und Graph-Parallelenberechnung, die auch Apache TinkerPop Gremlin verwendet.

Mit einer Kombination aus Azure Cosmos DB und Spark können Sie sowohl OLTP- als auch OLAP-Workloads in Gremlin ausführen. Dieser Schnellstart veranschaulicht, wie Gremlin-Abfragen für Azure Cosmos DB in einem Azure HDInsight Spark-Cluster ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

* Bevor Sie dieses Beispiel ausführen können, müssen folgende Voraussetzungen erfüllt sein:
   * Azure HDInsight Spark-Cluster 2.0
   * JDK 1.8 und höher (Führen Sie `apt-get install default-jdk` aus, wenn Sie nicht über JDK verfügen)
   * Maven (Führen Sie `apt-get install maven` aus, wenn Sie nicht über Maven verfügen)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Weitere Informationen zum Bereitstellen eines Azure HDInsight Spark-Clusters finden Sie unter [Bereitstellen von HDInsight-Clustern](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-an-azure-cosmos-db-database-account"></a>Erstellen eines Azure Cosmos DB-Datenbankkontos

Zuerst erstellen wir mit der Graph-API ein Datenbankkonto.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Hinzufügen einer Sammlung

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a>Installieren von TinkerPop

* Stellen Sie eine Remoteverbindung mit dem Masterknoten des HDInsight-Clusters her: `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`

* Klonen Sie TinkerPop3-Quellcode, erstellen Sie ihn lokal, und installieren Sie ihn im Maven-Cache.

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

* Installieren Sie das Spark-Gremlin-Plug-In. 

    1. Die Installation des Plug-Ins erfolgt durch Grape. Wir müssen zunächst die Repository-Informationen für Grape ausfüllen, damit die Umgebung das Plug-In und seine Abhängigkeiten herunterladen kann. 

        * Wenn die Grape-Konfigurationsdatei noch nicht vorhanden ist, erstellen Sie sie unter `~/.groovy/grapeConfig.xml`. Verwenden Sie folgende Einstellungen:

            ```xml
            <ivysettings>
            <settings defaultResolver="downloadGrapes"/>
            <resolvers>
                <chain name="downloadGrapes">
                <filesystem name="cachedGrapes">
                    <ivy pattern="${user.home}/.groovy/grapes/[organisation]/[module]/ivy-[revision].xml"/>
                    <artifact pattern="${user.home}/.groovy/grapes/[organisation]/[module]/[type]s/[artifact]-[revision].[ext]"/>
                </filesystem>
                <ibiblio name="codehaus" root="http://repository.codehaus.org/" m2compatible="true"/>
                <ibiblio name="central" root="http://central.maven.org/maven2/" m2compatible="true"/>
                <ibiblio name="jitpack" root="https://jitpack.io" m2compatible="true"/>
                <ibiblio name="java.net2" root="http://download.java.net/maven/2/" m2compatible="true"/>
                <ibiblio name="apache-snapshots" root="http://repository.apache.org/snapshots/" m2compatible="true"/>
                <ibiblio name="local" root="file:${user.home}/.m2/repository/" m2compatible="true"/>
                </chain>
            </resolvers>
            </ivysettings>
            ``` 

    2. Starten Sie die Gremlin-Konsole `bin/gremlin.sh`
        
    3. Installieren Sie das Spark-Gremlin-Plug-In mit Version 3.3.0-SNAPSHOT, das wir in den vorhergehenden Schritten erstellt haben.

        ```bash
        $ bin/gremlin.sh

                \,,,/
                (o o)
        -----oOOo-(3)-oOOo-----
        plugin activated: tinkerpop.server
        plugin activated: tinkerpop.utilities
        plugin activated: tinkerpop.tinkergraph
        gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
        ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart the console to use [tinkerpop.spark]
        gremlin> :q
        $ bin/gremlin.sh

                \,,,/
                (o o)
        -----oOOo-(3)-oOOo-----
        plugin activated: tinkerpop.server
        plugin activated: tinkerpop.utilities
        plugin activated: tinkerpop.tinkergraph
        gremlin> :plugin use tinkerpop.spark
        ==>tinkerpop.spark activated
        ```

    4. Überprüfen Sie, ob `Hadoop-Gremlin` mit `:plugin list` ist. Dieses Plug-In muss deaktiviert werden, da es das Spark-Gremlin-Plug-In `:plugin unuse tinkerpop.hadoop` beeinträchtigen könnte.

## <a name="prepare-tinkerpop3-dependencies"></a>Vorbereiten von TinkerPop3-Abhängigkeiten

Bei der Erstellung von TinkerPop3 im vorherigen Schritt wurden auch alle JAR-Abhängigkeiten für Spark und Hadoop in das Zielverzeichnis übertragen. Wir verwenden die JARs mit vorinstalliertem HDI, und rufen zusätzliche Abhängigkeiten nur bei Bedarf ab.

    1. Wechseln Sie zum Zielverzeichnis der Gremlin-Konsole unter `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`. 

    2. Verschieben Sie dort alle JARs unter `ext/` nach `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`

    3. Entfernen Sie alle JAR-Bibliotheken unter `lib/`, die sich nicht auf der folgenden Liste befinden.

        ```bash
        # TinkerPop3
        gremlin-core-3.3.0-SNAPSHOT.jar       
        gremlin-groovy-3.3.0-SNAPSHOT.jar     
        gremlin-shaded-3.3.0-SNAPSHOT.jar     
        hadoop-gremlin-3.3.0-SNAPSHOT.jar     
        spark-gremlin-3.3.0-SNAPSHOT.jar      
        tinkergraph-gremlin-3.3.0-SNAPSHOT.jar

        # Gremlin depedencies
        asm-3.2.jar                                
        avro-1.7.4.jar                             
        caffeine-2.3.1.jar                         
        cglib-2.2.1-v20090111.jar                  
        gbench-0.4.3-groovy-2.4.jar                
        gprof-0.3.1-groovy-2.4.jar                 
        groovy-2.4.9-indy.jar                      
        groovy-2.4.9.jar                           
        groovy-console-2.4.9.jar                   
        groovy-groovysh-2.4.9-indy.jar             
        groovy-json-2.4.9-indy.jar                 
        groovy-jsr223-2.4.9-indy.jar               
        groovy-sql-2.4.9-indy.jar                  
        groovy-swing-2.4.9.jar                     
        groovy-templates-2.4.9.jar                 
        groovy-xml-2.4.9.jar                       
        hadoop-yarn-server-nodemanager-2.7.2.jar   
        hppc-0.7.1.jar                             
        javatuples-1.2.jar                         
        jaxb-impl-2.2.3-1.jar                      
        jbcrypt-0.4.jar                            
        jcabi-log-0.14.jar                         
        jcabi-manifests-1.1.jar                    
        jersey-core-1.9.jar                        
        jersey-guice-1.9.jar                       
        jersey-json-1.9.jar                        
        jettison-1.1.jar                           
        scalatest_2.11-2.2.6.jar                   
        servlet-api-2.5.jar                        
        snakeyaml-1.15.jar                         
        unused-1.0.0.jar                           
        xml-apis-1.3.04.jar                        
        ```

## <a name="get-the-cosmos-db-spark-connector"></a>Abrufen des Cosmos DB Spark-Connectors

1. Rufen Sie den Cosmos DB Spark-Connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` und das Cosmos DB-Java-SDK `azure-documentdb-1.10.0.jar` von [Azure Cosmos DB Spark Connector auf Github](https://github.com/Azure/azure-documentdb-spark/tree/master/releases/azure-documentdb-spark-0.0.3_2.0.2_2.11) ab.

2. Alternativ können Sie es lokal erstellen. Da die aktuelle Version von Spark-Gremlin mit Spark 1.6.1 erstellt wurde und nicht mit Spark 2.0.2 kompatibel ist, das derzeit im Cosmos DB Spark-Connector verwendet wird, können Sie den aktuellen TinkerPop3-Code erstellen und die JARs manuell installieren.

    1. Klonen des Cosmos DB Spark-Connectors

    2. Erstellen Sie TinkerPop3 (in den vorherigen Schritten bereits geschehen). Installieren Sie alle TinkerPop 3.3.0-SNAPSHOT-JARs lokal.
    
    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    2. Aktualisieren Sie `tinkerpop.version` `azure-documentdb-spark/pom.xml` auf `3.3.0-SNAPSHOT`.
    
    3. Erstellen Sie mit Maven. Die erforderlichen JARs befinden sich in `target` und `target/alternateLocation`.

    ```bash
    git clone https://github.com/Azure/azure-documentdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. Kopieren Sie die oben erwähnten JARs in ein lokales Verzeichnis unter „~/azure-documentdb-spark“.

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-the-dependencies-to-the-spark-worker-nodes"></a>Verteilen der Abhängigkeiten an die Spark-Workerknoten 

1. Da die Transformation von Graphdaten von TinkerPop3 abhängt, müssen wir die zugehörigen Abhängigkeiten an alle Spark-Workerknoten verteilen.

2. Kopieren Sie die zuvor aufgeführten Gremlin-Abhängigkeiten, die JAR-Datei des CosmosDB Spark-Connectors und das CosmosDB-Java-SDK wie folgt auf die Workerknoten.

    1. Kopieren Sie alle JARs in `~/azure-documentdb-spark`.

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    2. Rufen Sie die Liste aller Spark-Workerknoten ab. Diese finden Sie in Ambari Dashboard in der Liste `Spark2 Clients` in Abschnitt `Spark2`.

    3. Kopieren Sie dieses Verzeichnis auf jeden Knoten

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-the-environment-variables"></a>Festlegen der Umgebungsvariablen

1. Suchen Sie die HDP-Version des Spark-Clusters; dies ist der Verzeichnisname unter `/usr/hdp/`, z.B. „2.5.4.2-7“.

2. Legen Sie die HDP-Version für alle Knoten fest: Wechseln Sie in Ambari Dashboard zu `YARN section -> Configs -> Advanced`. Fügen Sie in `Custom yarn-site` die neue Eigenschaft `hdp.version` mit dem Wert der HDP-Version auf dem Masterknoten hinzu. Speichern Sie die Konfigurationen – dabei werden Warnungen ausgegeben, die jedoch ignoriert werden können. Starten Sie anschließend wie von den Benachrichtigungssysmbolen angegeben die YARN- und Oozie-Dienste neu.

3. Legen Sie auf dem Masterknoten die folgenden Umgebungsvariablen fest (ersetzen Sie die Werte nach Bedarf):

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-the-graph-configuration"></a>Vorbereiten der Graphkonfiguration

1. Erstellen Sie eine Konfigurationsdatei mit den Cosmos DB-Verbindungsparametern und Spark-Einstellungen, und legen Sie sie unter `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties` ab.

    ```
    gremlin.graph=org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph
    gremlin.hadoop.jarsInDistributedCache=true
    gremlin.hadoop.defaultGraphComputer=org.apache.tinkerpop.gremlin.spark.process.computer.SparkGraphComputer

    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    gremlin.hadoop.graphWriter=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBOutputRDD

    ####################################
    # SparkGraphComputer Configuration #
    ####################################
    spark.master=yarn
    spark.executor.memory=3g
    spark.executor.instances=6
    spark.serializer=org.apache.spark.serializer.KryoSerializer
    spark.kryo.registrator=org.apache.tinkerpop.gremlin.spark.structure.io.gryo.GryoRegistrator
    gremlin.spark.persistContext=true

    # Classpath for the driver and executors
    spark.driver.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    spark.executor.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    
    ######################################
    # DocumentDB Spark Connector         #
    ######################################
    spark.documentdb.connectionMode=Gateway
    spark.documentdb.schema_samplingratio=1.0
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    spark.documentdb.preferredRegions=FILLIN
    ```

2. Aktualisieren Sie `spark.driver.extraClassPath` und `spark.executor.extraClassPath`, sodass das Verzeichnis der JARs enthalten ist, die wir im vorhergehenden Schritt verteilt haben, in diesem Fall `/home/sshuser/azure-documentdb-spark/*`.

3. Geben Sie die folgenden Details für Cosmos DB ein:

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-tinkerpops-graph-and-save-to-cosmos-db"></a>Laden des Graphs von TinkerPop und Speichern in Cosmos DB
Zur Veranschaulichung, wie ein Graph dauerhaft in Cosmos DB gespeichert werden kann, verwenden das vordefinierte moderne TinkerPop-Graph als Beispiel. Der Graph wurde im Kryo-Format gespeichert und im TinkerPop-Repository bereitgestellt.

1. Da wir Gremlin im YARN-Modus ausführen, müssen wir die Graphdaten im Hadoop-Dateisystem verfügbar machen. Mit den folgenden Befehlen wird ein Verzeichnis erstellt, in das die lokalen Graphdateien kopiert werden. 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. Aktualisieren Sie die Datei `gremlin-spark.properties` vorübergehend so, dass sie `GryoInputFormat` zum Lesen des Graphs verwendet. Wir geben auch `inputLocation` als Verzeichnis an, das wir wie folgt erstellt haben:

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

2. Starten Sie die Gremlin-Konsole, und erstellen Sie diese Berechnungsschritte, um Daten dauerhaft in der konfigurierten Cosmos DB-Sammlung zu speichern:  

    1. Erstellen Sie den Graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`

    2. Verwenden Sie SparkGraphComputer zum Schreiben: `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[gryoinputformat->documentdboutputrdd]
    gremlin> hg = graph.
                compute(SparkGraphComputer.class).
                result(GraphComputer.ResultGraph.NEW).
                persist(GraphComputer.Persist.EDGES).
                program(TraversalVertexProgram.build().
                    traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)), "gremlin-groovy", "g.V()").
                    create(graph)).
                submit().
                get() 
    ==>result[hadoopgraph[documentdbinputrdd->documentdboutputrdd],memory[size:1]]
    ```

3. Im Daten-Explorer können wir überprüfen, ob die Daten in Cosmos DB aufgenommen wurden.

## <a name="load-the-graph-from-cosmos-db-and-run-gremlin-queries"></a>Laden des Graphs aus Cosmos DB und Ausführen von Gremlin-Abfragen

1. Bearbeiten Sie zum Laden des Graphs `gremlin-spark.properties` so, dass `graphReader` auf `DocumentDBInputRDD` festgelegt ist:

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. Führen Sie die folgenden Schritte aus, um den Graph zu laden, die Daten zu übertragen und Gremlin-Abfragen damit auszuführen:

    1. Starten Sie die Gremlin-Konsole `bin/gremlin.sh`

    2. Erstellen Sie den Graph mit folgenden Konfigurationen: `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`

    3. Erstellen Sie einen Graphdurchlauf mit SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`

    4. Führen Sie Gremlin-Graphabfragen aus:

        ```bash
        gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
        ==>hadoopgraph[documentdbinputrdd->documentdboutputrdd]
        gremlin> g = graph.traversal().withComputer(SparkGraphComputer)
        ==>graphtraversalsource[hadoopgraph[documentdbinputrdd->documentdboutputrdd], sparkgraphcomputer]
        gremlin> g.V().count()
        ==>6
        gremlin> g.E().count()
        ==>6
        gremlin> g.V(1).out().values('name')
        ==>josh
        ==>vadas
        ==>lop
        gremlin> g.V().hasLabel('person').coalesce(values('nickname'), values('name'))
        ==>josh
        ==>peter
        ==>vadas
        ==>marko
        gremlin> g.V().hasLabel('person').
               choose(values('name')).
                 option('marko', values('age')).
                 option('josh', values('name')).
                 option('vadas', valueMap()).
                 option('peter', label())
        ==>josh
        ==>person
        ==>[name:[vadas],age:[27]]
        ==>29
        ```

> [!NOTE]
> Zum Anzuzeigen einer ausführlicheren Protokollierung legen Sie in `conf/log4j-console.properties` eine ausführlichere Protokollebene fest.
>

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Graphen mithilfe einer Kombination aus Azure Cosmos DB und Spark verwendet werden.

> [!div class="nextstepaction"]

