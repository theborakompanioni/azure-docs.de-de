---
title: "Was ist Hadoop? Einführung in Azure HDInsight | Microsoft-Dokumentation"
description: "In diesem Artikel erhalten Sie eine Einführung in das Hadoop-Ökosystem und die Komponenten von HDInsight. HDInsight enthält Hadoop, Spark, HBase und weitere Funktionen für die Verarbeitung und Analyse von Big Data."
keywords: "Big Data-Analyse,Einführung in Hadoop,Was ist Hadoop,Hadoop-Technologiestapel,Hadoop-Ökosystem"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2016
ms.author: cgronlun
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: e0ebb2224c1f3290df2a89cfaeb9797a7dbcc051
ms.lasthandoff: 03/11/2017


---
# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Einführung in das Hadoop-Ökosystem in Azure HDInsight
 Dieser Artikel enthält eine Einführung in Hadoop in Azure HDInsight, das dazugehörige Ökosystem und Big Data. Sie erhalten Informationen zu den Hadoop-Komponenten, zu gängiger Terminologie und zu Szenarien für die Big Data-Analyse.

## <a name="what-is-hadoop-on-hdinsight"></a>Was ist Hadoop in HDInsight?
Hadoop bezieht sich auf ein Ökosystem mit Open-Source-Software, das als Framework für die verteilte Verarbeitung, Speicherung und Analyse von Big Data-Datasets in Clustern von Computern dient. Azure HDInsight macht die Hadoop-Komponenten der Distribution **Hortonworks Data Platform (HDP)** in der Cloud verfügbar, stellt verwaltete Cluster mit hoher Zuverlässigkeit und Verfügbarkeit bereit und bietet mit Active Directory Sicherheit sowie Governance für Unternehmen.  

