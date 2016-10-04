<properties
   pageTitle="Erstellen Ihrer ersten Service Fabric-Anwendung in Visual Studio | Microsoft Azure"
   description="Erstellen, Bereitstellen und Debuggen einer Service Fabric-Anwendung in Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="ryanwi"/>


# Erstellen Ihrer ersten Azure Service Fabric-Anwendung

> [AZURE.SELECTOR]
- [C Sharp](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java](service-fabric-create-your-first-linux-application-with-java.md)

Das Service Fabric-SDK enthält ein Add-In für Visual Studio, das Vorlagen und Tools zum Erstellen, Bereitstellen und Debuggen von Service Fabric-Anwendungen bietet. In diesem Thema werden Sie durch die Erstellung Ihrer ersten Anwendung in Visual Studio geführt.

## Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass [Ihre Entwicklungsumgebung eingerichtet ist](service-fabric-get-started.md).

## Exemplarische Vorgehensweise per Video

Im folgenden Video werden die Schritte in diesem Lernprogramm behandelt:

>[AZURE.VIDEO creating-your-first-service-fabric-application-in-visual-studio]

## Erstellen der Anwendung

Eine Service Fabric-Anwendung kann einen oder mehrere Dienste enthalten, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Mit dem Assistenten für neue Projekte können Sie mit Ihrem ersten Dienstprojekt ein Anwendungsprojekt erstellen. Sie können später weitere Dienste hinzufügen.

1. Starten Sie Visual Studio als Administrator.

2. Klicken Sie auf **Datei > Neues Projekt > Cloud > Service Fabric-Anwendung**.

3. Geben Sie der Anwendung einen Namen, und klicken Sie auf **OK**.

	![Dialogfeld „Neues Projekt“ in Visual Studio][1]

4. Wählen Sie auf der nächsten Seite die Option **Zustandsbehaftet** als ersten Diensttyp zur Einbindung in die Anwendung aus. Geben Sie ihm einen Namen, und klicken Sie auf **OK**.

	![Dialogfeld „Neuer Dienst“ in Visual Studio][2]

	>[AZURE.NOTE] Weitere Informationen zu den Optionen finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md).

	Visual Studio erstellt das Anwendungsprojekt und das Projekt für den zustandsbehafteten Dienst und zeigt sie im Projektmappen-Explorer an.

	![Projektmappen-Explorer nach der Erstellung der Anwendung mit einem zustandsbehafteten Dienst][3]

	Das Anwendungsprojekt enthält Code nicht direkt. Stattdessen verweist es eine Reihe von Dienstprojekten. Darüber hinaus enthält es drei Arten von Inhalt:

	- **Veröffentlichungsprofile**: Werden zum Verwalten von Tooleinstellungen für unterschiedliche Umgebungen verwendet.

	- **Skripts**: Umfasst ein PowerShell-Skript für Bereitstellung/Upgrade der Anwendung. Das Skript wird von Visual Studio im Hintergrund verwendet. Es kann auch direkt an der Befehlszeile aufgerufen werden.

	- **Anwendungsdefinition**: Enthält das Anwendungsmanifest unter *ApplicationPackageRoot*. Die zugehörigen Anwendungsparameterdateien befinden sich unter *ApplicationParameters*. Sie definieren die Anwendung und ermöglichen es Ihnen, diese speziell für eine bestimmte Umgebung zu konfigurieren.

    Eine Übersicht über den Inhalt des Dienstprojekts finden Sie unter [Erste Schritte mit Reliable Services](service-fabric-reliable-services-quick-start.md).

## Bereitstellen und Debuggen der Anwendung

Da jetzt eine Anwendung vorhanden ist, können Sie versuchen, sie auszuführen.

1. Drücken Sie in Visual Studio F5, um die Anwendung für das Debuggen bereitzustellen.

	>[AZURE.NOTE] Die Bereitstellung dauert beim ersten Mal eine Weile, da Visual Studio einen lokalen Cluster für die Entwicklung erstellt. Ein lokaler Cluster führt den gleichen Plattformcode aus, den Sie in einem Cluster mit mehreren Computern erstellen, aber auf einem einzelnen Computer. Der Status der Clustererstellung wird im Ausgabefenster von Visual Studio angezeigt.

	Wenn der Cluster bereit ist, erhalten Sie eine Benachrichtigung von der Taskleistenanwendung Local Cluster Manager, die im SDK enthalten ist.

	![Benachrichtigung in der Taskleiste zum lokalen Cluster][4]

2. Sobald die Anwendung gestartet wird, startet Visual Studio automatisch die Diagnoseereignisanzeige, in der die Ausgabe der Ablaufverfolgung des Diensts angezeigt wird.

	![Diagnoseereignisanzeige][5]

	Bei der Vorlage für einen zustandsbehafteten Dienst zeigen die Meldungen einfach die Erhöhung des Zählerwerts in der `RunAsync`-Methode von „MyStatefulService.cs“ an.

