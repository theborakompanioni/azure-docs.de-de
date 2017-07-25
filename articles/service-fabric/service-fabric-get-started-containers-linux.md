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
ms.date: 06/28/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: e4ca1e8df8337e578e014cedec68553e6fc56299
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Erstellen Ihrer ersten Service Fabric-Containeranwendung unter Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Zum Ausführen einer vorhandenen Anwendung eines Linux-Containers in einem Service Fabric-Cluster sind keine Änderungen an Ihrer Anwendung erforderlich. In diesem Artikel wird schrittweise beschrieben, wie Sie ein Docker-Image mit einer Python [Flask](http://flask.pocoo.org/)-Webanwendung erstellen und in einem Service Fabric-Cluster bereitstellen.  Außerdem stellen Sie Ihre Containeranwendung per [Azure Container Registry](/azure/container-registry/) bereit.  In diesem Artikel werden grundlegende Kenntnisse von Docker vorausgesetzt. Weitere Informationen zu Docker finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Entwicklungscomputer, auf dem Folgendes ausgeführt wird:
  * [Service Fabric-SDK und -Tools](service-fabric-get-started-linux.md)
  * [Docker CE für Linux](https://docs.docker.com/engine/installation/#prior-releases). 

* Eine Registrierung in Azure Container Registry – erstellen Sie in Ihrem Azure-Abonnement eine [Containerregistrierung](../container-registry/container-registry-get-started-portal.md). 

## <a name="define-the-docker-container"></a>Definieren des Docker-Containers
Erstellen Sie ein Image auf Grundlage des [Python-Image](https://hub.docker.com/_/python/) in Docker Hub. 

Definieren Sie Ihren Docker-Container in einer Dockerfile-Datei. Die Dockerfile-Datei enthält eine Anleitung zum Einrichten der Umgebung in Ihrem Container, Laden der auszuführenden Anwendung und Zuordnen von Ports. Die Dockerfile-Datei ist die Eingabe für den Befehl `docker build`, der das Image erstellt. 

Erstellen Sie ein leeres Verzeichnis, und erstellen Sie die Datei *Dockerfile* (ohne Dateierweiterung). Fügen Sie der *Dockerfile* Folgendes hinzu, und speichern Sie Ihre Änderungen:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
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
Führen Sie den Befehl `docker build` aus, um das Image zu erstellen, mit dem Ihre Webanwendung ausgeführt wird. Öffnen Sie ein PowerShell-Fenster, und navigieren Sie zu *c:\temp\helloworldapp*. Führen Sie den folgenden Befehl aus:

```bash
docker build -t helloworldapp .
```

Dieser Befehl erstellt das neue Image mithilfe der Anweisungen in der Dockerfile-Datei. Das Image erhält den Namen „helloworldapp“ (-t tagging). Durch das Erstellen eines Image wird das Basisimage per Pullvorgang von Docker Hub abgerufen, und es wird ein neues Image erstellt, mit dem Ihre Anwendung zusätzlich zum Basisimage hinzugefügt wird.  

Wenn der Buildbefehl abgeschlossen ist, rufen Sie den Befehl `docker images` ab, um Informationen zum neuen Image anzuzeigen:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung
Stellen Sie sicher, dass Ihre Containeranwendung lokal ausgeführt wird, bevor Sie sie an die Containerregistrierung übertragen.  

Führen Sie die Anwendung aus, und ordnen Sie den Port 4000 Ihres Computers dem verfügbar gemachten Port 80 des Containers zu:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

*name* vergibt einen Namen für den ausgeführten Container (anstelle der Container-ID).

Stellen Sie eine Verbindung mit dem ausgeführten Container her.  Öffnen Sie einen Webbrowser, und verweisen Sie auf die IP-Adresse, die über Port 4000 zurückgegeben wurde, z.B. „http://localhost:4000“. Die Überschrift „Hello World!“ wird im Browser angezeigt.

![Hello World!][hello-world]

Führen Sie Folgendes aus, um den Container zu beenden:

```bash
docker stop my-web-site
```

Löschen Sie den Container von Ihrem Entwicklungscomputer:

```bash
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a>Übertragen des Images an die Containerregistrierung mithilfe von Push
Nachdem Sie sichergestellt haben, dass die Anwendung in Docker ausgeführt wird, können Sie das Image per Pushvorgang in Ihre Registrierung in der Azure Container Registry übertragen.

Führen Sie `docker login` aus, um sich mit Ihren [Registrierungsanmeldeinformationen](../container-registry/container-registry-authentication.md) an der Containerregistrierung anzumelden.

Im folgenden Beispiel werden die ID und das Kennwort eines Azure Active Directory-[Dienstprinzipals](../active-directory/active-directory-application-objects.md) übergeben. Angenommen, Sie haben Ihrer Registrierung für ein Automatisierungsszenario einen Dienstprinzipal zugewiesen.  Oder Sie können sich mit Ihrem Benutzernamen und Kennwort für die Registrierung anmelden.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Mit dem folgenden Befehl wird ein Tag oder Alias des Images mit einem vollqualifizierten Pfad zur Registrierung erstellt. In diesem Beispiel wird das Image im `samples`-Namespace platziert, um den Stamm der Registrierung nicht zu überladen.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Übertragen des Image mithilfe von Push an Ihre Containerregistrierung:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-the-docker-image-with-yeoman"></a>Packen des Docker-Image mit Yeoman
Das Service Fabric SDK für Linux enthält einen [Yeoman](http://yeoman.io/)-Generator, mit dem Sie problemlos die Anwendung erstellen und ein Containerimage hinzufügen können. Lassen Sie uns mit Yeoman eine Anwendung mit einem einzelnen Docker-Container erstellen, deren Name *SimpleContainerApp* lautet.

Öffnen Sie zum Erstellen einer Service Fabric-Containeranwendung ein Terminalfenster, und führen Sie `yo azuresfcontainer` aus.  

Geben Sie der Anwendung einen Namen (z.B. „mycontainer“). 

Geben Sie die URL für das Containerimage in einer Containerregistrierung (z.B. „“) an. 

Da für dieses Image ein Workloadeinstiegspunkt definiert wird, müssen Sie Eingabebefehle explizit angeben (Befehle werden im Container ausgeführt, sodass die Ausführung des Containers nach dem Start beibehalten wird). 

Geben Sie als Instanzanzahl „1“ an.

![Service Fabric-Yeoman-Generator für Container][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>Konfigurieren der Portzuordnung und der Authentifizierung für das Containerrepository
Für Ihren im Container ausgeführten Dienst wird ein Endpunkt für die Kommunikation benötigt.  Fügen Sie einem `Endpoint` in der Datei „ServiceManifest.xml“ nun das Protokoll, den Port und den Typ hinzu. In diesem Artikel lauscht der im Container ausgeführte Dienst auf Port 4000: 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
Durch die Bereitstellung von `UriScheme` wird automatisch der Containerendpunkt im Service Fabric Naming Service registriert, damit er einfacher erkennbar ist. Eine vollständige „ServiceManifest.xml“-Beispieldatei finden Sie am Ende dieses Artikels. 

Konfigurieren Sie auch die Zuordnung vom Containerport zum Hostport, indem Sie in `ContainerHostPolicies` in der Datei „ApplicationManifest.xml“ eine `PortBinding`-Richtlinie angeben.  In diesem Artikel ist der `ContainerPort` 80 (der Container macht Port 80 verfügbar, wie in der Dockerfile-Datei angegeben), und `EndpointRef` ist „myserviceTypeEndpoint“ (der im Dienstmanifest angegebene Endpunkt).  Über Port 4000 an den Dienst eingehende Anforderungen werden Port 80 im Container zugeordnet.  Wenn der Container eine Authentifizierung mit einem privaten Repository durchführen muss, fügen Sie `RepositoryCredentials` hinzu.  Fügen Sie für diesen Artikel den Kontonamen und das Kennwort für die Containerregistrierung „myregistry.azurecr.io“ hinzu. 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>Erstellen und Packen der Service Fabric-Anwendung
Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript für [Gradle](https://gradle.org/). Damit können Sie die Anwendung über das Terminal erstellen. Führen Sie Folgendes aus, um die Anwendung zu erstellen und zu packen:

```bash
cd mycontainer
gradle
```

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Die erstellte Anwendung kann mithilfe der Azure-Befehlszeilenschnittstelle im lokalen Cluster bereitgestellt werden.

Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.

```bash
azure servicefabric cluster connect
```

Verwenden Sie das in der Vorlage bereitgestellte Installationsskript, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

```bash
./install.sh
```

Navigieren Sie in einem Browser zu Service Fabric Explorer (http://localhost:19080/Explorer). Falls Sie Vagrant unter Mac OS X verwenden, ersetzen Sie „localhost“ durch die private IP-Adresse des virtuellen Computers. Erweitern Sie den Knoten „Anwendungen“. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.

Stellen Sie eine Verbindung mit dem ausgeführten Container her.  Öffnen Sie einen Webbrowser, und verweisen Sie auf die IP-Adresse, die über Port 4000 zurückgegeben wurde, z.B. „http://localhost:4000“. Die Überschrift „Hello World!“ wird im Browser angezeigt.

![Hello World!][hello-world]

## <a name="clean-up"></a>Bereinigen
Verwenden Sie das in der Vorlage bereitgestellte Deinstallationsskript, um die Anwendungsinstanz aus dem lokalen Entwicklungscluster zu löschen und die Registrierung des Anwendungstyps aufzuheben.

```bash
./uninstall.sh
```

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
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="myservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
      cluster, set InstanceCount to -1 for the container service to run on every node in 
      the cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
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

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png

