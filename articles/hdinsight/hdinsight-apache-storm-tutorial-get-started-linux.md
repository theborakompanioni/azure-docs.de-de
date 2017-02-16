---
title: 'Apache Storm-Tutorial: Erste Schritte mit Storm in HDInsight unter Linux | Microsoft Docs'
description: Erste Schritte mit Big Data-Analysen in HDInsight unter Linux mit Apache Storm und den Storm-Starter-Beispielen. Erfahren Sie, wie Sie Storm zur Verarbeitung von Daten in Echtzeit verwenden.
keywords: Apache Storm, Apache Storm-Tutorial, Big Data-Analysen, Storm Starter
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: 856043148b3fc28594850ae27bedd57d48292582


---
# <a name="apache-storm-tutorial-get-started-with-the-storm-starter-samples-for-big-data-analytics-on-hdinsight"></a>Apache Storm-Lernprogramm: Erste Schritte mit Storm-Starter-Beispielen für die Big Data-Analyse in HDInsight

Apache Storm ist ein skalierbares, fehlertolerantes, verteiltes Echtzeit-Berechnungssystem für die Verarbeitung von Datenströmen. Mit Storm in Azure HDInsight können Sie einen cloudbasierten Storm-Cluster erstellen, der Big Data-Analysen in Echtzeit durchführt.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Zur Ausführung dieses Lernprogramms zu Apache Storm benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Erfahrung mit SSH und SCP**. Weitere Informationen zur Verwendung von SSH und SCP mit HDInsight finden Sie in den folgenden Artikeln:
  
    * **Linux-, Unix- oder OS X-Clients**: Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows-Clients**: Siehe [Verwenden von SSH (PuTTY) mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-storm-cluster"></a> Erstellen eines Storm-Clusters

In diesem Abschnitt erstellen Sie mithilfe einer Azure Resource Manager-Vorlage einen HDInsight-Cluster der Version 3.5 (Storm-Version 1.0.1). Informationen zu den verschiedenen HDInsight-Versionen und ihren SLAs finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md). Andere Methoden zur Erstellung von Clustern finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen:         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-storm-cluster-in-hdinsight-35.json" target="_blank"><img src="./media/hdinsight-apache-storm-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Die Vorlage befindet sich in einem öffentlichen Blob-Container (*https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-storm-cluster-in-hdinsight.json*). 

2. Wechseln Sie vom Blatt __Benutzerdefinierte Bereitstellung__ zu den folgenden Optionen:
   
    * __Ressourcengruppe__: die Ressourcengruppe, in der der Cluster erstellt wird

    * **Clustername**: der Name für den Hadoop-Cluster

    * __Clusterbenutzername__ und __-kennwort__: Der Standardname für die Anmeldung lautet „admin“.
    
    * __SSH-Benutzername__ und __-kennwort__: der Benutzer und das Kennwort zum Herstellen einer Verbindung mit dem Cluster mithilfe der SSH

    * __Speicherort__: der geografische Speicherort des Clusters.
     
     Bitte notieren Sie diese Werte.  Sie werden diese später im Lernprogramm benötigen.
     
     > [!NOTE]
     > SSH wird verwendet, um remote über eine Befehlszeile auf den HDInsight-Cluster zuzugreifen. Der Benutzername und das Kennwort, die Sie hier verwenden, dienen zum Herstellen der Verbindung mit dem Cluster über SSH. Der SSH-Benutzername muss eindeutig sein, da mit ihm auf allen HDInsight-Clusterknoten ein Benutzerkonto erstellt wird. Die folgenden Namen sind einige der Namen, die für die Verwendung durch Dienste auf dem Cluster reserviert sind und daher nicht als SSH-Benutzername verwendet werden können:
     > 
     > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.
     > 
     > Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Artikeln:
     > 
     > * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
     > * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows mit PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Wählen Sie __Ich stimme den oben genannten Geschäftsbedingungen zu__, klicken Sie auf **OK**, und wählen Sie dann __An Dashboard anheften__.

