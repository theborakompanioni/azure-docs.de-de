---
title: "Problembehandlung bei HIVE – Azure HDInsight | Microsoft-Dokumentation"
description: "Verwenden Sie die HIVE-FAQ für Antworten auf häufig gestellte Fragen zu HIVE auf der Azure HDInsight-Plattform."
keywords: "Azure HDInsight, HIVE, FAQ, Problembehandlungshandbuch, häufig gestellte Fragen"
services: Azure HDInsight
documentationcenter: na
author: dharmeshkakadia
manager: 
editor: 
ms.assetid: 15B8D0F3-F2D3-4746-BDCB-C72944AA9252
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: dharmeshkakadia
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 67b096c0585f7b73a57b784683944052d1cfaa33
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---

# <a name="hive-troubleshooting"></a>Problembehandlung bei HIVE

Dieser Artikel beschreibt die wichtigsten Probleme und ihre Lösungen für die Arbeit mit HIVE-Nutzlasten in Apache Ambari.

## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Wie exportiere ich einen Hive-Metastore und importiere ihn in einen anderen Cluster?

### <a name="issue"></a>Problem:

Ich muss einen Hive-Metastore exportieren und ihn in einen anderen Cluster importieren.  

### <a name="resolution-steps"></a>Lösungsschritte: 

1. Stellen Sie mit einem Secure Shell-Client (SSH) eine Verbindung mit dem HDInsight-Cluster her (beachten Sie „Weitere nützliche Informationen“ im Abschnitt unten).
1. Führen Sie den folgenden Befehl auf dem HDInsight-Cluster aus, von wo aus Sie den Metastore exportieren möchten:

```apache
for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
```

So wird eine Datei namens `allatables.sql` generiert.

- Kopieren Sie die Datei `alltables.sql` in den neuen HDInsight-Cluster, und führen Sie folgenden Befehl aus:

```apache
hive -f alltables.sql
```

Dieser Code setzt voraus, dass Datenpfade auf dem neuen Cluster mit denen auf dem alten identisch sind. Wenn dies nicht der Fall ist, können Sie die manuell generierte Datei  
`alltables.sql` so bearbeiten, dass sie die Änderungen widerspiegelt.

### <a name="further-reading"></a>Weitere nützliche Informationen:

- [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)


## <a name="how-do-locate-hive-logs-on-a-cluster"></a>Wie finde ich Hive-Protokolle auf einem Cluster?

### <a name="issue"></a>Problem:

Ich muss Hive-Client-, Hive-Metastore- und Hive-Server-Protokolle auf dem HDInsight-Cluster finden.  

### <a name="resolution-steps"></a>Lösungsschritte: 

- Stellen Sie mit einem Secure Shell-Client (SSH) eine Verbindung mit dem HDInsight-Cluster her (beachten Sie „Weitere nützliche Informationen“ im Abschnitt unten).
- Hive-Client-Protokolle finden Sie unter:

```apache
/tmp/<username>/hive.log 
```

- Hive-Metastore-Protokolle finden Sie unter:

```apache
/var/log/hive/hivemetastore.log 
```

- Hive-Server-Protokolle finden Sie unter:

```apache
/var/log/hive/hiveserver2.log 
```

### <a name="further-reading"></a>Weitere nützliche Informationen:

- [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)

## <a name="how-do-i-launch-hive-shell-with-specific-configurations-on-a-cluster"></a>Wie starte ich die Hive-Shell mit bestimmten Konfigurationen auf einem Cluster?

### <a name="issue"></a>Problem:

Ich muss Hive-Shell-Konfigurationen zur Startzeit auf HDInsight-Clustern außer Kraft setzen oder angeben.  

### <a name="resolution-steps"></a>Lösungsschritte: 

- Geben Sie beim Starten der Hive-Shell ein Konfigurations-Schlüssel-Wert-Paar an (beachten Sie „Weitere nützliche Informationen“ im Abschnitt unten):

