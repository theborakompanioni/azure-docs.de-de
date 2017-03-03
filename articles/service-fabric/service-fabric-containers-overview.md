---
title: "Übersicht über Service Fabric und Container | Microsoft-Dokumentation"
description: "Der Artikel enthält eine Übersicht über Service Fabric und die Nutzung von Containern zur Bereitstellung von Microserviceanwendungen. Dieser Artikel enthält eine Übersicht über die Verwendungsweise von Containern sowie über die verfügbaren Funktionen in Service Fabric."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/6/2017
ms.author: msfussell
translationtype: Human Translation
ms.sourcegitcommit: 93e0493e6a62a70a10b8315142765a3c3892acd1
ms.openlocfilehash: abf5e4bc69aa32ca9af8998ef81de20baae24560
ms.lasthandoff: 02/08/2017


---
# <a name="preview-service-fabric-and-containers"></a>Vorschau: Service Fabric und Container
> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase für Linux und Windows Server 2016. 
>   

## <a name="introduction"></a>Einführung
Azure Service Fabric ist ein [Orchestrator](service-fabric-cluster-resource-manager-introduction.md) von Diensten in einem Cluster von Computern, der seit Jahren für zahlreiche Dienste bei Microsoft verwendet und optimiert wird. Dienste können auf vielerlei Arten entwickelt werden – von der Nutzung von [Service Fabric](service-fabric-choose-framework.md)-Programmiermodellen bis zur Bereitstellung von [ausführbaren Gastanwendungsdateien](service-fabric-deploy-existing-app.md). Standardmäßig werden diese Dienste von Service Fabric als Prozesse bereitgestellt und aktiviert. Prozesse ermöglichen die schnellste Aktivierung und Nutzung von Ressourcen in einem Cluster mit der höchsten Dichte. Service Fabric kann Dienste auch in Containerimages bereitstellen. Wichtig ist, dass Sie in derselben Anwendung Dienste in Prozessen und Dienste in Containern mischen können. 

## <a name="containers-and-service-fabric-roadmap"></a>Roadmap für Container und Service Fabric
In den nächsten Releases wird die Service Fabric-Unterstützung für Container unter Windows und Linux weiter ausgebaut. Dazu gehören Verbesserungen bei Netzwerken, Einschränkungen von Ressourcen, Sicherheit, Diagnose, Volumetreiber und Toolunterstützung, insbesondere in Visual Studio. Dies führt zu einer großen Benutzerfreundlichkeit bei der Verwendung von Containerimages zum Bereitstellen von Diensten. Sie haben die Wahl zwischen der Verwendung von Containern zum Packen von vorhandenem Code (z.B. IIS MVC-Apps) oder Service Fabric-Programmiermodellen. Da diese Optionen in Service Fabric gleich behandelt werden, können Sie sie in Ihren Anwendungen kombinieren und bleiben beim Bereitstellen von Code flexibel. Sie erhalten also für jedes Szenario die beste Lösung.

## <a name="what-are-containers"></a>Was sind Container?
Container sind gekapselte, individuell bereitstellbare Komponenten, die als isolierte Instanzen in demselben Kernel ausgeführt werden. Hierbei wird die von einem Betriebssystem bereitgestellte Virtualisierung genutzt. Dies bedeutet, dass jede Anwendung und ihre Laufzeit, Abhängigkeiten und Systembibliotheken in einem Container mit privatem Vollzugriff auf die eigene isolierte Containersicht von Betriebssystemkonstrukten ausgeführt werden. Zusammen mit der Portabilität ist dieser hohe Grad an Sicherheit und Ressourcenisolierung der Hauptvorteil bei der Verwendung von Containern mit Service Fabric. Unter Service Fabric werden Dienste ansonsten in Prozessen ausgeführt.

Container sind eine Virtualisierungstechnologie, bei der das zugrunde liegende Betriebssystem von Anwendungen virtualisiert wird. Sie stellen eine unveränderliche Umgebung für Anwendungen zur Ausführung mit verschiedenen Isolationsgraden dar. Container werden direkt oberhalb des Kernels ausgeführt und verfügen über eine isolierte Ansicht des Dateisystems und anderer Ressourcen. Im Vergleich zu virtuellen Computern haben Container die folgenden Vorteile:

* **Geringe Größe:** Für Container werden nur ein Speicherbereich und kleinere Deltas für jede Schicht verwendet, um die Effizienz zu erhöhen.
* **Kurze Startdauer:** Für Container muss kein Betriebssystem gestartet werden, sodass sie viel schneller als virtuelle Computer verfügbar sind (meist innerhalb weniger Sekunden).
* **Portabilität:** Ein in einem Container enthaltenes Anwendungsimage kann für die Ausführung in der Cloud oder lokal, auf virtuellen Computern oder direkt auf physischen Computern portiert werden.
* **Ressourcenkontrolle:** Ein Container kann die physischen Ressourcen beschränken, die er auf seinem Host verbrauchen kann.

## <a name="container-types"></a>Containertypen
Service Fabric unterstützt die folgenden Containertypen:

### <a name="docker-containers-on-linux"></a>Docker-Container unter Linux
Docker bietet High-Level-APIs zum Erstellen und Verwalten von Containern oberhalb von Linux-Kernelcontainern. Docker Hub ist ein zentrales Repository zum Speichern und Abrufen von Containerimages.

Eine exemplarische Vorgehensweise dazu finden Sie unter [Deploy a Docker container to Service Fabric](service-fabric-deploy-container-linux.md) (Bereitstellen eines Docker-Containers in Service Fabric).

