---
title: "Erstellen eines zuverlässigen Azure Service Fabric-Diensts mit C#"
description: Es wird beschrieben, wie Sie eine Reliable Services-Anwendung, die auf Azure Service Fabric basiert, mit Visual Studio erstellen, bereitstellen und debuggen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: f93298e6483fd8c9dfda835964aeebd1a430af69
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Erstellen Ihrer ersten Anwendung für einen zustandsbehafteten zuverlässigen Service Fabric-Dienst mit C#

Es wird beschrieben, wie Sie in wenigen Minuten Ihre erste Service Fabric-Anwendung für .NET unter Windows bereitstellen. Nach der Ausführung der Schritte verfügen Sie über einen lokalen Cluster mit einer zuverlässigen Dienstanwendung.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass [Ihre Entwicklungsumgebung eingerichtet ist](service-fabric-get-started.md). Dies umfasst auch das Installieren des Service Fabric SDK und von Visual Studio 2017 oder 2015.

## <a name="create-the-application"></a>Erstellen der Anwendung

Starten Sie Visual Studio als **Administrator**.

Erstellen Sie mit `CTRL`+`SHIFT`+`N` ein Projekt.

Wählen Sie im Dialogfeld **Neues Projekt** die Option **Cloud > Service Fabric-Anwendung**.

Geben Sie der Anwendung den Namen **MyApplication**, und klicken Sie auf **OK**.

   
![Dialogfeld „Neues Projekt“ in Visual Studio][1]

Mit dem nächsten Dialogfeld können Sie eine beliebige Art von Service Fabric-Anwendung erstellen. Wählen Sie für diesen Schnellstart die Option **Zustandsbehafteter Dienst**.

Geben Sie dem Dienst den Namen **MyStatefulService**, und klicken Sie auf **OK**.

![Dialogfeld „Neuer Dienst“ in Visual Studio][2]


Visual Studio erstellt das Anwendungsprojekt und das Projekt für den zustandsbehafteten Dienst und zeigt sie im Projektmappen-Explorer an.

![Projektmappen-Explorer nach der Erstellung der Anwendung mit einem zustandsbehafteten Dienst][3]

Das Anwendungsprojekt (**MyApplication**) enthält den Code nicht direkt. Stattdessen verweist es eine Reihe von Dienstprojekten. Darüber hinaus enthält es drei Arten von Inhalt:

* **Veröffentlichungsprofile**  
Dies sind Profile für die Bereitstellung in anderen Umgebungen.

* **Skripts**  
Ein PowerShell-Skript für die Bereitstellung bzw. Aktualisierung der Anwendung.

* **Anwendungsdefinition**  
Enthält die Datei „ApplicationManifest.xml“ unter *ApplicationPackageRoot*, in der die Zusammensetzung Ihrer Anwendung beschrieben ist. Die zugeordneten Anwendungsparameterdateien befinden sich unter *ApplicationParameters* und können zum Angeben von umgebungsspezifischen Parametern verwendet werden. In Visual Studio wird eine Anwendungsparameterdatei ausgewählt, die während der Bereitstellung in einer bestimmten Umgebung im zugeordneten Veröffentlichungsprofil angegeben wird.
    
