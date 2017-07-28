---
title: "Verwenden von Hadoop Hive mit Curl in HDInsight – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Pig-Aufträge mithilfe von Curl remote an HDInsight übermitteln."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: afd0bc8c19456fd123f53de7d1704619405bed67
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017

---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Ausführen von Hive-Abfragen mit Hadoop in HDInsight mit REST

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Erfahren Sie, wie Sie die WebHCat-REST-API verwenden, um Hive-Abfragen mit Hadoop in Azure HDInsight-Clustern auszuführen.

[Curl](http://curl.haxx.se/) dient zum Veranschaulichen der Interaktion mit HDInsight über unformatierte HTTP-Anforderungen. Das Hilfsprogramm [q](http://stedolan.github.io/jq/) wird verwendet, um die von REST-Anforderungen zurückgegebenen JSON-Daten zu verarbeiten.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, jedoch noch nicht mit HDInsight, finden Sie weitere Informationen im Artikel [Was Sie über Hadoop auf Linux-basiertem HDInsight wissen müssen](hdinsight-hadoop-linux-information.md).

## <a id="curl"></a>Ausführen von Hive-Abfragen

> [!NOTE]
> Wenn Sie cURL oder eine andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, indem Sie den Benutzernamen und das Kennwort des Administrators des HDInsight-Clusters bereitstellen.
>
> Ersetzen Sie für die Befehle in diesem Abschnitt die Option **BENUTZERNAME** zur Authentifizierung im Cluster durch den Benutzer und die Option **KENNWORT** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters.
>
> Die REST-API wird durch [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Anforderungen immer über HTTPS (Secure HTTP), um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Sie empfangen eine Antwort, die in etwa dem folgenden Text entspricht:

        {"status":"ok","version":"v1"}

    Folgende Parameter werden in diesem Befehl verwendet:

   * **-u** – Der Benutzername und das Kennwort für die Authentifizierung der Anforderung
   * **-G**: Gibt an, dass diese Anforderung ein GET-Vorgang ist.

     Der Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, ist für alle Anforderungen identisch. Der Pfad, **/status**, gibt an, dass die Anforderung den Status von WebHCat (auch bekannt als Templeton) für den Server zurückgibt. Sie können mit dem folgenden Befehl auch die Version von Hive anfordern:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     Die Anforderung gibt in etwa folgenden Text zurück:

       {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Verwenden Sie Folgendes, um eine Tabelle namens **log4jLogs**zu erstellen:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Die folgenden Parameter werden mit dieser Anforderung verwendet:

   * **-d**: Da `-G` nicht verwendet wird, verwendet die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.

     * **user.name** – Der Benutzer, der den Befehl ausführt
     * **execute** – Die auszuführenden HiveQL-Anweisungen
     * **statusdir**: Das Verzeichnis, in das die Statusangaben für diesen Auftrag geschrieben werden.

     Diese Anweisungen führen die folgenden Aktionen aus:
   * **DROP TABLE**: Wenn die Tabelle bereits vorhanden ist, wird sie gelöscht.
   * **EXTERNE TABELLE ERSTELLEN** : Erstellt eine neue "externe" Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. Die Daten verbleiben an ihrem ursprünglichen Speicherort.

     > [!NOTE]
     > Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden. Das könnte z.B. ein automatisierter Datenupload oder ein anderer MapReduce-Vorgang sein.
     >
     > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

   * **ROW FORMAT**: Gibt an, wie die Daten formatiert werden. Die Felder werden in den einzelnen Protokollen durch Leerzeichen getrennt.
   * **STORED AS TEXTFILE LOCATION**: Gibt den Speicherort der Daten (das Verzeichnis „example/data“) und die Information an, dass die Speicherung als Text erfolgt.
   * **SELECT**: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte **t4** den Wert **[ERROR]** enthält. Mit dieser Anweisung wird der Wert **3** zurückgegeben, da dieser Wert in drei Zeilen enthalten ist.

     > [!NOTE]
     > Beachten Sie, dass die Leerzeichen zwischen HiveQL-Anweisungen bei Curl durch das Zeichen `+` ersetzt werden. Werte in Anführungszeichen, die ein Leerzeichen enthalten, z. B. das Trennzeichen, dürfen nicht durch `+` ersetzt werden.

   * **INPUT__FILE__NAME LIKE '%25.log'**: Diese Anweisung begrenzt die Suche auf Dateien mit der Erweiterung „.log“.

     > [!NOTE]
     > `%25` entspricht der URL-codierten Form von „%“. Daher lautet die tatsächliche Bedingung `like '%.log'`. "%" muss URL-codiert werden, da dieses Zeichen in URLs als Sonderzeichen behandelt wird.

     Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann.

       {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Ersetzen Sie **JOBID** durch den Wert, der im vorherigen Schritt zurückgegeben wurde. Wenn der Rückgabewert z. B. `{"id":"job_1415651640909_0026"}` lautet, ist die **JOBID** `job_1415651640909_0026`.

    Wenn der Auftrag abgeschlossen ist, wird der Status **ERFOLGREICH** angezeigt.

   > [!NOTE]
   > Diese Curl-Anforderung gibt ein JSON-Dokument (JavaScript Object Notation) mit Informationen zum Auftrag zurück. Mithilfe von „jq“ wird nur der Statuswert abgerufen.

4. Sobald der Status des Auftrags zu **ERFOLGREICH** wechselt, können Sie die Ergebnisse des Auftrags aus dem Azure Blob-Speicher abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Fall ist dies **/example/curl**. Diese Adresse speichert die Ausgabe im Verzeichnis **Beispiel/Curl** im Standardspeicher des Clusters.

    Sie können diese Dateien mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) auflisten und herunterladen. Weitere Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle mit Azure Storage finden Sie im Artikel [Verwenden der Azure CLI 2.0 mit Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs).

5. Verwenden Sie die folgenden Anweisungen zum Erstellen einer neuen "internen" Tabelle namens **errorLogs**.

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

   * **TABELLE ERSTELLEN, FALLS NICHT VORHANDEN** – Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Diese Anweisung erstellt eine interne Tabelle, die im Hive-Data Warehouse gespeichert und vollständig von Hive verwaltet wird.

     > [!NOTE]
     > Anders als bei externen Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

   * **ALS ORC GESPEICHERT** – Speichert die Daten im ORC-Format (Optimized Row Columnar). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.
   * **ÜBERSCHREIBEN EINFÜGEN ... SELECT**: Wählt die Zeilen aus der Tabelle **log4jLogs** aus, die den Wert **[ERROR]** enthalten. Dann werden die Daten in die Tabelle **errorLogs** eingefügt.
   * **SELECT**: Wählt alle Zeilen aus der neuen Tabelle **errorLogs** aus.

6. Verwenden Sie die zurückgegebene Auftrags-ID, um den Status des Auftrags zu überprüfen. Nach der erfolgreichen Ausführung verwenden Sie die Azure-Befehlszeilenschnittstelle wie zuvor beschrieben, um die Ergebnisse herunterzuladen und anzuzeigen. Die Ausgabe sollte drei Zeilen mit dem Inhalt **[ERROR]**enthalten.

## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Hive mit HDInsight:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie mit Tez mit Hive verwenden, finden Sie in den folgenden Dokumenten Informationen zum Debuggen:

* [Verwenden der Ambari-Tez-Ansicht in Linux-basiertem HDInsight](hdinsight-debug-ambari-tez-view.md)

Weitere Informationen zu der in diesem Artikel verwendeten REST-API finden Sie in der [WebHCat-Referenz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

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




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx



