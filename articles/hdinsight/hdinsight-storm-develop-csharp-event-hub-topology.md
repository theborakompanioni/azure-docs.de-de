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
ms.date: 10/27/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 77d0dfe7e09baab9b923b3c49af0cb7c28cd8625


---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Verarbeitung von Ereignissen von Azure-Event-Hubs mit Storm auf HDInsight (C#)
Azure Event Hubs ermöglichen Ihnen die Verarbeitung sehr großer Datenmengen aus Websites, Apps und Geräten. Der Event Hubs-Spout vereinfacht die Verwendung von Apache Storm in HDInsight, um diese Daten in Echtzeit zu analysieren. Sie können auch Daten aus Storm mithilfe des Event Hubs-Bolts in Event Hub schreiben.

In diesem Tutorial erfahren Sie, wie Sie die Visual Studio-Vorlagen verwenden, die mit HDInsight-Tools für Visual Studio installiert werden, um zwei Topologien zu erstellen, die mit Azure Event Hubs funktionieren.

* **EventHubWriter:**generiert Daten nach dem Zufallsprinzip und schreibt sie in Event Hubs
* **EventHubReader**: Liest Daten aus Event Hubs und protokolliert die Daten in Storm-Protokollen

> [!NOTE]
> Während Sie für die Schritte in diesem Dokument eine Windows-Entwicklungsumgebung mit Visual Studio benötigen, kann das kompilierte Projekt an einen Linux- oder Windows-basierten HDInsight-Cluster übermittelt werden. Nur Linux-basierte Cluster, die nach dem 28.10.2016 erstellt wurden, unterstützen SCP.NET-Topologien.
> 
> Um eine C#-Topologie mit einem Linux-basierten Cluster zu verwenden, müssen Sie das NuGet-Paket „Microsoft.SCP.Net.SDK“, das vom Projekt verwendet wird, auf Version 0.10.0.6 oder höher aktualisieren. Die Version des Pakets muss zudem mit der Hauptversion der Storm-Installation auf HDInsight übereinstimmen. Beispielsweise verwenden die Storm in HDInsight-Versionen 3.3 und 3.4 die Storm-Version 0.10.x, während HDInsight 3.5 Storm 1.0.x verwendet.
> 
> C#-Topologien in Linux-basierten Clustern müssen .NET 4.5 verwenden. Zudem muss Mono im HDInsight-Cluster ausgeführt werden. Das meiste ist funktionsfähig, Sie sollten jedoch das Dokument zur [Mono-Kompatibilität](http://www.mono-project.com/docs/about-mono/compatibility/) auf mögliche Inkompatibilitäten überprüfen.
> 
> Eine Java-Version dieses Projekts, das auch mit einem Linux- oder Windows-basierten Cluster funktioniert, finden Sie unter [Verarbeiten von Ereignissen aus Azure Event Hubs mit Storm in HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* Ein [Apache Storm in HDInsight-Cluster](hdinsight-apache-storm-tutorial-get-started.md)
* Ein [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
* Das [Azure .NET SDK](http://azure.microsoft.com/downloads/)
* Die [HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

## <a name="completed-project"></a>Abgeschlossenes Projekt
Sie können eine vollständige Version des Projekts von GitHub herunterladen, die in folgendem Lernprogramm erstellt wurde: [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Sie müssen jedoch weiterhin Konfigurationseinstellungen bereitstellen, indem Sie die Schritte in diesem Lernprogramm befolgen:

## <a name="event-hubs-spout-and-bolt"></a>Spout und Bolt für Event Hubs
Der Spout und der Bolt für Event Hubs sind Java-Komponenten, die Ihnen die Arbeit mit Event Hubs aus Apache Storm vereinfachen. Obwohl diese Komponenten in Java geschrieben sind, ermöglichen die HDInsight-Tools für Visual Studio das Erstellen von hybriden Topologien, in denen C#- und Java-Komponenten gemeinsam verwendet werden.

Der Spout und der Bolt werden als einzelne Java-Archivdatei (JAR) mit dem Namen **eventhubs-storm-spout-#.#-jar-with-dependencies.jar** bereitgestellt. Dabei ist #.# die Version der Datei.

### <a name="download-the-jar-file"></a>Herunterladen der JAR-Datei
Die neueste Version der Datei ist im Projekt [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples) im Ordner **lib/eventhubs** enthalten. Laden Sie die Datei mithilfe einer der folgenden Methoden herunter.

> [!NOTE]
> Spout und Bolt wurden zur Verwendung im Apache Storm-Projekt übermittelt. Weitere Informationen finden Sie bei GitHub unter [STORM-583: Initiali check-in for storm-eventhubs](https://github.com/apache/storm/pull/336/files) .
> 
> 

* **Herunterladen einer ZIP-Datei**: Wählen Sie auf der Website für [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples) die Schaltfläche **Download ZIP** aus, um eine ZIP-Datei mit dem Projekt herunterzuladen.
  
    ![Schaltfläche "Download ZIP"](./media/hdinsight-storm-develop-csharp-event-hub-topology/download.png)
  
    Nach dem Herunterladen der Datei können Sie das Archiv extrahieren – die Datei befindet sich im Verzeichnis **lib** .
* **Klonen des Projekts**: Wenn [Git](http://git-scm.com/) installiert ist, verwenden Sie folgenden Befehl, um das Repository lokal zu klonen – die Datei befindet sich anschließend im Verzeichnis **lib**.
  
        git clone https://github.com/hdinsight/hdinsight-storm-examples

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
2. Wählen Sie im Dialogfeld **Topologie übermitteln** Ihren **Storm-Cluster** aus. Erweitern Sie **Zusätzliche Konfigurationen**, wählen Sie **Java File Paths** und dann **...** aus, und wählen Sie das Verzeichnis aus, das die zuvor heruntergeladene Datei **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** enthält. Klicken Sie abschließend auf **Senden**.
   
    ![Bild des Dialogfelds "Senden"](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)
3. Sobald die Topologie übermittelt wurde, wird der **Storm Topologies Viewer** angezeigt. Wählen Sie im linken Bereich die Topologie **EventHubReader** aus, um die Statistiken für diese Topologie anzuzeigen. Aktuell sollte dort nichts angezeigt werden, da noch keine Ereignisse in Event Hubs geschrieben wurden.
   
    ![Beispiel für Speicheransicht](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)
4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **EventHubWriter**, und wählen Sie **An Storm in HDInsight übermitteln** aus.
5. Wählen Sie im Dialogfeld **Topologie übermitteln** Ihren **Storm-Cluster** aus. Erweitern Sie **Zusätzliche Konfigurationen**, wählen Sie **Java File Paths** und dann **...** aus, und wählen Sie das Verzeichnis aus, das die zuvor heruntergeladene Datei **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** enthält. Klicken Sie abschließend auf **Senden**.
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

## <a name="notes"></a>Hinweise
### <a name="checkpointing"></a>Setzen von Prüfpunkten
Der EventHubSpout erstellt in regelmäßigen Abständen Prüfpunkte im Zookeeper-Knoten, der den aktuellen Offset für aus der Warteschlange gelesene Nachrichten speichert. Dies ermöglicht der Komponente, in den folgenden Szenarios mit dem Empfang von Nachrichten an dem gespeicherten Offset zu beginnen:

* Die Instanz der Komponente wird aufgrund eines Fehlers neu gestartet.
* Sie vergrößern oder verkleinern den Cluster durch Hinzufügen oder Entfernen von Knoten.
* Die Topologie wird beendet und **mit demselben Namen**neu gestartet.

Die persistenten Prüfpunkte können auch an WASB (dem von Ihrem HDInsight-Cluster verwendeten Azure-Speicher) exportiert und importiert werden. Die dafür geeigneten Skripts befinden sich auf dem Storm-Cluster in HDInsight unter **c:\apps\dist\storm-0.9.3.2.2.1.0-2340\zkdatatool-1.0\bin**.

> [!NOTE]
> Die Versionsnummer im Pfad kann abweichen, da sich die auf dem Cluster installierte Storm-Version in Zukunft ändern könnte.
> 
> 

Die Skripts in diesem Verzeichnis sind:

* **stormmeta_import.cmd**: importiert alle Storm-Metadaten vom standardmäßigen Speichercontainer des Clusters in Zookeeper.
* **stormmeta_export.cmd**: exportiert alle Storm-Metadaten von Zookeeper in den standardmäßigen Speichercontainer des Clusters.
* **stormmeta_delete.cmd**: Alle Storm-Metadaten werden aus Zookeeper gelöscht.

Durch das Exportieren und Importieren können Sie Prüfpunktdaten persistent speichern, um beispielsweise einen Cluster zu löschen und nach dem Onlineschalten des neuen Clusters die Verarbeitung am aktuellen Offset fortsetzen zu können.

> [!NOTE]
> Da die Daten im Standard-Speichercontainer persistent gespeichert werden, **muss** der neue Cluster das gleiche Speicherkonto und den gleichen Container verwenden wie der vorherige Cluster.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie den Java-Spout und den -Bolt für Event Hubs aus einer C#-Topologie verwenden, um mit Daten in Azure Event Hub zu arbeiten. Weitere Informationen zum Erstellen von C#-Topologien finden Sie in folgenden Themen.

* [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [SCP-Programmierleitfaden](hdinsight-storm-scp-programming-guide.md)
* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)




<!--HONumber=Nov16_HO3-->


