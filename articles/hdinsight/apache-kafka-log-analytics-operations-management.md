---
title: "Log Analytics mit Apache Kafka – Azure HDInsight | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie mithilfe der Operations Management Suite Protokolle aus Apache Kafka-Clustern in Azure HDInsight analysieren.
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
ms.date: 09/11/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: be549c8fd072663ae9bbb9441a654a2bf1912198
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="analyze-logs-for-apache-kafka-preview-on-hdinsight"></a>Analysieren von Protokollen für Apache Kafka (Vorschau) in HDInsight

Erfahren Sie, wie Sie mithilfe der Microsoft Operations Management Suite Protokolle, die von Apache Kafka generiert wurden, in HDInsight analysieren.

## <a name="enable-oms-for-kafka"></a>Aktivieren der OMS für Kafka

Die Schritte zum Aktivieren von Log Analytics für HDInsight sind für alle HDInsight-Cluster identisch. Verwenden Sie die folgenden Links, um zu verstehen, wie die erforderlichen Dienste erstellt und konfiguriert werden:

1. Erstellen Sie einen Log Analytics-Arbeitsbereich. Weitere Informationen finden Sie im Dokument [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-get-started.md).

2. Erstellen Sie ein Kafka-Cluster im HDInsight-Cluster. Weitere Informationen finden Sie unter [Einstieg in Apache Kafka in HDInsight](hdinsight-apache-kafka-get-started.md).

3. Konfigurieren Sie das Kafka-Cluster für die Verwendung von Log Analytics. Weitere Informationen finden Sie im Dokument [Überwachen von HDInsight mit Log Analytics](hdinsight-hadoop-oms-log-analytics-tutorial.md).

    > [!NOTE]
    > Sie können auch das Cluster konfigurieren, um Log Analytics mit dem Cmdlet `Enable-AzureRmHDInsightOperationsManagementSuite` zu verwenden. Für dieses Cmdlet benötigen Sie die folgenden Informationen:
    >
    > * Den HDInsight-Clusternamen
    > * Die Arbeitsbereichs-ID für Log Analytics. Sie finden die Arbeitsbereichs-ID im OMS-Arbeitsbereich für den Log Analytics-Arbeitsbereich.
    > * Den Primärschlüssel für die OMS-Verbindung. Um den primären Schlüssel zu suchen, wählen Sie Ihre Log Analytics-Instanz und dann das __OMS-Portal__ aus. Wählen Sie im OMS-Portal die Option __Einstellungen__, __Verbundene Quellen__ und dann __Linux-Server__.


> [!IMPORTANT]
> Es dauert ca. 20 Minuten, bis die Daten für Log Analytics zur Verfügung stehen.

## <a name="query-logs"></a>Abfrageprotokolle

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Log Analytics-Arbeitsbereich aus.

2. Wählen Sie __Protokollsuche__ aus. In dieser Ansicht können Sie die von Kafka gesammelten Daten suchen. Nachfolgend sehen Sie einige Beispielsuchen:

    * Datenträgerauslastung: `Type=Perf ObjectName="Logical Disk" (CounterName="Free Megabytes")  InstanceName="_Total" Computer='hn*-*' or Computer='wn*-*' | measure avg(CounterValue) by   Computer interval 1HOUR`
    * CPU-Auslastung: `Type:Perf CounterName="% Processor Time" InstanceName="_Total" Computer='hn*-*' or Computer='wn*-*' | measure avg(CounterValue) by Computer interval 1HOUR`
    * Eingehende Nachrichten pro Sekunde: `Type=metrics_kafka_CL ClusterName_s="your_kafka_cluster_name" InstanceName_s="kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | measure avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s interval 1HOUR`
    * Eingehende Bytes pro Sekunde: `Type=metrics_kafka_CL HostName_s="wn0-kafka" InstanceName_s="kafka-BrokerTopicMetrics-BytesInPerSec-Count" | measure avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) interval 1HOUR`
    * Ausgehende Bytes pro Sekunde: `Type=metrics_kafka_CL ClusterName_s="your_kafka_cluster_name" InstanceName_s="kafka-BrokerTopicMetrics-BytesOutPerSec-Count" |  measure avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) interval 1HOUR`

    > [!IMPORTANT]
    > Ersetzen Sie die Abfragewerte durch die jeweiligen Informationen Ihres Clusters. Beispielsweise muss `ClusterName_s` auf den Namen Ihres Clusters festgelegt werden. `HostName_s` muss auf den Domänennamen eines Workerknotens im Cluster festgelegt werden.

    Sie können auch `*` eingeben, um alle protokollierte Typen zu suchen. Derzeit sind folgende Protokolle für Abfragen verfügbar:

    | Protokolltyp | Beschreibung |
    | ---- | ---- |
    | log\_kafkaserver\_CL | server.log des Kafka-Brokers |
    | log\_kafkacontroller\_CL | controller.log des Kafka-Brokers |
    | metrics\_kafka\_CL | Kafka-JMX-Metriken |

    ![Abbildung der Suche nach der CPU-Auslastung](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Log Analytics finden Sie im Dokument [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-get-started.md).

Weitere Informationen zum Arbeiten mit Kafka finden Sie in den folgenden Dokumenten:

 * [Spiegeln von Kafka zwischen HDInsight-Clustern](hdinsight-apache-kafka-mirroring.md)
 * [Erhöhen der Skalierbarkeit von Kafka in HDInsight](hdinsight-apache-kafka-scalability.md)
 * [Verwenden von Spark-Streaming (DStreams) mit Kafka](hdinsight-apache-spark-with-kafka.md)
 * [Verwenden von strukturiertem Spark-Streaming mit Kafka](hdinsight-apache-kafka-spark-structured-streaming.md)
