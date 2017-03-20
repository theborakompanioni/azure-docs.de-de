---
title: "Kontinuierliche Erstellung und Integration für Ihre Azure Service Fabric-Linux-Java-Anwendung mit Jenkins | Microsoft-Dokumentation"
description: "Kontinuierliche Erstellung und Integration für Ihre Linux-Java-Anwendung mit Jenkins"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 335f41e2e4a40e64e87382ea338673de3ab79c27
ms.lasthandoff: 03/14/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Verwenden Sie Jenkins, um Ihre Linux-Java-Anwendung zu erstellen und bereitzustellen.
Jenkins ist ein beliebtes Tool für Continuous Integration und Continuous Deployment. In diesem Dokument erfahren Sie Schritt für Schritt, wie Sie mithilfe von Jenkins Ihre Service Fabric-Anwendung erstellen und bereitstellen.

## <a name="general-prerequisites"></a>Allgemeine Voraussetzungen
1. Lokale Git-Installation. Eine passende Git-Version für Ihr Betriebssystem finden Sie [hier](https://git-scm.com/downloads). Falls Sie noch nicht mit Git vertraut sind, können Sie sich anhand der Schritte in der [Dokumentation](https://git-scm.com/docs) über Git informieren.
2. Sie benötigen das Service Fabric-Jenkins-Plug-In. Das Service Fabric-Jenkins-Plug-In können Sie [hier](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) herunterladen.

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Einrichten von Jenkins in einem Service Fabric-Cluster

### <a name="prerequisites"></a>Voraussetzungen
1. Sie benötigen einen Service Fabric-Linux-Cluster. Bei einem Service Fabric-Cluster, der über das Azure-Portal erstellt wird, ist Docker bereits installiert. Überprüfen Sie bei lokaler Ausführung des Clusters mithilfe des Befehls ``docker info``, ob Docker installiert ist. Falls nicht, installieren Sie Docker mit den folgenden Befehlen:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Stellen Sie die Service Fabric-Containeranwendung mit den folgenden Schritten im Cluster bereit:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker
cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080   # Azure CLI cluster connect command
bash Scripts/install.sh
```
Der Jenkins-Container wird im Cluster installiert und kann mit dem Service Fabric Explorer überwacht werden.

### <a name="steps"></a>Schritte
1. Rufen Sie in Ihrem Browser die URL ``http://PublicIPorFQDN:8081`` auf. Hierdurch erhalten Sie den Pfad des ursprünglichen Administratorkennworts, das für die Anmeldung erforderlich ist. Nach der Anmeldung mit dem ursprünglichen Administratorkonto können Sie Jenkins entweder weiterhin als Administratorbenutzer verwenden oder einen Benutzer erstellen und ändern.

  > [!NOTE]
  > Achten Sie bei der Clustererstellung darauf, dass der Port 8081 als Anwendungsendpunkt-Port angegeben ist.
  >

2. Rufen Sie mithilfe von ``docker ps -a`` die ID der Containerinstanz ab.
3. Melden Sie sich per SSH am Container an, und fügen Sie den Pfad aus dem Jenkins-Portal ein. Falls im Portal also beispielsweise der Pfad `PATH_TO_INITIAL_ADMIN_PASSWORD` angezeigt wird, können Sie Folgendes verwenden:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Richten Sie GitHub für Jenkins ein, indem Sie die Schritte unter [diesem Link](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) ausführen.
    * Generieren Sie mithilfe der Anweisungen von GitHub den SSH-Schlüssel, und fügen Sie ihn dem GitHub-Konto hinzu, unter dem Ihr Repository gehostet wird.
    * Führen Sie die unter dem obigen Link erwähnten Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
    * Verwenden Sie den folgenden Befehl, um sich über Ihren Host an der Jenkins-Shell anzumelden:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Einrichten von Jenkins außerhalb eines Service Fabric-Clusters

### <a name="prerequisites"></a>Voraussetzungen
Docker muss installiert sein. Sie können die folgenden Befehle verwenden, um Docker über das Terminal zu installieren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Wenn Sie nun ``docker info`` im Terminal ausführen, wird in der Ausgabe angezeigt, dass der Docker-Dienst ausgeführt wird.

### <a name="steps"></a>Schritte
  1. Rufen Sie das Service Fabric-Jenkins-Containerimage per Pullvorgang ab: ``docker pull servicefabric-microsoft.azurecr.io/jenkins:v1``
  2. Führen Sie das Containerimage aus: ``docker run -itd -p 8080:8080 servicefabric-microsoft.azurecr.io/jenkins:v1``
  3. Rufen Sie die ID der Containerimageinstanz ab. Mit dem Befehl ``docker ps –a`` können Sie eine Liste mit allen Docker-Containern anzeigen.
  4. Melden Sie sich am Jenkins-Portal an, indem Sie die folgenden Schritte ausführen:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Verwenden Sie beispielsweise „2d24“, wenn die Container-ID „2d24a73b5964“ lautet.
    * Dieses Kennwort wird für die Anmeldung beim Jenkins-Dashboard über das Portal (``http://<HOST-IP>:8080``) benötigt.
    * Nach der erstmaligen Anmeldung können Sie ein eigenes Benutzerkonto erstellen oder weiterhin das Administratorkonto verwenden. Wenn Sie einen Benutzer erstellen, müssen Sie mit diesem Benutzer fortfahren.
  5. Richten Sie GitHub für Jenkins ein, indem Sie die Schritte unter [diesem Link](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) ausführen.
      * Generieren Sie mithilfe der Anweisungen von GitHub den SSH-Schlüssel, und fügen Sie ihn dem GitHub-Konto hinzu, das zum Hosten des Repositorys dient.
      * Führen Sie die unter dem obigen Link beschriebenen Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
      * Verwenden Sie den folgenden Befehl, um sich über Ihren Host an der Jenkins-Shell anzumelden:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

    > [!NOTE]
    > Stellen Sie sicher, dass der Cluster/Computer, auf dem das Jenkins-Containerimage gehostet wird, über eine öffentlich zugängliche IP-Adresse verfügt, sodass Benachrichtigungen von GitHub von der Jenkins-Instanz empfangen werden.
    >

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>Installieren des Service Fabric-Jenkins-Plug-Ins über das Portal

1. Besuchen Sie ``http://PublicIPorFQDN:8081``.
2. Wählen Sie im Jenkins-Dashboard die Optionen **Manage Jenkins (Jenkins verwalten)** -> **Manage Plugins (Plug-Ins verwalten)** -> **Advanced (Erweitert)**.
Hier können Sie ein Plug-In hochladen. Wählen Sie die Option **Choose file** (Datei auswählen) und anschließend die Datei „serviceFabric.hpi“ aus, die Sie im Rahmen der Vorbereitung heruntergeladen haben. Nach dem Auswählen des Uploads wird das Plug-In von Jenkins automatisch installiert. Lassen Sie einen Neustart zu, falls die entsprechende Bestätigungsmeldung angezeigt wird.

## <a name="creating-and-configuring-a-jenkins-job"></a>Erstellen und Konfigurieren eines Jenkins-Auftrags

1. Erstellen eines **neuen Elements** über das Dashboard
2. Geben Sie einen Elementnamen ein (z.B. **MyJob**), wählen Sie ein Freestyleprojekt aus, und klicken Sie auf „OK“.
3. Navigieren Sie anschließend zur Auftragsseite, und klicken Sie auf **Configure** - (Konfigurieren).
  * Geben Sie im allgemeinen Abschnitt unter **GitHub project** (GitHub-Projekt) Ihre GitHub-Projekt-URL an, unter der die Service Fabric-Java-Anwendung gehostet wird, die Sie in den Jenkins-CI/CD-Flow integrieren möchten (z.B. ``https://github.com/sayantancs/SFJenkins``).
  * Wählen Sie im Abschnitt **Quellcodeverwaltung** die Option **Git**. Geben Sie die Repository-URL an, unter der die Service Fabric-Java-Anwendung gehostet wird, die Sie in den Jenkins-CI/CD-Flow integrieren möchten (z.B. ``https://github.com/sayantancs/SFJenkins.git``). Hier können Sie auch die zu erstellende Verzweigung angeben, z.B. **/master**.
4. Konfigurieren Sie Ihre *GitHub*-Instanz (in der das Repository gehostet wird), um die Kommunikation mit Jenkins zu ermöglichen. Führen Sie hierzu die folgenden Schritte aus:
  1. Wechseln Sie zur Seite Ihres GitHub-Repositorys. Navigieren Sie zu **Settings (Einstellungen)** -> **Integrations and Services (Integrationen und Dienste)**.
  2. Wählen Sie **Add Service** (Dienst hinzufügen), geben Sie „Jenkins“ ein, und wählen Sie das **Jenkins-Github-Plug-In** aus.
  3. Geben Sie Ihre Jenkins-Webhook-URL ein. (Diese lautet standardmäßig ``http://<PublicIPorFQDN>:8081/github-webhook/``.) Klicken Sie auf die Option zum Hinzufügen/Aktualisieren des Diensts.
  4. An die Jenkins-Instanz wird ein Testereignis gesendet. Für den Webhook in GitHub sollte ein grünes Häkchen angezeigt werden, und das Projekt wird erstellt.
  5. Wählen Sie im Abschnitt **Build Triggers** (Trigger erstellen) die gewünschten Erstellungsoptionen aus. In diesem Szenario soll bei jedem Pushvorgang in das Repository ein Buildvorgang ausgelöst werden. Die entsprechende Option hierfür ist **GitHub hook trigger for GITScm polling** (GitHub-Hooktrigger für GITScm-Abruf). Vorher war dies „Build when a change is pushed to GitHub“ (Erstellen, wenn eine Änderung mittels Push an GitHub übertragen wird).
  6. Wählen Sie im Abschnitt **Build** in der Dropdownliste **Buildschritt hinzufügen** die Option **Invoke Gradle Script** (Gradle-Skript aufrufen). Geben Sie im daraufhin angezeigten Widget den Pfad zu **Root build script** (Stammerstellungsskript) für Ihre Anwendung an. „build.gradle“ wird an dem angegebenen Pfad abgerufen und entsprechend verwendet. Wenn Sie mithilfe des Eclipse-Plug-Ins oder des Yeoman-Generators ein Projekt namens ``MyActor`` erstellen, muss das Stammerstellungsskript Folgendes enthalten: ``${WORKSPACE}/MyActor``. Der Abschnitt sollte in etwa wie folgt aussehen:

    ![Service Fabric-Jenkins-Buildvorgang][build-step]
  7. Wählen Sie in der Dropdownliste **Post-Build Actions** (Aktionen nach dem Erstellen) die Option **Deploy Service Fabric Project** (Service Fabric-Projekt bereitstellen). In den Clusterdetails muss angegeben werden, wo die von Jenkins kompilierte Service Fabric-Anwendung bereitgestellt wird. Geben Sie außerdem zusätzliche Anwendungsdetails für die Anwendungsbereitstellung an. Orientieren Sie sich dabei am folgenden Screenshot:

    ![Service Fabric-Jenkins-Buildvorgang][post-build-step]

 > [!NOTE]
 > Hier kann der gleiche Cluster angegeben werden, der auch als Host für die Jenkins-Containeranwendung fungiert, falls Sie das Jenkins-Containerimage mithilfe von Service Fabric bereitstellen.
 >

### <a name="end-to-end-scenario"></a>End-to-End-Szenario
GitHub und Jenkins sind nun konfiguriert. Als Nächstes können Sie Beispieländerungen an Ihrem ``MyActor``-Projekt im Repositorybeispiel unter „https://github.com/sayantancs/SFJenkins“ vornehmen und die Änderungen mittels Push an die Remoteverzweigung ``master`` übermitteln (oder an eine andere Verzweigung, die Sie für die Verwendung konfiguriert haben). Daraufhin wird nun der von Ihnen konfigurierte Jenkins-Auftrag ``MyJob`` ausgelöst. Dieser ruft im Wesentlichen die Änderungen von GitHub ab, erstellt sie und stellt die Anwendung für den Clusterendpunkt bereit, den Sie in Aktionen nach der Erstellung angegeben haben.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

