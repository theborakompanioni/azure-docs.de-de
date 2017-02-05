---
title: Verwenden von Apache Kafka mit Storm in HDInsight | Microsoft Docs
description: "Apache Kafka wird mit Apache Storm in HDInsight installiert. Es wird beschrieben, wie Sie in Kafka schreiben und dann Daten auslesen, indem Sie die von Storm bereitgestellten Komponenten KafkaBolt und KafkaSpout verwenden. Außerdem erfahren Sie, wie Sie das Flux-Framework einsetzen, um Storm-Topologien zu definieren und zu übermitteln."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: paulettm
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/09/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: e1c99bbe9d6317d83cc5e71ca4f79d862223aa0a
ms.openlocfilehash: 080def517b9a443d33148354226a394494a66e03

---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a>Verwenden von Apache Kafka (Vorschau) mit Storm in HDInsight

Apache Kafka ist eine Veröffentlichen-Abonnieren-Messaginglösung, die für HDInsight verfügbar ist. Apache Storm ist ein verteiltes System, das zum Analysieren von Daten in Echtzeit verwendet werden kann. In diesem Dokument wird veranschaulicht, wie Sie Storm in HDInsight verwenden können, um Daten aus Kafka in HDInsight zu lesen und zu verarbeiten. Im Beispiel in diesem Dokument wird eine Java-basierte Storm-Topologie verwendet, die auf den unter Apache Storm verfügbaren Kafka-Spout- und -Bolt-Komponenten basiert.

