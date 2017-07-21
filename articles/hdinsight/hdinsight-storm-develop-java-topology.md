---
title: "Apache Storm-Beispiel, Java-Topologie – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Apache Storm-Topologien in Java erstellen, indem Sie eine Beispieltopologie zur Wortzählung erstellen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: Apache Storm, Apache Storm-Beispiel, Storm-Java, Storm-Topologiebeispiel
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 36285fbaf1da3c566d338bd5612eebad327eaf50
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="create-an-apache-storm-topology-in-java"></a>Erstellen einer Apache Storm-Topologie in Java

In diesem Artikel erfahren Sie, wie Sie eine Java-basierte Topologie für Apache Storm erstellen. Sie erstellen eine Storm-Topologie, die eine Anwendung zur Wortzählung implementiert. Sie verwenden Maven zum Erstellen und Verpacken des Projekts. Anschließend erfahren Sie, wie Sie die Topologie mit dem Flux-Framework definieren.

> [!NOTE]
> Das Flux-Framework ist unter Storm 0.10.0 oder höher verfügbar. Storm 0.10.0 ist unter HDInsight 3.3 und 3.4 verfügbar.

Nach Abschluss der Schritte in diesem Dokument können Sie die Topologie für Apache Storm in HDInsight bereitstellen.

> [!NOTE]
> Eine vollständige Version der Storm-Topologiebeispiele, die in diesem Dokument erstellt werden, finden Sie unter [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Voraussetzungen

* [Java Developer Kit (JDK), Version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* [Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven ist ein Projektbuildsystem für Java-Projekte.

* Ein Text-Editor oder eine IDE.

## <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Bei der Installation von Java und dem JDK können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* **JAVA_HOME** – sollte auf das Verzeichnis verweisen, in dem die Java-Laufzeitumgebung (Java Runtime Environment, JRE) installiert ist. Für eine Unix- oder Linux-Distribution sollte z. B. ein Wert wie `/usr/lib/jvm/java-7-oracle` verwendet werden. Unter Windows sollte der Wert so ähnlich sein wie `c:\Program Files (x86)\Java\jre1.7`

* **PATH** – sollte die folgenden Pfade enthalten:

  * **JAVA_HOME** (oder den entsprechenden Pfad)

  * **JAVA_HOME\bin** (oder den entsprechenden Pfad)

  * Das Verzeichnis, in dem Maven installiert ist.

## <a name="create-a-maven-project"></a>Erstellen eines Maven-Projekts

Geben Sie folgenden Befehl in die Befehlszeile ein, um ein Maven-Projekt namens **WordCount**zu erstellen:

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> Wenn Sie PowerShell verwenden, müssen Sie die `-D`-Parameter in doppelte Anführungszeichen setzen.
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

Dieser Befehl erstellt am aktuellen Speicherort ein neues Verzeichnis namens `WordCount`, das ein einfaches Maven-Projekt enthält. Das `WordCount`-Verzeichnis enthält die folgenden Elemente:

* `pom.xml`: Enthält Einstellungen für das Maven-Projekt.
* `src\main\java\com\microsoft\example`: Enthält Ihren Anwendungscode.
* `src\test\java\com\microsoft\example`: Enthält Tests für Ihre Anwendung. 

### <a name="remove-the-generated-example-code"></a>Entfernen des generierten Beispielcodes

Löschen Sie den generierten Test und die Anwendungsdateien:

* **src\test\java\com\microsoft\example\AppTest.java**
* **src\main\java\com\microsoft\example\App.java**

## <a name="add-maven-repositories"></a>Hinzufügen von Maven-Repositorys

HDInsight basiert auf der Hortonworks Data Platform (HDP), daher empfehlen wir, das Hortonworks-Repository zum Herunterladen von Abhängigkeiten für Ihre Apache Storm-Projekte zu verwenden. Fügen Sie in der Datei __pom.xml__ nach der Zeile `<url>http://maven.apache.org</url>` folgenden XML-Code hinzu:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Eigenschaften hinzufügen

Maven ermöglicht Ihnen das Definieren von Werten auf Projektebene, die als Eigenschaften bezeichnet werden. Fügen Sie in der Datei __pom.xml__ nach der Zeile `</repositories>` den folgenden Text hinzu:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

Sie können diesen Wert nun in anderen Abschnitten von `pom.xml` verwenden. Beim Angeben der Version von Storm-Komponenten können Sie z.B. `${storm.version}` verwenden, anstatt einen Wert hartzucodieren.

## <a name="add-dependencies"></a>Hinzufügen von Abhängigkeiten

Fügen Sie eine Abhängigkeit für Storm-Komponenten hinzu. Öffnen Sie die Datei `pom.xml`, und fügen Sie den folgenden Code im Abschnitt `<dependencies>` hinzu:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Zum Zeitpunkt der Kompilierung verwendet Maven diese Informationen für die Suche nach `storm-core` im Maven-Repository. Zuerst wird im Repository auf dem lokalen Computer gesucht. Wenn die Dateien nicht vorhanden sind, lädt Maven sie aus dem öffentlichen Maven-Repository herunter und speichert sie im lokalen Repository.

> [!NOTE]
> Beachten Sie die Zeile `<scope>provided</scope>` in diesem Abschnitt. Diese Einstellung weist Maven an, **storm-core** aus allen erstellten JAR-Dateien auszuschließen, da es vom System bereitgestellt wird.

## <a name="build-configuration"></a>Buildkonfiguration

Mithilfe von Maven-Plug-Ins können Sie die Buildphasen des Projekts anpassen. Beispielsweise, wie das Projekt kompiliert oder wie es in eine JAR-Datei verpackt wird. Öffnen Sie die Datei `pom.xml`, und fügen Sie den folgenden Code direkt oberhalb der Zeile `</project>` ein.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Dieser Abschnitt wird zum Hinzufügen von Plug-Ins, Ressourcen und anderen Optionen für die Buildkonfiguration verwendet. Eine vollständige Referenz zur Datei **pom.xml** finden Sie unter [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Hinzufügen von Plug-Ins

Für in Java implementierte Apache Storm-Topologien ist das [Exec Maven-Plug-In](http://www.mojohaus.org/exec-maven-plugin/) hilfreich, da es Ihnen ermöglicht, die Topologie einfach lokal in Ihrer Entwicklungsumgebung auszuführen. Fügen Sie den folgenden Code im Abschnitt `<plugins>` der Datei `pom.xml` hinzu, um das Exec Maven-Plug-In einzubeziehen:

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

Ein weiteres nützliches Plug-In ist das [Apache Maven Compiler-Plug-In](http://maven.apache.org/plugins/maven-compiler-plugin/), das zum Ändern von Kompilierungsoptionen verwendet wird. Die Änderungen der Java-Version, die Maven für die Quelle und das Ziel Ihrer Anwendung verwendet.

* Für HDInsight __3.4 oder früher__ legen Sie Java-Version __1.7__ für Quelle und Ziel fest.

* Für HDInsight __3.5__ legen Sie Java-Version __1.8__ für Quelle und Ziel fest.

Fügen Sie folgenden Text im Abschnitt `<plugins>` der Datei `pom.xml` hinzu, um das Apache Maven Compiler-Plug-In einzubeziehen. Dieses Beispiel legt 1.8 fest, damit die Ziel-HDInsight-Version 3.5 ist.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a>Konfigurieren der Ressourcen

Im Abschnitt „resources“ können Sie nicht codebezogene Ressourcen, z.B. Konfigurationsdateien, hinzufügen, die von Komponenten in der Topologie benötigt werden. Fügen Sie für dieses Beispiel der Datei „pom.xml“ im Abschnitt `<resources>` den folgenden Text hinzu.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

Dieses Beispiel fügt dem Verzeichnis „resources“ im Stammverzeichnis des Projekts (`${basedir}`) als Speicherort hinzu, der Ressourcen und die Datei mit dem Namen `log4j2.xml` enthält. Diese Datei dient zum Konfigurieren, welche Informationen von der Topologie protokolliert werden.

## <a name="create-the-topology"></a>Erstellen der Topologie

Eine Java-basierte Apache Storm-Topologie besteht aus drei Komponenten, die Sie als Abhängigkeit erstellen (oder referenzieren) müssen.

* **Spouts:**liest Daten aus externen Quellen und gibt Datenströme in die Topologie aus.

* **Bolts.**verarbeitet Datenströme, die von Spouts oder anderen Bolts ausgegeben werden, und gibt einen oder mehrere Datenströme aus.

* **Topology:**definiert die Anordnung der Spouts und Bolts und stellt den Einstiegspunkt für die Topologie bereit.

### <a name="create-the-spout"></a>Erstellen des Spouts

Um die Anforderungen für das Einrichten von externen Datenquellen zu verringern, gibt der folgende Spout willkürliche Sätze aus. Es handelt sich um eine modifizierte Version eines Spouts, der mit den [Storm-Startbeispielen](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter)bereitgestellt wird.

> [!NOTE]
> Ein Beispiel zu einem Spout, der aus einer externen Datenquelle liest, finden Sie in den folgenden Beispielen:
>
> * [TwitterSampleSpout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): ein Beispiel-Spout, der Daten aus Twitter liest
> * [Storm-Kafka:](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka)ein Spout, der Dateien aus Kafka liest

Erstellen Sie für den Spout im Verzeichnis `src\main\java\com\microsoft\example` eine Datei namens `RandomSentenceSpout.java`, und verwenden Sie folgenden Java-Code als Inhalt:

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used to emit output
  SpoutOutputCollector _collector;
  //Used to generate a random number
  Random _rand;

  //Open is called when an instance of the class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set the instance collector to the one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data to the stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //The sentences that are randomly emitted
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit the sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare the output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> Obwohl in dieser Topologie nur ein Spout verwendet wird, verfügen andere Topologien möglicherweise über mehrere Spouts, die der Topologie Daten aus verschiedenen Quellen zuführen.

### <a name="create-the-bolts"></a>Erstellen des Bolts

Bolts übernehmen die Datenverarbeitung. Diese Topologie verwendet zwei Bolts:

* **SplitSentence**: unterteilt die von **RandomSentenceSpout** ausgegebenen Sätze in einzelne Wörter.

* **WordCount**: zählt das Vorkommen der einzelnen Wörter

> [!NOTE]
> Bolts können alle Aufgaben übernehmen, beispielsweise Berechnungen, Persistenz oder Kommunikation mit externen Komponenten.

Erstellen Sie die beiden neuen Dateien `SplitSentence.java` und `WordCount.java` im Verzeichnis `src\main\java\com\microsoft\example`. Fügen Sie den folgenden Text in die Dateien ein:

#### <a name="splitsentence"></a>SplitSentence

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get the sentence content from the tuple
    String sentence = tuple.getString(0);
    //An iterator to get each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give the iterator the sentence
    boundary.setText(sentence);
    //Find the beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it to the output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get the word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>WordCount

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often to emit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default to 60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used to trigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get the word contents from the tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment the count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-the-topology"></a>Definieren der Topologie

Die Topologie verbindet die Spouts und Bolts in einem Diagramm, in dem definiert ist, wie die Daten zwischen den Komponenten verlaufen. Darüber hinaus bietet sie Hinweise zu Parallelität, die von Storm beim Erstellen von Instanzen der Komponenten innerhalb des Clusters verwendet werden.

Nachfolgend sehen Sie eine einfache Abbildung des Komponentendiagramms für diese Topologie.

![Diagramm mit der Anordnung von Spouts und Bolts](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Um die Topologie zu implementieren, erstellen Sie eine Datei namens `WordCountTopology.java` im Verzeichnis `src\main\java\com\microsoft\example`. Verwenden Sie als Inhalt der Datei den folgenden Java-Code:

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for the topology
  public static void main(String[] args) throws Exception {
  //Used to build the topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add the spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add the SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes to the spout, and equally distributes
    //tuples (sentences) across instances of the SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add the counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes to the split bolt, and
    //ensures that the same word is sent to the same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set to false to disable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint to set the number of workers
      conf.setNumWorkers(3);
      //submit the topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap the maximum number of executors that can be spawned
      //for a component to 3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used to run locally
      LocalCluster cluster = new LocalCluster();
      //submit the topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down the cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>Konfigurieren der Protokollierung

Storm verwendet Apache Log4j für die Protokollierung von Informationen. Wenn Sie die Protokollierung nicht konfigurieren, gibt die Topologie Diagnoseinformationen aus. Um zu steuern, was protokolliert wird, erstellen Sie eine Datei namens `log4j2.xml` im Verzeichnis `resources`. Fügen Sie folgenden XML-Code als Inhalt der Datei hinzu.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

Hiermit wird eine neue Protokollierung für die `com.microsoft.example`-Klasse konfiguriert, die die Komponenten in dieser Beispieltopologie enthält. Die Ebene für diese Protokollierung wird auf „trace“ festgelegt. Dadurch werden alle Protokollinformationen erfasst, die von Komponenten in dieser Topologie ausgegeben werden.

Der Abschnitt `<Root level="error">` konfiguriert die Stammebene der Protokollierung (alles, was nicht in `com.microsoft.example` enthalten ist) so, dass nur Fehlerinformationen protokolliert werden.

Weitere Informationen zum Konfigurieren der Protokollierung für Log4j finden Sie unter [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]
> Storm-Version 0.10.0 und höher verwenden Log4j 2.x. Ältere Versionen von Storm verwenden Log4j 1.x mit einem anderen Format für die Protokollkonfiguration. Informationen zur älteren Konfiguration finden Sie unter [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-the-topology-locally"></a>Lokales Testen der Topologie

Nach dem Speichern der Dateien verwenden Sie folgenden Befehl, um die Topologie lokal zu testen.

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Während der Ausführung zeigt die Topologie zunächst Startinformationen an. Der folgende Text ist ein Beispiel der Wortzählungsausgabe:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Dieses Beispielprotokoll gibt an, dass das Wort „und“ 113 Mal ausgegeben wurde. Diese Anzahl steigt weiter an, solange die Topologie ausgeführt wird, da der Spout fortlaufend die gleichen Sätze ausgibt.

Zwischen der Ausgabe von Wörtern und der Anzahl liegt außerdem ein Intervall von fünf Sekunden. Die **WordCount**-Komponente wird so konfiguriert, dass nur Informationen ausgegeben werden, wenn ein „Tick“-Tupel eingeht. Es wird angefordert, dass „Tick“-Tupel nur alle fünf Sekunden zugestellt werden.

## <a name="convert-the-topology-to-flux"></a>Konvertieren der Topologie in Flux

Flux ist ein neues Framework von Storm 0.10.0 und höher, mit dem Sie die Konfiguration von der Implementierung trennen können. Ihre Komponenten werden weiterhin in Java definiert, aber die Topologie wird mit einer YAML-Datei definiert. Sie können eine Standardtopologiedefinition mit Ihrem Projekt verpacken oder eine eigenständige Datei verwenden, wenn Sie die Topologie übermitteln. Wenn Sie die Topologie an Storm übermitteln, können Sie Umgebungsvariablen oder Konfigurationsdateien verwenden, um die YAML-Topologiedefinition mit Werten aufzufüllen.

Die YAML-Datei definiert die für die Topologie zu verwendenden Komponenten und den Datenfluss zwischen ihnen. Sie können eine YAML-Datei als Teil der JAR-Datei einfügen, oder Sie können eine externe YAML-Datei verwenden.

Weitere Informationen zu Flux finden Sie unter [Flux-Framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

> [!WARNING]
> Aufgrund eines [Fehlers (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) in Storm 1.0.1 müssen Sie möglicherweise eine [Storm-Entwicklungsumgebung](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) installieren, um Flux-Topologien lokal auszuführen.

1. Verschieben Sie die Datei `WordCountTopology.java` aus dem Projekt. Früher definierte diese Datei die Topologie, wird aber mit Flux nicht benötigt.

2. Erstellen Sie im Verzeichnis `resources` eine Datei namens `topology.yaml`. Verwenden Sie als Inhalt der Datei den folgenden Text.

        name: "wordcount"       # friendly name for the topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for the number of workers to create
        
        spouts:                 # Spout definitions
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            parallelism: 1      # parallelism hint
        
        bolts:                  # Bolt definitions
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1
         
        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
                - 10
            parallelism: 1
        
        streams:                # Stream definitions
            - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            from: "sentence-spout"       # The stream emitter
            to: "splitter-bolt"          # The stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) to group on

3. Nehmen Sie die folgenden Änderungen in der Datei `pom.xml` vor.
   
   * Fügen Sie im Abschnitt `<dependencies>` die folgende neue Abhängigkeit hinzu:
     
        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * Fügen Sie im Abschnitt `<plugins>` das folgende Plug-In hinzu. Dieses Plug-In ist für die Erstellung eines Pakets (JAR-Datei) für das Projekt zuständig und wendet beim Erstellen des Pakets einige spezifische Transformationen für Flux an.
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer to it as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   * Ändern Sie im Abschnitt **exec-maven-plugin** `<configuration>` den Wert für `<mainClass>` in `org.apache.storm.flux.Flux`. Diese Einstellung ermöglicht Flux, die Ausführung der Topologie lokal in der Entwicklung zu steuern.

   * Fügen Sie `<includes>` im Abschnitt `<resources>` Folgendes hinzu: Dieser XML-Code enthält die YAML-Datei, mit der die Topologie als Teil des Projekts definiert wird.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a>Lokales Testen der Flux-Topologie

1. Verwenden Sie Folgendes, um die Flux-Topologie zu kompilieren und per Maven auszuführen:

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    Verwenden Sie den folgenden Befehl, wenn Sie PowerShell nutzen:

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > Wenn in der Topologie Storm 1.0.1-Bits verwendet werden, schlägt dieser Befehl fehl. Dieser Fehler wird durch [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055) verursacht. [Installieren Sie stattdessen Storm in Ihrer Entwicklungsumgebung](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), und gehen Sie wie folgt vor.

    Wenn Sie [Storm in der Entwicklungsumgebung installiert haben](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), können Sie stattdessen die folgenden Befehle verwenden:

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    Mit dem Parameter `--local` wird die Topologie im lokalen Modus in Ihrer Entwicklungsumgebung ausgeführt. Der Parameter `-R /topology.yaml` nutzt die Dateiressource `topology.yaml` aus der JAR-Datei, um die Topologie zu definieren.

    Während der Ausführung zeigt die Topologie zunächst Startinformationen an. Der folgende Text ist ein Beispiel der Ausgabe:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Es gibt eine Verzögerung von 10 Sekunden zwischen Batches protokollierter Informationen.

2. Erstellen Sie eine Kopie der Datei `topology.yaml` aus dem Projekt. Geben Sie der neuen Datei den Namen `newtopology.yaml`. Suchen Sie in der Datei `newtopology.yaml` nach dem folgenden Abschnitt, und ändern Sie den Wert von `10` in `5`. Durch diese Änderung wird das Intervall zwischen dem Ausgeben der Batches mit der Wortanzahl von 10 Sekunden in 5 Sekunden geändert.

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. To run the topology, use the following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    Oder wenn Sie Storm in der Entwicklungsumgebung installiert haben:

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    Ändern Sie `/path/to/newtopology.yaml` in den Pfad zur Datei „newtopology.yaml“, die Sie im vorherigen Schritt erstellt haben. Bei diesem Befehl wird „newtopology.yaml“ als Topologiedefinition verwendet. Da wir den Parameter `compile` nicht eingebunden haben, verwendet Maven wieder die Version des Projekts, das in den vorherigen Schritten erstellt wurde.

    Nachdem die Topologie gestartet wurde, sollte erkennbar sein, dass sich der Zeitraum zwischen der Ausgabe der Batches gemäß dem Wert in „newtopology.yaml“ geändert hat. Sie haben gelernt, dass Sie die Konfiguration über eine YAML-Datei ändern können, ohne die Topologie neu kompilieren zu müssen.

Weitere Informationen zu diesen und anderen Features des Flux-Frameworks finden Sie unter [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

## <a name="trident"></a>Trident

Trident ist eine von Storm bereitgestellte hohe Abstraktionsebene. Trident ermöglicht eine statusbehaftete Verarbeitung. Der wichtigste Vorteil von Trident ist die Gewährleistung, dass jede Nachricht, die in die Topologie eingegeben wird, nur einmal verarbeitet wird. Ohne Trident kann Ihre Topologie nur sicherstellen, dass Nachrichten mindestens einmal verarbeitet werden. Es gibt noch weitere Unterschiede, z. B. integrierte Komponenten, die Anstelle der Erstellung von Bolts verwendet werden können. Tatsächlich werden Bolts durch weniger generische Komponenten wie Filter, Projektionen und Funktionen ersetzt.

Trident-Anwendungen können mithilfe von Maven-Projekten erstellt werden. Sie verwenden die gleichen Schritte wie weiter oben – nur der Code ist unterschiedlich. Trident kann (derzeit) auch nicht mit dem Flux-Framework verwendet werden.

Weitere Informationen zu Trident finden Sie unter [Trident API Overview](http://storm.apache.org/documentation/Trident-API-Overview.html) (in englischer Sprache).

Ein Beispiel einer Trident-Anwendung finden Sie unter [Beliebte Twitter-Themen mit Apache Storm in HDInsight](hdinsight-storm-twitter-trending.md).

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfahren, wie Sie eine Storm-Topologie mit Java erstellen. Nun lernen Sie folgende Inhalte:

* [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Weitere Beispiel-Storm-Topologien finden Sie unter [Beispieltopologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).


