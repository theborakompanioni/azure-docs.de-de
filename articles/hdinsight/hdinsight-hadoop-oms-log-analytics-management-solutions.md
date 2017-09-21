---
title: "Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit Azure Log Analytics benutzerdefinierte Ansichten für HDInsight-Cluster erstellen."
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
ms.openlocfilehash: 21b474e37ef0a6037e05ee1fe8e5088cb3e3601d
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics-preview"></a>Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics (Vorschau)

HDInsight bietet clusterspezifische Verwaltungslösungen, die Sie Azure Log Analytics hinzufügen können. [Verwaltungslösungen](../log-analytics/log-analytics-add-solutions.md) erweitern den Funktionsumfang von OMS und stellen zusätzliche Daten und Analysetools für Log Analytics bereit. Diese Lösungen sammeln wichtige Leistungsmetriken aus Ihrem HDInsight-Cluster und stellen die Tools bereit, um die Metriken zu durchsuchen. Außerdem bieten diese Lösungen Visualisierungen und Dashboards für die meisten in HDInsight unterstützten Clustertypen. Anhand der mit der Lösung erfassten Kennzahlen können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen. 

In diesem Artikel erfahren Sie, wie Sie einem OMS-Arbeitsbereich clusterspezifische Verwaltungslösungen hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen einen HDInsight-Cluster zur Verwendung von Azure Log Analytics konfiguriert haben. Anweisungen hierzu finden Sie unter [Use Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-tutorial.md) (Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern [Vorschau]).

## <a name="add-cluster-specific-management-solutions"></a>Hinzufügen von clusterspezifischen Verwaltungslösungen

In diesem Abschnitt fügen Sie eine HBase-Clusterverwaltungslösung einem vorhandenen OMS-Arbeitsbereich hinzu. Ähnliche Lösungen für andere HDInsight-Clustertypen werden bald verfügbar sein.

1. Öffnen Sie das OMS-Dashboard. Öffnen Sie im Azure-Portal das HDInsight-Clusterblatt, das Sie mit Azure Log Analytics zugeordnet haben, klicken Sie auf die Registerkarte „Überwachung“ und dann auf **OMS-Dashboard öffnen**.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

1. Klicken Sie im OMS-Dashboard auf **Lösungskatalog** oder das Ansicht-Designer-Symbol im linken Bereich.

    ![Hinzufügen einer Verwaltungslösung in OMS](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "Hinzufügen einer Verwaltungslösung in OMS")

2. Suchen Sie im Lösungskatalog **HDInsight HBase-Überwachung**, und klicken Sie dann auf die Kachel.

    ![Suchen der HBase-Verwaltungslösung](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/find-hbase-management-solution.png "Suchen der HBase-Verwaltungslösung")

3. Klicken Sie im nächsten Bildschirm auf **Hinzufügen**.

     ![Hinzufügen der HBase-Verwaltungslösung](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "Hinzufügen der HBase-Verwaltungslösung")

4. Sie sollten jetzt eine Kachel für die HBase-Verwaltungslösung im OMS-Dashboard sehen. Wenn der Cluster, den Sie mit OMS (als Teil der erforderlichen Voraussetzung für diesen Artikel) zugeordnet haben, ein HBase-Cluster ist, zeigt die Kachel den Namen des Clusters und die Anzahl der Knoten im Cluster an.

    ![Hinzugefügte HBase-Verwaltungslösung](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "Hinzugefügte HBase-Verwaltungslösung")

## <a name="next-steps"></a>Nächste Schritte

* [Query Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-use-queries.md) (Abfragen von Azure Log Analytics zum Überwachen der HDInsight-Cluster [Vorschau])

## <a name="see-also"></a>Weitere Informationen

* [OMS Log Analytics: Create Tiles, Drill-ins and Dashboards with the View Designer](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/) (OMS Log Analytics: Erstellen von Kacheln, Drill-ins und Dashboards mit dem Ansicht-Designer)
* [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)

