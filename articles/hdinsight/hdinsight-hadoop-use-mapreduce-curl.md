---
title: "Verwenden von MapReduce und Curl mit Hadoop in HDInsight – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie MapReduce-Aufträge mit Hadoop in HDInsight mithilfe von Curl remote ausführen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
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
ms.openlocfilehash: df22c7315cc55df0e7963bc03a215ab09589b25b
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017

---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>Ausführen von MapReduce-Aufträgen mit Hadoop in HDInsight mithilfe von REST

Erfahren Sie, wie mithilfe der WebHCat-REST-API MapReduce-Aufträge auf einem Hadoop-Cluster in HDInsight ausführen. Curl dient zum Veranschaulichen der Interaktion mit HDInsight über unformatierte HTTP-Anforderungen zum Ausführen von MapReduce-Aufträgen.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, nicht jedoch mit HDInsight, finden Sie weitere Informationen im Artikel [Was Sie über Linux-basiertes Hadoop in HDInsight wissen müssen](hdinsight-hadoop-linux-information.md).


## <a id="prereq"></a>Voraussetzungen

* Hadoop in einem HDInsight-Cluster
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Ausführen von MapReduce-Aufträgen mit Curl

> [!NOTE]
> Wenn Sie Curl oder eine andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, indem Sie den Benutzernamen und das Kennwort des Administrators des HDInsight-Clusters bereitstellen. Sie müssen den Clusternamen als Teil des URI verwenden, der zum Senden der Anforderungen an den Server genutzt wird.
>
> Ersetzen Sie für die Befehle in diesem Abschnitt zur Authentifizierung im Cluster **BENUTZERNAME** durch den Benutzer und **KENNWORT** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters.
>
> Die REST-API wird durch [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Sie sollten Anforderungen immer über HTTPS stellen, um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.


1. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Sie sollten eine Antwort empfangen, die in etwa der folgenden JSON entspricht:

        {"status":"ok","version":"v1"}

    Folgende Parameter werden in diesem Befehl verwendet:

   * **-u**: Gibt den Benutzernamen und das Kennwort für die Authentifizierung der Anforderung an
   * **-G**: Gibt an, dass dieser Vorgang eine GET-Anforderung ist.

     Der Anfang des URIs, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, ist für alle Anforderungen identisch.

2. Verwenden Sie den folgenden Befehl, um einen MapReduce-Auftrag zu übermitteln.

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    Das Ende des URIs (/mapreduce/jar) weist WebHCat darauf hin, dass diese Anforderung einen MapReduce-Auftrag aus einer Klasse in einer JAR-Datei startet. Folgende Parameter werden in diesem Befehl verwendet:

   * **-d**: `-G` wird nicht verwendet, daher verwendet die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.
    * **user.name**: Der Benutzer, der den Befehl ausführt
    * **jar**: Der Speicherort der JAR-Datei, die die auszuführende Klasse enthält
    * **class**: Die Klasse, die die MapReduce-Logik enthält
    * **arg**: Die Argumente, die an den MapReduce-Auftrag übergeben werden, in diesem Fall die Eingabetextdatei und das Verzeichnis für die Ausgabe

     Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann:

       {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Ersetzen Sie **JOBID** durch den Wert, der im vorherigen Schritt zurückgegeben wurde. Wenn der Rückgabewert z. B. `{"id":"job_1415651640909_0026"}` lautet, ist die JOBID `job_1415651640909_0026`.

    Wenn der Auftrag abgeschlossen ist, wird der Status `SUCCEEDED` angezeigt.

   > [!NOTE]
   > Diese Curl-Anforderung gibt ein JSON-Dokument mit Informationen zum Auftrag zurück. Mithilfe von „jq“ wird nur der Statuswert abgerufen.

4. Sobald der Status des Auftrags in `SUCCEEDED` wechselt, können Sie die Ergebnisse des Auftrags aus Azure-BLOB-Speicher abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Beispiel ist dies `/example/curl`. Diese Adresse speichert die Ausgabe des Auftrags unter `/example/curl` im Standardspeicher des Clusters.

Sie können diese Dateien mithilfe von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) auflisten und herunterladen. Weitere Informationen zur Verwendung von BLOBs über die Azure-Befehlszeilenschnittstelle finden Sie im Artikel [Verwenden der Azure CLI 2.0 mit Azure Storage](../storage/storage-azure-cli.md#create-and-manage-blobs).

## <a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce-Aufträgen in HDInsight:

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Weitere Informationen zur REST-Schnittstelle, die in diesem Artikel verwendet wird, finden Sie in der [WebHCat-Referenz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
