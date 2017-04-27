---
title: Verwenden der Ambari-Tez-Ansicht mit HDInsight | Microsoft Docs
description: "Erfahren Sie, wie Sie die Ambari-Tez-Ansicht zum Debuggen von Tez-Aufträgen in HDInsight verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/14/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 08dac5205e50dd5c33d71ba15277da66fd7b22fe
ms.lasthandoff: 04/17/2017


---
# <a name="use-ambari-views-to-debug-tez-jobs-on-hdinsight"></a>Debuggen von Tez-Aufträgen in HDInsight mithilfe von Ambari-Ansichten

Die Ambari-Webbenutzeroberfläche für HDInsight enthält eine Tez-Ansicht, die verwendet werden kann, um Aufträge zu verstehen und zu debuggen, die Tez nutzen. Die Tez-Ansicht ermöglicht Ihnen das Visualisieren des Auftrags als Graphen verbundener Elemente, einen Drilldown in die einzelnen Elemente und das Abrufen von Statistiken und Protokollinformationen.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Linux-basierter HDInsight-Cluster. Anweisungen zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit Linux-basierten HDInsight-Clustern](hdinsight-hadoop-linux-tutorial-get-started.md).
* Ein zeitgemäßer Webbrowser, der HTML5 unterstützt.

## <a name="understanding-tez"></a>Grundlegendes zu Tez

Tez ist ein erweiterbares Framework für die Datenverarbeitung in Hadoop, das eine schnellere Verarbeitung als übliche MapReduce-Verfahren bietet. Bei Linux-basierten Clustern ist Tez das Standardmodul für Hive.

Tez erstellt einen gerichteten azyklischen Graphen (Directed Acyclic Graph, DAG), der die Reihenfolge der Aktionen beschreibt, die für Aufgaben erforderlich sind. Einzelne Aktionen werden auch Scheitelpunkte genannt und führen einen Teil des gesamten Auftrags aus. Die tatsächliche Ausführung des Auftrags, die von einem Scheitelpunkt beschrieben wird, heißt Aufgabe und kann auf mehrere Knoten im Cluster verteilt werden.

### <a name="understanding-the-tez-view"></a>Grundlegendes zur Tez-Ansicht

Die Tez-Ansicht stellt Verlaufsdaten und Informationen zu laufenden Prozessen bereit. Diese Informationen verdeutlichen, wie ein Auftrag auf Clustern verteilt ist. Außerdem werden Indikatoren, die von Aufgaben und Scheitelpunkten verwendet werden, sowie Fehlerinformationen, die im Zusammenhang mit dem Auftrag stehen, angezeigt. Diese Ansicht kann in den folgenden Szenarien nützliche Informationen bieten:

* Überwachen lang andauernder Prozesse und Anzeigen des Status von Map/Reduce-Aufgaben.
* Analysieren von Verlaufsdaten erfolgreicher oder fehlerhafter Prozesse, um zu erfahren, wie die Verarbeitung verbessert werden kann oder warum sie misslungen ist.

## <a name="generate-a-dag"></a>Generieren eines gerichteten azyklischen Graphen (DAG)

Die Tez-Ansicht enthält nur Daten, wenn ein Auftrag, der das Tez-Modul verwendet, derzeit ausgeführt wird oder zuvor ausgeführt wurde. Einfache Hive-Abfragen können ohne Tez aufgelöst werden. Komplexere Abfragen, die Filterung, Gruppierung, Sortierung, Joins usw. ausführen,  verwenden das Tez-Modul.

Gehen Sie folgendermaßen vor, um eine Hive-Abfrage mit Tez auszuführen:

1. Öffnen Sie einen Webbrowser „https://CLUSTERNAME.azurehdinsight.net“, wobei **CLUSTERNAME** für den Namen des HDInsight-Clusters steht.

