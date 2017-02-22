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
ms.date: 2/17/2017
ms.author: msfussell
translationtype: Human Translation
ms.sourcegitcommit: 47b3fffb2d5c24b7473884e490be19ff17b61b61
ms.openlocfilehash: 97b0cb7a5f04f2c5c547cb4b70d87273aa8f2383


---
# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Vorschau: Bereitstellen eines Windows-Containers in Service Fabric
> [!div class="op_single_selector"]
> * [Bereitstellen von Windows Containern](service-fabric-deploy-container.md)
> * [Bereitstellen von Docker-Containern](service-fabric-deploy-container-linux.md)
> 
> 

In diesem Artikel werden Sie durch den Prozess zum Erstellen von Diensten in Windows-Containern geführt.

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase für Windows Server 2016.
>  

Service Fabric verfügt über mehrere Containerfunktionen für die Erstellung von Anwendungen, die sich aus Microservices in Containern zusammensetzen. 

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
3. Wählen Sie **Imagename** aus, und geben Sie den Pfad zu dem Image in Ihrem Containerrepository an, z.B. https://hub.docker.com/beispielsweise myrepo/myimage:v1 
4. Geben Sie dem Dienst einen Namen, und klicken Sie auf **OK**.
5. Wenn der im Container ausgeführte Dienst einen Endpunkt für die Kommunikation benötigt, können Sie in der Datei „ServiceManifest.xml“ das Protokoll, den Port und den Typ hinzufügen. Beispiel: 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    Durch die Bereitstellung des `UriScheme` wird automatisch der Containerendpunkt im Service Fabric Naming Service registriert, damit er einfacher erkennbar ist. Der Port kann wie bei jedem anderen Dienst entweder fest sein (wie im Beispiel oben) oder dynamisch zugeordnet werden (der Port wird leer gelassen und aus dem der Anwendung zugeordneten Bereich zugewiesen).
    Sie müssen auch die Zuordnung vom Containerport zum Hostport konfigurieren, indem Sie im Anwendungsmanifest eine `PortBinding`-Richtlinie angeben, wie unten beschrieben.
6. Wenn Ihr Container eine Ressourcenkontrolle erfordert, fügen Sie eine `ResourceGovernancePolicy` hinzu.
8. Wenn der Container eine Authentifizierung mit einem privaten Repository durchführen muss, fügen Sie `RepositoryCredentials` hinzu.
7. Sie können das Paket jetzt verwenden und Aktionen für den lokalen Cluster veröffentlichen, wenn die Ausführung unter Windows Server 2016 mit aktivierter Containerunterstützung erfolgt. 
8. Wenn Sie bereit sind, können Sie die Anwendung in einem Remotecluster veröffentlichen oder die Projektmappe in die Quellcodeverwaltung einchecken. 

Eine Beispielanwendung finden Sie in den [Codebeispielen zu Service Fabric-Containern auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers).

## <a name="creating-a-windows-server-2016-cluster"></a>Erstellen eines Windows Server 2016-Clusters
Um die in einem Container enthaltene Anwendung bereitzustellen, müssen Sie einen Cluster unter Windows Server 2016 mit aktivierter Containerunterstützung erstellen. Dafür können Sie den lokalen Entwicklungscomputer oder Azure Resource Manager (ARM) in Azure verwenden. 

Um einen Cluster mit ARM bereitzustellen, wählen Sie das Image **Windows Server 2016 mit Containern** in Azure aus. Weitere Informationen finden Sie im Artikel [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Stellen Sie sicher, dass Sie die folgenden ARM-Einstellungen verwenden:

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
Sie können auch die [hier verfügbare ARM-Vorlage mit&5; Knoten ](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) zum Erstellen eines Clusters verwenden. Lesen Sie alternativ [den Blogbeitrag von Leok](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) zur Verwendung von Service Fabric und Windows-Containern.

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

Sie können Eingabebefehle verwenden, indem Sie das optionale `Commands`-Element mit einer per Komma getrennten Gruppe von Befehlen für die Ausführung im Container angeben.

## <a name="understand-resource-governance"></a>Grundlagen der Ressourcenkontrolle
Die Ressourcenkontrolle ist eine Funktion des Containers, mit der die Ressourcen beschränkt werden, die vom Container auf dem Host verwendet werden können. Mit dem `ResourceGovernancePolicy`-Element, das im Anwendungsmanifest angegeben ist, werden Ressourcenlimits für ein Dienstcodepaket deklariert. Ressourcenlimits können für die folgenden Ressourcen festgelegt werden:

* Arbeitsspeicher
* MemorySwap
* CpuShares (relative CPU-Gewichtung)
* MemoryReservationInMB  
* BlkioWeight (relative BlockIO-Gewichtung)

> [!NOTE]
> In einer zukünftigen Version wird die Unterstützung für das Angeben von bestimmten Block-E/A-Grenzwerten enthalten sein, z.B. IOPS, Bit/s (Lesen/Schreiben) und andere.
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

## <a name="configure-container-port-to-host-port-mapping"></a>Konfigurieren der Zuordnung vom Containerport zum Hostport
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
Mit der `PortBinding`-Richtlinie können Sie einen Containerport einem `Endpoint` im Dienstmanifest zuordnen. Dies wird im folgenden Beispiel veranschaulicht. Der Endpunkt `Endpoint1` kann einen festen Port angeben (z.B. Port 80). Es kann auch kein Port angegeben werden. In diesem Fall wird ein zufälliger Port aus dem Anwendungsportbereich für Sie ausgewählt.

Wenn Sie einen Endpunkt angeben, kann Service Fabric mit dem `Endpoint`-Tag im Dienstmanifest eines Gastcontainers den Endpunkt automatisch für den Naming Service veröffentlichen. Andere Dienste, die im Cluster ausgeführt werden, können diesen Container dann ermitteln, indem sie die REST-Abfragen für die Auflösung nutzen.

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

Durch die Registrierung mit dem Naming Service können Sie im Code für Ihre Container per [Reverseproxy](service-fabric-reverseproxy.md) leicht eine Kommunikation von Container zu Container einrichten. Die Kommunikation erfolgt, indem der HTTP-Lauschport für den Reverseproxy und die Namen der Dienste, mit denen Sie kommunizieren möchten, als Umgebungsvariablen angegeben werden. Weitere Informationen finden Sie im nächsten Abschnitt. 

## <a name="configure-and-set-environment-variables"></a>Konfigurieren und Festlegen von Umgebungsvariablen
Umgebungsvariablen können für jedes Codepaket im Dienstmanifest angegeben werden, und zwar sowohl für Dienste, die in Containern bereitgestellt werden, als auch für Dienste, die als Prozesse oder ausführbare Gastanwendungsdateien bereitgestellt werden. Diese Werte von Umgebungsvariablen können im Anwendungsmanifest spezifisch überschrieben oder während der Bereitstellung als Anwendungsparameter angegeben werden.

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
* Eine Beispielanwendung finden Sie in den [Codebeispielen zu Service Fabric-Containern auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers).



<!--HONumber=Feb17_HO3-->


