---
title: Verwenden von Skriptaktionen zum Installieren von Solr in Linux-basiertem HDInsight | Microsoft Docs
description: Erfahren Sie, wie Sie Solr auf Linux-basierten HDInsight Hadoop-Clustern mit Skriptaktionen installieren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 8458470ecdd787834d76767c9ed2c967c1c9b188
ms.lasthandoff: 04/12/2017


---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von Solr in HDInsight Hadoop-Clustern
In diesem Thema wird beschrieben, wie Sie Solr in Azure HDInsight mithilfe der Funktion "Skriptaktion" installieren. Solr ist eine leistungsfähige Suchplattform und bietet Suchfunktionen der Unternehmensklasse für von Hadoop verwaltete Daten. Sobald Sie Solr in einem HDInsight-Cluster installiert haben, erfahren Sie auch, wie Sie Daten mithilfe von Solr suchen können.

> [!IMPORTANT]
    > Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

Das in diesem Thema verwendete Beispielskript erstellt einen Solr-Cluster mit einer bestimmten Konfiguration. Wenn Sie den Solr-Cluster mit anderen Auflistungen, Shards, Schemas, Replikaten usw. konfigurieren möchten, müssen Sie das Skript und die Solr-Binärdateien entsprechend ändern.

## <a name="whatis"></a>Was ist Solr?
[Apache Solr](http://lucene.apache.org/solr/features.html) ist eine Unternehmensplattform für die leistungsstarke Volltextsuche in Daten. Während Hadoop das Speichern und Verwalten von großen Datenmengen ermöglicht, bietet Apache Solr die Suchfunktionen, um schnell Daten abzurufen. In diesem Thema wird beschrieben, wie Sie ein HDInsight-Cluster zum Installieren von Solr anpassen können.

> [!WARNING]
> Komponenten, die mit dem HDInsight-Cluster bereitgestellt werden, werden vollständig unterstützt, und Microsoft Support hilft Ihnen, Probleme im Zusammenhang mit diesen Komponenten zu isolieren und zu beheben.
>
> Für benutzerdefinierte Komponenten wie Solr steht in wirtschaftlich angemessenem Rahmen Support für eine weiterführende Behebung des Problems zur Verfügung. Auf diese Weise kann das Problem behoben werden, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z. B. viele Communitywebsites verwendet werden, wie: das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [http://apache.org](http://apache.org), [z. B. Hadoop](http://hadoop.apache.org/).
>
>

## <a name="what-the-script-does"></a>Funktion des Skripts
Mit diesem Skript werden die folgenden Änderungen am HDInsight-Cluster vorgenommen:

* Installiert Solr auf `/usr/hdp/current/solr`
* Erstellt einen neuen Benutzer (**Solrusr**), der zum Ausführen des Solr-Dienstes verwendet wird.
* Legt **Solruser** als Besitzer von `/usr/hdp/current/solr` fest.
* Fügt eine [Upstart](http://upstart.ubuntu.com/) -Konfiguration hinzu, die Solr startet, wenn ein Clusterknoten neu gestartet wird. Solr wird außerdem nach der Installation automatisch auf den Clusterknoten gestartet.

## <a name="install"></a>Installation von Solr mithilfe von Skriptaktionen
Ein Beispielskript zum Installieren von Solr in einem HDInsight-Cluster ist an folgendem Ort verfügbar:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

Dieser Abschnitt enthält Anweisungen zur Verwendung des Beispielskripts während der Erstellung eines neuen Clusters mithilfe des Azure-Portals.

> [!NOTE]
> Zum Anwenden von Skriptaktionen können auch Azure PowerShell, die Azure-Befehlszeilenschnittstelle, das HDInsight .NET SDK oder Azure Resource Manager-Vorlagen verwendet werden. Sie können auch Skriptaktionen auf bereits ausgeführte Cluster anwenden. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Beginnen Sie die Bereitstellung eines Clusters anhand der Schritte in [Bereitstellen Linux-basierter HDInsight-Cluster](hdinsight-hadoop-create-linux-clusters-portal.md), schließen Sie sie jedoch nicht ab.
2. Wählen Sie auf dem Blatt **Optionale Konfiguration** die Option **Skriptaktionen**, und geben Sie die folgenden Informationen an:

   * **NAME**: Geben Sie einen Anzeigenamen für die Skriptaktion ein.
   * **SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Aktivieren Sie diese Option.
   * **WORKER**: Aktivieren Sie diese Option.
   * **ZOOKEEPER**: Aktivieren Sie diese Option für die Installation auf dem Zookeeper-Knoten.
   * **PARAMETERS**: Lassen Sie dieses Feld leer.
3. Verwenden Sie am unteren Rand der **Skriptaktionen** die Schaltfläche **Auswählen**, um die Konfiguration zu speichern. Verwenden Sie schließlich die Schaltfläche **Auswählen** am unteren Rand des Blatts **Optionale Konfiguration**, um die optionalen Konfigurationsinformationen zu speichern.
4. Setzen Sie die Bereitstellung des Clusters wie unter [Bereitstellen Linux-basierter HDInsight-Cluster](hdinsight-hadoop-create-linux-clusters-portal.md)beschrieben fort.

## <a name="usesolr"></a>Wie verwende ich Solr in HDInsight?
### <a name="indexing-data"></a>Indizieren von Daten
Sie müssen mit der Indizierung von Solr mit einigen Datendateien beginnen. Sie können dann Solr verwenden, um Suchabfragen der indizierten Daten auszuführen. Verwenden Sie die folgenden Schritte, um einige Beispieldaten zu Solr hinzuzufügen und diese dann abzufragen:

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

     > [!IMPORTANT]
     > Die späteren Schritte in diesem Dokument verwenden einen SSL-Tunnel, um mit der Solr-Webbenutzeroberfläche eine Verbindung herzustellen. Um diese Schritte ausführen zu können, müssen Sie einen SSL-Tunnel einrichten, und Ihren Browser dann zu dessen Verwendung einrichten.
     >
     > Weitere Informationen finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md)
     >
     >
2. Verwenden Sie die folgenden Befehle, mit denen Solr Beispieldaten indiziert:

        cd /usr/hdp/current/solr/example/exampledocs
        java -jar post.jar solr.xml monitor.xml

    In der Konsole wird die folgende Ausgabe angezeigt:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Das Hilfsprogramm „post.jar“ indiziert Solr mit zwei Beispieldokumenten: **solr.xml** und **monitor.xml**. Diese werden in **collection1** innerhalb von Solr gespeichert.
3. Verwenden Sie folgenden Befehl, um die von Solr verfügbar gemachte REST-API abzufragen:

        curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"

    Dadurch wird eine Abfrage für **collection1** für alle Dokumente ausgestellt, die **\*:\*** (codiert als \*%3A\* in der Abfragezeichenfolge) entsprechen. Die Antwort sollte im JSON-Format zurückgegeben werden. Die Antwort sollte in etwa wie folgt aussehen:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }

### <a name="using-the-solr-dashboard"></a>Verwenden des Solr-Dashboards
Das Solr-Dashboard ist eine Webbenutzeroberfläche, mit dem Sie über Ihren Webbrowser mit Solr arbeiten können. Das Solr-Dashboard wir nicht direkt im Internet aus Ihrem HDInsight-Cluster verfügbar gemacht. Der Zugriff muss mithilfe eines SSH-Tunnels stattfinden. Weitere Informationen zum Verwenden eines SSH-Tunnels finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md).

