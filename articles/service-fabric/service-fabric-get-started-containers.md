---
title: Erstellen einer Azure Service Fabric-Containeranwendung | Microsoft-Dokumentation
description: "Erstellen Sie Ihre erste Windows-Containeranwendung unter Azure Service Fabric.  Erstellen Sie ein Docker-Image mit einer Python-Anwendung, übertragen Sie es per Pushvorgang an eine Containerregistrierung, erstellen Sie eine Service Fabric-Container-App, und stellen Sie diese bereit."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 8c9d6c65666b5ffedf058e0a83d4fc41fff80235
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Erstellen Ihrer ersten Service Fabric-Containeranwendung unter Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Zum Ausführen einer vorhandenen Anwendung eines Windows-Containers in einem Service Fabric-Cluster sind keine Änderungen an Ihrer Anwendung erforderlich. In diesem Artikel wird schrittweise beschrieben, wie Sie ein Docker-Image mit einer Python [Flask](http://flask.pocoo.org/)-Webanwendung erstellen und in einem Service Fabric-Cluster bereitstellen.  Außerdem stellen Sie Ihre Containeranwendung per [Azure Container Registry](/azure/container-registry/) bereit.  In diesem Artikel werden grundlegende Kenntnisse von Docker vorausgesetzt. Weitere Informationen zu Docker finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Voraussetzungen
Ein Entwicklungscomputer, auf dem Folgendes ausgeführt wird:
* Visual Studio 2015 oder Visual Studio 2017
* [Service Fabric-SDK und -Tools](service-fabric-get-started.md)
*  Docker für Windows  [Laden Sie „Docker CE for Windows (stable)“ herunter](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Klicken Sie nach dem Installieren und Starten von Docker mit der rechten Maustaste auf das Taskleistensymbol, und wählen Sie **Switch to Windows containers** (Zu Windows-Containern wechseln). Dies ist erforderlich, um Docker-Images auszuführen, die auf Windows basieren.

Ein Windows-Cluster mit mindestens drei Knoten, die unter Windows Server 2016 mit Containern ausgeführt werden – [erstellen Sie einen Cluster](service-fabric-cluster-creation-via-portal.md), oder [testen Sie Service Fabric kostenlos](https://aka.ms/tryservicefabric). 

Eine Registrierung in Azure Container Registry – erstellen Sie in Ihrem Azure-Abonnement eine [Containerregistrierung](../container-registry/container-registry-get-started-portal.md). 

## <a name="define-the-docker-container"></a>Definieren des Docker-Containers
Erstellen Sie ein Image auf Grundlage des [Python-Image](https://hub.docker.com/_/python/) in Docker Hub. 

Definieren Sie Ihren Docker-Container in einer Dockerfile-Datei. Die Dockerfile-Datei enthält eine Anleitung zum Einrichten der Umgebung in Ihrem Container, Laden der auszuführenden Anwendung und Zuordnen von Ports. Die Dockerfile-Datei ist die Eingabe für den Befehl `docker build`, der das Image erstellt. 

Erstellen Sie ein leeres Verzeichnis, und erstellen Sie die Datei *Dockerfile* (ohne Dateierweiterung). Fügen Sie der *Dockerfile* Folgendes hinzu, und speichern Sie Ihre Änderungen:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Weitere Informationen finden Sie in der [Dockerfile-Referenz](https://docs.docker.com/engine/reference/builder/).

## <a name="create-a-simple-web-application"></a>Erstellen einer einfachen Webanwendung
Erstellen Sie eine Flask-Webanwendung, die über Port 80 lauscht und „Hello World!“ zurückgibt.  Erstellen Sie in demselben Verzeichnis die Datei *requirements.txt*.  Fügen Sie Folgendes hinzu, und speichern Sie die Änderungen:
```
Flask
```

Erstellen Sie auch die Datei *app.py*, und fügen Sie Folgendes hinzu:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-the-image"></a>Erstellen des Image
Führen Sie den Befehl `docker build` aus, um das Image zu erstellen, mit dem Ihre Webanwendung ausgeführt wird. Öffnen Sie ein PowerShell-Fenster, und navigieren Sie zu dem Verzeichnis, das die Dockerfile-Datei enthält. Führen Sie den folgenden Befehl aus:

```
docker build -t helloworldapp .
```

Dieser Befehl erstellt das neue Image mithilfe der Anweisungen in der Dockerfile-Datei. Das Image erhält den Namen „helloworldapp“ (-t tagging). Durch das Erstellen eines Image wird das Basisimage per Pullvorgang von Docker Hub abgerufen, und es wird ein neues Image erstellt, mit dem Ihre Anwendung zusätzlich zum Basisimage hinzugefügt wird.  

Wenn der Buildbefehl abgeschlossen ist, rufen Sie den Befehl `docker images` ab, um Informationen zum neuen Image anzuzeigen:

```
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung
Überprüfen Sie Ihr Image lokal, bevor Sie es mithilfe von Push an die Containerregistrierung übertragen.  

Führen Sie die Anwendung aus.

```
docker run -d --name my-web-site helloworldapp
```

*name* vergibt einen Namen für den ausgeführten Container (anstelle der Container-ID).

Ermitteln Sie nach dem Start des Containers seine IP-Adresse, damit Sie über einen Browser eine Verbindung mit Ihrem ausgeführten Container herstellen können:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Stellen Sie eine Verbindung mit dem ausgeführten Container her.  Öffnen Sie einen Webbrowser, und verweisen Sie auf die zurückgegebene IP-Adresse, „http://172.31.194.61“. Die Überschrift „Hello World!“ wird im Browser angezeigt.

Führen Sie Folgendes aus, um den Container zu beenden:

```
docker stop my-web-site
```

Löschen Sie den Container von Ihrem Entwicklungscomputer:

```
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a>Übertragen des Images an die Containerregistrierung mithilfe von Push
Wenn Sie überprüft haben, ob der Container auf dem Entwicklungscomputer ausgeführt wird, übertragen Sie das Image mithilfe von Push an Ihre Registrierung in Azure Container Registry.

Führen Sie ``docker login`` aus, um sich mit Ihren [Registrierungsanmeldeinformationen](../container-registry/container-registry-authentication.md) an der Containerregistrierung anzumelden.

Im folgenden Beispiel werden die ID und das Kennwort eines Azure Active Directory-[Dienstprinzipals](../active-directory/active-directory-application-objects.md) übergeben. Angenommen, Sie haben Ihrer Registrierung für ein Automatisierungsszenario einen Dienstprinzipal zugewiesen. Oder Sie können sich mit Ihrem Benutzernamen und Kennwort für die Registrierung anmelden.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Mit dem folgenden Befehl wird ein Tag oder Alias des Images mit einem vollqualifizierten Pfad zur Registrierung erstellt. In diesem Beispiel wird das Image im ```samples```-Namespace platziert, um den Stamm der Registrierung nicht zu überladen.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Übertragen des Image mithilfe von Push an Ihre Containerregistrierung:

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
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    ```
    Durch die Bereitstellung von ```UriScheme``` wird automatisch der Containerendpunkt im Service Fabric Naming Service registriert, damit er einfacher erkennbar ist. Eine vollständige „ServiceManifest.xml“-Beispieldatei finden Sie am Ende dieses Artikels. 
7. Konfigurieren Sie auch die Zuordnung vom Containerport zum Hostport, indem Sie in ```ContainerHostPolicies``` in der Datei „ApplicationManifest.xml“ eine ```PortBinding```-Richtlinie angeben.  In diesem Artikel ist ```ContainerPort``` 8081 (der Container macht Port 80 verfügbar, wie in der Dockerfile-Datei angegeben), und ```EndpointRef``` ist „Guest1TypeEndpoint“ (der im Dienstmanifest angegebene Endpunkt).  Über Port 8081 an den Dienst eingehende Anforderungen werden Port 80 im Container zugeordnet.  Wenn der Container eine Authentifizierung mit einem privaten Repository durchführen muss, fügen Sie ```RepositoryCredentials``` hinzu.  Fügen Sie für diesen Artikel den Kontonamen und das Kennwort für die Containerregistrierung „myregistry.azurecr.io“ hinzu. 

    ```xml
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ```

    Eine vollständige ApplicationManifest.xml-Beispieldatei finden Sie am Ende dieses Artikels.
8. Konfigurieren Sie den Cluster-Verbindungsendpunkt, damit Sie die Anwendung in Ihrem Cluster veröffentlichen können.  Sie finden den Client-Verbindungsendpunkt im [Azure-Portal](https://portal.azure.com) auf dem Blatt „Übersicht“ für Ihren Cluster. Öffnen Sie im Projektmappen-Explorer *Cloud.xml* unter **MyFirstContainer**->**PublishProfiles**.  Fügen Sie den Clusternamen und den Verbindungsport zu **ClusterConnectionParameters** hinzu.  Beispiel:
    ```xml
    <ClusterConnectionParameters ConnectionEndpoint="containercluster.westus2.cloudapp.azure.com:19000" />
    ```
    
9. Speichern Sie alle Dateien, und erstellen Sie Ihr Projekt.  

10. Klicken Sie zum Packen Ihrer Anwendung im Projektmappen-Explorer mit der rechten Maustaste auf **MyFirstContainer**, und wählen Sie **Paket**. 

## <a name="deploy-the-container-application"></a>Bereitstellen der Containeranwendung
Klicken Sie zum Veröffentlichen Ihrer Anwendung im Projektmappen-Explorer mit der rechten Maustaste auf **MyFirstContainer**, und wählen Sie **Veröffentlichen**.

[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) ist ein webbasiertes Tool zum Untersuchen und Verwalten von Anwendungen und Knoten in einem Service Fabric-Cluster. Öffnen Sie einen Browser, navigieren Sie zu „http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/“, und führen Sie die Anwendungsbereitstellung aus.  Die Anwendung wird bereitgestellt, befindet sich bis zum Herunterladen des Image auf die Clusterknoten aber in einem Fehlerzustand (je nach Imagegröße kann dies einige Zeit in Anspruch nehmen): ![Fehler][1]

Die Anwendung ist bereit, wenn Sie sich im Zustand ```Ready``` befindet: ![Bereit][2]

Öffnen Sie einen Browser, und navigieren Sie zu „http://containercluster.westus2.cloudapp.azure.com:8081“. Die Überschrift „Hello World!“ wird im Browser angezeigt.

## <a name="clean-up"></a>Bereinigen
Während der Ausführung des Clusters fallen weiterhin Gebühren an. [Löschen](service-fabric-get-started-azure-cluster.md#remove-the-cluster) Sie Ihren Cluster daher ggf.  [Partycluster](http://tryazureservicefabric.westus.cloudapp.azure.com/) werden nach einigen Stunden automatisch gelöscht.

Wenn Sie das Image mithilfe von Push an die Containerregistrierung übertragen haben, können Sie das lokale Image von Ihrem Entwicklungscomputer löschen:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Vollständige Beispiele für Service Fabric-Anwendungs- und Dienstmanifeste
Dies sind die vollständigen Dienst- und Anwendungsmanifeste, die in diesem Artikel verwendet werden.

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
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
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
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
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
* Lesen Sie sich das Tutorial [Bereitstellen einer .NET-App in einem Container in Azure Service Fabric](service-fabric-host-app-in-a-container.md) durch.
* Weitere Informationen zum [Anwendungslebenszyklus](service-fabric-application-lifecycle.md) von Service Fabric
* [Codebeispiele zu Service Fabric-Containern](https://github.com/Azure-Samples/service-fabric-dotnet-containers) in GitHub

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