3. Erweitern Sie eines der Ereignisse, um weitere Details anzuzeigen, einschließlich des Knotens, in dem der Code ausgeführt wird. In diesem Fall ist es „_Node_2“, aber dies kann auf Ihrem Computer abweichen.

	![Detail der Diagnoseereignisanzeige][6]

	Der lokale Cluster enthält fünf Knoten, die auf einem einzelnen Computer gehostet werden. Er imitiert einen Cluster mit fünf Knoten auf unterschiedlichen Computern. Deaktivieren Sie einen der Knoten im lokalen Cluster, um den Verlust eines Computers zu simulieren, und führen Sie gleichzeitig den Visual Studio-Debugger aus.

    >[AZURE.NOTE] Von der Projektvorlage ausgegebene Anwendungsdiagnoseereignisse verwenden die `ServiceEventSource`-Klasse. Weitere Informationen finden Sie unter [Lokales Überwachen und Diagnostizieren von Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

4. Suchen Sie in Ihrem Dienstprojekt die Klasse, die von StatefulService abgeleitet ist (z.B. MyStatefulService), und legen Sie in der ersten Zeile der `RunAsync`-Methode einen Haltepunkt fest.

	![Haltepunkt in der RunAsync-Methode des zustandsbehafteten Diensts][7]

5. Klicken Sie mit der rechten Maustaste auf die Taskleisten-App Local Cluster Manager, und wählen Sie die Option zum **Verwalten des lokalen Clusters** aus, um den Service Fabric Explorer zu starten.

    ![Starten des Service Fabric Explorers über den Manager für den lokalen Cluster][systray-launch-sfx]

    Der Service Fabric Explorer bietet eine visuelle Darstellung eines Clusters, einschließlich der darin bereitgestellten Anwendungen und der physischen Knoten, aus denen er besteht. Weitere Informationen zu Service Fabric Explorer finden Sie unter [Visualisieren des Clusters](service-fabric-visualizing-your-cluster.md).

6. Erweitern Sie im linken Bereich **Cluster > Knoten**, und suchen Sie den Knoten, auf dem der Code ausgeführt wird.

7. Klicken Sie auf **Aktionen > Deaktivieren (Neustarten)**, um einen Neustart des Computers zu simulieren. (Beachten Sie, dass Sie das Deaktivieren auch über das Kontextmenü in der Listenansicht eines Knotens im linken Bereich durchführen können.)

	![Beenden eines Knotens im Service Fabric Explorer][sfx-stop-node]

	Sofort sollten Sie den Haltepunkt in Visual Studio sehen, während die Berechnung, die Sie auf einem Knoten durchgeführt haben, nahtlos auf einem anderen fortgeführt wird.

8. Kehren Sie zur Diagnoseereignisanzeige zurück, und beobachten Sie die Meldungen. Beachten Sie, dass der Zähler weiter erhöht wurde, obwohl die Ereignisse tatsächlich von einem anderen Knoten stammen.

    ![Diagnoseereignisanzeige nach einem Failover][diagnostic-events-viewer-detail-post-failover]

## Wechseln des Clustermodus

Standardmäßig ist der lokale Entwicklungscluster als Cluster mit fünf Knoten konfiguriert. Dies ist zum Debuggen von Diensten nützlich, die auf mehreren Knoten bereitgestellt werden. Das Bereitstellen einer Anwendung im Entwicklungscluster mit fünf Knoten kann aber einige Zeit dauern. Wenn Sie möchten, dass Codeänderungen schnell durchlaufen werden, ohne dass die App auf fünf Knoten ausgeführt wird, können Sie für den Entwicklungscluster die Umstellung auf den Modus mit nur einem Knoten durchführen. Klicken Sie zum Ausführen Ihres Codes in einem Cluster mit nur einem Knoten in der Taskleiste mit der rechten Maustaste auf den Manager für den lokalen Cluster, und wählen Sie **Switch Cluster Mode > 1 Node** (Clustermodus ändern > 1 Knoten).

![Wechseln des Clustermodus][switch-cluster-mode]

Wenn Sie den Clustermodus ändern, wird der Entwicklungscluster zurückgesetzt, und alle Anwendungen, die bereitgestellt oder im Cluster ausgeführt werden, werden entfernt.

## Bereinigen

  Bevor Sie Ihre Arbeit abschließen, sollten Sie bedenken, dass der lokale Cluster sehr real ist. Wenn Sie den Debugger beenden, wird Ihre Anwendungsinstanz entfernt, und die Registrierung des Anwendungstyps wird aufgehoben. Der Cluster wird im Hintergrund aber weiter ausgeführt. Sie haben mehrere Möglichkeiten, den Cluster zu verwalten:

  1. Zum Beenden des Clusters bei Beibehaltung der Anwendungsdaten und Ablaufverfolgungen klicken Sie in der Infobereichs-App auf **Lokalen Cluster beenden**.

  2. Zum vollständigen Entfernen des Clusters klicken Sie in der Infobereichs-App auf **Lokalen Cluster entfernen**. Beachten Sie, dass diese Option zu einer weiteren langsamen Bereitstellung führt, wenn Sie das nächste Mal in Visual Studio F5 drücken. Löschen Sie den Cluster nur, wenn Sie nicht beabsichtigen, den lokalen Cluster einige Zeit zu verwenden, oder wenn Sie Ressourcen freigeben müssen.

## Nächste Schritte

- Informieren Sie sich, wie Sie einen [Cluster in Azure](service-fabric-cluster-creation-via-portal.md) oder einen [eigenständigen Cluster in Windows](service-fabric-cluster-creation-for-windows-server.md) erstellen.
- Versuchen Sie, einen Dienst zu erstellen, indem Sie das Programmiermodell [Reliable Services](service-fabric-reliable-services-quick-start.md) oder [Reliable Actors](service-fabric-reliable-actors-get-started.md) verwenden.
- Erfahren Sie, wie Sie Ihre Dienste über ein [Webdienst-Front-End](service-fabric-add-a-web-frontend.md) im Internet verfügbar machen können.
- Machen Sie eine [praktische Übung](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx), und erstellen Sie einen zustandslosen Dienst, konfigurieren sie Überwachungs- und Integritätsberichte, und führen Sie ein Anwendungsupgrade durch.

<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png

<!---HONumber=AcomDC_0928_2016-->