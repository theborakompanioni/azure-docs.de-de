---
title: "Problembehandlung bei STORM – Azure HDInsight | Microsoft-Dokumentation"
description: "Verwenden Sie die Storm-FAQ für Antworten auf häufig gestellte Fragen zu Storm auf der Azure HDInsight-Plattform."
keywords: Azure HDInsight, Storm, FAQ, Problembehandlungshandbuch, allgemeine Probleme
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: raviperi
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: b52462096ce977b94ff9514df650607b05e17030
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---

# <a name="storm-troubleshooting"></a>Problembehandlung bei STORM

Dieser Artikel beschreibt die wichtigsten Probleme und ihre Lösungen für die Arbeit mit Storm-Nutzlasten in Apache Ambari.

## <a name="how-do-i-access-storm-ui-on-a-cluster"></a>Wie greife ich auf die Storm-Benutzeroberfläche in einem Cluster zu?

### <a name="issue"></a>Problem:
Es gibt zwei Möglichkeiten, in einem Browser auf die Storm-Benutzeroberfläche zuzugreifen:

#### <a name="ambari-ui"></a>Ambari-Benutzeroberfläche
1. Navigieren zum Ambari-Dashboard
1. Auswählen von Storm aus der Liste der Dienste auf der linken Seite
1. Auswählen der Storm-Benutzeroberflächenoption im Quicklinks-Dropdownmenü

#### <a name="direct-link"></a>Direkte Verknüpfung
URL für den Zugriff auf die Storm-Benutzeroberfläche:

https://\<ClusterDnsName\>/stormui

Beispiel: https://stormcluster.azurehdinsight.net/stormui

## <a name="how-do-i-transfer-storm-eventhub-spout-checkpoint-information-from-one-topology-to-another"></a>Wie übertrage ich Storm-EventHub-Spout-Prüfpunktinformationen von einer Topologie zu einer anderen?

### <a name="issue"></a>Problem:
Wie kann beim Entwickeln von Topologien, die mithilfe der Storm-Eventhub-Spout-JAR-Datei von HDInsight aus Event Hubs lesen, eine Topologie mit dem gleichen Namen auf einem neuen Cluster bereitgestellt werden, wobei die Prüfpunktdaten, für die ein Commit auf Zookeeper ausgeführt wurde, jedoch im alten Cluster beibehalten werden?

#### <a name="where-is-checkpoint-data-stored"></a>Wo werden die Prüfpunktdaten gespeichert?
Prüfpunktdaten für Offsets werden vom EventHub-Spout unter zwei Stammpfaden in Zookeeper gespeichert:
- Nicht transaktionale Spout-Prüfpunkte werden unter „/eventhubspout“ gespeichert.
- Transaktions-Spout-Prüfpunktdaten werden unter „/transactional“ gespeichert.

#### <a name="how-to-restore"></a>Vorgehensweise beim Wiederherstellen
Die Skripts und Bibliotheken zum Exportieren von Daten aus Zookeeper und Zurückimportieren unter einem neuen Namen finden Sie unter: „https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0“.

Der Ordner „lib“ enthält die Jar-Dateien, die die Implementierung für den Import/Export-Vorgang enthalten.
Der Ordner „bash“ enthält ein Beispielskript zum Exportieren von Daten aus dem Zookeeper-Server auf dem alten Cluster und Zurückimportieren in den Zookeeper-Server auf dem neuen Cluster.

Zum Importieren/Exportieren von Daten muss das [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh)-Skript vom Zookeeper aus ausgeführt werden.
Das Skript muss aktualisiert werden, um die HDP-Versionszeichenfolge darin zu korrigieren.
(Die HDInsight-Entwickler arbeiten daran, diese Skripts generisch zu machen, sodass sie von einem beliebigen Knoten im Cluster aus ausgeführt werden können, ohne dass der Endbenutzer Änderungen vornehmen muss).

Der Exportbefehl schreibt die Metadaten am angegebenen Speicherort in einen HDFS-Pfad (ADLS- oder Blobspeicher).

### <a name="examples"></a>Beispiele

##### <a name="export-offset-metadata"></a>Exportieren von Offsetmetadaten:
1. Per SSH-Verbindung nach dem Zookeeper-Cluster auf dem alten Cluster, von dem aus der Prüfpunktoffset exportiert werden muss.
1. Führen Sie den folgenden Befehl aus (nach der Aktualisierung der HDP-Versionszeichenfolge), um Zookeeper-Offsetdaten nach dem HDFS-Pfad „/stormmetadta/zkdata“ zu exportieren.

```apache   
   java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
```

##### <a name="import-offset-metadata"></a>Importieren von Offsetmetadaten:
1. Per SSH-Verbindung nach dem Zookeeper-Cluster auf dem alten Cluster, von dem aus der Prüfpunktoffset exportiert werden muss.
1. Führen Sie den folgenden Befehl aus (nach der Aktualisierung der HDP-Versionszeichenfolge), um Zookeeper-Offsetdaten vom HDFS-Pfad „/stormmetadta/zkdata“ in den Zookeeper-Server auf dem Zielcluster zu importieren.

```apache
   java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
```
   
