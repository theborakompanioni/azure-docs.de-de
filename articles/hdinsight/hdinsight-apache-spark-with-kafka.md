---
title: "Apache Spark-Streaming mit Kafka – Azure HDInsight | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Apache Spark in HDInsight verwenden, um Daten in Apache Kafka-Cluster in HDInsight zu lesen und zu schreiben. In diesem Beispiel wird Scala in einem Jupyter-Notebook verwendet, um Daten nach Kafka in HDInsight zu schreiben und sie mithilfe von Spark-Streaming wieder einzulesen.
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
ms.date: 05/15/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: ceff0df193b3356ed2a23f381ea65369063957b1
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---
# <a name="use-apache-spark-with-kafka-preview-on-hdinsight"></a>Verwenden von Apache Spark mit Kafka (Vorschau) in HDInsight

Erfahren Sie, wie Sie Spark verwenden. Apache Spark kann verwendet werden, um Daten in oder aus Apache Kafka zu streamen. In diesem Dokument erfahren Sie, wie Sie Daten in oder aus Kafka mit einem Jupyter-Notebook von Spark in HDInsight streamen können.

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
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Azure Resource Manager-Vorlage befindet sich unter **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.json**.

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
> Beachten Sie, dass die Namen der HDInsight-Cluster **spark-BASENAME** und **kafka-BASENAME** lauten, wobei BASENAME der Name ist, den Sie für die Vorlage angegeben haben. Sie werden diese Namen in späteren Schritten verwenden, wenn Sie eine Verbindung mit den Clustern herstellen.

## <a name="get-the-code"></a>Abrufen des Codes

Der Code für das in diesem Dokument beschriebene Beispiel ist unter [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka) verfügbar.

## <a name="understand-the-code"></a>Grundlegendes zum Code

In diesem Beispiel wird eine Scala-Anwendung mit Jupyter-Notebook verwendet. Der Code in diesem Notebook basiert auf folgenden Datenelementen:

* __Kafka-Broker__: Der Brokerprozess wird auf jedem Workerknoten im Kafka-Cluster ausgeführt. Die Brokerliste wird von der Producer-Komponente benötigt, die Daten in Kafka schreibt.

* __Zookeeper-Hosts__: Die Zookeeper-Hosts für den Kafka-Cluster werden verwendet, wenn Daten von Kafka genutzt (gelesen) werden.

* __Themenname__: Der Name des Themas, zu dem Daten geschrieben und gelesen wurden. In diesem Beispiel wird von einem Thema namens `sparktest` ausgegangen.

