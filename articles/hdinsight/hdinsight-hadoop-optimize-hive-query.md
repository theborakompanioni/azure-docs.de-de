---
title: Optimieren von Hive-Abfragen in Azure HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ihre Hive-Abfragen für Hadoop in HDInsight optimieren."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2016
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 7d269a5805da405e4e5f7a3caf5a58fa454b9abb
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>Optimieren von Hive-Abfragen in Azure HDInsight

Hadoop-Cluster sind standardmäßig nicht leistungsoptimiert. In diesem Artikel werden einige der gängigsten Methoden für die Optimierung der Leistung von Hive vorgestellt, die Sie auf Ihre Abfragen anwenden können.

## <a name="scale-out-worker-nodes"></a>Horizontales Skalieren der Worker-Knoten

In einem Cluster, dem mehr Worker-Knoten zur Verfügung stehen, können mehr Mapper und Reducer parallel ausgeführt werden. In HDInsight können Sie die horizontale Skalierung auf zwei Weisen erhöhen:

* Bei der Bereitstellung können Sie die Anzahl der Workerknoten im Azure-Portal, in Azure PowerShell oder an der plattformübergreifenden Befehlszeilenschnittstelle angeben.  Weitere Informationen finden Sie unter [Erstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md). Der folgende Screenshot zeigt die Konfiguration der Workerknoten im Azure-Portal:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* Auch zur Laufzeit kann ein Cluster horizontal hochskaliert werden, ohne ihn neu zu erstellen:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Weitere Informationen zu den von HDInsight unterstützten virtuellen Computern finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Aktivieren von Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) ist eine Alternative zum Ausführungsmodul MapReduce:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez ist jedoch aus folgenden Gründen schneller:

* **Ausführen eines gerichteten azyklischen Graphen als einzelnen Auftrag im MapReduce-Modul**. Der gerichtete azyklische Graph erfordert, dass auf jede Gruppe von Mappern eine Gruppe von Reducern folgt. Dadurch müssen für jede Hive-Abfrage mehrere MapReduce-Aufträge gestartet werden. Für Tez gilt diese Einschränkung nicht. Es kann auch ein komplexes DAG in einem Auftrag verarbeiten, so dass weniger Aufträge gestartet werden müssen.
* **Vermeiden unnötiger Schreibvorgänge**. Aufgrund der Eröffnung mehrerer Aufträge für eine Hive-Abfrage im MapReduce-Modul wird die Ausgabe jedes Auftrags in den HDFS-Zwischenspeicher geschrieben. Tez hingegen minimiert die Anzahl der Aufträge für jede Hive-Abfrage und vermeidet so unnötige Schreibvorgänge.
* **Minimierung von Startverzögerungen**. Tez minimiert Startverzögerungen durch Reduzierung der Anzahl der für den Start erforderlichen Mapper sowie durch eine insgesamt bessere Optimierung.
* **Wiederverwendung von Containern**. Tez versucht, Container möglichst wiederzuverwenden, und verringert so Latenzzeiten aufgrund von Containerstarts.
* **Techniken für die fortlaufende Optimierung**. Bislang erfolgte die Optimierung in der Regel in der Kompilierungsphase. Nun aber stehen mehr Informationen zu den Eingaben zur Verfügung, die auch während der Laufzeit eine Optimierung ermöglichen. Tez verwendet fortlaufende Optimierungstechniken, durch die der Plan noch weit in der Laufzeitphase optimiert werden kann.

