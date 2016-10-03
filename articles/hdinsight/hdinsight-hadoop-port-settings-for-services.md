<properties
pageTitle="Von HDInsight verwendete Ports | Azure"
description="Enthält eine Liste mit Ports, die von in HDInsight ausgeführten Hadoop-Diensten verwendet werden."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/13/2016"
ms.author="larryfr"/>

# Von HDInsight verwendete Ports und URIs

Dieses Dokument enthält eine Liste mit den Ports für Hadoop-Dienste, die in Linux-basierten HDInsight-Clustern ausgeführt werden. Außerdem enthält es Informationen zu Ports, die zum Herstellen einer Verbindung mit dem Cluster per SSH verwendet werden.

## Öffentliche Ports im Vergleich mit nicht öffentlichen Ports

Für Linux-basierte HDInsight-Cluster werden nur drei Ports öffentlich im Internet verfügbar gemacht: 22, 23 und 443. Diese werden zum sicheren Zugreifen auf den Cluster per SSH und auf Dienste verwendet, die über das sichere HTTPS-Protokoll bereitgestellt werden.

Intern wird HDInsight von mehreren Azure Virtual Machines (Knoten im Cluster) implementiert, die in einem Azure Virtual Network ausgeführt werden. Aus dem virtuellen Netzwerk können Sie auf Ports zugreifen, die nicht über das Internet verfügbar gemacht werden. Wenn Sie beispielsweise eine Verbindung mit einem der Hauptknoten per SSH herstellen, können Sie vom Hauptknoten dann direkt auf Dienste zugreifen, die auf den Clusterknoten ausgeführt werden.

> [AZURE.IMPORTANT] Wenn Sie einen HDInsight-Cluster erstellen und kein Azure Virtual Network als Konfigurationsoption angeben, wird eines erstellt. Es ist aber nicht möglich, diesem automatisch erstellten virtuellen Netzwerk andere Computer hinzuzufügen (z.B. andere Azure Virtual Machines oder Ihren Cliententwicklungscomputer).

Um für weitere Computer den Beitritt zum virtuellen Netzwerk durchzuführen, müssen Sie zuerst das virtuelle Netzwerk erstellen und beim Erstellen des HDInsight-Clusters dann angeben. Weitere Informationen finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

## Öffentliche Ports

Alle Knoten eines HDInsight-Clusters befinden sich in einem Azure Virtual Network, und darauf kann aus dem Internet nicht direkt zugegriffen werden. Ein öffentliches Gateway ermöglicht den Internetzugriff auf die folgenden Ports, die für alle HDInsight-Clustertypen gleich sind:

| Dienst | Port | Protocol | Beschreibung |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Stellt eine Verbindung zwischen Clients und SSHD auf dem primären Hauptknoten her. Siehe [Verwenden von SSH mit Linux-basiertem HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md). |
| sshd | 22 | SSH | Verbindet Clients mit sshd auf dem Edgeknoten (nur HDInsight Premium). Siehe [Erste Schritte mit R Server in HDInsight](hdinsight-hadoop-r-server-get-started.md). |
| sshd | 23 | SSH | Stellt eine Verbindung zwischen Clients und SSHD auf dem sekundären Hauptknoten her. Siehe [Verwenden von SSH mit Linux-basiertem HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md). |
| Ambari | 443 | HTTPS | Ambari-Webbenutzeroberfläche. Siehe [Verwalten von HDInsight mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md). |
| Ambari | 443 | HTTPS | Ambari-REST-API. Siehe [Verwalten von HDInsight mithilfe der Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md). |
| WebHCat | 443 | HTTPS | HCatalog-REST-API. Siehe [Verwenden von Hive mit Curl](hdinsight-hadoop-use-Pig-curl.md), [Verwenden von Pig mit Curl](hdinsight-hadoop-use-Pig-curl.md), [Verwenden von MapReduce mit Curl](hdinsight-hadoop-use-mapreduce-curl.md). |
| HiveServer2 | 443 | ODBC | Stellt per ODBC ein Verbindung mit Hive her. Siehe [Verbinden von Excel mit HDInsight mithilfe des Microsoft ODBC-Treibers](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Stellt per JDBC ein Verbindung mit Hive her. Siehe [Herstellen einer Verbindung mit Hive unter HDInsight per Hive-JDBC-Treiber](hdinsight-connect-hive-jdbc-driver.md). |

Für spezielle Clustertypen ist Folgendes verfügbar:

| Dienst | Port | Protocol |Clustertyp | Beschreibung |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase-REST-API. Siehe [Erste Schritte mit HBase](hdinsight-hbase-tutorial-get-started-linux.md). |
| Livy | 443 | HTTPS | Spark | Spark-REST-API. Siehe [Remoteübermittlung von Spark-Aufträgen mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md). |
| Storm | 443 | HTTPS | Storm | Storm-Webbenutzeroberfläche. Siehe [Bereitstellen und Verwalten von Storm-Topologien in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).

### Authentifizierung

Alle Dienste, die im Internet öffentlich verfügbar gemacht werden, müssen authentifiziert werden:

| Port | Anmeldeinformationen |
| ---- | ----------- |
| 22 oder 23 | SSH-Benutzeranmeldeinformationen, die während der Clustererstellung angegeben werden |
| 443 | Anmeldename (Standard: admin,) und Kennwort, die während der Clustererstellung angegeben werden |

## Nicht öffentliche Ports

> [AZURE.NOTE] Einige Dienste stehen nur bei bestimmten Clustertypen zur Verfügung. HBase beispielsweise ist nur bei HBase-Clustertypen verfügbar.

### HDFS-Ports

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode-Webbenutzeroberfläche | Hauptknoten | 30070 | HTTPS | Webbenutzeroberfläche zum Anzeigen des aktuellen Status |
| NameNode-Metadatendienst | Hauptknoten | 8020 | IPC | Dateisystem-Metadaten 
| DataNode | Alle Workerknoten | 30075 | HTTPS | Webbenutzeroberfläche zum Anzeigen von Status, Protokollen usw. |
| DataNode | Alle Workerknoten | 30010 | &nbsp; | Datenübertragung |
| DataNode | Alle Workerknoten | 30020 | IPC | Metadatenvorgänge |
| Sekundärer NameNode | Hauptknoten | 50090 | HTTP | Prüfpunkt für NameNode-Metadaten |

### YARN-Ports

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Resource Manager-Webbenutzeroberfläche | Hauptknoten | 8088 | HTTP | Webbenutzeroberfläche für Resource Manager |
| Resource Manager-Webbenutzeroberfläche | Hauptknoten | 8090 | HTTPS | Webbenutzeroberfläche für Resource Manager |
| Resource Manager-Administratoroberfläche | Hauptknoten | 8141 | IPC | Für Anwendungsübermittlungen (Hive, Hive-Server, Pig usw.) |
| Resource Manager-Planer | Hauptknoten | 8030 | HTTP | Administratoroberfläche |
| Resource Manager-Anwendungsschnittstelle | Hauptknoten | 8050 | HTTP |Adresse der Anwendungs-Manager-Schnittstelle |
| NodeManager | Alle Workerknoten | 30050 | &nbsp; | Adresse des Container-Managers |
| NodeManager-Webbenutzeroberfläche | Alle Workerknoten | 30060 | HTTP | Resource Manager-Schnittstelle |
| Zeitachsenadresse | Hauptknoten | 10200 | RPC | RPC-Dienst des Zeitachsendiensts |
| Zeitachsen-Webbenutzeroberfläche | Hauptknoten | 8181 | HTTP | Webbenutzeroberfläche des Zeitachsendiensts |

### Hive-Ports

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Hauptknoten | 10001 | Thrift | Dienst für die programmgesteuerte Verbindungsherstellung mit Hive (Thrift/JDBC) |
| HiveServer | Hauptknoten | 10000 | Thrift | Dienst für die programmgesteuerte Verbindungsherstellung mit Hive (Thrift/JDBC) |
| Hive-Metastore | Hauptknoten | 9083 | Thrift | Dienst für die programmgesteuerte Verbindungsherstellung mit Hive-Metadaten (Thrift/JDBC) |

### WebHCat-Ports

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat-Server | Hauptknoten | 30111 | HTTP | Web-API zusätzlich zu HCatalog und anderen Hadoop-Diensten |

### MapReduce-Ports

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Hauptknoten | 19888 | HTTP | MapReduce JobHistory-Webbenutzeroberfläche |
| JobHistory | Hauptknoten | 10020 | &nbsp; | MapReduce JobHistory-Server |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Überträgt Map-Zwischenausgaben an anfordernde Reducer |

### Oozie

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie-Server | Hauptknoten | 11000 | HTTP | URL für Oozie-Dienst |
| Oozie-Server | Hauptknoten | 11001 | HTTP | Port für Oozie-Verwaltung |

### Ambari-Metriken

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| TimeLine (Anwendungsverlauf) | Hauptknoten | 6188 | HTTP | Webbenutzeroberfläche des TimeLine-Diensts |
| TimeLine (Anwendungsverlauf) | Hauptknoten | 30200 | RPC | Webbenutzeroberfläche des TimeLine-Diensts |

### HBase-Ports

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Hauptknoten | 16000 | &nbsp; | &nbsp; |
| HMaster-Info-Webbenutzeroberfläche | Hauptknoten | 16010 | HTTP | Port für die HBase-Master-Webbenutzeroberfläche |
| Regionsserver | Alle Workerknoten | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Port, der von Clients für die Verbindungsherstellung mit ZooKeeper verwendet wird |

### Kafka-Ports

| Dienst | Knoten | Port | Protocol | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Broker | Workerknoten | 9092 | [Kafka Wire Protocol](http://kafka.apache.org/protocol.html) | Für die Clientkommunikation |
| &nbsp; | Zookeeper-Knoten | 2181 | &nbsp; | Port, der von Clients für die Verbindungsherstellung mit Zookeeper verwendet wird |

<!---HONumber=AcomDC_0921_2016-->