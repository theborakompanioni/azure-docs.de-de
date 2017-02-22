---
title: Verwenden von Hadoop Pig mit Curl in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mit Curl Pig Latin-Aufträge in einem Hadoop-Cluster in Azure HDInsight ausführen können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/09/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ecc141c9afa46f23d31de4356068ef4f98a92aa
ms.openlocfilehash: d4d9ed8380a0e8726fe2e2835e4b10fd262e1562


---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Ausführen von Pig-Aufträgen mit Hadoop in HDInsight mithilfe von Curl

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In diesem Dokument erfahren Sie, wie mithilfe von Curl Pig Latin-Aufträge auf einem HDInsight-Cluster ausgeführt werden. Mit der Programmiersprache Pig Latin können Sie Transformationen beschreiben, die auf die Eingabedaten angewendet werden, um die gewünschte Ausgabe zu generieren.

Curl wird verwendet, um zu veranschaulichen, wie Sie über unformatierte HTTP-Anforderungen zum Ausführen, Überwachen und Abrufen der Ergebnisse der Pig-Aufträge mit HDInsight interagieren können. Dies funktioniert mithilfe der WebHCat REST-API (ehemals Templeton), die von Ihrem HDInsight-Cluster bereitgestellt wird.

> [!NOTE]
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, Ihnen HDInsight jedoch neu ist, finden Sie weitere Informationen unter [Tipps zu Linux-basiertem HDInsight](hdinsight-hadoop-linux-information.md).

## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Azure HDInsight-Cluster (Hadoop in HDInsight), der auf Linux oder Windows basiert

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a name="a-idcurlarun-pig-jobs-by-using-curl"></a><a id="curl"></a>Ausführen von Pig-Aufträgen mit Curl

> [!NOTE]
> Wenn Sie Curl oder eine andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, indem Sie den Benutzernamen und das Kennwort des Administrators des HDInsight-Clusters bereitstellen. Sie müssen auch den Clusternamen als Teil des URI (Uniform Resource Identifier) verwenden, der zum Senden der Anforderungen an den Server genutzt wird.
> 
> Ersetzen Sie für die Befehle in diesem Abschnitt die Option **BENUTZERNAME** zur Authentifizierung im Cluster durch den Benutzer und die Option **KENNWORT** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters.
> 
> Die REST-API wird durch [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Sie sollten Anforderungen immer über HTTPS (Secure HTTP) stellen, um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    Sie sollten eine Antwort empfangen, die in etwa der im Folgenden aufgeführten entspricht:
   
        {"status":"ok","version":"v1"}
   
    Folgende Parameter werden in diesem Befehl verwendet:
   
    * **-u**– Der Benutzername und das Kennwort für die Authentifizierung der Anforderung
    * **-G**: Gibt an, dass dies eine GET-Anforderung ist
     
     Der Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, ist für alle Anforderungen identisch. Der Pfad **/status** gibt an, dass die Anforderung den Status von WebHCat (auch bekannt als Templeton) für den Server zurückgibt.

2. Verwenden Sie den folgenden Code, um einen Pig Latin-Auftrag an den Cluster zu übermitteln.
   
        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
   
    Folgende Parameter werden in diesem Befehl verwendet:
   
    * **-d**: Da `-G` nicht verwendet wird, verwendet die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.
    
    * **user.name**: Der Benutzer, der den Befehl ausführt
    * **execute**: Die auszuführenden Pig Latin-Anweisungen
    * **statusdir**– Das Verzeichnis, in das die Statusangaben für diesen Auftrag geschrieben werden.
    
    > [!NOTE]
    > Beachten Sie, dass die Leerzeichen in Pig Latin-Anweisungen bei Curl durch das Zeichen `+` ersetzt werden.
    
    Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann. Beispiel:
     
        {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen. Ersetzen Sie **JOBID** durch den Wert, der im vorherigen Schritt zurückgegeben wurde. Wenn der Rückgabewert z. B. `{"id":"job_1415651640909_0026"}` lautet, ist die **JOBID** `job_1415651640909_0026`.
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    Wenn der Auftrag abgeschlossen ist, wird der Status **ERFOLGREICH**angezeigt.
   
    > [!NOTE]
    > Diese Curl-Anforderung gibt ein JSON-Dokument (JavaScript Object Notation) mit Informationen zum Auftrag zurück, und mithilfe von "jq" wird nur der Statuswert abgerufen.

## <a name="a-idresultsaview-results"></a><a id="results"></a>Anzeigen der Ergebnisse

Sobald der Status des Auftrags in **SUCCEEDED** wechselt, können Sie die Ergebnisse des Auftrags aus dem vom Cluster verwendeten Standardspeicher abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Fall ist dies **/example/pigcurl**. 

Der Sicherungsspeicher von HDInsight kann Azure Storage oder Azure Data Lake Store sein. Die Daten aus diesen Speichern können, je nach verwendetem Speicher, auf verschiedene Weisen abgerufen werden. Weitere Informationen zur Verwendung von Azure Storage und Azure Data Lake Store finden Sie im Dokument „HDInsight unter Linux“ im Abschnitt [HDFS, Blob-Speicher und Data Lake Store](hdinsight-hadoop-linux-information.md##hdfs-blob-storage-and-data-lake-store).

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie in diesem Dokument veranschaulicht, können Sie unformatierte HTTP-Anforderungen dazu verwenden, um Pig-Aufträge auf dem HDInsight-Cluster auszuführen, zu überwachen und deren Ergebnisse anzuzeigen.

Weitere Informationen zur REST-Schnittstelle, die in diesem Artikel verwendet wird, finden Sie in der [WebHCat-Referenz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Pig in HDInsight:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)




<!--HONumber=Feb17_HO2-->


