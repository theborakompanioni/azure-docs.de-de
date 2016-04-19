<properties
   pageTitle="Nächste Schritte bei der Service Fabric-Projekterstellung | Microsoft Azure"
   description="Dieser Artikel enthält Links zu einer Reihe wichtiger Entwicklungsaufgaben für Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/27/2016"
   ms.author="seanmck"/>

# Ihre Service Fabric-Anwendung und nächste Schritte
Ihre Azure Service Fabric-Anwendung wurde erstellt. Dieser Artikel beschreibt die Zusammensetzung des Projekts und einige mögliche nächste Schritte.

## Ihre Anwendung
Jede neue Anwendung umfasst ein Anwendungsprojekt. Es gibt möglicherweise je nach gewähltem Diensttyp ein oder zwei zusätzliche Projekte.

### Das Anwendungsprojekt
Das Anwendungsprojekt besteht aus:

- einem Satz von Verweisen auf die Dienste, aus denen die Anwendung besteht

- zwei Veröffentlichungsprofilen (Lokal und Cloud), die Sie zum Verwalten der Voreinstellungen für die Arbeit mit unterschiedlichen Umgebungen verwenden können – z. B. als Clusterendpunkt und ob standardmäßig Upgradebereitstellungen ausgeführt werden sollen

- zwei Anwendungsparameterdateien (Lokal und Cloud), mit denen Sie umgebungsspezifische Anwendungskonfigurationen verwalten können, wie z. B. die Anzahl der Partitionen, die für einen Dienst erstellt werden sollen

- einem Bereitstellungsskript, mit dem Sie Ihre Anwendung über die Befehlszeile oder als Teil einer automatisierten fortlaufenden Integrations- und Bereitstellungspipeline bereitstellen können

- das Anwendungsmanifest, das die Anwendung beschreibt Das Manifest finden Sie im Ordner „ApplicationPackageRoot“.

### Zustandsloser Dienst
Wenn Sie einen neuen zustandslosen Dienst hinzufügen, fügt Visual Studio Ihrer Lösung ein Dienstprojekt hinzu, das einen von `StatelessService` abstammenden Typ enthält. Der Dienst erhöht eine lokale Variable im Zähler schrittweise.

### Zustandsbehafteter Dienst
Wenn Sie einen neuen zustandsbehafteten Dienst hinzufügen, fügt Visual Studio Ihrer Lösung ein Dienstprojekt hinzu, das einen von `StatefulService` abstammenden Typ enthält. Der Dienst erhöht einen Zähler schrittweise in seiner `RunAsync`-Methode und speichert das Ergebnis in einem `ReliableDictionary`.

### Actordienst
Wenn Sie einen neuen Reliable Actor hinzufügen, fügt Visual Studio zwei Projekte zur Projektmappe hinzu: ein Akteurprojekt und ein Schnittstellenprojekt.

Das Actor-Projekt bietet Methoden zum Einrichten und Abrufen des Werts eines Zählers, der innerhalb des Zustands des Actors zuverlässig persistent ist. Das Projekt enthält eine Schnittstelle, die andere Dienste zum Aufrufen des Akteurs nutzen können.

### Zustandslose Web-API
Das zustandslose Web-API-Projekt bietet einen einfachen Webdienst, den Sie zum Öffnen Ihrer Anwendung für externe Clients verwenden können. Weitere Informationen dazu, wie das Projekt aufgebaut ist, finden Sie unter [Erste Schritte: Web-API-Dienste von Service Fabric mit selbstgehostetem OWIN](service-fabric-reliable-services-communication-webapi).

## Nächste Schritte
### Erstellen eines Azure-Clusters
Das Service Fabric-SDK stellt einen lokalen Cluster zu Entwicklungs- und Testzwecken bereit. Informationen zum Erstellen eines Clusters in Azure finden Sie unter [Erstellen eines Service Fabric-Clusters im Azure-Portal][create-cluster-in-portal].

### Kostenloses Testen der Bereitstellung von Party Clustern in Azure

Wenn Sie das Bereitstellen und Verwalten von Anwendungen in Azure testen möchten, ohne eigene Cluster einrichten zu müssen, können Sie den kostenlosen [Party Cluster-Dienst](http://aka.ms/tryservicefabric) verwenden.

### Veröffentlichen der Anwendung in Azure
Sie können Ihre Anwendung direkt aus Visual Studio in einem Azure-Cluster veröffentlichen. Informationen dazu finden Sie unter [Veröffentlichen Ihrer Anwendung in Azure][publish-app-to-azure].

### Visualisieren des Clusters mit Service Fabric-Explorer
Service Fabric-Explorer bietet eine einfache Möglichkeit zum Visualisieren Ihres Clusters, einschließlich der bereitgestellten Anwendungen und des physischen Layouts. Informationen dazu finden Sie unter [Visualisieren Ihres Clusters mit Service Fabric Explorer][visualize-with-sfx].

### Versionierung und Upgrade Ihrer Dienste
Service Fabric ermöglicht eine unabhängige Versionierung und Upgrades von unabhängigen Diensten in einer Anwendung. Weitere Informationen finden Sie unter [Versionierung und Upgrade Ihrer Dienste][app-upgrade-tutorial].

### Konfigurieren der Continuous Integration in Visual Studio Team Services
Informationen zur Einrichtung einer Continuous Integration für die Service Fabric-Anwendung finden Sie unter [Konfigurieren der Continuous Integration in Visual Studio Team Services][ci-with-vso].



<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md

<!---HONumber=AcomDC_0406_2016-->