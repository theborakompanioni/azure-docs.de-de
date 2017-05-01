---
title: Verwenden von Python-Komponenten in einer Storm-Topologie in HDinsight | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Python-Komponenten mit Apache Storm in Azure HDInsight verwenden.
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
ms.date: 04/04/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 39a8eb9ce6a5363f541f02c33cd46d56fdabcae8
ms.lasthandoff: 04/12/2017


---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Entwickeln von Apache Storm-Topologien mit Python in HDInsight

Hier erfahren Sie, wie Sie Python-Komponenten mit Storm in HDInsight verwenden. Apache Storm unterstützt mehrere Sprachen und ermöglicht sogar das Kombinieren von Komponenten in verschiedenen Sprachen in einer Topologie. Mit dem (mit Storm 0.10.0 eingeführten) Flux-Framework können Sie problemlos Lösungen erstellen, die Python-Komponenten verwenden.

> [!IMPORTANT]
> Die Informationen in diesem Dokument wurden mit Storm unter HDInsight 3.5 getestet. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight 3.3 und 3.4](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

Den Code für dieses Projekt finden Sie unter [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Voraussetzungen

* Python 2.7 oder höher

* Java JDK 1.8 oder höher

* Maven 3

* (Optional) Eine lokale Storm-Entwicklungsumgebung. Eine lokale Storm-Umgebung ist nur erforderlich, wenn die Topologie lokal ausgeführt werden soll. Weitere Informationen finden Sie unter [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) (Einrichten einer Entwicklungsumgebung).

## <a name="storm-multi-language-support"></a>Storm-Unterstützung für mehrere Sprachen

Storm wurde für den Einsatz mit Komponenten entwickelt, die in einer beliebigen Programmiersprache geschrieben wurden. Die Komponenten müssen mit der [Thrift-Definition für Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift) umgehen können. Für Python wird ein Modul als Teil des Apache Storm-Projekts bereitgestellt, das Ihnen eine problemlose Verknüpfung mit Storm ermöglicht. Sie finden dieses Modul unter [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Apache Storm ist ein Java-Prozess, der auf der Java Virtual Machine (JVM) ausgeführt wird. In anderen Sprachen geschriebene Komponenten werden als Unterprozesse ausgeführt. Storm kommuniziert mit diesen Unterprozessen über JSON-Meldungen, die über „stdin/stdout“ gesendet werden. Weitere Informationen zur Kommunikation zwischen Komponenten finden Sie in der Dokumentation zum [Multi-Lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) .

## <a name="python-and-the-flux-framework"></a>Python und das Flux-Framework

Mit dem Flux-Framework können Sie Storm-Topologien von den Komponenten getrennt definieren. Während die Komponenten in diesem Beispiel mit Python geschrieben sind, wird die Topologie mit YAML definiert. Der folgende Text zeigt, wie im YAML-Dokument auf eine Python-Komponente verwiesen wird:

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

Die Klasse `FluxShellSpout` dient zum Starten des `sentencespout.py`-Skripts, das den Spout implementiert.

Flux erwartet, dass sich die Python-Skripts im `/resources`-Verzeichnis der JAR-Datei befinden, die die Topologie enthält. Also sind die Python-Skripts in diesem Beispiel im `/multilang/resources`-Verzeichnis gespeichert. Die `pom.xml` bezieht diese Datei mit folgendem XML-Code ein:

```xml
<!-- include the Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Wie bereits erwähnt, gibt es eine `storm.py`-Datei, die die Thrift-Definition für Storm implementiert. Das Flux-Framework bezieht sie automatisch ein, wenn das Projekt erstellt wird, Sie müssen sich also nicht um die Einbeziehung kümmern.

## <a name="build-the-project"></a>Erstellen des Projekts

Führen Sie im Stammverzeichnis des Projekts folgenden Befehl aus:

```bash
mvn clean compile package
```

Dieser Befehl erstellt eine `target/WordCount-1.0-SNAPSHOT.jar`-Datei, die die kompilierte Topologie enthält.

## <a name="run-the-topology-locally"></a>Lokales Ausführen der Topologie

Verwenden Sie den folgenden Befehl, um die Topologie lokal auszuführen:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> Dieser Befehl setzt eine lokale Storm-Entwicklungsumgebung voraus. Weitere Informationen finden Sie unter [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) (Einrichten einer Entwicklungsumgebung).

Sobald die Topologie startet, gibt er Informationen über die lokale Konsole aus, die folgendem Text ähneln:

```
24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
^C24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}
```

Sie können die Topologie mit STRG+C beenden.

## <a name="run-the-topology-on-hdinsight"></a>Ausführen der Topologie unter HDInsight

1. Verwenden Sie den folgenden Befehl, um die `WordCount-1.0-SNAPSHOT.jar`-Datei in Ihren Cluster vom Typ „Storm in HDInsight“ zu kopieren.

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Ersetzen Sie `sshuser` durch den SSH-Benutzer Ihres Clusters. Ersetzen Sie `mycluster` durch den Namen des Clusters. Möglicherweise werden Sie zur Eingabe des Kennworts für den SSH-Benutzer aufgefordert.

    Weitere Informationen zur Verwendung von SSH und SCP finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Wenn die Datei hochgeladen ist, stellen Sie über SSH eine Verbindung mit dem Cluster her:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Verwenden Sie in der SSH-Sitzung den folgenden Befehl, um die Topologie im Cluster zu starten:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. Sie können in der Storm-Benutzeroberfläche die Topologie im Cluster anzeigen. Die Storm-Benutzeroberfläche befindet sich unter „https://mycluster.azurehdinsight.net/stormui“. Ersetzen Sie `mycluster` durch den Namen Ihres Clusters.

> [!NOTE]
> Eine gestartete Storm-Topologie wird ausgeführt, bis sie beendet wird. Sie können die Topologie mithilfe einer der folgenden Methoden beenden:
>
> * Eingabe des `storm kill TOPOLOGYNAME`-Befehls über die Befehlszeile
> * Mit der **Kill**-Schaltfläche in der Storm-Benutzeroberfläche.


## <a name="next-steps"></a>Nächste Schritte

In den folgenden Dokumenten werden weitere Möglichkeiten zur Verwendung von Python mit HDInsight beschrieben:

* [Verwenden von Python für Streaming-MapReduce-Aufträge](hdinsight-hadoop-streaming-python.md)
* [Verwenden von benutzerdefinierten Python-Funktionen (UDFs) in Pig und Hive](hdinsight-python.md)

