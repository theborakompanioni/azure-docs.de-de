---
title: "Nächste Schritte bei der Service Fabric-Projekterstellung | Microsoft Docs"
description: "Dieser Artikel enthält Links zu einer Reihe wichtiger Entwicklungsaufgaben für Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: rwike77
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 8208a1a41388a8cc36f3702bd0cad2bb82e16403
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017


---
# <a name="your-service-fabric-application-and-next-steps"></a>Ihre Service Fabric-Anwendung und nächste Schritte
Ihre Azure Service Fabric-Anwendung wurde erstellt. Dieser Artikel beschreibt die Zusammensetzung des Projekts und einige mögliche nächste Schritte.

## <a name="your-application"></a>Ihre Anwendung
Jede neue Anwendung umfasst ein Anwendungsprojekt. Es gibt möglicherweise je nach gewähltem Diensttyp ein oder zwei zusätzliche Projekte.

### <a name="the-application-project"></a>Das Anwendungsprojekt
Das Anwendungsprojekt besteht aus Folgendem:

* Ein Satz von Verweisen auf die Dienste, aus denen die Anwendung besteht.
* Drei Veröffentlichungsprofile („1-Node Local“, „5-Node Local“ und „Cloud“), die Sie zum Verwalten der Voreinstellungen für die Arbeit mit unterschiedlichen Umgebungen verwenden können – z.B. für den Clusterendpunkt und eine Einstellung, ob standardmäßig Upgradebereitstellungen ausgeführt werden sollen.
* Drei Anwendungsparameterdateien (wie oben), mit denen Sie umgebungsspezifische Anwendungskonfigurationen verwalten können, wie z.B. die Anzahl der Partitionen, die für einen Dienst erstellt werden sollen.
* einem Bereitstellungsskript, mit dem Sie Ihre Anwendung über die Befehlszeile oder als Teil einer automatisierten fortlaufenden Integrations- und Bereitstellungspipeline bereitstellen können
* das Anwendungsmanifest, das die Anwendung beschreibt Das Manifest finden Sie im Ordner „ApplicationPackageRoot“.

### <a name="stateless-service"></a>Zustandsloser Dienst
Wenn Sie einen neuen zustandslosen Dienst hinzufügen, fügt Visual Studio Ihrer Lösung ein Dienstprojekt hinzu, das einen von `StatelessService`abstammenden Typ enthält. Der Dienst erhöht eine lokale Variable im Zähler schrittweise.

### <a name="stateful-service"></a>Zustandsbehafteter Dienst
Wenn Sie einen neuen zustandsbehafteten Dienst hinzufügen, fügt Visual Studio Ihrer Lösung ein Dienstprojekt hinzu, das einen von `StatefulService`abstammenden Typ enthält. Der Dienst erhöht einen Zähler schrittweise in seiner `RunAsync`-Methode und speichert das Ergebnis in einem `ReliableDictionary`.

### <a name="actor-service"></a>Actordienst
Wenn Sie einen neuen Reliable Actor hinzufügen, fügt Visual Studio zwei Projekte zur Projektmappe hinzu: ein Akteurprojekt und ein Schnittstellenprojekt.

Das Actor-Projekt bietet Methoden zum Einrichten und Abrufen des Werts eines Zählers, der innerhalb des Zustands des Actors zuverlässig persistent ist. Das Projekt enthält eine Schnittstelle, die andere Dienste zum Aufrufen des Akteurs nutzen können.

### <a name="stateless-web-api"></a>Zustandslose Web-API
Das zustandslose Web-API-Projekt bietet einen einfachen Webdienst, den Sie zum Öffnen Ihrer Anwendung für externe Clients verwenden können. Weitere Informationen dazu, wie das Projekt aufgebaut ist, finden Sie unter [Erste Schritte: Web-API-Dienste von Service Fabric mit selbstgehostetem OWIN](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>ASP.NET Core
Das Service Fabric SDK umfasst die gleichen ASP.NET Core-Vorlagen, die auch für eigenständige ASP.NET Core-Projekte verfügbar sind: leere Vorlage, [Web-API][aspnet-webapi] und [Webanwendung][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Ausführbare Gastanwendungsdateien und Gastcontainer

Bei einer Service Fabric-Gastanwendung handelt es sich um einen Dienst, der nicht mit den Programmiermodellen der Plattform erstellt wird. Sie können die Binärdateien für eine Gastanwendung entweder [direkt im Anwendungspaket](service-fabric-deploy-existing-app.md) oder [über ein Containerimage](service-fabric-deploy-container.md) packen. In beiden Fällen werden in Visual Studio die erforderlichen Artefakte im Ordner **ApplicationPackageRoot** des Anwendungsprojekts erstellt. Ein neues Dienstprojekt wird in Visual Studio nicht erstellt, da der Code bereits an anderer Stelle vorhanden ist. Wenn Sie Ihre Gastprojekte zusammen mit dem Service Fabric-Anwendungsprojekt verwalten möchten, können Sie sie der gleichen Visual Studio-Projektmappe hinzufügen.

## <a name="next-steps"></a>Nächste Schritte
### <a name="create-an-azure-cluster"></a>Erstellen eines Azure-Clusters
Das Service Fabric-SDK stellt einen lokalen Cluster zu Entwicklungs- und Testzwecken bereit. Informationen zum Erstellen eines Clusters in Azure finden Sie unter [Einrichten eines Service Fabric-Clusters im Azure-Portal][create-cluster-in-portal].

### <a name="publish-your-application-to-azure"></a>Veröffentlichen der Anwendung in Azure
Sie können Ihre Anwendung direkt aus Visual Studio in einem Azure-Cluster veröffentlichen. Informationen dazu finden Sie unter [Veröffentlichen Ihrer Anwendung in Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Visualisieren des Clusters mit Service Fabric Explorer
Service Fabric Explorer bietet eine einfache Möglichkeit zum Visualisieren Ihres Clusters, einschließlich der bereitgestellten Anwendungen und des physischen Layouts. Informationen dazu finden Sie unter [Visualisieren Ihres Clusters mit Service Fabric Explorer][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Versionierung und Upgrade Ihrer Dienste
Service Fabric ermöglicht eine unabhängige Versionierung und Upgrades von unabhängigen Diensten in einer Anwendung. Weitere Informationen finden Sie unter [Versionierung und Upgrade Ihrer Dienste][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfigurieren der Continuous Integration in Visual Studio Team Services
Informationen zur Einrichtung eines Continuous Integration-Prozesses für Ihre Service Fabric-Anwendung finden Sie unter [Konfigurieren der Continuous Integration in Visual Studio Team Services][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html

