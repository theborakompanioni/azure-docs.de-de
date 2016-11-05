---
title: Übersicht über Service Fabric und Container | Microsoft Docs
description: Der Artikel enthält eine Übersicht über Service Fabric und die Nutzung von Containern zur Bereitstellung von Microserviceanwendungen. Im Artikel wird beschrieben, wie Container verwendet werden können und welche Funktionen in Service Fabric verfügbar sind.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: ''
editor: ''

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2016
ms.author: msfussell

---
# <a name="preview:-service-fabric-and-containers"></a>Vorschau: Service Fabric und Container
> [!NOTE]
> Dieses Feature befindet sich für Linux in der Vorschauphase und ist für Windows Server derzeit nicht verfügbar. Die Vorschauphase für Windows Server beginnt mit der nächsten Version von Service Fabric nach Windows Server 2016 GA, und das Feature wird in der darauffolgenden Version unterstützt.
> 
> 

## <a name="introduction"></a>Einführung
Service Fabric ist ein [Orchestrator](service-fabric-cluster-resource-manager-introduction.md) von Diensten in einem Cluster mit Computern. Dienste können auf vielerlei Arten entwickelt werden – von der Nutzung von [Service Fabric](service-fabric-choose-framework.md)-Programmiermodellen bis zur Bereitstellung von [ausführbaren Gastanwendungsdateien](service-fabric-deploy-existing-app.md). Standardmäßig werden diese Dienste von Service Fabric als Prozesse bereitgestellt und aktiviert. Prozesse ermöglichen die schnellste Aktivierung und Nutzung von Ressourcen im Cluster mit der höchsten Dichte. Service Fabric kann Dienste auch in Containerimages bereitstellen. Ein wichtiger Hinweis ist, dass Sie in derselben Anwendung beide Dienste in Prozessen und Diensten in Containern mischen können. Sie erhalten also für jedes Szenario die beste Lösung.

## <a name="what-are-containers?"></a>Was sind Container?
Container sind gekapselte, individuell bereitstellbare Komponenten, die als isolierte Instanzen in demselben Kernel ausgeführt werden. Hierbei wird die Virtualisierung auf Betriebssystemebene genutzt. Dies bedeutet, dass jede Anwendung, ihre Laufzeit und ihre Systembibliotheken in einem Container mit privatem Vollzugriff auf die eigene isolierte Sicht von Betriebssystemkonstrukten ausgeführt werden. Zusammen mit der Portabilität ist dieser hohe Grad an Sicherheit und Ressourcenisolierung der Hauptvorteil bei der Verwendung von Containern mit Service Fabric. Unter Service Fabric werden Dienste ansonsten in Prozessen ausgeführt. 

Container sind eine Virtualisierungstechnologie, bei der das zugrunde liegende Betriebssystem von Anwendungen virtualisiert wird. Sie stellen eine unveränderliche Umgebung für Anwendungen zur Ausführung mit verschiedenen Isolationsgraden dar. Container werden direkt oberhalb des Kernels ausgeführt und verfügen über eine isolierte Ansicht des Dateisystems und anderer Ressourcen. Im Vergleich zu VMs haben Container die folgenden Vorteile:

1. **Geringe Größe**: Für Container werden nur ein Speicherbereich und kleinere Deltas für jede Schicht verwendet, um eine höhere Effizienz zu erzielen.
2. **Kurze Startdauer**: Für Container muss kein Betriebssystem gestartet werden, sodass sie viel schneller als VMs verfügbar sind (meist innerhalb weniger Sekunden).
3. **Portabilität**: Ein in einem Container enthaltenes Anwendungsimage kann für die Ausführung in der Cloud oder lokal, auf VMs oder direkt auf physischen Computern portiert werden.
4. **Ressourcenkontrolle** zum Beschränken der physischen Ressourcen, die ein Container auf seinem Host verbrauchen kann.

## <a name="container-types"></a>Containertypen
Service Fabric unterstützt die folgenden Containertypen:

### <a name="docker-containers-on-linux"></a>Docker-Container unter Linux
Docker ermöglicht höhere API-Ebenen zum Erstellen und Verwalten von Containern oberhalb von Linux-Kernelcontainern und stellt den Docker-Hub als zentrales Repository zum Speichern und Abrufen von Containerimages bereit. 

### <a name="windows-server-containers"></a>Windows Server-Container
Unter Windows Server 2016 werden zwei verschiedene Arten von Containern bereitgestellt, die sich anhand des Isolationsgrads unterscheiden. Windows Server-Container ähneln Docker-Containern darin, dass sie über eine Namespace- und Dateisystemisolation verfügen, den Kernel aber gemeinsam mit dem Host nutzen, auf dem sie ausgeführt werden. Unter Linux wurde diese Isolation üblicherweise mit „cgroups“ und Namespaces erzielt. Windows Server-Container verhalten sich ähnlich. 

Windows Hyper-V-Container bieten einen höheren Isolationsgrad und mehr Sicherheit, da die Container den Betriebssystemkernel weder untereinander noch mit dem Host gemeinsam verwenden. Aufgrund dieses höheren Grads an Sicherheitsisolation sind Hyper-V-Container besonders für kritischere Szenarien mit mehreren Mandanten geeignet.

In der folgenden Abbildung sind die unterschiedlichen Arten der Virtualisierung und der Isolationsebenen dargestellt, die unter dem Betriebssystem verfügbar sind.
![Service Fabric-Plattform][Image1]

## <a name="scenarios-for-using-containers"></a>Szenarien für die Verwendung von Containern
Typische Beispiele für die Nutzung von Containern sind

1. **IIS-Lift und -Shift**. Wenn Sie einige vorhandene ASP.NET-MVC-Apps weiterverwenden möchten, können Sie sie in einem Container anordnen, anstatt sie zu ASP.NET Core zu migrieren. Diese ASP.NET-MVC-Apps sind von IIS abhängig. Sie können diese in Containerimages aus dem vorab erstellten IIS-Image verpacken und mit Service Fabric bereitstellen. Informationen zur Erstellung von IIS-Images finden Sie unter [Containerimages unter Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images) .
2. **Mischen Sie Container und Service Fabric-Microservices**. Verwenden Sie ein vorhandenes Containerimage für einen Teil Ihrer Anwendung. Beispielsweise können Sie den [NGINX-Container](https://hub.docker.com/_/nginx/) für das Web-Front-End Ihrer Anwendung und zustandsbehaftete Dienste, die mit Reliable Services erstellt wurden, für die rechenintensiveren Back-End-Vorgänge verwenden. Ein Beispiel für dieses Szenario sind Gaminganwendungen.
3. **Reduzieren Sie die Auswirkungen der Beeinträchtigung durch andere Dienste („Noisy Neighbors“)**. Dank der Möglichkeit zur Ressourcenkontrolle von Containern können Sie die Ressource einschränken, die ein Dienst auf einem Host verwendet. Wenn Dienste vorhanden sind, die eine große Zahl von Ressourcen verbrauchen und die Leistung anderer Dienste beeinträchtigen (z.B. ein Abfragevorgang mit langer Ausführungsdauer), können Sie diese in Containern mit Ressourcenkontrolle anordnen.

## <a name="service-fabric-support-for-containers"></a>Service Fabric-Unterstützung für Container
Service Fabric unterstützt derzeit die Bereitstellung von Docker-Containern in Linux- und Windows Server-Containern unter Windows Server 2016. Die Unterstützung für Hyper-V-Container wird in einer zukünftigen Version hinzugefügt. 

Im Service Fabric- [Anwendungsmodell](service-fabric-application-model.md)stellt ein Container einen Anwendungshost dar, in dem mehrere Dienstreplikate angeordnet werden. Die folgenden Szenarien werden für Container unterstützt:

1. **Gastcontainer**: Ist mit dem [Szenario einer ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md) identisch, bei dem Sie vorhandene Anwendungen in einem Container bereitstellen können. Beispiel: „nodejs“, „javascript“ oder beliebiger Code (EXE-Dateien).
2. **Zustandslose Dienste in Containern**: Dies sind zustandslose Dienste, für die das Reliable Services- und Reliable Actors-Programmiermodell verwendet wird. Dies wird derzeit nur unter Linux unterstützt. Die Unterstützung für zustandslose Dienste in Windows-Containern wird im Rahmen einer zukünftigen Version hinzugefügt.
3. **Zustandsbehaftete Dienste in Containern**: Dies sind zustandsbehaftete Dienste, für die das Reliable Actors-Programmiermodell unter Linux verwendet wird. Reliable Services werden unter Linux derzeit nicht unterstützt.  Die Unterstützung für zustandsbehaftete Dienste in Windows-Containern wird im Rahmen einer zukünftigen Version hinzugefügt.

Service Fabric verfügt über mehrere Containerfunktionen für die Erstellung von Anwendungen, die sich aus Microservices in Containern zusammensetzen. Diese werden als Dienste in Containern bezeichnet. Die Funktionen sind:

* Bereitstellung und Aktivierung von Containerimages
* Ressourcenkontrolle
* Repositoryauthentifizierung
* Containerport zum Hosten der Portzuordnung
* Container-zu-Container-Ermittlung und -Kommunikation
* Möglichkeit zum Konfigurieren und Festlegen von Umgebungsvariablen

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, was Container sind, dass Service Fabric ein „Orchestrator“ von Containern ist und welche Funktionen von Service Fabric zur Unterstützung von Containern bereitgestellt werden. Im nächsten Schritt arbeiten wir Beispiele für die einzelnen Funktionen durch, um deren Nutzung zu veranschaulichen. 

[Bereitstellen eines Containers in Service Fabric](service-fabric-deploy-container.md)

[Bild1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png




<!--HONumber=Oct16_HO2-->


