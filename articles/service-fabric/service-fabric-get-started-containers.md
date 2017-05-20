---
title: Erstellen einer Azure Service Fabric-Container-App | Microsoft-Dokumentation
description: "Erstellen Sie Ihre erste Container-App in Azure Service Fabric.  Erstellen Sie ein Docker-Image mit Ihrer App, übertragen Sie dieses mittels Push an eine Containerregistrierung, erstellen Sie eine Service Fabric-Container-App, und stellen Sie diese bereit."
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
ms.date: 05/08/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: acb68b274228aa647dc7be5d36b2b077bd213c1b
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="create-your-first-service-fabric-container-app"></a>Erstellen Ihrer ersten Service Fabric-Container-App
Zum Ausführen einer vorhandenen Anwendung in einem Windows-Container in einem Service Fabric-Cluster sind keine Änderungen an Ihrer App erforderlich. Dieser Schnellstart führt Sie durch das Erstellen eines Docker-Images mit einer Web-App, das Übertragen des neuen Images mittels Push an Azure Container Registry, das Erstellen einer Service Fabric-Container-App und das Bereitstellen der Container-App in einem Service Fabric-Cluster.  In diesem Artikel werden grundlegende Kenntnisse von Docker vorausgesetzt. Weitere Informationen zu Docker finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Voraussetzungen
Ein Entwicklungscomputer, auf dem Folgendes ausgeführt wird:
* Visual Studio 2015 oder Visual Studio 2017
* [Service Fabric-SDK und -Tools](service-fabric-get-started.md)
*  Docker für Windows  [Laden Sie „Docker CE for Windows (stable)“ herunter](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Klicken Sie nach dem Installieren und Starten von Docker mit der rechten Maustaste auf das Taskleistensymbol, und wählen Sie **Switch to Windows containers** (Zu Windows-Containern wechseln). Dies ist erforderlich, um Docker-Images auszuführen, die auf Windows basieren.

Ein Windows-Cluster mit mindestens drei Knoten, die unter Windows Server 2016 mit Containern ausgeführt werden – [erstellen Sie einen Cluster](service-fabric-get-started-azure-cluster.md), oder [testen Sie Service Fabric kostenlos](http://tryazureservicefabric.westus.cloudapp.azure.com/). 

Eine Registrierung in Azure Container Registry – erstellen Sie in Ihrem Azure-Abonnement eine [Containerregistrierung](../container-registry/container-registry-get-started-portal.md). 

## <a name="create-a-simple-web-app"></a>Erstellen einer einfachen Web-App
Sammeln Sie alle Ressourcen, die Sie in ein Docker-Image laden müssen, an einem Ort. Erstellen Sie für diesen Schnellstart eine „Hallo Welt“-Web-App auf Ihrem Entwicklungscomputer.

1. Erstellen Sie ein Verzeichnis wie z.B. *c:\temp\helloworldapp*.
2. Erstellen Sie das Unterverzeichnis *c:\temp\helloworldapp\content*.
3. Erstellen Sie die Datei *index.html* unter *c:\temp\helloworldapp\content*.
4. Bearbeiten Sie *index.html*, und fügen Sie die folgende Zeile hinzu:
    ```
    <h1>Hello World!</h1>
    ```
5. Speichern Sie Ihre Änderungen an *index.html*.

## <a name="build-the-docker-image"></a>Erstellen des Docker-Images
Erstellen Sie ein Image auf Grundlage von [microsft/iis image](https://hub.docker.com/r/microsoft/iis/) in Docker Hub. „microsoft/iis image“ wird vom Windows Server Core-Betriebssystem-Basisimage abgeleitet und enthält Internetinformationsdienste (Internet Information Services, IIS).  Durch das Ausführen dieses Images in Ihrem Container werden IIS und die installierten Websites automatisch gestartet.

Definieren Sie Ihr Docker-Image in einer Dockerfile-Datei. Die Dockerfile-Datei enthält Anweisungen zum Erstellen des Images und zum Laden der auszuführenden App. Die Dockerfile-Datei ist die Eingabe für den Befehl ```docker build```, der das Image erstellt. 

1. Erstellen Sie eine *Dockerfile*-Datei (ohne Dateierweiterung) unter *c:\temp\helloworldapp*, und fügen Sie Folgendes hinzu:

    ```
    # The `FROM` instruction specifies the base image. You are
    # extending the `microsoft/iis` image.
    FROM microsoft/iis

    # Create a directory to hold the web app in the container.
    RUN mkdir C:\site

    # Create a new IIS site.
    RUN powershell -NoProfile -Command \
        Import-module IISAdministration; \
        New-IISSite -Name "Site" -PhysicalPath C:\site -BindingInformation "*:8000:"

    # Opens port 8000 on the container.
    EXPOSE 8000

    # The final instruction copies the web app you created earlier into the container.
    ADD content/ /site
    ```

    In dieser Dockerfile-Datei ist kein ```ENTRYPOINT```-Befehl enthalten. Ein solcher Befehl ist nicht erforderlich. Wenn Windows Server mit IIS ausgeführt wird, ist der IIS-Prozess der Einstiegspunkt, der für den Start im Basisimage konfiguriert ist.

    Weitere Informationen finden Sie in der [Dockerfile-Referenz](https://docs.docker.com/engine/reference/builder/).

2. Führen Sie den Befehl ```docker build``` aus, um das Image zu erstellen, das Ihre Web-App ausführt. Öffnen Sie ein PowerShell-Fenster, und navigieren Sie zu *c:\temp\helloworldapp*. Führen Sie den folgenden Befehl aus:

    ```
    docker build -t helloworldapp .
    ```
    Dieser Befehl erstellt das neue Image mithilfe der Anweisungen in der Dockerfile-Datei und benennt das Image „helloworldapp“ als (-t tagging). Durch das Erstellen eines Images wird das Basisimage mithilfe von Pull von Docker Hub abgerufen, und ein neues Image wird erstellt, das Ihre App zusätzlich zum Basisimage hinzufügt.  [microsoft/iis image](https://hub.docker.com/r/microsoft/iis/) und Betriebssystem-Basisimages haben eine Größe von 10,5 GB. Das Herunterladen und Extrahieren auf Ihrem Entwicklungscomputer nimmt daher einige Zeit in Anspruch.  Gehen Sie in der Zeit mittagessen, oder trinken Sie einen Kaffee.  Das Herunterladen nimmt weniger Zeit in Anspruch, wenn Sie das Betriebssystem-Basisimage zuvor bereits mithilfe von Pull auf Ihren Entwicklungscomputer übertragen haben.

3. Wenn der Buildbefehl abgeschlossen ist, rufen Sie den Befehl `docker images` ab, um Informationen zum neuen Image anzuzeigen:

    ```
    docker images
    
    REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
    helloworldapp              latest              86838648aab6        2 minutes ago       10.1 GB
    ```

## <a name="verify-the-image-runs-locally"></a>Lokales Prüfen der Ausführung des Images
Überprüfen Sie Ihr Image lokal, bevor Sie es mithilfe von Push an die Containerregistrierung übertragen.  

1. Starten Sie den Container mit ```docker run```:

    ```
    docker run -d -p 8000:8000 --name my-web-site helloworldapp
    ```

    *name* vergibt einen Namen für den ausgeführten Container (anstelle der Container-ID).

2. Ermitteln Sie nach dem Start des Containers seine IP-Adresse, damit Sie über einen Browser eine Verbindung mit Ihrem ausgeführten Container herstellen können:
    ```
    docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
    ```

3. Stellen Sie eine Verbindung mit dem ausgeführten Container her.  Öffnen Sie einen Webbrowser, und verweisen Sie auf die IP-Adresse, die auf Port 8000 zurückgegeben wurde, z.B. http://172.31.194.61:8000. Die Überschrift „Hallo Welt!“ wird im Browser angezeigt.

4. Führen Sie Folgendes aus, um den Container zu beenden:

    ```
    docker stop my-web-site
    ```

5. Löschen Sie den Container von Ihrem Entwicklungscomputer:

    ```
    docker rm my-web-site
    ```

## <a name="push-the-image-to-the-container-registry"></a>Übertragen des Images an die Containerregistrierung mithilfe von Push
Wenn Sie überprüft haben, ob der Container auf dem Entwicklungscomputer ausgeführt wird, übertragen Sie das Image mithilfe von Push an Ihre Registrierung in Azure Container Registry.

1. Führen Sie ``docker login`` aus, um sich mit Ihren [Registrierungsanmeldeinformationen](../container-registry/container-registry-authentication.md) an der Containerregistrierung anzumelden.

    Im folgenden Beispiel werden die ID und das Kennwort eines Azure Active Directory-[Dienstprinzipals](../active-directory/active-directory-application-objects.md) übergeben. Angenommen, Sie haben Ihrer Registrierung für ein Automatisierungsszenario einen Dienstprinzipal zugewiesen.

    ```
    docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
    ```

2. Mit dem folgenden Befehl wird ein Tag oder Alias des Images mit einem vollqualifizierten Pfad zur Registrierung erstellt. In diesem Beispiel wird das Image im ```samples```-Namespace platziert, um den Stamm der Registrierung nicht zu überladen.

    ```
    docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
    ```

3.  Übertragen des Image mithilfe von Push an Ihre Containerregistrierung:

    ```
    docker push myregistry.azurecr.io/samples/helloworldapp
    ```

## <a name="create-and-package-the-containerized-service-in-visual-studio"></a>Erstellen und Packen des im Container ausgeführten Diensts in Visual Studio
Das Service Fabric-SDK und die Tools stellen eine Dienstvorlage bereit, um Sie beim Bereitstellen eines Containers für einen Service Fabric-Cluster zu unterstützen.

1. Starten Sie Visual Studio.  Wählen Sie **Datei** > **Neu** > **Projekt**.
2. Wählen Sie **Service Fabric-Anwendung**, benennen Sie sie „MyFirstContainer“, und klicken Sie auf **OK**.
3. Wählen Sie **Gastcontainer** aus der Liste der **Dienstvorlagen** aus.
4. Geben Sie unter **Imagename** „myregistry.azurecr.io/samples/helloworldapp“ ein, das Image, das Sie mithilfe von Push an Ihr Containerrepository übertragen haben. 
5. Geben Sie dem Dienst einen Namen, und klicken Sie auf **OK**.
6. Wenn der im Container ausgeführte Dienst einen Endpunkt für die Kommunikation benötigt, können Sie einem ```Endpoint``` in der Datei „ServiceManifest.xml“ das Protokoll, den Port und den Typ hinzufügen. In diesem Schnellstart lauscht der im Container ausgeführte Dienst auf Port 80: 

    ```xml
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    ```
    Durch die Bereitstellung von ```UriScheme``` wird automatisch der Containerendpunkt im Service Fabric Naming Service registriert, damit er einfacher erkennbar ist. Eine vollständige „ServiceManifest.xml“-Beispieldatei finden Sie am Ende dieses Artikels. 
7. Konfigurieren Sie auch die Zuordnung vom Containerport zum Hostport, indem Sie in ```ContainerHostPolicies``` in der Datei „ApplicationManifest.xml“ eine ```PortBinding```-Richtlinie angeben.  In diesem Schnellstart ist ```ContainerPort``` 8000 (der Container macht Port 8000 verfügbar, wie in der Dockerfile-Datei angegeben), und ```EndpointRef``` ist „Guest1TypeEndpoint“ (der im Dienstmanifest angegebene Endpunkt).  Auf Port 80 an den Dienst eingehende Anforderungen werden Port 8000 im Container zugeordnet.  Wenn der Container eine Authentifizierung mit einem privaten Repository durchführen muss, fügen Sie ```RepositoryCredentials``` hinzu.  Fügen Sie für diesen Schnellstart den Kontonamen und das Kennwort für die Containerregistrierung „myregistry.azurecr.io“ hinzu. 

    ```xml
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ```

    Eine vollständige ApplicationManifest.xml-Beispieldatei finden Sie am Ende dieses Artikels.
8. Konfigurieren Sie den Cluster-Verbindungsendpunkt, damit Sie die App in Ihrem Cluster veröffentlichen können.  Sie finden den Client-Verbindungsendpunkt im [Azure-Portal](https://portal.azure.com) auf dem Blatt „Übersicht“ für Ihren Cluster. Öffnen Sie im Projektmappen-Explorer *Cloud.xml* unter **MyFirstContainer**->**PublishProfiles**.  Fügen Sie den Clusternamen und den Verbindungsport zu **ClusterConnectionParameters** hinzu.  Beispiel:
    ```xml
    <ClusterConnectionParameters ConnectionEndpoint="containercluster.westus2.cloudapp.azure.com:19000" />
    ```
    
9. Speichern Sie alle Dateien, und erstellen Sie Ihr Projekt.  

10. Klicken Sie zum Packen Ihrer App mit der rechten Maustaste auf **MyFirstContainer** im Projektmappen-Explorer, und wählen Sie **Paket**. 

## <a name="deploy-the-container-app"></a>Bereitstellen der Container-App
1. Klicken Sie zum Veröffentlichen Ihrer App mit der rechten Maustaste auf **MyFirstContainer** im Projektmappen-Explorer, und wählen Sie **Veröffentlichen**.

2. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) ist ein webbasiertes Tool zum Untersuchen und Verwalten von Anwendungen und Knoten in einem Service Fabric-Cluster. Öffnen Sie einen Browser, navigieren Sie zu http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/, und führen Sie die App-Bereitstellung aus.  Die App wird bereitgestellt, befindet sich bis zum Herunterladen des Images auf die Clusterknoten jedoch in einem Fehlerzustand (je nach Imagegröße kann dies einige Zeit in Anspruch nehmen):  ![Fehler][1]

3. Die App ist bereit, wenn Sie sich im Zustand ```Ready``` befindet:  ![Bereit][2]

4. Öffnen Sie einen Browser, und navigieren Sie zu http://containercluster.westus2.cloudapp.azure.com. Die Überschrift „Hallo Welt!“ wird im Browser angezeigt.

## <a name="clean-up"></a>Bereinigen
Während der Ausführung des Clusters fallen weiterhin Gebühren an. [Löschen](service-fabric-get-started-azure-cluster.md#remove-the-cluster) Sie Ihren Cluster daher ggf.  [Partycluster](http://tryazureservicefabric.westus.cloudapp.azure.com/) werden nach einigen Stunden automatisch gelöscht.

Wenn Sie das Image mithilfe von Push an die Containerregistrierung übertragen haben, können Sie das lokale Image von Ihrem Entwicklungscomputer löschen:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Vollständige Beispiele für Service Fabric-Anwendungs- und Dienstmanifeste
Dies sind die vollständigen Dienst- und Anwendungsmanifeste, die in diesem Schnellstart verwendet werden.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
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
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
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
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Ausführen von [Containern in Service Fabric](service-fabric-containers-overview.md)
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric
* [Codebeispiele zu Service Fabric-Containern](https://github.com/Azure-Samples/service-fabric-dotnet-containers) in GitHub

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

