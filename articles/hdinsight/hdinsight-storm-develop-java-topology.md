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
   ms.date="08/01/2016"
   ms.author="larryfr"/>

#Entwickeln von Java-basierten Topologien für eine einfache Anwendung zum Zählen von Wörtern mit Apache Storm und Maven auf HDInsight

Erlernen Sie die grundlegende Vorgehensweise für das Erstellen einer Java-basierten Topologie für Apache Storm in HDInsight mithilfe von Maven. Sie werden durch den Prozess der Erstellung einer grundlegenden Anwendung zur Wortzählung mithilfe von Maven und Java geführt. Obwohl die entsprechenden Anweisungen für die Verwendung von Eclipse bereitgestellt werden, können Sie auch einen beliebigen Text-Editor wählen.

Nach Abschluss der Schritte in diesem Dokument verfügen Sie über eine einfache Topologie, die Sie für Apache Storm in HDInsight bereitstellen können.

> [AZURE.NOTE] Eine vollständige Version dieser Topologie finden Sie unter [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

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

##Hinzufügen von Abhängigkeiten

Da dies eine Storm-Topologie ist, müssen Sie eine Abhängigkeit für Storm-Komponenten hinzufügen. Öffnen Sie die Datei **pom.xml**, und fügen Sie im Abschnitt **&lt;dependencies>** folgenden Code hinzu:

	<dependency>
	  <groupId>org.apache.storm</groupId>
	  <artifactId>storm-core</artifactId>
      <!-- Storm 0.10.0 is for HDInsight 3.3 and 3.4.
           To find the version information for earlier HDInsight cluster
           versions, see https://azure.microsoft.com/de-DE/documentation/articles/hdinsight-component-versioning/ -->
	  <version>0.10.0</version>
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

Um die Anforderungen für das Einrichten von externen Datenquellen zu verringern, gibt der folgende Spout willkürliche Sätze aus. Es handelt sich um eine modifizierte Version eines Spouts, der mit den ([Storm-Startbeispielen](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter)) bereitgestellt wird.

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

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
      //Create logger for this class
      private static final Logger logger = LogManager.getLogger(WordCount.class);
      
      //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
          //Get the word contents from the tuple
          String word = tuple.getString(0);
          //Have we counted any already?
          Integer count = counts.get(word);
          if (count == null)
            count = 0;
          //Increment the count and store it
          count++;
          counts.put(word, count);
          //Emit the word and the current count
          collector.emit(new Values(word, count));
          //Log information
          logger.info("Emitting a count of " + count + " for word " + word);
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

Der Abschnitt `<Root level="error">` dient zum Konfigurieren der Stammebene der Protokollierung (alles, was nicht in __com.microsoft.example__ enthalten ist) auf ausschließlich Fehlerinformationen.

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
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word i
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word at
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word with
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word nature
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word two
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 51 for word am

Wenn Sie die Protokolldaten betrachten, die vom Bolt „WordCount“ ausgegeben werden, erkennen Sie, dass „apple“ 53 Mal ausgegeben wurde. Der Zähler steigt weiter an, solange die Topologie aktiv ist, da dieselben Sätze fortlaufend nach dem Zufallsprinzip ausgegeben werden und der Zähler nie zurückgesetzt wird.

##Trident

Trident ist eine von Storm bereitgestellte hohe Abstraktionsebene. Trident ermöglicht eine statusbehaftete Verarbeitung. Der wichtigste Vorteil von Trident ist die Gewährleistung, dass jede Nachricht, die in die Topologie eingegeben wird, nur einmal verarbeitet wird. In einer reinen Java-Topologie, in der gewährleistet wird, dass jede Nachricht mindestens einmal verarbeitet wird, ist dies schwer zu erreichen. Es gibt noch weitere Unterschiede, z. B. integrierte Komponenten, die Anstelle der Erstellung von Bolts verwendet werden können. Tatsächlich werden Bolts durch weniger generische Komponenten wie Filter, Projektionen und Funktionen vollständig ersetzt.

Trident-Anwendungen können mithilfe von Maven-Projekten erstellt werden. Sie verwenden die gleichen Schritte wie weiter oben – nur der Code ist unterschiedlich.

Weitere Informationen zu Trident finden Sie unter <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Trident API Overview</a> (in englischer Sprache).

Ein Beispiel einer Trident-Anwendung finden Sie unter [Beliebte Twitter-Themen mit Apache Storm in HDInsight](hdinsight-storm-twitter-trending.md).

##Nächste Schritte

Sie haben erfahren, wie Sie eine Storm-Topologie mit Java erstellen. Nun lernen Sie folgende Inhalte:

* [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Weitere Beispiel-Storm-Topologien finden Sie unter [Beispieltopologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).

<!---HONumber=AcomDC_0914_2016-->