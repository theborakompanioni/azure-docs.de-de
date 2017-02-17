---
title: Visualisieren des Clusters mit Service Fabric Explorer | Microsoft Docs
description: Service Fabric Explorer ist ein webbasiertes Tool zum Untersuchen und Verwalten von Cloudanwendungen und Knoten in einem Microsoft Azure Service Fabric-Cluster.
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: dafaf29b6827a6f1c043af3d6bfe62d480d31ad5
ms.openlocfilehash: bebfd5e0cc209d8c1fc28d2300c57e3519410954


---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualisieren Ihres Clusters mit Service Fabric Explorer
Service Fabric Explorer ist ein webbasiertes Tool zum Untersuchen und Verwalten von Anwendungen und Knoten in einem Azure Service Fabric-Cluster. Service Fabric Explorer wird direkt innerhalb des Clusters gehostet und ist daher immer verfügbar – unabhängig davon, wo der Cluster ausgeführt wird.

## <a name="video-tutorial"></a>Videotutorial

Im folgenden Microsoft Virtual Academy-Video erfahren Sie, wie Sie den Service Fabric Explorer verwenden:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a>Verbinden mit Service Fabric Explorer
Wenn Sie die Anweisungen zum [Vorbereiten Ihrer Entwicklungsumgebung](service-fabric-get-started.md) befolgt haben, können Sie Service Fabric Explorer im lokalen Cluster starten, indem Sie zu „http://localhost:19080/Explorer“ navigieren.

> [!NOTE]
> Wenn Sie Internet Explorer mit Service Fabric Explorer zum Verwalten eines Remoteclusters verwenden, müssen Sie einige Internet Explorer-Einstellungen konfigurieren. Um sicherzustellen, dass alle Informationen richtig geladen werden, wechseln Sie zu **Extras** > **Einstellungen der Kompatibilitätsansicht**, und deaktivieren Sie **Intranetsites in Kompatibilitätsansicht anzeigen**.
>
>

## <a name="understand-the-service-fabric-explorer-layout"></a>Grundlegendes zum Layout von Service Fabric Explorer
Sie können in Service Fabric Explorer navigieren, indem Sie die Strukturansicht auf der linken Seite verwenden. Auf der Stammebene der Struktur bietet das Clusterdashboard eine Clusterübersicht, einschließlich einer Zusammenfassung der Anwendungs- und Knotenintegrität.

![Service Fabric Explorer-Clusterdashboard][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Anzeigen des Cluster-Layouts
Knoten in einem Service Fabric-Cluster werden in einem zweidimensionalen Raster aus Fehlerdomänen und Upgradedomänen angeordnet. Durch diese Anordnung wird dafür gesorgt, dass Ihre Anwendungen auch bei Hardwarefehlern und Anwendungsupgrades verfügbar bleiben. Mit der Clusterzuweisung können Sie anzeigen, wie der aktuelle Cluster angeordnet ist.

![Service Fabric Explorer-Clusterzuweisung][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Anzeigen von Anwendungen und Diensten
Der Cluster enthält zwei Unterstrukturen: eine für Anwendungen und eine für Knoten.

Sie können die Anwendungsansicht für die Navigation durch die logische Hierarchie von Service Fabric verwenden: Anwendungen, Dienste, Partitionen und Replikate.

Im Beispiel unten besteht die Anwendung **MyApp** aus zwei Diensten: **MyStatefulService** und **WebService**. Da **MyStatefulService** zustandsbehaftet ist, enthält er eine Partition mit einem primären und zwei sekundären Replikaten. Im Gegensatz dazu ist „WebSvcService“ zustandslos und enthält eine einzelne Instanz.

![Service Fabric Explorer-Anwendungsansicht][sfx-application-tree]

Auf jeder Ebene der Struktur werden im Hauptbereich relevante Informationen zum Element angezeigt. Beispielsweise werden der Integritätsstatus und die Version für einen bestimmten Dienst angezeigt.

![Service Fabric Explorer-Bereich für essentielle Informationen][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Anzeigen der Knoten des Clusters
Die Knotenansicht zeigt das physische Layout des Clusters. Für einen Knoten können Sie überprüfen, für welche Anwendungen Code auf dem Knoten bereitgestellt wurde. Genauer gesagt können Sie anzeigen, welche Replikate derzeit darauf ausgeführt werden.

## <a name="actions"></a>Actions
Service Fabric Explorer bietet eine schnelle Möglichkeit zum Aufrufen von Aktionen für die Knoten, Anwendungen und Dienste in Ihrem Cluster.

Um beispielsweise eine Anwendungsinstanz zu löschen, wählen Sie die Anwendung in der Struktur auf der linken Seite und dann **Aktionen** > **Anwendung löschen** aus.

![Löschen einer Anwendung in Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> Sie können die gleichen Aktionen ausführen, indem Sie auf die Auslassungspunkte neben jedem Element klicken.
>
>

In der folgenden Tabelle sind die für jede Entität verfügbaren Aktionen aufgeführt:

| **Entität** | **Aktion** | **Beschreibung** |
| --- | --- | --- |
| Anwendungstyp |Bereitstellung des Typs aufheben |Entfernt das Anwendungspaket aus dem Imagespeicher des Clusters. Dabei müssen zunächst alle Anwendungen dieses Typs entfernt werden. |
| Anwendung |Anwendung löschen |Löscht die Anwendung, einschließlich aller Dienste und zugehörigen Zustände (sofern vorhanden). |
| Dienst |Dienst löschen |Löscht den Dienst und seinen Zustand (sofern vorhanden). |
| Knoten |Aktivieren |Aktiviert den Knoten. |
| Deaktivieren (anhalten) |Hält den Knoten im aktuellen Zustand an. Die Dienste werden weiterhin ausgeführt, Service Fabric verschiebt jedoch nur dann Elemente proaktiv in oder aus einem Dienst, wenn dies zur Vermeidung von Ausfällen oder Dateninkonsistenzen erforderlich ist. Diese Aktion wird normalerweise zum Aktivieren von Debuggingdiensten auf einem bestimmten Knoten verwendet, um sicherzustellen, dass sie während der Überprüfung nicht verschoben werden. | |
| Deaktivieren (neu starten) |Verschiebt alle In-Memory-Dienste sicher aus einem Knoten und schließt dauerhafte Dienste. Wird in der Regel verwendet, wenn die Hostprozesse oder Hostcomputer neu gestartet werden müssen. | |
| Deaktivieren (Daten entfernen) |Ermöglicht das sichere Schließen aller auf dem Knoten ausgeführten Dienste, nachdem ausreichend Ersatzreplikate erstellt wurden. Wird in der Regel verwendet, wenn ein Knoten (oder zumindest sein Speicher) dauerhaft außer Betrieb genommen wird. | |
| Knotenzustand entfernen |Entfernt die Replikatinformationen eines Knotens aus dem Cluster. Wird in der Regel verwendet, wenn ein Knoten, auf dem bereits ein Fehler aufgetreten ist, als nicht mehr wiederherstellbar gilt. | |
| Knoten | Neu starten | Simuliert einen Knotenausfall durch einen Neustart des Knotens. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/en-us/powershell/servicefabric/vlatest/Restart-ServiceFabricNode). ||

Da viele Aktionen destruktiv sind, werden Sie aufgefordert, den Vorgang zu bestätigen, bevor die Aktion abgeschlossen wird.

> [!TIP]
> Jede Aktion, die mit Service Fabric Explorer ausgeführt werden kann, kann auch mithilfe von PowerShell oder einer REST-API ausgeführt werden, um die Automatisierung zu ermöglichen.
>
>

Sie können Service Fabric Explorer auch verwenden, um Anwendungsinstanzen für einen bestimmten Anwendungstyp und eine bestimmte Anwendungsversion zu erstellen. Wählen Sie in der Strukturansicht den gewünschten Anwendungstyp aus, und klicken Sie dann neben der im rechten Bereich angezeigten gewünschten Version auf den Link **App-Instanz erstellen** .

![Erstellen einer Anwendung in Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Anwendungsinstanzen, die über Service Fabric Explorer erstellt wurden, können zurzeit nicht parametrisiert werden. Sie werden mit standardmäßigen Parameterwerten erstellt.
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a>Herstellen einer Verbindung mit einem Service Fabric-Remotecluster
Wenn Sie den Clusterendpunkt kennen und über ausreichende Berechtigungen verfügen, können Sie über einen beliebigen Browser auf Service Fabric Explorer zugreifen. Service Fabric Explorer ist nur ein weiterer Dienst, der im Cluster ausgeführt wird.

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Ermitteln des Service Fabric Explorer-Endpunkts für einen Remotecluster
Um Service Fabric Explorer für einen bestimmten Cluster zu erreichen, navigieren Sie in Ihrem Browser zu:

http://&lt;Ihr_Clusterendpunkt&gt;:19080/Explorer

Bei Azure-Clustern finden Sie die vollständige URL auch im Zusammenfassungsbereich für den Cluster im Azure-Portal.

### <a name="connect-to-a-secure-cluster"></a>Herstellen einer Verbindung mit einem sicheren Cluster
Sie können den Clientzugriff auf Ihren Service Fabric-Cluster entweder mit Zertifikaten oder über Azure Active Directory (AAD) steuern.

Wenn Sie versuchen, eine Verbindung mit Service Fabric Explorer in einem sicheren Cluster herzustellen, müssen Sie je nach Konfiguration des Clusters entweder ein Clientzertifikat bereitstellen oder sich mithilfe von AAD anmelden.

## <a name="next-steps"></a>Nächste Schritte
* [Testability – Übersicht](service-fabric-testability-overview.md)
* [Verwalten von Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric-Anwendungsbereitstellung per PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png



<!--HONumber=Feb17_HO3-->