Eine Übersicht über den Inhalt des Dienstprojekts finden Sie unter [Erste Schritte mit Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Bereitstellen und Debuggen der Anwendung

Nachdem Sie nun über eine Anwendung verfügen, können Sie sie ausführen.

Drücken Sie in Visual Studio `F5`, um die Anwendung für das Debuggen bereitzustellen.

>[!NOTE]
>Wenn Sie die Anwendung zum ersten Mal lokal ausführen und bereitstellen, erstellt Visual Studio einen lokalen Cluster für das Debuggen. Dies kann einige Zeit dauern. Der Status der Clustererstellung wird im Ausgabefenster von Visual Studio angezeigt.

Wenn der Cluster bereit ist, erhalten Sie eine Benachrichtigung von der Taskleistenanwendung Local Cluster Manager, die im SDK enthalten ist.
   
![Benachrichtigung in der Taskleiste zum lokalen Cluster][4]

Sobald die Anwendung gestartet wird, startet Visual Studio automatisch die **Diagnoseereignisanzeige**, in der die Ausgabe der Ablaufverfolgung Ihrer Dienste angezeigt wird.
   
![Diagnoseereignisanzeige][5]

In der verwendeten Vorlage für den zustandsbehafteten Dienst wird einfach ein Zählerwert angezeigt, der in der `RunAsync`-Methode von **MyStatefulService.cs** inkrementiert wird.

Erweitern Sie eines der Ereignisse, um weitere Details anzuzeigen, einschließlich des Knotens, in dem der Code ausgeführt wird. In diesem Fall ist es „\_Node\_2“, aber dies kann auf Ihrem Computer ggf. anders sein.
   
![Detail der Diagnoseereignisanzeige][6]

Der lokale Cluster enthält fünf Knoten, die auf einem einzelnen Computer gehostet werden. In einer Produktionsumgebung wird jeder Knoten auf einem eigenen physischen oder virtuellen Computer gehostet. Deaktivieren Sie einen der Knoten im lokalen Cluster, um den Verlust eines Computers zu simulieren, und führen Sie gleichzeitig den Visual Studio-Debugger aus.

Öffnen Sie im **Projektmappen-Explorer**-Fenster die Datei **MyStatefulService.cs**. 

Suchen Sie nach der `RunAsync`-Methode, und legen Sie für die erste Zeile der Methode einen Haltepunkt fest.

![Haltepunkt in der RunAsync-Methode des zustandsbehafteten Diensts][7]

Starten Sie das Tool **Service Fabric Explorer**, indem Sie mit der rechten Maustaste auf die Infobereich-Anwendung **Local Cluster Manager** (Manager für lokale Cluster) klicken und **Manage Local Cluster** (Lokalen Cluster verwalten) wählen.

![Starten des Service Fabric Explorers über den Manager für den lokalen Cluster][systray-launch-sfx]

Im [**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) kann eine visuelle Darstellung eines Clusters angezeigt werden. Sie enthält die darin bereitgestellten Anwendungen und die Gruppe der physischen Knoten des Clusters.

Erweitern Sie im linken Bereich **Cluster > Knoten**, und suchen Sie den Knoten, auf dem der Code ausgeführt wird.

Klicken Sie auf **Aktionen > Deaktivieren (Neustarten)**, um einen Neustart des Computers zu simulieren.

![Beenden eines Knotens im Service Fabric Explorer][sfx-stop-node]

Sofort sollten Sie den Haltepunkt in Visual Studio sehen, während die Berechnung, die Sie auf einem Knoten durchgeführt haben, nahtlos auf einem anderen fortgeführt wird.


Kehren Sie als Nächstes zur Diagnoseereignisanzeige zurück, und beobachten Sie die Meldungen. Der Zähler wurde weiter erhöht, obwohl die Ereignisse tatsächlich von einem anderen Knoten stammen.

![Diagnoseereignisanzeige nach einem Failover][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a>Bereinigen des lokalen Clusters (optional)

Bedenken Sie, dass dies ein echter lokaler Cluster ist. Wenn Sie den Debugger beenden, wird Ihre Anwendungsinstanz entfernt, und die Registrierung des Anwendungstyps wird aufgehoben. Der Cluster wird im Hintergrund aber weiter ausgeführt. Wenn Sie zum Beenden des lokalen Clusters bereit sind, können Sie zwischen zwei Optionen wählen.

### <a name="keep-application-and-trace-data"></a>Beibehalten der Anwendungs- und Ablaufverfolgungsdaten

Fahren Sie den Cluster herunter, indem Sie mit der rechten Maustaste auf die Infobereich-Anwendung **Local Cluster Manager** (Manager für lokalen Cluster) klicken und **Stop Local Cluster** (Lokalen Cluster beenden) wählen.

### <a name="delete-the-cluster-and-all-data"></a>Löschen des Clusters und aller Daten

Entfernen Sie den Cluster, indem Sie mit der rechten Maustaste auf die Anwendung **Local Cluster Manager** (Manager für lokalen Cluster) im Infobereich klicken und **Remove Local Cluster** (Lokalen Cluster entfernen) wählen. 

Wenn Sie diese Option wählen, wird der Cluster bei der nächsten Ausführung der Anwendung von Visual Studio erneut bereitgestellt. Wählen Sie diese Option, wenn Sie beabsichtigen, den lokalen Cluster erst einmal nicht zu verwenden, oder wenn Sie Ressourcen freigeben müssen.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich ausführlicher über [Reliable Services](service-fabric-reliable-services-introduction.md).
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
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png

