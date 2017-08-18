---
title: "Dokumentation für Azure HDInsight – Handbuch zur Problembehandlung | Microsoft-Dokumentation"
description: "Behandeln Sie Probleme mit Hadoop-Workloads auf HDInsight. Die Dokumentation zeigt Ihnen Schritt für Schritt, wie Sie häufig auftretende Probleme mit Hive, Spark, HBase, Storm und Kafka auf HDInsight lösen."
services: hdinsight
author: arijitt
manager: arijitt
layout: LandingPage
ms.assetid: 
ms.service: hdinsight
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing - page
ms.date: 7/06/2017
ms.author: arijitt
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 9313f0713818f80bae99c32a438d97ba544a7a6b
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---


# <a name="azure-hdinsight-troubleshooting"></a>Azure HDInsight-Problembehandlung

| Apache-Workload | Wichtige Fragen |
|---|---|
|![HBASE](./media/hdinsight-troubleshoot-guide/HBASE.png)<br>[Problembehandlung bei HBASE](hdinsight-troubleshoot-hbase.md)|<br>[Wie führe ich hbck-Befehlsberichte mit mehreren nicht zugewiesenen Regionen aus?](hdinsight-troubleshoot-hbase.md#how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions)<br><br>[Wie behebe ich Timeoutprobleme mit hbck-Befehlen für Regionszuweisungen?](hdinsight-troubleshoot-hbase.md#how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments)<br><br>[Wie erzwinge ich das Deaktivieren des abgesicherten HDFS-Modus in einem Cluster?](hdinsight-troubleshoot-hbase.md#how-do-i-force-disable-hdfs-safe-mode-in-an-cluster)<br><br>[Wie behebe ich JDBC- oder sqlline-Konnektivitätsprobleme mit Apache Phoenix?](hdinsight-troubleshoot-hbase.md#how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix)<br><br>[Was ist die Ursache dafür, dass ein Masterserver nicht startet?](hdinsight-troubleshoot-hbase.md#what-causes-a-master-server-to-fail-to-start)<br><br>[Was verursacht einen Neustartfehler bei einem Regionsserver?](hdinsight-troubleshoot-hbase.md#what-causes-a-restart-failure-on-a-region-server)|
|![HDFS](./media/hdinsight-troubleshoot-guide/HDFS.png)<br>[Problembehandlung bei HDFS](hdinsight-troubleshoot-hdfs.md)|<br>[Wie greife ich aus einem Cluster heraus auf lokales HDFS zu?](hdinsight-troubleshoot-hdfs.md#how-do-i-access-local-hdfs-from-inside-a-cluster)<br><br>[Wie erzwinge ich das Deaktivieren des abgesicherten HDFS-Modus in einem Cluster?](hdinsight-troubleshoot-hdfs.md#how-do-i-force-disable-hdfs-safe-mode-in-a-cluster)|
|![HIVE](./media/hdinsight-troubleshoot-guide/HIVE.png)<br>[Problembehandlung bei HIVE](hdinsight-troubleshoot-hive.md)|<br>[Wie exportiere ich einen Hive-Metastore und importiere ihn in einen anderen Cluster?](hdinsight-troubleshoot-hive.md#how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster)<br><br>[Wie finde ich Hive-Protokolle auf einem Cluster?](hdinsight-troubleshoot-hive.md#how-do-locate-hive-logs-on-a-cluster)<br><br>[Wie starte ich die Hive-Shell mit bestimmten Konfigurationen auf einem Cluster?](hdinsight-troubleshoot-hive.md#how-do-i-launch-hive-shell-with-specific-configurations-on-a-cluster)<br><br>[Wie analysiere ich Tez DAG-Daten (Directed Acyclic Graph, gerichteter azyklischer Graph) in einem clusterkritischen Pfad?](hdinsight-troubleshoot-hive.md#how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path)<br><br>[Wie lade ich Tez DAG-Daten (Directed Acyclic Graph, gerichteter azyklischer Graph) aus einem Cluster herunter?](hdinsight-troubleshoot-hive.md#how-do-i-download-tez-dag-data-from-a-cluster)|
|![SPARK](./media/hdinsight-troubleshoot-guide/SPARK.png)<br>[Problembehandlung bei Spark](hdinsight-troubleshoot-SPARK.md)|<br>[Wie konfiguriere ich eine Spark-Anwendung über Ambari auf Clustern?](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-through-ambari-on-clusters)<br><br>[Wie konfiguriere ich eine Spark-Anwendung über ein Jupyter-Notebook auf Clustern?](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-through-a-jupyter-notebook-on-clusters)<br><br>[Wie konfiguriere ich eine Spark-Anwendung über LIVY auf Clustern?](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-through-livy-on-clusters)<br><br>[Wie konfiguriere ich eine Spark-Anwendung über spark-submit auf Clustern?](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-through-spark-submit-on-clusters)<br><br>[Wodurch wird eine OutOfMemoryError-Ausnahme in einer Spark-Anwendung verursacht?](hdinsight-troubleshoot-spark.md#what-causes-a-spark-application-outofmemoryerror-exception)|
|![STORM](./media/hdinsight-troubleshoot-guide/STORM.png)<br>[Problembehandlung bei STORM](hdinsight-troubleshoot-STORM.md)|<br>[Wie greife ich auf die Storm-Benutzeroberfläche in einem Cluster zu?](hdinsight-troubleshoot-storm.md#how-do-i-access-storm-ui-on-a-cluster)<br><br>[Wie übertrage ich Storm-EventHub-Spout-Prüfpunktinformationen von einer Topologie zu einer anderen?](hdinsight-troubleshoot-storm.md#how-do-i-transfer-storm-eventhub-spout-checkpoint-information-from-one-topology-to-another)<br><br>[Wie finde ich Storm-Binärdateien auf einem Cluster?](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-binaries-on-a-cluster)<br><br>[Wie bestimme ich die Bereitstellungstopologie eines Storm-Clusters?](hdinsight-troubleshoot-storm.md#how-do-i-determine-the-deployment-topology-of-a-storm-cluster)<br><br>[Wie finde ich Storm-EventHub-Spout-Binärdateien für die Entwicklung?](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-eventhub-spout-binaries-for-development)<br><br>[Wie finde ich Storm-Log4J-Konfigurationsdateien auf Clustern?](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-log4j-configuration-files-on-clusters)|
|![YARN](./media/hdinsight-troubleshoot-guide/YARN.png)<br>[Problembehandlung bei YARN](hdinsight-troubleshoot-YARN.md)|<br>[Wie erstelle ich eine neue Yarn-Warteschlange auf einem Cluster?](hdinsight-troubleshoot-yarn.md#how-do-i-create-a-new-yarn-queue-on-a-cluster)<br><br>[Wie lade ich Yarn-Protokolle aus einem Cluster herunter?](hdinsight-troubleshoot-yarn.md#how-do-i-download-yarn-logs-from-a-cluster)|

## <a name="hdinsight-troubleshooting-resources"></a>Ressourcen zur HDInsight-Problembehandlung
Weitere Hilfe zur Problembehandlung finden Sie in folgenden Artikeln.

| Informationen über | Finden Sie hier |
| --- | --- |
| HDInsight unter Linux und Optimierung | [Informationen zur Verwendung von HDInsight unter Linux](hdinsight-hadoop-linux-information.md)<br>[Hadoop-Arbeitsspeicher- und Leistung](hdinsight-hadoop-stack-trace-error-messages.md)<br>[Leistung von Hive-Abfragen](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) |
| Protokolle und Speicherabbilder | [Zugriff auf Hadoop YARN-Anwendungsprotokolle unter Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)<br>[Aktivieren von Heapdumps für Hadoop-Dienste](hdinsight-hadoop-collect-debug-heap-dump-linux.md)|
| Fehler | [Verstehen und Beheben von WebHCat-Fehlern](hdinsight-hadoop-templeton-webhcat-debug-errors.md)<br>[Hive-Einstellungen: Beheben von Fehlern vom Typ „Nicht genügend Arbeitsspeicher“](hdinsight-hadoop-hive-out-of-memory-error-oom.md) |
| Tools | [Debuggen von Tez-Aufträgen mithilfe von Ambari-Ansichten](hdinsight-debug-ambari-tez-view.md)<br>[Optimieren von Hive-Abfragen](hdinsight-hadoop-optimize-hive-query.md) |



