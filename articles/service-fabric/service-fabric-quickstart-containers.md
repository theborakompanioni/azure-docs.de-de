---
title: Erstellen einer Azure Service Fabric-Containeranwendung unter Windows | Microsoft-Dokumentation
description: Erstellen Sie Ihre erste Windows-Containeranwendung unter Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: c4f8c94e23a165b22533ffd74e04c9a7310f2d22
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-a-service-fabric-windows-container-application-on-azure"></a>Bereitstellen einer Service Fabric-Containeranwendung unter Windows in Azure
Azure Service Fabric ist eine Plattform für verteilte Systeme zum Bereitstellen und Verwalten von skalierbaren und zuverlässigen Microservices und Containern. 

Zum Ausführen einer vorhandenen Anwendung eines Windows-Containers in einem Service Fabric-Cluster sind keine Änderungen an Ihrer Anwendung erforderlich. In dieser Schnellstartanleitung erfahren Sie, wie Sie ein vorgefertigtes Docker-Containerimage in einer Service Fabric-Anwendung bereitstellen. Nach Abschluss des Vorgangs verfügen Sie über einen aktiven Container für Windows Server 2016 Nano Server und IIS. Diese Schnellstartanleitung enthält Informationen zum Bereitstellen eines Windows-Containers. Informationen zum Bereitstellen eines Linux-Containers finden Sie in [dieser Schnellstartanleitung](service-fabric-quickstart-containers-linux.md).

![IIS-Standardwebseite][iis-default]

In dieser Schnellstartanleitung wird Folgendes vermittelt:
> [!div class="checklist"]
> * Packen eines Docker-Imagecontainers
> * Konfigurieren der Kommunikation
> * Erstellen und Packen der Service Fabric-Anwendung
> * Bereitstellen der Containeranwendung in Azure

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. (Sie können ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.)
* Ein Entwicklungscomputer, auf dem Folgendes ausgeführt wird:
  * Visual Studio 2015 oder Visual Studio 2017
  * [Service Fabric-SDK und -Tools](service-fabric-get-started.md)

## <a name="package-a-docker-image-container-with-visual-studio"></a>Packen eines Docker-Imagecontainers mit Visual Studio
Das Service Fabric-SDK und die Tools stellen eine Dienstvorlage bereit, um Sie beim Bereitstellen eines Containers für einen Service Fabric-Cluster zu unterstützen.

Starten Sie Visual Studio als Administrator.  Wählen Sie **Datei** > **Neu** > **Projekt**.

Wählen Sie **Service Fabric-Anwendung**, benennen Sie sie „MyFirstContainer“, und klicken Sie auf **OK**.

Wählen Sie in der Liste **Dienstvorlagen** die Option **Container** aus.

Geben Sie unter **Imagename** die Zeichenfolge „nanoserver/iis“ ([Basisimage für Windows Server 2016 Nano Server und IIS](https://hub.docker.com/r/nanoserver/iis/)) ein. 

Nennen Sie den Dienst „MyContainerService“, und klicken Sie auf **OK**.

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurieren der Kommunikation und der Zuordnung von Containerport zu Hostport
Der Dienst benötigt einen Endpunkt für die Kommunikation.  Sie können einem `Endpoint` in der Datei „ServiceManifest.xml“ nun das Protokoll, den Port und den Typ hinzufügen. Im Rahmen dieser Schnellstartanleitung lauscht der im Container ausgeführte Dienst an Port 80: 

```xml
<Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
```
Durch die Bereitstellung von `UriScheme` wird automatisch der Containerendpunkt im Service Fabric Naming Service registriert, damit er einfacher erkennbar ist. Eine vollständige „ServiceManifest.xml“-Beispieldatei finden Sie am Ende dieses Artikels. 

Konfigurieren Sie auch die Zuordnung vom Containerport zum Hostport, indem Sie in `ContainerHostPolicies` in der Datei „ApplicationManifest.xml“ eine `PortBinding`-Richtlinie angeben.  In dieser Schnellstartanleitung hat `ContainerPort` den Wert 80, und `EndpointRef` hat den Wert „MyContainerServiceTypeEndpoint“ (der im Dienstmanifest angegebene Endpunkt).  Anforderungen, die beim Dienst an Port 80 eingehen, werden im Container dem Port 80 zugeordnet.  

```xml
<ServiceManifestImport>
...
  <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
    </ContainerHostPolicies>
  </Policies>
</ServiceManifestImport>
```

Eine vollständige ApplicationManifest.xml-Beispieldatei finden Sie am Ende dieses Artikels.

## <a name="create-a-cluster"></a>Erstellen eines Clusters
Für die Anwendungsbereitstellung in einem Cluster in Azure können Sie entweder einen eigenen Cluster erstellen oder einen Partycluster verwenden.

Partycluster sind kostenlose, zeitlich begrenzte Service Fabric-Cluster, die in Azure gehostet und vom Service Fabric-Team ausgeführt werden, in denen jeder Benutzer Anwendungen bereitstellen und mehr über die Plattform erfahren kann. Eine Anleitung für den Zugriff auf einen Partycluster finden Sie [hier](http://aka.ms/tryservicefabric).  

Informationen zum Erstellen Ihres eigenen Clusters finden Sie unter [Erstellen Ihres ersten Service Fabric-Clusters in Azure](service-fabric-get-started-azure-cluster.md).

Notieren Sie sich den Verbindungsendpunkt, da Sie ihn im nächsten Schritt benötigen.  

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>Bereitstellen der Anwendung für Azure mithilfe von Visual Studio
Nachdem die Anwendung nun bereit ist, können Sie sie direkt aus Visual Studio in einem Cluster bereitstellen.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **MyFirstContainer**, und wählen Sie **Veröffentlichen** aus. Das Dialogfeld „Veröffentlichen“ wird angezeigt.

![Dialogfeld „Veröffentlichen“](./media/service-fabric-quickstart-dotnet/publish-app.png)

Tragen Sie den Verbindungsendpunkt des Clusters in das Feld **Verbindungsendpunkt** ein, und klicken Sie auf **Veröffentlichen**. Bei der Registrierung für den Partycluster wird der Verbindungsendpunkt im Browser angegeben. Beispiel: `winh1x87d1d.westus.cloudapp.azure.com:19000`.

Navigieren Sie in einem Browser zu http://winh1x87d1d.westus.cloudapp.azure.com:80. Die IIS-Standardwebseite sollte angezeigt werden: ![IIS-Standardwebseite][iis-default]

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Vollständige Beispiele für Service Fabric-Anwendungs- und Dienstmanifeste
Im Anschluss finden Sie die vollständigen Dienst- und Anwendungsmanifeste, die in dieser Schnellstartanleitung verwendet werden:

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>nanoserver/iis</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="MyContainerService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="MyContainerService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="[MyContainerService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Packen eines Docker-Imagecontainers
> * Konfigurieren der Kommunikation
> * Erstellen und Packen der Service Fabric-Anwendung
> * Bereitstellen der Containeranwendung in Azure

* Weitere Informationen zum Ausführen von [Containern in Service Fabric](service-fabric-containers-overview.md)
* Lesen Sie sich das Tutorial [Bereitstellen einer .NET-App in einem Container in Azure Service Fabric](service-fabric-host-app-in-a-container.md) durch.
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric
* [Codebeispiele zu Service Fabric-Containern](https://github.com/Azure-Samples/service-fabric-dotnet-containers) in GitHub

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png

