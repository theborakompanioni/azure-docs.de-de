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
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 1155f534869f240c1567fa8b791dbcb7750c8a40
ms.lasthandoff: 03/31/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Einführung in Apache Storm in HDInsight: Echtzeitanalysen für Hadoop

Mit Apache Storm in HDInsight können Sie verteilte Echtzeitanalyselösungen in Azure erstellen.

Apache Storm ist ein verteiltes, fehlertolerantes Open-Source-Berechnungssystem für die Verarbeitung von Daten in Echtzeit mit Hadoop. Storm-Lösungen sind außerdem in der Lage, die Verarbeitung von Daten zu garantieren und Daten erneut abzuspielen, die beim ersten Versuch nicht erfolgreich verarbeitet wurden.

## <a name="how-does-storm-work"></a>Funktionsweise von Storm

Apache Storm verarbeitet **Topologien** anstelle der MapReduce-Aufträge, die Sie möglicherweise aus HDInsight oder Hadoop kennen. Topologien setzen sich aus mehreren Komponenten zusammen, die in einem gerichteten azyklischen Graph (DAG) angeordnet sind. Das folgende Diagramm veranschaulicht den Datenfluss zwischen Komponenten in einer einfachen Wortzählungstopologie:

![Beispielanordnung von Komponenten in einer Storm-Topologie](./media/hdinsight-storm-overview/wordcount-topology.png)

* Komponenten vom Typ __Spout__ bringen Daten in eine Topologie ein. Sie geben mindestens einen Datenstrom in die Topologie aus.

* Komponenten vom Typ __Bolt__ nutzen Datenströme, die von Spouts oder von anderen Bolts ausgegeben werden. Bolts können optional auch neue Datenströme in die Topologie ausgeben. Außerdem sind Bolts für das Schreiben von Daten in den permanenten Speicher (wie etwa HDFS oder HBase) zuständig.

## <a name="why-use-storm-on-hdinsight"></a>Vorteile von Storm in HDInsight

Apache Storm in HDInsight bietet die folgenden Hauptvorteile:

* Ausführung als verwalteter Dienst mit einer SLA von 99,9% Betriebszeit.

* Einfache Anpassung durch Ausführung von Skripts für den Cluster während oder nach der Erstellung. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

* Nutzung der Sprache Ihrer Wahl: Storm-Komponenten können in vielen verschiedenen Sprachen geschrieben werden. Hierzu zählen beispielsweise **Java**, **C#** und **Python**.

    * Visual Studio-Integration mit HDInsight für die Entwicklung, Verwaltung und Überwachung von C#-Topologien. Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Unterstützt die Java-Schnittstelle **Trident**. Über diese Schnittstelle können Sie Storm-Topologien erstellen, die eine exakt einmalige Verarbeitung von Nachrichten, transaktionale Datenspeicherpersistenz sowie eine Reihe allgemeiner Stream Analytics-Vorgänge unterstützen.

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

## <a name="ease-of-creation"></a>Einfache Erstellung

Sie können einen neuen Storm in HDInsight-Cluster in wenigen Minuten bereitstellen. Weitere Informationen zum Erstellen eines Storm-Clusters finden Sie unter [Erste Schritte mit Storm-Starter-Beispielen für die Big Data-Analyse in Linux-basiertem HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Einfache Bedienung

* __Secure Shell-Konnektivität__: Sie können über das Internet per SSH auf die Hauptknoten Ihres HDInsight-Clusters zugreifen. Mit SSH haben Sie die Möglichkeit, Befehle direkt im Cluster auszuführen.

  Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webkonnektivität__: Für HDInsight-Cluster wird die Ambari-Webbenutzeroberfläche bereitgestellt. Mit der Ambari-Webbenutzeroberfläche können Sie Dienste in Ihrem Cluster leicht überwachen, konfigurieren und verwalten. Außerdem wird für Storm in HDInsight die Storm-Benutzeroberfläche bereitgestellt, mit der Sie die Ausführung von Storm-Topologien in Ihrem Browser überwachen und verwalten können.

  Weitere Informationen finden Sie unter [Verwalten von HDInsight mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md) und [Überwachen und Verwalten mit der Storm-Benutzeroberfläche](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Azure PowerShell und -CLI__: Sowohl Azure PowerShell als auch die Azure-CLI verfügen über Befehlszeilen-Hilfsprogramme, die Sie über Ihr Clientsystem verwenden können, um mit HDInsight und anderen Azure-Diensten zu arbeiten.

* __Visual Studio-Integration__: Data Lake Tools for Visual Studio enthält Projektvorlagen für die Erstellung von C#-Storm-Topologien unter Verwendung des SCP.Net-Frameworks. Data Lake Tools bietet zudem Tools zum Bereitstellen, Überwachen und Verwalten von Lösungen mit Storm in HDInsight.

  Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Integration in andere Azure-Dienste

* __Azure Data Lake Store__: Ein Beispiel für die Verwendung von Data Lake Store mit Storm finden Sie unter [Verwenden von Azure Data Lake Store mit Apache Storm und HDInsight (Java)](hdinsight-storm-write-data-lake-store.md).

* __Azure Event Hubs__: Ein Beispiel für die Verwendung von Azure Event Hubs mit Storm finden Sie in den folgenden Dokumenten:

    * [Verwenden von Maven zum Entwickeln einer Java-basierten Wortzählungstopologie für Storm in HDInsight](hdinsight-storm-develop-java-topology.md)

    * [Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* Beispiele für die Arbeit mit __SQL DB__, __DocumentDB__, __EventHub__ und __HBase__ sind als Vorlagen in den Azure Data Lake-Tools für Visual Studio enthalten. Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Zuverlässigkeit

Mit Apache Storm ist garantiert, dass jede eingehende Nachricht auch dann vollständig verarbeitet wird, wenn die Datenanalyse über Hunderte von Knoten verteilt ist.

Der **Nimbus-Knoten** stellt dem Hadoop JobTracker ähnliche Funktionen zur Verfügung und weist über **Zookeeper** anderen Knoten im Cluster Aufgaben zu. Zookeeper-Knoten stellen Koordination für den Cluster bereit und erleichtern die Kommunikation zwischen Nimbus und dem **Supervisor** -Prozess der Workerknoten. Wenn ein Verarbeitungsknoten ausfällt, wird der Nimbus-Knoten darüber informiert, und die Aufgabe sowie zugehörige Daten werden einem anderen Knoten zugewiesen.

In der Standardkonfiguration verfügt Apache Storm über einen einzelnen Nimbus-Knoten. Storm in HDInsight betreibt zwei Nimbus-Knoten. Wenn der primäre Knoten ausfällt, wechselt der HDInsight-Cluster auf den sekundären Knoten, während der primäre Knoten wiederhergestellt wird.

![Diagramm mit Nimbus, Zookeeper und Supervisor](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Skalieren

Obwohl Sie die Anzahl der Knoten im Cluster bei der Erstellung angeben können, sollten Sie den Cluster je nach Workload vergrößern oder verkleinern. Alle HDInsight-Cluster bieten die Möglichkeit, die Anzahl der Knoten im Cluster sogar während der Verarbeitung von Daten zu ändern.

> [!NOTE]
> Um neue Knoten zu nutzen, die über eine Skalierung hinzugefügt wurden, müssen Sie Topologien neu ausrichten, die vor dem Vergrößern des Clusters gestartet wurden.

## <a name="support"></a>Support

Für Storm in HDInsight steht umfassender Support auf Unternehmensebene rund um die Uhr zur Verfügung. Storm in HDInsight bietet eine SLA von 99,9 %. Dies bedeutet, dass der Cluster garantiert mindestens 99,9% der Zeit über externe Konnektivität verfügt.

Weitere Informationen finden Sie unter [Azure-Support](https://azure.microsoft.com/support/options/).

## <a name="common-use-cases"></a>Gängige Anwendungsfälle

Hier finden Sie eine Liste der typischen Szenarien, in denen Apache Storm in HDInsight eingesetzt wird. 

* Internet der Dinge (IoT, Internet of Things)
* Betrugserkennung
* Soziale Analysen
* Extraktions-, Umwandlungs- und Ladeprozesse (Extract, Transform and Load, ETL)
* Netzwerküberwachung
* Suche
* Mobile Engagement

Informationen zu Praxisszenarien finden Sie [hier](https://storm.apache.org/documentation/Powered-By.html).

## <a name="development"></a>Entwicklung

Mit __Data Lake Tools for Visual Studio__ können .NET-Entwickler eine Topologie in __C#__ entwerfen und implementieren. Sie können auch Hybridtopologien erstellen, die Java- und C#-Komponenten verwenden.

Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Sie können auch __Java-Lösungen__ entwickeln und dabei Ihre bevorzugte IDE verwenden. Weitere Informationen finden Sie unter [Verwenden von Maven zum Entwickeln einer Java-basierten Wortzählungstopologie für Storm in HDInsight](hdinsight-storm-develop-java-topology.md).

Storm-Komponenten können auch unter Verwendung von Python entwickelt werden. Weitere Informationen finden Sie unter [Entwickeln von Apache Storm-Topologien mit Python in HDInsight](hdinsight-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Gängige Entwicklungsmuster

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

Wenn Ihre Topologie auf die Berechnung eines Top N-Werts angewiesen ist, empfiehlt es sich, diesen Wert parallel zu berechnen. Führen Sie anschließend die Ausgabe der entsprechenden Berechnungen zu einem globalen Wert zusammen. Verwenden Sie hierfür [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), um Daten nach Feld für die parallele Verarbeitung weiterzuleiten, und leiten Sie sie anschließend an einen Bolt weiter, der den Top N-Wert global bestimmt.

Ein Beispiel für die Berechnung eines „Top N“-Werts finden Sie im Beispiel [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java).

## <a name="logging"></a>Protokollierung

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

