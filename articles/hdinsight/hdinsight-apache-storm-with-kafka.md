---
title: "Verwenden von Apache Kafka mit Storm in HDInsight – Azure | Microsoft-Dokumentation"
description: "Apache Kafka wird mit Apache Storm in HDInsight installiert. Es wird beschrieben, wie Sie in Kafka schreiben und dann Daten auslesen, indem Sie die von Storm bereitgestellten Komponenten KafkaBolt und KafkaSpout verwenden. Außerdem erfahren Sie, wie Sie das Flux-Framework einsetzen, um Storm-Topologien zu definieren und zu übermitteln."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: e8895ef3c11aea48513e4060a20f5f49b11fc961
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a>Verwenden von Apache Kafka (Vorschau) mit Storm in HDInsight

Erfahren Sie, wie Sie mithilfe von Apache Storm Lese- und Schreibvorgänge in Apache Kafka ausführen. In diesem Beispiel wird außerdem erläutert, wie Sie Daten aus einer Storm-Topologie in das von HDInsight verwendete HDFS-kompatible Dateisystem speichern.

> [!NOTE]
> Mit den Schritten in diesem Dokument wird eine Azure-Ressourcengruppe erstellt, die sowohl ein Cluster vom Typ „Storm in HDInsight“ als auch vom Typ „Kafka in HDInsight“ enthält. Diese Cluster befinden sich beide in einem virtuellen Azure-Netzwerk, damit der Storm-Cluster direkt mit dem Kafka-Cluster kommunizieren kann.
> 
> Denken Sie nach dem Ausführen der Schritte in diesem Dokument daran, die Cluster zu löschen, um das Anfallen von Gebühren zu verhindern.

## <a name="get-the-code"></a>Abrufen des Codes

Der Code für das in diesem Dokument verwendete Beispiel ist unter [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka) verfügbar.

Zum Kompilieren dieses Projekts benötigen Sie die folgende Konfiguration für Ihre Entwicklungsumgebung:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder höher HDInsight 3.5 oder höher erfordert Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* Einen SSH-Client (Sie benötigen die Befehle `ssh` und `scp`) – weitere Informationen finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* Ein Text-Editor oder eine IDE.

Bei der Installation von Java und dem JDK auf Ihrer Entwicklungsworkstation können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* `JAVA_HOME` – sollte auf das Verzeichnis verweisen, in dem das JDK installiert ist.
* `PATH` – sollte die folgenden Pfade enthalten:
  
    * `JAVA_HOME` (oder entsprechender Pfad)
    * `JAVA_HOME\bin` (oder entsprechender Pfad)
    * Das Verzeichnis, in dem Maven installiert ist

## <a name="create-the-clusters"></a>Erstellen von Clustern

Apache Kafka in HDInsight ermöglicht keinen Zugriff auf die Kafka-Broker über das öffentliche Internet. Komponenten, die mit Kafka kommunizieren, müssen sich jeweils in demselben virtuellen Azure-Netzwerk wie die Knoten im Kafka-Cluster befinden. Für dieses Beispiel sind die Kafka- und Storm-Cluster in einem virtuellen Azure-Netzwerk angeordnet. Im folgenden Diagramm ist dargestellt, wie der Kommunikationsfluss zwischen den Clustern abläuft:

