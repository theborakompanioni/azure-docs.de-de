---
title: Verwenden von Azure Data Lake-Speicher mit Apache Storm in HDInsight
description: "Erfahren Sie, wie Daten aus einer Apache Storm-Topologie in HDInsight in Azure Data Lake-Speicher geschrieben werden. In diesem Dokument und dem dazugehörigen Beispiel wird veranschaulicht, wie die Komponente HdfsBolt zum Schreiben von Daten in Data Lake-Speicher verwendet werden kann."
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
ms.date: 03/03/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 514319dfcb532ab3708352b2467c095d7775b714
ms.lasthandoff: 03/25/2017


---
# <a name="use-azure-data-lake-store-with-apache-storm-with-hdinsight-java"></a>Verwenden von Azure Data Lake Store mit Apache Storm und HDInsight (Java)

Azure Data Lake-Speicher ist ein mit HDFS-kompatibler Cloudspeicherdienst, der für Ihre Daten hohe Durchsatzraten, Verfügbarkeit, Stabilität und Zuverlässigkeit bietet. In diesem Dokument erfahren Sie, wie Sie eine Java-basierte Storm-Topologie verwenden, um Daten in Azure Data Lake Store zu schreiben. In diesem Dokument kommt die Komponente [HdfsBolt](http://storm.apache.org/releases/1.0.2/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) aus dem Funktionsumfang von Apache Storm zum Einsatz.

> [!IMPORTANT]
> Die in diesem Dokument verwendete Beispieltopologie basiert auf Komponenten im Funktionsumfang von Storm in HDInsight-Clustern, die ggf. überarbeitet werden müssen, um bei Verwenden mit anderen Apache Storm-Clustern mit dem Azure Data Lake-Speicher zusammenarbeiten zu können.

## <a name="how-to-work-with-azure-data-lake-store"></a>Arbeiten mit Azure Data Lake Store

Data Lake Store ist für HDInsight ein HDFS-kompatibles Dateisystem, sodass Sie den Storm-HDFS-Bolt verwenden können, um darin zu schreiben. Beim Arbeiten mit Azure Data Lake von HDInsight können Sie ein `adl://`-Dateischema verwenden.

* Wenn der Data Lake-Speicher der primäre Speicher für den Cluster ist, verwenden Sie `adl:///`. Dies ist der Stamm des Clusterspeichers in Azure Data Lake. Dies kann in Ihrem Data Lake-Speicherkonto in den Pfad /clusters/CLUSTERNAME übersetzt werden.
* Wenn der Data Lake-Speicher der sekundäre Speicher für den Cluster ist, verwenden Sie `adl://DATALAKEACCOUNT.azuredatalakestore.net/`. Dieser URI legt das Data Lake-Speicherkonto fest, auf das Daten geschrieben werden. Daten werden ab dem Stamm von Data Lake Store geschrieben.

    > [!NOTE]
    > Sie können dieses URI-Format auch zum Speichern von Daten in dem Data Lake Store-Konto verwenden, das primären Speicher für Ihren Cluster enthält. So können Sie die Daten außerhalb des Verzeichnispfads speichern, der HDInsight enthält.

Der folgende Java-Code veranschaulicht die Verwendung des Storm-HDFS-Bolts zum Schreiben von Daten in das Verzeichnis `/stormdata` im Data Lake Store-Konto `MYDATALAKE`.

```java
// 1. Create sync and rotation policies to control when data is synched
//    (written) to the file system and when to roll over into a new file.
SyncPolicy syncPolicy = new CountSyncPolicy(1000);
FileRotationPolicy rotationPolicy = new FileSizeRotationPolicy(0.5f, Units.KB);
// 2. Set the format. In this case, comma delimited
RecordFormat recordFormat = new DelimitedRecordFormat().withFieldDelimiter(",");
// 3. Set the directory name. In this case, '/stormdata/'
FileNameFormat fileNameFormat = new DefaultFileNameFormat().withPath("/stormdata/");
// 4. Create the bolt using the previously created settings,
//    and also tell it the base URL to your Data Lake Store.
// NOTE! Replace 'MYDATALAKE' below with the name of your data lake store.
HdfsBolt adlsBolt = new HdfsBolt()
    .withFsUrl("adl://MYDATALAKE.azuredatalakestore.net/")
        .withRecordFormat(recordFormat)
        .withFileNameFormat(fileNameFormat)
        .withRotationPolicy(rotationPolicy)
        .withSyncPolicy(syncPolicy);
// 4. Give it a name and wire it up to the bolt it accepts data
//    from. NOTE: The name used here is also used as part of the
//    file name for the files written to Data Lake Store.
builder.setBolt("ADLStoreBolt", adlsBolt, 1)
    .globalGrouping("finalcount");
```

Die folgenden YAML-Code veranschaulicht die Verwendung des Storm-HDFS-Bolts aus dem Flux-Framework:

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000
  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.FileSizeRotationPolicy"
    constructorArgs:
      - 5
      - KB

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

  - id: "rotationAction"
    className: "org.apache.storm.hdfs.common.rotation.MoveFileAction"
    configMethods:
      - name: "toDestination"
        args: ["${hdfs.dest.dir}"]

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
    parallelism: 1
```

> [!NOTE]
> In den Beispielen in diesem Dokument wird das Flux-Framework verwendet.

## <a name="prerequisites"></a>Voraussetzungen

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder höher Für HDInsight 3.5 ist Java 8 erforderlich.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* Ein Storm-in-HDInsight-Cluster der Version 3.5. Erstellen Sie einen neuen Storm-in-HDInsight-Cluster, indem Sie die Schritte im Dokument [Bereitstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) ausführen.

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Bei der Installation von Java und dem JDK auf Ihrer Entwicklungsworkstation können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* **JAVA_HOME** – sollte auf das Verzeichnis verweisen, in dem die Java-Laufzeitumgebung (Java Runtime Environment, JRE) installiert ist. Für eine Unix- oder Linux-Distribution sollte z. B. ein Wert wie `/usr/lib/jvm/java-8-oracle` verwendet werden. Unter Windows sollte der Wert so ähnlich sein wie `c:\Program Files (x86)\Java\jre1.8`.
* **PATH** – sollte die folgenden Pfade enthalten:
  
  * **JAVA\_HOME** (oder den entsprechenden Pfad)
  * **JAVA\_HOME\bin** (oder den entsprechenden Pfad)
  * Das Verzeichnis, in dem Maven installiert ist.

## <a name="topology-implementation"></a>Implementierung der Topologie

Das Beispiel in diesem Dokument wurde in Java geschrieben und verwendet die folgenden Komponenten:

* **TickSpout**: Generiert die Daten, die von anderen Komponenten in der Topologie verwendet werden.
* **PartialCount**: Zählt von TickSpout generierte Ereignisse.
* **FinalCount**: Aggregiert Zählerdaten von PartialCount.
* **ADLStoreBolt**: Schreibt Daten mit der Komponente [HdfsBolt](http://storm.apache.org/releases/1.0.2/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) in Azure Data Lake Store.

Das Projekt mit dieser Topologie steht unter [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store)als Download bereit.

## <a name="build-and-package-the-topology"></a>Erstellen und Packen der Topologie

1. Laden Sie das Beispielprojekt von [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) in Ihre Entwicklungsumgebung herunter.

2. Öffnen Sie die Datei `StormToDataLake\src\main\java\com\microsoft\example\StormToDataLakeStore.java` in einem Editor, und suchen Sie die Zeile mit `.withFsUrl("adl://MYDATALAKE.azuredatalakestore.net/")`. Ändern Sie **MYDATALAKE** in den Namen des Azure Data Lake-Speichers, den Sie beim Erstellen Ihres HDInsight-Servers verwendet haben.

3. Ändern Sie über eine Eingabeaufforderung, ein Terminal oder eine Shellsitzung Verzeichnisse in das Stammverzeichnis des heruntergeladenen Projekts, und führen Sie die folgenden Befehle aus, um die Topologie zu erstellen und zu packen.
   
        mvn compile
        mvn package
   
    Nach Abschluss des Erstellungs- und Packvorgangs finden Sie ein neues Verzeichnis namens `target` vor, das die Datei `StormToDataLakeStore-1.0-SNAPSHOT.jar` enthält. Diese Datei enthält die kompilierte Topologie.

## <a name="deploy-and-run-the-topology"></a>Bereitstellen und Ausführen der Topologie

1. Führen Sie den folgenden Befehl aus, um die Topologie in den HDInsight-Cluster zu kopieren. Ersetzen Sie **USER** durch den SSH-Benutzernamen, den Sie beim Erstellen des Clusters verwendet haben. Ersetzen Sie **CLUSTERNAME** durch den Namen des Clusters.
   
        scp target\StormToDataLakeStore-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToDataLakeStore-1.0-SNAPSHOT.jar
   
    Geben Sie nach Aufforderung das Kennwort ein, das Sie beim Erstellen des SSH-Benutzers für den Cluster verwendet haben. Wenn Sie einen öffentlichen Schlüssel anstelle eines Kennworts verwendet haben, müssen Sie unter Umständen den Parameter `-i` verwenden und den Pfad zum passenden privaten Schlüssel angeben.
   
   > [!NOTE]
   > Wenn Sie einen Windows-Client für die Entwicklung verwenden, steht der Befehl `scp` ggf. nicht zur Verfügung. In diesem Fall können Sie `pscp` verwenden, das unter [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) zur Verfügung steht.

2. Führen Sie nach Abschluss des Uploads den folgenden Befehl aus, um mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster herzustellen. Ersetzen Sie **USER** durch den SSH-Benutzernamen, den Sie beim Erstellen des Clusters verwendet haben. Ersetzen Sie **CLUSTERNAME** durch den Namen des Clusters.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    Geben Sie nach Aufforderung das Kennwort ein, das Sie beim Erstellen des SSH-Benutzers für den Cluster verwendet haben. Wenn Sie einen öffentlichen Schlüssel anstelle eines Kennworts verwendet haben, müssen Sie unter Umständen den Parameter `-i` verwenden und den Pfad zum passenden privaten Schlüssel angeben.
   
   Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Verwenden Sie den folgenden Befehl, um eine Datei mit dem Namen `dev.properties` zu erstellen:

        nano dev.properties

4. Verwenden Sie als Inhalt der Datei `dev.properties` den folgenden Text:

        hdfs.write.dir: /stormdata
        hdfs.url: adl:///
    
    Verwenden Sie __STRG + X__, um die Datei zu speichern. Geben Sie dann __Y__ ein, und drücken Sie die __EINGABETASTE__. Mit den Werten in dieser Datei bestimmen Sie die Data Lake Store-URL und den Namen des Verzeichnisses, in das Daten geschrieben werden.

3. Verwenden Sie den folgenden Befehl, um die Topologie zu starten:
   
        storm jar StormToDataLakeStore-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /datalakewriter.yaml --filter dev.properties

    Dieser Befehl startet die Topologie mit dem Flux-Framework. Die Topologie wird durch die `datalakewriter.yaml`-Datei in der JAR-Datei definiert. Die `dev.properties`-Datei wird als Filter übergeben, und die Werte in der Datei werden von der Topologie gelesen.

## <a name="view-output-data"></a>Anzeigen von Ausgabedaten

Verwenden Sie zum Anzeigen der Daten den folgenden Befehl:

    hdfs dfs -ls /stormdata/

Dadurch wird eine Liste der Dateien angezeigt, die von der Topologie erstellt wurden.

Wenn Data Lake Store nicht der Standardspeicher für den Cluster ist, geben Sie den folgenden Befehl an, um die Daten anzuzeigen:

    hdfs dfs -ls adl://MYDATALAKE.azuredatalakestore.net/stormdata/

Ersetzen Sie im vorherigen Befehl __MYDATALAKE__ mit dem Data Lake Store-Konto.

Die folgende Liste ist ein Beispiel der von den vorherigen Befehlen zurückgegebenen Daten:

    Found 30 items
    -rw-r-----+  1 larryfr larryfr       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 larryfr larryfr       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 larryfr larryfr       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 larryfr larryfr       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 larryfr larryfr       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 larryfr larryfr       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 larryfr larryfr       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-the-topology"></a>Beenden der Topologie

Storm-Topologien werden ausgeführt, bis sie beendet werden oder der Cluster gelöscht wird. Gehen Sie wie folgt vor, um die Topologien zu beenden.

**Für Linux-basiertes HDInsight**:

Führen Sie in einer SSH-Sitzung mit dem Cluster den folgenden Befehl aus:

    storm kill datalakewriter

## <a name="delete-your-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Daten mithilfe von Storm in Azure Data Lake-Speicher geschrieben werden, können Sie sich mit anderen [Storm-Beispielen für HDInsight](hdinsight-storm-example-topology.md)beschäftigen.


