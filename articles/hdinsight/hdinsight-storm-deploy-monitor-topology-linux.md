---
title: Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight unter Linux | Microsoft Docs
description: "Erfahren Sie, wie Sie Apache Storm-Topologien mithilfe des Storm-Dashboards in Linux-basiertem HDInsight bereitstellen, überwachen und verwalten. Hadoop-Tools für Visual Studio verwenden"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: ca8f3ac0dd5301e1fd06abaf3a292872eb631f47


---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a>Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight

Dieses Dokument enthält die Grundlagen zur Verwaltung und Überwachung von Storm-Topologien, die unter Storm in HDInsight-Clustern ausgeführt werden.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen Linux-basierten HDInsight-Cluster. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date). 
> 
> Informationen zur Bereitstellung und Überwachung von Topologien in Windows-basiertem HDInsight finden Sie unter [Bereitstellen und Verwalten von Apache Storm-Topologien in Windows-basiertem HDInsight](hdinsight-storm-deploy-monitor-topology.md)


## <a name="prerequisites"></a>Voraussetzungen
* **Storm unter Linux in einem HDInsight-Cluster**: Informationen zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit Apache Storm in HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) .

* **Erfahrung mit SSH und SCP**: Weitere Informationen zur Verwendung von SSH und SCP mit HDInsight finden Sie in den folgenden Artikeln:
  
  * **Linux-, Unix- oder OS X-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

  * **Windows-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Ein SCP-Client**: Dieser wird auf allen Linux-, Unix- und OS X-Systemen bereitgestellt. Bei Windows-Clients empfehlen wir PSCP, der auf der [PuTTY-Downloadseite](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)verfügbar ist.

## <a name="start-a-storm-topology"></a>Starten einer Storm-Topologie

### <a name="using-ssh-and-the-storm-command"></a>Mithilfe von SSH und dem Storm-Befehl

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Ersetzen Sie **USERNAME** durch den Namen des SSH-Anmeldung. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters.
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Weitere Informationen zur Verwendung von SSH für den Verbindungsaufbau mit dem HDInsight-Cluster finden Sie in den folgenden Dokumenten:
   
   * **Linux-, Unix- oder OS X-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

   * **Windows-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie zum Starten einer Beispieltopologie den folgenden Befehl.
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar storm.starter.WordCountTopology WordCount
   
    Hierdurch wird die WordCount-Beispieltopologie im Cluster gestartet. Nach dem Zufallsprinzip werden Sätze generiert und die Instanzen jedes Worts in den Sätzen gezählt.
   
   > [!NOTE]
   > Wenn die Topologie an den Cluster gesendet wird, müssen Sie zuerst die JAR-Datei mit dem Cluster kopieren, bevor Sie den Befehl `storm` verwenden. Hierzu können Sie den Befehl `scp` auf dem Client mit der Datei verwenden. Beispiel: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   > 
   > Das Beispiel "WordCount" und andere Storm-Starter-Beispiele sind unter `/usr/hdp/current/storm-client/contrib/storm-starter/`bereits auf dem Cluster enthalten.
   > 
   > 

### <a name="programmatically"></a>Programmgesteuert

Sie können programmgesteuert eine Topologie für Storm in HDInsight bereitstellen, indem Sie mit dem in Ihrem Cluster gehosteten Nimbus-Dienst kommunizieren. [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) zeigt, wie Sie eine Topologie über den Nimbus-Dienst bereitstellen und starten.

## <a name="monitor-and-manage-using-the-storm-command"></a>Überwachen und Verwalten mit dem Befehl "storm"

Mit dem Dienstprogramm `storm` können Sie an der Befehlszeile mit ausgeführten Topologien arbeiten. Im Folgenden finden Sie eine Liste mit häufig verwendeten Befehlen. Eine vollständige Liste der Befehle erhalten Sie mit `storm -h` .

### <a name="list-topologies"></a>Auflisten der Topologien
Mit dem folgenden Befehl können Sie alle ausgeführten Topologien auflisten:

    storm list

Die Ausgabe dieses Befehls sieht etwa so aus:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deaktivieren und erneutes Aktivieren
Durch das Deaktivieren wird eine Topologie unterbrochen, bis sie beendet oder erneut aktiviert wird. Verwenden Sie zum Deaktivieren und erneuten Aktivieren folgende Befehle:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Beenden einer ausgeführten Topologie
Storm-Topologien werden, nachdem sie einmal gestartet wurden, so lange ausgeführt, bis sie beendet werden. Führen Sie zum Beenden einer Topologie den folgenden Befehl aus:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Ausgleichen
Durch das Ausgleichen einer Topologie kann das System die Parallelität der Topologie überarbeiten. Wenn Sie z. B. die Größe des Clusters geändert haben, um zusätzliche Notizen hinzuzufügen, ermöglicht ein Ausgleich einer ausgeführten Topologie, die neuen Knoten zu verwenden.

> [!WARNING]
> Beim Ausgleichen einer Topologie wird diese zunächst deaktiviert. Anschließend werden die Worker gleichmäßig auf den Cluster verteilt, und zum Schluss wird die Topologie wieder in den Zustand vor dem Ausgleich zurückgesetzt. Wenn die Topologie also z. B. aktiv war, wird sie wieder aktiviert. Wenn sie deaktiviert war, bleibt sie deaktiviert.
> 
> 

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Überwachen und Verwalten mit der Storm-Benutzeroberfläche
Die Storm-Benutzeroberfläche bietet eine Weboberfläche zum Arbeiten mit ausgeführten Topologien und befindet sich auf dem HDInsight-Cluster. Öffnen Sie **https://CLUSTERNAME.azurehdinsight.net/stormui** in einem Browser, wobei **CLUSTERNAME** der Name Ihres Clusters ist.

> [!NOTE]
> Wenn Sie dazu aufgefordert werden, einen Benutzernamen und ein Kennwort anzugeben, geben Sie den Namen des Cluster-Administrators (Admin) und das entsprechende Kennwort ein, die Sie beim Erstellen des Clusters verwendet haben.
> 
> 

### <a name="main-page"></a>Hauptseite
Die Hauptseite der Storm-Benutzeroberfläche bietet die folgenden Informationen:

* **Clusterzusammenfassung:**grundlegende Informationen zum Storm-Cluster.
* **Topologiezusammenfassung:**eine Liste der aktiven Topologien. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Topologien anzuzeigen.
* **Supervisor-Zusammenfassung:**Informationen zum Storm-Supervisor.
* **Nimbus-Konfiguration:**die Nimbus-Konfiguration für den Cluster.

### <a name="topology-summary"></a>Topologiezusammenfassung:
Wenn Sie einen Link aus dem Abschnitt **Topologiezusammenfassung** auswählen, werden die folgenden Informationen zur Topologie angezeigt:

* **Topologiezusammenfassung:**grundlegende Informationen zur Topologie.
* **Topologieaktionen:**Verwaltungsaktionen, die für die Topologie ausgeführt werden können.
  
  * **Aktivieren:**setzt die Verarbeitung einer deaktivierten Topologie fort.
  * **Deaktivieren:**hält eine aktive Topologie an.
  * **Ausgleichen:**passt die Parallelität der Topologie an. Sie sollten aktive Topologien ausgleichen, nachdem Sie die Anzahl der Knoten im Cluster geändert haben. Dadurch kann die Topologie die Parallelität anpassen, um die höhere oder geringere Anzahl der Knoten im Cluster zu kompensieren.
    
    Weitere Informationen finden Sie unter <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the Parallelism of a Storm Topology</a> (Grundlegendes zur Parallelität einer Storm-Topologie, in englischer Sprache).
  * **Beenden:**beendet eine Storm-Topologie nach dem angegebenen Timeout.
* **Topologiestatistik:**Statistiken zur Topologie. Verwenden Sie die Links in der Spalte **Fenster** , um den Zeitrahmen für die verbleibenden Einträge auf der Seite festzulegen.
* **Spouts:**die von der Topologie verwendeten Spouts. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Spouts anzuzeigen.
* **Bolts:**die von der Topologie verwendeten Bolts. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Bolts anzuzeigen.
* **Topologiekonfiguration:**die Konfiguration der ausgewählten Topologie.

### <a name="spout-and-bolt-summary"></a>Spout und Bolt: Zusammenfassung
Wenn Sie im Abschnitt **Spouts** oder **Bolts** einen Spout auswählen, werden die folgenden Informationen zum ausgewählten Element angezeigt:

