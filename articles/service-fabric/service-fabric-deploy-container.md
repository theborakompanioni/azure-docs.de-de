---
title: Service Fabric und Bereitstellung von Containern | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie Service Fabric und Container zur Bereitstellung von Microserviceanwendungen nutzen. In diesem Artikel werden die Funktionen, die in Service Fabric für Container enthalten sind, und die Bereitstellung von Windows-Containerimages in einem Cluster beschrieben."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 799cc9ad-32fd-486e-a6b6-efff6b13622d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: 25d6b056421e71fa70ed20a39589f77dbbc25c69
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---
# <a name="deploy-a-windows-container-to-service-fabric"></a>Bereitstellen eines Windows-Containers in Service Fabric
> [!div class="op_single_selector"]
> * [Bereitstellen von Windows-Containern](service-fabric-deploy-container.md)
> * [Bereitstellen von Docker-Containern](service-fabric-deploy-container-linux.md)
> 
> 

In diesem Artikel werden Sie durch den Prozess zum Erstellen von Diensten in Windows-Containern geführt.

Service Fabric verfügt über mehrere Funktionen für die Erstellung von Anwendungen, die sich aus in Containern laufenden Microservices zusammensetzen. 

Die Funktionen sind:

* Bereitstellung und Aktivierung von Containerimages
* Ressourcenkontrolle
* Repositoryauthentifizierung
* Zuordnung vom Containerport zum Hostport
* Container-zu-Container-Ermittlung und -Kommunikation
* Möglichkeit zum Konfigurieren und Festlegen von Umgebungsvariablen

Als Nächstes sehen wir uns die einzelnen Funktionen beim Packen eines Diensts in einem Container zum Einfügen in Ihre Anwendung an.

