---
title: Service Fabric und Bereitstellung von Containern in Linux | Microsoft Docs
description: "Informationen dazu, wie Sie Service Fabric und Linux-Container zur Bereitstellung von Microserviceanwendungen nutzen. In diesem Artikel werden die Funktionen, die in Service Fabric für Container enthalten sind, und die Bereitstellung von Linux-Container-Images in einem Cluster beschrieben."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: msfussell
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 9dcec753e5f999a1bac07276373c0c25f89ec58d
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
# <a name="deploy-a-linux-container-to-service-fabric"></a>Bereitstellen eines Linux-Containers in Service Fabric
> [!div class="op_single_selector"]
> * [Bereitstellen von Windows-Containern](service-fabric-deploy-container.md)
> * [Bereitstellen von Linux-Containern](service-fabric-deploy-container-linux.md)
>
>

Dieser Artikel zeigt Ihnen, wie Sie unter Linux Dienste in Docker-Containern erstellen.

Service Fabric verfügt über mehrere Containerfunktionen für die Erstellung von Anwendungen, die sich aus Microservices in Containern zusammensetzen. Diese Dienste werden als Dienste in Containern bezeichnet.

Die Funktionen sind:

* Bereitstellung und Aktivierung von Containerimages
* Ressourcenkontrolle
* Repositoryauthentifizierung
* Containerport zum Hosten der Portzuordnung
* Container-zu-Container-Ermittlung und -Kommunikation
* Möglichkeit zum Konfigurieren und Festlegen von Umgebungsvariablen

## <a name="packaging-a-docker-container-with-yeoman"></a>Packen von Docker-Containern mit Yeoman
Beim Packen eines Containers unter Linux können Sie wählen, ob Sie eine Yeoman-Vorlage verwenden oder das [Anwendungspaket manuell erstellen](#manually).

Eine Service Fabric-Anwendung kann einen oder mehrere Container enthalten, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Das Service Fabric SDK für Linux enthält einen [Yeoman](http://yeoman.io/)-Generator, mit dem Sie problemlos die Anwendung erstellen und ein Containerimage hinzufügen können. Lassen Sie uns mit Yeoman eine Anwendung mit einem einzelnen Docker-Container erstellen, deren Name *SimpleContainerApp* lautet. Sie können später weitere Dienste hinzufügen, indem Sie die generierten Manifestdateien bearbeiten.

## <a name="install-docker-on-your-development-box"></a>Installieren von Docker auf dem Entwicklungscomputer

Führen Sie die folgenden Befehle aus, um Docker auf Ihrem Linux-Entwicklungscomputer zu installieren (bei Verwendung des vagrant-Images unter OSX ist Docker bereits installiert):

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-the-application"></a>Erstellen der Anwendung
1. Geben Sie in einem Terminal `yo azuresfcontainer` ein.
2. Benennen Sie die Anwendung, z.B. „meinecontainerapp“.
3. Geben Sie die URL für das Containerimage aus einem DockerHub-Repository an. Der image-Parameter hat das Format [Repository]/[Imagename].
4. Wenn für das Image kein Workloadeinstiegspunkt definiert ist, müssen Sie explizit Eingabebefehle angeben mit einer durch Trennzeichen getrennten Liste der Befehle, die im Container ausgeführt werden sollen, damit der Container nach dem Start weiter ausgeführt wird.

![Service Fabric-Yeoman-Generator für Container][sf-yeoman]

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung

### <a name="using-xplat-cli"></a>Verwendung der XPlat-CLI
Die erstellte Anwendung kann mithilfe der Azure-Befehlszeilenschnittstelle im lokalen Cluster bereitgestellt werden.

1. Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.

    ```bash
    azure servicefabric cluster connect
    ```

2. Verwenden Sie das in der Vorlage bereitgestellte Installationsskript, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

    ```bash
    ./install.sh
    ```

3. Navigieren Sie in einem Browser zu Service Fabric Explorer (http://localhost:19080/Explorer). Falls Sie Vagrant unter Mac OS X verwenden, ersetzen Sie „localhost“ durch die private IP-Adresse des virtuellen Computers.
4. Erweitern Sie den Knoten „Anwendungen“. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.
5. Verwenden Sie das in der Vorlage bereitgestellte Deinstallationsskript, um die Anwendungsinstanz zu löschen und die Registrierung des Anwendungstyps aufzuheben.

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a>Mithilfe von Azure-CLI 2.0

Weitere Informationen finden Sie in der Referenzdokumentation zum Verwalten eines [Anwendungslebenszyklus mit der Azure-CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).

Eine Beispielanwendung finden Sie in den [Codebeispielen zu Service Fabric-Containern auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers).

## <a name="adding-more-services-to-an-existing-application"></a>Hinzufügen weiterer Dienste zu einer vorhandenen Anwendung

Führen Sie zum Hinzufügen eines weiteren Containerdiensts zu einer Anwendung, die bereits mit `yo` erstellt wurde, die folgenden Schritte aus:

1. Legen Sie das Verzeichnis auf den Stamm der vorhandenen Anwendung fest.  Beispiel: `cd ~/YeomanSamples/MyApplication`, wenn `MyApplication` die von Yeoman erstellte Anwendung ist.
2. Führen Sie `yo azuresfcontainer:AddService` aus.

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

> [!NOTE]
> Wenn für das Image kein Workloadeinstiegspunkt definiert ist, müssen Sie explizit Eingabebefehle im `Commands`-Element angeben mit einer durch Trennzeichen getrennten Liste der Befehle, die im Container ausgeführt werden sollen, damit der Container nach dem Start weiter ausgeführt wird.

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
Mit der `PortBinding`-Richtlinie können Sie einen Containerport einem `Endpoint` im Dienstmanifest zuordnen. Der Endpunkt `Endpoint1` kann einen festen Port angeben (z.B. Port 80). Es kann auch kein Port angegeben werden. In diesem Fall wird ein zufälliger Port aus dem Anwendungsportbereich für Sie ausgewählt.

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
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a>Verwandte Artikel

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Erste Schritte mit Service Fabric und der Azure CLI 2.0)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