Wenn Sie einen SSH-Tunnel eingerichtet haben, gehen Sie folgendermaßen vor, um das Solr-Dashboard zu verwenden:

1. Bestimmen Sie den Hostnamen für den primären Hauptknoten:

   1. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem Cluster an Port 22. Beispiel: `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`. Dabei steht **USERNAME** für den SSH-Benutzernamen und **CLUSTERNAME** für den Namen Ihres Clusters.

       Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Verwenden Sie den folgenden Befehl, um den vollqualifizierten Hostnamen abzurufen:

           hostname -f

       Die Ausgabe dieses Befehls sieht etwa so aus:

           hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

       Dies ist der Hostname, der in den folgenden Schritten verwendet werden muss.
2. Stellen Sie in Ihrem Browser eine Verbindung mit **http://HOSTNAME:8983/solr/#/** her, wobei **HOSTNAME** der Name ist, den Sie in den vorherigen Schritten bestimmt haben.

    Die Anfrage sollte über den SSH-Tunnel auf den Hauptknoten für Ihren HDInsight-Cluster weitergeleitet werden. Eine Seite ähnlich der folgenden wird angezeigt:

    ![Bild des Solr-Dashboards](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)
3. Wählen Sie im linken Bereich in der Dropdownliste **Core Selector** (Core-Auswahl) den Eintrag **collection1** aus. Unterhalb von **collection1** sollten dann mehrere Einträge angezeigt werden.
4. Wählen Sie aus den Einträgen unter **collection1** den Eintrag **Abfrage** aus. Verwenden Sie die folgenden Werte zum Auffüllen der Suchseite:

   * Geben Sie im Textfeld **q** Folgendes ein: **\*:**\*. Dadurch werden alle Dokumente zurückgegeben, die in Solr indiziert sind. Wenn Sie nach einer bestimmten Zeichenfolge innerhalb der Dokumente suchen möchten, können diese Zeichenfolge hier eingeben.
   * Wählen Sie im Textfeld **wt** das Ausgabeformat aus. Der Standardwert ist **json**.

     Klicken Sie abschließend am unteren Rand der Suchseite auf die Schaltfläche **Abfrage ausführen** .

     ![Anpassen eines Clusters mit "Skriptaktion"](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     Die Ausgabe gibt die beiden Dokumente zurück, die wir zur Indizierung von Solr verwendet haben. Die Ausgabe sieht ungefähr so aus:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }

### <a name="starting-and-stopping-solr"></a>Starten und Anhalten von Solr
Wenn Sie Solr manuell beenden oder starten müssen, verwenden Sie die folgenden Befehle:

    sudo stop solr

    sudo start solr

## <a name="backup-indexed-data"></a>Sichern indizierter Daten
Bewährt hat sich auch das Sichern der indizierten Daten auf den Solr-Clusterknoten in Azure-Blobspeicher. Führen Sie dazu die folgenden Schritte aus:

1. Stellen Sie über SSH eine Verbindung mit dem Cluster her, und verwenden Sie dann den folgenden Befehl, um den Hostnamen für den Hauptknoten zu erhalten.

        hostname -f
2. Erstellen Sie folgendermaßen eine Momentaufnahme der indizierten Daten. Ersetzen Sie **HOSTNAME** durch den aus dem vorherigen Befehl zurückgegebenen Namen:

        curl http://HOSTNAME:8983/solr/replication?command=backup

    Sie sollte eine Rückgabe wie diese erhalten:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>
3. Als Nächstes wechseln Sie zum Verzeichnis **/usr/hdp/current/solr/example/solr**. Dort ist ein Unterverzeichnis für jede Sammlung vorhanden. Jedes Sammlungsverzeichnis enthält ein **Datenverzeichnis**, in dem sich die Momentaufnahme für diese Sammlung befindet.

    Wenn Sie beispielsweise die Schritte oben zum Indizieren der Beispieldokumente verwendet haben, sollte das Verzeichnis **/usr/hdp/current/solr/example/solr/collection1/data** jetzt ein Verzeichnis namens **snapshot.###########** enthalten. Die #-Zeichen stellen das Datum und die Uhrzeit der Momentaufnahme dar.
4. Erstellen Sie ein komprimiertes Archiv des Momentaufnahmenordnders, indem Sie einen Befehl ähnlich dem folgenden verwenden:

        tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855

    Dadurch entsteht ein neues Archiv mit dem Namen **snapshot.20150806185338855.tgz**, das den Inhalt des Verzeichnisses **snapshot.20150806185338855** enthält.
5. Sie können das Archiv dann im primären Speicher des Clusters mit dem folgenden Befehl speichern:

    hadoop fs -copyFromLocal snapshot.20150806185338855.tgz /example/data

   > [!NOTE]
   > Möglicherweise möchten Sie ein dediziertes Verzeichnis für Solr-Momentaufnahmen erstellen. Beispiel: `hadoop fs -mkdir /solrbackup`.
   >
   >

Weitere Informationen zum Arbeiten mit Solr-Backups und -Wiederherstellungen finden Sie unter [Making and restoring backups of SolrCores](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups+of+SolrCores)(auf Englisch).

## <a name="see-also"></a>Siehe auch
* [Installieren und Verwenden von Hue in HDInsight-Clustern](hdinsight-hadoop-hue-linux.md). Hue ist eine Webbenutzeroberfläche, die das Erstellen, Ausführen und Speichern von Pig- und Hive-Aufträgen sowie das Durchsuchen des Standardspeichers für Ihre HDInsight-Cluster vereinfacht.
* [Installieren und Verwenden von R in HDInsight-Clustern][hdinsight-install-r]. Verwenden Sie die Clusteranpassung, um R in HDInsight Hadoop-Clustern zu installieren. R ist eine Open-Source-Sprache und -Umgebung für statistische Berechnungen. Sie bietet Hunderte integrierter Statistikfunktionen und eine eigene Programmiersprache, die Aspekte der funktionalen und objektorientierten Programmierung kombiniert. Darüber hinaus werden umfangreiche Grafikfunktionen geboten.
* [Installieren von Giraph in HDInsight-Clustern](hdinsight-hadoop-giraph-install-linux.md). Verwenden Sie die Clusteranpassung, um Giraph in HDInsight Hadoop-Clustern zu installieren. Giraph ermöglicht Ihnen, mithilfe von Hadoop Graphen zu verarbeiten, und kann mit Azure HDInsight verwendet werden.
* [Installieren Sie Hue in HDInsight-Clustern.](hdinsight-hadoop-hue-linux.md) Verwenden Sie die Clusteranpassung, um Hue in HDInsight Hadoop-Clustern zu installieren. Bei Hue handelt es sich um einen Satz von Webanwendungen zur Interaktion mit einem Hadoop-Cluster.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

