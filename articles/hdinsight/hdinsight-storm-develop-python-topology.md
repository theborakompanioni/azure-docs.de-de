---
title: Verwenden von Python-Komponenten in einer Storm-Topologie in HDinsight | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Python-Komponenten mit Apache Storm in Azure HDInsight verwenden. Das Dokument enthält Informationen zur Verwendung von Python-Komponenten in Java- und Clojure-basierten Storm-Topologien."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: edd0ec4f-664d-4266-910c-6ecc94172ad8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 8b32aa77e1dbe18076d73e10914b59be107c3588
ms.lasthandoff: 03/25/2017


---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Entwickeln von Apache Storm-Topologien mit Python in HDInsight

Apache Storm unterstützt mehrere Sprachen und ermöglicht sogar das Kombinieren von Komponenten in verschiedenen Sprachen in einer Topologie. In diesem Dokument erfahren Sie, wie Sie Python-Komponenten in Ihren Java- und Clojure-basierten Storm-Topologien in HDInsight verwenden.

> [!IMPORTANT]
> Dieses Dokument enthält die Schritte zur Verwendung von Windows- und Linux-basierten HDInsight-Clustern. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Voraussetzungen

* Python 2.7 oder höher
* Java JDK 1.7 oder höher
* [Leiningen](http://leiningen.org/)

## <a name="storm-multi-language-support"></a>Storm-Unterstützung für mehrere Sprachen

Storm ist für die Verwendung mit in beliebigen Programmiersprachen geschriebenen Komponenten konzipiert. Dies erfordert jedoch, dass die Komponenten „verstehen“, wie die [Thrift-Definition für Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift) verwendet wird. Für Python wird ein Modul als Teil des Apache Storm-Projekts bereitgestellt, das Ihnen eine problemlose Verknüpfung mit Storm ermöglicht. Sie finden dieses Modul unter [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Da Apache Storm ein Java-Prozess ist, der auf der Java Virtual Machine (JVM) ausgeführt wird, werden in anderen Sprachen geschriebene Komponenten als Unterprozesse ausgeführt. Die in der JVM ausgeführten Storm-Bits kommunizieren mithilfe von JSON-Nachrichten, die über „stdin/stdout“ gesendet werden, mit diesen Unterprozessen. Weitere Informationen zur Kommunikation zwischen Komponenten finden Sie in der Dokumentation zum [Multi-Lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) .

### <a name="the-storm-module"></a>Das Storm-Modul
Das Storm-Modul (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py) bietet die erforderlichen Bits zum Erstellen von Python-Komponenten, die mit Storm eingesetzt werden können.

Das Modul unterstützt u. a. `storm.emit` zum Ausgeben von Tupeln und `storm.logInfo` zum Schreiben in die Protokolle. Ich empfehle Ihnen, diese Datei durchzulesen und sich damit vertraut zu machen, welche Möglichkeiten sie bietet.

## <a name="challenges"></a>Herausforderungen
Mithilfe des Moduls **storm.py** können Sie Python-Spouts erstellen, die Daten nutzen, und Bolts, die Daten verarbeiten. Die allgemeine Storm-Topologiedefinition für die Kommunikation zwischen Komponenten wird jedoch nach wie vor in Java oder Clojure geschrieben. Zudem müssen Sie bei der Verwendung von Java auch Java-Komponenten erstellen, die als Schnittstelle zu den Python-Komponenten fungieren.

Da Storm-Cluster verteilt ausgeführt werden, müssen Sie außerdem sicherstellen, dass alle erforderlichen Module für Ihre Python-Komponenten auf sämtlichen Workerknoten im Cluster verfügbar sind. Storm bietet keine Möglichkeit, mit der sich dies für mehrsprachige Ressourcen einfach bewerkstelligen lässt. Sie müssen entweder alle Abhängigkeiten in die JAR-Datei für die Topologie aufnehmen oder Abhängigkeiten manuell auf jedem Workerknoten im Cluster installieren.

### <a name="java-vs-clojure-topology-definition"></a>Java- und Clojure-Topologiedefinitionen im Vergleich
Von den zwei Methoden zum Definieren einer Topologie ist Clojure bei weitem die einfachste/sauberste Methode, da Sie direkt in der Topologiedefinition auf Python-Komponenten verweisen können. Bei Java-basierten Topologiedefinitionen müssen Sie auch Java-Komponenten für bestimmte Aufgaben definieren, z. B. zum Deklarieren der Felder in den von den Python-Komponenten zurückgegebenen Tupeln.

In diesem Dokument werden beide Methoden anhand von Beispielprojekten beschrieben.

## <a name="python-components-with-a-java-topology"></a>Python-Komponenten mit einer Java-Topologie
> [!NOTE]
> Dieses Beispiel finden Sie unter [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) im Verzeichnis **JavaTopology** . Dies ist ein Maven-basiertes Projekt. Falls Sie nicht mit Maven vertraut sind, finden Sie unter [Entwickeln von Java-basierten Topologien mit Apache Storm in HDInsight](hdinsight-storm-develop-java-topology.md) weitere Informationen zum Erstellen eines Maven-Projekts für eine Storm-Topologie.
> 
> 

Eine Java-basierte Topologie mit Python (oder anderen JVM-Sprachkomponenten) scheint auf den ersten Blick Java-Komponenten zu verwenden. Wenn Sie sich aber die einzelnen Java-Spouts/-Bolts ansehen, werden Sie Code wie den folgenden finden:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

An dieser Stelle ruft Java Python auf und führt das Skript aus, das die eigentliche Bolt-Logik enthält. Die Java-Spouts/-Bolts (in diesem Beispiel) deklarieren lediglich die Felder im Tupel, das von der zugrunde liegenden Python-Komponente ausgegeben wird.

Die eigentlichen Python-Dateien werden in diesem Beispiel im Verzeichnis `/multilang/resources` gespeichert. Auf das Verzeichnis `/multilang` wird in der Datei **pom.xml**verwiesen:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir}/multilang</directory>
    </resource>
</resources>

Dadurch werden alle Dateien im Ordner `/multilang` in die JAR-Datei eingeschlossen, die aus diesem Projekt erstellt wird.

> [!IMPORTANT]
> Beachten Sie, dass hiermit nur das Verzeichnis `/multilang` angegeben wird und nicht `/multilang/resources`. Da Storm Nicht-JVM-Ressourcen in einem `resources` -Verzeichnis erwartet, wird bereits intern danach gesucht. Die Platzierung von Komponenten in diesem Ordner ermöglicht es Ihnen lediglich, im Java-Code anhand des Namens auf sie zu verweisen. Beispiel: `super("python", "countbolt.py");`. Sie können sich das so vorstellen, als würde Storm das Verzeichnis `resources` beim Zugriff auf mehrsprachige Ressourcen als das Stammverzeichnis (/) betrachten.
> 
> Bei diesem Beispielprojekt ist das Modul `storm.py` im Verzeichnis `/multilang/resources` enthalten.
> 
> 

### <a name="build-and-run-the-project"></a>Erstellen und Ausführen des Projekts
Zur lokalen Ausführung können Sie das Projekt einfach mit dem folgenden Maven-Befehl erstellen und im lokalen Modus ausführen:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Verwenden Sie STRG+C, um den Prozess zu beenden.

Um das Projekt auf einem HDInsight-Cluster mit Apache Storm bereitzustellen, gehen Sie wie folgt vor:

1. Erstellen Sie ein Uberjar:
   
        mvn package
   
    Dadurch wird eine Datei mit dem Namen **WordCount--1.0-SNAPSHOT.jar** im Verzeichnis `/target` für das Projekt erstellt.
2. Laden Sie die JAR-Datei mit einer der folgenden Methoden in den Hadoop-Cluster hoch:
   
   * Für **Linux-basierte** HDInsight-Cluster: Verwenden Sie `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar`, um die JAR-Datei in den Cluster zu kopieren, und ersetzen Sie dabei USERNAME durch Ihren SSH-Benutzernamen und CLUSTERNAME durch den Namen des HDInsight-Clusters.
     
       Nachdem die Datei hochgeladen wurde, stellen Sie über SSH eine Verbindung mit dem Cluster her, und starten Sie die Topologie mit `storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`
   * Für **Windows-basierte** HDInsight-Cluster: Stellen Sie eine Verbindung mit dem Storm-Dashboard her, indem Sie in Ihrem Browser zu „HTTPS://CLUSTERNAME.azurehdinsight.net/“ wechseln. Ersetzen Sie CLUSTERNAME durch den Namen Ihres HDInsight-Clusters, und geben Sie den Administratornamen und das Kennwort ein, wenn Sie dazu aufgefordert werden.
     
       Führen Sie mithilfe des Formulars die folgenden Aktionen aus:
     
     * **JAR-Datei**: Wählen Sie **Durchsuchen**, und wählen Sie dann die Datei **WordCount-1.0-SNAPSHOT.jar** aus.
     * **Klassenname**: Geben Sie `com.microsoft.example.WordCount` ein.
     * **Zusätzliche Parameter**: Geben Sie einen Anzeigenamen (z. B. `wordcount`) zum Identifizieren der Topologie ein.
       
       Klicken Sie abschließend auf **Senden** , um die Topologie zu starten.

> [!NOTE]
> Eine gestartete Storm-Topologie wird ausgeführt, bis sie beendet (abgebrochen) wird. Führen Sie zum Beenden der Topologie entweder den Befehl `storm kill TOPOLOGYNAME` über die Befehlszeile aus (z. B. in einer SSH-Sitzung mit einem Linux-Cluster), oder wählen Sie in der Storm-Benutzeroberfläche die Topologie aus, und klicken Sie dann auf die Schaltfläche **Kill**.
> 
> 

## <a name="python-components-with-a-clojure-topology"></a>Python-Komponenten mit einer Clojure-Topologie
> [!NOTE]
> Dieses Beispiel finden Sie unter [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) im Verzeichnis **ClojureTopology** .
> 
> 

Diese Topologie wurde mithilfe von [Leiningen](http://leiningen.org) erstellt, um [ein neues Clojure-Projekt zu erstellen](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project). Danach wurden die folgenden Änderungen am erstellten Projekt vorgenommen:

* **project.clj**: Es wurden Abhängigkeiten für Storm und Ausschlüsse für Elemente hinzugefügt, die bei der Bereitstellung auf dem HDInsight-Server Probleme verursachen können.
* **resources/resources**: Leiningen erstellt ein Standardverzeichnis `resources`, die darin gespeicherten Dateien werden jedoch anscheinend dem Stamm der aus dem Projekt erstellten JAR-Datei hinzugefügt, und Storm erwartet Dateien in einem Unterverzeichnis mit dem Namen `resources`. Daher wurde ein Unterverzeichnis hinzugefügt, und die Python-Dateien werden in `resources/resources`gespeichert. Zur Laufzeit wird dieses Unterverzeichnis als das Stammverzeichnis (/) für den Zugriff auf Python-Komponenten behandelt.
* **src/wordcount/core.clj**: Diese Datei enthält die Topologiedefinition. Auf die Datei wird in **project.clj** verwiesen. Weitere Informationen zur Verwendung von Clojure zum Definieren einer Storm-Topologie finden Sie unter [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

### <a name="build-and-run-the-project"></a>Erstellen und Ausführen des Projekts
Verwenden Sie den folgenden Befehl, um das **Projekt zu erstellen und lokal auszuführen**:

    lein clean, run

Verwenden Sie zum Beenden der Topologie **STRG+C**.

Gehen Sie wie folgt vor, um **ein Uberjar zu erstellen und in HDInsight bereitzustellen**:

1. Erstellen Sie ein Uberjar, das die Topologie und die erforderlichen Abhängigkeiten enthält:
   
        lein uberjar
   
    Dadurch wird eine neue Datei namens `wordcount-1.0-SNAPSHOT.jar` im Verzeichnis `target\uberjar+uberjar` .
2. Verwenden Sie eine der folgenden Methoden, um die Topologie in einem HDInsight-Cluster bereitzustellen und auszuführen:
   
   * **HDInsight (Linux-basiert)**
     
     1. Kopieren Sie die Datei mit `scp`in den Hauptknoten des HDInsight-Clusters. Beispiel:
        
             scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
        
         Ersetzen Sie USERNAME durch einen SSH-Benutzer für Ihren Cluster und CLUSTERNAME durch den Namen Ihres HDInsight-Clusters.
     2. Nachdem die Datei in den Cluster kopiert wurde, stellen Sie über SSH eine Verbindung mit dem Cluster her, und übermitteln Sie den Auftrag. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
     
     3. Führen Sie nach dem Herstellen der Verbindung den folgenden Befehl aus, um die Topologie zu starten:
        
             storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
   * **HDInsight (Windows-basiert)**
     
     1. Stellen Sie eine Verbindung mit dem Storm-Dashboard her, indem Sie in Ihrem Browser zu „HTTPS://CLUSTERNAME.azurehdinsight.net/“ wechseln. Ersetzen Sie CLUSTERNAME durch den Namen Ihres HDInsight-Clusters, und geben Sie den Administratornamen und das Kennwort ein, wenn Sie dazu aufgefordert werden.
     2. Führen Sie mithilfe des Formulars die folgenden Aktionen aus:
        
        * **JAR-Datei**: Wählen Sie **Durchsuchen**, und wählen Sie dann die Datei **wordcount-1.0-SNAPSHOT.jar** aus.
        * **Klassenname**: Geben Sie `wordcount.core` ein.
        * **Zusätzliche Parameter**: Geben Sie einen Anzeigenamen (z. B. `wordcount`) zum Identifizieren der Topologie ein.
          
          Klicken Sie abschließend auf **Senden** , um die Topologie zu starten.

> [!NOTE]
> Eine gestartete Storm-Topologie wird ausgeführt, bis sie beendet (abgebrochen) wird. Führen Sie zum Beenden der Topologie entweder den Befehl `storm kill TOPOLOGYNAME` über die Befehlszeile aus (in einer SSH-Sitzung mit einem Linux-Cluster), oder wählen Sie in der Storm-Benutzeroberfläche die Topologie aus, und klicken Sie dann auf die Schaltfläche **Kill**.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Python-Komponenten in einer Storm-Topologie verwendet werden. In den folgenden Dokumenten werden weitere Möglichkeiten zur Verwendung von Python mit HDInsight beschrieben:

* [Verwenden von Python für Streaming-MapReduce-Aufträge](hdinsight-hadoop-streaming-python.md)
* [Verwenden von benutzerdefinierten Python-Funktionen (UDFs) in Pig und Hive](hdinsight-python.md)


