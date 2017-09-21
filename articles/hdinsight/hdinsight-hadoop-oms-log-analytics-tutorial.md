---
title: "Verwenden von Log Analytics zum Überwachen von Azure HDInsight-Clustern | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Log Analytics zum Überwachen von Aufträgen verwenden, die in einem HDInsight-Cluster ausgeführt werden."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: af5b05a366c1abbe7c91d186358dba2b4a957f92
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Verwenden von Azure Log Analytics zum Überwachen der HDInsight-Cluster (Vorschau)

In diesem Artikel erfahren Sie, wie Sie mit Azure Log Analytics HDInsight Hadoop-Clustervorgänge überwachen.

Log Analytics ist ein Dienst in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), der Ihre cloudbasierten und lokalen Umgebungen überwacht, um die Verfügbarkeit und Leistung sicherzustellen. Er sammelt Daten, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen sowie von anderen Überwachungstools generiert werden, um Analysen für mehrere Quellen zu ermöglichen. 

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Für dieses Tutorial wird ein Azure-Abonnement benötigt. Weitere Informationen finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free).

* **Ein Azure HDInsight-Cluster**. Derzeit können Sie Azure OMS mit den folgenden HDInsight-Cluster-Typen verwenden:
    * Hadoop
    * Spark
    * HBase
    * Storm
    * Kafka
    * Interactive Hive

    Anweisungen zum Erstellen von HDInsight-Clustern finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).


* **Ein Log Analytics-Arbeitsbereich**. Sie können sich diesen Arbeitsbereich als einzigartige Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen vorstellen. Sie müssen bereits einen solchen Arbeitsbereich erstellt haben, den Sie Azure HDInsight-Clustern zuordnen können. Anweisungen hierzu finden Sie unter [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-get-started.md#2-create-a-workspace).

## <a name="configure-hdinsight-cluster-to-use-azure-log-analytics"></a>Konfigurieren eines HDInsight-Clusters zur Verwendung von Azure Log Analytics

In diesem Abschnitt konfigurieren Sie einen vorhandenen HDInsight Hadoop-Cluster zur Verwendung eines Azure Log Analytics-Arbeitsbereichs zum Überwachen von Aufträgen, Debugprotokollen usw.

1. Klicken Sie im Azure-Portal im linken Bereich auf **HDInsight-Cluster** und dann auf den Namen des Clusters, den Sie mit Azure Log Analytics konfigurieren möchten.

2. Klicken Sie auf dem Blatt „Cluster“ im linken Bereich auf **Überwachung**.

3. Klicken Sie im rechten Bereich auf **Aktivieren**, und wählen Sie dann einen vorhandenen Log Analytics-Arbeitsbereich. Klicken Sie auf **Speichern**.

    ![Aktivieren der Überwachung für HDInsight-Cluster](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Aktivieren der Überwachung für HDInsight-Cluster")

4. Sobald der Cluster zur Verwendung von Log Analytics für die Überwachung konfiguriert ist, sehen Sie am oberen Rand der Registerkarte die Option **OMS-Dashboard öffnen**. Klicken Sie auf die Schaltfläche.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "OMS-Dashboard öffnen")

5. Geben Sie nach Aufforderung Ihre Azure-Anmeldeinformationen ein. Sie werden zum Microsoft OMS-Dashboard weitergeleitet.

    ![Operations Management Suite-Portal](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "Operations Management Suite-Portal")

## <a name="next-steps"></a>Nächste Schritte
* [Add HDInsight cluster management solutions to Log Analytics (Preview)](hdinsight-hadoop-oms-log-analytics-management-solutions.md) (Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics [Vorschau])

