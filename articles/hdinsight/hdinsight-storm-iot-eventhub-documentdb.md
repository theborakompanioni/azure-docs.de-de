---
title: Verarbeiten von Fahrzeugsensordaten mit Apache Storm auf HDInsight | Microsoft Docs
description: "Erfahren Sie, wie Sie Fahrzeugsensordaten von Event Hubs mit Apache Storm auf HDInsight verarbeiten. Fügen Sie Modelldaten aus Azure Cosmos DB hinzu, und speichern Sie die Ausgabe im Speicher."
services: hdinsight,documentdb,notification-hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 78980635-8bef-4c33-96c3-fae50e932e31
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8e8ebc724e1c70e8fcd56312adef5da2342373ea
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Verarbeiten von Fahrzeugsensordaten von Azure Event Hubs mit Apache Storm auf HDInsight

Erfahren Sie, wie Sie Fahrzeugsensordaten von Azure Event Hubs mit Apache Storm auf HDInsight verarbeiten. In diesem Beispiel werden Sensordaten von Azure Event Hubs gelesen, und dann werden die Daten durch Verweisen auf in Azure Cosmos DB gespeicherte Daten erweitert. Die Daten werden mithilfe des Hadoop-Dateisystems (HDFS) in Azure Storage gespeichert.

![Architekturdiagramm für HDInsight und das Internet der Dinge (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

## <a name="overview"></a>Übersicht

Durch das Hinzufügen von Sensoren zu Fahrzeugen können Sie bestimmte Probleme basierend auf Vergangenheitsdatentrends vorhersagen. Außerdem können Sie dadurch Verbesserungen an zukünftigen Versionen basierend auf Nutzungsverhaltenanalysen vornehmen. Sie müssen in der Lage sein, die Daten von allen Fahrzeugen schnell und effizient in Hadoop zu laden, bevor die MapReduce-Verarbeitung beginnen kann. Darüber wollen Sie möglicherweise Analysen für kritischen Fehlerpfade (Motortemperatur, Bremsen usw.) in Echtzeit durchführen.

Azure Event Hubs sind für die Verarbeitung großer Datenmengen konzipiert, die von Sensoren generiert werden. Apache Storm kann verwendet werden, um die Daten vor dem Speichern in HDFS zu laden und zu verarbeiten.

## <a name="solution"></a>Lösung

Telemetriedaten für die Motortemperatur, Umgebungstemperatur und Geschwindigkeit des Fahrzeugs werden von Sensoren aufgezeichnet. Anschließend werden die Daten zusammen mit der Fahrgestellnummer (FIN) des Fahrzeugs und einem Zeitstempel an Event Hubs gesendet. Von dort liest eine Storm-Topologie, die auf einem Apache-Storm auf HDInsight-Cluster ausgeführt wird, die Daten, verarbeitet diese und speichert sie in HDFS.

Während der Verarbeitung wird die FIN verwendet, um Modellinformationen aus Cosmos DB abzurufen. Diese Daten werden dem Datenstrom hinzugefügt, bevor er gespeichert wird.

Die in der Storm-Topologie verwendet Komponenten sind:

* **EventHubSpout** - Liest Daten aus den Azure-Event Hubs
* **TypeConversionBolt** – Konvertiert die JSON-Zeichenfolge von Event Hubs in ein Tupel, das die folgenden Sensordaten enthält:
    * Engine temperature (Motortemperatur)
    * Umgebungstemperatur
    * Speed (Geschwindigkeit)
    * VIN (Fahrzeug-Identifizierungsnummer)
    * Zeitstempel
* **DataReferencBolt** – Sucht das Fahrzeugmodell über die FIN in Cosmos DB
* **WasbStoreBolt** - Speichert die Daten in HDFS (Azure-Speicher)

In der folgenden Abbildung ist ein Diagramm dieser Lösung dargestellt:

![Storm-Topologie](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

## <a name="implementation"></a>Implementierung

Eine vollständige, automatisierte Lösung für dieses Szenario ist als Teil des Repositorys [HDInsight-Storm-Examples](https://github.com/hdinsight/hdinsight-storm-examples) auf GitHub verfügbar. Um dieses Beispiel zu verwenden, führen Sie die Schritte in [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md)aus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiel-Storm-Topologien finden Sie unter [Beispieltopologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).