2. Wählen im Menü oben auf der Seite das Symbol **Ansichten** aus. Das Symbol sieht aus wie eine Reihe von Quadraten. Wählen Sie in der eingeblendeten Dropdownliste **Hive-Ansicht** aus.

    ![Auswählen der Hive-Ansicht](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. Fügen Sie in der geladenen Hive-Ansicht die folgende Abfrage in den Abfrage-Editor ein, und klicken Sie dann auf **Ausführen**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    Sobald der Auftrag abgeschlossen ist, sollte die Ausgabe im Abschnitt **Abfrageprozessergebnisse** angezeigt werden. Die Ergebnisse sollten in etwa wie der folgende Text aussehen:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Wählen Sie die Registerkarte **Protokoll** aus. Die angezeigten Informationen sehen in etwa folgendermaßen aus:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Speichern Sie den Wert von **App-ID**, da dieser im nächsten Abschnitt verwendet wird.

## <a name="use-the-tez-view"></a>Verwenden der Tez-Ansicht

1. Wählen im Menü oben auf der Seite das Symbol **Ansichten** aus. Wählen Sie in der eingeblendeten Dropdownliste **Tez-Ansicht** aus.

    ![Auswählen der Tez-Ansicht](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. Nach Laden der Tez-Ansicht sehen Sie eine Liste mit gerichteten azyklischen Graphen (Directed Acyclic Graphs, DAGs), die derzeit im Cluster ausgeführt werden oder zuvor ausgeführt wurden.

    ![Alle DAGs](./media/hdinsight-debug-ambari-tez-view/alldags.png)

3. Wenn nur ein Eintrag vorhanden ist, gehört dieser zur Abfrage, die Sie im vorherigen Abschnitt ausgeführt haben. Wenn mehrere Einträge vorhanden sind, können Sie eine Suche durch Eingeben der Anwendungs-ID in das Feld **Anwendungs-ID** und Drücken der EINGABETASTE starten.

4. Wählen Sie den **Namen des DAG** aus. Informationen zum DAG werden angezeigt. Sie können auch eine ZIP-Datei mit JSON-Dateien, die diese Informationen enthalten, herunterladen.

    ![DAG-Details](./media/hdinsight-debug-ambari-tez-view/dagdetails.png)

5. Über den **DAG-Details** befinden sich mehrere Links, die zum Anzeigen von Informationen über den DAG verwendet werden können.

   * **DAG Counters** (DAG-Leistungsindikatoren): Zeigt Informationen zu Leistungsindikatoren für diesen DAG an.
   * **Graphical View** (Grafische Ansicht): Zeigt eine grafische Darstellung dieses DAG an.
   * **All Vertices** (Alle Scheitelpunkte): Zeigt eine Liste mit allen Scheitelpunkten in diesem DAG an.
   * **All Tasks** (Alle Aufgaben): Zeigt eine Liste der Aufgaben für alle Scheitelpunkte in diesem DAG.
   * **All TaskAttempts** (Alle Aufgabenversuche): Zeigt Informationen zu den Versuchen der Ausführung von Aufgaben für diesen DAG.

     > [!NOTE]
     > Wenn Sie die Spaltenanzeige für „Vertices“, „Tasks“ und „TaskAttempts“ durchlaufen, sehen Sie Links zum Anzeigen von **Leistungsindikatoren** und **Anzeigen oder Herunterladen von Protokollen** für jede Zeile.

     Wenn beim Auftrag ein Fehler aufgetreten ist, werden in „DAG Details“ der Status FAILED sowie Informationen zur fehlerhaften Aufgabe angezeigt. Diagnoseinformationen werden unter den DAG-Details angezeigt.

     ![Eine DAG-Detailbildschirm mit Details zu einem Fehler](./media/hdinsight-debug-ambari-tez-view/faileddag.png)

6. Wählen Sie **Graphical View**aus. In dieser Ansicht wird eine grafische Darstellung des DAG gezeigt. Sie können den Mauszeiger über jedem Scheitelpunkt in der Ansicht platzieren, um Informationen dazu anzuzeigen.

    ![Graphical View](./media/hdinsight-debug-ambari-tez-view/dagdiagram.png)

7. Wählen Sie einen Scheitelpunkt aus, um die **Scheitelpunktdetails** für dieses Element zu laden. Wählen Sie den Scheitelpunkt **Map 1** aus, um Informationen zu diesem Element anzuzeigen.

    ![Vertex Details](./media/hdinsight-debug-ambari-tez-view/vertexdetails.png)

8. Oben auf der Seite werden jetzt Links angezeigt, die sich auf Scheitelpunkte und Aufgaben beziehen.

   > [!NOTE]
   > Sie können auch auf diese Seite gelangen, indem Sie zu **DAG Details** zurückkehren, die **Scheitelpunktdetails** und dann den Scheitelpunkt **Map 1** auswählen.

   * **Vertex Counters** (Scheitelpunkt-Leistungsindikatoren): Zeigt Leistungsindikatorinformationen für diesen Scheitelpunkt an.
   * **Tasks** (Aufgaben): Zeigt Aufgaben für diesen Scheitelpunkt an.
   * **Task Attempts** (Aufgabenversuche): Zeigt Informationen zu Versuchen der Ausführung von Aufgaben für diesen Scheitelpunkt an.
   * **Sources & Sinks** (Quellen und Senken): Zeigt Datenquellen und Senken für diesen Scheitelpunkt an.

     > [!NOTE]
     > Wie beim vorherigen Menü können Sie die Spaltenanzeige für „Tasks“, „Task Attempts“ und „Sources & Sinks“ durchlaufen, um Links zu weiteren Informationen zu den einzelnen Elementen anzuzeigen.

9. Wählen Sie **Tasks** und dann das Element mit dem Namen **00_000000** aus. Die **Aufgabendetails** für diese Aufgabe werden angezeigt. Auf diesem Bildschirm können Sie **Leistungsindikatoren für Aufgaben** und **Aufgabenversuche** anzeigen.

   ![Taskdetails](./media/hdinsight-debug-ambari-tez-view/taskdetails.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit der Verwendung der Tez-Ansicht vertraut gemacht haben, erfahren Sie mehr zum [Verwenden von Hive in HDInsight](hdinsight-use-hive.md).

Ausführliche technische Informationen zu Tez finden Sie auf der [Seite zu Tez bei Hortonworks](http://hortonworks.com/hadoop/tez/).

Weitere Informationen zur Verwendung von Ambari mit HDInsight finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md)

