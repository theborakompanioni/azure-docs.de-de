<properties
   pageTitle="Entwickeln Java-basierter Topologien für Apache Storm | Microsoft Azure"
   description="Erfahren Sie, wie Sie Storm-Topologien in Java erstellen, indem Sie eine einfache Topologie zur Wortzählung erstellen."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
	tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#Entwickeln von Java-basierten Topologien für eine einfache Anwendung zum Zählen von Wörtern mit Apache Storm und Maven auf HDInsight

In diesem Artikel erfahren Sie, wie Sie eine Java-basierte Topologie für Apache Storm in HDInsight mithilfe von Maven erstellen. Der Prozess zur Erstellung einer grundlegenden Anwendung zur Wortzählung mithilfe von Maven und Java wird beschrieben, wobei die Topologie in Java definiert ist. Anschließend erfahren Sie, wie Sie die Topologie mit dem Flux-Framework definieren.

> [AZURE.NOTE] Das Flux-Framework ist unter Storm 0.10.0 oder höher verfügbar. Storm 0.10.0 ist unter HDInsight 3.3 und 3.4 verfügbar.

Nach Abschluss der Schritte in diesem Dokument verfügen Sie über eine einfache Topologie, die Sie für Apache Storm in HDInsight bereitstellen können.

> [AZURE.NOTE] Eine vollständige Version der Topologien, die in diesem Dokument erstellt werden, finden Sie unter [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

##Voraussetzungen

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java Developer Kit (JDK), Version 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven</a> – Maven ist ein Projekterstellungssystem für Java-Projekte.

* Ein Text-Editor wie z. B. Notepad, <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Sublime Text</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Oder verwenden Sie eine integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) wie z. B. <a href="https://eclipse.org/" target="_blank">Eclipse</a> (Version Luna oder höher).

	> [AZURE.NOTE] Der Editor oder die IDE verfügen möglicherweise über bestimmte Funktionen für die Arbeit mit Maven, die in diesem Dokument nicht behandelt werden. Informationen zu den Funktionen der Bearbeitungsumgebung finden Sie in der Dokumentation für das von Ihnen verwendete Produkt.

##Konfigurieren von Umgebungsvariablen

Bei der Installation von Java und dem JDK können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* **JAVA\_HOME** – sollte auf das Verzeichnis verweisen, in dem die Java-Laufzeitumgebung (Java Runtime Environment, JRE) installiert ist. Für eine Unix- oder Linux-Distribution sollte z. B. ein Wert wie `/usr/lib/jvm/java-7-oracle` verwendet werden. Unter Windows sollte der Wert so ähnlich sein wie `c:\Program Files (x86)\Java\jre1.7`

* **PATH** – sollte die folgenden Pfade enthalten:

	* **JAVA\_HOME** (oder den entsprechenden Pfad)

	* **JAVA\_HOME\\bin** (oder den entsprechenden Pfad)

	* Das Verzeichnis, in dem Maven installiert ist.

##Erstellen eines neuen Maven-Projekts

Verwenden Sie folgenden Code über die Befehlszeile, um ein neues Maven-Projekt namens **WordCount** zu erstellen:

	mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

Dadurch wird am aktuellen Speicherort ein neues Verzeichnis namens **WordCount** erstellt, das ein einfaches Maven-Projekt enthält.

Das Verzeichnis **WordCount** enthält die folgenden Elemente:

* **pom.xml**: enthält Einstellungen für das Maven-Projekt.

* **src\\main\\java\\com\\microsoft\\example**: enthält den Anwendungscode.

* **src\\test\\java\\com\\microsoft\\example**: enthält Tests für die Anwendung. In diesem Beispiel werden keine Tests erstellt.

###Entfernen des Beispielcodes

Da unsere Anwendung von Grund auf neu erstellt wird, löschen Sie die generierten Test- und Anwendungsdateien:

*  **src\\test\\java\\com\\microsoft\\example\\AppTest.java**

*  **src\\main\\java\\com\\microsoft\\example\\App.java**

##Eigenschaften hinzufügen

Maven ermöglicht Ihnen das Definieren von Werten auf Projektebene, die als Eigenschaften bezeichnet werden. Fügen Sie nach der Zeile `<url>http://maven.apache.org</url>` Folgendes hinzu:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Wir können diese Werte nun in anderen Abschnitten verwenden. Beim Angeben der Version von Storm-Komponenten können wir `${storm.version}` verwenden, anstatt einen Wert hartzucodieren.

##Hinzufügen von Abhängigkeiten

