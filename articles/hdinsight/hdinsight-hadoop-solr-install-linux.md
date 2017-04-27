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
ms.date: 04/14/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 741e67d1419eb54e0ca1913dd0d4fb5e56895bbd
ms.lasthandoff: 04/17/2017


---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von Solr in HDInsight Hadoop-Clustern

Erfahren Sie, wie Sie Solr in Azure HDInsight mithilfe der Funktion „Skriptaktion“ installieren. Solr ist eine leistungsfähige Suchplattform und bietet Suchfunktionen der Unternehmensklasse für von Hadoop verwaltete Daten.

> [!IMPORTANT]
    > Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

> [!IMPORTANT]
> Das in diesem Dokument verwendete Beispielskript erstellt einen Solr-Cluster mit einer bestimmten Konfiguration. Wenn Sie den Solr-Cluster mit anderen Auflistungen, Shards, Schemas, Replikaten usw. konfigurieren möchten, müssen Sie das Skript und die Solr-Binärdateien ändern.

## <a name="whatis"></a>Was ist Solr?

[Apache Solr](http://lucene.apache.org/solr/features.html) ist eine Unternehmensplattform für die leistungsstarke Volltextsuche in Daten. Während Hadoop das Speichern und Verwalten von großen Datenmengen ermöglicht, bietet Apache Solr die Suchfunktionen, um schnell Daten abzurufen.

> [!WARNING]
> Mit dem HDInsight-Cluster bereitgestellte Komponenten werden von Microsoft vollständig unterstützt.
>
> Für benutzerdefinierte Komponenten wie Solr steht in wirtschaftlich angemessenem Rahmen Support für eine weiterführende Behebung des Problems zur Verfügung. Microsoft-Support kann Probleme mit benutzerdefinierten Komponenten möglicherweise nicht beheben. Gegebenenfalls müssen Sie die Open Source-Communitys um Unterstützung bitten. So können z. B. viele Communitywebsites verwendet werden, wie: das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [http://apache.org](http://apache.org), [z. B. Hadoop](http://hadoop.apache.org/).

## <a name="what-the-script-does"></a>Funktion des Skripts

Mit diesem Skript werden die folgenden Änderungen am HDInsight-Cluster vorgenommen:

* Installiert Solr auf `/usr/hdp/current/solr`
* Erstellt einen Benutzer (**Solrusr**), der zum Ausführen des Solr-Diensts verwendet wird.
* Legt **Solruser** als Besitzer von `/usr/hdp/current/solr` fest.
* Fügt eine [Upstart](http://upstart.ubuntu.com/)-Konfiguration hinzu, die Solr automatisch startet.

## <a name="install"></a>Installation von Solr mithilfe von Skriptaktionen

Ein Beispielskript zum Installieren von Solr in einem HDInsight-Cluster ist an folgendem Ort verfügbar:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

Zum Erstellen eines Clusters, in dem Solr installiert ist, führen Sie die im Artikel [Erstellen von HDInsight-Clustern](hdinsight-hadoop-create-linux-clusters-portal.md) beschriebenen Schritte aus. Während des Erstellungsvorgangs installieren Sie Solr mithilfe der folgenden Schritte:

1. Wählen Sie auf dem Blatt __Clusterübersicht__ die Option „Erweiterte Einstellungen“ und dann __Skriptaktionen__ aus. Verwenden Sie die folgenden Informationen zum Füllen des Formulars:

   * **NAME**: Geben Sie einen Anzeigenamen für die Skriptaktion ein.
   * **SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Aktivieren Sie diese Option.
   * **WORKER**: Aktivieren Sie diese Option.
   * **ZOOKEEPER**: Aktivieren Sie diese Option für die Installation auf dem Zookeeper-Knoten.
   * **PARAMETERS**: Lassen Sie dieses Feld leer.

2. Verwenden Sie am unteren Rand des Blatts **Skriptaktionen** die Schaltfläche **Auswählen**, um die Konfiguration zu speichern. Kehren Sie abschließend mit der Schaltfläche **Weiter** zur __Clusterübersicht__ zurück.

3. Wählen Sie auf der Seite __Clusterübersicht__ die Option __Erstellen__ aus, um den Cluster zu erstellen.

## <a name="usesolr"></a>Wie verwende ich Solr in HDInsight?

> [!IMPORTANT]
> Die Schritte in diesem Abschnitt verdeutlichen grundlegende Solr-Funktionen. Weitere Informationen zur Verwendung von Solr finden Sie auf der [Apache Solr-Website](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Indizieren der Daten

Verwenden Sie die folgenden Schritte, um Beispieldaten zu Solr hinzuzufügen und diese dann abzufragen:

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

     > [!IMPORTANT]
     > Die späteren Schritte in diesem Artikel verwenden einen SSL-Tunnel, um mit der Solr-Webbenutzeroberfläche eine Verbindung herzustellen. Um diese Schritte ausführen zu können, müssen Sie einen SSL-Tunnel einrichten, und Ihren Browser dann zu dessen Verwendung einrichten.
     >
     > Weitere Informationen finden Sie im Artikel [Verwenden von SSH-Tunneling mit HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

2. Verwenden Sie die folgenden Befehle, mit denen Solr Beispieldaten indiziert:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    Die folgende Ausgabe wird an die Konsole zurückgegeben:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Das Dienstprogramm `post.jar` fügt die Dokumente **solr.xml** und **monitor.xml** zum Index hinzu.
  
3. Verwenden Sie den folgenden Befehl, um die Solr-REST-API abzufragen:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Mit diesem Befehl wird **collection1** nach allen Dokumenten durchsucht, die **\*:\*** (in der Abfragezeichenfolge als \*%3A\* kodiert) entsprechen. Das folgende JSON-Dokument ist ein Beispiel für die Antwort:

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

Das Solr-Dashboard ist eine Webbenutzeroberfläche, mit dem Sie über Ihren Webbrowser mit Solr arbeiten können. Das Solr-Dashboard wir nicht direkt im Internet aus Ihrem HDInsight-Cluster verfügbar gemacht. Der Zugriff kann mithilfe eines SSH-Tunnels stattfinden. Weitere Informationen zum Verwenden eines SSH-Tunnels finden Sie im Artikel [Verwenden von SSH-Tunneling mit HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

Wenn Sie einen SSH-Tunnel eingerichtet haben, gehen Sie folgendermaßen vor, um das Solr-Dashboard zu verwenden:

1. Bestimmen Sie den Hostnamen für den primären Hauptknoten:

   1. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem Clusterhauptknoten. Beispiel: `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       Weitere Informationen zur Verwendung von SSH finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Verwenden Sie den folgenden Befehl, um den vollqualifizierten Hostnamen abzurufen:

        ```bash
        hostname -f
        ```

        Dieser Befehl gibt einen Wert zurück, der in etwa wie der folgende Hostname aussieht:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Speichern Sie den zurückgegebenen Wert, da er später benötigt wird.

2. Stellen Sie in Ihrem Browser eine Verbindung mit **http://HOSTNAME:8983/solr/#/** her, wobei **HOSTNAME** der Name ist, den Sie in den vorherigen Schritten bestimmt haben.

    Die Anforderung wird durch den SSH-Tunnel an die Solr-Webbenutzeroberfläche auf dem Cluster weitergeleitet. Die Seite entspricht in etwa der folgenden Abbildung:

    ![Bild des Solr-Dashboards](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. Wählen Sie im linken Bereich in der Dropdownliste **Core Selector** (Core-Auswahl) den Eintrag **collection1** aus. Unterhalb von **collection1** sollten dann mehrere Einträge angezeigt werden.

4. Wählen Sie aus den Einträgen unter **collection1** den Eintrag **Abfrage** aus. Verwenden Sie die folgenden Werte zum Auffüllen der Suchseite:

   * Geben Sie im Textfeld **q** Folgendes ein: **\*:**\*. Diese Abfrage gibt alle Dokumente zurück, die in Solr indiziert sind. Wenn Sie nach einer bestimmten Zeichenfolge innerhalb der Dokumente suchen möchten, können diese Zeichenfolge hier eingeben.
   * Wählen Sie im Textfeld **wt** das Ausgabeformat aus. Der Standardwert ist **json**.

     Klicken Sie abschließend am unteren Rand der Suchseite auf die Schaltfläche **Abfrage ausführen** .

     ![Anpassen eines Clusters mit "Skriptaktion"](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     Die Ausgabe gibt die beiden Dokumente zurück, die Sie zuvor dem Index hinzugefügt haben. Die Ausgabe sieht in etwa wie das folgende JSON-Dokument aus:

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

Verwenden Sie die folgenden Befehle, um Solr manuell zu beenden und zu starten:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Sichern indizierter Daten

Verwenden Sie die folgenden Schritte, um Solr-Daten im Standardspeicher für Ihren Cluster zu sichern:

1. Stellen Sie über SSH eine Verbindung mit dem Cluster her, und verwenden Sie dann den folgenden Befehl, um den Hostnamen für den Hauptknoten zu erhalten.

    ```bash
    hostname -f
    ```

2. Verwenden Sie den folgenden Befehl, um eine Momentaufnahme der indizierten Daten zu erstellen. Ersetzen Sie **HOSTNAME** durch den aus dem vorherigen Befehl zurückgegebenen Namen:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    Die Antwort ähnelt dem folgenden XML-Code:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Wechseln Sie in das Verzeichnis `/usr/hdp/current/solr/example/solr`. Hier finden Sie Unterverzeichnisse für jede Sammlung. Jedes Sammlungsverzeichnis enthält ein `data`-Verzeichnis, in dem sich die Momentaufnahme für diese Sammlung befindet.

4. Erstellen Sie mit dem folgenden Befehl ein komprimiertes Archiv des Momentaufnahmenordners:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Ersetzen Sie die `snapshot.20150806185338855`-Werte durch den Namen der Momentaufnahme für Ihre Sammlung.

    Dieser Befehl erstellt ein Archiv mit dem Namen **snapshot.20150806185338855.tgz**, das den Inhalt des Verzeichnisses **snapshot.20150806185338855** enthält.

5. Sie können das Archiv dann im primären Speicher des Clusters mit dem folgenden Befehl speichern:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Weitere Informationen zum Arbeiten mit Solr-Backups und -Wiederherstellungen finden Sie unter [Making and restoring backups of SolrCores](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups+of+SolrCores)(auf Englisch).

## <a name="next-steps"></a>Nächste Schritte

* [Installieren von Giraph in HDInsight-Clustern](hdinsight-hadoop-giraph-install-linux.md). Verwenden Sie die Clusteranpassung, um Giraph in HDInsight Hadoop-Clustern zu installieren. Giraph ermöglicht Ihnen, mithilfe von Hadoop Graphen zu verarbeiten, und kann mit Azure HDInsight verwendet werden.

* [Installieren Sie Hue in HDInsight-Clustern.](hdinsight-hadoop-hue-linux.md) Verwenden Sie die Clusteranpassung, um Hue in HDInsight Hadoop-Clustern zu installieren. Bei Hue handelt es sich um einen Satz von Webanwendungen zur Interaktion mit einem Hadoop-Cluster.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

