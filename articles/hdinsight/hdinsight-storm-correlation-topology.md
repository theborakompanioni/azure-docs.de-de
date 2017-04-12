---
title: Korrelieren von Ereignissen im Zeitverlauf mithilfe von Storm und HBase in HDInsight
description: Erfahren Sie, wie Ereignisse, die zu unterschiedlichen Zeitpunkten eingehen, mithilfe von Storm und HBase in HDInsight korreliert werden.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d0a5bc9d3365a48b03d6f53c21d672a928777956
ms.lasthandoff: 04/12/2017

---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a>Korrelieren von Ereignissen, die zu unterschiedlichen Zeiten eintreffen, mithilfe von Storm und HBase

Durch die Verwendung eines persistenten Datenspeichers für Apache Storm können Sie Dateneinträge korrelieren, die zu unterschiedlichen Zeitpunkten eingehen. Beispielsweise kann durch die Verknüpfung der An- und Abmeldungsereignisse für eine Benutzersitzung berechnet werden, wie lange die Sitzung gedauert hat.

In diesem Dokument erfahren Sie, wie Sie eine einfache C#-Storm-Topologie erstellen, die Anmelde-und Abmeldevorgänge für Benutzersitzungen verfolgt und die Sitzungsdauer berechnet. In dieser Topologie wird HBase als persistenter Datenspeicher verwendet. Mit HBase können Sie auch Batch-Abfragen auf den Verlaufsdaten ausführen, um zusätzliche Erkenntnisse zu gewinnen. Zum Beispiel, wie viele Benutzersitzungen während eines bestimmten Zeitraums gestartet oder beendet wurden.

## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio und HDInsight-Tools für Visual Studio Weitere Informationen finden Sie unter [Erste Schritte mit den HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Apache Storm in HDInsight-Cluster (Windows-basiert).

  > [!IMPORTANT]
  > Zwar werden SCP.NET-Topologien auf Linux-basierten Storm-Clustern unterstützt, die nach dem 28.10.2016 erstellt wurden, aber das HBase-SDK für das .NET-Paket, das seit dem 28.10.2016 verfügbar ist, funktioniert unter Linux nicht ordnungsgemäß.