> [!NOTE]
> Mit den Schritten in diesem Dokument wird eine Azure-Ressourcengruppe erstellt, die sowohl ein Cluster vom Typ „Storm in HDInsight“ als auch vom Typ „Kafka in HDInsight“ enthält. Diese Cluster befinden sich beide in einem virtuellen Azure-Netzwerk, damit der Storm-Cluster direkt mit dem Kafka-Cluster kommunizieren kann.
> 
> Denken Sie nach dem Ausführen der Schritte in diesem Dokument daran, die Cluster zu löschen, um das Anfallen von Gebühren zu verhindern.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 1.8 oder höher Oder ein gleichwertiges Kit, z.B. [OpenJDK](http://openjdk.java.net/)
  
    > [!NOTE]
    > In den Schritten in diesem Dokument wird ein HDInsight 3.5-Cluster mit Java 8 verwendet.

* [Maven 3.x](http://maven.apache.org/): Ein Buildverwaltungspaket für Java-Anwendungen.

* Ein Text-Editor oder eine Java-IDE

* Ein SSH-Client (Befehle `ssh` und `scp` erforderlich). Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:
  
  * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

  * [Verwenden von SSH mit Linux-basiertem HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="create-the-clusters"></a>Erstellen von Clustern

Apache Kafka in HDInsight ermöglicht keinen Zugriff auf die Kafka-Broker über das öffentliche Internet. Komponenten, die mit Kafka kommunizieren, müssen sich jeweils in demselben virtuellen Azure-Netzwerk wie die Knoten im Kafka-Cluster befinden. Für dieses Beispiel sind die Kafka- und Storm-Cluster in einem virtuellen Azure-Netzwerk angeordnet. Im folgenden Diagramm ist dargestellt, wie der Kommunikationsfluss zwischen den Clustern abläuft:

![Diagramm mit Storm- und Kafka-Clustern in einem virtuellen Azure-Netzwerk](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> Kafka selbst ist zwar auf die Kommunikation innerhalb des virtuellen Netzwerks beschränkt, aber auf andere Dienste im Cluster, z.B. SSH und Ambari, kann über das Internet zugegriffen werden. Weitere Informationen zu den öffentlichen Ports, die für HDInsight verfügbar sind, finden Sie unter [Von HDInsight verwendete Ports und URIs](hdinsight-hadoop-port-settings-for-services.md).


Es ist zwar möglich, ein virtuelles Azure-Netzwerk, Kafka-Cluster und Storm-Cluster manuell zu erstellen, aber mit einer Azure Resource Manager-Vorlage ist dies erheblich einfacher. Führen Sie die folgenden Schritte aus, um ein virtuelles Azure-Netzwerk und Kafka- und Storm-Cluster für Ihr Azure-Abonnement bereitzustellen.

1. Verwenden Sie die folgende Schaltfläche, um sich bei Azure anzumelden, und öffnen Sie die Vorlage im Azure-Portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Die Azure Resource Manager-Vorlage befindet sich unter **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet.json**.

2. Verwenden Sie die folgende Anleitung, um die Einträge auf dem Blatt **Benutzerdefinierte Bereitstellung** einzufügen:
   
    ![Benutzerdefinierte HDInsight-Bereitstellung](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * **Ressourcengruppe**: Erstellen Sie eine Gruppe, oder wählen Sie eine vorhandene Gruppe aus. Diese Gruppe enthält den HDInsight-Cluster.
   
    * **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus. Der Standort muss mit dem Standort im Abschnitt __EINSTELLUNGEN__ übereinstimmen.

    * **Base Cluster Name** (Cluster-Basisname): Dieser Wert wird als Basisname für Storm- und Kafka-Cluster verwendet. Wenn Sie beispielsweise **hdi** eingeben, wird ein Storm-Cluster mit dem Namen **storm-hdi** und ein Kafka-Cluster mit dem Namen **kafka-hdi** erstellt.
   
    * **Benutzername für Clusteranmeldung**: Der Administratorbenutzername für die Storm- und Kafka-Cluster.
   
    * **Kennwort für Clusteranmeldung**: Das Administratorbenutzerkennwort für die Storm- und Kafka-Cluster.
    
    * **SSH-Benutzername**: Der SSH-Benutzer, der für die Storm- und Kafka-Cluster erstellt wird.
    
    * **SSH-Kennwort**: Das Kennwort für den SSH-Benutzer für die Storm- und Kafka-Cluster.
    
    * **Standort**: Die Region, in der die Cluster erstellt werden.

3. Lesen Sie die **Geschäftsbedingungen**, und wählen Sie anschließend die Option **Ich stimme den oben genannten Geschäftsbedingungen zu**.

4. Aktivieren Sie zum Schluss **An Dashboard anheften**, und wählen Sie dann **Kaufen** aus. Das Erstellen der Cluster dauert ca. 20 Minuten.

Sobald die Ressourcen erstellt sind, werden Sie zu einem Blatt für die Ressourcengruppe weitergeleitet, das die Cluster und das Webdashboard enthält.

![Ressourcengruppenblatt für vnet und Cluster](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> Beachten Sie, dass die Namen der HDInsight-Cluster **storm-BASENAME** und **kafka-BASENAME** lauten, wobei BASENAME der Name ist, den Sie für die Vorlage angegeben haben. Sie verwenden diese Namen in späteren Schritten, wenn Sie eine Verbindung mit den Clustern herstellen.

## <a name="get-the-code"></a>Abrufen des Codes

Der Code für das in diesem Dokument beschriebene Beispiel ist unter [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka) verfügbar.

## <a name="understanding-the-code"></a>Grundlegendes zum Code

Dieses Projekt enthält zwei Topologien:

* **KafkaWriter**: Diese Topologie wird mit der Datei **writer.yaml** definiert. Sie schreibt zufällige Sätze in Kafka, indem sie die KafkaBolt-Komponente von Apache Storm verwendet.
  
    Für diese Topologie wird eine benutzerdefinierte **SentenceSpout**-Komponente zum Generieren von zufälligen Sätzen verwendet.

* **KafkaReader**: Diese Topologie wird mit der Datei **reader.yaml** definiert. Sie liest Daten aus Kafka, indem sie die KafkaSpout-Komponente von Apache Storm verwendet, und protokolliert die Daten dann in stdout.
  
    Für diese Topologie wird eine benutzerdefinierte **PrinterBolt**-Komponente verwendet, um aus Kafka ausgelesene Daten zu protokollieren.

### <a name="flux"></a>Flux

Die Topologien werden mit [Flux](https://storm.apache.org/releases/1.0.1/flux.html) definiert. Flux wurde mit Storm 0.10.x eingeführt und ermöglicht Ihnen die Trennung der Topologiekonfiguration vom Code. Für Topologien, die das Flux-Framework nutzen, wird die Topologie in einer YAML-Datei definiert. Die YAML-Datei kann als Teil der Topologie eingebunden oder angegeben werden, wenn Sie die Topologie an den Storm-Server übermitteln. Außerdem unterstützt Flux die Variablenersetzung zur Laufzeit, die in diesem Beispiel verwendet wird.

Für beide Topologien werden die folgenden Umgebungsvariablen erwartet:

* **KAFKATOPIC**: Der Name des Kafka-Themas, für das die Topologien die Lese-/Schreibvorgänge durchführen.

* **KAFKABROKERS**: Die Hosts, auf denen die Kafka-Broker ausgeführt werden. Die Brokerinformationen werden von der KafkaBolt-Komponente verwendet, wenn auf Kafka geschrieben wird.

* **KAFKAZKHOSTS**: Die Hosts, auf denen Zookeeper ausgeführt wird.

Anhand der Schritte in diesem Dokument wird veranschaulicht, wie Sie diese Umgebungsvariablen festlegen.

## <a name="create-a-kafka-topic"></a>Erstellen eines Kafka-Themas

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem Kafka-Cluster her. Ersetzen Sie **USERNAME** durch den SSH-Benutzernamen, der beim Erstellen des Clusters verwendet wurde. Ersetzen Sie **BASENAME** durch den Basisnamen, der beim Erstellen des Clusters verwendet wurde.
   
        ssh USERNAME@kafka-BASENAME-ssh.azurehdinsight.net
   
    Geben Sie nach der entsprechenden Aufforderung das Kennwort ein, das Sie beim Erstellen der Cluster verwendet haben.
   
    Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:
   
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basiertem HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie für die SSH-Verbindung mit dem Kafka-Cluster die folgenden Befehle, um die Zookeeper-Knoten aus Ambari abzurufen:

        # Install JQ to make working with JSON easier
        sudo apt -y install jq
        # Query Ambari for 
        KAFKAZKHOSTS=`curl -u admin:PASSWORD -G "http://headnodehost:8080/api/v1/clusters/kafka-BASENAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")'`
    
    Ersetzen Sie __PASSWORD__ durch das Administratorkennwort, das Sie beim Erstellen des Clusters verwendet haben. Ersetzen Sie __BASENAME__ durch den Basisnamen, den Sie beim Erstellen des Clusters verwendet haben.

    Mit diesem Befehl werden die Werte für die Zookeeper-Hosts aus Ambari ausgelesen und in der Variablen KAFKAZKHOSTS gespeichert. Verwenden Sie Folgendes, um diese Werte anzuzeigen:

        echo $KAFKAZKHOSTS
    
    Die Ausgabe dieses Befehls ähnelt dem folgenden Beispiel:

        zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk3-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181

    Speichern Sie die von diesem Befehl zurückgegebenen Werte, da diese Werte verwendet werden, wenn die Topologie im Storm-Cluster gestartet wird.

    > [!NOTE]
    > In den vorherigen Befehlen wird __http://headnodehost:8080/__ verwendet, um eine direkte Verbindung mit Ambari herzustellen. Wenn Sie diese Informationen von außerhalb eines Clusters über das Internet abrufen müssen, müssen Sie stattdessen __https://kafka-BASENAME/__ verwenden.

3. Nutzen Sie den folgenden Befehl, um in Kafka ein Thema zu erstellen:
   
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic stormtest --zookeeper $KAFKAZKHOSTS
   
    Mit diesem Befehl wird eine Verbindung mit Zookeeper hergestellt, indem die in `$KAFKAZKHOSTS` gespeicherten Informationen verwendet werden. Anschließend wird ein Kafka-Thema mit dem Namen **stormtest** erstellt. Sie können überprüfen, ob das Thema erstellt wurde, indem Sie den folgenden Befehl zum Auflisten von Themen verwenden:
   
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
   
    In der Ausgabe dieses Befehls werden Kafka-Themen aufgelistet, und das neue Thema **stormtest** sollte in der Liste enthalten sein.

Lassen Sie die SSH-Verbindung mit dem Kafka-Cluster aktiv. Sie können damit überprüfen, ob die Storm-Topologie Nachrichten an das Thema schreibt.

## <a name="download-and-compile-the-project"></a>Herunterladen und Kompilieren des Projekts

1. Laden Sie das Projekt in Ihrer Entwicklungsumgebung unter [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka) herunter, öffnen Sie eine Befehlszeile, und ändern Sie die Verzeichnisse gemäß dem Speicherort, an den Sie das Projekt heruntergeladen haben.
   
    Nehmen Sie sich kurz Zeit, um den Code zu prüfen und sich mit der Funktionsweise des Projekts vertraut zu machen.

2. Verwenden Sie im Verzeichnis **hdinsight-storm-java-kafka** den folgenden Befehl, um das Projekt zu kompilieren und ein Paket für die Bereitstellung zu erstellen:
   
        mvn clean package
   
    Mit dem Paketprozess wird eine Datei mit dem Namen `KafkaTopology-1.0-SNAPSHOT.jar` im Verzeichnis `target` erstellt.

3. Verwenden Sie die folgenden Befehle, um das Paket in Ihren Cluster vom Typ „Storm in HDInsight“ zu kopieren. Ersetzen Sie **USERNAME** durch den SSH-Benutzernamen für den Cluster. Ersetzen Sie **BASENAME** durch den Basisnamen, den Sie beim Erstellen des Clusters verwendet haben.
   
        scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
   
    Geben Sie nach der entsprechenden Aufforderung das Kennwort ein, das Sie beim Erstellen der Cluster verwendet haben.

4. Verwenden Sie den folgenden Befehl, um die Datei `set-env-variables.sh` aus dem Verzeichnis `scripts` des Projekts in den Storm-Cluster zu kopieren:

        scp ./scripts/set-env-variables.sh USERNAME@storm-BASENAME-ssh.azurehdinsight.net:set-env-variables.sh
    
    Dieses Skript wird zum Festlegen der Umgebungsvariablen verwendet, die von den Storm-Topologien zum Kommunizieren mit dem Kafka-Cluster genutzt werden.

## <a name="start-the-writer"></a>Starten der Writer-Topologie

1. Verwenden Sie Folgendes, um per SSH eine Verbindung mit dem Storm-Cluster herzustellen. Ersetzen Sie **USERNAME** durch den SSH-Benutzernamen, der beim Erstellen des Clusters verwendet wurde. Ersetzen Sie **BASENAME** durch den Basisnamen, der beim Erstellen des Clusters verwendet wurde.
   
        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
   
    Geben Sie nach der entsprechenden Aufforderung das Kennwort ein, das Sie beim Erstellen der Cluster verwendet haben.
   
    Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:
   
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basiertem HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie für die SSH-Verbindung mit dem Storm-Cluster die folgenden Befehle, um das Skript `set-env-variables.sh` auszuführen:

        chmod +x set-env-variables.sh
        . ./set-env-variables.sh KAFKACLUSTERNAME PASSWORD

    Ersetzen Sie __KAFKACLUSTERNAME__ durch den Namen des Kafka-Clusters. Ersetzen Sie __PASSWORD__ durch das Administrator-Anmeldekennwort für den Kafka-Cluster.

    Mit dem Skript wird eine Verbindung mit dem Kafka-Cluster hergestellt und eine Liste mit dem Kafka-Broker und den Zookeeper-Hosts abgerufen. Die Informationen werden dann in Umgebungsvariablen gespeichert, die von den Storm-Topologien verwendet werden.

    Die Ausgabe des Skripts ähnelt dem folgenden Beispiel:

        Checking for jq: install ok installed
        Exporting variables:
        $KAFKATOPIC=stormtest
        $KAFKABROKERS=wn0-storm.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:9092,wn1-storm.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:9092
        $KAFKAZKHOSTS=zk1-storm.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:2181,zk3-storm.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:2181,zk5-storm.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:2181

3. Verwenden Sie für die SSH-Verbindung mit dem Storm-Cluster den folgenden Befehl, um die Writer-Topologie zu starten:
   
        storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml -e
   
    Für diesen Befehl werden die folgenden Parameter verwendet:
   
    * **org.apache.storm.flux.Flux**: Flux wird verwendet, um diese Topologie zu konfigurieren und auszuführen.
   
    * **--remote**: Die Topologie wird an Nimbus übermittelt. Die Topologie wird auf die Workerknoten im Cluster verteilt.
   
    * **-R /writer.yaml**: **writer.yaml** wird verwendet, um die Topologie zu konfigurieren. `-R` gibt an, dass diese Ressource in der JAR-Datei enthalten ist. Da sie im Stamm der JAR-Datei enthalten ist, lautet der Pfad `/writer.yaml`.
   
    * **-e**: Dient zum Verwenden der Ersetzung von Umgebungsvariablen. Flux verwendet die Werte von $KAFKABROKERS und $KAFKATOPIC, die Sie zuvor festgelegt haben, und setzt sie in der Datei „reader.yaml“ anstelle der Einträge `${ENV-KAFKABROKER}` und `${ENV-KAFKATOPIC}` ein.

5. Wechseln Sie nach dem Starten der Topologie zur SSH-Verbindung mit dem Kafka-Cluster, und verwenden Sie den folgenden Befehl zum Anzeigen von Nachrichten, die in das Thema **stormtest** geschrieben werden:
   
         /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtest
   
    Für diesen Befehl wird ein Skript, das in Kafka enthalten ist, zum Überwachen des Themas genutzt. Nach kurzer Zeit sollten zufällige Sätze zurückgegeben werden, die in das Thema geschrieben wurden. Die Ausgabe sieht in etwa wie das folgende Beispiel aus:
   
        i am at two with nature             
        an apple a day keeps the doctor away
        snow white and the seven dwarfs     
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        four score and seven years ago      
        snow white and the seven dwarfs     
        snow white and the seven dwarfs     
        i am at two with nature             
        an apple a day keeps the doctor away
   
    Verwenden Sie STRG+C, um das Skript zu beenden.

## <a name="start-the-reader"></a>Starten der Reader-Topologie

1. Verwenden Sie für die SSH-Sitzung mit dem Storm-Cluster den folgenden Befehl, um die Reader-Topologie zu starten:
   
        storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml -e

2. Öffnen Sie nach dem Starten der Topologie die Storm-Benutzeroberfläche. Diese Webbenutzeroberfläche befindet sich unter „https://storm-BASENAME.azurehdinsight.net/stormui“. Ersetzen Sie __BASENAME__ durch den Basisnamen, der beim Erstellen des Clusters verwendet wurde. 

    Geben Sie nach der entsprechenden Aufforderung den Administrator-Anmeldenamen (Standard: `admin`) und das Kennwort ein, der bzw. das beim Erstellen des Clusters verwendet wurde. Es wird eine Webseite angezeigt, die in etwa wie folgt aussieht:

    ![Storm-Benutzeroberfläche](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. Wählen Sie auf der Storm-Benutzeroberfläche im Abschnitt mit der __Topologiezusammenfassung__ den Link __kafka-reader__ aus, um Informationen zur __kafka-reader__-Topologie anzuzeigen.

    ![Abschnitt mit Topologiezusammenfassung der Storm-Webbenutzeroberfläche](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. Wählen Sie im Abschnitt __Bolts (All time)__ (Bolts (Gesamte Zeit)) den Link __logger-bolt__ aus, um Informationen zu den Instanzen der logger-bolt-Komponente anzuzeigen.

    ![Link „logger-bolt“ im Abschnitt „Bolts“](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. Wählen Sie im Abschnitt __Executors__ einen Link in der Spalte __Port__ aus, um Protokollierungsinformationen zu dieser Instanz der Komponente anzuzeigen.

    ![Link unter „Executors“](./media/hdinsight-apache-storm-with-kafka/executors.png)

    Das Protokoll enthält die Protokollierung der Daten, die aus dem Kafka-Thema gelesen werden. Die Informationen im Protokoll ähneln dem folgenden Text:

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] the cow jumped over the moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: the cow jumped over the moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps the doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps the doctor away

## <a name="stop-the-topologies"></a>Beenden der Topologien

Verwenden Sie in einer SSH-Sitzung für den Storm-Cluster die folgenden Befehle, um die Storm-Topologien zu beenden:

    storm kill kafka-writer
    storm kill kafka-reader

## <a name="delete-the-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Da mit den Schritten in diesem Dokument beide Cluster in derselben Azure-Ressourcengruppe erstellt werden, können Sie die Ressourcengruppe im Azure-Portal löschen. Hierbei werden alle Ressourcen, die beim Durcharbeiten dieses Dokuments erstellt wurden, sowie das virtuelle Azure-Netzwerk und das von den Clustern verwendete Speicherkonto entfernt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispieltopologien, die mit Storm in HDInsight verwendet werden können, finden Sie unter [Beispiel-Storm-Topologien und -Komponenten für Apache Storm auf HDInsight](hdinsight-storm-example-topology.md).

Informationen zur Bereitstellung und Überwachung von Topologien in Linux-basiertem HDInsight finden Sie unter [Bereitstellen und Verwalten von Apache Storm-Topologien in Linux-basiertem HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).




<!--HONumber=Nov16_HO3-->


