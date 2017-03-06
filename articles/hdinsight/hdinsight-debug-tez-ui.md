---
title: "Verwenden der Tez-Benutzeroberfläche mit Windows-basiertem HDInsight | Microsoft Docs"
description: "Informationen zum Verwenden der Tez-Benutzeroberfläche zum Debuggen von Tez-Aufträgen in Windows-basiertem HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: ccd1dffda19718a434fc09bb74a536714799740a
ms.openlocfilehash: 6de7798f117f0362ac80a390b7e341c7f6fc01b8
ms.lasthandoff: 01/18/2017


---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a>Verwenden der Tez-Benutzeroberfläche zum Debuggen von Tez-Aufträgen in Windows-basiertem HDInsight
Die Tez-Benutzeroberfläche ist eine Webseite, die verwendet werden kann, um Aufträge zu verstehen und zu debuggen, die Tez als Ausführungsmodul in Windows-basierten Clustern nutzen. Die Tez-Benutzeroberfläche ermöglicht Ihnen das Visualisieren des Auftrags als Graphen verbundener Elemente, einen Drilldown in die einzelnen Elemente und das Abrufen von Statistiken und Protokollinformationen.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Windows. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Windows-basierter HDInsight-Cluster Anweisungen zum Erstellen eines neuen Clusters finden Sie unter [Erste Schritte mit Windows-basiertem HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).
  
  > [!IMPORTANT]
  > Die Tez-Benutzeroberfläche ist nur für Windows-basierte Cluster verfügbar, die nach dem 8. Februar 2016 erstellt wurden.
  > 
  > 
* Ein Windows-basierter Remotedesktopclient.

## <a name="understanding-tez"></a>Grundlegendes zu Tez
Tez ist ein erweiterbares Framework für die Datenverarbeitung in Hadoop, das eine schnellere Verarbeitung als übliche MapReduce-Verfahren bietet. Bei Windows-basierten Clustern handelt es sich um ein optionales Modul, das mit dem folgenden Befehl als Teil Ihrer Hive-Abfrage für Hive aktiviert werden kann:

    set hive.execution.engine=tez;

Wenn Aufträge an Tez übermittelt werden, erstellt das Tool einen gerichteten azyklischen Graphen (Directed Acyclic Graph, DAG), der die Reihenfolge der Ausführung der Aktionen beschreibt, die für den Auftrag erforderlich sind. Einzelne Aktionen werden auch Scheitelpunkte genannt und führen einen Teil des gesamten Auftrags aus. Die tatsächliche Ausführung des Auftrags, die von einem Scheitelpunkt beschrieben wird, heißt Aufgabe und kann auf mehrere Knoten im Cluster verteilt werden.

### <a name="understanding-the-tez-ui"></a>Grundlegendes zur Tez-Benutzeroberfläche
Die Tez-Benutzeroberfläche ist eine Webseite mit Informationen zu Prozessen, die ausgeführt werden oder zuvor mithilfe von Tez ausgeführt wurden. Sie ermöglicht das Anzeigen des von Tez generierten DAG, seiner Verteilung auf Cluster, von Leistungsindikatoren wie für den von Aufgaben und Scheitelpunkten belegten Arbeitsspeicher sowie von Fehlerinformationen. Diese Ansicht kann in den folgenden Szenarien nützliche Informationen bieten:

* Überwachen lang andauernder Prozesse und Anzeigen des Status von Map/Reduce-Aufgaben.
* Analysieren von Verlaufsdaten erfolgreicher oder fehlerhafter Prozesse, um zu erfahren, wie die Verarbeitung verbessert werden kann oder warum sie misslungen ist.

## <a name="generate-a-dag"></a>Generieren eines gerichteten azyklischen Graphen (DAG)
Die Tez-Benutzeroberfläche enthält nur Daten, wenn ein Auftrag, der das Tez-Modul verwendet, derzeit ausgeführt wird oder zuvor ausgeführt wurde. Einfache Hive-Abfragen können normalerweise ohne Tez aufgelöst werden. Doch komplexere Abfragen mit Filterung, Gruppierung, Sortierung, Joins usw. benötigen in der Regel Tez.

