---
title: Packen Ihrer Azure Service Fabric-Microservices in Container (Vorschauversion)
description: "Azure Service Fabric verfügt über eine neue Funktion, mit der Sie Ihre Service Fabric-Microservices in Container packen können. Diese Funktion steht derzeit als Vorschau zur Verfügung."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: anmolah
editor: anmolah
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/04/2017
ms.author: anmola
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: 6f8ad0bad8d1ae861e6b72f7e1a32ab0675813c2
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="how-to-containerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a>Packen Ihrer Reliable Services und Reliable Actors von Service Fabric in Container (Vorschauversion)

Service Fabric unterstützt das Packen von Service Fabric-Microservices (auf Reliable Services und Reliable Actors basierende Dienste) in Container. Weitere Informationen finden Sie unter [Service Fabric und Container](service-fabric-containers-overview.md).


 Dieses Feature befindet sich in der Vorschauphase. Der vorliegende Artikel enthält die Schritte, mit denen Sie Ihren Dienst innerhalb eines Containers ausführen können.  

> [!NOTE]
> Das Feature befindet sich derzeit in der Vorschauphase und wird in Produktionsumgebungen nicht unterstützt. Dieses Feature steht derzeit nur für Windows zur Verfügung.

## <a name="steps-to-containerize-your-service-fabric-application"></a>Schritte zum Packen Ihrer Service Fabric-Anwendung in einen Container

1. Öffnen Sie Ihre Service Fabric-Anwendung in Visual Studio.

2. Fügen Sie Ihrem Projekt die Klasse [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) hinzu. Bei dem Code in dieser Klasse handelt es sich um ein Hilfsprogramm, mit dem die Binärdateien der Service Fabric-Laufzeit korrekt in der Anwendung geladen werden, wenn die Ausführung innerhalb eines Containers erfolgt.

3. Initialisieren Sie für jedes Codepaket, das Sie in einen Container packen möchten, das Ladeprogramm am Programmeinstiegspunkt. Fügen Sie Ihrer Programmeinstiegspunkt-Datei den statischen Konstruktor aus dem folgenden Codeausschnitt hinzu.

  ```csharp
  namespace MyApplication
  {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is the entry point of the service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. Erstellen und [Packen](service-fabric-package-apps.md#Package-App) Sie Ihr Projekt. Klicken Sie zum Erstellen des Projekts und eines Pakets im Projektmappen-Explorer mit der rechten Maustaste auf das Anwendungsprojekt, und wählen Sie den Befehl **Paket** aus.

5. Führen Sie für jedes Codepaket, das Sie in einen Container packen möchten, das PowerShell-Skript [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1) aus. Die Nutzung lautet wie folgt:
  ```powershell
    $codePackagePath = 'Path to the code package to containerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
    $applicationExeName = 'Name of the ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  Das Skript erstellt unter „$dockerPackageOutputDirectoryPath“ einen Ordner mit Docker-Artefakten. Ändern Sie die generierte Dockerfile-Datei, um beispielsweise Ports verfügbar zu machen oder Setupskripts auszuführen.

6. Als Nächstes müssen Sie Ihr Docker-Containerpaket [erstellen](service-fabric-get-started-containers.md#Build-Containers) und an Ihr Repository [pushen](service-fabric-get-started-containers.md#Push-Containers).

7. Ändern Sie die Dateien „ApplicationManifest.xml“ und „ServiceManifest.xml“, um das Containerimage, Repositoryinformationen, die Registrierungsauthentifizierung und die Port-zu-Host-Zuordnung hinzuzufügen. Informationen zum Ändern der Manifeste finden Sie unter [Erstellen Ihrer ersten Service Fabric-Containeranwendung unter Windows](service-fabric-get-started-containers.md). Die Codepaketdefinition im Dienstmanifest muss durch das entsprechende Containerimage ersetzt werden. Legen Sie den Einstiegspunkt auf einen Containerhosttyp fest.

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables to your container: -->    
</CodePackage>
  ```

8. Fügen Sie die Port-zu-Host-Zuordnung für Ihren Replikator- und Dienstendpunkt hinzu. Da diese beiden Ports zur Laufzeit von Service Fabric zugewiesen werden, wird der Containerport auf „0“ festgelegt, um den zugewiesenen Port für die Zuordnung zu verwenden.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. Zum Testen der Anwendung muss sie in einem Cluster bereitgestellt werden, der mindestens über die Version 5.7 verfügt. Darüber hinaus müssen Sie die Clustereinstellungen bearbeiten und aktualisieren, um dieses Vorschaufeature zu aktivieren. Führen Sie die Schritte in [diesem Artikel](service-fabric-cluster-fabric-settings.md) aus, um die folgende Einstellung hinzuzufügen:
```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
```
10. Führen Sie als Nächstes die [Bereitstellung](service-fabric-deploy-remove-applications.md) des bearbeiteten Anwendungspakets für diesen Cluster durch.

Sie verfügen nun über eine in einen Container gepackte Service Fabric-Anwendung, die in Ihrem Cluster ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Ausführen von [Containern in Service Fabric](service-fabric-get-started-containers.md)
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric

