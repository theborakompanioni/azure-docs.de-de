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
ms.date: 03/31/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 73e524bf9e7d51cee841a3c5599ab33aa31cbc37
ms.lasthandoff: 04/06/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Einführung in Apache Storm in HDInsight: Echtzeitanalysen für Hadoop

Mit Apache Storm in Azure HDInsight können Sie verteilte Echtzeitanalyselösungen erstellen.

Storm ist ein verteiltes, fehlertolerantes Open Source-Berechnungssystem. Mithilfe von Storm können Sie Daten in Echtzeit mit Hadoop verarbeiten. Storm-Lösungen sind außerdem in der Lage, die Verarbeitung von Daten zu garantieren und Daten erneut abzuspielen, die beim ersten Versuch nicht erfolgreich verarbeitet wurden.

## <a name="how-does-storm-work"></a>Funktionsweise von Storm

Anstelle der MapReduce-Aufträge, die Sie möglicherweise aus HDInsight oder Hadoop kennen, führt Storm Topologien aus. Topologien setzen sich aus mehreren Komponenten zusammen, die in einem gerichteten azyklischen Graph (DAG) angeordnet sind. Das folgende Diagramm veranschaulicht den Datenfluss zwischen Komponenten in einer einfachen Wortzählungstopologie:

![Beispielanordnung von Komponenten in einer Storm-Topologie](./media/hdinsight-storm-overview/wordcount-topology.png)

* Spout-Komponenten bringen Daten in eine Topologie ein. Sie geben mindestens einen Datenstrom in die Topologie aus.

* Bolt-Komponenten nutzen Datenströme, die von Spouts oder von anderen Bolts ausgegeben werden. Bolts können optional auch neue Datenströme in die Topologie ausgeben. Außerdem sind Bolts für das Schreiben von Daten in den permanenten Speicher (wie etwa HDFS oder HBase) zuständig.

## <a name="why-use-storm-on-hdinsight"></a>Vorteile von Storm in HDInsight

Storm in HDInsight bietet folgende zentrale Vorteile:

* Ausführung als verwalteter Dienst mit SLA für eine Betriebszeit von 99,9 Prozent.

* Einfache Anpassung durch Ausführung von Skripts für einen Cluster während oder nach der Erstellung. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

* Verwendung verschiedener Sprachen. Sie können die Storm-Komponenten in der Sprache Ihrer Wahl schreiben (etwa in Java, C# oder Python).

    * Zusammenarbeit von Visual Studio und HDInsight für die Entwicklung, Verwaltung und Überwachung von C#-Topologien. Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Unterstützung der Java-Schnittstelle „Trident“. Sie können Storm-Topologien erstellen, die eine exakt einmalige Verarbeitung von Nachrichten, transaktionale Datenspeicherpersistenz sowie eine Reihe allgemeiner Stream Analytics-Vorgänge unterstützen.

*  Problemlose Clusterskalierung. Sie können Workerknoten ohne Auswirkungen auf aktive Storm-Topologien hinzufügen oder entfernen.

* Integration in folgende Azure-Dienste:

    * Azure Event Hubs

    * Azure Virtual Network

    * Azure SQL-Datenbank

    * Azure Storage

    * Azure DocumentDB

* Sichere Kombination der Funktionen mehrerer HDInsight-Cluster durch Verwendung von Virtual Network. Sie können analytische Pipelines erstellen, die HDInsight-, HBase- oder Hadoop-Cluster nutzen.

Eine Liste mit Unternehmen, die Storm für ihre Echtzeitanalyselösungen verwenden, finden Sie [hier](https://storm.apache.org/documentation/Powered-By.html).

Eine Einführung in Storm finden Sie unter [Erste Schritte mit Storm in HDInsight][gettingstarted].

## <a name="ease-of-creation"></a>Einfache Erstellung

Ein neuer Storm-Cluster in HDInsight kann innerhalb weniger Minuten bereitgestellt werden. Weitere Informationen zum Erstellen eines Storm-Clusters finden Sie unter [Erste Schritte mit Storm-Starter-Beispielen für die Big Data-Analyse in Linux-basiertem HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Einfache Bedienung

* __SSH-Konnektivität (Secure Shell)__: Sie können per SSH über das Internet auf die Hauptknoten Ihres HDInsight-Clusters zugreifen. Mit SSH können Sie Befehle direkt in Ihrem Cluster ausführen.

  Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webkonnektivität__: Für HDInsight-Cluster wird die Ambari-Webbenutzeroberfläche bereitgestellt. Über die Ambari-Webbenutzeroberfläche können Sie Dienste in Ihrem Cluster komfortabel überwachen, konfigurieren und verwalten. Für Storm in HDInsight steht auch die Storm-Benutzeroberfläche zur Verfügung. Mit der Storm-Benutzeroberfläche können Sie die Ausführung von Storm-Topologien über Ihren Browser überwachen und verwalten.

  Weitere Informationen finden Sie unter [Verwalten von HDInsight mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md) und [Überwachen und Verwalten mit der Storm-Benutzeroberfläche](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Azure PowerShell und Azure-Befehlszeilenschnittstelle__: Azure PowerShell und die Azure-Befehlszeilenschnittstelle bieten Befehlszeilen-Hilfsprogramme, die Sie auf Ihrem Clientsystem für die Arbeit mit HDInsight und anderen Azure-Diensten verwenden können.

* __Visual Studio-Integration__: Azure Data Lake Tools für Visual Studio enthält Projektvorlagen für die Erstellung von C#-Storm-Topologien unter Verwendung des SCP.NET-Frameworks. Data Lake Tools bietet zudem Tools zum Bereitstellen, Überwachen und Verwalten von Lösungen mit Storm in HDInsight.

  Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Integration in andere Azure-Dienste

* __Azure Data Lake Store__: Ein Beispiel für die Verwendung von Data Lake Store mit Storm finden Sie unter [Verwenden von Azure Data Lake Store mit Apache Storm und HDInsight (Java)](hdinsight-storm-write-data-lake-store.md).

* __Event Hubs__: Ein Beispiel für die Verwendung von Event Hubs mit Storm finden Sie in den folgenden Dokumenten:

    * [Verwenden von Maven zum Entwickeln einer Java-basierten Wortzählungstopologie für Storm in HDInsight](hdinsight-storm-develop-java-topology.md)

    * [Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* __SQL-Datenbank__, __DocumentDB__, __Event Hubs__ und __HBase__: Beispielvorlagen sind in Data Lake Tools für Visual Studio enthalten. Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Zuverlässigkeit

Mit Storm ist garantiert, dass jede eingehende Nachricht auch dann vollständig verarbeitet wird, wenn die Datenanalyse über Hunderte von Knoten verteilt ist.

Der Nimbus-Knoten bietet ähnliche Funktionen wie Hadoop JobTracker und weist anderen Knoten im Cluster über Zookeeper Aufgaben zu. Zookeeper-Knoten kümmern sich um die Koordination eines Clusters und erleichtern die Kommunikation zwischen Nimbus und dem Supervisor-Prozess auf den Workerknoten. Wenn ein Verarbeitungsknoten ausfällt, wird der Nimbus-Knoten darüber informiert, und die Aufgabe sowie zugehörige Daten werden einem anderen Knoten zugewiesen.

In der Standardkonfiguration verfügt Storm nur über einen einzelnen Nimbus-Knoten. Storm in HDInsight betreibt zwei Nimbus-Knoten. Wenn der primäre Knoten ausfällt, wechselt der HDInsight-Cluster auf den sekundären Knoten, während der primäre Knoten wiederhergestellt wird. Das folgende Diagramm veranschaulicht die Konfiguration des Aufgabenablaufs für Storm in HDInsight:

![Diagramm mit Nimbus, Zookeeper und Supervisor](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Skalieren

Die Anzahl von Knoten im Cluster kann zwar bei der Erstellung angegeben werden, es kann jedoch vorkommen, dass Sie die Clustergröße an die Workload anpassen möchten. Daher besteht bei allen HDInsight-Clustern die Möglichkeit, die Anzahl von Knoten zu ändern – und das sogar während der Verarbeitung von Daten.

> [!NOTE]
> Um neue Knoten zu nutzen, die über eine Skalierung hinzugefügt wurden, müssen Sie Topologien neu ausrichten, die vor dem Vergrößern des Clusters gestartet wurden.

## <a name="support"></a>Support

Storm in HDInsight bietet durchgehend umfassenden professionellen Support. Außerdem gilt für Storm in HDInsight eine SLA von 99,9 Prozent. Für einen Cluster wird also eine externe Konnektivität von mindestens 99,9 Prozent garantiert.

Weitere Informationen finden Sie unter [Azure-Support](https://azure.microsoft.com/support/options/).

## <a name="common-use-cases"></a>Gängige Anwendungsfälle

Im Anschluss finden Sie eine Liste mit typischen Verwendungsszenarien für Storm in HDInsight. 

* Internet der Dinge (IoT, Internet of Things)
* Betrugserkennung
* Soziale Analysen
* Extrahieren, Transformieren und Laden (ETL)
* Netzwerküberwachung
* Suche
* Mobile Engagement

Informationen zu Praxisszenarien finden Sie [hier](https://storm.apache.org/documentation/Powered-By.html).

## <a name="development"></a>Entwicklung

Mit Data Lake Tools für Visual Studio können .NET-Entwickler Topologien in C# entwerfen und implementieren. Sie können auch Hybridtopologien erstellen, die Java- und C#-Komponenten verwenden.

Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Sie können auch Java-Lösungen entwickeln und dabei Ihre bevorzugte IDE verwenden. Weitere Informationen finden Sie unter [Verwenden von Maven zum Entwickeln einer Java-basierten Wortzählungstopologie für Storm in HDInsight](hdinsight-storm-develop-java-topology.md).

Storm-Komponenten können auch unter Verwendung von Python entwickelt werden. Weitere Informationen finden Sie unter [Entwickeln von Apache Storm-Topologien mit Python in HDInsight](hdinsight-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Gängige Entwicklungsmuster

### <a name="guaranteed-message-processing"></a>Garantierte Nachrichtenverarbeitung

Mit Storm lassen sich verschiedene Stufen der garantierten Nachrichtenverarbeitung implementieren. Eine einfache Storm-Anwendung kann beispielsweise die Verarbeitung nach dem „At-Least-Once“-Prinzip garantieren, während Trident Nachrichten nach dem „Exactly-Once“-Prinzip garantiert.

Weitere Informationen finden Sie unter [Guarantees on Data Processing](https://storm.apache.org/about/guarantees-data-processing.html) (in englischer Sprache) auf apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Eine übliche Methode besteht darin, ein Eingangstupel zu lesen, mindestens ein Tupel auszugeben und das Eingangstupel am Ende der execute-Methode dann sofort zu bestätigen. Storm stellt die [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html)-Schnittstelle für die Automatisierung dieses Musters bereit.

### <a name="joins"></a>Joins

Die Vorgehensweise beim Verknüpfen von Datenströmen variiert zwischen Anwendungen. So können Sie beispielsweise alle Tupel aus mehreren Datenströmen in einem neuen Datenstrom zusammenführen oder nur Tupel-Batches für ein bestimmtes Intervall zusammenführen. Die Zusammenführung erfolgt in beiden Fällen über [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), um zu definieren, wie Tupel an Bolts weitergeleitet werden.

Das folgende Java-Beispiel verwendet „fieldsGrouping“, um Tupel aus den Komponenten „1“, „2“ und „3“ an den MyJoiner-Bolt weiterzuleiten:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Batches

Storm stellt einen internen Timer-Mechanismus bereit, der auch als „Tick-Tupel“ bezeichnet wird. Er kann verwendet werden, um alle x Sekunden einen Batch auszugeben.

Ein Beispiel für die Verwendung eines Tick-Tupels für eine C#-Komponente finden Sie unter [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Trident basiert auf der Verarbeitung von Tupel-Batches.

### <a name="caches"></a>Caches

Speicherinterne Zwischenspeicherung wird oft verwendet, um die Verarbeitung zu beschleunigen, da häufig verwendete Elemente im Speicher erhalten bleiben. Da eine Topologie auf mehrere Knoten und mehrere Prozesse auf jedem Knoten verteilt ist, sollten Sie erwägen, [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) zu verwenden. Verwenden Sie `fieldsGrouping`, um sicherzustellen, dass Tupel mit den Feldern, die für die Cachesuche verwendet werden, immer an den gleichen Prozess weitergeleitet werden. Mit dieser Gruppierungsfunktion wird die prozessübergreifende Duplizierung von Cacheeinträgen verhindert.

### <a name="stream-top-n"></a>Datenstrom „Top N“

Wenn Ihre Topologie auf die Berechnung eines Top N-Werts angewiesen ist, berechnen Sie diesen Wert parallel. Führen Sie anschließend die Ausgabe der entsprechenden Berechnungen zu einem globalen Wert zusammen. Verwenden Sie hierfür [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), um Daten nach Feld für die parallele Verarbeitung weiterzuleiten, und leiten Sie sie anschließend an einen Bolt weiter, der den Top N-Wert global bestimmt.

Ein Beispiel für die Berechnung eines Top N-Werts finden Sie im Beispiel [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java).

## <a name="logging"></a>Protokollierung

Storm verwendet Apache Log4j für die Protokollierung von Informationen. Standardmäßig wird eine große Datenmenge protokolliert, was die Durchsicht der Informationen erschweren kann. Sie können eine Protokollierungskonfigurationsdatei als Teil Ihrer Storm-Topologie hinzufügen, um das Protokollierungsverhalten zu steuern.

Eine Beispieltopologie, die das Konfigurieren der Protokollierung veranschaulicht, finden Sie im [Java-basierten WordCount](hdinsight-storm-develop-java-topology.md) -Beispiel für Storm in HDInsight.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Lösungen für Echtzeitanalysen mit Storm in HDInsight:

* [Erste Schritte mit Storm-Starter-Beispielen für die Big Data-Analyse in Linux-basiertem HDInsight][gettingstarted]
* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

