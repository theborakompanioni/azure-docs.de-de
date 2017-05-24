---
title: Was ist Hive und wie wird HiveQL verwendet? | Microsoft Docs
description: "Informieren Sie sich über Apache Hive und dessen Verwendung mit Hadoop in HDInsight. Wählen Sie die richtige Ausführung von Hive-Aufträgen, und verwenden Sie HiveQL zum Analysieren einer Apache-Log4j-Beispieldatei."
keywords: HiveQL,was ist Hive
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
ms.date: 04/14/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: db324cc8269a1f23613e5193f5a1dec1d26a62b1
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017


---
# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight"></a>Verwenden von Hive und HiveQL mit Hadoop in HDInsight

Erfahren Sie, wie Sie Hive mit HDInsight verwenden. In der folgenden Tabelle finden Sie verschiedene Möglichkeiten der Verwendung von Hive mit HDInsight:

| **Verwenden Sie dies**, wenn Sie Folgendes wünschen: | ... eine **interaktive** Shell | ...**Batchverarbeitung** | ...mit diesem **Clusterbetriebssystem** | ...von diesem **Clusterbetriebssystem** |
|:--- |:---:|:---:|:--- |:--- |
| [Struktur anzeigen](hdinsight-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |Alle (browserbasiert) |
| [Beeline-Befehl](hdinsight-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X oder Windows |
| [REST-API](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux oder Windows |Linux, Unix, Mac OS X oder Windows |
| [Abfragekonsole](hdinsight-hadoop-use-hive-query-console.md) (HDInsight 3.2 und 3.3) |&nbsp; |✔ |Windows |Alle (browserbasiert) |
| [HDInsight-Tools für Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |✔ |Linux oder Windows |Windows |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux oder Windows |Windows |
| [Remotedesktop](hdinsight-hadoop-use-hive-remote-desktop.md) (HDInsight 3.2 und 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight 3.2 und 3.3](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a id="why"></a>Was ist Hive?

[Apache Hive](http://hive.apache.org/) ist ein Data Warehouse-System für Hadoop, das die Zusammenfassung, Abfrage und Analyse von Daten ermöglicht. Hive ermöglicht Ihnen die Strukturierung größtenteils unstrukturierter Daten. Nachdem Sie die Struktur definiert haben, können Sie mit Hive Daten abfragen, ohne Java- oder MapReduce-Kenntnisse zu besitzen.

Hive-Abfragen werden in HiveQL geschrieben, einer SQL-ähnlichen Abfragesprache. Hive kann verwendet werden, um Daten interaktiv zu durchsuchen oder um mehrmals durchführbare Batchverarbeitungsaufträge zu erstellen.

Hive kann mit strukturierten und semistrukturierten Daten arbeiten, z. B. Textdateien, in denen die Felder durch bestimmte Zeichen getrennt sind. Hive unterstützt auch benutzerdefinierte **Serialisierer/Deserialisierer (SerDe)** für komplexe oder unregelmäßig strukturierte Daten. Weitere Informationen finden Sie im Artikel [Verwenden eines benutzerdefinierten JSON-SerDe mit HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Benutzerdefinierte Funktionen (User Defined Functions, UDFs)

Hive kann auch über **benutzerdefinierte Funktionen (UDF)**erweitert werden. Mit einer UDF-Datei können Sie Funktionen oder Logik implementieren, die sich nicht einfach in HiveQL modellieren lässt. Ein Beispiel für benutzerdefinierte Funktionen mit Hive finden Sie in den folgenden Artikeln:

* [Verwenden einer benutzerdefinierten Java-Funktion mit Hive](hdinsight-hadoop-hive-java-udf.md)

* [Verwenden von Python mit Hive und Pig in HDInsight](hdinsight-python.md)

* [Verwenden von C# mit Hive und Pig in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [How to add a custom Hive UDF to HDInsight (in englischer Sprache)](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Custom Hive UDF example to convert date/time formats to Hive timestamp](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Interne und externe Hive-Tabellen

Folgendes sollten Sie über interne und externe Hive-Tabellen wissen:

* Mit dem Befehl **CREATE TABLE** erstellen Sie eine interne Tabelle. Die Datendatei muss sich im Standardcontainer befinden.

* Der Befehl **CREATE TABLE** verschiebt die Datendatei in das Verzeichnis `/hive/warehouse/<TableName>` im Standardspeicher für den Cluster.

* Mit dem Befehl **CREATE EXTERNAL TABLE** wird eine externe Tabelle erstellt. Die Datendatei kann sich außerhalb des Standardcontainers befinden.

* Beim Befehl **CREATE EXTERNAL TABLE** wird die Datendatei nicht verschoben.

Weitere Informationen finden Sie unter [HDInsight: Einführung in interne und externe Tabellen in Hive][cindygross-hive-tables].

## <a id="data"></a>Beispieldaten

HDInsight enthält verschiedene Beispieldatasets, die in den Verzeichnissen `/example/data` und `/HdiSamples` gespeichert sind. Diese Verzeichnisse befinden sich im Standardspeicher für den Cluster. In diesem Artikel wird die Datei `/example/data/sample.log` verwendet. Diese Datei ist eine *log4j*-Datei. Jedes Protokoll innerhalb der Datei besteht aus einer Reihe von Feldern, unter denen sich ein Feld namens `[LOG LEVEL]` befindet, das die Art und den Schweregrad des jeweiligen Fehlers anzeigt, beispielsweise:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Die Protokollebene im vorherigen Beispiel ist ERROR.

> [!NOTE]
> Sie können auch eine Log4j-Datei generieren, indem Sie das [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) -Protokollierungstool verwenden und die Datei dann in Ihren Blobcontainer hochladen. Anweisungen hierzu finden Sie unter [Hochladen von Daten in HDInsight](hdinsight-upload-data.md) . Weitere Informationen zur Verwendung des Azure-Blobspeichers mit HDInsight finden Sie unter [Verwenden von Azure-Blobspeicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Hive-Beispielabfrage

Die folgenden HiveQL-Anweisungen projizieren Spalten in die Datei `/example/data/sample.log`:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Im vorherigen Beispiel führen die HiveQL-Anweisungen die folgenden Aktionen aus:

* **set hive.execution.engine=tez;**: Legt die Verwendung von Tez durch das Ausführungsmodul fest. Durch die Verwendung von Tez anstelle von MapReduce kann eine höhere Abfrageleistung erreicht werden. Weitere Informationen zu Tez finden Sie im Abschnitt [Verwenden von Apache Tez zur Verbesserung der Leistung](#usetez) .

    > [!NOTE]
    > Diese Anweisung ist nur erforderlich, wenn Sie einen Windows-basierten HDInsight-Cluster verwenden. Tez ist das Standardausführungsmodul für Linux-basiertes HDInsight.

* **DROP TABLE**: Wenn die Tabelle bereits vorhanden ist, wird sie gelöscht.

* **CREATE EXTERNAL TABLE**: Erstellt eine neue **externe** Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort und behalten das Originalformat bei.

* **ZEILENFORMAT**: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

* **STORED AS TEXTFILE LOCATION**: Teilt Hive den Speicherort der Daten (das Verzeichnis `example/data`) und die Information mit, dass die Speicherung als Text erfolgt. Die Daten können sich in einer Datei befinden oder auf mehrere Dateien im Verzeichnis verteilt sein.

* **SELECT**: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Mit dieser Anweisung wird der Wert **3** zurückgegeben, da dieser Wert in drei Zeilen enthalten ist.

* **INPUT__FILE__NAME LIKE '%.log'**: Teilt Hive mit, dass nur Daten aus Dateien mit der Erweiterung „.log“ zurückgeben werden sollen. Diese Anweisung schränkt die Suche auf die Datei `sample.log` ein, die die Daten enthält.

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

* **CREATE TABLE IF NOT EXISTS**: Wenn die Tabelle nicht vorhanden ist, wird sie erstellt. Da das Schlüsselwort **EXTERN** nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle, die im Hive-Data-Warehouse gespeichert und vollständig von Hive verwaltet wird.

* **ALS ORC GESPEICHERT**: Speichert die Daten im ORC-Format (Optimized Row Columnar). Dies ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.

* **ÜBERSCHREIBEN EINFÜGEN ...** SELECT**: Wählt Zeilen in der Tabelle **log4jLogs **aus, die** [ERROR] **enthalten. Dann werden die Daten in die Tabelle **errorLogs eingefügt.

> [!NOTE]
> Anders als bei externen Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

## <a id="usetez"></a>Verwenden von Apache Tez zur Verbesserung der Leistung

[Apache Tez](http://tez.apache.org) ist ein Framework, mit dem datenintensive Anwendungen wie Hive wesentlich effizienter ausgeführt und skaliert werden können. In der neuesten Version von HDInsight unterstützt Hive die Ausführung in Tez. Tez ist für Linux-basierte HDInsight-Cluster standardmäßig aktiviert.

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

## <a id="run"></a>Ausführen des HiveQL-Auftrags

HDInsight kann HiveQL-Aufträge mithilfe verschiedener Methoden ausführen. Die folgende Tabelle hilft Ihnen bei der Entscheidung, welche Methode für Sie geeignet ist. Folgen Sie anschließend dem Link für eine exemplarische Vorgehensweise.

| **Verwenden Sie dies**, wenn Sie Folgendes wünschen: | ... eine **interaktive** Shell | ...**Batchverarbeitung** | ...mit diesem **Clusterbetriebssystem** | ...von diesem **Clusterbetriebssystem** |
|:--- |:---:|:---:|:--- |:--- |
| [Struktur anzeigen](hdinsight-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |Alle (browserbasiert) |
| [Beeline-Befehl](hdinsight-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X oder Windows |
| [REST-API](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux oder Windows |Linux, Unix, Mac OS X oder Windows |
| [Abfragekonsole](hdinsight-hadoop-use-hive-query-console.md) (HDInsight 3.2 und 3.3) |&nbsp; |✔ |Windows |Alle (browserbasiert) |
| [HDInsight-Tools für Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |✔ |Linux oder Windows |Windows |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux oder Windows |Windows |
| [Remotedesktop](hdinsight-hadoop-use-hive-remote-desktop.md) (HDInsight 3.2 und 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight 3.2 und 3.3](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a name="hive-jobs-and-sql-server-integration-services"></a>Hive-Aufträge und SQL Server Integration Services

Sie können auch mit den SQL Server Integration Services (SSIS) einen Hive-Auftrag ausführen. Das Azure Feature Pack für SSIS bietet die folgenden Komponenten, die mit Hive-Aufträgen in HDInsight funktionieren.

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

