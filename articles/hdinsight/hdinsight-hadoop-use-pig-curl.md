---
title: Verwenden von Hadoop Pig mit REST in HDInsight | Microsoft-Dokumentation
description: "Dieser Artikel erläutert, wie Sie mit REST Pig Latin-Aufträge in einem Hadoop-Cluster in Azure HDInsight ausführen können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: e729868bd64dfaa3f471f0c49f6a2c98aaf87215
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a>Ausführen von Pig-Aufträgen mit Hadoop in HDInsight mithilfe von REST

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Erfahren Sie hier, wie Sie mithilfe von REST-Anforderungen in einem Azure HDInsight-Cluster Pig Latin-Aufträge ausführen können. Curl dient zur Veranschaulichung der Interaktion mit HDInsight über die WebHCat REST-API.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, Ihnen HDInsight jedoch neu ist, finden Sie weitere Informationen unter [Tipps zu Linux-basiertem HDInsight](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Voraussetzungen

* Einen Azure HDInsight-Cluster (Hadoop in HDInsight), der auf Linux oder Windows basiert

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Ausführen von Pig-Aufträgen mit Curl

> [!NOTE]
> Die REST-API wird durch [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Anforderungen immer über HTTPS (Secure HTTP), um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.
>
> Ersetzen Sie beim Verwenden der Befehle in diesem Abschnitt die Option `USERNAME` zur Authentifizierung im Cluster durch den Benutzer und `PASSWORD` durch das Kennwort für das Benutzerkonto. Ersetzen Sie `CLUSTERNAME` durch den Namen Ihres Clusters.
>


1. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Daraufhin sollten Sie die folgende JSON-Antwort erhalten:

        {"status":"ok","version":"v1"}

    Folgende Parameter werden in diesem Befehl verwendet:

    * **-u**– Der Benutzername und das Kennwort für die Authentifizierung der Anforderung
    * **-G**: Gibt an, dass diese Anforderung eine GET-Anforderung ist.

     Der Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, ist für alle Anforderungen identisch. Der Pfad **/status** gibt an, dass die Anforderung den Status von WebHCat (auch bekannt als Templeton) für den Server zurückgibt.

2. Verwenden Sie den folgenden Code, um einen Pig Latin-Auftrag an den Cluster zu übermitteln.

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    Folgende Parameter werden in diesem Befehl verwendet:

    * **-d**: Da `-G` nicht verwendet wird, verwendet die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.

    * **user.name**: Der Benutzer, der den Befehl ausführt
    * **execute**: Die auszuführenden Pig Latin-Anweisungen
    * **statusdir**: Das Verzeichnis, in das die Statusangaben für diesen Auftrag geschrieben werden.

    > [!NOTE]
    > Beachten Sie, dass die Leerzeichen in Pig Latin-Anweisungen bei Curl durch das Zeichen `+` ersetzt werden.

    Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann. Beispiel:

        {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen:

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Ersetzen Sie `JOBID` durch den Wert, der im vorherigen Schritt zurückgegeben wurde. Wenn der Rückgabewert z.B. `{"id":"job_1415651640909_0026"}` lautet, ist `JOBID``job_1415651640909_0026`.

    Wenn der Auftrag abgeschlossen ist, wird der Status **ERFOLGREICH** angezeigt.

    > [!NOTE]
    > Diese Curl-Anforderung gibt ein JSON-Dokument (JavaScript Object Notation) mit Informationen zum Auftrag zurück, und mithilfe von "jq" wird nur der Statuswert abgerufen.

## <a id="results"></a>Anzeigen der Ergebnisse

Sobald der Status des Auftrags in **SUCCEEDED** wechselt, können Sie die Ergebnisse des Auftrags aus dem vom Cluster verwendeten Standardspeicher abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Fall ist dies `/example/pigcurl`.

HDInsight kann als Standarddatenspeicher entweder Azure Storage oder Azure Data Lake Store verwenden. Je nach verwendetem Standardspeicher gibt es verschiedene Möglichkeiten, die Daten abzurufen. Weitere Informationen finden Sie im Abschnitt „Speicher“ des Dokuments [Linux-basierte HDInsight-Informationen](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store).

## <a id="summary"></a>Zusammenfassung

Wie in diesem Dokument veranschaulicht, können Sie unformatierte HTTP-Anforderungen dazu verwenden, um Pig-Aufträge auf dem HDInsight-Cluster auszuführen, zu überwachen und deren Ergebnisse anzuzeigen.

Weitere Informationen zur REST-Schnittstelle, die in diesem Artikel verwendet wird, finden Sie in der [WebHCat-Referenz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Pig in HDInsight:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

