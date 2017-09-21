---
title: "Abfragen von Azure Log Analytics zum Überwachen von Azure HDInsight-Clustern | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie in Azure Log Analytics Abfragen zum Überwachen von Aufträgen ausführen, die in einem HDInsight-Cluster ausgeführt werden."
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
ms.date: 09/11/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 8fe91bed69a1c06367346041d8caba4aaee4c82a
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Abfragen von Azure Log Analytics zum Überwachen von HDInsight-Clustern (Vorschau)

In diesem Artikel betrachten Sie einige Szenarien zur Verwendung von Azure Log Analytics mit Azure HDInsight-Clustern. Die drei häufigsten Szenarien sind:

* Analysieren von HDInsight-Cluster-Metriken in OMS
* Suchen nach bestimmten Protokollmeldungen für HDInsight-Cluster
* Erstellen von Warnungen basierend auf Ereignissen, die in den Clustern auftreten

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen einen HDInsight-Cluster zur Verwendung von Azure Log Analytics konfiguriert haben. Anweisungen hierzu finden Sie unter [Use Azure Log Analytics to monitor HDInsight clusters (Preview)](hdinsight-hadoop-oms-log-analytics-tutorial.md) (Verwenden von Azure Log Analytics zum Überwachen von HDInsight-Clustern [Vorschau]).

* Sie müssen dem OMS-Arbeitsbereich die für HDInsight-Cluster spezifischen Verwaltungslösungen hinzugefügt haben, wie in [Add HDInsight cluster management solutions to Log Analytics (Preview)](hdinsight-hadoop-oms-log-analytics-management-solutions.md) (Hinzufügen von HDInsight-Clusterverwaltungslösungen zu Azure Log Analytics [Vorschau]) beschrieben.

## <a name="analyze-hdinsight-cluster-metrics-in-oms"></a>Analysieren von HDInsight-Cluster-Metriken in OMS

In diesem Abschnitt führen wir die Schritte zur Suche nach bestimmten Metriken für Ihren HDInsight-Cluster aus.

1. Öffnen Sie das OMS-Dashboard. Öffnen Sie im Azure-Portal das HDInsight-Clusterblatt, das Sie mit Azure Log Analytics zugeordnet haben, klicken Sie auf die Registerkarte „Überwachung“ und dann auf **OMS-Dashboard öffnen**.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

2. Klicken Sie im OMS-Dashboard auf der Startseite auf **Protokollsuche**.

    ![Protokollsuche öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Protokollsuche öffnen")

3. Geben Sie im Fenster „Protokollsuche“ im Textfeld **Hier Suche starten** zur Suche nach allen Metriken für alle verfügbaren Metriken für alle HDInsight-Cluster, die für die Verwendung von Azure Log Analytics konfiguriert sind, `*` ein. Drücken Sie die EINGABETASTE.

    ![Suchen aller Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Suchen aller Metriken")

4. Die Ausgabe sollte wie folgt aussehen.

    ![Ausgabe bei Suchen aller Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "Ausgabe bei Suchen aller Metriken")

5. Suchen Sie im linken Bereich unter der Kategorie **Typ** eine Metrik, die Sie näher untersuchen möchten. Für dieses Tutorial verwenden wir `metrics_resourcemanager_queue_root_default_CL`. Aktivieren Sie das Kontrollkästchen der entsprechenden Metrik, und klicken Sie auf **Übernehmen**.

    > [!NOTE]
    > Möglicherweise müssen Sie auf die Schaltfläche **[+]Weitere** klicken, um die gesuchte Metrik zu finden. Außerdem befindet sich die Schaltfläche **Übernehmen** am unteren Rand der Liste, sodass Sie nach unten scrollen müssen, um sie anzuzeigen.
    > 
    >    
    Beachten Sie, dass die Abfrage im Textfeld sich jetzt in eine ändert, die im folgenden Screenshot im hervorgehoben Feld angezeigt wird:

    ![Suchen nach bestimmten Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Suchen nach bestimmten Metriken")

6. Sie können diese bestimmte Metrik jetzt näher untersuchen. Beispielsweise können Sie jetzt die vorhandene Ausgabe basierend auf dem Mittelwert der in einem Intervall von 10 Minuten verwendeten Ressourcen optimieren – kategorisiert nach Clusternamen. Geben Sie im Abfragetextfeld die folgende Abfrage ein.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure avg(UsedAMResourceMB_d) by ClusterName_s interval 10minute

    ![Suchen nach bestimmten Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics.png "Suchen nach bestimmten Metriken")

7. Anstelle der auf dem Mittelwert der verwendeten Ressourcen basierenden Optimierung können Sie die Ergebnisse mit der folgenden Abfrage basierend auf dem Zeitpunkt in einem 10-Minuten-Fenster optimieren, an dem die meisten Ressourcen verwendet wurden (sowie den Prozentwerten 90 und 95).

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure max(UsedAMResourceMB_d) , pct95(UsedAMResourceMB_d), pct90(UsedAMResourceMB_d)  by ClusterName_s interval 10minute

    ![Suchen nach bestimmten Metriken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics-1.png "Suchen nach bestimmten Metriken")

## <a name="search-for-specific-log-messages-in-hdinsight-clusters"></a>Suchen nach bestimmten Protokollmeldungen in HDInsight-Clustern

In diesem Abschnitt führen wir die Schritte zum Suchen von Fehlermeldungen während eines bestimmten Zeitfensters aus. Die folgenden Schritte sind nur ein Beispiel dafür, wie Sie die Fehlermeldung finden können, die Sie interessiert. Sie können jede verfügbare Eigenschaft verwenden, um nach den Fehlern zu suchen, die Sie finden möchten.

1. Öffnen Sie das OMS-Dashboard. Öffnen Sie im Azure-Portal das HDInsight-Clusterblatt, das Sie mit Azure Log Analytics zugeordnet haben, klicken Sie auf die Registerkarte „Überwachung“ und dann auf **OMS-Dashboard öffnen**.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

2. Klicken Sie im OMS-Dashboard auf der Startseite auf **Protokollsuche**.

    ![Protokollsuche öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Protokollsuche öffnen")

3. Geben Sie im Fenster „Protokollsuche“ im Textfeld **Hier Suche starten** zur Suche nach allen Fehlermeldungen für alle HDInsight-Cluster, die für die Verwendung von Azure Log Analytics konfiguriert sind, `"Error"` (mit den Fragezeichen) ein. Drücken Sie die EINGABETASTE.

    ![Suchen aller Fehler](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors.png "Suchen aller Fehler")

4. Die Ausgabe sollte wie folgt aussehen.

    ![Ausgabe bei Suchen aller Fehler](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "Ausgabe bei Suchen aller Fehler")

5. Suchen Sie im linken Bereich unter der Kategorie **Typ** einen Fehlertyp, den Sie näher untersuchen möchten. Für dieses Tutorial verwenden wir `log_sparkappsexecutors_CL`. Aktivieren Sie das Kontrollkästchen der entsprechenden Metrik, und klicken Sie auf **Übernehmen**.

    ![Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error.png "Suchen nach bestimmten Fehlern")

        
6. Beachten Sie, dass die Abfrage im Textfeld sich jetzt in eine ändert, die im hervorgehobenen Feld unten angezeigt wird, und die Ergebnisse so optimiert sind, dass nur der Fehler des von Ihnen ausgewählten Typs angezeigt wird.

    ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-output.png "Ausgabe bei Suchen nach bestimmten Fehlern")

7. Sie können diese bestimmte Fehlerliste jetzt mithilfe der im linken Bereich verfügbaren Optionen näher untersuchen. Beispielsweise können Sie die Abfrage so optimieren, dass nur Fehlermeldungen aus einem bestimmten Workerknoten berücksichtigt werden.

    ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "Ausgabe bei Suchen nach bestimmten Fehlern")

8. Sie können den Zeitraum weiter eingrenzen, in dem der Fehler nach Ihrer Einschätzung aufgetreten ist, indem Sie den relevanten Zeitwert im linken Bereich auswählen.

    ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "Ausgabe bei Suchen nach bestimmten Fehlern")

9. Sie sind nun bei dem bestimmten Fehler angelangt, den Sie suchen. Sie können auf **[+] Mehr anzeigen** klicken, um die eigentliche Fehlermeldung anzuzeigen.

    ![Ausgabe bei Suchen nach bestimmten Fehlern](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "Ausgabe bei Suchen nach bestimmten Fehlern")

## <a name="create-alerts-to-track-events"></a>Erstellen von Warnungen zum Nachverfolgen von Ereignissen

Damit eine Warnung erstellt werden kann, muss eine Abfrage vorhanden sein, auf deren Basis die Warnung ausgelöst wird. Der Einfachheit halber verwenden wir die folgende Abfrage, die eine Liste auf HDInsight-Clustern ausgeführter Anwendungen bereitstellt, bei denen Fehler aufgetreten sind.

    * (Type=metrics_resourcemanager_queue_root_default_CL) AppsFailed_d>0 

Sie können jede beliebige Abfrage verwenden, um eine Warnung zu erstellen.

1. Öffnen Sie das OMS-Dashboard. Öffnen Sie im Azure-Portal das HDInsight-Clusterblatt, das Sie mit Azure Log Analytics zugeordnet haben, klicken Sie auf die Registerkarte „Überwachung“ und dann auf **OMS-Dashboard öffnen**.

    ![OMS-Dashboard öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS-Dashboard öffnen")

2. Klicken Sie im OMS-Dashboard auf der Startseite auf **Protokollsuche**.

    ![Protokollsuche öffnen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "Protokollsuche öffnen")

3. Fügen Sie im Fenster „Protokollsuche“ im Textfeld **Hier Suche starten** die Abfrage ein, zu der Sie eine Warnung erstellen möchten, drücken Sie EINGABE, und klicken Sie dann auf die Schaltfläche **Warnung**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Abfrage eingeben, um eine Warnung zu erstellen")

4. Geben Sie im Fenster **Warnungsregel hinzufügen** die Abfrage und andere Details zum Erstellen einer Warnung ein, und klicken Sie dann auf **Speichern**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Abfrage eingeben, um eine Warnung zu erstellen")

    In diesem Screenshot senden wir nur eine E-Mail-Benachrichtigung, wenn die Warnungsabfrage eine Ausgabe abruft.

5. Sie können auch eine vorhandene Warnung bearbeiten oder löschen. Klicken Sie dazu auf einer anderen Seite im OMS-Portal auf das Symbol **Einstellungen**.

    ![Abfrage eingeben, um eine Warnung zu erstellen](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "Abfrage eingeben, um eine Warnung zu erstellen")

6. Klicken Sie auf der Seite **Einstellungen** auf **Warnungen**, um die Warnungen anzuzeigen, die Sie erstellt haben. Sie können eine Warnung auch aktivieren oder deaktivieren, bearbeiten oder löschen. Weitere Informationen finden Sie unter [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Weitere Informationen

* [OMS Log Analytics: Create Tiles, Drill-ins and Dashboards with the View Designer](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/) (OMS Log Analytics: Erstellen von Kacheln, Drill-ins und Dashboards mit dem Ansicht-Designer)
* [Arbeiten mit Warnungsregeln in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)

