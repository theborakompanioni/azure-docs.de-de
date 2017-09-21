---
title: "Strukturiertes Apache Spark-Streaming mit Kafka – Azure HDInsight | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Apache Spark-Streaming (DStream) verwenden, um Daten in oder aus Apache Kafka zu streamen. In diesem Beispiel streamen Sie Daten mithilfe eines Jupyter Notebooks aus Spark auf HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 565f840e0ac7ec1a68361ac1613da5d42459c979
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a>Verwenden Sie strukturiertes Spark-Streaming mit Kafka (Vorschau) auf HDInsight

Erfahren Sie, wie Sie strukturiertes Spark-Streaming verwenden, um Daten aus Apache Kafka auf Azure HDInsight zu lesen.

Strukturiertes Spark-Streaming ist ein auf Spark SQL basierendes Stream-Verarbeitungsmodul. Sie können damit Streamingberechnungen genauso wie Batchberechnung auf statischen Daten ausdrücken. Weitere Informationen zu strukturiertem Streaming finden Sie unter [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) (Programmierhandbuch zu strukturiertem Streaming [Alpha]) auf Apache.org.

> [!IMPORTANT]
> In diesem Beispiel wird Spark 2.1 auf HDInsight 3.6 verwendet. Strukturiertes Streaming gilt auf Spark 2.1 als __Alpha__.
>
> Mit den in diesem Dokument beschriebenen Schritten wird eine Azure-Ressourcengruppe erstellt, die jeweils einen Spark- und einen Kafka-Cluster in HDInsight beinhaltet. Die Cluster befinden sich innerhalb eines virtuellen Azure-Netzwerks, wodurch Spark- und Kafka-Cluster direkt miteinander kommunizieren können.
>
> Denken Sie nach dem Ausführen der Schritte in diesem Dokument daran, die Cluster zu löschen, um das Anfallen von Gebühren zu verhindern.

## <a name="create-the-clusters"></a>Erstellen von Clustern

Apache Kafka in HDInsight ermöglicht keinen Zugriff auf die Kafka-Broker über das öffentliche Internet. Komponenten, die mit Kafka kommunizieren, müssen sich jeweils im selben virtuellen Azure-Netzwerk befinden wie die Knoten im Kafka-Cluster. Für dieses Beispiel sind die Kafka- und Spark-Cluster in einem virtuellen Azure-Netzwerk angeordnet. Im folgenden Diagramm ist dargestellt, wie der Kommunikationsfluss zwischen den Clustern abläuft:

