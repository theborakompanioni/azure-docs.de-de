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
ms.date: 01/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: b015e3d99c72c1840a31bacee5d9655fa9f48da7
ms.lasthandoff: 03/25/2017


---
# <a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Verwenden von Hive mit Hadoop in HDInsight über Beeline
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Sie mit SSH (Secure Shell) eine Verbindung mit einem Linux-basierten HDInsight-Cluster herstellen und dann Hive-Abfragen mithilfe des [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) -Befehlszeilentools interaktiv übermitteln.

> [!NOTE]
> Beeline verwendet JDBC für die Verbindung mit Hive. Weitere Informationen zur Verwendung von JDBC mit Hive finden Sie unter [Herstellen einer Verbindung mit Hive unter Azure HDInsight per Hive-JDBC-Treiber](hdinsight-connect-hive-jdbc-driver.md).

## <a id="prereq"></a>Voraussetzungen
Um die in diesem Artikel aufgeführten Schritte auszuführen, benötigen Sie Folgendes:

* Einen Linux-basierten Hadoop auf einem HDInsight-Cluster.

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Einen SSH-Client Linux, Unix und Mac OS sollten über einen SSH-Client verfügbar sein. Windows-Benutzer müssen einen Client herunterladen, z. B [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a id="ssh"></a>Verbinden mit SSH
Stellen Sie mithilfe des SSH-Befehls eine Verbindung zum vollqualifizierten Domänennamen (FQDN) des HDInsight-Clusters her. Der FQDN entspricht dem Namen, den Sie dem Cluster zugewiesen haben, gefolgt von **.azurehdinsight.net**. Mithilfe des folgenden Befehls wird z. B. eine Verbindung mit einem Cluster namens **myhdinsight** hergestellt:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie beim Erstellen des HDInsight-Clusters einen Zertifikatschlüssel für die SSH-Authentifizierung** angegeben haben, müssen Sie möglicherweise den Speicherort des privaten Schlüssel im Clientsystem angeben:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Wenn Sie beim Erstellen des HDInsight-Clusters ein Kennwort für die SSH-Authentifizierung** angegeben haben, müssen Sie nach der entsprechenden Aufforderung das Kennwort angeben.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Verwenden des Beeline-Befehls
1. Führen Sie nach dem Herstellen der Verbindung den folgenden Befehl aus, um Beeline zu starten:
   
        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
   
    Dadurch wird der Beeline-Client gestartet und eine Verbindung mit der JDBC-URL hergestellt. Hier wird `localhost` verwendet, da HiveServer2 auf beiden Hauptknoten im Cluster ausgeführt wird und wir Beeline direkt auf dem primären Hauptknoten ausführen.
   
    Nach Abschluss des Befehls gelangen Sie zur Eingabeaufforderung `jdbc:hive2://localhost:10001/>`.
2. Beeline-Befehle beginnen normalerweise mit dem Zeichen `!`, z. B. `!help` zum Anzeigen der Hilfe. Das `!` kann jedoch häufig weggelassen werden. `help` funktioniert beispielsweise auch.
   
    Beim Anzeigen der Hilfe sehen Sie `!sql` zum Ausführen von HiveQL-Anweisungen. HiveQL wird aber so häufig genutzt, dass Sie das vorangestellte `!sql` weglassen können. Die folgenden beiden Anweisungen liefern genau die gleichen Ergebnisse. Es werden die Tabellen angezeigt, die derzeit über Hive verfügbar sind:
   
        !sql show tables;
        show tables;
   
    In einem neuen Cluster sollte nur eine Tabelle aufgeführt sein: **hivesampletable**.
3. Verwenden Sie Folgendes, um das Schema für „hivesampletable“ anzuzeigen:
   
        describe hivesampletable;
   
    Die folgenden Informationen werden zurückgegeben:
   
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
   
    Hiermit werden die Spalten in der Tabelle angezeigt. Obwohl wir einige Abfragen für diese Daten ausführen konnten, werden wir stattdessen eine ganz neue Tabelle erstellen, um das Laden von Daten in Hive und das Anwenden eines Schemas zu veranschaulichen.
4. Geben Sie die folgenden Anweisungen ein, um eine neue Tabelle mit dem Namen **log4jLogs** zu erstellen, indem Sie die über das HDInsight-Cluster bereitgestellten Beispieldaten verwenden:
   
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   
    Diese Anweisungen führen die folgenden Aktionen aus:
   
   * **TABELLE LÖSCHEN** : Löscht Tabelle und Datendatei, falls die Tabelle bereits existiert.
   * **EXTERNE TABELLE ERSTELLEN** : Erstellt eine neue "externe" Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort.
   * **ZEILENFORMAT** : Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.
   * **SPEICHERORT DER TEXTDATEI** – Teilt Hive den Speicherort der Daten (das Verzeichnis "example/data") und die Information mit, dass die Speicherung als Text erfolgt.
   * **SELECT**: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Dadurch sollte der Wert **3** zurückgegeben werden, da dieser Wert in drei Zeilen enthalten ist.
   * **INPUT__FILE__NAME LIKE '%.log'**: Teilt Hive mit, dass nur Daten aus Dateien mit der Erweiterung „.log“ zurückgeben werden sollen. Normalerweise befinden sich in einem Ordner beim Abfragen mit Hive nur Daten mit dem gleichen Schema. Diese Beispielprotokolldatei wird aber mit anderen Datenformaten gespeichert.
     
     > [!NOTE]
     > Wenn erwartet wird, dass die zugrunde liegenden Daten über eine externe Quelle, z. B. einen automatisierten Prozess zum Hochladen von Daten, oder über einen anderen MapReduce-Vorgang aktualisiert, aber von Hive immer die neuesten Daten verwendet werden, sollten externe Tabellen verwendet werden.
     > 
     > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.
     > 
     > 
     
     Die Ausgabe dieses Befehls sollte ungefähr wie folgt aussehen:

     ```
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
     ```
5. Verwenden Sie `!quit`, um Beeline zu beenden.

## <a id="file"></a>Ausführen einer HiveQL-Datei
Beeline kann auch verwendet werden, um eine Datei auszuführen, die HiveQL-Anweisungen enthält. Verwenden Sie die folgenden Schritte, um eine Datei zu erstellen und sie dann mit Beeline auszuführen.

1. Verwenden Sie den folgenden Befehl, um eine neue Datei mit dem Namen **query.hql**zu erstellen:
   
        nano query.hql
2. Verwenden Sie nach dem Öffnen des Editors Folgendes als Inhalt für die Datei. Mit dieser Abfrage wird eine neue „interne“ Tabelle mit dem Namen **errorLogs**erstellt:
   
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   
    Diese Anweisungen führen die folgenden Aktionen aus:
   
   * **TABELLE ERSTELLEN, FALLS NICHT VORHANDEN** – Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Da das Schlüsselwort **EXTERN** nicht verwendet wird, ist dies eine "interne" Tabelle, die im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet wird.
   * **ALS ORC GESPEICHERT** – Speichert die Daten im ORC-Format (Optimized Row Columnar). Dies ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.
   * **ÜBERSCHREIBEN EINFÜGEN ... SELECT**: Wählt die Zeilen aus der Tabelle **log4jLogs** aus, die den Wert **[ERROR]** enthalten. Dann werden die Daten in die Tabelle **errorLogs** eingefügt.
     
     > [!NOTE]
     > Im Gegensatz zu externen Tabellen werden beim Ablegen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.
     > 
     > 
3. Verwenden Sie **STRG**+**_X**, um die Datei zu speichern. Geben Sie dann **Y** ein, und drücken Sie die EINGABETASTE****.
4. Verwenden Sie Folgendes, um die Datei mit Beeline auszuführen: Ersetzen Sie **HOSTNAME** durch den Namen, den Sie bereits für den Hauptknoten abgerufen haben, und **PASSWORD** durch das Kennwort des Administratorkontos:
   
        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql
   
   > [!NOTE]
   > Der `-i`-Parameter startet Beeline, führt die Anweisungen in der Datei „query.hql“ aus und verbleibt in Beeline an der Eingabeaufforderung `jdbc:hive2://localhost:10001/>`. Sie können auch eine Datei mit dem `-f` -Parameter ausführen, wodurch Sie zu Bash zurückkehren, nachdem die Datei verarbeitet wurde.
   > 
   > 
5. Um zu überprüfen, ob die Tabelle **errorLogs** erstellt wurde, verwenden Sie die folgende Anweisung, um alle Zeilen aus **errorLogs** zurückzugeben:
   
        SELECT * from errorLogs;
   
    Es sollten drei Datenzeilen zurückgegeben werden, die alle in Spalte „t4“ den Wert **[FEHLER]** enthalten:
   
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Weitere Informationen zur Beeline-Konnektivität
Für die Schritte in diesem Dokument wird `localhost` zum Herstellen einer Verbindung mit HiveServer2 verwendet, der auf dem Hauptknoten des Clusters ausgeführt wird. Sie können auch den Hostnamen oder den vollqualifizierten Domänennamen des Hauptknoten verwenden. In diesem Fall sind jedoch zusätzliche Schritte erforderlich (zum Ermitteln des Hostnamen oder FQDN). Bei Verwendung von Beeline über den Hauptknoten ist `localhost` ausreichend.

Wenn in Ihrem Cluster ein Edgeknoten mit Beeline vorhanden ist, müssen Sie den Hostnamen oder den FQDN des Hauptknotens für die Verbindung verwenden.

Wenn Beeline auf einem Client außerhalb Ihres Clusters installiert wurde, können Sie mithilfe des folgenden Befehls eine Verbindung herstellen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters. Ersetzen Sie **PASSWORD** durch das Kennwort des Administratorkontos (HTTP-Anmeldung).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Beachten Sie, dass die Parameter und der URI von den Parametern/URIs abweichen, die bei einer direkten Ausführung auf einem Hauptknoten oder über einen Edgeknoten innerhalb des Clusters verwendet werden. Der Grund dafür ist, dass beim Herstellen einer Verbindung mit dem Cluster über das Internet ein öffentliches Gateway verwendet wird, dass Datenverkehr über Port 443 leitet. Da eine Vielzahl anderer Dienste über das öffentliche Gateway an Port 443 verfügbar gemacht werden, weicht der URI zudem vom URI ab, der bei einer direkten Verbindung verwendet wird. Beim Herstellen einer Verbindung über das Internet muss für die Authentifizierung der Sitzung außerdem das Kennwort angegeben werden.

## <a id="summary"></a><a id="nextsteps"></a>Nächste Schritte
Sie haben gesehen, dass der Beeline-Befehl eine einfache Möglichkeit für die interaktive Ausführung von Hive-Abfragen in einem HDInsight-Cluster darstellt.

Allgemeine Informationen zu Hive in HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie mit Tez mit Hive verwenden, finden Sie in den folgenden Dokumenten Informationen zum Debuggen:

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


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


