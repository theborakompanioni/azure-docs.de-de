---
title: "Einführung in Apache Storm in HDInsight | Microsoft Docs"
description: "Enthält eine Einführung in Apache Storm in HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 5e6ffbb8f1373f7170f87ad0e345a63cc20f08dd
ms.openlocfilehash: 0aa2a7075f64b353f6b052ab6b973a06622a9339
ms.lasthandoff: 03/24/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Einführung in Apache Storm in HDInsight: Echtzeitanalysen für Hadoop

Mit Apache Storm in HDInsight können Sie verteilte Echtzeitanalyselösungen in Azure erstellen.

Apache Storm ist ein verteiltes, fehlertolerantes Open-Source-Berechnungssystem für die Verarbeitung von Daten in Echtzeit mit Hadoop. Storm-Lösungen sind außerdem in der Lage, die Verarbeitung von Daten zu garantieren und Daten erneut abzuspielen, die beim ersten Versuch nicht erfolgreich verarbeitet wurden.

## <a name="why-use-storm-on-hdinsight"></a>Vorteile von Storm in HDInsight

Apache Storm in HDInsight wird als verwalteter Cluster angeboten, der in die Azure-Umgebung integriert ist. Storm und andere Hadoop-Komponenten unter HDInsight basieren auf der Hortonworks Data Platform (HDP), während das Betriebssystem für den Cluster Ubuntu ist (eine Linux-Distribution). Diese Konfiguration umfasst eine Plattform, die kompatibel mit beliebten Tools und Diensten im Hadoop-Ökosystem ist.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

Apache Storm in HDInsight bietet die folgenden Hauptvorteile:

* Ausführung als verwalteter Dienst mit einer SLA von 99,9% Betriebszeit.

* Einfache Anpassung durch Ausführung von Skripts für den Cluster während oder nach der Erstellung. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

* Nutzung der Sprache Ihrer Wahl: Storm-Komponenten können in vielen verschiedenen Sprachen geschrieben werden, z.B. **Java**, **C#** und **Python**.

  * Visual Studio-Integration mit HDInsight für die Entwicklung, Verwaltung und Überwachung von C#-Topologien. Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

  * Unterstützt die Java-Schnittstelle **Trident**. Mit dieser Schnittstelle können Sie Storm-Topologien erstellen, die eine „exakt einmalige“ Verarbeitung von Nachrichten, „transaktionale“ Datenspeicherpersistenz sowie eine Reihe allgemeiner Datenstrom-Analysevorgänge unterstützen.

* Einfache zentrale Hoch- und Herunterskalierung des Clusters: Sie können Workerknoten hinzufügen oder entfernen, ohne dass sich dies auf aktive Storm-Topologien auswirkt.

* Integration in die folgenden Azure-Dienste:

    * Event Hubs
    * Virtuelles Netzwerk
    * SQL-Datenbank
    * Azure Storage
    * DocumentDB gespeichert.

  * Sicheres Kombinieren der Funktionen mehrerer HDInsight-Cluster mithilfe von Azure Virtual Network: Erstellen Sie analytische Pipelines, die HDInsight-, HBase- oder Hadoop-Cluster nutzen.