Da dies eine Storm-Topologie ist, müssen Sie eine Abhängigkeit für Storm-Komponenten hinzufügen. Öffnen Sie die Datei **pom.xml**, und fügen Sie im Abschnitt **&lt;dependencies>** folgenden Code hinzu:

	<dependency>
	  <groupId>org.apache.storm</groupId>
	  <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
	  <!-- keep storm out of the jar-with-dependencies -->
	  <scope>provided</scope>
	</dependency>

Zum Zeitpunkt der Kompilierung verwendet Maven diese Informationen für die Suche nach **storm-core** im Maven-Repository. Zuerst wird im Repository auf dem lokalen Computer gesucht. Wenn die Dateien nicht vorhanden sind, werden sie aus dem öffentlichen Maven-Repository heruntergeladen und im lokalen Repository gespeichert.

> [AZURE.NOTE] Beachten Sie die Zeile `<scope>provided</scope>` im hinzugefügten Abschnitt. Diese Zeile weist Maven an, dass **storm-core** aus allen erstellten JAR-Dateien ausgeschlossen wird, da es vom System bereitgestellt wird. Dadurch können kleinere Pakete erstellt und somit sichergestellt werden, dass sie die auf dem Storm-Cluster in HDInsight enthaltenen **storm-core**-Bits verwenden.

##Buildkonfiguration

Mithilfe von Maven-Plug-Ins können Sie die Erstellungsphasen für das Projekt anpassen, z. B. die Vorgehensweise bei der Kompilierung oder beim Verpacken in eine JAR-Datei. Öffnen Sie die Datei **pom.xml**, und fügen Sie den folgenden Code direkt oberhalb der Zeile `</project>` ein.

	<build>
	  <plugins>
	  </plugins>
      <resources>
      </resources>
	</build>

