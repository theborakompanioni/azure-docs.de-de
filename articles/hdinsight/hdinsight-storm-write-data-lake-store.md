---
title: "Apache Storm-Schreibzugriff auf Storage/Data Lake Store – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit Apache Storm in den HDFS-kompatiblen Speicher für HDInsight schreiben. Azure Storage bzw. Azure Data Lake Store bietet den HDFS-kompatiblen Speicher für HDInsight. In diesem Dokument und dem dazugehörigen Beispiel wird veranschaulicht, wie die Komponente HdfsBolt zum Schreiben in den standardmäßigen Speicher von Storm auf einem HDInsight-Cluster verwendet werden kann."
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/19/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 10dc8789e8f4a2b27fd3a4c6fec2ab28c674170a
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="write-to-hdfs-from-apache-storm-on-hdinsight"></a>Schreiben in HDFS aus Apache Storm auf HDInsight

Erfahren Sie, wie Sie mit Storm Daten in den von Apache Storm auf HDInsight verwendeten HDFS-kompatiblen Speicher schreiben. HDInsight kann sowohl Azure Storage als auch Azure Data Lake Store als HDFS-kompatiblen Speicher verwenden. Storm bietet eine [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html)-Komponente, die Daten in HDFS schreibt. Dieses Dokument enthält Informationen zum Schreiben in jeden Speichertyp von HdfsBolt aus. 

> [!IMPORTANT]
> Die in diesem Dokument verwendete Beispieltopologie basiert auf Komponenten, die in Storm auf HDInsight enthalten sind. Bei Verwendung mit anderen Apache Storm-Clustern können Änderungen zum Arbeiten mit Azure Data Lake Store erforderlich sein.

## <a name="get-the-code"></a>Abrufen des Codes

Das Projekt mit dieser Topologie steht unter [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store)als Download bereit.

Um dieses Projekt zu kompilieren, benötigen Sie die folgende Konfiguration für Ihre Entwicklungsumgebung:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder höher HDInsight 3.5 oder höher erfordert Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

Bei der Installation von Java und dem JDK auf Ihrer Entwicklungsworkstation können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* `JAVA_HOME` – sollte auf das Verzeichnis verweisen, in dem das JDK installiert ist.
* `PATH` – sollte die folgenden Pfade enthalten:
  
    * `JAVA_HOME` (oder entsprechender Pfad).
    * `JAVA_HOME\bin` (oder entsprechender Pfad).
    * Das Verzeichnis, in dem Maven installiert ist.

## <a name="how-to-use-the-hdfsbolt-with-hdinsight"></a>Verwenden von HdfsBolt mit HDInsight

