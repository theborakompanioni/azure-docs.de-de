---
title: "Was ist Apache Storm – Azure HDInsight | Microsoft-Dokumentation"
description: "Mit Apache Storm können Sie Datenströme in Echtzeit verarbeiten. Mit Azure HDInsight können Sie auf einfache Weise Storm-Cluster in der Azure-Cloud erstellen. Mit Visual Studio können Sie Storm-Lösungen mithilfe von C# erstellen und dann in Ihren HDInsight Storm-Clustern bereitstellen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "Apache Storm-Anwendungsfälle, Storm-Cluster, was ist Apache Storm"
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 073672f1223313938baedee027072cb96062294b
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Was ist Apache Storm in Azure HDInsight?

[Apache Storm](http://storm.apache.org/) ist ein verteiltes, fehlertolerantes Open Source-Berechnungssystem. Mithilfe von Storm können Sie Datenströme in Echtzeit mit Hadoop verarbeiten. Storm-Lösungen sind außerdem in der Lage, die Verarbeitung von Daten zu garantieren und Daten erneut abzuspielen, die beim ersten Versuch nicht erfolgreich verarbeitet wurden.

Storm in HDInsight bietet folgende zentrale Vorteile:

* Ausführung als verwalteter Dienst mit SLA für eine Betriebszeit von 99,9 Prozent.

* Einfache Anpassung durch Ausführung von Skripts für einen Storm-Cluster während oder nach der Erstellung. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

* Verwendung verschiedener Sprachen. Sie können die Storm-Komponenten in der Sprache Ihrer Wahl schreiben (etwa in Java, C# oder Python).

    * Zusammenarbeit von Visual Studio und HDInsight für die Entwicklung, Verwaltung und Überwachung von C#-Topologien. Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Unterstützung der Java-Schnittstelle „Trident“. Sie können Storm-Topologien erstellen, die eine exakt einmalige Verarbeitung von Nachrichten, transaktionale Datenspeicherpersistenz sowie eine Reihe allgemeiner Stream Analytics-Vorgänge unterstützen.

*  Sie können Storm-Cluster leicht hoch- oder herunterskalieren. Sie können Workerknoten ohne Auswirkungen auf aktive Storm-Topologien hinzufügen oder entfernen.

* Integration in folgende Azure-Dienste:

    * Azure Event Hubs

    * Azure Virtual Network

    * Azure SQL-Datenbank

    * Azure Storage

    * Azure Cosmos DB

* Sichere Kombination der Funktionen mehrerer HDInsight-Cluster durch Verwendung von Virtual Network. Sie können analytische Pipelines erstellen, die Storm-, Kafka-, Spark-, HBase- oder Hadoop-Cluster nutzen.

Eine Liste der Unternehmen, die Apache Storm für ihre Echtzeitanalyselösungen verwenden, finden Sie unter [Unternehmen, die Apache Storm verwenden](https://storm.apache.org/documentation/Powered-By.html).

Eine Einführung in Storm finden Sie unter [Erste Schritte mit Storm in HDInsight][gettingstarted].

## <a name="how-does-storm-work"></a>Funktionsweise von Storm

Anstelle der MapReduce-Aufträge, die Sie möglicherweise kennen, führt Storm Topologien aus. Storm-Topologien setzen sich aus mehreren Komponenten zusammen, die in einem gerichteten azyklischen Graph (DAG) angeordnet sind. Daten werden zwischen den Komponenten im Diagramm übertragen. Jede Komponente nutzt einen oder mehrere Datenströme und kann optional einen oder mehrere Datenströme ausgeben kann. Das folgende Diagramm veranschaulicht den Datenfluss zwischen Komponenten in einer einfachen Wortzählungstopologie:

![Beispielanordnung von Komponenten in einer Storm-Topologie](./media/hdinsight-storm-overview/example-apache-storm-topology-diagram.png)

* Spout-Komponenten bringen Daten in eine Topologie ein. Sie geben mindestens einen Datenstrom in die Topologie aus.

* Bolt-Komponenten nutzen Datenströme, die von Spouts oder von anderen Bolts ausgegeben werden. Bolts können optional auch Datenströme in die Topologie ausgeben. Außerdem sind Bolts für das Schreiben von Daten in externe Dienste oder externen Speicher (wie etwa HDFS, Kafka oder HBase) zuständig.

## <a name="ease-of-creation"></a>Einfache Erstellung

Ein neuer Storm-Cluster in HDInsight kann innerhalb weniger Minuten bereitgestellt werden. Weitere Informationen zum Erstellen eines Storm-Clusters finden Sie unter [Erste Schritte mit Storm-Starter-Beispielen für die Big Data-Analyse in Linux-basiertem HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Einfache Bedienung

* __SSH-Konnektivität (Secure Shell)__: Sie können per SSH über das Internet auf die Hauptknoten Ihres Storm-Clusters zugreifen. Mit SSH können Sie Befehle direkt in Ihrem Cluster ausführen.

  Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webkonnektivität__: Für alle HDInsight-Cluster wird die Ambari-Webbenutzeroberfläche bereitgestellt. Über die Ambari-Webbenutzeroberfläche können Sie Dienste in Ihrem Cluster komfortabel überwachen, konfigurieren und verwalten. Storm-Cluster bieten auch die Storm-Benutzeroberfläche. Mit der Storm-Benutzeroberfläche können Sie die Ausführung von Storm-Topologien über Ihren Browser überwachen und verwalten.

  Weitere Informationen finden Sie in den Dokumenten [Verwalten von HDInsight mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md) und [Überwachen und Verwalten mit der Storm-Benutzeroberfläche](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Azure PowerShell und Azure-Befehlszeilenschnittstelle__: Azure PowerShell und die Azure-Befehlszeilenschnittstelle bieten Befehlszeilen-Hilfsprogramme, die Sie auf Ihrem Clientsystem für die Arbeit mit HDInsight und anderen Azure-Diensten verwenden können.

* __Visual Studio-Integration__: Azure Data Lake Tools für Visual Studio enthält Projektvorlagen für die Erstellung von C#-Storm-Topologien unter Verwendung des SCP.NET-Frameworks. Data Lake Tools bietet zudem Tools zum Bereitstellen, Überwachen und Verwalten von Lösungen mit Storm in HDInsight.

  Weitere Informationen finden Sie unter [Entwickeln von C#-Storm-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Integration in andere Azure-Dienste

* __Azure Data Lake Store__: Ein Beispiel für die Verwendung von Data Lake Store in einem Storm-Cluster finden Sie unter [Verwenden von Azure Data Lake Store mit Apache Storm und HDInsight](hdinsight-storm-write-data-lake-store.md).

* __Event Hubs__: Ein Beispiel für die Verwendung von Event Hubs in einem Storm-Cluster finden Sie in den folgenden Dokumenten:

    * [Verwenden von Maven zum Entwickeln einer Java-basierten Wortzählungstopologie für Storm in HDInsight](hdinsight-storm-develop-java-topology.md)

    * [Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* __SQL-Datenbank__, __Cosmos DB__, __Event Hubs__ und __HBase__: Beispielvorlagen sind in Data Lake Tools für Visual Studio enthalten. Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Zuverlässigkeit

Mit Apache Storm ist garantiert, dass jede eingehende Nachricht auch dann vollständig verarbeitet wird, wenn die Datenanalyse über Hunderte von Knoten verteilt ist.

Der Nimbus-Knoten bietet ähnliche Funktionen wie Hadoop JobTracker und weist anderen Knoten im Cluster über Zookeeper Aufgaben zu. Zookeeper-Knoten kümmern sich um die Koordination eines Clusters und erleichtern die Kommunikation zwischen Nimbus und dem Supervisor-Prozess auf den Workerknoten. Wenn ein Verarbeitungsknoten ausfällt, wird der Nimbus-Knoten darüber informiert, und die Aufgabe sowie zugehörige Daten werden einem anderen Knoten zugewiesen.

In der Standardkonfiguration verfügen Apache Storm-Cluster über einen einzelnen Nimbus-Knoten. Storm in HDInsight bietet zwei Nimbus-Knoten. Wenn der primäre Knoten ausfällt, wechselt der Storm-Cluster auf den sekundären Knoten, während der primäre Knoten wiederhergestellt wird. Das folgende Diagramm veranschaulicht die Konfiguration des Aufgabenablaufs für Storm in HDInsight:

![Diagramm mit Nimbus, Zookeeper und Supervisor](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Skalieren

HDInsight-Cluster können dynamisch skaliert werden, indem Workerknoten hinzugefügt oder entfernt werden. Dieser Vorgang kann während der Verarbeitung von Daten ausgeführt werden.

> [!IMPORTANT]
> Um neue Knoten zu nutzen, die über eine Skalierung hinzugefügt wurden, müssen Sie Storm-Topologien neu ausrichten, die vor dem Vergrößern des Clusters gestartet wurden.

## <a name="support"></a>Support

Storm in HDInsight bietet durchgehend umfassenden professionellen Support. Außerdem gilt für Storm in HDInsight eine SLA von 99,9 Prozent. Für einen Storm-Cluster wird also eine externe Konnektivität für mindestens 99,9 Prozent der Zeit garantiert.

Weitere Informationen finden Sie unter [Azure-Support](https://azure.microsoft.com/support/options/).

## <a name="apache-storm-use-cases"></a>Anwendungsfälle für Apache Storm

Im Folgenden finden Sie eine Liste mit typischen Verwendungsszenarien für Storm in HDInsight:

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

Mit Apache Storm lassen sich verschiedene Stufen der garantierten Nachrichtenverarbeitung implementieren. Eine einfache Storm-Anwendung kann beispielsweise die Verarbeitung nach dem „At-Least-Once“-Prinzip garantieren, während Trident Nachrichten nach dem „Exactly-Once“-Prinzip garantiert.

Weitere Informationen finden Sie unter [Guarantees on Data Processing](https://storm.apache.org/about/guarantees-data-processing.html) (in englischer Sprache) auf apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Eine übliche Methode besteht darin, ein Eingangstupel zu lesen, mindestens ein Tupel auszugeben und das Eingangstupel am Ende der execute-Methode dann sofort zu bestätigen. Storm stellt die [IBasicBolt](https://storm.apache.org/releases/1.0.3/javadocs/org/apache/storm/topology/IBasicBolt.html)-Schnittstelle für die Automatisierung dieses Musters bereit.

### <a name="joins"></a>Joins

Die Vorgehensweise beim Verknüpfen von Datenströmen variiert zwischen Anwendungen. So können Sie beispielsweise alle Tupel aus mehreren Datenströmen in einem neuen Datenstrom zusammenführen oder nur Tupel-Batches für ein bestimmtes Intervall zusammenführen. In allen Fällen kann das Zusammenführen mithilfe von [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) erreicht werden. Die Feldgruppierung ist eine Möglichkeit, um zu definieren, wie Tupel an Bolts weitergeleitet werden.

Das folgende Java-Beispiel verwendet „fieldsGrouping“, um Tupel aus den Komponenten „1“, „2“ und „3“ an den MyJoiner-Bolt weiterzuleiten:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Batches

Apache Storm bietet einen internen Timermechanismus, der als „Tick-Tupel“ bezeichnet wird. Sie können festlegen, wie oft ein Tick-Tupel in der Topologie ausgegeben wird.

Ein Beispiel für die Verwendung eines Tick-Tupels für eine C#-Komponente finden Sie unter [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

### <a name="caches"></a>Caches

Speicherinterne Zwischenspeicherung wird oft verwendet, um die Verarbeitung zu beschleunigen, da häufig verwendete Elemente im Speicher erhalten bleiben. Da eine Topologie auf mehrere Knoten und mehrere Prozesse auf jedem Knoten verteilt ist, sollten Sie erwägen, [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) zu verwenden. Verwenden Sie `fieldsGrouping`, um sicherzustellen, dass Tupel mit den Feldern, die für die Cachesuche verwendet werden, immer an den gleichen Prozess weitergeleitet werden. Mit dieser Gruppierungsfunktion wird die prozessübergreifende Duplizierung von Cacheeinträgen verhindert.

### <a name="stream-top-n"></a>Datenstrom „Top N“

Wenn Ihre Topologie auf die Berechnung eines Top N-Werts angewiesen ist, berechnen Sie diesen Wert parallel. Führen Sie anschließend die Ausgabe der entsprechenden Berechnungen zu einem globalen Wert zusammen. Hierzu können Sie [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) verwenden, um Daten für die parallele Verarbeitung nach Feldern weiterzuleiten. Anschließend können Sie Daten an einen Bolt weiterleiten, der global den Top N-Wert bestimmt.

Ein Beispiel für die Berechnung eines Top N-Werts finden Sie im Beispiel [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java).

## <a name="logging"></a>Protokollierung

Storm verwendet Apache Log4j für die Protokollierung von Informationen. Standardmäßig wird eine große Datenmenge protokolliert, was die Durchsicht der Informationen erschweren kann. Sie können eine Protokollierungskonfigurationsdatei als Teil Ihrer Storm-Topologie hinzufügen, um das Protokollierungsverhalten zu steuern.

Eine Beispieltopologie, die das Konfigurieren der Protokollierung veranschaulicht, finden Sie im [Java-basierten WordCount](hdinsight-storm-develop-java-topology.md) -Beispiel für Storm in HDInsight.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Lösungen für Echtzeitanalysen mit Storm in HDInsight:

* [Erste Schritte mit Apache Storm in HDInsight][gettingstarted]
* [Beispieltopologien für Apache Storm in HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

