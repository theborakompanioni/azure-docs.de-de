---
title: Spiegeln von Apache Kafka im HDInsight-Cluster | Microsoft Docs
description: "Es wird beschrieben, wie Sie die Spiegelungsfunktion von Kafka verwenden, um ein Replikat von Kafka in einem HDInsight-Cluster aufzubewahren, indem Sie die Themen in einem sekundären Cluster spiegeln."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: e1c99bbe9d6317d83cc5e71ca4f79d862223aa0a
ms.openlocfilehash: 6110432cba7703b95342c9e8bf3b71cb0a377ad0

---
# <a name="use-mirrormaker-to-create-a-replica-of-a-kafka-on-hdinsight-cluster-preview"></a>Verwenden von MirrorMaker zum Erstellen eines Replikats von Kafka in einem HDInsight-Cluster (Vorschau)

Apache Kafka enthält eine Spiegelungsfunktion, mit der Sie Themen von einem Kafka-Cluster in einem anderen replizieren können. Beispiel: Replikation von Datensätzen zwischen Kafka-Clustern in unterschiedlichen Azure-Regionen.

Die Spiegelung kann als fortlaufender Prozess ausgeführt oder zu bestimmten Zeitpunkten als Methode zum Migrieren von Daten aus einem Cluster in einen anderen verwendet werden.

> [!WARNING]
> Die Spiegelung sollte nicht als Mittel zum Erzielen von Fehlertoleranz angesehen werden. Der Versatz von Elementen in einem Thema unterscheidet sich für Quell- und Zielcluster, sodass diese für Clients nicht austauschbar sind.
> 
> Falls Sie Bedenken wegen der Fehlertoleranz haben, sollten Sie die Replikation für die Themen in Ihrem Cluster festlegen. Weitere Informationen finden Sie unter [Erste Schritte mit Apache Kafka (Vorschau) in HDInsight](hdinsight-apache-kafka-get-started.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein virtuelles Azure-Netzwerk: Die Kafka-Quell- und -Zielcluster müssen direkt miteinander kommunizieren können. HDInsight macht Kafka-APIs öffentlich im Internet verfügbar, sodass die Quell- und Zielcluster in demselben virtuellen Azure-Netzwerk vorhanden sein müssen.

* Zwei Kafka-Cluster: In diesem Dokument wird eine Azure Resource Manager-Vorlage verwendet, um zwei Cluster vom Typ „Kafka in HDInsight“ in einem virtuellen Azure-Netzwerk zu erstellen.

## <a name="how-does-mirroring-work"></a>Wie funktioniert die Spiegelung?

Für die Spiegelung wird das Tool MirrorMaker (Teil von Apache Kafka) verwendet, um Datensätze aus Themen im Quellcluster zu nutzen und anschließend eine lokale Kopie im Zielcluster zu erstellen. MirrorMaker nutzt einen (oder mehrere) *Consumer*, mit denen aus dem Quellcluster gelesen wird, und einen *Producer*, der in den lokalen Cluster (Zielcluster) schreibt.

Im folgenden Diagramm ist der Spiegelungsprozess dargestellt:

![Diagramm des Spiegelungsprozesses](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

Die Quell- und Zielcluster können sich in Bezug auf die Anzahl von Knoten und Partitionen unterscheiden, und auch der Versatz in den Themen ist unterschiedlich. Beim Spiegeln wird der Schlüsselwert beibehalten, der für die Partitionierung verwendet wird, sodass die Datensatzreihenfolge pro Schlüssel beibehalten wird.

### <a name="mirroring-between-networks"></a>Spiegelung zwischen Netzwerken

Wenn Sie eine Spiegelung zwischen Kafka-Clustern in unterschiedlichen Netzwerken durchführen müssen, sollten Sie außerdem Folgendes beachten:

* **Gateways**: Die Netzwerke müssen auf TCP/IP-Ebene kommunizieren können.

* **Namensauflösung**: Die Kafka-Cluster in jedem Netzwerk müssen per Hostname eine Verbindung miteinander herstellen können. Hierfür ist unter Umständen ein DNS-Server (Domain Name System) in jedem Netzwerk erforderlich, der dafür konfiguriert ist, Anforderungen an die anderen Netzwerke weiterzuleiten. 
  
    Beim Erstellen eines virtuellen Azure-Netzwerks müssen Sie einen benutzerdefinierten DNS-Server und die IP-Adresse für den Server angeben, anstatt das automatisch bereitgestellte DNS des Netzwerks zu verwenden. Nach der Erstellung des virtuellen Netzwerks müssen Sie dann einen virtuellen Azure-Computer erstellen, für den diese IP-Adresse verwendet wird, und anschließend die DNS-Software darauf installieren und konfigurieren.
  
    > [!WARNING]
    > Erstellen und konfigurieren Sie den benutzerdefinierten DNS-Server, bevor Sie HDInsight im virtuellen Netzwerk installieren. Es ist keine zusätzliche Konfiguration erforderlich, damit HDInsight den für das virtuelle Netzwerk konfigurierten DNS-Server verwenden kann.

Weitere Informationen zum Verbinden von zwei virtuellen Azure-Netzwerken finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-kafka-clusters"></a>Erstellen von Kafka-Clustern

Apache Kafka in HDInsight ermöglicht keinen Zugriff auf den Kafka-Dienst über das öffentliche Internet. Komponenten, die mit Kafka kommunizieren, müssen sich jeweils in demselben virtuellen Azure-Netzwerk wie die Knoten im Kafka-Cluster befinden. Für dieses Beispiel sind die Kafka-Quell- und -Zielcluster in einem virtuellen Azure-Netzwerk angeordnet. Im folgenden Diagramm ist dargestellt, wie der Kommunikationsfluss zwischen den Clustern abläuft:

![Diagramm mit Kafka-Quell- und -Zielclustern in einem virtuellen Azure-Netzwerk](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

> [!NOTE]
> Kafka selbst ist zwar auf die Kommunikation innerhalb des virtuellen Netzwerks beschränkt, aber auf andere Dienste im Cluster, z.B. SSH und Ambari, kann über das Internet zugegriffen werden. Weitere Informationen zu den öffentlichen Ports, die für HDInsight verfügbar sind, finden Sie unter [Von HDInsight verwendete Ports und URIs](hdinsight-hadoop-port-settings-for-services.md).

Es ist zwar möglich, ein virtuelles Azure-Netzwerk und Kafka-Cluster manuell zu erstellen, aber mit einer Azure Resource Manager-Vorlage ist dies erheblich einfacher. Führen Sie die folgenden Schritte aus, um ein virtuelles Azure-Netzwerk und zwei Kafka-Cluster für Ihr Azure-Abonnement bereitzustellen.

1. Verwenden Sie die folgende Schaltfläche, um sich bei Azure anzumelden, und öffnen Sie die Vorlage im Azure-Portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Die Azure Resource Manager-Vorlage befindet sich unter **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet.json**.

2. Verwenden Sie die folgenden Informationen, um die Einträge auf dem Blatt **Benutzerdefinierte Bereitstellung** aufzufüllen:
    
    ![Benutzerdefinierte HDInsight-Bereitstellung](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * **Ressourcengruppe**: Erstellen Sie eine Gruppe, oder wählen Sie eine vorhandene Gruppe aus. Diese Gruppe enthält den HDInsight-Cluster.

    * **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus. Der Standort muss mit dem Standort im Abschnitt __EINSTELLUNGEN__ übereinstimmen.
     
    * **Base Cluster Name** (Cluster-Basisname): Dieser Wert wird als Basisname für den Kafka-Cluster verwendet. Beispiel: Mit der Eingabe von **hdi** werden Cluster mit den Namen **source-hdi** und **dest-hdi** erstellt.

    * **Benutzername für Clusteranmeldung**: Administratorbenutzername für die Kafka-Quell- und -Zielcluster.

    * **Kennwort für Clusteranmeldung**: Das Administratorbenutzerkennwort für die Kafka-Quell- und -Zielcluster.

    * **SSH-Benutzername**: Der SSH-Benutzer, der für die Kafka-Quell- und -Zielcluster erstellt werden soll.

    * **SSH-Kennwort**: Das Kennwort für den SSH-Benutzer für die Kafka-Quell- und -Zielcluster.

    * **Standort**: Die Region, in der die Cluster erstellt werden.

3. Lesen Sie die **Geschäftsbedingungen**, und wählen Sie anschließend die Option **Ich stimme den oben genannten Geschäftsbedingungen zu**.

4. Aktivieren Sie zum Schluss **An Dashboard anheften**, und wählen Sie dann **Kaufen** aus. Das Erstellen der Cluster dauert ca. 20 Minuten.

Sobald die Ressourcen erstellt sind, werden Sie zu einem Blatt für die Ressourcengruppe weitergeleitet, das die Cluster und das Webdashboard enthält.

![Ressourcengruppenblatt für vnet und Cluster](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> Beachten Sie, dass die Namen der HDInsight-Cluster **source-BASENAME** und **dest-BASENAME** lauten, wobei BASENAME der Name ist, den Sie für die Vorlage angegeben haben. Sie verwenden diese Namen in späteren Schritten, wenn Sie eine Verbindung mit den Clustern herstellen.

## <a name="create-topics"></a>Erstellen von Themen

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem **Quell**cluster her:
   
        ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
   
    Ersetzen Sie **sshuser** durch den SSH-Benutzernamen, den Sie beim Erstellen des Clusters verwendet haben. Ersetzen Sie **BASENAME** durch den Basisnamen, der beim Erstellen des Clusters verwendet wurde.
   
    Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:
   
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
   
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie den folgenden Befehl, um nach den Zookeeper-Hosts zu suchen, die Variable `SOURCE_ZKHOSTS` festzulegen und dann mehrere neue Themen mit dem Namen `testtopic` zu erstellen:
   
        # Get a list of zookeeper hosts for the source cluster
        SOURCE_ZKHOSTS=`grep -R zk /etc/hadoop/conf/yarn-site.xml | grep 2181 | grep -oPm1 "(?<=<value>)[^<]+"`
        # Create a topic on the source cluster
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS

3. Verwenden Sie den folgenden Befehl, um zu bestätigen, dass das Thema erstellt wurde:
   
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
   
    Die Antwort enthält `testtopic`.

4. Verwenden Sie Folgendes, um die Informationen zum Zookeeper-Host für diesen Cluster (**Quell**cluster) anzuzeigen:
   
        echo $SOURCE_ZKHOSTS
   
    Die Ausgabe sieht in etwa wie folgt aus:
   
        zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk6-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181
   
    Speichern Sie diese Informationen. Sie werden im nächsten Abschnitt verwendet.

## <a name="configure-mirroring"></a>Konfigurieren der Spiegelung

1. Stellen Sie per SSH eine Verbindung mit dem **Ziel**cluster her:
   
        ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
   
    Ersetzen Sie **sshuser** durch den SSH-Benutzernamen, den Sie beim Erstellen des Clusters verwendet haben. Ersetzen Sie **BASENAME** durch den Basisnamen, der beim Erstellen des Clusters verwendet wurde.
   
    Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:
   
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie den folgenden Befehl, um eine Datei mit dem Namen `consumer.config` zu erstellen. Mit der Datei wird beschrieben, wie Sie mit dem **Quell**cluster kommunizieren:
   
        nano consumer.config
   
    Verwenden Sie als Inhalt der Datei `consumer.config` den folgenden Text:
   
        zookeeper.connect=SOURCE_ZKHOSTS
        group.id=mirrorgroup
   
    Ersetzen Sie **SOURCE_ZKHOSTS** durch die Informationen zu Zookeeper-Hosts aus dem **Quell**cluster.
   
    In dieser Datei werden die Consumerinformationen beschrieben, die beim Lesen vom Kafka-Quellcluster verwendet werden sollten. Weitere Informationen zur Consumerkonfiguration finden Sie unter [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) (Consumerkonfigurationen) bei „kafka.apache.org“.
   
    Drücken Sie zum Speichern der Datei **STRG+X**, **Y** und dann die EINGABETASTE.

3. Bevor Sie den Producer konfigurieren, der mit dem Zielcluster kommuniziert, müssen Sie nach den Brokerhosts für den **Ziel**cluster suchen. Verwenden Sie die folgenden Befehle, um diese Informationen abzurufen:
   
        # Install JQ for parsing JSON documents
        sudo apt -y install jq
        # Get the broker information for the destination cluster
        DEST_BROKERHOSTS=`sudo bash -c 'ls /var/lib/ambari-agent/data/command-[0-9]*.json' | tail -n 1 | xargs sudo cat | jq -r '["\(.clusterHostInfo.kafka_broker_hosts[]):9092"] | join(",")'`
        # Display the information
        echo $DEST_BROKERHOSTS
   
    Diese Befehle geben etwa die folgenden Informationen zurück:
   
        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. Verwenden Sie Folgendes, um eine Datei mit dem Namen `producer.config` zu erstellen. Mit der Datei wird beschrieben, wie Sie mit dem **Ziel**cluster kommunizieren:
   
        bootstrap.servers=DEST_BROKERS
        compression.type=none
   
    Ersetzen Sie **DEST_BROKERS** durch die Brokerinformationen aus dem vorherigen Schritt. 
   
    Weitere Informationen zur Producerkonfiguration finden Sie unter [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) (Producerkonfigurationen) bei „kafka.apache.org“.

## <a name="start-mirrormaker"></a>Starten von MirrorMaker

1. Verwenden Sie für den Weg von der SSH-Verbindung zum **Ziel**cluster den folgenden Befehl, um den MirrorMaker-Prozess zu starten:
   
        /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
   
    Die in diesem Beispiel verwendeten Parameter sind:
   
    * **--consumer.config**: Gibt die Datei an, in der die Consumereigenschaften enthalten sind. Diese Eigenschaften werden verwendet, um einen Consumer zu erstellen, mit dem aus dem Kafka-*Quell*cluster gelesen wird.

    * **--producer.config**: Gibt die Datei an, in der die Producereigenschaften enthalten sind. Diese Eigenschaften werden zum Erstellen eines Producers verwendet, mit dem in den Kafka-*Ziel*cluster geschrieben wird.

    * **--whitelist**: Eine Liste mit Themen, die von MirrorMaker aus dem Quellcluster am Zielort repliziert werden.
    
    * **--num.streams**: Die Anzahl von Consumerthreads, die erstellt werden sollen.
     
    Nach dem Starten gibt MirrorMaker Informationen zurück, die dem folgenden Text ähneln:
        
    ```{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}  metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}  metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. From the SSH connection to the **source** cluster, use the following command to start a producer and send messages to the topic:
   
        # Install JQ for working with JSON
        sudo apt -y install jq
        # Retrieve the Kafka brokers
        SOURCE_BROKERHOSTS=`sudo bash -c 'ls /var/lib/ambari-agent/data/command-[0-9]*.json' | tail -n 1 | xargs sudo cat | jq -r '["\(.clusterHostInfo.kafka_broker_hosts[]):9092"] | join(",")'`
        # Start a producer
        /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
   
    When you arrive at a blank line with a cursor, type in a few text messages. These are sent to the topic on the **source** cluster. When done, use **Ctrl + C** to end the producer process.

3. From the SSH connection to the **destination** cluster, use **Ctrl + C** to end the MirrorMaker process. Then use the following commands to verify that the `testtopic` topic was created, and that data in the topic was replicated to this mirror:
   
        # Get a list of zookeeper hosts for the destination cluster
        DEST_ZKHOSTS=`grep -R zk /etc/hadoop/conf/yarn-site.xml | grep 2181 | grep -oPm1 "(?<=<value>)[^<]+"`
        # List topics on destination
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
        # Retrieve messages from the `testtopic`
        /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
   
    The list of topics will now include `testtopic`, which is created when MirrorMaster mirrors the topic from the source cluster to the destination. The messages retrieved from the topic are the same as entered on the source cluster.

## Delete the cluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal. Deleting the resource group removes all resources created by following this document, the Azure Virtual Network, and storage account used by the clusters.

## Next Steps

In this document, you learned how to use MirrorMaker to create a replica of a Kafka cluster. Use the following links to discover other ways to work with Kafka:

* [Apache Kafka MirrorMaker documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) at cwiki.apache.org.
* [Get started with Apache Kafka on HDInsight](hdinsight-apache-kafka-get-started.md)
* [Use Apache Spark with Kafka on HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Use Apache Storm with Kafka on HDInsight](hdinsight-apache-storm-with-kafka.md)




<!--HONumber=Nov16_HO3-->