> [!IMPORTANT]
> Bevor Sie HdfsBolt mit Storm in HDInsight verwenden, müssen Sie mit einer Skriptaktion erforderliche JAR-Dateien in den `extpath` für Storm kopieren. Weitere Informationen finden Sie unter [Konfigurieren des Clusters](#configure).

HdfsBolt verwendet das von Ihnen bereitgestellte Dateischema, um zu verstehen, wie in HDFS geschrieben wird. Verwenden Sie mit HDInsight eines der folgenden Schemas:

* `wasb://`: Wird mit einem Azure Storage-Konto verwendet.
* `adl://`: Wird mit Azure Data Lake Store verwendet.

Die folgende Tabelle enthält Beispiele für die Verwendung des Dateischemas für verschiedene Szenarien:

| Schema | Hinweise |
| ----- | ----- |
| `wasb:///` | Das Standardspeicherkonto ist ein Blobcontainer in einem Azure Storage-Konto. |
| `adl:///` | Das Standardspeicherkonto ist ein Verzeichnis in Azure Data Lake Store. Während der Clustererstellung geben Sie das Verzeichnis in Data Lake Store an, das der Stamm des Clusters von HDFS ist. Beispiel: das `/clusters/myclustername/`-Verzeichnis. |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | Ein nicht standardmäßiges (zusätzliches) Azure Storage-Konto, das dem Cluster zugeordnet ist. |
| `adl://STORENAME/` | Der Stamm des vom Cluster verwendeten Data Lake Store. Mit diesem Schema können Sie auf Daten zugreifen, die sich außerhalb des das Clusterdateisystem enthaltenden Verzeichnisses befinden. |

Weitere Informationen finden Sie unter Apache.org in dem Verweis auf [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html).

### <a name="example-configuration"></a>Beispielkonfiguration

Der folgende YAML-Code ist ein Auszug aus der im Beispiel enthaltenen `resources/writetohdfs.yaml`-Datei. Diese Datei definiert die Storm-Topologie mit dem [Flux](https://storm.apache.org/releases/1.1.0/flux.html)-Framework für Apache Storm.

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.NoRotationPolicy"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

Dieser YAML-Code definiert die folgenden Elemente:

* `syncPolicy`: Definiert, wann Dateien mit dem Dateisystem synchronisiert/darin geleert werden. In diesem Beispiel alle 1.000 Tupel.
* `fileNameFormat`: Definiert das beim Schreiben von Dateien zu verwendende Pfad- und Dateinamensmuster. In diesem Beispiel wird der Pfad zur Laufzeit mithilfe eines Filters bereitgestellt, und die Erweiterung der Datei lautet `.txt`.
* `recordFormat`: Definiert das interne Format der geschriebenen Dateien. In diesem Beispiel werden Felder durch das `|`-Zeichen begrenzt.
* `rotationPolicy`: Definiert das Rotieren der Dateien. In diesem Beispiel erfolgt keine Rotation.
* `hdfs-bolt`: Verwendet die vorherigen Komponenten als Konfigurationsparameter für die `HdfsBolt`-Klasse.

Weitere Informationen zum Flux-Framework finden Sie unter [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="configure-the-cluster"></a>Konfigurieren des Clusters

Standardmäßig schließt Storm in HDInsight nicht die Komponenten ein, die HdfsBolt zur Kommunikation mit Azure Storage oder Data Lake Store im Klassenpfad von Storm verwendet. Fügen Sie diese Komponenten mit folgender Skriptaktion dem `extlib`-Verzeichnis für Storm auf dem Cluster hinzu:

| Skript-URI | Darauf anzuwendende Knoten | Parameter || `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, Supervisor | Keine |

Informationen zum Verwenden dieses Skripts mit Ihrem Cluster finden Sie im Dokument [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](./hdinsight-hadoop-customize-cluster-linux.md).

## <a name="build-and-package-the-topology"></a>Erstellen und Packen der Topologie

1. Laden Sie das Beispielprojekt von [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) in Ihre Entwicklungsumgebung herunter.

2. Ändern Sie über eine Eingabeaufforderung, ein Terminal oder eine Shellsitzung Verzeichnisse in das Stammverzeichnis des heruntergeladenen Projekts. Erstellen und packen Sie die Topologie mit folgendem Befehl:
   
        mvn compile package
   
    Nach Abschluss des Erstellungs- und Packvorgangs finden Sie ein neues Verzeichnis namens `target` vor, das die Datei `StormToHdfs-1.0-SNAPSHOT.jar` enthält. Diese Datei enthält die kompilierte Topologie.

## <a name="deploy-and-run-the-topology"></a>Bereitstellen und Ausführen der Topologie

1. Führen Sie den folgenden Befehl aus, um die Topologie in den HDInsight-Cluster zu kopieren. Ersetzen Sie **USER** durch den SSH-Benutzernamen, den Sie beim Erstellen des Clusters verwendet haben. Ersetzen Sie **CLUSTERNAME** durch den Namen des Clusters.
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    Geben Sie nach Aufforderung das Kennwort ein, das Sie beim Erstellen des SSH-Benutzers für den Cluster verwendet haben. Wenn Sie einen öffentlichen Schlüssel anstelle eines Kennworts verwendet haben, müssen Sie unter Umständen den Parameter `-i` verwenden und den Pfad zum passenden privaten Schlüssel angeben.
   
   > [!NOTE]
   > Weitere Informationen zum Verwenden von `scp` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).

2. Führen Sie nach Abschluss des Uploads den folgenden Befehl aus, um mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster herzustellen. Ersetzen Sie **USER** durch den SSH-Benutzernamen, den Sie beim Erstellen des Clusters verwendet haben. Ersetzen Sie **CLUSTERNAME** durch den Namen des Clusters.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    Geben Sie nach Aufforderung das Kennwort ein, das Sie beim Erstellen des SSH-Benutzers für den Cluster verwendet haben. Wenn Sie einen öffentlichen Schlüssel anstelle eines Kennworts verwendet haben, müssen Sie unter Umständen den Parameter `-i` verwenden und den Pfad zum passenden privaten Schlüssel angeben.
   
   Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Verwenden Sie den folgenden Befehl, um eine Datei mit dem Namen `dev.properties` zu erstellen:

        nano dev.properties

4. Verwenden Sie als Inhalt der Datei `dev.properties` den folgenden Text:

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > Dieses Beispiel setzt voraus, dass Ihr Cluster ein Azure Storage-Konto als Standardspeicher verwendet. Wenn Ihr Cluster Azure Data Lake Store verwendet, verwenden Sie stattdessen `hdfs.url: adl:///`.
    
    Verwenden Sie __STRG + X__, um die Datei zu speichern. Geben Sie dann __Y__ ein, und drücken Sie die __EINGABETASTE__. Mit den Werten in dieser Datei bestimmen Sie die Data Lake Store-URL und den Namen des Verzeichnisses, in das Daten geschrieben werden.

3. Verwenden Sie den folgenden Befehl, um die Topologie zu starten:
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    Dieser Befehl startet die Topologie mithilfe des Flux-Frameworks, indem sie an den Nimbus-Knoten des Clusters gesendet wird. Die Topologie wird durch die `writetohdfs.yaml`-Datei in der JAR-Datei definiert. Die `dev.properties`-Datei wird als Filter übergeben, und die Werte in der Datei werden von der Topologie gelesen.

## <a name="view-output-data"></a>Anzeigen von Ausgabedaten

Verwenden Sie zum Anzeigen der Daten den folgenden Befehl:

    hdfs dfs -ls /stormdata/

Eine Liste der von dieser Topologie erstellten Dateien wird angezeigt.

Die folgende Liste ist ein Beispiel der von den vorherigen Befehlen zurückgegebenen Daten:

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-the-topology"></a>Beenden der Topologie

Storm-Topologien werden ausgeführt, bis sie beendet werden oder der Cluster gelöscht wird. Verwenden Sie den folgenden Befehl, um die Topologie zu beenden:

    storm kill hdfswriter

## <a name="delete-your-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Daten mithilfe von Storm in Azure Storage und Azure Data Lake Store geschrieben werden, können Sie sich mit anderen [Storm-Beispielen für HDInsight](hdinsight-storm-example-topology.md) beschäftigen.


