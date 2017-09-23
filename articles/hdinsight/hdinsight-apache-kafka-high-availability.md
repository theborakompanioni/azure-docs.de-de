---
title: "Hochverfügbarkeit mit Apache Kafka – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie mit Apache Kafka in Azure HDInsight hohe Verfügbarkeit sichergestellt wird. Erfahren Sie, wie Partitionsreplikate in Kafka ausgeglichen werden, damit sie sich in verschiedenen Fehlerdomänen innerhalb der Azure-Region befinden, die HDInsight enthält."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/20/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: f8164d1c3483b28e5f2abcc8035da78880daec1e
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a>Hohe Verfügbarkeit Ihrer Daten mit Apache Kafka (Vorschauversion) in HDInsight

Erfahren Sie, wie Partitionsreplikate für Kafka-Themen für die Nutzung der zugrunde liegenden Konfiguration des Hardwareracks konfiguriert werden. Diese Konfiguration stellt die Verfügbarkeit von Daten sicher, die in Apache Kafka in HDInsight gespeichert sind.

## <a name="fault-and-update-domains-with-kafka"></a>Fehler- und Updatedomänen bei Kafka

Eine Fehlerdomäne ist eine logische Gruppierung von zugrundeliegender Hardware in einem Azure-Rechenzentrum. Jede Fehlerdomäne verwendet eine Stromquelle und einen Netzwerkswitch gemeinsam. Die virtuellen Computer und verwalteten Datenträger, die die Knoten innerhalb eines HDInsight-Clusters implementieren, werden auf diese Fehlerdomänen verteilt. Diese Architektur schränkt die potenziellen Auswirkungen physischer Hardwarefehler ein.

Jede Azure-Region weist eine bestimmte Anzahl von Fehlerdomänen auf. Eine Liste der Domänen und die Anzahl der Fehlerdomänen, die sie enthalten, finden Sie in der Dokumentation zu [Verfügbarkeitsgruppen](../virtual-machines/linux/regions-and-availability.md#availability-sets).

> [!IMPORTANT]
> Fehlerdomänen sind Kafka nicht bekannt. Beim Erstellen eines Themas in Kafka werden u.U. alle Partitionsreplikate in der gleichen Fehlerdomäne gespeichert. Zur Lösung dieses Problems stellen wir das [Tool zum Ausgleichen von Kafka-Partitionen](https://github.com/hdinsight/hdinsight-kafka-tools) bereit.

## <a name="when-to-rebalance-partition-replicas"></a>Wann sollten Partitionsreplikate ausgeglichen werden?

Um die höchste Verfügbarkeit Ihrer Kafka-Daten sicherzustellen, sollten Sie die Partitionsreplikate für Ihr Thema zu folgenden Zeitpunkten ausgleichen:

* Wenn ein neues Thema oder eine neue Partition erstellt wird

* Wenn Sie einen Cluster zentral hochskalieren

## <a name="replication-factor"></a>Replikationsfaktor

> [!IMPORTANT]
> Es wird empfohlen, eine Azure-Region mit drei Fehlerdomänen und den Replikationsfaktor 3 zu verwenden.

Wenn Sie eine Region verwenden müssen, die nur zwei Fehlerdomänen enthält, verwenden Sie den Replikationsfaktor 4, um die Replikate gleichmäßig auf die zwei Fehlerdomänen zu verteilen.

Ein Beispiel zum Erstellen von Themen und zum Festlegen des Replikationsfaktors finden Sie im Dokument zu den [ersten Schritten mit Kafka in HDInsight](hdinsight-apache-kafka-get-started.md).

## <a name="how-to-rebalance-partition-replicas"></a>Ausgleichen von Partitionsreplikaten

Verwenden Sie das [Tool zum Ausgleichen von Kafka-Partitionen](https://github.com/hdinsight/hdinsight-kafka-tools), um ausgewählte Themen auszugleichen. Dieses Tool muss über eine SSH-Sitzung für den Hauptknoten des Kafka-Clusters ausgeführt werden.

Weitere Informationen zum Herstellen einer Verbindung mit HDInsight mithilfe von SSH finden Sie im Dokument [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="next-steps"></a>Nächste Schritte

* [Skalierbarkeit von Kafka in HDInsight](hdinsight-apache-kafka-scalability.md)
* [Spiegelung mit Kafka in HDInsight](hdinsight-apache-kafka-mirroring.md)
