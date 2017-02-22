---
title: Verarbeiten von Ereignissen aus Event Hubs mit Storm in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Event Hubs-Daten mit einer C#-Storm-Topologie verarbeiten, die mithilfe der HDInsight-Tools für Visual Studio in Visual Studio erstellt wurde."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: c9a5091973395dd888939432292fbd06dcbf0680
ms.openlocfilehash: c0349b5890a75c6ffaa6b7eca93baa3101912cf6


---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (C#)

Azure Event Hubs ermöglichen Ihnen die Verarbeitung sehr großer Datenmengen aus Websites, Apps und Geräten. Der Event Hubs-Spout vereinfacht die Verwendung von Apache Storm in HDInsight, um diese Daten in Echtzeit zu analysieren. Sie können auch Daten aus Storm mithilfe des Event Hubs-Bolts in Event Hub schreiben.

In diesem Tutorial erfahren Sie, wie Sie die Visual Studio-Vorlagen verwenden, die mit HDInsight-Tools für Visual Studio installiert werden, um zwei Topologien zu erstellen, die mit Azure Event Hubs funktionieren.

* **EventHubWriter:**generiert Daten nach dem Zufallsprinzip und schreibt sie in Event Hubs
* **EventHubReader**: Liest Daten aus Event Hubs und protokolliert die Daten in Storm-Protokollen

> [!NOTE] 
> Eine Java-Version dieses Projekts finden Sie unter [Verarbeiten von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="scpnet"></a>SCP.NET

Diese Projekte verwenden SCP.NET, ein NuGet-Paket, das die Erstellung von C#-Topologien und -Komponenten zur Verwendung mit Storm auf HDInsight vereinfacht.

> [!IMPORTANT]
> Während Sie für die Schritte in diesem Dokument eine Windows-Entwicklungsumgebung mit Visual Studio benötigen, kann das kompilierte Projekt an einen Linux-basierten Cluster mit Storm auf HDInsight übermittelt werden. __Nur Linux-basierte Cluster, die nach dem 28.10.2016 erstellt wurden, unterstützen SCP.NET-Topologien.__

### <a name="cluster-versioning"></a>Clusterversionsverwaltung

Das Microsoft.SCP.Net.SDK-NuGet-Paket, das von Ihrem Projekt verwendet wird, muss der Hauptversion von Storm entsprechen, das auf HDInsight installiert ist. Die Versionen 3.3 und 3.4 von Storm auf HDInsight verwenden die Storm-Version 0.10.x, daher müssen Sie mit diesen Clustern die SCP.NET-Version 0.10.x.x verwenden. HDInsight 3.5 verwendet Storm 1.0.x., daher müssen Sie mit dieser Clusterversion die SCP.NET-Version 1.0.x.x verwenden.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das mit HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

Die HDInsight-Versionen 3.4 und höher verwenden Mono, um C#-Topologien auszuführen. Die meisten Funktionen lassen sich mit Mono ausführen, Sie sollten jedoch das Dokument zur [Mono-Kompatibilität](http://www.mono-project.com/docs/about-mono/compatibility/) auf mögliche Inkompatibilitäten überprüfen.

C#-Topologien müssen auch auf .NET 4.5 ausgerichtet sein.

## <a name="how-to-work-with-event-hubs"></a>Arbeiten mit Event Hubs

Microsoft stellt eine Reihe von Java-Komponenten bereit, die für die Kommunikation mit Azure Event Hubs über eine Storm-Topologie verwendet werden können. Die Jar-Datei mit der neuesten Version dieser Komponenten finden Sie unter [https://github.com/hdinsight/hdinsight-storm-examples/blob/master/lib/eventhubs/](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/lib/eventhubs/).

> [!IMPORTANT]
> Die Komponenten sind zwar in Java geschrieben, Sie können sie jedoch problemlos in einer C#-Topologie verwenden.

Sie werden vermutlich primär mit folgenden Komponenten arbeiten:

* __EventHubSpout__: Liest Daten aus Event Hubs.
* __EventHubBolt__: Schreibt Daten in Event Hubs.
* __EventHubSpoutConfig__: Wird zum Konfigurieren von EventHubSpout verwendet.
* __EventHubBoltConfig__: Wird zum Konfigurieren von EventHubBolt verwendet.
* __UnicodeEventDataScheme__: Wird zum Konfigurieren des Spouts zur Verwendung der UTF-8-Codierung beim Lesen aus Event Hubs verwendet. Wenn diese Komponente nicht verwendet wird, verwendet der Spout standardmäßig die Zeichenfolgencodierung.

### <a name="example-spout-usage"></a>Beispiel für die Verwendung von Spouts

SCP.NET stellt Methoden bereit, die speziell zum Hinzufügen eines EventHubSpout zu Ihrer Topologie dienen. Mit diesen Methoden lassen sich Spouts einfach hinzufügen als mit den allgemeinen Methoden zum Hinzufügen einer Java-Komponente. Das folgende Beispiel zeigt, wie Sie mit dem __SetEventHubSpout__ und den von SCP.NET bereitgestellten EventHubSpoutConfig-Methoden einen neuen Spout erstellen:

```csharp
topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        // the shared access signature name and key used to read the data
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        // The namespace that contains the Event Hub to read from
        ConfigurationManager.AppSettings["EventHubNamespace"],
        // The Event Hub name to read from
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        // The number of partitions in the Event Hub
        eventHubPartitions),
    // Parallelism hint for this component. Should be set to the partition count.
    eventHubPartitions);
```

Das obige Beispiel erstellt eine neue Spoutkomponente namens __EventHubSpout__, und konfiguriert diese für die Kommunikation mit einem Event Hub. Beachten Sie, dass der Parallelitätshinweis für die Komponente auf die Anzahl von Partitionen im Event Hub festgelegt ist. So kann Storm eine Instanz der Komponente für jede Partition erstellen.

> [!WARNING]
> Seit 1. Januar 2017 wird durch Verwendung der Methoden SetEventHubSpout und EventHubSpoutConfig ein Spout erstellt, der beim Lesen von Daten aus Event Hubs die Zeichenfolgencodierung verwendet. Informationen zum Verwenden der UTF-8-Codierung finden Sie im folgenden Beispiel.

Sie können auch die allgemeine JavaComponentConstructor-Methode verwenden, wenn Sie einen Spout erstellen. Das folgende Beispiel veranschaulicht die Erstellung eines neuen Spouts mithilfe der JavaComponentConstructor-Methode. Es zeigt auch, wie Sie den Spout so konfigurieren, dass Daten nicht mit Zeichenfolgencodierung, sondern mit UTF-8-Codierung gelesen werden:

```csharp
// Create an instance of UnicodeEventDataScheme
var schemeConstructor = new JavaComponentConstructor("com.microsoft.eventhubs.spout.UnicodeEventDataScheme");
// Create an instance of EventHubSpoutConfig
var eventHubSpoutConfig = new JavaComponentConstructor(
    "com.microsoft.eventhubs.spout.EventHubSpoutConfig",
    new List<Tuple<string, object>>()
    {
        // the shared access signature name and key used to read the data
        Tuple.Create<string, object>(JavaComponentConstructor.JAVA_LANG_STRING, ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"]),
        Tuple.Create<string, object>(JavaComponentConstructor.JAVA_LANG_STRING, ConfigurationManager.AppSettings["EventHubSharedAccessKey"]),
        // The namespace that contains the Event Hub to read from
        Tuple.Create<string, object>(JavaComponentConstructor.JAVA_LANG_STRING, ConfigurationManager.AppSettings["EventHubNamespace"]),
        // The Event Hub name to read from
        Tuple.Create<string, object>(JavaComponentConstructor.JAVA_LANG_STRING, ConfigurationManager.AppSettings["EventHubEntityPath"]),
        // The number of partitions in the Event Hub
        Tuple.Create<string, object>("int", eventHubPartitions),
        // The encoding scheme to use when reading
        Tuple.Create<string, object>("com.microsoft.eventhubs.spout.IEventDataScheme", schemeConstructor)
    }
    );
// Create an instance of the spout
var eventHubSpout = new JavaComponentConstructor(
    "com.microsoft.eventhubs.spout.EventHubSpout",
    new List<Tuple<string, object>>()
    {
        Tuple.Create<string, object>("com.microsoft.eventhubs.spout.EventHubSpoutConfig", eventHubSpoutConfig)
    }
    );
// Set the spout in the topology
topologyBuilder.SetJavaSpout("EventHubSpout", eventHubSpout, eventHubPartitions);
```

> [!IMPORTANT]
> UnicodeEventDataScheme ist nur in Version 9.5 der Event Hub-Komponenten verfügbar, die hier zur Verfügung steht: [https://github.com/hdinsight/hdinsight-storm-examples/blob/master/lib/eventhubs/](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/lib/eventhubs/).

### <a name="example-bolt-usage"></a>Beispiel für die Verwendung von Bolts

Sie müssen die JavaComponentConstructor-Methode verwenden, um eine Instanz des Bolts zu erstellen. Das folgende Beispiel veranschaulicht, wie Sie eine neue Instanz von EventHubBolt erstellen und konfigurieren:

```csharp
//Create constructor for the Java bolt
JavaComponentConstructor constructor =
    // Use a Clojure expression to create the EventHubBoltCOnfig
    JavaComponentConstructor.CreateFromClojureExpr(
        String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
    // The policy name and key used to read from Event Hubs
    ConfigurationManager.AppSettings["EventHubPolicyName"],
    ConfigurationManager.AppSettings["EventHubPolicyKey"],
    // The namespace that contains the Event Hub
    ConfigurationManager.AppSettings["EventHubNamespace"],
    "servicebus.windows.net", //suffix for the namespace fqdn
    // The Evetn Hub Name)
    ConfigurationManager.AppSettings["EventHubName"],
    "true"));

//Set the bolt
topologyBuilder.SetJavaBolt(
        "EventHubBolt",
        constructor,
        partitionCount). //Parallelism hint uses partition count
    shuffleGrouping("Spout"); //Consume data from spout
```

> [!NOTE]
> Dieses Beispiel verwendet einen als Zeichenfolge übergebenen Clojure-Ausdruck anstelle von JavaComponentConstructor, um eine andere EventHubBoltConfig-Komponente zu erstellen als das Spout-Beispiel. Beide Methoden funktionieren gut – verwenden Sie diejenige, die für Sie am besten geeignet ist.

## <a name="download-the-completed-project"></a>Herunterladen des abgeschlossenen Projekts

Sie können eine vollständige Version des Projekts von GitHub herunterladen, die in folgendem Lernprogramm erstellt wurde: [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Sie müssen jedoch weiterhin Konfigurationseinstellungen bereitstellen, indem Sie die Schritte in diesem Lernprogramm befolgen:

### <a name="prerequisites"></a>Voraussetzungen

* Ein [Apache Storm auf HDInsight-Clusterversion 3.5](hdinsight-apache-storm-tutorial-get-started.md)

    > [!WARNING]
    > Das in diesem Dokument verwendete Beispiel erfordert Storm auf HDInsight Version 3.5. Von den Versionen auf älteren Clustern zu der in HDInsight 3.5 enthaltenen Version von Storm wurden Änderungen der für Kernkomponenten von Storm verwendeten Klassennamen vorgenommen. Eine Version dieses Beispiels, die mit älteren Clustern funktioniert, finden Sie unter [https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* Ein [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

* Das [Azure .NET SDK](http://azure.microsoft.com/downloads/)

* Die [HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

* Java JDK 1.7 oder höher in Ihrer Entwicklungsumgebung. JDK-Downloads stehen über [http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html) zur Verfügung.

  * Die **JAVA_HOME**-Umgebungsvariable muss auf das Verzeichnis mit Java zeigen.
  * Das **%JAVA_HOME%/bin**-Verzeichnis muss im Pfad liegen.

## <a name="download-the-event-hub-components"></a>Herunterladen der Event Hub-Komponenten

Der Spout und der Bolt werden als einzelne Java-Archivdatei (JAR) mit dem Namen **eventhubs-storm-spout-#.#-jar-with-dependencies.jar** bereitgestellt. Dabei ist #.# die Version der Datei.

Eine Version der Jar-Datei, die mit Storm auf HDInsight Version 3.5 funktioniert, kann hier heruntergeladen werden: [https://github.com/hdinsight/hdinsight-storm-examples/blob/master/lib/eventhubs/](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/lib/eventhubs/).

Erstellen Sie ein neues Verzeichnis mit dem Namen `eventhubspout`, und speichern Sie die Datei in dem Verzeichnis.

## <a name="configure-event-hubs"></a>Konfigurieren von Event Hubs

Event Hubs ist die Datenquelle für dieses Beispiel. Verwenden Sie die Informationen im Abschnitt **Erstellen eines Event Hubs** im Dokument [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

1. Nachdem der Event Hub erstellt wurde, zeigen Sie das EventHub-Blatt im Azure-Portal an, und wählen Sie **Freigegebene Zugriffsrichtlinien** aus. Verwenden Sie den Eintrag **+ Hinzufügen**, um die folgenden Richtlinien hinzuzufügen:
   
   | Name | Berechtigungen |
   | --- | --- |
   | writer |Send |
   | reader |Empfangen |
   
    ![Richtlinien](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. Wählen Sie die Richtlinien **reader** und **writer** aus. Kopieren und speichern Sie den Wert **PRIMÄRSCHLÜSSEL** für beide Richtlinien, da diese später verwendet werden.

## <a name="configure-the-eventhubwriter"></a>Konfigurieren von EventHubWriter

1. Wenn Sie die neueste Version der HDInsight-Tools für Visual Studio noch nicht installiert haben, finden Sie Informationen dazu unter [Erste Schritte mit den HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Laden Sie die Projektmappe von [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub) herunter. Öffnen Sie die Projektmappe, und nehmen Sie sich ein wenig Zeit, den Code für das Projekt **EventHubWriter** durchzusehen.

3. Öffnen Sie im Projekt **EventHubWriter** die Datei **App.config**. Verwenden Sie die Informationen aus dem Event Hub, den Sie zuvor konfiguriert haben, um den Wert für die folgenden Schlüssel anzugeben:
   
   | Schlüssel | Wert |
   | --- | --- |
   | EventHubPolicyName |writer (Wenn Sie einen anderen Namen für die Richtlinie mit der Berechtigung *Send* verwendet haben, verwenden sie ihn stattdessen.) |
   | EventHubPolicyKey |Der Schlüssel für die writer-Richtlinie |
   | EventHubNamespace |Der Namespace, der den Event Hub enthält |
   | EventHubName |Ihr Event Hub-Name |
   | EventHubPartitionCount |Die Anzahl der Partitionen in Ihrem Event Hub |

4. Speichern und schließen Sie die Datei **App.config**.

## <a name="configure-the-eventhubreader"></a>Konfigurieren von EventHubReader

1. Öffnen Sie das Projekt **EventHubReader**, und nehmen Sie sich ein wenig Zeit, den Code durchzusehen.

2. Öffnen Sie **App.config** für **EventHubWriter**. Verwenden Sie die Informationen aus dem Event Hub, den Sie zuvor konfiguriert haben, um den Wert für die folgenden Schlüssel anzugeben:
   
   | Schlüssel | Wert |
   | --- | --- |
   | EventHubPolicyName |reader (Wenn Sie einen anderen Namen für die Richtlinie mit der Berechtigung *Listen* verwendet haben, verwenden sie ihn stattdessen.) |
   | EventHubPolicyKey |Der Schlüssel für die reader-Richtlinie |
   | EventHubNamespace |Der Namespace, der den Event Hub enthält |
   | EventHubName |Ihr Event Hub-Name |
   | EventHubPartitionCount |Die Anzahl der Partitionen in Ihrem Event Hub |

3. Speichern und schließen Sie die Datei **App.config**.

## <a name="deploy-the-topologies"></a>Bereitstellen der Topologien

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **EventHubReader**, und wählen Sie **An Storm in HDInsight übermitteln** aus.
   
    ![An Storm senden](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Wählen Sie im Dialogfeld **Topologie übermitteln** Ihren **Storm-Cluster** aus. Erweitern Sie **Zusätzliche Konfigurationen**, wählen Sie **Java File Paths** und dann **...** aus, und wählen Sie das Verzeichnis aus, das die zuvor heruntergeladene JAR-Datei enthält. Klicken Sie abschließend auf **Senden**.
   
    ![Bild des Dialogfelds "Senden"](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. Sobald die Topologie übermittelt wurde, wird der **Storm Topologies Viewer** angezeigt. Wählen Sie im linken Bereich die Topologie **EventHubReader** aus, um die Statistiken für diese Topologie anzuzeigen. Aktuell sollte dort nichts angezeigt werden, da noch keine Ereignisse in Event Hubs geschrieben wurden.
   
    ![Beispiel für Speicheransicht](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **EventHubWriter**, und wählen Sie **An Storm in HDInsight übermitteln** aus.

5. Wählen Sie im Dialogfeld **Topologie übermitteln** Ihren **Storm-Cluster** aus. Erweitern Sie **Zusätzliche Konfigurationen**, wählen Sie **Java File Paths** und dann **...** aus, und wählen Sie das Verzeichnis aus, das die zuvor heruntergeladene JAR-Datei enthält. Klicken Sie abschließend auf **Senden**.

6. Sobald die Topologie übermittelt wurde, aktualisieren Sie die Topologieliste im **Storm Topologies Viewer** , um sicherzustellen, dass beide Topologien auf dem Cluster ausgeführt werden.

7. Wählen Sie im **Storm Topologies Viewer** die Topologie **EventHubReader** aus.

8. Doppelklicken Sie in der Diagrammansicht auf die Komponente **LogBolt**. Dadurch wird die Seite **Komponentenübersicht** für den Bolt geöffnet.

9. Wählen Sie im Abschnitt **Ausführer** einen der Links in der Spalte **Port** aus. Dadurch werden die von der Komponente protokollierten Informationen angezeigt. Die protokollierten Informationen ähneln den folgenden:
   
        2016-10-20 13:26:44.186 m.s.s.b.ScpNetBolt [INFO] Processing tuple: source: com.microsoft.eventhubs.spout.EventHubSpout:7, stream: default, id: {5769732396213255808=520853934697489134}, [{"deviceId":3,"deviceValue":1379915540}]
        2016-10-20 13:26:44.234 m.s.s.b.ScpNetBolt [INFO] Processing tuple: source: com.microsoft.eventhubs.spout.EventHubSpout:7, stream: default, id: {7154038361491319965=4543766486572976404}, [{"deviceId":3,"deviceValue":459399321}]
        2016-10-20 13:26:44.335 m.s.s.b.ScpNetBolt [INFO] Processing tuple: source: com.microsoft.eventhubs.spout.EventHubSpout:6, stream: default, id: {513308780877039680=-7571211415704099042}, [{"deviceId":5,"deviceValue":845561159}]
        2016-10-20 13:26:44.445 m.s.s.b.ScpNetBolt [INFO] Processing tuple: source: com.microsoft.eventhubs.spout.EventHubSpout:7, stream: default, id: {-2409895457033895206=5479027861202203517}, [{"deviceId":8,"deviceValue":2105860655}]

## <a name="stop-the-topologies"></a>Beenden der Topologien

Um die Topologien zu beenden, wählen Sie jede Topologie im **Storm Topologies Viewer** aus, und klicken Sie dann auf **Beenden**.

![Bild der Beendigung einer Topologie](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie den Java-Spout und den -Bolt für Event Hubs aus einer C#-Topologie verwenden, um mit Daten in Azure Event Hub zu arbeiten. Weitere Informationen zum Erstellen von C#-Topologien finden Sie in folgenden Themen.

* [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [SCP-Programmierleitfaden](hdinsight-storm-scp-programming-guide.md)
* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)




<!--HONumber=Jan17_HO3-->