Weitere Informationen darüber, wie Sie Informationen zum Kafka-Broker und zum Zookeeper-Host erhalten, finden Sie unter [Informationen zum Kafka-Host](#kafkahosts).

Mit dem Code im Notebook werden die folgenden Aufgaben ausgeführt:

* Es wird ein Consumer erstellt, der Daten aus dem Kafka-Thema `sparktest` liest, die Wörter in den Daten zählt und die Wörter und die Wortanzahl in der temporären Tabelle `wordcounts` speichert.

* Es wird ein Producer erstellt, der Sätze per Zufallsprinzip in das Kafka-Thema `sparktest` schreibt.

* Es werden Daten aus der `wordcounts`-Tabelle ausgewählt, um die Zählung anzuzeigen.

Jede Zelle innerhalb des Projekts enthält Kommentare oder einen Textabschnitt, in dem erläutert wird, was der Code bewirkt.

## <a id="kafkahosts"></a>Informationen zum Kafka-Host

Wenn Sie eine Anwendung für Kafka in HDInsight erstellen, sollten Sie zunächst die Informationen zum Kafka-Broker und zum Zookeeper-Host für den Kafka-Cluster abrufen. Diese werden von den Clientanwendungen für die Kommunikation mit Kafka verwendet.

> [!NOTE]
> Der Kafka-Broker und die Zookeeper-Hosts sind nicht direkt über das Internet zugänglich. Jede Anwendung, die Kafka verwendet, muss entweder im Kafka-Cluster oder im selben virtuellen Azure-Netzwerk wie der Kafka-Cluster ausgeführt werden. In diesem Fall wird das Beispiel in einem Spark-Cluster in HDInsight im selben virtuellen Netzwerk ausgeführt.

Verwenden Sie in Ihrer Entwicklungsumgebung die folgenden Befehle, um die Broker- und Zookeeper-Informationen abzurufen:

* So rufen Sie die Informationen zum __Kafka-Broker__ ab:

    ```bash
    curl -u admin:$PASSWORD -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")'
    ```

    > [!NOTE]
    > Legen Sie `$PASSWORD` auf das Anmeldekennwort (Administrator) fest, das Sie beim Erstellen des Clusters verwendet haben. Legen Sie `$CLUSTERNAME` auf den Basisnamen fest, das Sie beim Erstellen des Clusters verwendet haben.

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    > [!NOTE]
    > Legen Sie `$cluterName` auf den Namen des HDInsight-Clusters fest. Geben Sie bei der entsprechenden Aufforderung das Kennwort des Anmeldekontos (Administrator) für den Cluster ein.

* So rufen Sie die Informationen zum __Zookeeper-Host__ ab:

    ```bash
    curl -u admin:$PASSWORD -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")'
    ```

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

Beide Befehle geben Informationen ähnlich dem folgenden Text zurück:

* __Kafka-Broker__: `wn0-kafka.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:9092,wn1-kafka.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:9092`

* __Zookeeper-Hosts__: `zk0-kafka.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:2181,zk1-kafka.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:2181,zk2-kafka.4rf4ncirvydube02fuj0gpxp4e.ex.internal.cloudapp.net:2181`

> [!IMPORTANT]
> Speichern Sie dieses Informationen, weil sie in verschiedenen Schritten in diesem Dokument verwendet werden.

## <a name="use-the-jupyter-notebook"></a>Verwenden des Jupyter-Notebooks

Um das Jupyter-Beispielnotebook zu verwenden, müssen Sie es auf den Jupyter-Notebookserver im Spark-Cluster hochladen. Führen Sie die folgenden Schritte aus, um das Notebook hochzuladen:

1. Verwenden Sie Ihrem Webbrowser die folgende URL, um eine Verbindung mit dem Jupyter-Notebookserver im Spark-Cluster herzustellen. Ersetzen Sie `CLUSTERNAME` durch den Namen Ihres Spark-Clusters.

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Geben Sie bei Aufforderung den Clusterbenutzernamen (Administrator) und das Kennwort ein, den bzw. das Sie beim Erstellen des Clusters verwendet haben.

2. Klicken Sie oben rechts auf der Seite auf die Schaltfläche __Hochladen__, um die `KafkaStreaming.ipynb`-Datei hochzuladen. Wählen Sie die Datei im Dialogfeld zur Dateiauswahl aus, und wählen Sie __Öffnen__.

    ![Verwenden der Schaltfläche „Hochladen“ für die Auswahl und das Hochladen eines Notebooks](./media/hdinsight-apache-spark-with-kafka/upload-button.png)

    ![Auswahl der KafkaStreaming.ipynb-Datei](./media/hdinsight-apache-spark-with-kafka/select-notebook.png)

3. Suchen Sie den Eintrag __KafkaStreaming.ipynb__ in der Liste der Notebooks, und klicken Sie auf die Schaltfläche __Hochladen__ neben dem Eintrag.

    ![Verwenden der Schaltfläche „Hochladen“ neben dem Eintrag „KafkaStreaming.ipynb“ zum Hochladen der Datei auf den Notebookserver](./media/hdinsight-apache-spark-with-kafka/upload-notebook.png)

4. Sobald die Datei hochgeladen wurde, wählen Sie den Eintrag __KafkaStreaming.ipynb__, um das Notebook zu öffnen. Um dieses Beispiel abzuschließen, folgen Sie den Anweisungen im Notebook.

## <a name="delete-the-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Da mit den Schritten in diesem Dokument beide Cluster in derselben Azure-Ressourcengruppe erstellt werden, können Sie die Ressourcengruppe im Azure-Portal löschen. Durch das Löschen der Gruppe werden alle beim Durcharbeiten dieses Dokuments erstellten Ressourcen sowie das virtuelle Azure-Netzwerk und das von den Clustern verwendete Speicherkonto entfernt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Spark verwendet wird, um in Kafka Lese- und Schreibvorgänge auszuführen. Verwenden Sie die folgenden Links, um weitere Möglichkeiten zur Arbeit mit Kafka kennenzulernen:

* [Erste Schritte mit Apache Kafka in HDInsight](hdinsight-apache-kafka-get-started.md)
* [Verwenden von MirrorMaker zum Erstellen eines Replikats von Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Verwenden von Apache Storm mit Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)