##### <a name="delete-offset-metadata-so-topologies-can-start-processing-data-from-either-beginning-or-timestamp-of-user-choice"></a>Löschen von Offsetmetadaten, damit Topologien entweder vom Anfang oder einem vom Benutzer gewählten Zeitstempel an mit der Verarbeitung von Daten beginnen können
1. Per SSH-Verbindung nach dem Zookeeper-Cluster auf dem alten Cluster, von dem aus der Prüfpunktoffset exportiert werden muss.
1. Führen Sie den folgenden Befehl aus (nach der Aktualisierung der HDP-Versionszeichenfolge), um alle Zookeeper-Offsetdaten für den aktuellen Cluster zu löschen.

```apache
   java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Wie finde ich Storm-Binärdateien auf einem Cluster?

### <a name="issue"></a>Problem:
 Ich muss den Speicherort von Binärdateien für Storm-Dienste auf dem HDInsight-Cluster erfahren.

### <a name="resolution-steps"></a>Lösungsschritte:

Storm-Binärdateien für den aktuellen HDP-Stapel finden Sie unter „/usr/hdp/current/storm-client“.

Dieser Speicherort ist für Hauptknoten und Workerknoten gleich.
 
Es gibt möglicherweise mehrere spezifische Binärdateien für mehrere HDP-Versionen unter „/usr/hdp“ (Beispiel: /usr/hdp/2.5.0.1233/storm).

Aber „/usr/hdp/current/storm-client“ ist über einen SYM-Link mit der neuesten Version verknüpft, die auf dem Cluster ausgeführt wird.

### <a name="further-reading"></a>Weitere nützliche Informationen:
 [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) [Storm](http://storm.apache.org/)
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Wie bestimme ich die Bereitstellungstopologie eines Storm-Clusters?
 
### <a name="issue"></a>Problem:
 
Identifizieren aller Komponenten, die mit HDInsight Storm installiert sind.
 
Der Storm-Cluster besteht aus 4 Knotenkategorien.
1. Gateway
1. Hauptknoten
1. Zookeeper-Knoten
1. Workerknoten
 
#### <a name="gateway-nodes"></a>Gatewayknoten
Ein Gateway- und Reverseproxydienst, der öffentlichen Zugriff auf den aktiven Ambari-Verwaltungsdienst ermöglicht und die Ambari Leader-Wahl abwickelt.
 
#### <a name="zookeeper-nodes"></a>Zookeeper-Knoten
Im Lieferumfang von HDInsight ist ein 3-Knoten-Zookeeper-Quorum enthalten.
Die Größe des Quorums ist unveränderlich und nicht konfigurierbar.
 
Storm-Dienste im Cluster werden für die automatische Verwendung des ZK-Quorums konfiguriert.
 
#### <a name="head-nodes"></a>Hauptknoten
Storm-Hauptknoten führen die folgenden Dienste aus:
1. Nimbus
1. Ambari-Server
1. Ambari Metrics-Server
1. Ambari Metrics-Sammler
 
#### <a name="worker-nodes"></a>Workerknoten
 Storm-Workerknoten führen die folgenden Dienste aus:
1. Supervisor
1. Worker JVMs für die Ausführung von Topologien
1. Ambari-Agent
 
## <a name="how-do-i-locate-storm-eventhub-spout-binaries-for-development"></a>Wie finde ich Storm-EventHub-Spout-Binärdateien für die Entwicklung?
 
### <a name="issue"></a>Problem:
Wie erhalte ich weitere Informationen zur Verwendung von Storm-EventHub-Spout-JAR-Dateien für die Verwendung mit meiner Topologie?
 
#### <a name="msdn-articles-on-how-to"></a>MSDN-Artikel zur Vorgehensweise
 
##### <a name="java-based-topology"></a>Java-basierte Topologie
https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology
 
##### <a name="c-based-topology-using-mono-on-hdi-34-linux-storm-clusters"></a>C#-basierte Topologie (mit Mono auf HDI 3.4+-Linux-Storm-Clustern)
https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology
 
##### <a name="latest-storm-eventhub-spout-binaries-for-hdi35-linux-storm-clusters"></a>Neueste Storm-EventHub-Spout-Binärdateien für HDI3.5+-Linux-Storm-Cluster
Unter „https://github.com/hdinsight/mvn-repo/blob/master/README.md“ erfahren Sie, wie Sie den neuesten Storm-Eventhub-Spout verwenden, der mit HDI3.5+-Linux-Storm-Clustern arbeitet.
 
##### <a name="source-code-examples"></a>Quellcodebeispiele:
https://github.com/Azure-Samples/hdinsight-java-storm-eventhub
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a>Wie finde ich Storm-Log4J-Konfigurationsdateien auf Clustern?
 
### <a name="issue"></a>Problem:
 
Identifizieren von Log4J-Konfigurationsdateien für Storm-Dienste.
 
#### <a name="on-headnodes"></a>Auf Hauptknoten:
Die Nimbus Log4J-Konfiguration wird von „usr/hdp/<HDPVersion>/storm/log4j2/cluster.xml“ gelesen.
 
#### <a name="worker-nodes"></a>Workerknoten
Die Supervisor Log4J-Konfiguration wird von „usr/hdp/<HDPVersion>/storm/log4j2/cluster.xml“ gelesen.
 
Die Worker Log4J-Konfiguration wird von „usr/hdp/<HDPVersion>/storm/log4j2/worker.xml“ gelesen.
 
Beispiel: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml







