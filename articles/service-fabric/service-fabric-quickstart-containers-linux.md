---
title: Erstellen einer Azure Service Fabric-Containeranwendung unter Linux | Microsoft-Dokumentation
description: "Erstellen Sie Ihre erste Linux-Containeranwendung unter Azure Service Fabric.  Erstellen Sie ein Docker-Image mit Ihrer Anwendung, übertragen Sie es per Push an eine Containerregistrierung, erstellen Sie eine Service Fabric-Containeranwendung, und stellen Sie diese bereit."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 78306672e812745fd1902ae264c2adea196ab721
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-a-service-fabric-linux-container-application-on-azure"></a>Bereitstellen einer Service Fabric-Containeranwendung unter Linux in Azure
Azure Service Fabric ist eine Plattform für verteilte Systeme zum Bereitstellen und Verwalten von skalierbaren und zuverlässigen Microservices und Containern. 

Zum Ausführen einer vorhandenen Anwendung eines Linux-Containers in einem Service Fabric-Cluster sind keine Änderungen an Ihrer Anwendung erforderlich. In dieser Schnellstartanleitung erfahren Sie, wie Sie ein vorgefertigtes Docker-Containerimage in einer Service Fabric-Anwendung bereitstellen. Nach Abschluss des Vorgangs verfügen Sie über einen aktiven nginx-Container.  Diese Schnellstartanleitung enthält Informationen zum Bereitstellen eines Linux-Containers. Informationen zum Bereitstellen eines Windows-Containers finden Sie in [dieser Schnellstartanleitung](service-fabric-quickstart-containers.md).

![nginx][nginx]

In dieser Schnellstartanleitung wird Folgendes vermittelt:
> [!div class="checklist"]
> * Packen eines Docker-Imagecontainers
> * Konfigurieren der Kommunikation
> * Erstellen und Packen der Service Fabric-Anwendung
> * Bereitstellen der Containeranwendung in Azure

## <a name="prerequisites"></a>Voraussetzungen
Installieren Sie [das Service Fabric SDK, die Service Fabric-Befehlszeilenschnittstelle und die Service Fabric-Yeoman-Vorlagengeneratoren](service-fabric-get-started-linux.md).
  
## <a name="package-a-docker-image-container-with-yeoman"></a>Packen eines Docker-Imagecontainers mit Yeoman
Das Service Fabric SDK für Linux enthält einen [Yeoman](http://yeoman.io/)-Generator, mit dem Sie problemlos die Anwendung erstellen und ein Containerimage hinzufügen können. 

Öffnen Sie zum Erstellen einer Service Fabric-Containeranwendung ein Terminalfenster, und führen Sie `yo azuresfcontainer` aus.  

Nennen Sie Ihre Anwendung „MyFirstContainer“ und den Anwendungsdienst „MyContainerService“.

Geben Sie den Containerimagenamen „nginx:latest“ (das [nginx-Containerimage](https://hub.docker.com/r/_/nginx/) auf Docker Hub) an. 

Dieses Image verfügt über einen definierten Workload-Einstiegspunkt und erfordert daher die explizite Angabe von Eingabebefehlen. 

Geben Sie als Instanzanzahl „1“ an.

![Service Fabric-Yeoman-Generator für Container][sf-yeoman]

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurieren der Kommunikation und der Zuordnung von Containerport zu Hostport
Konfigurieren Sie einen HTTP-Endpunkt, damit Clients mit dem Dienst kommunizieren können.  Öffnen Sie die Datei *./MyFirstContainer/MyContainerServicePkg/ServiceManifest.xml*, und deklarieren Sie im Element **ServiceManifest** eine Endpunktressource.  Fügen Sie das Protokoll, den Port und den Namen hinzu. Im Rahmen dieser Schnellstartanleitung lauscht der Dienst an Port 80: 

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
    <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
Durch die Bereitstellung von `UriScheme` wird automatisch der Containerendpunkt im Service Fabric Naming Service registriert, damit er einfacher erkennbar ist. Eine vollständige „ServiceManifest.xml“-Beispieldatei finden Sie am Ende dieses Artikels. 

Ordnen Sie in der Datei „ApplicationManifest.xml“ unter `ContainerHostPolicies` mithilfe einer `PortBinding`-Richtlinie einem `Endpoint` einen Containerport zu.  In dieser Schnellstartanleitung hat `ContainerPort` den Wert 80 (der Container macht Port 80 verfügbar), und `EndpointRef` den Wert „myserviceTypeEndpoint“ (der zuvor im Dienstmanifest angegebene Endpunkt).  Anforderungen, die beim Dienst an Port 80 eingehen, werden im Container dem Port 80 zugeordnet.  

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>Erstellen und Packen der Service Fabric-Anwendung
Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript für [Gradle](https://gradle.org/). Damit können Sie die Anwendung über das Terminal erstellen. Speichern Sie alle Änderungen.  Führen Sie Folgendes aus, um die Anwendung zu erstellen und zu packen:

```bash
cd MyFirstContainer
gradle
```
## <a name="create-a-cluster"></a>Erstellen eines Clusters
Für die Anwendungsbereitstellung in einem Cluster in Azure können Sie entweder einen eigenen Cluster erstellen oder einen Partycluster verwenden.

Partycluster sind kostenlose, zeitlich begrenzte Service Fabric-Cluster, die in Azure gehostet und vom Service Fabric-Team ausgeführt werden, in denen jeder Benutzer Anwendungen bereitstellen und mehr über die Plattform erfahren kann. Eine Anleitung für den Zugriff auf einen Partycluster finden Sie [hier](http://aka.ms/tryservicefabric).  

Informationen zum Erstellen Ihres eigenen Clusters finden Sie unter [Erstellen Ihres ersten Service Fabric-Clusters in Azure](service-fabric-get-started-azure-cluster.md).

Notieren Sie sich den Verbindungsendpunkt, da Sie ihn im nächsten Schritt benötigen.

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung für Azure
Die erstellte Anwendung kann mithilfe der Service Fabric-Befehlszeilenschnittstelle im Azure-Cluster bereitgestellt werden.

Stellen Sie eine Verbindung mit dem Service Fabric-Cluster in Azure her.

```bash
sfctl cluster select --endpoint http://lnxt10vkfz6.westus.cloudapp.azure.com:19080
```

Verwenden Sie das in der Vorlage bereitgestellte Installationsskript, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

```bash
./install.sh
```

Navigieren Sie in einem Browser zu Service Fabric Explorer (http://lnxt10vkfz6.westus.cloudapp.azure.com:19080/Explorer). Erweitern Sie den Knoten „Anwendungen“. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.

![Service Fabric Explorer][sfx]

Stellen Sie eine Verbindung mit dem ausgeführten Container her.  Öffnen Sie in einem Webbrowser die am Port 80 zurückgegebene IP-Adresse (beispielsweise „lnxt10vkfz6.westus.cloudapp.azure.com:80“). Im Browser sollte die Willkommensseite von nginx angezeigt werden.

![nginx][nginx]

## <a name="clean-up"></a>Bereinigen
Verwenden Sie das Deinstallationsskript aus der Vorlage, um die Anwendungsinstanz aus dem Cluster zu löschen und die Registrierung des Anwendungstyps aufzuheben.

```bash
./uninstall.sh
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Vollständige Beispiele für Service Fabric-Anwendungs- und Dienstmanifeste
Im Anschluss finden Sie die vollständigen Dienst- und Anwendungsmanifeste, die in dieser Schnellstartanleitung verwendet werden:

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>nginx:latest</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>

```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest  ApplicationTypeName="MyFirstContainerType" ApplicationTypeVersion="1.0.0"
                      xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
   <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
</ServiceManifestImport>
   
   <DefaultServices>
      <Service Name="MyContainerService">
        <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
        cluster, set InstanceCount to -1 for the container service to run on every node in 
        the cluster.
        -->
        <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="1">
            <SingletonPartition />
        </StatelessService>
      </Service>
   </DefaultServices>
   
</ApplicationManifest>

```

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung wird Folgendes vermittelt:
> [!div class="checklist"]
> * Packen eines Docker-Imagecontainers
> * Konfigurieren der Kommunikation
> * Erstellen und Packen der Service Fabric-Anwendung
> * Bereitstellen der Containeranwendung in Azure

* Weitere Informationen zum Ausführen von [Containern in Service Fabric](service-fabric-containers-overview.md)
* Lesen Sie sich das Tutorial [Bereitstellen einer .NET-App in einem Container in Azure Service Fabric](service-fabric-host-app-in-a-container.md) durch.
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric
* [Codebeispiele zu Service Fabric-Containern](https://github.com/Azure-Samples/service-fabric-dotnet-containers) in GitHub

[sfx]: ./media/service-fabric-quickstart-containers-linux/SFX.png
[nginx]: ./media/service-fabric-quickstart-containers-linux/nginx.png
[sf-yeoman]: ./media/service-fabric-quickstart-containers-linux/YoSF.png