* Apache HBase in HDInsight-Cluster (Linux- oder Windows-basiert)

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* [Java](https://java.com) 1.7 oder höher in Ihrer Entwicklungsumgebung. Java wird verwendet, um die Topologie zu verpacken, wenn sie an den HDInsight-Cluster übermittelt wird.

  * Die **JAVA_HOME**-Umgebungsvariable muss auf das Verzeichnis mit Java zeigen.
  * Das **%JAVA_HOME%/bin**-Verzeichnis muss im Pfad liegen.

## <a name="architecture"></a>Architektur

![Diagramm des Datenflusses in der Topologie](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

Zum Korrelieren von Ereignissen ist ein allgemeiner Bezeichner für die Ereignisquelle erforderlich. Z. B. eine Benutzer-ID, Sitzungs-ID oder andere Daten, die (a) eindeutig und (b) in allen Daten, die an Storm gesendet werden, enthalten sind. In diesem Beispiel wird ein GUID-Wert zur Darstellung der Sitzungs-ID verwendet.

Dieses Beispiel besteht aus zwei HDInsight-Clustern:

* HBase: persistenter Datenspeicher für historische Daten
* Storm: wird zum Erfassen eingehender Daten verwendet

Die Daten werden von der Storm-Topologie nach dem Zufallsprinzip generiert und beinhalten die folgenden Elemente:

* Sitzungs-ID: eine GUID, die jede Sitzung eindeutig identifiziert
* Ereignis: ein START- oder END-Ereignis. In diesem Beispiel tritt das START- immer vor dem END-Ereignis auf.
* Zeit: die Uhrzeit des Ereignisses.

Diese Daten werden verarbeitet und in HBase gespeichert.

### <a name="storm-topology"></a>Storm-Topologie

Beim Start einer Sitzung wird ein **START** -Ereignis von der Topologie empfangen und in HBase protokolliert. Sobald ein **END**-Ereignis empfangen wird, ruft die Topologie das **START**-Ereignis ab und berechnet den Zeitraum zwischen den beiden Ereignissen. Dieser Wert (**Duration**) wird dann zusammen mit den **END**-Ereignisinformationen in HBase gespeichert.

> [!IMPORTANT]
> Mit diese Topologie wird das grundlegende Muster beschrieben, in einer Produktionslösung müssten jedoch auch die folgenden Szenarien berücksichtigt werden:
>
> * Ereignisse, die nicht in der vorgesehenen Reihenfolge eintreffen
> * Doppelte Ereignisse
> * Gelöschte Ereignisse

Die Beispieltopologie besteht aus den folgenden Komponenten:

* Session.cs: Simuliert eine Benutzersitzung durch Erstellen einer zufälligen Sitzungs-ID, einer Startzeit und einer Sitzungsdauer.

* Spout.cs: Erstellt 100 Sitzungen, gibt ein START-Ereignis aus, wartet auf den Ablauf des zufälligen Timeouts für jede Sitzung und gibt dann ein END-Ereignis aus. Anschließend werden beendete Sitzungen wiederverwendet, um neue Sitzungen zu generieren.

* HBaseLookupBolt.cs: Verwendet die Sitzungs-ID, um die Sitzungsinformationen in HBase zu suchen. Während der Verarbeitung eines END-Ereignisses sucht das Programm das zugehörige START-Ereignis und berechnet die Dauer der Sitzung.

* HBaseBolt.cs: Speichert Informationen in HBase.

* TypeHelper.cs: Hilft bei der Typkonvertierung beim Lesen aus und Schreiben in HBase.

### <a name="hbase-schema"></a>HBase-Schema

In HBase werden die Daten in einer Tabelle mit den folgenden Schemaeinstellungen gespeichert:

* Zeilenschlüssel: Die Sitzungs-ID wird als Schlüssel für die Zeilen in dieser Tabelle verwendet.

* Spaltenfamilie: der Familienname lautet 'cf'. Spalten, die in dieser Familie gespeichert werden:

  * event: START oder END.

  * time: die Uhrzeitangabe in Millisekunden, zu der das Ereignis aufgetreten ist.

  * duration: die Länge des Zeitraums zwischen START- und END-Ereignis.

* VERSIONS: Die Familie 'cf' ist so konfiguriert, dass 5 Versionen jeder Zeile beibehalten werden.

  > [!NOTE]
  > Versionen sind ein Protokoll der vorherigen Werte, die für einen bestimmte Zeilenschlüssel gespeichert wurden. Standardmäßig gibt HBase nur den Wert für die neueste Version einer Zeile zurück. In diesem Fall wird dieselbe Zeile für alle Ereignisse (START, END) verwendet. Jede Version einer Zeile wird durch den Timestampwert identifiziert. Versionen stellen eine historische Ansicht der Ereignisse bereit, die für eine bestimmte ID protokolliert wurden.

## <a name="download-the-project"></a>Herunterladen des Projekts

Das Beispielprojekt kann von [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation)heruntergeladen werden.

Dieser Download enthält die folgenden C#-Projekte:

* CorrelationTopology: C#-Storm-Topologie, die nach dem Zufallsprinzip Start- und End-Ereignisse für Benutzersitzungen ausgibt. Jede Sitzung dauert zwischen 1 und 5 Minuten.

* SessionInfo: C#-Konsolenanwendung, die die HBase-Tabelle erstellt und Beispiele für Abfragen zur Rückgabe von Informationen über gespeicherte Daten enthält.

## <a name="create-the-table"></a>Erstellen der Tabelle

1. Öffnen Sie das Projekt **SessionInfo** in Visual Studio.

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **SessionInfo**, und wählen Sie **Eigenschaften** aus.

    ![Screenshot des Menüs mit den ausgewählten Eigenschaften](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. Wählen Sie **Einstellungen**, legen Sie die folgenden Werte fest:

   * HBaseClusterURL: die URL für den HBase-Cluster. Beispiel: https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: das Administrator-/HTTP-Benutzerkonto für den Cluster.

   * HBaseClusterPassword: das Kennwort für das Administrator-/HTTP-Benutzerkonto.

   * HBaseTableName: der Name der Tabelle, die mit diesem Beispiel verwendet wird.

   * HBaseTableColumnFamily: Der Spaltenfamilienname.

     ![Bild des Dialogfelds "Einstellungen"](./media/hdinsight-storm-correlation-topology/settings.png)

4. Führen Sie die Lösung aus. Wenn Sie dazu aufgefordert werden, wählen Sie 'c' zum Erstellen der Tabelle in Ihrem HBase-Cluster.

## <a name="build-and-deploy-the-storm-topology"></a>Erstellen und Bereitstellen der Storm-Topologie

1. Öffnen Sie die Lösung **CorrelationTopology** in Visual Studio.

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **CorrelationTopology**, und wählen Sie „Eigenschaften“ aus.

3. Wählen Sie im Eigenschaftenfenster **Einstellungen** aus, und geben Sie die Konfigurationswerte für dieses Projekt an. Die ersten 5 Werte sind mit den Werten identisch, die im Projekt **SessionInfo** verwendet wurden:

   * HBaseClusterURL: die URL für den HBase-Cluster. Beispiel: https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: das Administrator-/HTTP-Benutzerkonto für den Cluster.

   * HBaseClusterPassword: das Kennwort für das Administrator-/HTTP-Benutzerkonto.

   * HBaseTableName: der Name der Tabelle, die in diesem Beispiel verwendet wird. Dieser Wert ist der gleiche Tabellenname wie der, der für das SessionInfo-Projekt verwendet wurde.

   * HBaseTableColumnFamily: Der Spaltenfamilienname. Dieser Wert ist die gleiche Name der Spaltenfamilie wie der, der für das SessionInfo-Projekt verwendet wurde.

   > [!IMPORTANT]
   > Verändern Sie HBaseTableColumnNames nicht, da die Standardeinstellungen den Namen entsprechen, die in **SessionInfo** zum Abrufen von Daten verwendet werden.

4. Speichern Sie die Eigenschaften aus, und erstellen Sie das Projekt.

5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **An Storm in HDInsight übermitteln** aus. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement ein.

   ![Bild des Storm-Menüelements "An Storm senden"](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. Wählen Sie im Dialogfeld **Topologie übermitteln** den Storm-Cluster aus, in dem Sie diese Topologie bereitstellen möchten.

   > [!NOTE]
   > Bei der ersten Übermittlung einer Topologie kann das Abrufen der Namen Ihrer HDInsight-Cluster einige Sekunden dauern.

7. Sobald die Topologie hochgeladen und an den Cluster gesendet wurde, wird die **Storm-Topologieansicht** geöffnet und die aktive Topologie angezeigt. Wählen Sie **CorrelationTopology** aus, und verwenden Sie die Schaltfläche „Aktualisieren“ oben rechts auf der Seite, um die Daten zu aktualisieren.

   ![Bild der Topologie-Ansicht](./media/hdinsight-storm-correlation-topology/topologyview.png)

   Wenn die Topologie mit dem Generieren von Daten beginnt, erhöht sich der Wert in der Spalte **Emitted** .

   > [!NOTE]
   > Wenn die **Storm-Topologieansicht** nicht automatisch geöffnet wird, gehen Sie folgendermaßen vor, um diese Ansicht zu öffnen:
   >
   > 1. Erweitern Sie im **Projektmappen-Explorer** zunächst **Azure** und dann **HDInsight**.
   > 2. Klicken Sie mit der rechten Maustaste auf den Storm-Cluster, in dem die Topologie ausgeführt wird, und wählen Sie dann **View Storm Topologies**

## <a name="query-the-data"></a>Abfragen von Daten

Sobald Daten ausgegeben wurden, gehen Sie folgendermaßen vor, um die Daten abzufragen.

1. Kehren Sie zum Projekt **SessionInfo** zurück. Wenn es nicht ausgeführt wird, starten Sie eine neue Instanz dieses Projekts.

2. Wenn Sie dazu aufgefordert werden, wählen Sie **s**, um nach dem START-Ereignis zu suchen. Sie werden aufgefordert, eine Start- und eine Endzeit zum Definieren eines Zeitraums einzugeben. Nur Ereignisse zwischen diesen beiden Zeitangaben werden zurückgegeben.

    Verwenden Sie zur Eingabe der Start- und Endzeiten folgendes Format: HH:MM und "am" oder "pm". Beispiel: 11:20pm.

    Um protokollierte Ereignisse zurückzugeben, wählen Sie eine Startzeit aus die vor der Bereitstellung der Storm-Topologie liegt, und als Endzeit die aktuelle Uhrzeit. Die Rückgabedaten enthält Einträge ähnlich dem folgenden Text:

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

Die Suche nach END-Ereignissen wird auf dieselbe Weise wie die Suche nach START-Ereignissen ausgeführt. END-Ereignisse werden jedoch nach dem Zufallsprinzip zwischen 1 und 5 Minuten nach dem START-Ereignis generiert. Sie müssen möglicherweise einige Zeitabschnitte ausprobieren, um die END-Ereignisse zu finden. END-Ereignisse enthalten außerdem die Dauer der Sitzung. Dies ist die Differenz zwischen der Zeitangabe des START-Ereignisses und der Zeitangabe des END-Ereignisses. Es folgen Beispieldaten für END-Ereignisse:

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> Sie geben die Zeitwerte in Ortszeit ein, von der Abfrage werden jedoch UTC-Zeitwerte zurückgegeben.

## <a name="stop-the-topology"></a>Beenden der Topologie

Wenn Sie die Topologie beenden möchten, kehren Sie zum Projekt **CorrelationTopology** in Visual Studio zurück. Wählen Sie in der **Storm-Topologieansicht** die Topologie aus, und verwenden Sie dann die Schaltfläche **Kill** am oberen Rand der Topologieansicht.

## <a name="delete-your-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Storm-Beispiele finden Sie unter [Beispieltopologien für Storm in HDInsight](hdinsight-storm-example-topology.md).