Eine Liste der Unternehmen, die Apache Storm für ihre Echtzeitanalyselösungen verwenden, finden Sie unter [Unternehmen, die Apache Storm verwenden](https://storm.apache.org/documentation/Powered-By.html).

Eine Einführung in Storm finden Sie unter [Erste Schritte mit Storm in HDInsight][gettingstarted].

### <a name="ease-of-creation"></a>Einfache Erstellung

Sie können einen neuen Storm in HDInsight-Cluster in wenigen Minuten bereitstellen. Geben Sie den Clusternamen, die Größe, das Administratorkonto und das Speicherkonto an. Azure erstellt den Cluster, einschließlich Beispieltopologien und Dashboard für die Webverwaltung.

> [!NOTE]
> Sie können Storm-Cluster auch über die [Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) oder [Azure PowerShell](/powershell/azureps-cmdlets-docs) bereitstellen.

Innerhalb von 15 Minuten nach Übermittlung der Anforderung verfügen Sie über einen neuen Storm-Cluster, der bereit ist für die erste Echtzeitanalysepipeline.

### <a name="ease-of-use"></a>Einfache Bedienung

* __Secure Shell-Konnektivität__: Sie können über das Internet per SSH auf die Hauptknoten Ihres HDInsight-Clusters zugreifen. Mit SSH haben Sie die Möglichkeit, Befehle direkt im Cluster auszuführen.

  Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webkonnektivität__: Für HDInsight-Cluster wird die Ambari-Webbenutzeroberfläche bereitgestellt. Mit der Ambari-Webbenutzeroberfläche können Sie Dienste in Ihrem Cluster leicht überwachen, konfigurieren und verwalten. Außerdem wird für Storm in HDInsight die Storm-Benutzeroberfläche bereitgestellt, mit der Sie die Ausführung von Storm-Topologien in Ihrem Browser überwachen und verwalten können.

  Weitere Informationen finden Sie unter [Verwalten von HDInsight mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md) und [Überwachen und Verwalten mit der Storm-Benutzeroberfläche](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Azure PowerShell und -CLI__: Sowohl Azure PowerShell als auch die Azure-CLI verfügen über Befehlszeilen-Hilfsprogramme, die Sie über Ihr Clientsystem verwenden können, um mit HDInsight und anderen Azure-Diensten zu arbeiten.

* __Visual Studio-Integration__: Die Data Lake Tools für Visual Studio enthalten Projektvorlagen zur Erstellung von C#-Storm-Topologien und Tools zum Überwachen von Storm in HDInsight. Sie können Ihre C#-Topologien in Visual Studio erstellen, bereitstellen, überwachen und verwalten.

  Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

* __Integration in andere Azure-Dienste__

  * Für die __Java__-Entwicklung nutzt Microsoft vorhandene Storm-Komponenten für die Integration mit anderen Azure-Diensten, soweit dies möglich ist. In einigen Fällen ist unter Umständen eine dienstspezifische Komponente oder Lösung erforderlich.

    * __Azure Data Lake Store__: Mit Java-basierten Topologien kann auf Data Lake Store zugegriffen werden, indem der Storm-HDFS-Bolt mit dem URI-Schema `adl://` verwendet wird. Ein Beispiel für die Verwendung des Storm-HDFS-Bolts finden Sie unter [Verwenden von Azure Data Lake-Speicher mit Apache Storm und HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-write-data-lake-store).

    * __Azure Storage__ (bei Verwendung als Speicher für HDInsight): Mit Java-basierten Topologien kann auf Azure Storage zugegriffen werden, indem der Storm-HDFS-Bolt mit dem URI-Schema `wasb://` verwendet wird.

    * __Azure Event Hubs__: Der Zugriff ist mit den EventHubSpout- und EventHubBolt-Komponenten möglich, die von Microsoft bereitgestellt werden. Diese Komponenten sind in Java geschrieben und werden als eigenständige JAR-Datei bereitgestellt.

    Weitere Informationen zur Entwicklung von Java-Lösungen finden Sie unter [Entwickeln von Java-basierten Topologien für eine einfache Anwendung zum Zählen von Wörtern mit Apache Storm und Maven auf HDInsight](hdinsight-storm-develop-java-topology.md).

  * Für die __C#__-Entwicklung können Sie normalerweise das .NET SDK für den Azure-Dienst verwenden. Es kann vorkommen, dass für das SDK Frameworks benötigt werden, die unter Linux nicht verfügbar sind (Hostbetriebssystem für HDInsight 3.4 und höher). In diesem Fall können Sie Java-Komponenten in Ihrer C#-Lösung verwenden.

    * Beispiele für die Arbeit mit __SQL DB__, __DocumentDB__, __EventHub__ und __HBase__ sind als Vorlagen in den Azure Data Lake-Tools für Visual Studio enthalten. Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * __Azure Event Hubs__: Ein Beispiel für die Verwendung von Java-Komponenten mit einer C#-Lösung finden Sie unter [Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md).

    Weitere Informationen zur Entwicklung von C#-Lösungen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="reliability"></a>Zuverlässigkeit

Mit Apache Storm ist garantiert, dass jede eingehende Nachricht auch dann vollständig verarbeitet wird, wenn die Datenanalyse über Hunderte von Knoten verteilt ist.

Der **Nimbus-Knoten** stellt dem Hadoop JobTracker ähnliche Funktionen zur Verfügung und weist über **Zookeeper** anderen Knoten im Cluster Aufgaben zu. Zookeeper-Knoten stellen Koordination für den Cluster bereit und erleichtern die Kommunikation zwischen Nimbus und dem **Supervisor** -Prozess der Workerknoten. Wenn ein Verarbeitungsknoten ausfällt, wird der Nimbus-Knoten darüber informiert, und die Aufgabe sowie zugehörige Daten werden einem anderen Knoten zugewiesen.

In der Standardkonfiguration verfügt Apache Storm über einen einzelnen Nimbus-Knoten. Storm in HDInsight betreibt zwei Nimbus-Knoten. Wenn der primäre Knoten ausfällt, wechselt der HDInsight-Cluster auf den sekundären Knoten, während der primäre Knoten wiederhergestellt wird.

![Diagramm mit Nimbus, Zookeeper und Supervisor](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Skalieren

Obwohl Sie die Anzahl der Knoten im Cluster bei der Erstellung angeben können, sollten Sie den Cluster je nach Workload vergrößern oder verkleinern. Alle HDInsight-Cluster bieten die Möglichkeit, die Anzahl der Knoten im Cluster sogar während der Verarbeitung von Daten zu ändern.

> [!NOTE]
> Um neue Knoten zu nutzen, die über eine Skalierung hinzugefügt wurden, müssen Sie Topologien neu ausrichten, die vor dem Vergrößern des Clusters gestartet wurden.

### <a name="support"></a>Support

Für Storm in HDInsight steht umfassender Support auf Unternehmensebene rund um die Uhr zur Verfügung. Storm in HDInsight bietet eine SLA von 99,9 %. Dies bedeutet, dass der Cluster garantiert mindestens 99,9% der Zeit über externe Konnektivität verfügt.

## <a name="common-use-cases-for-real-time-analytics"></a>Allgemeine Szenarien für Echtzeitanalysen

Hier finden Sie eine Liste der typischen Szenarien, in denen Apache Storm in HDInsight eingesetzt wird. Weitere Informationen zu realen Szenarien finden Sie auf der englischsprachigen [Seite mit Informationen zum Einsatz von Apache Storm in Unternehmen](https://storm.apache.org/documentation/Powered-By.html).

* Internet der Dinge (IoT, Internet of Things)
* Betrugserkennung
* Soziale Analysen
* Extraktions-, Umwandlungs- und Ladeprozesse (Extract, Transform and Load, ETL)
* Netzwerküberwachung
* Suche
* Mobile Engagement

## <a name="how-is-data-in-hdinsight-storm-processed"></a>Wie werden die Daten in HDInsight Storm verarbeitet?

Apache Storm verarbeitet **Topologien** anstelle der MapReduce-Aufträge, die Sie möglicherweise aus HDInsight oder Hadoop kennen. Ein Storm in HDInsight-Cluster enthält zwei Arten von Knoten: Hauptknoten, auf denen **Nimbus** ausgeführt wird, und Workerknoten, auf denen **Supervisor** ausgeführt wird.

* **Nimbus**: Ähnelt dem JobTracker in Hadoop und verteilt den Code über das Cluster, weist Aufgaben an virtuelle Computer zu und überwacht das System auf Fehler. HDInsight stellt zwei Nimbus-Knoten bereit, um einzelne Fehlerquellen für Storm in HDInsight auszuschließen.
* **Supervisor**: Die Supervisoren der einzelnen Workerknoten sind dafür verantwortlich, die **Workerprozesse** im Knoten zu starten und zu beenden.
* **Workerprozess**: Ein Workerprozess führt eine Teilmenge einer **Topologie** aus. Eine laufende Topologie ist über viele Arbeitsprozesse im gesamten Cluster verteilt.
* **Topologie**: Definiert einen Berechnungsgraphen zur Verarbeitung von **Datenströmen**. Im Gegensatz zu MapReduce-Aufträgen werden Topologien so lange ausgeführt, bis sie beendet werden.
* **Datenstrom**: Eine ungebundene Sammlung von **Tupeln**. Datenströme werden von **Spouts** und **Bolts** erzeugt und von **Bolts** konsumiert.
* **Tupel**: Eine benannte Liste dynamisch typisierter Werte.
* **Spout**: Konsumiert Daten aus einer Datenquelle und gibt einen oder mehrere **Datenströme** aus.

  > [!NOTE]
  > In vielen Fällen werden Daten aus einer Warteschlange gelesen, z.B. Kafka oder Azure Event Hubs. Die Warteschlange sorgt dafür, dass die Daten bei einem Ausfall beibehalten werden.

* **Bolt**: Konsumiert **Datenströme**, verarbeitet **Tupel** und kann **Datenströme** ausgeben. Bolts sind außerdem dafür verantwortlich, Daten in externe Speicher zu schreiben, z. B. eine Warteschlange, HDInsight, HBase, Blobs oder andere Datenspeicher.
* **Apache Thrift**: Ein Software-Framework für die Entwicklung skalierbarer, sprachübergreifender Dienste. Mit Thrift können Sie Dienste entwickeln, die zwischen C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk und anderen Sprachen funktionieren.

Weitere Informationen zu Storm-Komponenten finden Sie im [Storm-Tutorial][apachetutorial] auf „apache.org“.

## <a name="what-programming-languages-can-i-use"></a>Welche Programmiersprachen kann ich verwenden?

### <a name="c35"></a>C&#35;

Mit den Data Lake-Tools für Visual Studio können .NET-Entwickler eine Topologie in C# entwerfen und implementieren. Sie können auch Hybridtopologien erstellen, die Java- und C#-Komponenten verwenden.

Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="java"></a>Java

Bei den meisten Java-Beispielen handelt es sich entweder um reines Java oder um Trident. Trident ist eine Abstraktion auf einer hohen Ebene zur Erleichterung von Aufgaben wie z. B. Joins, Aggregation, Gruppierung und Filterung. Trident arbeitet jedoch mit Stapeln von Tupeln, wohingegen Datenströme in reinen Java-Lösungen tupelweise verarbeitet werden.

Weitere Informationen zu Trident finden Sie im [Trident Tutorial](https://storm.apache.org/documentation/Trident-tutorial.html) (in englischer Sprache) auf apache.org.

Beispiele für Java- und Trident-Topologien finden Sie in der [Liste der Storm-Beispieltopologien](hdinsight-storm-example-topology.md) oder den Storm-Starterbeispielen für Ihr HDInsight-Cluster.

Die Storm-Starterbeispiele befinden sich in Ihrem HDInsight-Cluster im Verzeichnis **/usr/hdp/current/storm-client/contrib/storm-starter**.

### <a name="python"></a>Python

Ein Beispiel für die Verwendung von Python-Komponenten finden Sie unter [Entwickeln von Apache Storm-Topologien mit Python in HDInsight](hdinsight-storm-develop-python-topology.md).

## <a name="what-are-some-common-development-patterns"></a>Welche Entwicklungsmuster werden häufig verwendet?

### <a name="guaranteed-message-processing"></a>Garantierte Nachrichtenverarbeitung

Mit Storm lassen sich verschiedene Stufen der garantierten Nachrichtenverarbeitung implementieren. Eine einfache Storm-Anwendung kann beispielsweise die Verarbeitung nach dem „At-Least-Once“-Prinzip garantieren, während Trident Nachrichten nach dem „Exactly-Once“-Prinzip garantiert.

Weitere Informationen finden Sie unter [Guarantees on Data Processing](https://storm.apache.org/about/guarantees-data-processing.html) (in englischer Sprache) auf apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Eine übliche Methode besteht darin, ein Eingangstupel zu lesen, mindestens ein Tupel auszugeben und das Eingangstupel am Ende der execute-Methode dann sofort zu bestätigen. Storm stellt die [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html)-Schnittstelle für die Automatisierung dieses Musters bereit.

### <a name="joins"></a>Joins

Die Vorgehensweise beim Verknüpfen von Datenströmen variiert zwischen Anwendungen. Sie können z. B. alle Tupel aus mehreren Datenströmen in einem neuen Datenstrom zusammenführen, oder Sie führen nur Stapel von Tupeln für ein bestimmtes Intervall zusammen. Die Zusammenführung erfolgt in beiden Fällen über [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), um zu definieren, wie Tupel an Bolts weitergeleitet werden.

Das folgende Java-Beispiel verwendet fieldsGrouping, um Tupel von den Komponenten "1", "2" und "3" an den **MyJoiner** -Bolt weiterzuleiten.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Batchverarbeitung

Storm stellt einen internen Timer-Mechanismus bereit, der auch als „Tick-Tupel“ bezeichnet wird. Er kann verwendet werden, um alle X Sekunden einen Batch auszugeben.

Ein Beispiel für die Verwendung eines Tick-Tupels für eine C#-Komponente finden Sie unter [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Falls Sie Trident verwenden, erfolgt die Verarbeitung immer als Batches von Tupeln.

### <a name="caching"></a>Caching

Speicherinterne Zwischenspeicherung wird oft verwendet, um die Verarbeitung zu beschleunigen, da häufig verwendete Elemente im Speicher erhalten bleiben. Da eine Topologie auf mehrere Knoten und mehrere Prozesse auf jedem Knoten verteilt ist, sollten Sie erwägen, [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) zu verwenden. Mit `fieldsGrouping` wird sichergestellt, dass Tupel mit den Feldern, die für die Cachesuche verwendet werden, immer an denselben Prozess weitergeleitet werden. Mit dieser Gruppierungsfunktion wird die prozessübergreifende Duplizierung von Cacheeinträgen verhindert.

### <a name="streaming-top-n"></a>Streaming Top N

Wenn Ihre Topologie darauf abzielt, einen „Top N“-Wert zu ermitteln, sollten Sie den „Top N“-Wert parallel berechnen und die Ausgabe dieser Berechnungen in einem globalen Wert zusammenführen. Verwenden Sie hierfür [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), um Daten für die parallele Verarbeitung nach Feldern weiterzuleiten. Leiten Sie sie anschließend an einen Bolt weiter, der den „Top N“-Wert global ermittelt.

Ein Beispiel für die Berechnung eines „Top N“-Werts finden Sie im Beispiel [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java).

## <a name="what-type-of-logging-does-storm-use"></a>Welche Art der Protokollierung verwendet Storm?

Storm verwendet Apache Log4j für die Protokollierung von Informationen. Standardmäßig wird eine große Datenmenge protokolliert, was die Durchsicht der Informationen erschweren kann. Sie können eine Protokollierungskonfigurationsdatei als Teil Ihrer Storm-Topologie hinzufügen, um das Protokollierungsverhalten zu steuern.

Eine Beispieltopologie, die das Konfigurieren der Protokollierung veranschaulicht, finden Sie im [Java-basierten WordCount](hdinsight-storm-develop-java-topology.md) -Beispiel für Storm in HDInsight.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Lösungen für Echtzeitanalysen mit Apache Storm in HDInsight:

* [Erste Schritte mit Storm in HDInsight][gettingstarted]
* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