Dieser Abschnitt wird zum Hinzufügen von Plug-Ins, Ressourcen und anderen Optionen für die Buildkonfiguration verwendet. Eine vollständige Referenz zur Datei __pom.xml__ finden Sie unter [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###Hinzufügen von Plug-Ins

Für Storm-Topologien ist das <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Exec Maven-Plug-In</a> hilfreich, da es Ihnen ermöglicht, die Topologie einfach lokal in Ihrer Entwicklungsumgebung auszuführen. Fügen Sie den folgenden Code im Abschnitt `<plugins>` der Datei **pom.xml** hinzu, um das Exec Maven-Plug-In einzubeziehen.

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
      </configuration>
    </plugin>

> [AZURE.NOTE] Beachten Sie, dass für den `<mainClass>`-Eintrag `${storm.topology}` verwendet wird. Wir haben dies oben im Abschnitt zu den Eigenschaften nicht definiert (aber es wäre möglich gewesen). Stattdessen legen Sie diesen Wert über die Befehlszeile fest, wenn Sie die Topologie in einem späteren Schritt in Ihrer Entwicklungsumgebung ausführen.

Ein weiteres nützliches Plug-In ist das <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache Maven Compiler-Plug-In</a>, das zum Ändern von Kompilierungsoptionen verwendet wird. Der Hauptgrund für die Erforderlichkeit dieses Plug-Ins ist die Änderung der Java-Version, die Maven für die Quelle und das Ziel der Anwendung verwendet. Erforderlich ist Version 1.7.

Fügen Sie folgenden Code im Abschnitt `<plugins>` der Datei **pom.xml** hinzu, um das Apache Maven Compiler-Plug-In einzubeziehen und als Quelle und Ziel Version 1.7 festzulegen.

	<plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###Konfigurieren der Ressourcen

Im Abschnitt „resources“ können Sie nicht codebezogene Ressourcen, z.B. Konfigurationsdateien, hinzufügen, die von Komponenten in der Topologie benötigt werden. Fügen Sie für dieses Beispiel der Datei **pom.xml** im Abschnitt `<resources>` Folgendes hinzu.

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Dadurch wird das Verzeichnis „resources“ im Stammverzeichnis des Projekts (`${basedir}`) als Speicherort hinzugefügt, der Ressourcen und die Datei mit dem Namen __log4j2.xml__ enthält. Diese Datei dient zum Konfigurieren, welche Informationen von der Topologie protokolliert werden.

##Erstellen der Topologie

Eine Java-basierte Storm-Topologie besteht aus drei Komponenten, die Sie als Abhängigkeit erstellen (oder referenzieren) müssen.

* **Spouts:** liest Daten aus externen Quellen und gibt Datenströme in die Topologie aus.

* **Bolts.** verarbeitet Datenströme, die von Spouts oder anderen Bolts ausgegeben werden, und gibt einen oder mehrere Datenströme aus.

* **Topology:** definiert die Anordnung der Spouts und Bolts und stellt den Einstiegspunkt für die Topologie bereit.

###Erstellen des Spouts

Um die Anforderungen für das Einrichten von externen Datenquellen zu verringern, gibt der folgende Spout willkürliche Sätze aus. Es handelt sich um eine modifizierte Version eines Spouts, der mit den [Storm-Startbeispielen](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter) bereitgestellt wird.

> [AZURE.NOTE] Ein Beispiel zu einem Spout, der aus einer externen Datenquelle liest, finden Sie in den folgenden Beispielen:
>
> * [TwitterSampleSpout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): ein Beispiel-Spout, der Daten aus Twitter liest
>
> * [Storm-Kafka:](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka) ein Spout, der Dateien aus Kafka liest

Erstellen Sie für den Spout eine neue Datei namens **RandomSentenceSpout.java** im Verzeichnis **src\\main\\java\\com\\microsoft\\example**, und verwenden Sie Folgendes als Inhalt:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

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
        //The sentences that will be randomly emitted
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

Lesen Sie die Codekommentare in Ruhe durch, um die Funktionsweise des Spouts zu verstehen.

> [AZURE.NOTE] Obwohl in dieser Topologie nur ein Spout verwendet wird, verfügen andere Topologien möglicherweise über mehrere Spouts, die der Topologie Daten aus verschiedenen Quellen zuführen.

###Erstellen des Bolts

Bolts übernehmen die Datenverarbeitung. In dieser Topologie gibt es zwei Bolts:

* **SplitSentence**: unterteilt die von **RandomSentenceSpout** ausgegebenen Sätze in einzelne Wörter

* **WordCount**: zählt das Vorkommen der einzelnen Wörter

> [AZURE.NOTE] Bolts können alle Aufgaben übernehmen, beispielsweise Berechnungen, Persistenz oder Kommunikation mit externen Komponenten.

Erstellen Sie zwei neue Dateien, **SplitSentence.java** und **WordCount.Java**, im Verzeichnis **src\\main\\java\\com\\microsoft\\example**. Fügen Sie den folgenden Inhalt in die Dateien ein:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
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

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
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

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Lesen Sie die Codekommentare in Ruhe durch, um die Funktionsweise der einzelnen Bolts zu verstehen.

###Definieren der Topologie

Die Topologie verbindet die Spouts und Bolts in einem Diagramm, in dem definiert ist, wie die Daten zwischen den Komponenten verlaufen. Darüber hinaus bietet sie Hinweise zu Parallelität, die von Storm beim Erstellen von Instanzen der Komponenten innerhalb des Clusters verwendet werden.

Nachfolgend sehen Sie eine einfache Darstellung des Komponentendiagramms für diese Topologie.

![Diagramm mit der Anordnung von Spouts und Bolts](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Erstellen Sie zum Implementieren der Topologie eine neue Datei namens **WordCountTopology.java** im Verzeichnis **src\\main\\java\\com\\microsoft\\example**. Fügen Sie den folgenden Inhalt in die Datei ein:

	package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

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
        //Set to false to disable debug information
        // when running in production mode.
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

Lesen Sie die Codekommentare in Ruhe durch, um die Definition der Topologie zu verstehen, die dann an den Cluster übermittelt wird.

###Konfigurieren der Protokollierung

Storm verwendet Apache Log4j für die Protokollierung von Informationen. Wenn Sie die Protokollierung nicht konfigurieren, gibt die Topologie sehr viele Diagnose-Informationen aus, die schwer zu lesen sein können. Um zu steuern, was protokolliert wird, erstellen Sie im Verzeichnis __resources__ die Datei __log4j2.xml__. Fügen Sie Folgendes als Inhalt der Datei hinzu.

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

Hiermit wird eine neue Protokollierung für die __com.microsoft.example__-Klasse konfiguriert, die die Komponenten in dieser Beispieltopologie enthält. Die Ebene für diese Protokollierung wird auf „trace“ festgelegt. Dadurch werden alle Protokollinformationen erfasst, die von Komponenten in dieser Topologie ausgegeben werden. Wenn Sie sich den Code dieses Projekt nochmals ansehen, werden Sie feststellen, dass nur die Datei „WordCount.java“ eine Protokollierung implementiert. Damit wird die Anzahl der einzelnen Worte aufgezeichnet.

Der Abschnitt `<Root level="error">` dient zum ausschließlichen Festlegen der Stammebene für die Protokollierung (alles, was nicht in __com.microsoft.example__ enthalten ist) auf Fehlerinformationen.

> [AZURE.IMPORTANT] Während dadurch die beim Testen einer Topologie in Ihrer Entwicklungsumgebung protokollierten Informationen wesentlich reduziert werden, lassen sich dadurch nicht alle Debuginformationen entfernen, die bei Ausführung in einem Produktionscluster erzeugt werden. Um diese Informationen zu verringern, müssen Sie auch in der Konfiguration, die an den Cluster übermittelt wird, „debugging“ auf „false“ festlegen. Ein Beispiel finden Sie in diesem Dokument im Code von „WordCountTopology.java“.

Weitere Informationen zum Konfigurieren der Protokollierung für Log4j finden Sie unter [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Die Storm-Version 0.10.0 verwendet Log4j 2.x. Ältere Versionen von Storm verwenden Log4j 1.x mit einem anderen Format für die Protokollkonfiguration. Informationen zur älteren Konfiguration finden Sie unter [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##Lokales Testen der Topologie

Nach dem Speichern der Dateien verwenden Sie folgenden Befehl, um die Topologie lokal zu testen.

	mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Während der Ausführung zeigt die Topologie zunächst Startinformationen an. Dann werden im Verlauf der Ausgabe von Sätzen durch den Spout und ihrer Verarbeitung durch die Bolts Zeilen ähnlich den folgenden angezeigt.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Wenn Sie die Protokolldaten betrachten, die vom Bolt „WordCount“ ausgegeben werden, erkennen Sie, dass „and“ 113-mal ausgegeben wurde. Diese Anzahl steigt weiter an, solange die Topologie ausgeführt wird, da der Spout fortlaufend die gleichen Sätze ausgibt.

Zwischen der Ausgabe von Wörtern und der Anzahl liegt außerdem ein Intervall von fünf Sekunden. Dies tritt auf, da die __WordCount__-Komponente so konfiguriert ist, dass nur Informationen ausgegeben werden, wenn ein Tick-Tupel eintrifft. Standardmäßig wird angefordert, dass Tupel dieser Art nur alle fünf Sekunden zugestellt werden.

## Konvertieren der Topologie in Flux

Flux ist ein neues Framework von Storm 0.10.0, mit dem Sie die Konfiguration von der Implementierung trennen können. Ihre Komponenten (Bolts und Spouts) werden weiterhin in Java definiert, aber die Topologie wird mit einer YAML-Datei definiert.

Die YAML-Datei definiert die Komponenten für die Topologie, den Datenfluss untereinander und welche Werte beim Initialisieren der Komponenten verwendet werden. Sie können bei der Bereitstellung eine YAML-Datei als Teil der JAR-Datei einbinden, in der das Projekt enthalten ist, oder Sie können eine externe YAML-Datei verwenden, wenn Sie die Topologie starten.

1. Verschieben Sie die Datei __WordCountTopology.java__ aus dem Projekt heraus. Bisher wurde die Topologie mit dieser Datei definiert, aber wir verwenden sie für Flux nicht.

2. Erstellen Sie im Verzeichnis __resources__ eine neue Datei mit dem Namen __topology.yaml__. Fügen Sie Folgendes als Inhalt der Datei hinzu:

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Nehmen Sie sich einen Moment Zeit, um sich die einzelnen Abschnitte durchzulesen und zu erfahren, was seine Aufgabe ist und wie er sich zur Java-basierten Definition in der Datei __WordCountTopology.java__ verhält.

3. Nehmen Sie die folgenden Änderungen an der Datei __pom.xml__ vor.

    * Fügen Sie im Abschnitt `<dependencies>` die folgende neue Abhängigkeit hinzu:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Fügen Sie im Abschnitt `<plugins>` das folgende Plug-In hinzu. Dieses Plug-In ist für die Erstellung eines Pakets (JAR-Datei) für das Projekt zuständig und wendet beim Erstellen des Pakets einige spezifische Transformationen für Flux an.

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

    * Ändern Sie im Abschnitt __exec-maven-plugin__ `<configuration>` den Wert für `<mainClass>` in `org.apache.storm.flux.Flux`. So ermöglichen wir, dass Flux die Ausführung der Topologie steuern kann, wenn wir sie während der Entwicklung lokal ausführen.

    * Fügen Sie `<includes>` im Abschnitt `<resources>` Folgendes hinzu: Hierin ist die YAML-Datei enthalten, mit der die Topologie als Teil des Projekts definiert wird.
    
            <include>topology.yaml</include>

## Lokales Testen der Flux-Topologie

1. Verwenden Sie Folgendes, um die Flux-Topologie zu kompilieren und per Maven auszuführen.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Gehen Sie wie folgt vor, wenn Sie PowerShell verwenden:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Wenn Sie sich auf einem System mit Linux/Unix/OS X befinden und [Storm in der Entwicklungsumgebung installiert haben](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), können Sie stattdessen die folgenden Befehle verwenden:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    Mit dem Parameter `--local` wird die Topologie im lokalen Modus in Ihrer Entwicklungsumgebung ausgeführt. Der Parameter `-R /topology.yaml` nutzt die Dateiressource `topology.yaml` aus der JAR-Datei, um die Topologie zu definieren.

    Während der Ausführung zeigt die Topologie zunächst Startinformationen an. Dann werden im Verlauf der Ausgabe von Sätzen durch den Spout und ihrer Verarbeitung durch die Bolts Zeilen ähnlich den folgenden angezeigt.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Zwischen Batches mit protokollierten Informationen liegt eine Verzögerung von zehn Sekunden, da von der Datei `topology.yaml` der Wert `10` übergeben wird, wenn die WordCount-Komponente erstellt wird. Hiermit wird das Verzögerungsintervall für das Tick-Tupel auf 10 Sekunden festgelegt.

2.  Erstellen Sie eine Kopie der Datei `topology.yaml` aus dem Projekt. Geben Sie ihr beispielsweise den Namen `newtopology.yaml`. Suchen Sie in der Datei nach dem folgenden Abschnitt, und ändern Sie den Wert von `10` in `5`. Das Intervall zwischen dem Ausgeben der Batches mit der Wortanzahl wird von 10 Sekunden in 5 Sekunden geändert.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Verwenden Sie den folgenden Befehl, um die Topologie auszuführen:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Verwenden Sie Folgendes, wenn Sie in Ihrer Linux/Unix/OS X-Entwicklungsumgebung Storm verwenden:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Ändern Sie `/path/to/newtopology.yaml` in den Pfad zur Datei „newtopology.yaml“, die Sie im vorherigen Schritt erstellt haben. Bei diesem Befehl wird „newtopology.yaml“ als Topologiedefinition verwendet. Da wir den Parameter `compile` nicht eingebunden haben, verwendet Maven wieder die Version des Projekts, das in den vorherigen Schritten erstellt wurde.

    Nachdem die Topologie gestartet wurde, sollte erkennbar sein, dass sich der Zeitraum zwischen der Ausgabe der Batches gemäß dem Wert in „newtopology.yaml“ geändert hat. Sie haben gelernt, dass Sie die Konfiguration über eine YAML-Datei ändern können, ohne die Topologie neu kompilieren zu müssen.

Es gibt mehrere andere Features, die von Flux bereitgestellt werden, auf die hier aber nicht eingegangen werden kann. Ein Beispiel ist die Variablenersetzung in der YAML-Datei anhand von Parametern, die zur Laufzeit übergeben werden, oder über Umgebungsvariablen. Weitere Informationen zu diesen und anderen Features des Flux-Frameworks finden Sie unter [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##Trident

Trident ist eine von Storm bereitgestellte hohe Abstraktionsebene. Trident ermöglicht eine statusbehaftete Verarbeitung. Der wichtigste Vorteil von Trident ist die Gewährleistung, dass jede Nachricht, die in die Topologie eingegeben wird, nur einmal verarbeitet wird. In einer reinen Java-Topologie, in der gewährleistet wird, dass jede Nachricht mindestens einmal verarbeitet wird, ist dies schwer zu erreichen. Es gibt noch weitere Unterschiede, z. B. integrierte Komponenten, die Anstelle der Erstellung von Bolts verwendet werden können. Tatsächlich werden Bolts durch weniger generische Komponenten wie Filter, Projektionen und Funktionen vollständig ersetzt.

Trident-Anwendungen können mithilfe von Maven-Projekten erstellt werden. Sie verwenden die gleichen Schritte wie weiter oben – nur der Code ist unterschiedlich. Trident kann (derzeit) auch nicht mit dem Flux-Framework verwendet werden.

Weitere Informationen zu Trident finden Sie unter <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Trident API Overview</a> (in englischer Sprache).

Ein Beispiel einer Trident-Anwendung finden Sie unter [Beliebte Twitter-Themen mit Apache Storm in HDInsight](hdinsight-storm-twitter-trending.md).

##Nächste Schritte

Sie haben erfahren, wie Sie eine Storm-Topologie mit Java erstellen. Nun lernen Sie folgende Inhalte:

* [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Weitere Beispiel-Storm-Topologien finden Sie unter [Beispieltopologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).

<!---HONumber=AcomDC_0921_2016-->