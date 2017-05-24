---
title: "Was sind Apache Hive und HiveQL – Azure HDInsight | Microsoft-Dokumentation"
description: "Apache Hive ist ein Data Warehouse-System für Hadoop. Sie können in Hive gespeicherte Daten mithilfe von HiveQL abrufen, die Transact-SQL ähnelt. Erfahren Sie in diesem Dokument, wie Sie Hive und HiveQL mit Azure HDInsight verwenden."
keywords: HiveQL, was ist Hive, Hadoop HiveQL, Verwenden von Hive, Hive lernen, was ist Hive
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/12/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: b8c32f6e15d65efc8dc464017027d3cde94b2667
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Was sind Apache Hive und HiveQL in Azure HDInsight?

[Apache Hive](http://hive.apache.org/) ist ein Data Warehouse-System für Hadoop. Hive ermöglicht das Zusammenfassen, Abfragen und Analysen von Daten. Hive-Abfragen werden in HiveQL geschrieben, einer SQL-ähnlichen Abfragesprache.

Hive ermöglicht Ihnen die Strukturierung größtenteils unstrukturierter Daten. Nachdem Sie die Struktur definiert haben, können Sie mit HiveQL Daten abfragen, ohne Java- oder MapReduce-Kenntnisse zu besitzen.

HDInsight bietet verschiedene Clustertypen, die für bestimmte Workloads optimiert sind. Die folgenden Clustertypen werden am häufigsten für Hive-Abfragen verwendet:

* __Interactive Hive:__ Hadoop-Cluster mit [LLAP-Funktionalität (Low Latency Analytical Processing)](https://cwiki.apache.org/confluence/display/Hive/LLAP) zur Verbesserung der Antwortzeiten für interaktive Abfragen. Weitere Informationen finden Sie unter [Einstieg in Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md).

* __Hadoop:__ Hadoop-Cluster, der für die Batchverarbeitung von Workloads optimiert ist. Weitere Informationen finden Sie unter [Einstieg in Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

* __Spark:__ Apache Spark verfügt über integrierte Funktionen für die Arbeit mit Hive. Weitere Informationen finden Sie unter [Einstieg in Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

* __HBase:__ HiveQL kann zum Abfragen von Daten verwendet werden, die in HBase gespeichert sind. Weitere Informationen finden Sie unter [Einstieg in HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md).

## <a name="how-to-use-hive"></a>Verwenden von Hive

In der folgenden Tabelle finden Sie Informationen zur Verwendung von Hive mit HDInsight:

| **Verwenden Sie diese Methode** für ... | ... eine **interaktive** Shell | ...**Batchverarbeitung** | ...mit diesem **Clusterbetriebssystem** | ...von diesem **Clusterbetriebssystem** |
|:--- |:---:|:---:|:--- |:--- |
| [Struktur anzeigen](hdinsight-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |Alle (browserbasiert) |
| [Beeline-Client](hdinsight-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X oder Windows |
| [REST-API](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux oder Windows* |Linux, Unix, Mac OS X oder Windows |
| [HDInsight-Tools für Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |✔ |Linux oder Windows* |Windows |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux oder Windows* |Windows |

> [!IMPORTANT]
> \*Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight 3.3](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).
>
> Wenn Sie einen Windows-basierten HDInsight-Cluster verwenden, können Sie mithilfe der [Abfragekonsole](hdinsight-hadoop-use-hive-query-console.md) in Ihrem Browser oder [Remotedesktop](hdinsight-hadoop-use-hive-remote-desktop.md) Hive-Abfragen ausführen.

## <a name="hiveql-language-reference"></a>Referenz zu HiveQL

Sie finden die HiveQL-Sprachreferenz im [Sprachhandbuch (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Hive und Datenstruktur

Hive kann mit strukturierten und halbstrukturierten Daten arbeiten. Dazu gehören z.B. Textdateien, in denen die Felder durch bestimmte Zeichen getrennt sind. Die folgende HiveQL-Anweisung erstellt eine Tabelle aus Daten, die durch Leerzeichen getrennt sind:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive unterstützt auch benutzerdefinierte **Serialisierer/Deserialisierer (SerDe)** für komplexe oder unregelmäßig strukturierte Daten. Weitere Informationen finden Sie im Artikel [Verwenden eines benutzerdefinierten JSON-SerDe mit HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

Weitere Informationen zu den von Hive unterstützten Dateiformaten finden Sie im [Sprachhandbuch (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-internal-tables-vs-external-tables"></a>Interne und externe Hive-Tabellen

Sie können mit Hive zwei Arten von Tabellen erstellen:

* __Intern:__ Die Daten werden im Hive-Data Warehouse gespeichert. Dieses Data Warehouse befindet sich unter `/hive/warehouse/` im Standardspeicher für den Cluster.

    Verwenden Sie interne Tabellen in folgenden Fällen:

    * Die Daten sind temporär.
    * Hive soll den Lebenszyklus der Tabelle und der Daten verwalten.

* __Extern:__ Die Daten werden außerhalb des Hive-Data Warehouse gespeichert. Die Daten können auf einem beliebigen Speicher gespeichert werden, der vom Cluster aus erreichbar ist.

    Verwenden Sie externe Tabellen in folgenden Fällen:

    * Die Daten werden auch außerhalb von Hive verwendet. Beispielsweise werden die Datendateien von einem anderen Prozess aktualisiert (der die Dateien nicht sperrt).
    * Die Daten müssen am zugrunde liegenden Speicherort verbleiben – auch nach dem Löschen der Tabelle.
    * Sie benötigen einen benutzerdefinierten Speicherort, z.B. ein anderes Speicherkonto als das Standardkonto.
    * Ein anderes Programm als Hive verwaltet das Datenformat, den Speicherort usw.

Weitere Informationen finden Sie im Blogbeitrag [Einführung in interne und externe Tabellen in Hive][cindygross-hive-tables].

## <a name="user-defined-functions-udf"></a>Benutzerdefinierte Funktionen (User Defined Functions, UDFs)

Hive kann auch über **benutzerdefinierte Funktionen (UDF)**erweitert werden. Mit einer UDF-Datei können Sie Funktionen oder Logik implementieren, die sich nicht einfach in HiveQL modellieren lässt. Ein Beispiel für benutzerdefinierte Funktionen mit Hive finden Sie in den folgenden Artikeln:

* [Verwenden einer benutzerdefinierten Java-Funktion mit Hive](hdinsight-hadoop-hive-java-udf.md)

* [Verwenden einer benutzerdefinierten Python-Funktion mit Hive und Pig](hdinsight-python.md)

* [Verwenden einer benutzerdefinierten C#-Funktion mit Hive und Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [How to add custom Hive UDFs to HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx) (Hinzufügen benutzerdefinierter Hive-Funktionen in HDInsight)

* [Beispiel für eine benutzerdefinierte Hive-Funktion für das Umwandeln von Datums-/Uhrzeitformaten in Hive-Zeitstempel](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Beispieldaten

Hive für HDInsight bietet eine vorab geladene, interne Tabelle mit dem Namen `hivesampletable`. HDInsight stellt auch Beispieldatasets bereit, die mit Hive verwendet werden können. Diese Datasets sind in den Verzeichnissen `/example/data` und `/HdiSamples` gespeichert. Diese Verzeichnisse befinden sich im Standardspeicher für den Cluster.

## <a id="job"></a>Hive-Beispielabfrage

Die folgenden HiveQL-Anweisungen projizieren Spalten in die Datei `/example/data/sample.log`:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Im vorherigen Beispiel führen die HiveQL-Anweisungen die folgenden Aktionen aus:

* `set hive.execution.engine=tez;`: Legt das Ausführungsmodul auf Tez fest. Durch die Verwendung von Tez anstelle von MapReduce kann eine höhere Abfrageleistung erreicht werden. Weitere Informationen zu Tez finden Sie im Abschnitt [Verwenden von Apache Tez zur Verbesserung der Leistung](#usetez) .

    > [!NOTE]
    > Diese Anweisung ist nur erforderlich, wenn Sie einen Windows-basierten HDInsight-Cluster verwenden. Tez ist das Standardausführungsmodul für Linux-basiertes HDInsight.

* `DROP TABLE`: Wenn die Tabelle bereits vorhanden ist, wird sie gelöscht.

* `CREATE EXTERNAL TABLE`: Erstellt eine neue **externe** Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort und behalten das Originalformat bei.

* `ROW FORMAT`: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

* `STORED AS TEXTFILE LOCATION`: Teilt Hive den Speicherort der Daten (das Verzeichnis `example/data`) und die Information mit, dass die Speicherung als Text erfolgt. Die Daten können sich in einer Datei befinden oder auf mehrere Dateien im Verzeichnis verteilt sein.

* `SELECT`: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Mit dieser Anweisung wird der Wert **3** zurückgegeben, da dieser Wert in drei Zeilen enthalten ist.

* `INPUT__FILE__NAME LIKE '%.log'`: Hive versucht, das Schema auf alle Dateien im Verzeichnis anzuwenden. In diesem Fall enthält das Verzeichnis Dateien, die dem Schema nicht entsprechen. Um überflüssige Daten in den Ergebnissen zu vermeiden, weist diese Anweisung Hive an, nur Daten aus Dateien zurückzugeben, die auf „.log“ enden.

> [!NOTE]
> Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden. Das könnte z.B. ein automatisierter Datenupload oder ein MapReduce-Vorgang sein.
>
> Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

Zum Erstellen einer **internen** anstelle einer externen Tabelle, verwenden Sie den folgenden HiveQL-Code:

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Diese Anweisungen führen die folgenden Aktionen aus:

* `CREATE TABLE IF NOT EXISTS`: Wenn die Tabelle nicht vorhanden ist, wird sie erstellt. Da das Schlüsselwort **EXTERNAL** nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle. Die Tabelle wird im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet.

* `STORED AS ORC`: Speichert die Daten im ORC-Format (Optimized Row Columnar). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.

* `INSERT OVERWRITE ... SELECT`: Wählt Zeilen in der Tabelle **log4jLogs** aus, die **[ERROR]** enthalten, und fügt die Daten in die Tabelle **errorLogs** ein.

> [!NOTE]
> Anders als bei externen Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

## <a name="improve-hive-query-performance"></a>Verbessern der Hive-Abfrageleistung

### <a id="usetez"></a>Apache Tez

[Apache Tez](http://tez.apache.org) ist ein Framework, mit dem datenintensive Anwendungen wie Hive wesentlich effizienter ausgeführt und skaliert werden können. Tez ist für Linux-basierte HDInsight-Cluster standardmäßig aktiviert.

> [!NOTE]
> Tez ist derzeit für Windows-basierte HDInsight-Cluster standardmäßig deaktiviert und muss aktiviert werden. Um die Vorteile von Tez zu nutzen, muss der folgende Wert in einer Hive-Abfrage gesetzt sein:
>
> `set hive.execution.engine=tez;`
>
> Tez ist das Standardmodul für Linux-basierte HDInsight-Cluster.

Die [Hive mit Tez-Design-Dokumenten](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) enthalten Details zu Implementierungsauswahl und Konfigurationseinstellungen.

Zur Unterstützung des Debuggings von in Tez ausgeführten Aufträgen bietet HDInsight die folgenden Webbenutzeroberflächen zum Anzeigen von Details zu Tez-Aufträgen:

* [Verwenden der Ambari-Tez-Ansicht in Linux-basiertem HDInsight](hdinsight-debug-ambari-tez-view.md)

* [Verwenden der Tez-Benutzeroberfläche in Windows-basiertem HDInsight](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a>Low Latency Analytical Processing (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (auch als „Live Long and Process“ bezeichnet) ist ein neues Feature in Hive 2.0, das die Zwischenspeicherung von Abfragen im Arbeitsspeicher ermöglicht. LLAP macht Hive-Abfragen deutlich schneller – bis zu [in einigen Fällen 26-mal schneller als Hive 1.x](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight stellt LLAP im Clustertyp „Interactive Hive“ bereit. Weitere Informationen finden Sie unter [Einstieg in Interactive Hive](hdinsight-hadoop-use-interactive-hive.md).

## <a name="hive-jobs-and-sql-server-integration-services"></a>Hive-Aufträge und SQL Server Integration Services

Sie können mit SQL Server Integration Services (SSIS) einen Hive-Auftrag ausführen. Das Azure Feature Pack für SSIS bietet die folgenden Komponenten, die mit Hive-Aufträgen in HDInsight funktionieren.

* [Hive-Aufgabe in Azure HDInsight][hivetask]

* [Verbindungs-Manager für Azure-Abonnements][connectionmanager]

[Hier][ssispack] erfahren Sie mehr zum Azure Feature Pack für SSIS.

## <a id="nextsteps"></a>Nächste Schritte

Nachdem Sie erfahren haben, was Hive ist und wie Sie es mit Hadoop in HDInsight verwenden, können Sie mithilfe der nachfolgenden Links andere Möglichkeiten für die Arbeit mit Azure HDInsight untersuchen.

* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Verwenden von Pig mit HDInsight][hdinsight-use-pig]
* [Verwenden von MapReduce-Aufträgen mit HDInsight][hdinsight-use-mapreduce]

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