Apache Hadoop war das ursprüngliche Open-Source-Projekt für die Big Data-Verarbeitung. Danach folgte die Entwicklung der zugehörigen Software und der Hilfsprogramme, die als Teil des Hadoop-Technologiestapels angesehen werden, z.B. Apache Hive, Apache HBase, Apache Spark, Apache Kafka und viele andere. Weitere Informationen finden Sie unter [Übersicht über das Hadoop-Ökosystem in HDInsight](#overview).

## <a name="what-is-big-data"></a>Was versteht man unter "Big Data"?
„Big Data“ beschreibt beliebig großen Mengen digitaler Informationen – vom Text in einem Twitter-Feed über Sensordaten von Industrieanlagen bis hin zu Daten über die Navigation und Einkäufe von Kunden auf einer Website. "Big Data" kann sich auf historische (gespeicherte) Daten oder auf Echtzeit-Daten (direkt von der Quelle gestreamt) beziehen. Big Data wird in immer größeren Mengen, mit immer höheren Geschwindigkeiten und in immer mehr Formaten erfasst.

Damit Big Data verwertbare Informationen und Erkenntnisse liefern kann, müssen Sie relevante Daten sammeln und die richtigen Fragen stellen. Außerdem müssen Sie sicherstellen, dass die Daten zugänglich sind und bereinigt, analysiert und dann auf passende Weise bereitgestellt werden. Hierbei kann die Big Data-Analyse mit Hadoop in HDInsight hilfreich sein.

## <a name="overview"></a>Übersicht über das Hadoop-Ökosystem in HDInsight
HDInsight ist eine Clouddistribution des schnell wachsenden Apache Hadoop-Technikstapels in Microsoft Azure für die Big Data-Analyse. HDInsight enthält Implementierungen von Apache Spark, HBase, Kafka, Storm, Pig, Hive, Interactive Hive, Sqoop, Oozie, Ambari, usw. HDInsight kann außerdem mit Business Intelligence-Tools (BI) wie z. B. Power BI, Excel, SQL Server Analysis Services oder SQL Server Reporting Services integriert werden.

### <a name="hadoop-hbase-spark-kafka-interactive-hive-storm-customized-and-other-clusters"></a>Hadoop-, HBase, Spark-, Kafka-, Interactive Hive-, Storm-, angepasste und andere Cluster
HDInsight bietet die folgenden Clustertypen:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: Bietet eine zuverlässige Datenspeicherung mithilfe von [HDFS](#hdfs) und ein einfaches [MapReduce](#mapreduce)-Programmiermodell zum parallelen Verarbeiten und Analysieren von Daten.
* **[Apache Spark](http://spark.apache.org/)**: Apache Spark ist ein Open-Source-Framework für die Parallelverarbeitung, das die arbeitsspeicherinterne Verarbeitung unterstützt, um die Leistung von Anwendungen zur Analyse von großen Datenmengen zu steigern. Spark funktioniert mit SQL, Datenströmen und Machine Learning. Weitere Informationen finden Sie unter [Übersicht: Was ist Apache Spark in HDInsight?](hdinsight-apache-spark-overview.md).
* **[Apache HBase](http://hbase.apache.org/)**: Eine auf Hadoop basierende NoSQL-Datenbank, die wahlfreien Zugriff und starke Konsistenz für große Mengen unstrukturierter und teilstrukturierter Daten bietet – in einer potenziellen Dimension von Milliarden von Zeilen multipliziert mit Milliarden von Spalten. Siehe [Übersicht über HBase in HDInsight](hdinsight-hbase-overview.md).
* **[Microsoft R Server](https://msdn.microsoft.com/en-us/microsoft-r/rserver)**: Ein Server für Unternehmen zum Hosten und Verwalten von parallelen, verteilten R-Prozessen. Diese Funktion ermöglicht Datenanalysten, Statistikern und R-Programmierern bei Bedarf den Zugriff auf skalierbare, verteilte Analysemethoden in HDInsight. Siehe [Übersicht über R Server in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-r-server-overview).
* **[Apache Storm](https://storm.incubator.apache.org/)**: Ein verteiltes Echtzeitberechnungssystem für die schnelle Verarbeitung großer Datenströme. Storm wird als verwalteter Cluster in HDInsight angeboten. Siehe [Analysieren von Echtzeit-Sensordaten mit Storm und Hadoop](hdinsight-storm-sensor-data-analysis.md).
* **[Apache Interactive Hive-Vorschau (auch bekannt als „Live Long and Process“, LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: In-Memory-Zwischenspeicherung für interaktive und schnellere Hive-Abfragen. Siehe [Use Interactive Hive in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-use-interactive-hive) (Verwenden von Interactive Hive in HDInsight).
* **[Apache Kafka-Vorschau](https://kafka.apache.org/)**: Eine Open Source-Plattform zum Erstellen von Streamingdatenpipelines und -anwendungen. Kafka bietet auch eine Nachrichtenwarteschlangenfunktion, die Ihnen das Veröffentlichen und Abonnieren von Datenströmen ermöglicht. Siehe [Introduction to Apache Kafka on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-kafka-introduction) (Einführung in Apache Kafka in HDInsight).
* **[Vorschau von in die Domäne eingebundenen Clustern](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-domain-joined-introduction)**: Ein in eine Active Directory-Domäne eingebundener Cluster, der es Ihnen ermöglicht, den Zugriff zu steuern und Governance für Daten bereitzustellen.
* **[Benutzerdefinierte Cluster mit Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md)**: Cluster mit Skripts, die während der Bereitstellung ausgeführt werden und zusätzliche Komponenten installieren.

### <a name="example-customization-scripts"></a>Beispielskripts für die Anpassung
Bei Skriptaktionen handelt es sich um Bash-Skripts unter Linux, die während der Clusterbereitstellung ausgeführt werden und zur Installation zusätzlicher Komponenten auf dem Cluster verwendet werden können.

Die folgenden Beispielskripts werden vom HDInsight-Team bereitgestellt:

* [Hue](hdinsight-hadoop-hue-linux.md)– eine Reihe von Webanwendungen zur Interaktion mit einem Hadoop-Cluster. Nur Linux-Cluster.
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)– Graphenverarbeitung zum Modellieren von Beziehungen zwischen Dingen oder Personen.
* [R](hdinsight-hadoop-r-scripts-linux.md)– eine Open-Source-Sprache und -Umgebung für statistische Berechnungen beim maschinellen Lernen.
* [Solr](hdinsight-hadoop-solr-install-linux.md)– eine Unternehmensplattform für die leistungsstarke Volltextsuche nach Daten.

Informationen zum Entwickeln eigener Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

## <a name="what-are-the-hadoop-components-and-utilities"></a>Aus welchen Komponenten und Dienstprogrammen besteht Hadoop?
Die folgenden Komponenten und Dienstprogramme sind in HDInsight-Clustern enthalten.

* **[Ambari](#ambari)**Bereitstellung, Verwaltung und Überwachung von Clustern sowie Hilfsprogramme für Cluster.
* **[Avro](#avro)** (Microsoft .NET Library für Avro): Datenserialisierung für die Microsoft .NET-Umgebung.
* **[Hive und HCatalog](#hive)**: SQL-ähnliche Abfragen und eine Tabellen- und Speicherverwaltungsschicht.
* **[Mahout](#mahout)**: Für skalierbare Machine Learning-Anwendungen.
* **[MapReduce](#mapreduce)**Legacy-Framework für verteilte Verarbeitung und Ressourcenverwaltung für Hadoop. Informationen finden Sie unter [YARN](#yarn), dem Ressourcenframework der nächsten Generation.
* **[Oozie](#oozie)**: Workflowverwaltung.
* **[Phoenix](#phoenix)**: Relationale Datenbankschicht über HBase.
* **[Pig](#pig)**: Einfachere Skripts für MapReduce-Transformationen.
* **[Sqoop](#sqoop)**: Import und Export von Daten.
* **[Tez](#tez)**: Effiziente Ausführung datenintensiver Prozesse.
* **[YARN](#yarn)**Teil der Hadoop-Kernbibliothek und nächste Generation des MapReduce-Softwareframeworks.
* **[ZooKeeper](#zookeeper)**: Koordination von Prozessen in verteilten Systemen.

> [!NOTE]
> Informationen zu den jeweiligen Komponenten und Versionsinformationen finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten, die in HDInsight verfügbar sind?][component-versioning].
>
>

### <a name="ambari"></a>Ambari
Apache Ambari dient zur Bereitstellung, Verwaltung und Überwachung von Apache Hadoop-Clustern. Es umfasst eine Sammlung intuitiver Operatortools und eine Reihe robuster APIs, welche die Komplexität von Hadoop verbergen und den Betrieb von Clustern vereinfachen. HDInsight-Cluster unter Linux stellen sowohl die Ambari-Webbenutzeroberfläche als auch die Ambari REST-API bereit, während Cluster unter Windows eine Teilmenge der REST-API bereitstellen. Ambari-Ansichten von HDInsight-Clustern ermöglichen Plug-In-Benutzerschnittstellenfunktionen.

Siehe [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md) (nur Linux), [Überwachen von Hadoop-Clustern in HDInsight mit der Ambari-API](hdinsight-monitor-use-ambari-api.md) und <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari-API-Referenz</a>.

### <a name="avro"></a>Avro (Microsoft .NET-Bibliothek für Avro)
Die Microsoft .NET-Bibliothek für Avro implementiert das kompakte Apache Avro-Datenaustauschformat für Binärdaten für die Microsoft .NET-Umgebung. Verwendet <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> zum Definieren eines sprachunabhängigen Schemas, das die Interoperabilität von Sprachen sicherstellt, sodass die in einer Sprache serialisierten Daten in einer anderen Sprache gelesen werden können. Ausführliche Informationen zum Format finden Sie in der <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro-Spezifikation</a>.
Das Format von Avro-Dateien unterstützt das verteilte MapReduce-Programmiermodell. Die Dateien sind "teilbar", d. h. es kann nach einem beliebigen Punkt in der Datei gesucht und ab einem beliebigen Block gelesen werden. Weitere Informationen hierzu finden Sie unter [Serialisieren von Daten mit der Microsoft .NET-Bibliothek für Avro](hdinsight-dotnet-avro-serialization.md).

### <a name="hdfs"></a>HDFS
Hadoop Distributed File System (HDFS) ist ein verteiltes Dateisystem, das zusammen mit MapReduce und YARN den Kern des Hadoop-Ökosystems bildet. HDFS ist das Standard-Dateisystem für Hadoop-Cluster in HDInsight.

### <a name="hive"></a>Hive und HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> ist eine auf Hadoop basierende Data Warehouse-Software, mit der Sie große Datasets in verteilten Speicherformen mithilfe einer SQL-ähnlichen Sprache namens HiveQL abfragen und verwalten können. Hive ist ebenso wie Pig eine auf MapReduce aufsetzende Abstraktion. Beim Ausführen übersetzt Hive Abfragen in eine Serie von MapReduce-Aufträgen. Das Hive-Konzept ähnelt eher einem Managementsystem für relationale Datenbanken als Pig und ist daher besser für strukturierte Daten geeignet. Für unstrukturierte Daten ist Pig die bessere Wahl. Siehe [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> ist eine Tabellen- und Speicherverwaltungsschicht für Hadoop, die Ihnen eine relationale Datenansicht bietet. In HCatalog können Sie Dateien in jedem Format lesen und schreiben, für das ein Hive-SerDe (Serialisierungs-/Deserialisierungsprogramm) geschrieben werden kann.

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> ist eine auf Hadoop basierende skalierbare Bibliothek mit Algorithmen für maschinelles Lernen. Anwendungen für maschinelles Lernen verwenden Grundsätze der Statistik, um aus Daten zu lernen und um zukünftiges Verhalten anhand vergangener Daten zu bestimmen. Siehe [Erstellen von Filmempfehlungen mithilfe von Mahout und Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce ist das Legacy-Softwareframework für Hadoop zum Schreiben von Anwendungen für die parallele Stapelverarbeitung großer Datasets. Ein MapReduce-Auftrag unterteilt große Datasets und verwaltet die Daten in Schlüssel-Wert-Paaren für die Verarbeitung.

[YARN](#yarn) ist das Ressourcen-Manager- und Anwendungsframework der nächsten Generation für Hadoop und wird als MapReduce 2.0 bezeichnet. MapReduce-Aufträge können mit YARN ausgeführt werden.

Weitere Informationen zu MapReduce finden Sie unter <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> im Hadoop-Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> ist ein Koordinationssystem für Workflows zur Verwaltung von Hadoop-Aufträgen. Es ist in den Hadoop-Stack integriert und unterstützt Hadoop-Jobs für MapReduce, Pig, Hive und Sqoop. Es kann auch dazu verwendet werden, bestimmte Jobs für ein System zu planen, beispielsweise Java-Programme oder Shellskripts. Weitere Informationen finden Sie unter [Verwenden von Oozie mit Hadoop](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> ist eine relationale Datenbankschicht über HBase. Phoenix umfasst einen JDBC-Treiber, mit dem Sie SQL-Tabellen direkt abfragen und verwalten können. Anstatt MapReduce zu verwenden, übersetzt Phoenix Abfragen und andere Anweisungen in systemeigene NoSQL-API-Aufrufe und ermöglicht so auf NoSQL-Speicher aufsetzend schnellere Anwendungen. Siehe [Verwenden von Apache Phoenix und SQuirreL mit HBase-Clustern](hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> ist eine allgemeine Plattform zum Anwenden komplexer MapReduce-Transformationen auf extrem große Datasets mithilfe einer einfachen Skriptsprache namens Pig Latin. Pig übersetzt Pig Latin-Skripts für die Ausführung in Hadoop. Sie können benutzerdefinierte Funktionen erstellen, um Pig Latin zu erweitern. Weitere Informationen finden Sie unter [Verwenden von Pig mit Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> ist ein Tool für die möglichst effektive Übertragung großer Datenmengen zwischen Hadoop und relationalen Datenbanken wie SQL oder anderen strukturierten Datenspeichern. Siehe [Verwenden von Sqoop mit Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> ist ein auf Hadoop YARN aufsetzendes Anwendungsframework für die Erstellung komplexer, azyklischer Diagramme der allgemeinen Datenverarbeitung. Es handelt sich hier um einen flexibleren und leistungsfähigeren Nachfolger des MapReduce-Frameworks, der auch bei datenintensiven Prozessen wie Hive eine effiziente Ausführung zulässt. Siehe ["Verwenden von Apache Tez zur Verbesserung der Leistung" unter "Verwenden von Hive und HiveQL"](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN ist die nächste Generation von MapReduce (MapReduce 2.0 bzw. MRv2) und unterstützt Datenverarbeitungsszenarien, die über die Stapelverarbeitung von MapReduce hinausgehen. YARN bietet mehr Skalierbarkeit und bessere Echtzeitverarbeitung. YARN bietet Ressourcenmanagement und ein verteiltes Anwendungsframework. MapReduce-Aufträge können mit YARN ausgeführt werden.

Weitere Informationen zu YARN finden Sie unter <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache Zookeeper</a> koordiniert Prozesse in großen verteilten Systemen mithilfe eines hierarchischen Namespace von Datenregistern (znodes). Znodes enthalten kleine Mengen an Metainformationen, die für die Koordination von Prozessen erforderlich sind: Status, Speicherort, Konfiguration usw.

## <a name="programming-languages-on-hdinsight"></a>Programmiersprachen in HDInsight
HDInsight-Cluster – Hadoop-, HBase-, Storm- und Spark-Cluster – unterstützen eine Vielzahl von Programmiersprachen, einige davon sind jedoch standardmäßig nicht installiert. Verwenden Sie eine Skriptaktion, um Bibliotheken, Module oder Pakete zu installieren, die standardmäßig nicht installiert sind. Weitere Informationen finden Sie unter [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Standardmäßige Unterstützung für Programmiersprachen
Standardmäßig unterstützen HDInsight-Cluster folgende Sprachen:

* Java
* Python

Zusätzliche Sprachen können mithilfe von Skriptaktionen installiert werden: [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>JVM-Sprachen (Java Virtual Machine)
Neben Java können viele weitere Sprachen mithilfe einer JVM (Java Virtual Machine) ausgeführt werden. Für einige dieser Sprachen müssen jedoch möglicherweise zusätzliche Komponenten im Cluster installiert werden.

Diese JVM-basierten Sprachen werden in HDInsight-Clustern unterstützt:

* Clojure
* Jython (Python für Java)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-spezifische Sprachen
HDInsight-Cluster bieten Unterstützung für die folgenden Sprachen, die für das Hadoop-Ökosystem spezifisch sind:

* Pig Latin für Pig-Aufträge
* HiveQL für Hive-Aufträge und SparkSQL

## <a name="advantage"></a>Vorzüge von Hadoop in der Cloud
Hadoop in HDInsight ist Teil des Azure Cloud-Ökosystems und bietet eine Vielzahl von Vorteilen, zum Beispiel:

* Automatische Bereitstellung von Hadoop-Clustern. HDInsight-Cluster sind viel einfacher zu erstellen als manuell konfigurierte Hadoop-Cluster. Weitere Informationen finden Sie unter [Bereitstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Modernste Hadoop-Komponenten. Weitere Informationen finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten, die in HDInsight verfügbar sind?][component-versioning].
* Hohe Verfügbarkeit und Zuverlässigkeit von Clustern. Weitere Details finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Hadoop-Clustern in HDInsight](hdinsight-high-availability-linux.md) .
* Effiziente und wirtschaftliche Datenspeicherung mit Azure Storage oder Azure Data Lake Store, zwei Hadoop-kompatiblen Optionen. Details finden Sie in den Artikeln zur [Verwendung von Azure Storage mit Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) bzw. zur [Verwendung von Data Lake Store mit einem HDInsight-Cluster](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).
* Integration mit anderen Azure-Diensten wie [Web-Apps](https://azure.microsoft.com/documentation/services/app-service/web/) und [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/).
* Zusätzliche VM-Größen und -Typen für die Ausführung von HDInsight-Clustern. Ausführliche Informationen finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten, die in HDInsight verfügbar sind?][component-versioning].
* Clusterskalierung. Mithilfe der Clusterskalierung können Sie die Anzahl der Knoten eines ausgeführten HDInsight-Clusters ändern, ohne ihn löschen oder neu erstellen zu müssen.
* Unterstützung für virtuelle Netzwerke. HDInsight-Cluster können mit virtuellen Azure-Netzwerken verwendet werden, um Cloud-Ressourcen zu isolieren oder um Hybrid-Lösungen zu unterstützen, die Cloud-Ressourcen mit Ressourcen in Ihrem Rechenzentrum verbinden.
* Niedrige Einstiegskosten. Probieren Sie die [kostenlose Testversion](https://azure.microsoft.com/free/) aus, oder entdecken Sie die [HDInsight-Preisdetails](https://azure.microsoft.com/pricing/details/hdinsight/).

Weitere Informationen zu den Vorteilen von Hadoop in HDInsight finden Sie auf der Seite mit [Azure-Features für HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard und HDInsight Premium
HDInsight bietet Cloudlösungen für Big Data in zwei Kategorien an: Standard und Premium. HDInsight Standard stellt einen Unternehmenscluster bereit, in dem Organisationen ihre Big Data-Workloads ausführen können. HDInsight Premium baut darauf auf und bietet erweiterte Analyse- und Sicherheitsfunktionen für einen HDInsight-Cluster. Weitere Informationen finden Sie unter [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a id="resources"></a>Ressourcen zum Veranschaulichen von Big Data-Analysen, Hadoop und HDInsight
Lesen Sie im Anschluss an diese Einführung in Hadoop in der Cloud und die Big Data-Analyse nachstehende Ressourcen.

### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop-Dokumentation für HDInsight
* [HDInsight-Dokumentation](https://azure.microsoft.com/documentation/services/hdinsight/): Die Dokumentationsseite für Hadoop in Azure HDInsight mit Links zu Artikeln, Videos und weiteren Ressourcen.
* [Erste Schritte mit Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): Ein Lernprogramm für die schnelle Bereitstellung von HDInsight-Hadoop-Clustern und Ausführung von Hive-Beispielabfragen.
* [Erste Schritte mit Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): Ein Schnellstart-Tutorial zum Erstellen eines Spark-Clusters und Ausführen von interaktiven Spark SQL-Abfragen.
* [Erste Schritte mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-get-started.md): Informationen zum Einstieg in die Verwendung von R Server in HDInsight Premium.
* [Bereitstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md): Erfahren Sie, wie ein HDInsight Hadoop-Cluster über das Azure-Portal, die Azure-Befehlszeilenschnittstelle (Azure-CLI) oder Azure PowerShell bereitgestellt wird.

### <a name="apache-hadoop"></a>Apache Hadoop
* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Erfahren Sie mehr über die Apache Hadoop-Softwarebibliothek, ein Framework, das die verteilte Verarbeitung großer Datasets in Computerclustern ermöglicht.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Erfahren Sie mehr über die Architektur und das Design von Hadoop Distributed File System (HDFS), dem primären Speichersystem, das von Hadoop-Anwendungen verwendet wird.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce Tutorial</a>: Erfahren Sie mehr über das Programmierframework für das Schreiben von Hadoop-Anwendungen, die riesige Datenmengen auf großen Compute-Knoten parallel verarbeiten.

### <a name="microsoft-business-intelligence"></a>Microsoft Business Intelligence
Bekannte Business-Intelligence-Tools wie Excel, PowerPivot, SQL Server Analysis Services und SQL Server Reporting Services rufen Daten, die in HDInsight integriert sind, entweder über das Add-In Power Query oder den Microsoft Hive-ODBC-Treiber ab, analysieren sie und erstellen einen Bericht.

Diese BI-Tools sind hilfreich für Ihre Big Data-Analyse:

* [Verbinden von Excel mit Hadoop über Power Query](hdinsight-connect-excel-power-query.md): Erfahren Sie, wie Sie Excel mithilfe von Microsoft Power Query mit dem Azure-Speicherkonto verbinden, in dem die Daten für Ihren HDInsight-Cluster gespeichert werden. Windows-Arbeitsstation ist erforderlich. Funktioniert mit Clustern unter Linux oder Windows.
* [Verbinden von Excel über den Microsoft Hive-ODBC-Treiber mit Hadoop](hdinsight-connect-excel-hive-odbc-driver.md): Informationen zum Importieren von Daten aus HDInsight mit dem Microsoft Hive-ODBC-Treiber. Windows-Arbeitsstation ist erforderlich. Funktioniert mit Clustern unter Linux oder Windows.
* [Microsoft-Cloudplattform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Entdecken Sie Power BI für Office 365, laden Sie die Testversion von SQL Server herunter, und richten Sie SharePoint Server 2013 und SQL Server BI ein.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).

[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/

