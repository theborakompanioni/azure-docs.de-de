---
title: Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight | Microsoft Docs
description: "Erfahren Sie, wie Sie Apache Storm-Topologien mithilfe des Storm-Dashboardsin HDInsight bereitstellen, überwachen und verwalten. Hadoop-Tools für Visual Studio verwenden"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 1160dbc2f2a6b8e427c5280b2eaa06307ecac80c
ms.openlocfilehash: 9b22dc671cd63f77ad6076404069c271455dc61e


---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight unter Windows
Mithilfe des Storm-Dashboards können Sie Apache Storm-Topologien für Ihren HDInsight-Cluster über Ihren Webbrowser ganz einfach bereitstellen und ausführen. Sie können das Dashboard auch zum Überwachen und Verwalten von aktiven Topologien verwenden. Wenn Sie Visual Studio verwenden, bieten die HDInsight Tools für Visual Studio ähnliche Features in Visual Studio.

Sowohl das Storm-Dashboard als auch die Storm-Features der HDInsight-Tools sind von der Storm-REST-API abhängig, mit der Sie eigene Lösungen zur Überwachung und Verwaltung erstellen können.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen Windows-basierten Storm in HDInsight-Cluster. Informationen zur Verwendung eines Linux-basierten Clusters finden Sie unter [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight unter Linux](hdinsight-storm-deploy-monitor-topology-linux.md)
>
>

## <a name="prerequisites"></a>Voraussetzungen
* **Apache Storm in HDInsight** – Informationen zum Erstellen eines Clusters finden Sie unter <a href="hdinsight-apache-storm-tutorial-get-started.md" target="_blank">Erste Schritte mit Apache Storm in HDInsight</a>
* Für das **Storm-Dashboard**: ein zeitgemäßer Webbrowser, der HTML5 unterstützt.
* Für **Visual Studio** : Azure SDK 2.5.1 oder höher und die HDInsight-Tools für Visual Studio. Informationen zum Installieren und Konfigurieren der HDInsight-Tools für Visual Studio finden Sie unter <a href="hdinsight-hadoop-visual-studio-tools-get-started.md" target="_blank">Erste Schritte mit den HDInsight-Tools für Visual Studio</a>.

    Eine der folgenden Versionen von Visual Studio:

  * Visual Studio 2012 mit <a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">Update 4</a>
  * Visual Studio 2013 mit <a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">Update 4</a> oder <a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio 2013 Community</a>
  * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>

    > [!NOTE]
    > Die HDInsight-Tools für Visual Studio unterstützen derzeit nur Storm auf der HDInsight-Clusterversion 3.2.
    >
    >

## <a name="storm-dashboard"></a>Storm-Dashboard
Das Storm-Dashboard ist eine Webseite, die in Ihrem Storm-Cluster zur Verfügung steht. Die URL lautet **https://&lt;Clustername.azurehdinsight.net/**, wobei **Clustername** den Namen des Storm-Clusters in HDInsight angibt.

Wählen Sie oben im Storm-Dashboard die Option **Topologie übermitteln**aus. Befolgen Sie die Anweisungen auf der Seite, um eine Beispieltopologie auszuführen oder eine von Ihnen erstellte Topologie hochzuladen und auszuführen.

![die Seite "Topologie übermitteln"][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm-Benutzeroberfläche
Wählen Sie im Storm-Dashboard den Link **Storm-UI** aus. Dadurch werden die Informationen zum Cluster sowie alle aktiven Topologien angezeigt.

![die Storm UI][storm-dashboard-ui]

> [!NOTE]
> Bei einigen Versionen von Internet Explorer kann es passieren, dass die Storm-Benutzeroberfläche nach dem ersten Öffnen nicht aktualisiert wird. Beispielsweise werden die von Ihnen übermittelten neuen Topologien nicht gezeigt, oder es wird eine Topologie als aktiv angezeigt, die Sie zuvor deaktiviert haben. Microsoft ist sich dieses Umstands bewusst und arbeitet an einer Lösung.
>
>

#### <a name="main-page"></a>Hauptseite
Die Hauptseite der Storm-Benutzeroberfläche bietet die folgenden Informationen:

* **Clusterzusammenfassung:**grundlegende Informationen zum Storm-Cluster.
* **Topologiezusammenfassung:**eine Liste der aktiven Topologien. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Topologien anzuzeigen.
* **Supervisor-Zusammenfassung:**Informationen zum Storm-Supervisor.
* **Nimbus-Konfiguration:**die Nimbus-Konfiguration für den Cluster.

#### <a name="topology-summary"></a>Topologiezusammenfassung:
Wenn Sie einen Link aus dem Abschnitt **Topologiezusammenfassung** auswählen, werden die folgenden Informationen zur Topologie angezeigt:

* **Topologiezusammenfassung:**grundlegende Informationen zur Topologie.
* **Topologieaktionen:**Verwaltungsaktionen, die für die Topologie ausgeführt werden können.

  * **Aktivieren:**setzt die Verarbeitung einer deaktivierten Topologie fort.
  * **Deaktivieren:**hält eine aktive Topologie an.
  * **Ausgleichen:**passt die Parallelität der Topologie an. Sie sollten aktive Topologien ausgleichen, nachdem Sie die Anzahl der Knoten im Cluster geändert haben. Dadurch kann die Topologie die Parallelität anpassen, um die höhere oder geringere Anzahl der Knoten im Cluster zu kompensieren.

      Weitere Informationen finden Sie unter <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the Parallelism of a Storm Topology</a> (Grundlegendes zur Parallelität einer Storm-Topologie, in englischer Sprache).
  * **Beenden:**beendet eine Storm-Topologie nach dem angegebenen Timeout.
* **Topologiestatistik:**Statistiken zur Topologie. Verwenden Sie die Links in der Spalte **Fenster** , um den Zeitrahmen für die verbleibenden Einträge auf der Seite festzulegen.
* **Spouts:**die von der Topologie verwendeten Spouts. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Spouts anzuzeigen.
* **Bolts:**die von der Topologie verwendeten Bolts. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Bolts anzuzeigen.
* **Topologiekonfiguration:**die Konfiguration der ausgewählten Topologie.

#### <a name="spout-and-bolt-summary"></a>Spout und Bolt: Zusammenfassung
Wenn Sie im Abschnitt **Spouts** oder **Bolts** einen Spout auswählen, werden die folgenden Informationen zum ausgewählten Element angezeigt:

* **Komponentenübersicht:**grundlegende Informationen zum Spout oder Bolt.
* **Statistik für Spout/Bolt:**Statistiken zum Spout oder Bolt. Verwenden Sie die Links in der Spalte **Fenster** , um den Zeitrahmen für die verbleibenden Einträge auf der Seite festzulegen.
* **Eingabestatistik** (nur Bolt): Informationen zu den Eingabedatenströmen, die vom Bolt verbraucht werden.
* **Ausgabestatistik:**Informationen zu den Datenströmen, die von diesem Spout oder Bolt ausgegeben werden.
* **Ausführer:**Informationen zu den Instanzen von Spout oder Bolt. Wählen Sie den Eintrag **Port** für einen bestimmten Ausführer aus, um ein Protokoll mit Diagnoseinformationen anzuzeigen, das für diese Instanz generiert wurde.
* **Fehler:**Fehlerinformationen für diesen Spout oder Bolt.

## <a name="hdinsight-tools-for-visual-studio"></a>HDInsight Tools für Visual Studio
Mithilfe der HDInsight-Tools können C#- oder hybride Topologien an Ihre Storm-Cluster übermittelt werden. Für die folgenden Schritte wird eine Beispielanwendung verwendet. Informationen über das Erstellen Ihrer eigenen Topologien mithilfe der HDInsight-Tools finden Sie unter [Entwickeln von C#-Topologien mithilfe der HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Gehen Sie wie folgt vor, um ein Beispiel für den Storm-Cluster in HDInsight bereitzustellen und dann die Topologie anzuzeigen und zu verwalten.

1. Wenn Sie die neueste Version der HDInsight-Tools für Visual Studio noch nicht installiert haben, finden Sie Informationen dazu unter <a href="hdinsight-hadoop-visual-studio-tools-get-started.md" target="_blank">Erste Schritte mit den HDInsight-Tools für Visual Studio</a>.
2. Öffnen Sie Visual Studio, und wählen Sie **Datei** > **Neu** > **Projekt** aus.
3. Erweitern Sie im Dialogfeld **Neues Projekt** die Optionen **Installiert** > **Vorlagen**, und wählen Sie dann **HDInsight** aus. Wählen Sie in der Liste der Vorlagen die Option **Storm Sample**aus. Geben Sie unten im Dialogfeld einen Namen für die Anwendung ein.

    ![image](./media/hdinsight-storm-deploy-monitor-topology/sample.png)
4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **An Storm in HDInsight übermitteln** aus.

   > [!NOTE]
   > Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement ein. Wenn Sie über mehrere Abonnements verfügen, melden Sie sich bei dem Abonnement an, das Ihren Storm-Cluster in HDInsight enthält.
   >
   >
5. Wählen Sie in der Dropdownliste **Storm Cluster** Ihren Storm-Cluster in HDInsight und dann die Option **Übermitteln** aus. Sie können über das Fenster **Ausgabe** überwachen, ob die Übermittlung erfolgreich ausgeführt wurde.
6. Sobald die Topologie erfolgreich übermittelt wurde, sollten die **Storm-Topologien** für den Cluster angezeigt werden. Wählen Sie die Topologie aus der Liste aus, um Informationen zur aktiven Topologie anzuzeigen.

    ![Visual Studio Monitor](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > Sie können die **Storm-Topologien** auch im **Server-Explorer** anzeigen, indem Sie **Azure** > **HDInsight** erweitern und dann mit der rechten Maustaste auf einen Storm-Cluster in HDInsight klicken und **View Storm Topologies** auswählen.
   >
   >

    Wählen Sie die Form für die Spouts oder Bolts aus, um Informationen über diese Komponenten anzuzeigen. Für jedes ausgewählte Element wird ein neues Fenster geöffnet.

   > [!NOTE]
   > Der Name der Topologie ist der Klassenname der Topologie (in diesem Fall `HelloWord`) mit angefügtem Zeitstempel.
   >
   >
7. Wählen Sie in der Ansicht **Topology Summary** die Option **Kill** aus, um die Topologie zu beenden.

   > [!NOTE]
   > Storm-Topologien werden weiterhin ausgeführt, bis sie beendet werden oder der Cluster gelöscht wird.
   >
   >

## <a name="rest-api"></a>REST-API
Die Storm-Benutzeroberfläche baut auf der REST-API auf, sodass Sie mithilfe der REST-API ähnliche Verwaltungs- und Überwachungsfunktionen ausführen können. Mithilfe der REST-API können Sie benutzerdefinierte Tools zum Verwalten und Überwachen von Storm-Topologien erstellen.

Weitere Informationen finden Sie unter [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md)(in englischer Sprache). Die folgenden Informationen gelten für die Verwendung der REST-API mit Apache Storm in HDInsight.

### <a name="base-uri"></a>Basis-URI
Der Basis-URI für die REST-API in HDInsight-Clustern ist **https://&lt;Clustername>.azurehdinsight.net/stormui/api/v1/**, wobei **Clustername** den Namen des Storm-Clusters in HDInsight angibt.

### <a name="authentication"></a>Authentifizierung
Anforderungen an die REST-API müssen die **Standardauthentifizierung**und somit den Benutzernamen und das Kennwort des HDInsight-Clusteradministrators verwenden.

> [!NOTE]
> Da die Standardauthentifizierung unverschlüsselt gesendet wird, sollten Sie **immer** HTTPS verwenden, um die Kommunikation mit dem Cluster zu schützen.
>
>

### <a name="return-values"></a>Rückgabewerte
Von der REST-API zurückgegebene Informationen sind möglicherweise nur innerhalb des Clusters oder auf den virtuellen Computern in demselben Azure Virtual Network wie der Cluster verwendbar. Auf den für Zookeeper-Server zurückgegebenen vollqualifizierten Domänennamen (FQDN) kann z. B. nicht über das Internet zugegriffen werden.

## <a name="next-steps"></a>Nächste Schritte
Nun, da Sie gelernt haben, wie man Topologien mithilfe des Storm-Dashboards bereitstellt und überwacht, können Sie folgende Inhalte erlernen:

* [Entwickeln von C#-Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Entwickeln von Java-basierten Topologien mit Maven](hdinsight-storm-develop-java-topology.md)

Eine Liste weiterer Beispieltopologien finden Sie unter [Beispieltopologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png



<!--HONumber=Nov16_HO3-->