Weitere Informationen zu diesen Konzepten finden unter [Apache Tez](http://hortonworks.com/hadoop/tez/).

Sie können jede von Tez ermöglichte Hive-Abfrage ausführen, indem Sie der Abfrage die folgende Einstellung voranstellen:

    set hive.execution.engine=tez;

Linux-basierte HDInsight-Cluster haben Tez standardmäßig aktiviert.


## <a name="hive-partitioning"></a>Partitionierung in Hive

E/A-Vorgänge sind bei der Ausführung von Hive-Abfragen der größte Leistungsengpass. Die Leistung lässt sich durch Reduzierung der einzulesenden Daten verbessern. Hive-Abfragen durchsuchen standardmäßig gesamte Hive-Tabellen. Dies eignet sich hervorragend für Abfragen wie Tabellenscans. Doch bei Abfragen, die nur einen Auszug der Daten durchsuchen müssen (z. B. bei Abfragen mit Filtern), bedeutet dies hingegen einen unnötigen Mehraufwand. Durch die Partitionierung in Hive beschränkt sich der Zugriff von Hive-Abfragen auf die erforderlichen Teilmengen von Hive-Tabellen.

Die Partitionierung in Hive erfolgt durch Neuorganisation der Rohdaten in neuen Verzeichnissen, wobei jede Partition ein eigenes, vom Benutzer definiertes Verzeichnis erhält. Das folgende Diagramm veranschaulicht die Partitionierung einer Hive-Tabelle nach der Spalte *Jahr*. Für jedes Jahr wird ein neues Verzeichnis erstellt.

![Partitionierung][image-hdi-optimize-hive-partitioning_1]

Überlegungen zur Partitionierung:

* **Partitionieren Sie großzügig**: Wenn Sie die Partitionierung für Spalten mit nur wenigen Werte durchführen, erhalten Sie nur wenige Partitionen. Partitionieren Sie zum Beispiel nach dem Geschlecht, so erhalten Sie nur zwei Partitionen (männlich, weiblich), sodass sich die Latenzzeit also höchstens halbiert.
* **Aber nicht zu großzügig**: Im anderen Extremfall werden bei Erstellung einer Partition anhand einer Spalte mit einem eindeutigen Wert (z.B. userid) mehrere Partitionen erzeugt. Eine zu großzügige Partitionierung verursacht eine große Belastung für den NameNode des Clusters, da dieser mit einer großen Anzahl von Verzeichnissen zurechtkommen muss.
* **Vermeiden Sie zu unterschiedliche Partitionsgrößen** – Wählen Sie Ihren Partitionsschlüssel so, dass sich etwa gleich große Partitionen ergeben. Partitionieren Sie zum Beispiel nach *Bundesland*, so wird die Partition für Nordrhein-Westfalen aufgrund der Einwohnerzahl etwa 17 mal so viele Einträge enthalten wie diejenige des Saarlands.

Zum Erstellen einer Partitionstabelle verwenden Sie die Klausel *Partitioned By*:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Für die erstellte Partitionstabelle können Sie eine statische oder eine dynamische Partitionierung auswählen.

* **Statische Partitionierung** bedeutet, dass die entsprechenden Verzeichnisse bereits freigegebene Daten enthalten und Sie Hive-Partitionen manuell auf Basis des Verzeichnispfads abfragen können. Der folgende Codeausschnitt ist ein Beispiel.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dynamische Partitionierung** bedeutet, dass Hive die Partitionen automatisch erstellen und anpassen soll. Da wir die Partitionstabelle bereits anhand der Stagingtabelle erstellt haben, müssen wir die Tabelle nun nur noch mit Daten füllen:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Weitere Informationen finden Sie unter [Partitionierte Tabellen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-the-orcfile-format"></a>Verwenden des Dateiformats ORC
Hive unterstützt verschiedene Dateiformate. Beispiel:

* **Text**: Dies ist das Standarddateiformat. Es funktioniert in den meisten Szenarien.
* **Avro**: Dieses Dateiformat eignet sich besonders für Interoperabilitätsszenarien.
* **ORC/Parquet**: Dieses Dateiformat ist leistungsorientiert.

Das ORC-Format (Optimized Row Columnar) ist eine sehr effiziente Speichermethode für Hive-Daten. Gegenüber anderen Formaten hat ORC die folgenden Vorteile:

* Unterstützung für komplexe Typen, einschließlich DateTime, und komplexe und teilstrukturierte Typen
* Komprimierung bis zu 70 %
* Indizierung aller 10.000 Zeilen, wodurch das Überspringen von Zeilen möglich wird
* Wesentlich schnellere Laufzeitausführung

Zum Aktivieren von ORC erstellen Sie zunächst eine Tabelle mit der Klausel *Stored as ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Danach fügen Sie der ORC-Tabelle Daten aus der Stagingtabelle hinzu. Zum Beispiel:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
            L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Weitere Informationen über das ORC-Format erhalten Sie [hier](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vektorisierung

Durch Vektorisierung kann Hive anstatt einzelnen Zeilen Batches mit jeweils 1024 Zeilen gleichzeitig verarbeiten. Einfache Vorgänge erfolgen so schneller, da weniger interner Code ausgeführt werden muss.

Zum Aktivieren der Vektorisierung stellen Sie Ihrer Hive-Abfrage folgende Einstellung voran:

    set hive.vectorized.execution.enabled = true;

Weitere Informationen finden Sie unter [Vektorisierte Abfrageausführung](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Weitere Optimierungsmethoden
Es gibt noch weitere Optimierungsmethoden, die durchaus erwägenswert sind, zum Beispiel die folgenden:

* **Hive-Bucketing**: Ein Verfahren, mit dem große Datenmengen zur Optimierung der Abfrageleistung zusammengefasst bzw. segmentiert werden.
* **Join-Optimierung**: Optimierung des Hive-Abfrageausführungsplans zur Steigerung der Effizienz von Joins. Außerdem sollen dadurch Benutzerhinweise weitgehend unnötig werden. Weitere Informationen finden Sie unter [Join-Optimierung](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Reducer erhöhen**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere allgemeine Hive-Methoden zur Optimierung von Abfragen kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwenden von Apache Hive in HDInsight](hdinsight-use-hive.md)
* [Analysieren von Flugverspätungsdaten mit Hive in HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Analysieren von Twitter-Daten mit Hive in HDInsight](hdinsight-analyze-twitter-data.md)
* [Analysieren von Sensordaten mit der Hive-Abfragekonsole für Hadoop in HDInsight](hdinsight-hive-analyze-sensor-data.md)
* [Verwenden von Hive in HDInsight zum Analysieren von Website-Protokollen](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png

