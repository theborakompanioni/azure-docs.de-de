---
title: Verwenden von Beeline zum Arbeiten mit Hive auf HDInsight (Hadoop) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mit SSH eine Verbindung zu einem Hadoop-Cluster in HDInsight herstellen und dann Hive-Abfragen mithilfe von Beeline interaktiv übermitteln. Beeline ist ein Dienstprogramm zum Arbeiten mit HiveServer2 über JDBC."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/05/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 7a1757a7ef2881b9e09389745a8e5aeaea2abf9d
ms.lasthandoff: 04/12/2017


---
# <a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Verwenden von Hive mit Hadoop in HDInsight über Beeline
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Erfahren Sie, wie Sie [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) verwenden, um Hive-Abfragen unter HDInsight auszuführen.

Beeline ist ein Befehlszeilentool, das auf den Hauptknoten des HDInsight-Clusters enthalten ist. Es verwendet JDBC, um eine Verbindung mit HiveServer2 herzustellen, einem Dienst, der auf Ihrem HDInsight-Cluster gehostet wird. Die folgende Tabelle enthält Verbindungszeichenfolgen für die Verwendung mit Beeline:

| Ausführungsort von Beeline | Verbindungszeichenfolge | Weitere Parameter |
| --- | --- | --- |
| Eine SSH-Verbindung mit einem Hauptknoten | `jdbc:hive2://localhost:10001/;transportMode=http` | `-n admin` |
| Ein Edgeknoten | `jdbc:hive2://headnodehost:10001/;transportMode=http` | `-n admin` |
| Außerhalb des Clusters | `jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2` | `-n admin -p password` |

> [!NOTE]
> Ersetzen Sie „admin“ durch das Clusteranmeldekonto für Ihren Cluster.
>
> Ersetzen Sie „password“ durch das Kennwort des Clusteranmeldekontos.
>
> Ersetzen Sie `clustername` durch den Namen Ihres HDInsight-Clusters.

## <a id="prereq"></a>Voraussetzungen