![Diagramm der Spark- und Kafka-Cluster in einem virtuellen Azure-Netzwerk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Der Kafka-Dienst ist auf die Kommunikation innerhalb des virtuellen Netzwerks beschränkt. Auf andere Dienste auf dem Cluster, wie z.B. SSH und Ambari, kann über das Internet zugegriffen werden. Weitere Informationen zu den öffentlichen Ports, die für HDInsight verfügbar sind, finden Sie unter [Von HDInsight verwendete Ports und URIs](hdinsight-hadoop-port-settings-for-services.md).

Es ist zwar möglich, ein virtuelles Azure-Netzwerk, einen Kafka-Cluster und einen Spark-Cluster manuell zu erstellen, aber mit einer Azure Resource Manager-Vorlage ist dies erheblich einfacher. Führen Sie die folgenden Schritte aus, um ein virtuelles Azure-Netzwerk, Kafka- und Spark-Cluster für Ihr Azure-Abonnement bereitzustellen.

1. Verwenden Sie die folgende Schaltfläche, um sich bei Azure anzumelden, und öffnen Sie die Vorlage im Azure-Portal.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Azure Resource Manager-Vorlage befindet sich unter **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    Diese Vorlage erstellt die folgenden Ressourcen:

    * Ein Kafka-auf-HDInsight-3.5-Cluster.
    * Ein Spark-auf-HDInsight-3.6-Cluster.
    * Ein Azure Virtual Network, das die HDInsight-Cluster enthält.

    > [!IMPORTANT]
    > Das in diesem Beispiel für strukturiertes Streaming verwendete Notebook benötigt Spark auf HDInsight 3.6. Bei Verwendung einer früheren Version von Spark auf HDInsight erhalten Sie Fehlermeldungen, wenn Sie das Notebook verwenden.

2. Verwenden Sie die folgenden Informationen, um die Einträge auf dem Abschnitt **Benutzerdefinierte Bereitstellung** aufzufüllen:
   
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

Sobald die Ressourcen erstellt wurden, wird eine Zusammenfassungsseite angezeigt.

![Ressourcengruppeninformationen für VNET und Cluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Beachten Sie, dass die Namen der HDInsight-Cluster **spark-BASENAME** und **kafka-BASENAME** lauten, wobei BASENAME der Name ist, den Sie für die Vorlage angegeben haben. Sie verwenden diese Namen in späteren Schritten, wenn Sie eine Verbindung mit den Clustern herstellen.

## <a name="get-the-kafka-brokers"></a>Abrufen der Kafka-Broker

Der Code in diesem Beispiel stellt eine Verbindung mit den Kafka-Broker-Hosts im Kafka-Cluster her. Verwenden Sie das folgende PowerShell- oder Bashbeispiel, um die Adressen der beiden Kafka-Broker-Hosts zu finden:

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
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Geben Sie bei der entsprechenden Aufforderung das Kennwort des Anmeldekontos (Administrator) für den Cluster ein.

> [!NOTE]
> In diesem Beispiel erwartet `$CLUSTERNAME`, den Namen des Kafka-Clusters aufzunehmen.
>
> Dieses Beispiel verwendet das [jq](https://stedolan.github.io/jq/)-Hilfsprogramm, um Daten aus dem JSON-Dokument zu analysieren.

Die Ausgabe sieht in etwa wie folgender Text aus:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Speichern Sie diese Informationen, da sie in den folgenden Abschnitten dieses Dokuments verwendet werden.

## <a name="get-the-notebooks"></a>Abrufen der Notebooks

Der Code für das in diesem Dokument beschriebene Beispiel ist unter [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming) verfügbar.

## <a name="upload-the-notebooks"></a>Hochladen der Notebooks

Befolgen Sie die folgenden Schritte, um die Notebooks aus dem Projekt auf Ihr Spark auf HDInsight-Cluster hochzuladen:

1. Verbinden Sie in Ihrem Webbrowser das Jupyter Notebook mit Ihrem Spark-Cluster. Ersetzen Sie `CLUSTERNAME` in der folgenden URL durch den Namen Ihres Kafka-Clusters:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Geben Sie bei Aufforderung den Clusterbenutzernamen (Administrator) und das Kennwort ein, den bzw. das Sie beim Erstellen des Clusters verwendet haben.

2. Klicken Sie oben rechts auf der Seite auf die Schaltfläche __Hochladen__, um die Datei __Stream-Tweets-To_Kafka.ipynb__ auf den Cluster hochzuladen. Wählen Sie __Öffnen__, um das Hochladen zu starten.

    ![Verwenden der Schaltfläche „Hochladen“ für die Auswahl und das Hochladen eines Notebooks](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Auswahl der KafkaStreaming.ipynb-Datei](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Suchen Sie den Eintrag __Stream-Tweets-To_Kafka.ipynb__ in der Liste der Notebooks, und klicken Sie auf die Schaltfläche __Hochladen__ neben dem Eintrag.

    ![Verwenden Sie zum Hochladen des Notebooks die Schaltfläche „Hochladen“ neben dem Eintrag „KafkaStreaming.ipynb“.](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. Wiederholen Sie die Schritte 1 bis 3, um das Notebook __Spark-Structured-Streaming-From-Kafka.ipynb__ zu laden.

## <a name="load-tweets-into-kafka"></a>Tweets in Kafka laden

Nachdem die Dateien hochgeladen wurden, wählen Sie den Eintrag __Stream-Tweets-To_Kafka.ipynb__, um das Notebook zu öffnen. Befolgen Sie die Schritte im Notebook, um Tweets in Kafka zu laden.

## <a name="process-tweets-using-spark-structured-streaming"></a>Tweets mithilfe von strukturiertem Spark-Streaming bearbeiten

Wählen Sie auf der Startseite des Jupyter Notebooks den Eintrag __Spark-Structured-Streaming-From-Kafka.ipynb__ aus. Befolgen Sie die Schritte im Notebook, um Tweets aus Kafka mithilfe des strukturierten Spark-Streamings zu laden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt wissen, wie Sie strukturiertes Spark-Streaming verwenden, finden Sie in den folgenden Dokumenten mehr Informationen zum Arbeiten mit Spark und Kafka:

* [How to use Spark streaming (DStream) with Kafka (Verwenden von Spark-Streaming (DStream) mit Kafka)](hdinsight-apache-spark-with-kafka.md)
* [Start with Jupyter Notebook and Spark on HDInsight (Erste Schritte mit Jupyter Notebook und Spark auf HDInsight)](hdinsight-apache-spark-jupyter-spark-sql.md)
