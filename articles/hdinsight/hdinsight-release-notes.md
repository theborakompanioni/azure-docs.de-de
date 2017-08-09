---
title: Versionshinweise zu Hadoop-Komponenten in Azure HDInsight | Microsoft Docs
description: "Neueste Versionshinweise und Versionen der Hadoop-Komponenten für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Spark, R Server, Hive und vieles mehr ab."
services: hdinsight
documentationcenter: 
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.assetid: a363e5f6-dd75-476a-87fa-46beb480c1fe
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 91620e993ae0413080dad03344290e971bf74949
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Versionshinweise zu Hadoop-Komponenten in Azure HDInsight

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie im Artikel zur [HDInsight-Versionsverwaltung](hdinsight-component-versioning.md).

## <a name="04062017---general-availability-of-hdinsight-36"></a>06.04.2017: Allgemeine Verfügbarkeit von HDInsight 3.6

* Mit diesem Release wird Azure HDInsight Version 3.6 eingeführt, die auf HDP 2.6 basiert. Hinweise zur HDP-Version 2.6 finden Sie [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html). Weitere Informationen zu HDInsight-Versionen finden Sie [hier](hdinsight-component-versioning.md). HDInsight 3.6 ist für die folgenden Workloads verfügbar:

    * Hadoop 2.7.3
    * HBase 1.1.2
    * Storm 1.1.0
    * Spark 2.1.0
    * Interactive Hive 2.1.0