6. Klicken Sie auf **Kaufen**. Daraufhin wird eine neue Kachel mit der Bezeichnung "Bereitstellung für Vorlagenbereitstellung wird gesendet" angezeigt. Das Erstellen des Clusters dauert ca. 20 Minuten.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Ausführen eines Storm Starter-Beispiels in HDInsight

Die [Storm-Starter](https://github.com/apache/storm/tree/master/examples/storm-starter) -Beispiele sind im HDInsight-Cluster enthalten. In den folgenden Schritten führen Sie das Beispiel "WordCount" aus.

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Wenn Sie zum Schutz Ihres SSH-Benutzerkontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` verwenden, um den passenden privaten Schlüssel anzugeben. Beispiel: `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
   
    Weitere Informationen zum Verwenden von SSH mit Linux-basierten HDInsight-Clustern finden Sie in den folgenden Artikeln:
   
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows mit PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie zum Starten einer Beispieltopologie den folgenden Befehl.
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar storm jar org.apache.storm.starter.WordCountTopology wordcount
   
    > [!NOTE]
    > Unter älteren Versionen von HDInsight ist der Klassenname der Topologie `storm.starter.WordCountTopology` anstelle von `org.apache.storm.starter.WordCountTopology`.
   
    Hierdurch wird die "WordCount"-Beispieltopologie im Cluster mit dem Anzeigenamen "Wordcount" gestartet. Es werden Sätze nach dem Zufallsprinzip generiert und die Instanzen jedes Worts in den Sätzen gezählt.
   
    > [!NOTE]
    > Wenn Ihre eigenen Topologien an den Cluster gesendet werden, müssen Sie zuerst die JAR-Datei mit dem Cluster kopieren, bevor Sie den Befehl `storm` verwenden. Hierzu können Sie den Befehl `scp` auf dem Client mit der Datei verwenden. Beispiel: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    > 
    > Das Beispiel "WordCount" und andere Storm-Starter-Beispiele sind unter `/usr/hdp/current/storm-client/contrib/storm-starter/`bereits auf dem Cluster enthalten.

Wenn Sie die Quelle für die Storm Starter-Beispiele anzeigen möchten, finden Sie den Code unter [https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter). Dieser Link gilt für Storm 1.0.x, das mit HDInsight 3.5 bereitgestellt wird. Verwenden Sie für andere Versionen von Storm die Schaltfläche __Verzweigung__ am oberen Rand der Seite zum Auswählen einer anderen Storm-Version.

## <a name="monitor-the-topology"></a>Überwachen der Topologie

Die Storm-Benutzeroberfläche bietet eine Weboberfläche zum Arbeiten mit ausgeführten Topologien und befindet sich auf dem HDInsight-Cluster.

Führen Sie die folgenden Schritte aus, um die Topologie mithilfe der Storm-Benutzeroberfläche zu überwachen:

1. Öffnen Sie „https://CLUSTERNAME.azurehdinsight.net/stormui“ in einem Browser, wobei **CLUSTERNAME** der Name des Clusters ist. Dadurch wird die Storm-Benutzeroberfläche geöffnet.
    
    > [!NOTE]
    > Wenn Sie dazu aufgefordert werden, einen Benutzernamen und ein Kennwort anzugeben, geben Sie den Namen des Cluster-Administrators (Admin) und das entsprechende Kennwort ein, die Sie beim Erstellen des Clusters verwendet haben.

2. Klicken Sie unter **Topologiezusammenfassung** in der Spalte **Name** auf den Eintrag **wordcount**. Dadurch werden weitere Informationen zur Topologie angezeigt.
    
    ![Storm-Dashboard mit Informationen zur Storm-Starter-WordCount-Topologie.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)
    
    Auf dieser Seite werden die folgenden Informationen angezeigt:
    
    * **Topologiestatistik** – Grundlegende Informationen zur Leistung der Topologie, aufgeteilt in Zeitfenster.
     
        > [!NOTE]
        > Durch die Auswahl eines bestimmten Zeitfensters werden die Informationen in den anderen Abschnitten der Seite entsprechend angepasst.

    * **Spouts** – Grundlegende Informationen zu Spouts, einschließlich des letzten von einem Spout zurückgegebenen Fehlers.
    
    * **Bolts** – Grundlegende Informationen zu Bolts.
    
    * **Topologiekonfiguration** – Ausführliche Informationen zur Konfiguration der Topologie.
     
    Außerdem gibt diese Seite die für die Topologie ausführbaren Aktionen an:
   
    * **Aktivieren** – Setzt die Verarbeitung einer deaktivierten Topologie fort.
    
    * **Deaktivieren** – Hält eine aktive Topologie an.
    
    * **Ausgleichen** – Passt die Parallelität der Topologie an. Sie sollten aktive Topologien ausgleichen, nachdem Sie die Anzahl der Knoten im Cluster geändert haben. Dadurch kann die Topologie die Parallelität anpassen, um die höhere/geringere Anzahl der Knoten im Cluster zu kompensieren. Weitere Informationen finden Sie unter [Grundlegendes zur Parallelität einer Storm-Topologie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).
    
    * **Beenden** – Beendet eine Storm-Topologie nach dem angegebenen Zeitlimit.

3. Wählen Sie auf dieser Seite einen Eintrag aus dem Abschnitt **Spouts** oder **Bolts** aus. Dadurch werden Informationen zur ausgewählten Komponente angezeigt.
   
    ![Storm-Dashboard mit Informationen zu ausgewählten Komponenten.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)
   
    Auf dieser Seite werden die folgenden Informationen angezeigt:
   
    * **Statistik für Spout/Bolt** – Grundlegende Informationen zur Leistung der Komponente, aufgeteilt in Zeitfenster.
     
        > [!NOTE]
        > Durch die Auswahl eines bestimmten Zeitfensters werden die Informationen in den anderen Abschnitten der Seite entsprechend angepasst.
     
    * **Eingabestatistik** (nur Bolt) – Informationen zu Komponenten, die vom Bold konsumierte Daten generieren.
    
    * **Ausgabestatistik** – Informationen zu den von diesem Bold ausgegebenen Daten.
    
    * **Executors** – Informationen zu Instanzen dieser Komponente.
    
    * **Fehler** – Von dieser Komponente generierte Fehler.

4. Zum Anzeigen der Spout- oder Bolt-Details wählen Sie im Abschnitt **Executors** einen Eintrag aus der Spalte **Port** aus, um die Details einer bestimmten Instanz der Komponente anzuzeigen.
   
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]
   
    In diesem Beispiel entnehmen Sie der Spalte, dass das Wort **seven** 1493957mal vorkam. So oft wurde dieses Wort seit dem Start der Topologie erkannt.

## <a name="stop-the-topology"></a>Beenden der Topologie

Kehren Sie zur **Topologiezusammenfassung** der WordCount-Topologie zurück, und klicken Sie im Abschnitt mit den **Topologieaktionen** auf die Schaltfläche **Beenden**. Geben Sie auf Aufforderung für die Verzögerung vor dem Beenden der Topologie 10 (Sekunden) ein. Nach dieser Wartezeit wird die Topologie im Abschnitt **Storm-UI** des Dashboards nicht mehr angezeigt.

## <a name="delete-the-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="a-idnextanext-steps"></a><a id="next"></a>Nächste Schritte

In diesem Lernprogramm zu Apache Storm lernen Sie anhand der Storm-Starter-Beispiele, wie ein Storm-Cluster in HDInsight erstellt wird und wie Storm-Topologien im Storm-Dashboard bereitgestellt, überwacht und verwaltet werden. Anschließend erfahren Sie, wie Sie [Java-basierte Topologien mit Maven entwickeln](hdinsight-storm-develop-java-topology.md).

Wenn Sie bereits mit der Entwicklung von Java-basierten Topologien vertraut sind und eine vorhandene Topologie in HDInsight bereitstellen möchten, lesen Sie die Informationen unter [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight unter Linux](hdinsight-storm-deploy-monitor-topology-linux.md).

Als .NET-Entwickler können Sie C#- oder hybride C#/Java-Topologien mit Visual Studio erstellen. Weitere Informationen finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Beispieltechnologien, die mit Storm in HDInsight verwendet werden können, finden Sie in den folgenden Beispielen:

* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/



<!--HONumber=Jan17_HO3-->


