<properties
   pageTitle="Service Fabric und Bereitstellung von Containern | Microsoft Azure"
   description="Es wird beschrieben, wie Sie Service Fabric und Container zur Bereitstellung von Microserviceanwendungen nutzen. In diesem Artikel geht es um die Funktionen, die in Service Fabric für Container enthalten sind, und um die Bereitstellung von Containerimages in einem Cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager=""
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="msfussell"/>


# <a name="preview:-deploy-a-container-to-service-fabric"></a>Vorschau: Bereitstellen eines Containers in Service Fabric

>[AZURE.NOTE] Dieses Feature befindet sich für Linux in der Vorschauphase und ist für Windows Server derzeit nicht verfügbar. Die Vorschauphase für Windows Server beginnt mit der nächsten Version von Service Fabric nach Windows Server 2016 GA, und das Feature wird in der darauffolgenden Version unterstützt.

Service Fabric verfügt über mehrere Containerfunktionen für die Erstellung von Anwendungen, die sich aus Microservices in Containern zusammensetzen. Diese werden als Dienste in Containern bezeichnet. Die Funktionen sind:

- Bereitstellung und Aktivierung von Containerimages
- Ressourcenkontrolle
- Repositoryauthentifizierung
- Containerport zum Hosten der Portzuordnung
- Container-zu-Container-Ermittlung und -Kommunikation
- Möglichkeit zum Konfigurieren und Festlegen von Umgebungsvariablen

Wir sehen uns nacheinander die Funktionen an, die beim Verpacken eines in einem Container enthaltenen Diensts für Ihre Anwendung verwendet werden.

## <a name="packaging-a-container"></a>Packen eines Containers

Beim Packen eines Containers können Sie wählen, ob Sie eine Visual Studio-Projektvorlage verwenden oder das [Anwendungspaket manuell erstellen](#manually). Mit Visual Studio werden die Anwendungspaketstruktur und Manifestdateien mit dem neuen Projekt-Assistenten für Sie erstellt.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Verwenden von Visual Studio zum Packen einer vorhandenen ausführbaren Datei

>[AZURE.NOTE] In einer zukünftigen Version des SDK mit den Visual Studio-Tools können Sie einer Anwendung einen Container auf ähnliche Weise hinzufügen, wie Sie dies bereits von einer ausführbaren Gastanwendung kennen. Weitere Informationen finden Sie im Thema [Bereitstellen einer ausführbaren Gastanwendungsdatei in Service Fabric](service-fabric-deploy-existing-app.md) . Derzeit müssen Sie das Packen wie unten beschrieben manuell durchführen.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-container"></a>Manuelles Packen und Bereitstellen von Containern
Der Vorgang zum manuellen Packen eines Diensts in einem Container basiert auf folgenden Schritten:

1. Veröffentlichen des Containers in Ihrem Repository
2. Erstellen der Verzeichnisstruktur des Pakets
3. Bearbeiten der Dienstmanifestdatei
4. Bearbeiten der Anwendungsmanifestdatei

## <a name="container-image-deployment-and-activation."></a>Bereitstellung und Aktivierung des Containerimage
Im Service Fabric- [Anwendungsmodell](service-fabric-application-model.md)stellt ein Container einen Anwendungshost dar, in dem mehrere Dienstreplikate angeordnet werden. Fügen Sie den Namen des Containerimage im Dienstmanifest in ein `ContainerHost` -Element ein, um einen Container bereitzustellen und zu aktivieren.

Fügen Sie im Dienstmanifest ein `ContainerHost`-Element für den Einstiegspunkt hinzu, und legen Sie `ImageName` als Name für das Containerrepository und Image fest. Der folgende Teil eines Manifests zeigt ein Beispiel für die Bereitstellung eines Containers mit dem Namen *myimage:v1* aus einem Repository mit dem Namen *myrepo*.

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Sie können für das Containerimage Eingabebefehle angeben, indem Sie das optionale `Commands` -Element mit einer per Komma getrennten Gruppe von Befehlen für die Ausführung im Container angeben. 

## <a name="resource-governance"></a>Ressourcenkontrolle
Die Ressourcenkontrolle ist eine Funktion des Containers, mit der die Ressourcen beschränkt werden, die vom Container auf dem Host verwendet werden können. Mit dem `ResourceGovernancePolicy`-Element, das im Anwendungsmanifest angegeben ist, können Ressourcenbegrenzungen für ein Dienstcodepaket deklariert werden. Ressourcenbegrenzungen können für Folgendes festgelegt werden:

- Arbeitsspeicher
- MemorySwap
- CpuShares (relative CPU-Gewichtung)
- MemoryReservationInMB  
- BlkioWeight (relative BlockIO-Gewichtung) 

>[AZURE.NOTE] In einer zukünftigen Version wird die Unterstützung für das Angeben von bestimmten Block-E/A-Grenzwerten möglich sein, z.B. IOPS, Bit/s (Lesen/Schreiben) und andere.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Repositoryauthentifizierung
Zum Herunterladen eines Containers müssen Sie unter Umständen die Anmeldeinformationen für das Containerrepository angeben. Die Anmeldeinformationen, die im *Anwendungs* manifest enthalten sind, werden zum Angeben der Anmeldeinformationen oder des SSH-Schlüssels zum Herunterladen des Containerimage aus dem Imagerepository verwendet.  Das folgende Beispiel enthält ein Konto mit dem Namen *TestUser* und dem Kennwort als Klartext. Dies ist **nicht** zu empfehlen.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Das Kennwort kann und muss verschlüsselt werden, indem ein für den Computer bereitgestelltes Zertifikat verwendet wird.

Im folgenden Beispiel wird ein Konto mit dem Namen *TestUser* verwendet, dessen Kennwort mit dem Zertifikat *MyCert* verschlüsselt wird. Sie können den PowerShell-Befehl `Invoke-ServiceFabricEncryptText` verwenden, um den Verschlüsselungstext für den geheimen Schlüssel für das Kennwort zu erstellen. Der Artikel [Verwalten von geheimen Daten in Service Fabric-Anwendungen](service-fabric-application-secret-management.md) enthält Details hierzu. Der private Schlüssel des Zertifikats zum Entschlüsseln des Kennworts muss auf dem lokalen Computer mit einer Out-of-Band-Methode bereitgestellt werden (in Azure über den Resource Manager). Wenn Service Fabric das Dienstpaket dann auf dem Computer bereitstellt, kann der geheime Schlüssel entschlüsselt und zusammen mit dem Kontonamen zum Authentifizieren für das Containerrepository mit diesen Anmeldeinformationen verwendet werden.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
            <SecurityAccessPolicies>
                <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
            </SecurityAccessPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Containerport zum Hosten der Portzuordnung
Sie können einen Hostport zum Kommunizieren mit dem Container konfigurieren, indem Sie im Anwendungsmanifest ein `PortBinding` -Element angeben. Über die Portbindung wird der Port, über den der Dienst im Container lauscht, einem Port auf dem Host zugeordnet.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Container-zu-Container-Ermittlung und -Kommunikation
Mit der `PortBinding`-Richtlinie können Sie einen Containerport einem `Endpoint` im Dienstmanifest zuordnen. Dies wird im folgenden Beispiel veranschaulicht. Für den Endpunkt `Endpoint1` kann ein fester Port angegeben werden, z.B. Port 80, oder es kann auch kein Port angegeben werden. In diesem Fall wird für Sie ein zufälliger Port aus dem Anwendungsportbereich der Cluster ausgewählt.

Für Gastcontainer ermöglicht diese Angabe eines `Endpoint` -Elements im Dienstmanifest Service Fabric das automatische Veröffentlichen des Endpunkts für den Naming Service, sodass andere im Cluster ausgeführte Dienste den Container über die REST-Abfragen zum Auflösen von Diensten ermitteln können. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Durch die Registrierung mit dem Naming Service können Sie im Code für Ihre Container per [Reverseproxy](service-fabric-reverseproxy.md)leicht eine Kommunikation von Container zu Container einrichten. Sie müssen lediglich den HTTP-Lauschport für den Reverseproxy und die Namen der Dienste angeben, mit denen Sie kommunizieren möchten, indem Sie sie als Umgebungsvariablen festlegen. Die Vorgehensweise ist im nächsten Abschnitt beschrieben.  

## <a name="configure-and-set-environment-variables"></a>Konfigurieren und Festlegen von Umgebungsvariablen
Umgebungsvariablen können für jedes Codepaket im Dienstmanifest für im Container bereitgestellte Dienste oder als Prozesse/ausführbare Gastanwendungsdateien angegeben werden. Diese Werte von Umgebungsvariablen können im Anwendungsmanifest spezifisch überschrieben oder während der Bereitstellung als Anwendungsparameter angegeben werden.

Der folgende XML-Codeausschnitt eines Dienstmanifests enthält ein Beispiel für die Angabe von Umgebungsvariablen für ein Codepaket. 

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

Diese Umgebungsvariablen können auf Anwendungsmanifestebene überschrieben werden:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

Im obigen Beispiel haben wir einen expliziten Wert für die Umgebungsvariable `HttpGateway` (19000) angegeben, und der Wert für den Parameter `BackendServiceName` wird über den Anwendungsparameter `[BackendSvc]` festgelegt. Dies ermöglicht Ihnen das Angeben des Werts für den `BackendServiceName`-Wert während der Anwendungsbereitstellung, und Sie müssen keinen festen Wert im Manifest verwenden. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Vollständige Beispiele für Anwendungs- und Dienstmanifest
Hier ist ein Beispiel für ein Anwendungsmanifest angegeben, in dem die Containerfunktionen enthalten sind:


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


Hier ist ein Beispiel für ein Dienstmanifest (aus dem vorherigen Anwendungsmanifest) angegeben, in dem die Containerfunktionen enthalten sind:

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
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
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>



<!--HONumber=Oct16_HO2-->


