---
title: "Einführung in Azure HDInsight, den Hadoop-Technologiestapel und Hadoop-Cluster | Microsoft Docs"
description: "Eine Einführung in HDInsight, den Hadoop-Technologiestapel und Hadoop-Komponenten, einschließlich Spark, Kafka, Hive, HBase für Big Data-Analysen."
keywords: "Azure Hadoop, Hadoop Azure, Hadoop-Intro, Hadoop-Einführung, Hadoop-Technologiestapel, Einführung in Hadoop, Einführung für Hadoop, was ist ein Hadoop-Cluster, was ist ein Hadoop-Cluster, wozu dient Hadoop"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/11/2017
ms.author: cgronlun
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 7d1f52550ca2b50e9536606d0f0099f4bf0f1e5e
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="introduction-to-azure-hdinsight-the-hadoop-technology-stack-and-hadoop-clusters"></a>Einführung in Azure HDInsight, den Hadoop-Technologiestapel und Hadoop-Cluster
 Dieser Artikel bietet eine Einführung in Azure HDInsight, eine Clouddistribution des Hadoop-Technologiestapels. Er behandelt auch, was ein Hadoop-Cluster ist und wann Sie ihn verwenden würden. 

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>Was sind HDInsight und der Hadoop-Technologiestapel? 
Azure HDInsight ist eine Clouddistribution der Hadoop-Komponenten von **Hortonworks Data Platform (HDP)**. [Apache Hadoop](http://hadoop.apache.org/) war ursprünglich ein Open Source-Framework für die verteilte Verarbeitung und Analyse großer Datasets in Clustern von Computern. 

Mit HDInsight bieten Hadoop-Technologien folgende Vorteile für die Nutzung:

*    Weniger Setup- und Konfigurationsaufwand. Siehe [Bereitstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
*    Hohe Verfügbarkeit und Zuverlässigkeit. Siehe [Verfügbarkeit und Zuverlässigkeit von HDInsight](hdinsight-high-availability-linux.md).
*    Sicherheit und Governance über die Active Directory-Integration. Siehe [In Domänen eingebundene Cluster](hdinsight-domain-joined-introduction.md).
*    Dynamisches Skalieren ohne Unterbrechung von Aufträgen
*   Komponentenupdates und aktuelle Versionen. Siehe [Hadoop-Komponenten und -Versionen in HDInsight][component-versioning].
*   Integration in andere Azure-Dienste wie [Web-Apps](https://docs.microsoft.com/azure/app-service-web/) und [SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/)

Der Hadoop-Technologiestapel umfasst verwandte Software und Hilfsprogramme, einschließlich Apache Hive, HBase, Spark, Kafka und viele andere. Weitere Informationen zu Hadoop in HDInsight finden Sie auf der Seite mit [Azure-Features für HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Was ist ein Hadoop-Cluster, und wann würden Sie ihn verwenden?
Der Begriff *Hadoop* bezieht sich auch auf einen Clustertyp mit folgenden Komponenten:

* Hadoop Distributed File System (HDFS)
* YARN für die Auftragsplanung und Ressourcenverwaltung
* MapReduce für die parallele Verarbeitung
  
Hadoop-Cluster werden meistens für die Batchverarbeitung gespeicherter Daten verwendet. Andere Arten von Clustern in HDInsight weisen zusätzliche Funktionen auf, z.B. eine schnellere In-Memory-Verarbeitung oder eine Verarbeitung für das Streaming von Nachrichtenwarteschlangen. Details finden Sie unter [Clustertypen in HDInsight](#overview).

## <a name="what-is-big-data"></a>Was versteht man unter "Big Data"?
Big Data beschreibt eine große Menge digitaler Informationen, Beispiele:

* Ein Twitter-Newsfeed
* Sensordaten von industriellen Anlagen
* Kundenaktivitäten, die auf einer Website gesammelt werden

Big Data kann sich auf Verlaufsdaten (also gespeicherte Daten) oder auf Echtzeitdaten (die von der Quelle gestreamt werden) beziehen. Big Data wird in immer größeren Mengen, mit immer höheren Geschwindigkeiten und in immer mehr Formaten erfasst.

## <a name="overview"></a>Clustertypen in HDInsight
HDInsight ist eine Clouddistribution des schnell wachsenden Apache Hadoop-Technikstapels in Microsoft Azure für die Big Data-Analyse. Sie umfasst bestimmte Clustertypen und Clusteranpassungsfunktionen, z.B. das Hinzufügen von Komponenten, Hilfsprogrammen und Sprachen.

### <a name="spark-kafka-interactive-hive-hbase-customized-and-other-cluster-types"></a>Spark-, Kafka-, Interactive Hive-, HBase-Cluster, angepasste Cluster und andere Clustertypen
HDInsight bietet die folgenden Clustertypen:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: Nutzt [HDFS](#hdfs), die [YARN](#yarn)-Ressourcenverwaltung und ein einfaches [MapReduce](#mapreduce)-Programmiermodell zum parallelen Verarbeiten und Analysieren von Daten.
* **[Apache Spark](http://spark.apache.org/)**: Apache Spark ist ein Open-Source-Framework für die Parallelverarbeitung, das die arbeitsspeicherinterne Verarbeitung unterstützt, um die Leistung von Anwendungen zur Analyse von großen Datenmengen zu steigern. Spark funktioniert mit SQL, Datenströmen und Machine Learning. Siehe [Was ist Apache Spark in HDInsight?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: Eine auf Hadoop basierende NoSQL-Datenbank, die wahlfreien Zugriff und starke Konsistenz für große Mengen unstrukturierter und teilstrukturierter Daten bietet – in einer potenziellen Dimension von Milliarden von Zeilen multipliziert mit Milliarden von Spalten. Siehe [Was ist HBase in HDInsight?](hdinsight-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Ein Server zum Hosten und Verwalten von parallelen, verteilten R-Prozessen. Diese Funktion ermöglicht Datenanalysten, Statistikern und R-Programmierern bei Bedarf den Zugriff auf skalierbare, verteilte Analysemethoden in HDInsight. Siehe [Übersicht über R Server in HDInsight](hdinsight-hadoop-r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: Ein verteiltes Echtzeitberechnungssystem für die schnelle Verarbeitung großer Datenströme. Storm wird als verwalteter Cluster in HDInsight angeboten. Siehe [Analysieren von Echtzeit-Sensordaten mit Storm und Hadoop](hdinsight-storm-sensor-data-analysis.md).
* **[Apache Interactive Hive-Vorschau (auch bekannt als „Live Long and Process“, LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: In-Memory-Zwischenspeicherung für interaktive und schnellere Hive-Abfragen. Siehe [Use Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md) (Verwenden von Interactive Hive in HDInsight).
* **[Apache Kafka](https://kafka.apache.org/)**: Eine Open Source-Plattform zum Erstellen von Streamingdatenpipelines und -anwendungen. Kafka bietet auch eine Nachrichtenwarteschlangenfunktion, die Ihnen das Veröffentlichen und Abonnieren von Datenströmen ermöglicht. Siehe [Introduction to Apache Kafka on HDInsight](hdinsight-apache-kafka-introduction.md) (Einführung in Apache Kafka in HDInsight).
* **[Vorschau von in die Domäne eingebundenen Clustern](hdinsight-domain-joined-introduction.md)**: Ein in eine Active Directory-Domäne eingebundener Cluster, der es Ihnen ermöglicht, den Zugriff zu steuern und Governance für Daten bereitzustellen.
* **[Benutzerdefinierte Cluster mit Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md)**: Cluster mit Skripts, die während der Bereitstellung ausgeführt werden und zusätzliche Komponenten installieren.

### <a name="example-cluster-customization-scripts"></a>Beispielskripts für die Clusteranpassung
Bei Skriptaktionen handelt es sich um Bash-Skripts unter Linux, die während der Clusterbereitstellung ausgeführt werden und zur Installation zusätzlicher Komponenten auf dem Cluster verwendet werden können. 

Die folgenden Beispielskripts werden vom HDInsight-Team bereitgestellt:

* **[Hue](hdinsight-hadoop-hue-linux.md)**: Eine Reihe von Webanwendungen zur Interaktion mit einem Hadoop-Cluster. Nur Linux-Cluster.
* **[Giraph](hdinsight-hadoop-giraph-install-linux.md)**: Graphenverarbeitung zum Modellieren von Beziehungen zwischen Dingen oder Personen.
* **[Solr](hdinsight-hadoop-solr-install-linux.md)**: Eine Unternehmensplattform für die Volltextsuche nach Daten.

Informationen zum Entwickeln eigener Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

## <a name="components-and-utilities-on-hdinsight-clusters"></a>Komponenten und Hilfsprogramme in HDInsight-Clustern
Die folgenden Komponenten und Hilfsprogramme sind in HDInsight-Clustern enthalten:

* **[Ambari](#ambari)**Bereitstellung, Verwaltung und Überwachung von Clustern sowie Hilfsprogramme für Cluster.
* **[Avro](#avro)** (Microsoft .NET Library für Avro): Datenserialisierung für die Microsoft .NET-Umgebung. 
* **[Hive und HCatalog](#hive)**: SQL-ähnliche Abfragen und eine Tabellen- und Speicherverwaltungsschicht.
* **[Mahout](#mahout)**: Für skalierbare Machine Learning-Anwendungen.
* **[MapReduce](#mapreduce)**Legacy-Framework für verteilte Verarbeitung und Ressourcenverwaltung für Hadoop. Siehe [YARN](#yarn).
* **[Oozie](#oozie)**: Workflowverwaltung.
* **[Phoenix](#phoenix)**: Relationale Datenbankschicht über HBase.
* **[Pig](#pig)**: Einfachere Skripts für MapReduce-Transformationen.
* **[Sqoop](#sqoop)**: Import und Export von Daten.
* **[Tez](#tez)**: Effiziente Ausführung datenintensiver Prozesse.
* **[YARN](#yarn)**: Ressourcenverwaltung, die Teil der Hadoop-Kernbibliothek ist.
* **[ZooKeeper](#zookeeper)**: Koordination von Prozessen in verteilten Systemen.

> [!NOTE]
> Informationen zu den jeweiligen Komponenten und Versionsinformationen finden Sie unter [Hadoop-Komponenten und -Versionen in HDInsight][component-versioning].
>
>

### <a name="ambari"></a>Ambari
Apache Ambari dient zur Bereitstellung, Verwaltung und Überwachung von Apache Hadoop-Clustern. Es umfasst eine Sammlung intuitiver Operatortools und eine Reihe robuster APIs, welche die Komplexität von Hadoop verbergen und den Betrieb von Clustern vereinfachen. HDInsight-Cluster unter Linux stellen die Ambari-Webbenutzeroberfläche und die Ambari-REST-API bereit. Ambari-Ansichten von HDInsight-Clustern ermöglichen Plug-In-Benutzerschnittstellenfunktionen.
Siehe [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md) und die <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari-API-Referenz</a>.

### <a name="avro"></a>Avro (Microsoft .NET-Bibliothek für Avro)
Die Microsoft .NET-Bibliothek für Avro implementiert das kompakte Apache Avro-Datenaustauschformat für Binärdaten für die Microsoft .NET-Umgebung. Damit wird ein sprachunabhängiges Schema definiert, sodass in einer Sprache serialisierte Daten in einer anderen gelesen werden können. Ausführliche Informationen zum Format finden Sie in der <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro-Spezifikation</a>. Das Format von Avro-Dateien unterstützt das verteilte MapReduce-Programmiermodell: Dateien sind „teilbar“, Sie können also einen beliebigen Punkt in einer Datei suchen und mit dem Lesen in einem bestimmten Block beginnen. Weitere Informationen hierzu finden Sie unter [Serialisieren von Daten mit der Microsoft .NET-Bibliothek für Avro](hdinsight-dotnet-avro-serialization.md). Linux-basierte Clusterunterstützung wird später ergänzt.

### <a name="hdfs"></a>HDFS
Hadoop Distributed File System (HDFS) ist ein Dateisystem, das zusammen mit YARN und MapReduce den Kern der Hadoop-Technologie bildet. Es ist das standardmäßige Dateisystem für Hadoop-Cluster in HDInsight. Siehe [Abfragen von Daten aus HDFS-kompatiblem Speicher](hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive und HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> ist eine auf Hadoop basierende Data Warehouse-Software, mit der Sie große Datasets in verteilten Speicherformen mithilfe einer SQL-ähnlichen Sprache namens HiveQL abfragen und verwalten können. Hive ist wie Pig eine Abstraktionsebene über MapReduce und übersetzt Abfragen in eine Reihe von MapReduce-Aufträgen. Hive ähnelt eher einem Verwaltungsystem für relationale Datenbanken als Pig und wird für besser strukturierte Daten verwendet. Für unstrukturierte Daten ist Pig die bessere Wahl. Siehe [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> ist eine Tabellen- und Speicherverwaltungsschicht für Hadoop, die Ihnen eine relationale Datenansicht bietet. In HCatalog können Sie Dateien in jedem Format lesen und schreiben, für das ein Hive-SerDe (Serialisierungs-/Deserialisierungsprogramm) geschrieben werden kann.

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> ist eine auf Hadoop basierende Bibliothek mit Machine Learning-Algorithmen. Anwendungen für maschinelles Lernen verwenden Grundsätze der Statistik, um aus Daten zu lernen und um zukünftiges Verhalten anhand vergangener Daten zu bestimmen. Siehe [Erstellen von Filmempfehlungen mithilfe von Mahout und Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce ist das Legacy-Softwareframework für Hadoop zum Schreiben von Anwendungen für die parallele Stapelverarbeitung großer Datasets. Ein MapReduce-Auftrag unterteilt große Datasets und verwaltet die Daten in Schlüssel-Wert-Paaren für die Verarbeitung. MapReduce-Aufträge können mit [YARN](#yarn) ausgeführt werden. Siehe <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> im Hadoop-Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> ist ein Koordinationssystem für Workflows zur Verwaltung von Hadoop-Aufträgen. Es ist in den Hadoop-Stack integriert und unterstützt Hadoop-Jobs für MapReduce, Pig, Hive und Sqoop. Es kann auch dazu verwendet werden, bestimmte Jobs für ein System zu planen, beispielsweise Java-Programme oder Shellskripts. Weitere Informationen finden Sie unter [Verwenden von Oozie mit Hadoop](hdinsight-use-oozie-linux-mac.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> ist eine relationale Datenbankschicht über HBase. Phoenix umfasst einen JDBC-Treiber, mit dem Sie SQL-Tabellen direkt abfragen und verwalten können. Anstatt MapReduce zu verwenden, übersetzt Phoenix Abfragen und andere Anweisungen in systemeigene NoSQL-API-Aufrufe und ermöglicht so auf NoSQL-Speicher aufsetzend schnellere Anwendungen. Siehe [Verwenden von Apache Phoenix und SQuirreL mit HBase-Clustern](hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> ist eine allgemeine Plattform zum Anwenden komplexer MapReduce-Transformationen auf große Datasets mithilfe einer einfachen Skriptsprache namens Pig Latin. Pig übersetzt Pig Latin-Skripts für die Ausführung in Hadoop. Sie können benutzerdefinierte Funktionen erstellen, um Pig Latin zu erweitern. Weitere Informationen finden Sie unter [Verwenden von Pig mit Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> ist ein Tool für die möglichst effektive Übertragung großer Datenmengen zwischen Hadoop und relationalen Datenbanken wie SQL oder anderen strukturierten Datenspeichern. Siehe [Verwenden von Sqoop mit Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> ist ein auf Hadoop YARN aufsetzendes Anwendungsframework für die Erstellung komplexer, azyklischer Diagramme der allgemeinen Datenverarbeitung. Es handelt sich hier um einen flexibleren und leistungsfähigeren Nachfolger des MapReduce-Frameworks, der auch bei datenintensiven Prozessen wie Hive eine effiziente Ausführung zulässt. Siehe ["Verwenden von Apache Tez zur Verbesserung der Leistung" unter "Verwenden von Hive und HiveQL"](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN ist die nächste Generation von MapReduce (MapReduce 2.0 bzw. MRv2) und unterstützt Datenverarbeitungsszenarien, die über die Stapelverarbeitung von MapReduce hinausgehen. YARN bietet mehr Skalierbarkeit und bessere Echtzeitverarbeitung. YARN bietet Ressourcenmanagement und ein verteiltes Anwendungsframework. MapReduce-Aufträge können mit YARN ausgeführt werden. Siehe <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordiniert Prozesse in großen verteilten Systemen mithilfe eines gemeinsamen hierarchischen Namespaces von Datenregistern (znodes). Znodes enthalten kleine Mengen an Metainformationen, die für die Koordination von Prozessen erforderlich sind: Status, Speicherort, Konfiguration usw. Ein Beispiel finden Sie unter [ZooKeeper mit einem HBase-Cluster und Apache Phoenix](hdinsight-hbase-phoenix-squirrel-linux.md). 

## <a name="programming-languages-on-hdinsight"></a>Programmiersprachen in HDInsight
HDInsight-Cluster – Spark, HBase Kafka, Hadoop und andere Cluster – unterstützen viele Programmiersprache, aber einige werden nicht standardmäßig installiert. [Verwenden Sie eine Skriptaktion](hdinsight-hadoop-script-actions-linux.md), um Bibliotheken, Module oder Pakete zu installieren, die standardmäßig nicht installiert sind. 

### <a name="default-programming-language-support"></a>Standardmäßige Unterstützung für Programmiersprachen
Standardmäßig unterstützen HDInsight-Cluster folgende Sprachen:

* Java
* Python

Zusätzliche Sprachen können mithilfe von [Skriptaktionen](hdinsight-hadoop-script-actions-linux.md) installiert werden.

### <a name="java-virtual-machine-jvm-languages"></a>JVM-Sprachen (Java Virtual Machine)
Neben Java können viele weitere Sprachen mithilfe einer JVM (Java Virtual Machine) ausgeführt werden. Für einige dieser Sprachen müssen jedoch möglicherweise zusätzliche Komponenten im Cluster installiert werden.

Diese JVM-basierten Sprachen werden in HDInsight-Clustern unterstützt:

* Clojure
* Jython (Python für Java)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-spezifische Sprachen
HDInsight-Cluster bieten Unterstützung für die folgenden Sprachen, die für den Hadoop-Technologiestapel spezifisch sind:

* Pig Latin für Pig-Aufträge
* HiveQL für Hive-Aufträge und SparkSQL

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard und HDInsight Premium
HDInsight bietet Cloudlösungen für Big Data in zwei Kategorien an: Standard und Premium. HDInsight Standard stellt einen Unternehmenscluster bereit, in dem Organisationen ihre Big Data-Workloads ausführen können. HDInsight Premium baut auf Standard-Funktionen auf und bietet erweiterte Analyse- und Sicherheitsfunktionen für einen HDInsight-Cluster. Weitere Informationen finden Sie unter [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="microsoft-business-intelligence-and-hdinsight"></a>Microsoft Business Intelligence und HDInsight
Bekannte Business Intelligence-Tools (BI) rufen Daten, die in HDInsight integriert sind, entweder über das Add-In Power Query oder den Microsoft Hive-ODBC-Treiber ab, analysieren sie und erstellen Berichte:

* [Verbinden von Excel mit Hadoop über Power Query](hdinsight-connect-excel-power-query.md): Erfahren Sie, wie Sie Excel mithilfe von Microsoft Power Query mit dem Azure-Speicherkonto verbinden, in dem die Daten für Ihren HDInsight-Cluster gespeichert werden. Windows-Arbeitsstation ist erforderlich. 
* [Verbinden von Excel über den Microsoft Hive-ODBC-Treiber mit Hadoop](hdinsight-connect-excel-hive-odbc-driver.md): Informationen zum Importieren von Daten aus HDInsight mit dem Microsoft Hive-ODBC-Treiber. Windows-Arbeitsstation ist erforderlich. 
* [Microsoft-Cloudplattform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Entdecken Sie Power BI für Office 365, laden Sie die Testversion von SQL Server herunter, und richten Sie SharePoint Server 2013 und SQL Server BI ein.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): Ein Lernprogramm für die schnelle Bereitstellung von HDInsight-Hadoop-Clustern und Ausführung von Hive-Beispielabfragen.
* [Erste Schritte mit Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): Ein Schnellstart-Tutorial zum Erstellen eines Spark-Clusters und Ausführen von interaktiven Spark SQL-Abfragen.
* [Erste Schritte mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-get-started.md): Informationen zum Einstieg in die Verwendung von R Server in HDInsight Premium.
* [Bereitstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md): Erfahren Sie, wie ein HDInsight Hadoop-Cluster über das Azure-Portal, die Azure-Befehlszeilenschnittstelle (Azure-CLI) oder Azure PowerShell bereitgestellt wird.


[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