* **Komponentenübersicht:**grundlegende Informationen zum Spout oder Bolt.
* **Statistik für Spout/Bolt:**Statistiken zum Spout oder Bolt. Verwenden Sie die Links in der Spalte **Fenster** , um den Zeitrahmen für die verbleibenden Einträge auf der Seite festzulegen.
* **Eingabestatistik** (nur Bolt): Informationen zu den Eingabedatenströmen, die vom Bolt verbraucht werden.
* **Ausgabestatistik:**Informationen zu den Datenströmen, die von diesem Spout oder Bolt ausgegeben werden.
* **Ausführer:**Informationen zu den Instanzen von Spout oder Bolt. Wählen Sie den Eintrag **Port** für einen bestimmten Ausführer aus, um ein Protokoll mit Diagnoseinformationen anzuzeigen, das für diese Instanz generiert wurde.
* **Fehler:**Fehlerinformationen für diesen Spout oder Bolt.

## <a name="rest-api"></a>REST-API
Die Storm-Benutzeroberfläche baut auf der REST-API auf, sodass Sie mithilfe der REST-API ähnliche Verwaltungs- und Überwachungsfunktionen ausführen können. Mithilfe der REST-API können Sie benutzerdefinierte Tools zum Verwalten und Überwachen von Storm-Topologien erstellen.

Weitere Informationen finden Sie unter [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html)(in englischer Sprache). Die folgenden Informationen gelten für die Verwendung der REST-API mit Apache Storm in HDInsight.

> [!IMPORTANT]
> Die Storm-REST-API ist nicht öffentlich über das Internet verfügbar. Der Zugriff muss über einen SSH-Tunnel zum Hauptknoten des HDInsight-Clusters erfolgen. Weitere Informationen zum Erstellen und Verwenden eines SSH-Tunnels finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md).
> 
> 

### <a name="base-uri"></a>Basis-URI
Der Basis-URI für die REST-API auf Linux-basierten HDInsight-Clustern ist auf dem Hauptknoten unter **https://HEADNODEFQDN:8744/api/v1/** verfügbar. Der Domänenname des Hauptknotens wird während der Clustererstellung generiert und ist nicht statisch.

Sie können den vollqualifizierten Domänennamen (FQDN) für den Clusterhauptknoten auf unterschiedliche Arten ermitteln:

* **Über eine SSH-Sitzung**: Verwenden Sie den Befehl `headnode -f` für eine SSH-Sitzung mit dem Cluster.
* **Über Ambari Web**: Wählen Sie oben auf der Seite **Dienste** und dann **Storm**. Wählen Sie auf der Registerkarte **Zusammenfassung** die Option **Storm UI-Server**. Der vollqualifizierte Domänenname des Knotens, auf dem die Storm UI und die REST-API ausgeführt wird, wird oben auf der Seite angezeigt.
* **Über die Ambari-REST-API**: Verwenden Sie den Befehl `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"`, um Informationen zu dem Knoten abzurufen, auf dem die Storm UI und die REST-API ausgeführt werden. Ersetzen Sie **PASSWORD** durch das Administratorkennwort für den Cluster. Ersetzen Sie **CLUSTERNAME** durch den Namen des Clusters. In der Antwort enthält der Eintrag „host_name“ den vollqualifizierten Domänennamen des Knotens.

### <a name="authentication"></a>Authentifizierung
Anforderungen an die REST-API müssen die **Standardauthentifizierung**und somit den Benutzernamen und das Kennwort des HDInsight-Clusteradministrators verwenden.

> [!NOTE]
> Da die Standardauthentifizierung unverschlüsselt gesendet wird, sollten Sie **immer** HTTPS verwenden, um die Kommunikation mit dem Cluster zu schützen.
> 
> 

### <a name="return-values"></a>Rückgabewerte
Von der REST-API zurückgegebene Informationen sind möglicherweise nur innerhalb des Clusters oder auf den virtuellen Computern in demselben Azure Virtual Network wie der Cluster verwendbar. Auf den für Zookeeper-Server zurückgegebenen vollqualifizierten Domänennamen (FQDN) kann z. B. nicht über das Internet zugegriffen werden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie erfahren haben, wie Sie Topologien mithilfe des Storm-Dashboards bereitstellen und überwachen, lesen Sie jetzt die Informationen zum [Entwickeln von Java-Topologien mithilfe von Maven](hdinsight-storm-develop-java-topology.md).

Eine Liste weiterer Beispieltopologien finden Sie unter [Beispieltopologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).




<!--HONumber=Jan17_HO3-->