* Einen Linux-basierten Hadoop auf einem HDInsight-Cluster.

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight 3.2 und 3.3](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* Einen SSH-Client Linux, Unix und Mac OS sollten über einen SSH-Client verfügbar sein. Weitere Informationen zur Verwendung von SSH finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="ssh"></a>Verbinden mit SSH

Stellen Sie mit dem folgenden Befehl mithilfe von SSH eine Verbindung mit Ihrem Cluster her:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Ersetzen Sie `sshuser` durch das SSH-Konto für Ihren Cluster. Ersetzen Sie `myhdinsight` durch den Namen Ihres HDInsight-Clusters.

**Wenn Sie beim Erstellen des HDInsight-Clusters ein Kennwort für die SSH-Authentifizierung** angegeben haben, müssen Sie nach der entsprechenden Aufforderung das Kennwort angeben.

**Wenn Sie beim Erstellen des HDInsight-Clusters einen Zertifikatschlüssel für die SSH-Authentifizierung** angegeben haben, müssen Sie möglicherweise den Speicherort des privaten Schlüssel im Clientsystem angeben:

    ssh -i ~/.ssh/mykey.key ssh@myhdinsight-ssh.azurehdinsight.net

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Verwenden des Beeline-Befehls

1. Verwenden Sie in der SSH-Sitzung den folgenden Befehl, um Beeline zu starten:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Mit diesem Befehl wird der Beeline-Client gestartet und eine Verbindung mit dem HiveServer2 auf dem Hauptknoten des Clusters hergestellt. Mit dem `-n`-Parameter wird das Anmeldekonto für den Cluster bereitgestellt. Der Standardanmeldename ist `admin`. Wenn Sie während der Clustererstellung einen anderen Namen verwendet haben, verwenden sie diesen anstelle von `admin`.

    Nach Abschluss des Befehls gelangen Sie zur Eingabeaufforderung `jdbc:hive2://localhost:10001/>`.

2. Beeline-Befehle beginnen mit dem Zeichen `!`, z.B. `!help` zum Anzeigen der Hilfe. Jedoch kann `!` bei einigen Befehlen ausgelassen werden. `help` funktioniert beispielsweise auch.

    Es gibt ein `!sql`, das zum Ausführen von HiveQL-Anweisungen verwendet wird. HiveQL wird aber so häufig genutzt, dass Sie das vorangestellte `!sql` weglassen können. Die folgenden beiden Anweisungen sind gleichwertig:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    In einem neuen Cluster wird nur eine Tabelle aufgelistet: **hivesampletable**.

3. Verwenden Sie folgenden Befehl, um das Schema für „hivesampletable“ anzuzeigen:

    ```bash
    describe hivesampletable;
    ```

    Dieser Befehl gibt folgende Information zurück:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Diese Information beschreibt die Spalten in der Tabelle. Obwohl wir einige Abfragen für diese Daten ausführen konnten, werden wir stattdessen eine ganz neue Tabelle erstellen, um das Laden von Daten in Hive und das Anwenden eines Schemas zu veranschaulichen.

4. Geben Sie die folgenden Anweisungen ein, um eine Tabelle mit dem Namen **log4jLogs** zu erstellen, indem Sie die über das HDInsight-Cluster bereitgestellten Beispieldaten verwenden:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

    * **DROP TABLE**: Wenn die Tabelle vorhanden ist, wird sie gelöscht.

    * **CREATE EXTERNAL TABLE**: Erstellt eine **externe** Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort.

    * **ROW FORMAT**: Gibt an, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

    * **STORED AS TEXTFILE LOCATION**: Wo die Daten gespeichert werden und in welchem Dateiformat.

    * **SELECT**: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Diese Abfrage gibt den Wert **3** zurück, da dieser Wert in drei Zeilen enthalten ist.

    * **INPUT__FILE__NAME LIKE '%.log'**: Die Beispieldatendatei wird mit anderen Dateien gespeichert. Diese Anweisung beschränkt die Abfrage auf Daten in Dateien, die auf „LOG“ enden.

  > [!NOTE]
  > Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden. Das könnte z.B. ein automatisierter Datenupload oder ein MapReduce-Vorgang sein.
  >
  > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

    Die Ausgabe dieses Befehls ähnelt dem folgenden Text:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. Verwenden Sie `!exit`, um Beeline zu beenden.

## <a id="file"></a>Ausführen einer HiveQL-Datei

Verwenden Sie die folgenden Schritte, um eine Datei zu erstellen und sie dann mit Beeline auszuführen.

1. Verwenden Sie den folgenden Befehl, um eine Datei mit dem Namen **query.hql**zu erstellen:

    ```bash
    nano query.hql
    ```

2. Verwenden Sie als Inhalt der Datei den folgenden Text. Diese Abfrage erstellt eine neue „interne“ Tabelle mit dem Namen **errorLogs**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

    * **CREATE TABLE IF NOT EXISTS**: Wenn die Tabelle noch nicht vorhanden ist, wird sie erstellt. Da das **EXTERNAL**-Schlüsselwort nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle. Interne Tabellen werden im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet.
    * **ALS ORC GESPEICHERT** – Speichert die Daten im ORC-Format (Optimized Row Columnar). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.
    * **ÜBERSCHREIBEN EINFÜGEN ... SELECT**: Wählt die Zeilen aus der Tabelle **log4jLogs** aus, die den Wert **[ERROR]** enthalten. Dann werden die Daten in die Tabelle **errorLogs** eingefügt.

    > [!NOTE]
    > Anders als bei externen Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

3. Verwenden Sie **STRG**+**_X**, um die Datei zu speichern. Geben Sie dann **Y** ein, und drücken Sie die EINGABETASTE****.

4. Verwenden Sie Folgendes, um die Datei mit Beeline auszuführen: Ersetzen Sie **HOSTNAME** durch den Namen, den Sie bereits für den Hauptknoten abgerufen haben, und **PASSWORD** durch das Kennwort des Administratorkontos:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql
    ```

    > [!NOTE]
    > Der `-i`-Parameter startet Beeline und führt die Anweisungen in der Datei „query.hql“ aus. Nach Abschluss der Abfrage wird die Eingabeaufforderung `jdbc:hive2://localhost:10001/>` angezeigt. Sie können eine Datei auch mit dem `-f`-Parameter ausführen, der Beeline beendet, nachdem die Abfrage abgeschlossen ist.

5. Um zu überprüfen, ob die Tabelle **errorLogs** erstellt wurde, verwenden Sie die folgende Anweisung, um alle Zeilen aus **errorLogs** zurückzugeben:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Es sollten drei Datenzeilen zurückgegeben werden, die alle in Spalte „t4“ den Wert **[FEHLER]** enthalten:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="edge-nodes"></a>Edgeknoten

Wenn Ihr Cluster über einen Edgeknoten verfügt, sollten Sie beim Herstellen der Verbindung mit SSH immer den Edgeknoten statt des Hauptknotens verwenden. Um Beeline von einer SSH-Verbindung mit einem Edgeknoten aus zu starten, verwenden Sie folgenden Befehl:

```bash
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -n admin
```

## <a name="remote-clients"></a>Remoteclients

Wenn Sie Beeline lokal installiert haben oder über ein Docker-Image wie z.B. [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/) verwenden, müssen Sie die folgenden Parameter verwenden:

* __Verbindungszeichenfolge__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Clusteranmeldename__: `-n admin`

* __Clusteranmeldekennwort__: `-p 'password'`

Ersetzen Sie `clustername` in der Verbindungszeichenfolge durch den Namen des HDInsight-Clusters.

Ersetzen Sie `admin` durch Ihren Clusteranmeldenamen und `password` durch das zugehörige Kennwort.

## <a id="summary"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Hive in HDInsight finden Sie im folgenden Artikel:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Weitere Informationen zu anderen Methoden zur Verwendung von Hadoop in HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie Tez mit Hive verwenden, lesen Sie folgende Dokumente:

* [Verwenden der Tez-Benutzeroberfläche in Windows-basiertem HDInsight](hdinsight-debug-tez-ui.md)
* [Verwenden der Ambari-Tez-Ansicht in Linux-basiertem HDInsight](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