Gehen Sie folgendermaßen vor, um eine Hive-Abfrage mit Tez auszuführen.

1. Öffnen Sie einen Webbrowser „https://CLUSTERNAME.azurehdinsight.net“, wobei **CLUSTERNAME** für den Namen des HDInsight-Clusters steht.
2. Wählen im Menü oben auf der Seite das Symbol **Hive-Editor** aus. Dadurch wird eine Seite mit der folgenden Beispielabfrage angezeigt.
   
        Select * from hivesampletable
   
    Löschen Sie die Beispielabfrage, und ersetzen Sie sie durch Folgendes.
   
        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Klicken Sie auf die Schaltfläche **Übermitteln**. Im Abschnitt **Auftragssitzung** unten auf der Seite wird der Status der Abfrage gezeigt. Sobald sich der Status in **Abgeschlossen** ändert, wählen Sie den Link **Details anzeigen**, um die Ergebnisse anzuzeigen. Die **Auftragsausgabe** sollte etwa wie folgt aussehen:
   
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a>Verwenden der Tez-Benutzeroberfläche
> [!NOTE]
> Die Tez-Benutzeroberfläche ist nur auf dem Desktop der Hauptknoten des Clusters verfügbar, weshalb Sie über Remotedesktop eine Verbindung mit den Hauptknoten herstellen müssen.
> 
> 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren HDInsight-Cluster aus. Wählen Sie oben im Blatt „HDInsight“ das Symbol **Remotedesktop** aus. Das Blatt „Remotedesktop“ wird angezeigt.
   
    ![Symbol „Remotedesktop“](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Wählen Sie auf dem Blatt „Remotedesktop“ **Verbinden** aus, um eine Verbindung mit dem Hauptknoten des Clusters herzustellen. Geben Sie bei Aufforderung den Remotedesktop-Benutzernamen samt Kennwort für den Cluster an, um die Verbindung zu authentifizieren.
   
    ![Symbol „Remotedesktopverbindung“](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)
   
   > [!NOTE]
   > Wenn Sie die Remotedesktopverbindung nicht aktiviert haben, geben Sie einen Benutzernamen, ein Kennwort und Ablaufdatum ein, und wählen Sie dann **Aktivieren** aus, um Remotedesktop zu aktivieren. Führen Sie nach der Aktivierung die zuvor genannten Schritte zum Herstellen der Verbindung aus.
   > 
   > 
3. Sobald die Verbindung steht, öffnen Sie auf dem Remotedesktop Internet Explorer. Wählen Sie rechts oben im Browser das Zahnradsymbol und dann **Einstellungen der Kompatibilitätsansicht** aus.
4. Deaktivieren Sie unten in **Einstellungen der Kompatibilitätsansicht** die Kontrollkästchen **Intranetsites in Kompatibilitätsansicht anzeigen** und **Kompatibilitätslisten von Microsoft verwenden**, und wählen Sie dann **Schließen** aus.
5. Navigieren Sie in Internet Explorer zu „http://headnodehost:8188/tezui/#/“. Die Tez-Benutzeroberfläche wird geöffnet.
   
    ![Tez-Benutzeroberfläche](./media/hdinsight-debug-tez-ui/tezui.png)
   
    Nach Laden der Tez-Benutzeroberfläche sehen Sie eine Liste mit gerichteten azyklischen Graphen (Directed Acyclic Graphs, DAGs), die derzeit im Cluster ausgeführt werden oder zuvor ausgeführt wurden. Die Standardansicht enthält die folgenden Angaben für den DAG: Name, Id, Submitter, Status, Start Time, End Time, Duration, Application ID und Queue. Weitere Spalten können über das Zahnradsymbol rechts auf der Seite hinzugefügt werden.
   
    Wenn nur ein Eintrag vorhanden ist, gehört dieser zur Abfrage, die Sie im vorherigen Abschnitt ausgeführt haben. Wenn mehrere Einträge vorhanden sind, können Sie eine Suche durch Eingeben von Suchkriterien in die Felder über den DAGs und Drücken der **EINGABETASTE** starten.
6. Wählen Sie den **DAG-Namen** des letzten DAG-Eintrags aus. Daraufhin werden Informationen zum DAG sowie die Option zum Herunterladen einer ZIP-Datei mit JSON-Dateien mit Informationen zum DAG angezeigt.
   
    ![DAG-Details](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Über den **DAG-Details** befinden sich mehrere Links, die zum Anzeigen von Informationen über den DAG verwendet werden können.
   
   * **DAG Counters** (DAG-Leistungsindikatoren) zeigt Informationen zu Leistungsindikatoren für diesen DAG an.
   * **Graphical View** (Grafische Ansicht) zeigt eine grafische Darstellung dieses DAG an.
   * **All Vertices** (Alle Scheitelpunkte) zeigt eine Liste mit allen Scheitelpunkten in diesem DAG an.
   * **All Tasks** (Alle Aufgaben) zeigt eine Liste der Aufgaben für alle Scheitelpunkte in diesem DAG.
   * **All TaskAttempts** (Alle Aufgabenversuche) zeigt Informationen zu den Versuchen der Ausführung von Aufgaben für diesen DAG.
     
     > [!NOTE]
     > Wenn Sie die Spaltenanzeige für „Vertices“, „Tasks“ und „TaskAttempts“ durchlaufen, sehen Sie Links zum Anzeigen von **Leistungsindikatoren** und **Anzeigen oder Herunterladen von Protokollen** für jede Zeile.
     > 
     > 
     
     Wenn beim Auftrag ein Fehler aufgetreten ist, werden in „DAG Details“ der Status FAILED sowie Informationen zur fehlerhaften Aufgabe angezeigt. Diagnoseinformationen wird unter den DAG-Details angezeigt.
8. Wählen Sie **Graphical View**aus. Es wird eine grafische Darstellung des DAG gezeigt. Sie können den Mauszeiger über jedem Scheitelpunkt in der Ansicht platzieren, um Informationen dazu anzuzeigen.
   
    ![Graphical View](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Durch Klicken auf einen Scheitelpunkt werden die **Scheitelpunktdetails** für dieses Element geladen. Klicken Sie auf den Scheitelpunkt **Map 1**, um Informationen zu diesem Element anzuzeigen. Wählen Sie **Bestätigen**, um die Navigation zu bestätigen.
   
    ![Vertex Details](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Beachten Sie, dass sich nun oben auf der Seite Links befinden, die sich auf Scheitelpunkte und Aufgaben beziehen.
    
    > [!NOTE]
    > Sie können auch auf diese Seite gelangen, indem Sie zu **DAG Details** zurückkehren, die **Scheitelpunktdetails** und dann den Scheitelpunkt **Map 1** auswählen.
    > 
    > 
    
    * **Vertex Counters** (Scheitelpunkt-Leistungsindikatoren) zeigt Leistungsindikatorinformationen für diesen Scheitelpunkt an.
    * **Tasks** (Aufgaben) zeigt Aufgaben für diesen Scheitelpunkt an.
    * **Task Attempts** (Aufgabenversuche) zeigt Informationen zu Versuchen der Ausführung von Aufgaben für diesen Scheitelpunkt an.
    * **Sources & Sinks** (Quellen und Senken) zeigt Datenquellen und Senken für diesen Scheitelpunkt an.
      
      > [!NOTE]
      > Wie beim vorherigen Menü können Sie die Spaltenanzeige für „Tasks“, „Task Attempts“ und „Sources & Sinks“ durchlaufen, um Links zu weiteren Informationen zu den einzelnen Elementen anzuzeigen.
      > 
      > 
11. Wählen Sie **Tasks** und dann das Element mit dem Namen **00_000000** aus. Dadurch werden **Task Details** für diesen Task angezeigt. Auf diesem Bildschirm können Sie **Leistungsindikatoren für Aufgaben** und **Aufgabenversuche** anzeigen.
    
    ![Taskdetails](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit der Verwendung der Tez-Ansicht vertraut gemacht haben, erfahren Sie mehr zum [Verwenden von Hive in HDInsight](hdinsight-use-hive.md).

Ausführliche technische Informationen zu Tez finden Sie auf der [Seite zu Tez bei Hortonworks](http://hortonworks.com/hadoop/tez/).


