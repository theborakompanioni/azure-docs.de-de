---
title: "Verwenden von DataFu mit Pig in HDInsight – Azure | Microsoft-Dokumentation"
description: "Bei DataFu handelt es sich um eine Sammlung von Bibliotheken für die Verwendung mit Hadoop. Erfahren Sie, wie Sie DataFu mit Pig in Ihrem HDInsight-Cluster verwenden können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 0016721a-82be-4773-88ad-91e6b2c21cbb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 272159d5017e62d1f606e69d6bdcde8ecbc0e3bf
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="use-datafu-with-pig-on-hdinsight"></a>Verwenden von DataFu mit Pig für HDInsight

Erfahren Sie, wie Sie DataFu mit HDInsight verwenden. Bei DataFu handelt es sich um eine Sammlung von Open Source-Bibliotheken für die Verwendung mit Pig in Hadoop.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement.

* Ein Azure HDInsight-Cluster (Linux- oder Windows-basiert)

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Grundlegende Kenntnisse der [Verwendung von Pig für HDInsight](hdinsight-use-pig.md)

## <a name="install-datafu-on-linux-based-hdinsight"></a>Installieren von DataFu für Linux-basiertes HDInsight

> [!NOTE]
> DataFu wird in Linux-basierten Clustern ab Version 3.3 und in Windows-basierten Clustern installiert. In Linux-basierten Clustern mit einer Version vor 3.3 wird DataFu nicht installiert.
>
> Wenn Sie einen Linux-basierten Cluster ab Version 3.3 oder einen Windows-basierten Cluster verwenden, können Sie diesen Abschnitt überspringen.

DataFu kann aus dem Maven-Repository heruntergeladen und installiert werden. Führen Sie die folgenden Schritte aus, um DataFu dem HDInsight-Cluster hinzuzufügen:

1. Stellen Sie per SSH eine Verbindung mit Ihrem Linux-basierten HDInsight-Cluster her. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Verwenden Sie den folgenden Befehl, um die DataFu-JAR-Datei mit dem Hilfsprogramm „wget“ herunterzuladen, oder fügen Sie den Link in Ihren Browser ein, um den Download zu starten.

    ```
    wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar
    ```

3. Laden Sie als Nächstes die Datei in den Standardspeicher für Ihren HDInsight-Cluster hoch. Wenn Sie die Datei im Standardspeicher ablegen, steht sie allen Clusterknoten zur Verfügung.

    ```
    hdfs dfs -put datafu-1.2.0.jar /example/jars
    ```

    > [!NOTE]
    > Im vorherigen Befehl wird die JAR-Datei unter `/example/jars` gespeichert, da dieses Verzeichnis im Clusterspeicher bereits vorhanden ist. Sie können im HDInsight-Clusterspeicher einen beliebigen Speicherort verwenden.

## <a name="use-datafu-with-pig"></a>Verwenden von DataFu mit Pig

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass Sie mit Pig in HDInsight vertraut sind. Weitere Informationen zum Verwenden von Pig für HDInsight finden Sie unter [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md).

> [!IMPORTANT]
> Bei Verwendung von DataFu über Pig in einem Linux-basierten HDInsight-Cluster müssen Sie zuerst die JAR-Datei registrieren.
>
> Wenn Ihr Cluster Azure Storage verwendet, verwenden Sie einen `wasb://`-Pfad. Beispiel: `register wasb:///example/jars/datafu-1.2.0.jar`.
>
> Wenn Ihr Cluster Azure Data Lake Store verwendet, verwenden Sie einen `adl://`-Pfad. Beispiel: `register adl://home/example/jars/datafu-1.2.0.jar`.
>
> DataFu wird standardmäßig auf Windows-basierten HDInsight-Clustern registriert.

Gewöhnlich definieren Sie für DataFu-Funktionen einen Alias. Beispiel:

    DEFINE SHA datafu.pig.hash.SHA();

Mit dieser Anweisung wird ein Alias mit dem Namen `SHA` für die SHA-Hashfunktion definiert. Diesen Alias können Sie dann in einem Pig Latin-Skript verwenden, um einen Hashwert für die Eingabedaten zu generieren. Im folgenden Code werden beispielsweise die Namen in den Eingabedaten durch einen Hashwert ersetzt:

```piglatin
raw = LOAD '/data/raw/' USING PigStorage(',') AS  
    (name:chararray,
    int1:int,
    int2:int,
    int3:int);
mask = FOREACH raw GENERATE SHA(name), int1, int2, int3;
DUMP mask;
```

Bei Verwendung dieses Codes mit den folgenden Eingabedaten:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5

Dadurch wird folgende Ausgabe generiert:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu DataFu oder Pig finden Sie in den folgenden Dokumenten:

* [Leitfaden zu Apache DataFu Pig](http://datafu.incubator.apache.org/docs/datafu/guide.html).
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)