```apache
hive -hiveconf a=b 
```

- Listen Sie alle effektiven Konfigurationen auf Hive-Shell mithilfe des folgenden Befehls auf:

```apache
hive> set;
```

Verwenden Sie beispielsweise den folgenden Befehl, um Hive-Shell mit auf der Konsole aktivierter Debugprotokollierung zu starten:
             
```apache
hive -hiveconf hive.root.logger=ALL,console 
```

### <a name="further-reading"></a>Weitere nützliche Informationen:

- [Hive configuration properties](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties) (Eigenschaften der Hive-Konfiguration)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Wie analysiere ich Tez DAG-Daten (Directed Acyclic Graph, gerichteter azyklischer Graph) in einem clusterkritischen Pfad?

### <a name="issue"></a>Problem:

Ich muss Tez DAG-Daten (Directed Acyclic Graph, gerichteter azyklischer Graph) analysieren, insbesondere den kritischen Pfad auf dem HDInsight-Cluster.

### <a name="resolution-steps"></a>Lösungsschritte:
 
- Stellen Sie mit einem Secure Shell-Client (SSH) eine Verbindung mit dem HDInsight-Cluster her (beachten Sie „Weitere nützliche Informationen“ im Abschnitt unten).

- Führen Sie an der Eingabeaufforderung den folgenden Befehl aus:
   
```apache
hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
```

- Listen Sie mit dem folgenden Befehl andere Analyzer auf, die zur Analyse von Tez DAG-Daten (Directed Acyclic Graph, gerichteter azyklischer Graph) verwendet werden können:

```apache
hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
```

Ein Beispielprogramm muss als erstes Argument angegeben werden.

Gültige Programmnamen sind: ContainerReuseAnalyzer: Drucken von Details zur Containerwiederverwendung in einer DAG CriticalPath: Suchen des kritischen Pfads einer DAG LocalityAnalyzer: Drucken der Ortsdetails in einer DAG ShuffleTimeAnalyzer: Analysieren der Shufflezeitdetails in einer DAG SkewAnalyzer: Analysieren der Datenschiefedetails in einer DAG SlowNodeAnalyzer: Drucken der Knotendetails in einer DAG SlowTaskIdentifier: Drucken der Details langsamer Aufgaben in einer DAG SlowestVertexAnalyzer: Drucken der Details des langsamsten Scheitelpunkts in einer DAG SpillAnalyzer: Drucken von Überlaufdetails in einer DAG TaskConcurrencyAnalyzer: Drucken der Aufgabenparallelitätsdetails in einer DAG VertexLevelCriticalPathAnalyzer: Suchen des kritischen Pfads auf Scheitelpunktebene in einer DAG


### <a name="further-reading"></a>Weitere nützliche Informationen:

- [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)



## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Wie lade ich Tez DAG-Daten (Directed Acyclic Graph, gerichteter azyklischer Graph) aus einem Cluster herunter?

#### <a name="issue"></a>Problem:

Ich muss Tez DAG-Daten aus dem HDInsight-Cluster herunterladen.

#### <a name="resolution-steps"></a>Lösungsschritte:

Es gibt zwei Möglichkeiten zum Erfassen der Tez DAG-Daten.

- Von der Befehlszeile aus:
 
    Stellen Sie mit einem Secure Shell-Client (SSH) eine Verbindung mit dem HDInsight-Cluster her. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus:
   
```apache
hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
```

- Oder Sie können die Ambari-Tez-Ansicht verwenden:
   
Wechseln Sie zu Ambari --> wechseln Sie zur Tez-Ansicht (ausgeblendet unter Kachelnsymbol in der oberen rechten Ecke) --> Klicken Sie auf die DAG-Daten, die Sie interessieren --> Klicken Sie auf „Download data“.

#### <a name="further-reading"></a>Weitere nützliche Informationen:

1) [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)