![Diagramm mit Storm- und Kafka-Clustern in einem virtuellen Azure-Netzwerk](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> Auf andere Dienste im Cluster, beispielsweise SSH und Ambari, kann über das Internet zugegriffen werden. Weitere Informationen zu den öffentlichen Ports, die für HDInsight verfügbar sind, finden Sie unter [Von HDInsight verwendete Ports und URIs](hdinsight-hadoop-port-settings-for-services.md).

Es ist zwar möglich, ein virtuelles Azure-Netzwerk, Kafka-Cluster und Storm-Cluster manuell zu erstellen, aber mit einer Azure Resource Manager-Vorlage ist dies erheblich einfacher. Führen Sie die folgenden Schritte aus, um ein virtuelles Azure-Netzwerk und Kafka- und Storm-Cluster für Ihr Azure-Abonnement bereitzustellen.

1. Verwenden Sie die folgende Schaltfläche, um sich bei Azure anzumelden, und öffnen Sie die Vorlage im Azure-Portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Die Azure Resource Manager-Vorlage befindet sich unter **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**. Er erstellt die folgenden Ressourcen:
    
    * Azure-Ressourcengruppe
    * Azure Virtual Network
    * Azure-Speicherkonto
    * Kafka in HDInsight Version 3.6 (drei Workerknoten)
    * Storm in HDInsight Version 3.6 (drei Workerknoten)

  > [!WARNING]
  > Um die Verfügbarkeit von Kafka in HDInsight zu gewährleisten, muss der Cluster mindestens drei Workerknoten enthalten. Diese Vorlage erstellt einen Kafka-Cluster, der drei Workerknoten enthält.

2. Verwenden Sie die folgende Anleitung, um die Einträge auf dem Blatt **Benutzerdefinierte Bereitstellung** einzufügen:
   
    ![Benutzerdefinierte HDInsight-Bereitstellung](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * **Ressourcengruppe**: Erstellen Sie eine Gruppe, oder wählen Sie eine vorhandene Gruppe aus. Diese Gruppe enthält den HDInsight-Cluster.
   
    * **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus.

    * **Base Cluster Name** (Cluster-Basisname): Dieser Wert wird als Basisname für Storm- und Kafka-Cluster verwendet. Wenn Sie beispielsweise **hdi** eingeben, wird ein Storm-Cluster mit dem Namen **storm-hdi** und ein Kafka-Cluster mit dem Namen **kafka-hdi** erstellt.
   
    * **Benutzername für Clusteranmeldung**: Der Administratorbenutzername für die Storm- und Kafka-Cluster.
   
    * **Kennwort für Clusteranmeldung**: Das Administratorbenutzerkennwort für die Storm- und Kafka-Cluster.
    
    * **SSH-Benutzername**: Der SSH-Benutzer, der für die Storm- und Kafka-Cluster erstellt wird.
    
    * **SSH-Kennwort**: Das Kennwort für den SSH-Benutzer für die Storm- und Kafka-Cluster.

3. Lesen Sie die **Geschäftsbedingungen**, und wählen Sie anschließend die Option **Ich stimme den oben genannten Geschäftsbedingungen zu**.

4. Aktivieren Sie zum Schluss **An Dashboard anheften**, und wählen Sie dann **Kaufen** aus. Das Erstellen der Cluster dauert ca. 20 Minuten.

Sobald die Ressourcen erstellt wurden, wird das Blatt für die Ressourcengruppe angezeigt.

![Ressourcengruppenblatt für vnet und Cluster](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> Beachten Sie, dass die Namen der HDInsight-Cluster **storm-BASENAME** und **kafka-BASENAME** lauten, wobei BASENAME der Name ist, den Sie für die Vorlage angegeben haben. Sie verwenden diese Namen in späteren Schritten, wenn Sie eine Verbindung mit den Clustern herstellen.

## <a name="understanding-the-code"></a>Grundlegendes zum Code

Dieses Projekt enthält zwei Topologien:

* **KafkaWriter**: Diese Topologie wird mit der Datei **writer.yaml** definiert. Sie schreibt zufällige Sätze in Kafka, indem sie die KafkaBolt-Komponente von Apache Storm verwendet.

    Für diese Topologie wird eine benutzerdefinierte **SentenceSpout**-Komponente zum Generieren von zufälligen Sätzen verwendet.

* **KafkaReader**: Diese Topologie wird mit der Datei **reader.yaml** definiert. Sie liest Daten aus Kafka, indem sie die KafkaSpout-Komponente von Apache Storm verwendet, und protokolliert die Daten dann in stdout.

    In dieser Topologie werden mithilfe der HdfsBolt-Komponente von Storm Daten in den Standardspeicher für den Storm-Cluster geschrieben.
### <a name="flux"></a>Flux

Die Topologien werden mit [Flux](https://storm.apache.org/releases/1.1.0/flux.html) definiert. Flux wurde mit Storm 0.10.x eingeführt und ermöglicht Ihnen die Trennung der Topologiekonfiguration vom Code. Für Topologien, die das Flux-Framework nutzen, wird die Topologie in einer YAML-Datei definiert. Die YAML-Datei kann als Teil der Topologie aufgenommen werden. Sie kann auch eine eigenständige Datei sein, die beim Übermitteln der Topologie verwendet wird. Außerdem unterstützt Flux die Variablenersetzung zur Laufzeit, die in diesem Beispiel verwendet wird.

Zur Laufzeit werden die folgenden Parameter für diese Topologien festgelegt:

* `${kafka.topic}`: Der Name des Kafka-Themas, für das die Topologien die Lese-/Schreibvorgänge durchführen

* `${kafka.broker.hosts}`: Die Hosts, auf denen die Kafka-Broker ausgeführt werden Die Brokerinformationen werden von der KafkaBolt-Komponente verwendet, wenn auf Kafka geschrieben wird.

* `${kafka.zookeeper.hosts}`: Die Hosts, auf denen Zookeeper im Kafka-Cluster ausgeführt wird

Weitere Informationen zu Flux-Topologien finden Sie unter [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="download-and-compile-the-project"></a>Herunterladen und Kompilieren des Projekts

1. Laden Sie das Projekt in Ihrer Entwicklungsumgebung unter [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka) herunter, öffnen Sie eine Befehlszeile, und ändern Sie die Verzeichnisse gemäß dem Speicherort, an den Sie das Projekt heruntergeladen haben.

2. Verwenden Sie im Verzeichnis **hdinsight-storm-java-kafka** den folgenden Befehl, um das Projekt zu kompilieren und ein Paket für die Bereitstellung zu erstellen:

  ```bash
  mvn clean package
  ```

    Mit dem Paketprozess wird eine Datei mit dem Namen `KafkaTopology-1.0-SNAPSHOT.jar` im Verzeichnis `target` erstellt.

3. Verwenden Sie die folgenden Befehle, um das Paket in Ihren Cluster vom Typ „Storm in HDInsight“ zu kopieren. Ersetzen Sie **USERNAME** durch den SSH-Benutzernamen für den Cluster. Ersetzen Sie **BASENAME** durch den Basisnamen, den Sie beim Erstellen des Clusters verwendet haben.

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    Geben Sie nach der entsprechenden Aufforderung das Kennwort ein, das Sie beim Erstellen der Cluster verwendet haben.

## <a name="configure-the-topology"></a>Konfigurieren der Topologie

1. Ermitteln Sie die Kafka-Brokerhosts mit einer der folgenden Methoden:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > Beim Bash-Beispiel wird davon ausgegangen, dass `$CLUSTERNAME` den Namen des HDInsight-Clusters enthält. Außerdem wird angenommen, das [jq](https://stedolan.github.io/jq/) installiert ist. Geben Sie bei entsprechender Aufforderung das Kennwort des Anmeldekontos für den Cluster ein.

    Der zurückgegebene Wert ähnelt dem folgenden Text:

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > Obwohl möglicherweise mehr als zwei Brokerhosts für den Cluster vorhanden sind, müssen Sie keine vollständige Liste aller Hosts für Clients angeben. Ein oder zwei genügen.

2. Ermitteln Sie die Kafka Zookeeper-Hosts mit einer der folgenden Methoden:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > Beim Bash-Beispiel wird davon ausgegangen, dass `$CLUSTERNAME` den Namen des HDInsight-Clusters enthält. Außerdem wird angenommen, das [jq](https://stedolan.github.io/jq/) installiert ist. Geben Sie bei entsprechender Aufforderung das Kennwort des Anmeldekontos für den Cluster ein.

    Der zurückgegebene Wert ähnelt dem folgenden Text:

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > Obwohl mehr als zwei Zookeeper-Knoten vorhanden sind, müssen Sie keine vollständige Liste aller Hosts für Clients angeben. Ein oder zwei genügen.

    Speichern Sie diesen Wert, da er später wieder verwendet wird.

3. Bearbeiten Sie die Datei `dev.properties` im Stammverzeichnis des Projekts. Fügen Sie die Broker- und Zookeeper-Hostinformationen in die entsprechenden Zeilen in dieser Datei ein. Das folgende Beispiel wird mithilfe der Beispielwerte aus den vorherigen Schritten konfiguriert:

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. Speichern Sie die Datei `dev.properties`, und laden Sie sie anschließend mit dem folgenden Befehl in den Storm-Cluster hoch:

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    Ersetzen Sie **USERNAME** durch den SSH-Benutzernamen für den Cluster. Ersetzen Sie **BASENAME** durch den Basisnamen, den Sie beim Erstellen des Clusters verwendet haben.

## <a name="start-the-writer"></a>Starten der Writer-Topologie

1. Verwenden Sie Folgendes, um per SSH eine Verbindung mit dem Storm-Cluster herzustellen. Ersetzen Sie **USERNAME** durch den SSH-Benutzernamen, der beim Erstellen des Clusters verwendet wurde. Ersetzen Sie **BASENAME** durch den Basisnamen, der beim Erstellen des Clusters verwendet wurde.

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    Geben Sie nach der entsprechenden Aufforderung das Kennwort ein, das Sie beim Erstellen der Cluster verwendet haben.
   
    Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Führen Sie über die SSH-Verbindung den folgenden Befehl aus, um das von der Topologie verwendete Kafka-Thema zu erstellen:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    Ersetzen Sie `$KAFKAZKHOSTS` durch die im vorherigen Abschnitt abgerufenen Zookeeper-Hostinformationen.

2. Verwenden Sie für die SSH-Verbindung mit dem Storm-Cluster den folgenden Befehl, um die Writer-Topologie zu starten:

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    Für diesen Befehl werden die folgenden Parameter verwendet:

    * `org.apache.storm.flux.Flux`: Flux wird verwendet, um diese Topologie zu konfigurieren und auszuführen.

    * `--remote`: Die Topologie wird an Nimbus übermittelt. Die Topologie wird auf die Workerknoten im Cluster verteilt.

    * `-R /writer.yaml`: Die Datei `writer.yaml` wird verwendet, um die Topologie zu konfigurieren. `-R` gibt an, dass diese Ressource in der JAR-Datei enthalten ist. Da sie im Stamm der JAR-Datei enthalten ist, lautet der Pfad `/writer.yaml`.

    * `--filter`: Einträge in der Topologie `writer.yaml` werden mit Werten in der Datei `dev.properties` aufgefüllt. Der Wert des Eintrags `kafka.topic` in der Datei wird beispielsweise verwendet, um den Eintrag `${kafka.topic}` in der Topologiedefinition zu ersetzen.

5. Überprüfen Sie nach dem Start der Topologie mit folgendem Befehl, ob Daten in das Kafka-Thema geschrieben werden:

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    Ersetzen Sie `$KAFKAZKHOSTS` durch die im vorherigen Abschnitt abgerufenen Zookeeper-Hostinformationen.

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

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. Öffnen Sie nach dem Starten der Topologie die Storm-Benutzeroberfläche. Diese Webbenutzeroberfläche befindet sich unter „https://storm-BASENAME.azurehdinsight.net/stormui“. Ersetzen Sie __BASENAME__ durch den Basisnamen, der beim Erstellen des Clusters verwendet wurde. 

    Geben Sie nach der entsprechenden Aufforderung den Administrator-Anmeldenamen (Standard: `admin`) und das Kennwort ein, der bzw. das beim Erstellen des Clusters verwendet wurde. Die daraufhin angezeigte Webseite ähnelt der folgenden Abbildung:

    ![Storm-Benutzeroberfläche](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. Wählen Sie auf der Storm-Benutzeroberfläche im Abschnitt mit der __Topologiezusammenfassung__ den Link __kafka-reader__ aus, um Informationen zur __kafka-reader__-Topologie anzuzeigen.

    ![Abschnitt mit Topologiezusammenfassung der Storm-Webbenutzeroberfläche](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. Klicken Sie im Abschnitt __Bolts (All time)__ (Bolts (Gesamte Zeit)) auf den Link __logger-bolt__, um Informationen zu den Instanzen der logger-bolt-Komponente anzuzeigen.

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

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-the-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Da mit den Schritten in diesem Dokument beide Cluster in derselben Azure-Ressourcengruppe erstellt werden, können Sie die Ressourcengruppe im Azure-Portal löschen. Beim Löschen der Ressourcengruppe werden alle Ressourcen entfernt, die anhand dieses Dokuments erstellt wurden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispieltopologien, die mit Storm in HDInsight verwendet werden können, finden Sie unter [Beispiel-Storm-Topologien und -Komponenten für Apache Storm auf HDInsight](hdinsight-storm-example-topology.md).

Informationen zur Bereitstellung und Überwachung von Topologien in Linux-basiertem HDInsight finden Sie unter [Bereitstellen und Verwalten von Apache Storm-Topologien in Linux-basiertem HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).