## <a name="package-a-windows-container"></a>Packen eines Windows-Containers
Beim Packen eines Containers können Sie wählen, ob Sie eine Visual Studio-Projektvorlage verwenden oder das [Anwendungspaket manuell erstellen](#manually).  Bei Verwendung von Visual Studio werden die Anwendungspaketstruktur und die Manifestdateien mit der neuen Projektvorlage für Sie erstellt.

> [!TIP]
> Der einfachste Weg zum Packen eines vorhandenen Containerimages in einen Dienst ist die Verwendung von Visual Studio.

## <a name="use-visual-studio-to-package-an-existing-container-image"></a>Verwenden von Visual Studio zum Packen eines vorhandenen Containerimage
Visual Studio stellt eine Service Fabric-Dienstvorlage bereit, um Sie beim Bereitstellen eines Containers für einen Service Fabric-Cluster zu unterstützen.

1. Wählen Sie **Datei** > **Neues Projekt**, und erstellen Sie eine Service Fabric-Anwendung.
2. Wählen Sie die Dienstvorlage **Gastcontainer** aus.
3. Wählen Sie **Abbildname** aus, und geben Sie den Pfad zu dem Image in Ihrem Containerrepository an. Beispielsweise `myrepo/myimage:v1` in https://hub.docker.com
4. Geben Sie dem Dienst einen Namen, und klicken Sie auf **OK**.
5. Wenn der im Container ausgeführte Dienst einen Endpunkt für die Kommunikation benötigt, können Sie in der Datei „ServiceManifest.xml“ das Protokoll, den Port und den Typ hinzufügen. Beispiel: 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    Durch die Bereitstellung von `UriScheme` registriert Service Fabric automatisch den Containerendpunkt im Service Fabric Naming Service, damit er einfacher erkennbar ist. Der Port kann fest (wie im vorherigen Beispiel gezeigt) oder dynamisch zugeordnet werden. Wenn Sie keinen Port angeben, wird der Port aus dem Portbereich der Anwendung dynamisch zugeordnet (wie es bei jedem anderen Dienst auch der Fall wäre).
    Sie müssen auch die Zuordnung vom Container zum Hostport konfigurieren, indem Sie im Anwendungsmanifest eine `PortBinding`-Richtlinie angeben. Weitere Informationen finden Sie unter [Konfigurieren der Zuordnung vom Containerport zum Hostport](#Portsection).
6. Wenn Ihr Container eine Ressourcenkontrolle erfordert, fügen Sie eine `ResourceGovernancePolicy` hinzu.
8. Wenn der Container eine Authentifizierung mit einem privaten Repository durchführen muss, fügen Sie `RepositoryCredentials` hinzu.
7. Wenn Sie einen Windows Server 2016-Computer mit aktivierter Containerunterstützung verwenden, können Sie das Paket verwenden und Aktionen veröffentlichen, die in Ihrem lokalen Cluster bereitgestellt werden sollen. 
8. Wenn Sie bereit sind, können Sie die Anwendung in einem Remotecluster veröffentlichen oder die Projektmappe in die Quellcodeverwaltung einchecken. 

Ein Beispiel finden Sie in den [Codebeispielen zu Service Fabric-Containern auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="creating-a-windows-server-2016-cluster"></a>Erstellen eines Windows Server 2016-Clusters
Um die in einem Container enthaltene Anwendung bereitzustellen, müssen Sie einen Cluster unter Windows Server 2016 mit aktivierter Containerunterstützung erstellen. Ihr Cluster kann lokal ausgeführt oder mit Azure Resource Manager in Azure bereitgestellt werden. 

Um einen Cluster mit Azure Resource Manager bereitzustellen, wählen Sie das Image **Windows Server 2016 mit Containern** in Azure aus. Weitere Informationen finden Sie im Artikel [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Stellen Sie sicher, dass Sie die folgenden Azure Resource Manager-Einstellungen verwenden:

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
Sie können auch die [Azure Resource Manager-Vorlage Five Node](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) verwenden, um einen Cluster zu erstellen. Lesen Sie auch [den Community-Blogbeitrag](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) zum Verwenden von Service Fabric und Windows-Containern.

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a>Manuelles Packen und Bereitstellen eines Containerimages
Der Vorgang zum manuellen Packen eines Diensts in einem Container basiert auf folgenden Schritten:

1. Veröffentlichen des Containers in Ihrem Repository
2. Erstellen der Verzeichnisstruktur des Pakets
3. Bearbeiten der Dienstmanifestdatei
4. Bearbeiten der Anwendungsmanifestdatei

## <a name="deploy-and-activate-a-container-image"></a>Bereitstellen und Aktivieren eines Containerimage
Im Service Fabric- [Anwendungsmodell](service-fabric-application-model.md)stellt ein Container einen Anwendungshost dar, in dem mehrere Dienstreplikate angeordnet werden. Fügen Sie den Namen des Containerimage im Dienstmanifest in ein `ContainerHost` -Element ein, um einen Container bereitzustellen und zu aktivieren.

Fügen Sie im Dienstmanifest einen `ContainerHost` für den Einstiegspunkt hinzu. Legen Sie anschließend `ImageName` als Namen für das Containerrepository und -image fest. Der folgende Teil eines Manifests zeigt ein Beispiel für die Bereitstellung eines Containers mit dem Namen `myimage:v1` aus einem Repository mit dem Namen `myrepo`:

```xml
    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimage:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>
```

Im Element `Commands` können Sie optionale Befehle angeben, die beim Containerstart ausgeführt werden. Mehrere Befehle sind per Komma zu trennen. 

## <a name="understand-resource-governance"></a>Grundlagen der Ressourcenkontrolle
Die Ressourcenkontrolle ist eine Funktion des Containers, mit der die Ressourcen beschränkt werden, die vom Container auf dem Host verwendet werden können. Mit dem `ResourceGovernancePolicy`-Element, das im Anwendungsmanifest angegeben ist, werden Ressourcenlimits für ein Dienstcodepaket deklariert. Ressourcenlimits können für die folgenden Ressourcen festgelegt werden:

* Arbeitsspeicher
* MemorySwap
* CpuShares (relative CPU-Gewichtung)
* MemoryReservationInMB  
* BlkioWeight (relative BlockIO-Gewichtung)

> [!NOTE]
> Die Unterstützung für das Angeben von bestimmten Block-E/A-Grenzwerten wie IOPs, Bit/s (Lesen/Schreiben) und andere sind für eine zukünftige Version geplant.
> 
> 

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500"
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="authenticate-a-repository"></a>Authentifizieren eines Repositorys
Zum Herunterladen eines Containers müssen Sie unter Umständen die Anmeldeinformationen für das Containerrepository angeben. Die Anmeldeinformationen, die im Anwendungsmanifest enthalten sind, werden zum Angeben der Anmeldeinformationen oder des SSH-Schlüssels zum Herunterladen des Containerimage aus dem Imagerepository verwendet. Das folgende Beispiel enthält ein Konto mit dem Namen *TestUser* und dem Kennwort als Klartext (*nicht* zu empfehlen):

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

Wir empfehlen Ihnen, das Kennwort mit einem Zertifikat zu verschlüsseln, das auf dem Computer bereitgestellt wird.

Im folgenden Beispiel wird ein Konto mit dem Namen *TestUser* verwendet, dessen Kennwort mit dem Zertifikat *MyCert* verschlüsselt wurde. Sie können den PowerShell-Befehl `Invoke-ServiceFabricEncryptText` verwenden, um den Verschlüsselungstext für das Kennwort zu erstellen. Weitere Informationen finden Sie im Artikel [Verwalten von Geheimnissen in Service Fabric-Anwendungen](service-fabric-application-secret-management.md).

Der private Schlüssel des Zertifikats zum Entschlüsseln des Kennworts muss auf dem lokalen Computer mit einer Out-of-Band-Methode bereitgestellt werden. (In Azure ist dies die Methode mit dem Azure Resource Manager.) Wenn Service Fabric das Dienstpaket dann auf dem Computer bereitstellt, kann das Geheimnis entschlüsselt werden. Indem das Geheimnis zusammen mit dem Kontonamen verwendet wird, ist die Authentifizierung mit dem Containerrepository möglich.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name ="Portsection"></a>Konfigurieren der Zuordnung vom Containerport zum Hostport
Sie können einen Hostport zum Kommunizieren mit dem Container konfigurieren, indem Sie im Anwendungsmanifest ein `PortBinding`-Element angeben. Über die Portbindung wird der Port, über den der Dienst im Container lauscht, einem Port auf dem Host zugeordnet.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-to-container-discovery-and-communication"></a>Konfigurieren der Container-zu-Container-Ermittlung und -Kommunikation
Sie können das Element `PortBinding` verwenden, um einen Containerport zu einem Endpunkt im Dienstmanifest zuzuordnen. Im folgenden Beispiel ist durch den Endpunkt `Endpoint1` der feststehende Port 8905 vorgegeben. Es kann auch kein Port angegeben werden. In diesem Fall wird ein zufälliger Port aus dem Anwendungsportbereich für Sie ausgewählt.


```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```
Wenn Sie einen Endpunkt angeben, kann Service Fabric mit dem `Endpoint`-Tag im Dienstmanifest eines Gastcontainers den Endpunkt automatisch für den Naming Service veröffentlichen. Andere Dienste, die im Cluster ausgeführt werden, können diesen Container dann ermitteln, indem sie die REST-Abfragen für die Auflösung nutzen.

Durch die Registrierung mit dem Naming Service können Sie per [Reverseproxy](service-fabric-reverseproxy.md) eine Kommunikation von Container zu Container einrichten. Die Kommunikation erfolgt, indem der HTTP-Lauschport für den Reverseproxy und die Namen der Dienste, mit denen Sie kommunizieren möchten, als Umgebungsvariablen angegeben werden. Weitere Informationen finden Sie im nächsten Abschnitt. 

## <a name="configure-and-set-environment-variables"></a>Konfigurieren und Festlegen von Umgebungsvariablen
Zu jedem Codepaket im Dienstmanifest können Umgebungsvariablen angegeben werden. Diese Funktion ist für alle Dienste verfügbar, unabhängig davon, ob diese als Container, Prozesse oder ausführbare Gastdateien bereitgestellt werden. Die können die Werte von Umgebungsvariablen im Anwendungsmanifest überschreiben oder während der Bereitstellung als Anwendungsparameter angeben.

Der folgende XML-Codeausschnitt eines Dienstmanifests enthält ein Beispiel für die Angabe von Umgebungsvariablen für ein Codepaket:

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>
```

Diese Umgebungsvariablen können auf Anwendungsmanifestebene überschrieben werden:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

Im obigen Beispiel haben wir einen expliziten Wert für die Umgebungsvariable `HttpGateway` (19000) angegeben, und der Wert für den Parameter `BackendServiceName` wird über den Anwendungsparameter `[BackendSvc]` festgelegt. Diese Einstellungen ermöglichen Ihnen das Angeben des Werts für den `BackendServiceName`-Wert während der Anwendungsbereitstellung, und Sie müssen keinen festen Wert im Manifest verwenden.

## <a name="configure-isolation-mode"></a>Isolationsmodus konfigurieren

Windows unterstützt zwei Isolationsmodi für Container: Prozesse und Hyper-V.  Mit dem Prozessisolationsmodus verwenden alle auf demselben Host ausgeführten Container denselben Kernel wie der Host. Mit dem Hyper-V-Isolationsmodus werden die Kernels der einzelnen Hyper-V-Container und des Containerhosts voneinander getrennt. Der Isolationsmodus wird im `ContainerHostPolicies`-Tag der Anwendungsmanifestdatei angegeben.  Als Isolationsmodi können `process`, `hyperv` und `default` angegeben werden. Der `default`-Standardisolationsmodus für Windows Server-Hosts lautet `process`, für Windows 10-Hosts `hyperv`.  Im folgenden Codeausschnitt wird gezeigt, wie der Isolationsmodus in der Anwendungsmanifestdatei angegeben wird.

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a>Vollständige Beispiele für Anwendungs- und Dienstmanifest

Hier ist ein Beispiel für ein Anwendungsmanifest angegeben:

```xml
    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>
```

Dies ist ein Beispiel für ein Dienstmanifest (aus dem obigen Anwendungsmanifest):

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless front end in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Endpoints>
                <Endpoint Name="Endpoint1" UriScheme="http" Port="80" Protocol="http"/>
            </Endpoints>
        </Resources>
    </ServiceManifest>
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun einen Dienst in einem Container bereitgestellt haben, können Sie sich unter [Service Fabric-Anwendungslebenszyklus](service-fabric-application-lifecycle.md) über die Verwaltung des dazugehörigen Lebenszyklus informieren.

* [Übersicht über Service Fabric und Container](service-fabric-containers-overview.md)
* Ein Beispiel finden Sie in [Codebeispiele zu Service Fabric-Containern in GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

