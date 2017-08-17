---
title: "Apache Spark-Streaming mit Kafka – Azure HDInsight | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Apache Spark verwenden, um Daten mithilfe von DStreams in oder aus Apache Kafka zu streamen. In diesem Beispiel werden Sie Daten mithilfe eines Jupyter Notebooks aus Spark auf HDInsight streamen.
keywords: Kafka Beispiel,Kafka ZooKeeper,Spark Streaming Kafka,Spark Streaming Kafka Beispiel
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd8f53c1-bdee-4921-b683-3be4c46c2039
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 81fa319f6fb94bdabacd8f68d14b9a1063a9749a
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a>Beispiel für Apache Spark-Streaming (DStream) Kafka (Vorschauversion) auf HDInsight

Erfahren Sie, wie Sie Apache Spark verwenden, um Daten mithilfe von DStreams in oder aus Apache Kafka auf HDInsight zu streamen. In diesem Beispiel wird ein auf dem Spark-Cluster ausgeführtes Jupyter Notebook verwendet.
> [!NOTE]
> Mit den in diesem Dokument beschriebenen Schritten wird eine Azure-Ressourcengruppe erstellt, die jeweils einen Spark- und einen Kafka-Cluster in HDInsight beinhaltet. Die Cluster befinden sich innerhalb eines virtuellen Azure-Netzwerks, wodurch Spark- und Kafka-Cluster direkt miteinander kommunizieren können.
>
> Denken Sie nach dem Ausführen der Schritte in diesem Dokument daran, die Cluster zu löschen, um das Anfallen von Gebühren zu verhindern.

## <a name="create-the-clusters"></a>Erstellen von Clustern

Apache Kafka in HDInsight ermöglicht keinen Zugriff auf die Kafka-Broker über das öffentliche Internet. Komponenten, die mit Kafka kommunizieren, müssen sich jeweils im selben virtuellen Azure-Netzwerk befinden wie die Knoten im Kafka-Cluster. Für dieses Beispiel sind die Kafka- und Spark-Cluster in einem virtuellen Azure-Netzwerk angeordnet. Im folgenden Diagramm ist dargestellt, wie der Kommunikationsfluss zwischen den Clustern abläuft:

![Diagramm der Spark- und Kafka-Cluster in einem virtuellen Azure-Netzwerk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Kafka selbst ist zwar auf die Kommunikation innerhalb des virtuellen Netzwerks beschränkt, aber auf andere Dienste im Cluster, z.B. SSH und Ambari, kann über das Internet zugegriffen werden. Weitere Informationen zu den öffentlichen Ports, die für HDInsight verfügbar sind, finden Sie unter [Von HDInsight verwendete Ports und URIs](hdinsight-hadoop-port-settings-for-services.md).

Es ist zwar möglich, ein virtuelles Azure-Netzwerk, einen Kafka-Cluster und einen Spark-Cluster manuell zu erstellen, aber mit einer Azure Resource Manager-Vorlage ist dies erheblich einfacher. Führen Sie die folgenden Schritte aus, um ein virtuelles Azure-Netzwerk, Kafka- und Spark-Cluster für Ihr Azure-Abonnement bereitzustellen.

1. Verwenden Sie die folgende Schaltfläche, um sich bei Azure anzumelden, und öffnen Sie die Vorlage im Azure-Portal.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Azure Resource Manager-Vorlage befindet sich unter **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > Um die Verfügbarkeit von Kafka in HDInsight zu gewährleisten, muss der Cluster mindestens drei Workerknoten enthalten. Diese Vorlage erstellt einen Kafka-Cluster, der drei Workerknoten enthält.

    Mit dieser Vorlage wird ein HDInsight 3.6-Cluster für Kafka und Spark erstellt.

2. Verwenden Sie die folgenden Informationen, um die Einträge auf dem Blatt **Benutzerdefinierte Bereitstellung** aufzufüllen:
   
    ![Benutzerdefinierte HDInsight-Bereitstellung](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Ressourcengruppe**: Erstellen Sie eine Gruppe, oder wählen Sie eine vorhandene Gruppe aus. Diese Gruppe enthält den HDInsight-Cluster.

    * **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus.

    * **Basisclustername**: Dieser Wert wird als Basisname für Spark- und Kafka-Cluster verwendet. Wenn Sie beispielsweise **hdi** eingeben, wird ein Spark-Cluster mit dem Namen spark-hdi__ und ein Kafka-Cluster mit dem Namen **kafka-hdi** erstellt.

    * **Benutzername für Clusteranmeldung**: Administratorbenutzername für die Spark- und Kafka-Cluster.

    * **Kennwort für Clusteranmeldung**: Administratorbenutzerkennwort für die Spark- und Kafka-Cluster.

    * **SSH-Benutzername**: SSH-Benutzer, der für die Spark- und Kafka-Cluster erstellt wird.

    * **SSH-Kennwort**: Kennwort für den SSH-Benutzer für die Spark- und Kafka-Cluster.

3. Lesen Sie die **Geschäftsbedingungen**, und wählen Sie anschließend die Option **Ich stimme den oben genannten Geschäftsbedingungen zu**.

4. Aktivieren Sie zum Schluss **An Dashboard anheften**, und wählen Sie dann **Kaufen** aus. Das Erstellen der Cluster dauert ca. 20 Minuten.

Sobald die Ressourcen erstellt sind, werden Sie zu einem Blatt für die Ressourcengruppe weitergeleitet, das die Cluster und das Webdashboard enthält.

![Ressourcengruppenblatt für vnet und Cluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Beachten Sie, dass die Namen der HDInsight-Cluster **spark-BASENAME** und **kafka-BASENAME** lauten, wobei BASENAME der Name ist, den Sie für die Vorlage angegeben haben. Sie verwenden diese Namen in späteren Schritten, wenn Sie eine Verbindung mit den Clustern herstellen.

## <a name="use-the-notebooks"></a>Verwenden der Notebooks

Der Code für das in diesem Dokument beschriebene Beispiel ist unter [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka) verfügbar.

Führen Sie die Schritte in der Datei `README.md` aus, um dieses Beispiel zu durchzuführen.

## <a name="delete-the-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Da mit den Schritten in diesem Dokument beide Cluster in derselben Azure-Ressourcengruppe erstellt werden, können Sie die Ressourcengruppe im Azure-Portal löschen. Durch das Löschen der Gruppe werden alle beim Durcharbeiten dieses Dokuments erstellten Ressourcen sowie das virtuelle Azure-Netzwerk und das von den Clustern verwendete Speicherkonto entfernt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Beispiel haben Sie erfahren, wie Spark verwendet wird, um in Kafka Lese- und Schreibvorgänge auszuführen. Verwenden Sie die folgenden Links, um weitere Möglichkeiten zur Arbeit mit Kafka kennenzulernen:

* [Erste Schritte mit Apache Kafka in HDInsight](hdinsight-apache-kafka-get-started.md)
* [Verwenden von MirrorMaker zum Erstellen eines Replikats von Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Verwenden von Apache Storm mit Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)


