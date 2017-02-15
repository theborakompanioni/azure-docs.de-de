---
title: "Einführung in Apache Kafka in HDInsight | Microsoft Docs"
description: "Enthält Informationen zu Apache Kafka in HDInsight. Es wird beschrieben, worum es sich handelt, welche Funktion erfüllt wird und wo Sie Beispiele und Informationen zu den ersten Schritten finden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/09/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2d744e753224e4ce98680d3228914fd89e87eba4
ms.openlocfilehash: 28d213fa23d480635fd4376e22ff54a5e6374350

---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>Einführung in Apache Kafka in HDInsight (Vorschau)

[Apache Kafka](https://kafka.apache.org) ist eine verteilte Open Source-Streamingplattform, die zum Erstellen von Datenpipelines und Anwendungen mit Echtzeitstreaming verwendet werden kann. Kafka verfügt auch über Nachrichtenbrokerfunktionen, die einer Nachrichtenwarteschlange ähneln, über die Sie benannte Datenströme veröffentlichen und diese abonnieren können. Bei Kafka in HDInsight erhalten Sie einen verwalteten, hoch skalierbaren und hoch verfügbaren Dienst in der Microsoft Azure-Cloud.

## <a name="why-use-kafka-on-hdinsight"></a>Gründe für Kafka in HDInsight

Kafka verfügt über die folgenden Features:

* Veröffentlichen-Abonnieren-Messagingmuster: Kafka umfasst eine Producer-API zum Veröffentlichen von Datensätzen in einem Kafka-Thema. Die Consumer-API wird verwendet, wenn Sie ein Thema abonnieren.

* Datenstromverarbeitung: Kafka wird häufig zusammen mit Apache Storm oder Spark für die Echtzeit-Datenstromverarbeitung eingesetzt. Mit Kafka 0.10.0.0 (HDInsight-Version 3.5) wurde eine Streaming-API eingeführt, mit der Sie Streaminglösungen erstellen können, ohne dass Sie dafür Storm oder Spark benötigen.

* Horizontale Skalierung: Bei Kafka werden Datenströme über die Knoten im HDInsight-Cluster hinweg partitioniert. Consumerprozesse können einzelnen Partitionen zugeordnet werden, um beim Nutzen von Datensätzen für einen Lastenausgleich zu sorgen.

* Geordnete Bereitstellung: In jeder Partition werden die Datensätze im Datenstrom in der Reihenfolge gespeichert, in der sie empfangen wurden. Indem ein Consumerprozess pro Partition zugeordnet wird, können Sie sicherstellen, dass die Datensätze in der richtigen Reihenfolge verarbeitet werden.

* Fehlertoleranz: Partitionen können zwischen Knoten repliziert werden, um für Fehlertoleranz zu sorgen.

## <a name="use-cases"></a>Anwendungsfälle

* **Messaging**: Da das Veröffentlichen-Abonnieren-Messagingmuster unterstützt wird, wird Kafka häufig als Nachrichtenbroker genutzt.

* **Aktivitätsüberwachung**: Da Kafka die geordnete Protokollierung von Datensätzen unterstützt, kann die Anwendung zum Nachverfolgen und Neuerstellen von Aktivitäten verwendet werden. Beispiele hierfür sind Benutzeraktionen auf einer Website oder in einer Anwendung.

* **Aggregation**: Mit der Datenstromverarbeitung können Sie Informationen aus unterschiedlichen Datenströmen aggregieren, um die Informationen zu operativen Daten zu kombinieren und zu zentralisieren.

* **Transformation**: Mit der Datenstromverarbeitung können Sie Daten aus mehreren Eingabethemen zu einem oder mehreren Ausgabethemen kombinieren und erweitern.

## <a name="where-do-i-start"></a>Wo beginne ich?

Schritte zum Erstellen eines Kafka-Clusters und Verwenden von Kafka, darunter Java-basierte Beispiele zur Verwendung der Producer-, Consumer- und Streaming-API, finden Sie unter [Erste Schritte mit Apache Kafka (Vorschau) in HDInsight](hdinsight-apache-kafka-get-started.md).

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die folgenden Links, um Informationen zur Verwendung von Apache Kafka unter HDInsight zu erhalten:

* [Erste Schritte mit Apache Kafka (Vorschau) in HDInsight](hdinsight-apache-kafka-get-started.md)

* [Verwenden von MirrorMaker zum Erstellen eines Replikats von Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)

* [Verwenden von Apache Storm mit Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)

* [Verwenden von Apache Spark mit Kafka in HDInsight](hdinsight-apache-spark-with-kafka.md)




<!--HONumber=Nov16_HO3-->


