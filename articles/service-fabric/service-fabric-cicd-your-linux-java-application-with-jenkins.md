---
title: "Kontinuierliche Erstellung und Bereitstellung für Ihre Linux-Java-Anwendung mithilfe von Jenkins | Microsoft-Dokumentation"
description: "Kontinuierliche Erstellung und Bereitstellung für Ihre Linux-Java-Anwendung mithilfe von Jenkins"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: raunakp
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: sayantancs
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: d3c64e3ed78a5379592b5e4276c83d26f04d2e1c
ms.lasthandoff: 03/08/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Verwenden Sie Jenkins, um Ihre Linux-Java-Anwendung zu erstellen und bereitzustellen.
Jenkins ist ein beliebtes Tool für Continuous Integration und Continuous Deployment. In diesem Dokument erfahren Sie Schritt für Schritt, wie Sie mithilfe von Jenkins Ihre Service Fabric-Anwendung erstellen und bereitstellen.

## <a name="general-prerequisites"></a>Allgemeine Voraussetzungen
1. Sie benötigen eine lokale Git-Installation. Eine passende Git-Version für Ihr Betriebssystem finden Sie [hier](https://git-scm.com/downloads). Falls Sie noch nicht mit Git vertraut sind, können Sie sich anhand der Schritte in den [Dokumentationen](https://git-scm.com/docs) mit Git vertraut machen.
2. Sie benötigen das Service Fabric-Jenkins-Plug-In. Das Service Fabric-Jenkins-Plug-In können Sie [hier](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) herunterladen.

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Einrichten von Jenkins in einem Service Fabric-Cluster

### <a name="prerequisites"></a>Voraussetzungen
1. Sie benötigen einen Service Fabric-Linux-Cluster. Bei Service Fabric-Clustern, die über das Azure-Portal erstellt werden, ist bereits Docker installiert. Überprüfen Sie bei Verwendung des lokalen Clusters mithilfe des Befehls ``docker info``, ob Docker installiert ist. Falls nicht, installieren Sie Docker mithilfe des folgenden Befehls:
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
2. Die Service Fabric-Containeranwendung muss im Cluster ``http://PublicIPorFQDN:19080`` bereitgestellt sein. Sie können folgende Schritte ausführen:
```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker && cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080 # Azure CLI cluster connect command
bash Scripts/install.sh
```
  Dadurch wird der Jenkins-Container in dem Cluster installiert, mit dem Sie eine Verbindung hergestellt haben. Die Containeranwendung kann mithilfe von Service Fabric Explorer überwacht werden.

### <a name="steps"></a>Schritte
1. Rufen Sie in Ihrem Browser die URL ``http://PublicIPorFQDN:8081`` auf. Dadurch erhalten Sie den Pfad des ursprünglichen, für die Anmeldung erforderlichen Administratorkennworts. Nach der Anmeldung mit dem ursprünglichen Administratorkonto können Sie Jenkins entweder weiterhin als Administratorbenutzer verwenden oder den Benutzer erstellen und ändern.
> [!NOTE]
> Achten Sie bei der Clustererstellung darauf, dass der Port 8081 als Anwendungsendpunkt-Port angegeben ist.
>

2. Rufen Sie mithilfe von ``docker ps -a`` die ID der Containerinstanz ab.
3. Melden Sie sich per SSH bei dem Container an, und fügen Sie den Pfad aus dem Jenkins-Portal ein. Falls im Portal also beispielsweise der Pfad `PATH_TO_INITIAL_ADMIN_PASSWORD` angezeigt wird, können Sie Folgendes verwenden:  
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash # This takes you inside docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Richten Sie GitHub [hiermit](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) für die Zusammenarbeit mit Jenkins ein.
    * Generieren Sie mithilfe der Anweisungen von GitHub den SSH-Schlüssel, und fügen Sie ihn dem GitHub-Konto hinzu, das zum Hosten des Repositorys dient.
    * Führen Sie die unter dem obigen Link erwähnten Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
    * Wenn Sie sich von Ihrem Host aus bei der Jenkins-Shell anmelden möchten, verwenden Sie Folgendes:
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Einrichten von Jenkins außerhalb eines Service Fabric-Clusters

### <a name="prerequisites"></a>Voraussetzungen
1. Docker muss installiert sein. Geben Sie andernfalls die folgenden Befehle in das Terminal ein, um Docker zu installieren.
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
Wenn Sie nun ``docker info`` im Terminal ausführen, wird in der Ausgabe angezeigt, dass der Docker-Dienst ausgeführt wird.

### <a name="steps"></a>Schritte
  1. Führen Sie für den Service Fabric-Jenkins-Container einen Pullvorgang aus: ``docker pull IMAGE_NAME ``
  2. Führen Sie das Containerimage aus: ``docker run -itd -p 8080:8080 IMAGE_NAME``
  3. Rufen Sie die ID Ihres Docker-Containers mit dem Jenkins-Image ab, das Sie soeben installiert haben. Mit dem folgenden Befehl können Sie eine Liste mit allen Docker-Containern anzeigen: ``docker ps –a``
  4. Rufen Sie das Administratorkennwort für das Jenkins-Konto ab. Hierzu können Sie Folgendes verwenden:
    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Hierbei gilt: Falls die Container-ID „2d24a73b5964“ lautet, müssen Sie lediglich „2d24“ einfügen.
    * Dieses Kennwort wird für die Anmeldung beim Jenkins-Dashboard über das Portal (``http://<HOST-IP>:8080``) benötigt.
    * Nach der erstmaligen Anmeldung können Sie ein eigenes Benutzerkonto erstellen oder weiterhin das Administratorkonto verwenden. Wenn Sie einen neuen Benutzer erstellen, müssen Sie mit diesem Benutzer fortfahren.
  5. Richten Sie GitHub [hiermit](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) für die Zusammenarbeit mit Jenkins ein.
      * Generieren Sie mithilfe der Anweisungen von GitHub den SSH-Schlüssel, und fügen Sie ihn dem GitHub-Konto hinzu, das zum Hosten des Repositorys dient.
      * Führen Sie die unter dem obigen Link erwähnten Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
      * Wenn Sie sich von Ihrem Host aus bei der Jenkins-Shell anmelden möchten, verwenden Sie Folgendes:
    ```sh
    docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
    ```

> [!NOTE]
> Stellen Sie sicher, dass der Cluster/Computer, auf dem das Jenkins-Containerimage gehostet wird, über eine öffentlich zugängliche IP-Adresse verfügt, sodass Benachrichtigungen von GitHub von der Jenkins-Instanz empfangen werden.
>

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>Installieren des Service Fabric-Jenkins-Plug-Ins über das Portal

1. Besuchen Sie ``http://PublicIPorFQDN:8081``.
2. Wählen Sie auf dem Jenkins-Dashboard Folgendes aus: ``Manage Jenkins`` -> ``Manage Plugins`` -> ``Advanced``.
Hier können Sie ein Plug-In hochladen. Wählen Sie die Option ``Choose file`` und anschließend die Datei „serviceFabric.hpi“ aus, die Sie im Rahmen der Vorbereitung heruntergeladen haben. Nachdem Sie die Option zum Hochladen ausgewählt haben, wird das Plug-In automatisch für Sie installiert. Unter Umständen ist ein Neustart erforderlich. Lassen Sie diesen zu.

## <a name="creating-and-configuring-a-jenkins-job"></a>Erstellen und Konfigurieren eines Jenkins-Auftrags

1. Erstellen Sie über das Dashboard ein neues Element (``new item``).
2. Geben Sie einen neuen Elementnamen (etwa ``MyJob``) ein, wählen Sie ein Freestyleprojekt aus, und klicken Sie auf „OK“.
3. Navigieren Sie anschließend zur Auftragsseite, und klicken Sie auf ``Configure`` -.
  * Geben Sie im allgemeinen Abschnitt unter ``Github project`` Ihre GitHub-Projekt-URL an, unter der die Service Fabric-Java-Anwendung gehostet wird, die Sie in den Jenkins-CI/CD-Flow integrieren möchten (beispielsweise ``https://github.com/sayantancs/SFJenkins``).
  * Wählen Sie im Abschnitt ``Source Code Management`` die Option ``Git`` aus. Geben Sie die Repository-URL an, unter der die Service Fabric-Java-Anwendung gehostet wird, die Sie in den Jenkins-CI/CD-Flow integrieren möchten (beispielsweise ``https://github.com/sayantancs/SFJenkins.git``). Hier können Sie auch die zu erstellende Verzweigung angeben (beispielsweise ``*/master``).
4. Konfigurieren Sie Ihre *GitHub*-Instanz (in der das Repository gehostet wird), um die Kommunikation mit Jenkins zu ermöglichen. Führen Sie hierzu die folgenden Schritte aus:
  - Wechseln Sie zur Seite Ihres GitHub-Repositorys. Navigieren Sie zu ``Settings`` -> ``Integrations and Services``.
  - Wählen Sie ``Add Service`` aus, geben Sie „Jenkins“ ein, und wählen das ``Jenkins-Github plugin`` aus.
  - Geben Sie Ihre Jenkins-Webhook-URL ein. (Diese lautet standardmäßig ``http://<PublicIPorFQDN>:8081/github-webhook/``.) Klicken Sie auf die Option zum Hinzufügen/Aktualisieren des Diensts.
  - An die Jenkins-Instanz wird ein Testereignis gesendet. Für den Webhook in GitHub sollte ein grünes Häkchen angezeigt werden, und das Projekt wird erstellt.
  - Wählen Sie im Abschnitt ``Build Triggers`` die gewünschten Erstellungsoptionen aus. In diesem Szenario soll bei jedem Pushvorgang in das Repository ein Buildvorgang ausgelöst werden. Die entsprechende Option hierfür ist ``GitHub hook trigger for GITScm polling``. Vorher war es „Build when a change is pushed to GitHub“ (Erstellen, wenn eine Änderung mittels Push an GitHub übertragen wird).
  - Wählen Sie unter ``Build section`` in der Dropdownliste ``Add build step`` die Option ``Invoke Gradle Script`` aus. Geben Sie im daraufhin angezeigten Widget den Pfad zu ``Root build script`` für Ihre Anwendung an. „build.gradle“ wird an dem angegebenen Pfad abgerufen und entsprechend verwendet. Wenn Sie mithilfe des Eclipse-Plug-Ins oder des Yeoman-Generators ein Projekt namens ``MyActor`` erstellen, muss das Stammerstellungsskript Folgendes enthalten: ``${WORKSPACE}/MyActor``. Der Abschnitt sollte in etwa wie folgt aussehen:

    ![Service Fabric-Jenkins-Buildvorgang][build-step]
  - Wählen Sie in der Dropdownliste ``Post-Build Actions`` die Option ``Deploy Service Fabric Project`` aus. In den Clusterdetails muss angegeben werden, wo die von Jenkins kompilierte Service Fabric-Anwendung bereitgestellt wird. Geben Sie außerdem zusätzliche Anwendungsdetails für die Anwendungsbereitstellung an. Orientieren Sie sich dabei am folgenden Screenshot:

    ![Service Fabric-Jenkins-Buildvorgang][post-build-step]

 >[!Note]
 > Hier kann der gleiche Cluster angegeben werden, der auch als Host für die Jenkins-Containeranwendung fungiert, falls Sie das Jenkins-Containerimage mithilfe von Service Fabric bereitstellen.
 >

### <a name="end-to-end-scenario"></a>End-to-End-Szenario
GitHub und Jenkins sind nun konfiguriert. Als Nächstes können Sie Beispieländerungen an Ihrem ``MyActor``-Projekt im Repository (beispielsweise „https://github.com/sayantancs/SFJenkins“) vornehmen und die Änderungen mittels Push an die Remoteverzweigung ``master`` (oder an eine andere Verzweigung, die Sie zur Verwendung konfiguriert haben) übermitteln. Daraufhin wird nun der von Ihnen konfigurierte Jenkins-Auftrag ``MyJob`` ausgelöst. Dieser ruft im Wesentlichen die Änderungen von GitHub ab, erstellt sie und stellt die Anwendung für den Clusterendpunkt bereit, den Sie in Aktionen nach der Erstellung angegeben haben.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