### <a name="windows-server-containers"></a>Windows Server-Container
Unter Windows Server 2016 werden zwei verschiedene Arten von Containern bereitgestellt, die sich anhand des Isolationsgrads unterscheiden. Windows Server-Container und Docker-Containern ähneln sich, da beide über eine Namespace- und Dateisystemisolation verfügen, den Kernel aber gemeinsam mit dem Host nutzen, auf dem sie ausgeführt werden. Unter Linux wurde diese Isolation üblicherweise mit „cgroups“ und Namespaces erzielt. Windows Server-Container verhalten sich ähnlich.

Windows Hyper-V-Container bieten mehr Isolation und Sicherheit, da kein Container den Betriebssystemkernel mit anderen Containern oder mit dem Host gemeinsam verwendet. Aufgrund dieses höheren Grads an Sicherheitsisolation sind Hyper-V-Container für kritischere Szenarien mit mehreren Mandanten geeignet.

Eine exemplarische Vorgehensweise dazu finden Sie unter [Vorschau: Bereitstellen eines Containers in Service Fabric](service-fabric-deploy-container.md).

In der folgenden Abbildung sind die unterschiedlichen Arten der Virtualisierung und der Isolationsgrade dargestellt, die unter dem Betriebssystem verfügbar sind.
![Service Fabric-Plattform][Image1]

## <a name="scenarios-for-using-containers"></a>Szenarien für die Verwendung von Containern
In den folgenden typischen Beispielen ist ein Container eine gute Wahl:

* **IIS-Lift und -Shift:** Wenn bereits [ASP.NET MVC](https://www.asp.net/mvc)-Apps vorhanden sind, die Sie weiterhin verwenden möchten, migrieren Sie sie nicht zu ASP.NET Core, sondern platzieren Sie sie in einem Container. Diese ASP.NET MVC-Apps sind abhängig von ISS (Internet Information Services, Internetinformationsdienste). Sie können diese in Containerimages aus dem vorab erstellten IIS-Image verpacken und mit Service Fabric bereitstellen. Informationen zur Erstellung von IIS-Images finden Sie unter [Containerimages unter Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images).
* **Mischen von Containern und Service Fabric-Microservices:** Verwenden Sie ein vorhandenes Containerimage für einen Teil Ihrer Anwendung. Beispielsweise können Sie den [NGINX-Container](https://hub.docker.com/_/nginx/) für das Web-Front-End Ihrer Anwendung und zustandsbehaftete Dienste für die rechenintensiveren Back-End-Vorgänge verwenden.
* **Reduzieren der Auswirkungen der Beeinträchtigung durch andere Dienste („Noisy Neighbors):** Sie können die Funktion zur Ressourcenkontrolle für Container verwenden, um die Ressourcen einzuschränken, die ein Dienst auf einem Host verwendet. Wenn Dienste eine große Menge von Ressourcen nutzen und die Leistung anderer Dienste beeinträchtigen (z.B. ein Abfragevorgang mit langer Ausführungsdauer), können Sie diese Dienste in Containern mit Ressourcenkontrolle anordnen.

## <a name="service-fabric-support-for-containers"></a>Service Fabric-Unterstützung für Container
Service Fabric unterstützt derzeit die Bereitstellung von Docker-Containern in Linux- und Windows Server-Containern unter Windows Server 2016. Die Unterstützung für Hyper-V-Container wird in einer zukünftigen Version hinzugefügt.

Im Service Fabric- [Anwendungsmodell](service-fabric-application-model.md)stellt ein Container einen Anwendungshost dar, in dem mehrere Dienstreplikate angeordnet werden. Die folgenden Szenarien werden für Container unterstützt:

* **Gastcontainer:** Dieses Szenario ist mit dem [Szenario einer ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md) identisch, bei dem Sie vorhandene Anwendungen in einem Container bereitstellen können. Beispiele sind Node.js, JavaScript oder beliebiger Code (ausführbare Dateien).
* **Zustandslose Dienste in Containern:** Dies sind zustandslose Dienste, die das Reliable Services- und Reliable Actors-Programmiermodell verwenden. Dies wird derzeit nur unter Linux unterstützt. Die Unterstützung für zustandslose Dienste in Windows-Containern wird im Rahmen einer zukünftigen Version hinzugefügt.
* **Zustandsbehaftete Dienste in Containern:** Dies sind zustandsbehaftete Dienste, die das Reliable Actors-Programmiermodell unter Linux verwenden. Reliable Services werden unter Linux derzeit nicht unterstützt.  Die Unterstützung für zustandsbehaftete Dienste in Windows-Containern wird im Rahmen einer zukünftigen Version hinzugefügt.

Service Fabric verfügt über mehrere Containerfunktionen für die Erstellung von Anwendungen, die sich aus Microservices in Containern zusammensetzen. Diese werden als Dienste in Containern bezeichnet. Die Funktionen sind:

* Bereitstellung und Aktivierung des Containerimage
* Ressourcenkontrolle
* Repositoryauthentifizierung
* Containerport zum Hosten der Portzuordnung
* Container-zu-Container-Ermittlung und -Kommunikation
* Möglichkeit zum Konfigurieren und Festlegen von Umgebungsvariablen

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, was Container sind, dass Service Fabric ein Containerorchestrator ist und dass Service Fabric Features zur Unterstützung von Containern bereitstellt. Im nächsten Schritt arbeiten wir Beispiele für die einzelnen Funktionen durch, um deren Nutzung zu veranschaulichen.

[Bereitstellen eines Windows-Containers in Service Fabric unter Windows Server 2016](service-fabric-deploy-container.md)

[Bereitstellen eines Docker-Containers in Service Fabric unter Linux](service-fabric-deploy-container-linux.md)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png