* **Unterstützung für Hive View 2.0:** Dient der Verbesserung der Benutzerfreundlichkeit von Interactive Hive. Weitere Informationen finden Sie in der [Hortonworks-Dokumentation](http://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-views/content/ch_using_hive_view.html).

* **Leistungsverbesserungen mit Hive LLAP:** Weitere Informationen finden Sie in der [Hortonworks-Dokumentation](https://hortonworks.com/blog/top-5-performance-boosters-with-apache-hive-llap/).

* **Neue Features in Hive:** Weitere Informationen finden Sie in der [Hortonworks-Dokumentation](https://hortonworks.com/apache/hive/#section_4).

* **Hive-Befehlszeilenschnittstelle veraltet:** Die Hive-Befehlszeilenschnittstelle gilt als veraltet. Kunden wird empfohlen, stattdessen Beeline zu verwenden. Weitere Informationen finden Sie in der [Apache-Dokumentation](https://cwiki.apache.org/confluence/display/Hive/Replacing+the+Implementation+of+Hive+CLI+Using+Beeline). Anweisungen zur Verwendung von Beeline mit HDInsight finden Sie unter [Verwenden von Beeline mit HDInsight Hadoop-Clustern](hdinsight-hadoop-use-hive-beeline.md).

* **Neue Features in Apache Phoenix und HBase:**
    * Unterstützung für Speicherkontingent: Wird am häufigsten in Umgebungen mit mehreren Mandanten verwendet und ermöglicht eingeschränkten Speicherplatz pro Tabelle und Namespace.
    * Phoenix-Indizierungsverbesserungen: Inkrementelle Indexerstellung und Neuerstellung/Wiederaufnahme der Indizierung aus vorherigen Fehlern.
    * Phoenix-Datenintegritätstool: Unterstützt die Überprüfung des Schemas, des Index und anderer Metadaten.


* **Problem mit HBase:** Beim Ausführen eines MapReduce-Auftrags zum CSV-Massenupload führt die folgende Syntax möglicherweise zu einem Fehler.

        HADOOP_CLASSPATH=$(hbase mapredcp):/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

    Verwenden Sie stattdessen die folgende Syntax:

        HADOOP_CLASSPATH=/path/to/hbase-protocol.jar:/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv


## <a name="02282017---release-of-spark-21-on-hdinsight-36-preview"></a>28.02.2017: Veröffentlichung von Spark 2.1 in HDInsight 3.6 (Vorschau)
* [Spark 2.1](http://spark.apache.org/releases/spark-release-2-1-0.html) bietet gegenüber früheren Versionen viele Verbesserungen bei der Stabilität und der Benutzerfreundlichkeit. Dazu kommen neue Features für alle Spark-Workloads, wie z.B. Spark Core, SQL, ML und Streaming.
* Das strukturierte Streaming bietet eine verbesserte Skalierbarkeit mit Unterstützung für Zeitwasserzeichen für Ereignis und den Kafka 0.10-Connector.
* Die Spark SQL-Partitionierung erfolgt jetzt mit dem neuen skalierbaren Partitionierungsmechanismus. Weitere Informationen zum Upgraden finden Sie [hier](http://spark.apache.org/releases/spark-release-2-1-0.html).
* Spark 2.1 in der Azure HDInsight 3.6-Vorschau unterstützt derzeit keine BI-Tool-Verbindungen mithilfe des ODBC-Treibers.
* Der Azure Data Lake Store-Zugriff von Spark 2.1-Clustern wird in dieser Vorschau nicht unterstützt.


## <a name="11182016---release-of-spark-201-on-hdinsight-35"></a>18.11.2016: Veröffentlichung von Spark 2.0.1 in HDInsight 3.5
Spark 2.0.1 ist nun für Spark-Cluster verfügbar (HDInsight-Version 3.5).

## <a name="11162016---release-of-r-server-90-on-hdinsight-35-spark-20"></a>16.11.2016: Veröffentlichung von R Server 9.0 in HDInsight 3.5 (Spark 2.0)
*   R Server-Cluster enthalten jetzt die Option für zwei Versionen: R Server 9.0 unter HDI 3.5 (Spark 2.0) und R Server 8.0 unter HDI 3.4 (Spark 1.6).
*   R Server 9.0 unter HDI 3.5 (Spark 2.0) basiert auf R 3.3.2 und enthält neue ScaleR-Datenquellenfunktionen mit den Bezeichnungen RxHiveData und RxParquetData, um Daten für die Analyse mit ScaleR direkt aus Hive und Parquet in Spark DataFrames zu laden. Weitere Informationen finden Sie in der Inlinehilfe zu diesen Funktionen in R über die Befehle **?RxHiveData** und **?RxParquetData**.
*   RStudio Server Community Edition wird jetzt standardmäßig (mit Abwahloption) als Teil des Bereitstellungsflows auf dem Blatt „Clusterkonfiguration“ installiert.

## <a name="11092016---release-of-spark-20-on-hdinsight"></a>09.11.2016: Veröffentlichung von Spark 2.0 in HDInsight
* Spark 2.0-Cluster in HDInsight 3.5 unterstützen jetzt Livy- und Jupyter-Dienste.

## <a name="10262016---release-of-r-server-on-hdinsight"></a>26.10.2016: Veröffentlichung von R Server in HDInsight
* Der URI für den Zugriff auf den Edgeknoten hat sich in „**clustername**-ed-ssh.azurehdinsight.net“ geändert.
* Die Bereitstellung von R Server in HDInsight-Clustern wurde optimiert.
* R Server in HDInsight steht jetzt als regulärer HDInsight-Clustertyp „R Server“ zur Verfügung und wird nicht mehr als separate HDInsight-Anwendung installiert. Der Edgeknoten und R-Binärdateien werden jetzt als Teil der R Server-Clusterbereitstellung bereitgestellt. Dies verbessert die Geschwindigkeit und die Zuverlässigkeit der Bereitstellung. Das Preismodell für R Server wird entsprechend aktualisiert.
* Der Preis für den Clustertyp „R Server“ basiert jetzt auf dem Standard-Tarif plus R Server-Zuschlag. Der Premium-Tarif wird jetzt für Premium-Funktionen, die für verschiedene Clustertypen verfügbar sind, reserviert und nicht für R Server-Cluster verwendet. Diese Änderung betrifft keine effektiven Preise für R Server, sondern ändert nur die Angabe der Gebühren auf der Rechnung. Alle vorhandenen R Server-Cluster funktionieren weiterhin, und Resource Manager-Vorlagen können weiterhin genutzt werden, bis Sie einen Hinweis zu veralteten Funktionen erhalten. **Es wird empfohlen, die skriptgesteuerten Bereitstellungen zu aktualisieren, sodass sie die neue Resource Manager-Vorlage verwenden.**

## <a name="08302016---release-of-r-server-on-hdinsight"></a>30.08.2016: Veröffentlichung von R Server in HDInsight
Die vollständigen Versionsnummern für mit dieser Version bereitgestellte Linux-basierte HDInsight-Cluster lauten:

| HDI | HDI-Clusterversion | HDP | HDP Build | Ambari-Build |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8268980 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8268980 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8269383 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Die vollständigen Versionsnummern für mit dieser Version bereitgestellte Windows-basierte HDInsight-Cluster lauten:

| HDI | HDI-Clusterversion | HDP | HDP Build |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1033.2559206 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1033.2559206 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1033.2559206 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1033.2559206 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1033.2559206 |2.3 |2.3.3.1-25 |






