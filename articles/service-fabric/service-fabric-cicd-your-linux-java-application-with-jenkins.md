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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 71e3d130f22515d22dc7f486f3dede936b874049
ms.lasthandoff: 03/25/2017


---
# <a name="use-jenkins-to-build-and-deploy-your-linux-java-application"></a>Verwenden von Jenkins zum Erstellen und Bereitstellen Ihrer Linux-Java-Anwendung
Jenkins ist ein beliebtes Tool für Continuous Integration und Continuous Deployment für Ihre Apps. Hier wird beschrieben, wie Sie Ihre Azure Service Fabric-Anwendung mit Jenkins erstellen und bereitstellen.

## <a name="general-prerequisites"></a>Allgemeine Voraussetzungen
- Lokale Git-Installation. Sie können die richtige Git-Version über die [Git-Downloadseite](https://git-scm.com/downloads) für Ihr Betriebssystem installieren. Wenn Git neu für Sie ist, hilft Ihnen die [Git-Dokumentation](https://git-scm.com/docs) weiter.
- Sie benötigen das Service Fabric-Jenkins-Plug-In. Der Download ist auf der Seite mit den [Service Fabric-Downloads](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) möglich.

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Einrichten von Jenkins in einem Service Fabric-Cluster

Sie können Jenkins innerhalb oder außerhalb eines Service Fabric-Clusters einrichten. In den folgenden Abschnitten wird gezeigt, wie Sie die Einrichtung in einem Cluster durchführen.

### <a name="prerequisites"></a>Voraussetzungen
1. Sie benötigen einen Service Fabric-Linux-Cluster. Für einen Service Fabric-Cluster, der über das Azure-Portal erstellt wird, ist Docker bereits installiert. Wenn Sie den Cluster lokal ausführen, können Sie mit dem Befehl ``docker info`` prüfen, ob Docker installiert ist. Falls nicht, können Sie es mit den folgenden Befehlen entsprechend installieren:

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
Im Cluster wird ein Jenkins-Container installiert und kann mit dem Service Fabric Explorer überwacht werden.

### <a name="steps"></a>Schritte
1. Navigieren Sie im Browser zu ``http://PublicIPorFQDN:8081``. Hierdurch erhalten Sie den Pfad des ursprünglichen Administratorkennworts, das für die Anmeldung erforderlich ist. Sie können Jenkins weiterhin als Administratorbenutzer verwenden. Oder Sie können den Benutzer erstellen und ändern, nachdem Sie sich mit dem ursprünglichen Administratorkonto angemeldet haben.

   > [!NOTE]
   > Stellen Sie sicher, dass Port 8081 als Anwendungsendpunkt-Port angegeben ist, während Sie den Cluster erstellen.
   >

2. Rufen Sie mithilfe von ``docker ps -a`` die ID der Containerinstanz ab.
3. Melden Sie sich per SSH (Secure Shell) am Container an, und fügen Sie den Pfad aus dem Jenkins-Portal ein. Führen Sie beispielsweise Folgendes aus, wenn im Portal der Pfad `PATH_TO_INITIAL_ADMIN_PASSWORD` angezeigt wird:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Richten Sie GitHub für Jenkins ein, indem Sie die Schritte unter [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generieren und Hinzufügen eines neuen SSH-Schlüssels zum SSH-Agent) ausführen.
    * Generieren Sie mithilfe der Anweisungen von GitHub den SSH-Schlüssel, und fügen Sie ihn dem GitHub-Konto hinzu, unter dem Ihr Repository gehostet wird.
    * Führen Sie die unter dem obigen Link beschriebenen Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
    * Verwenden Sie den folgenden Befehl, um sich über Ihren Host an der Jenkins-Shell anzumelden:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Einrichten von Jenkins außerhalb eines Service Fabric-Clusters

Sie können Jenkins innerhalb oder außerhalb eines Service Fabric-Clusters einrichten. In den folgenden Abschnitten wird gezeigt, wie Sie die Einrichtung außerhalb eines Clusters durchführen.

### <a name="prerequisites"></a>Voraussetzungen
Docker muss installiert sein. Sie können die folgenden Befehle verwenden, um Docker über das Terminal zu installieren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Wenn Sie nun ``docker info`` im Terminal ausführen, wird in der Ausgabe angezeigt, dass der Docker-Dienst ausgeführt wird.

### <a name="steps"></a>Schritte
  1. Rufen Sie das Service Fabric-Jenkins-Containerimage per Pullvorgang ab: ``docker pull raunakpandya/jenkins:v1``
  2. Führen Sie das Containerimage aus: ``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Rufen Sie die ID der Containerimageinstanz ab. Mit dem Befehl ``docker ps –a`` können Sie eine Liste mit allen Docker-Containern anzeigen.
  4. Melden Sie sich mit den folgenden Schritten am Jenkins-Portal an:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Verwenden Sie beispielsweise „2d24“, wenn die Container-ID „2d24a73b5964“ lautet.
    * Dieses Kennwort wird für die Anmeldung beim Jenkins-Dashboard über das Portal (``http://<HOST-IP>:8080``) benötigt.
    * Nach der erstmaligen Anmeldung können Sie ein eigenes Benutzerkonto erstellen oder weiterhin das Administratorkonto verwenden. Wenn Sie einen Benutzer erstellen, müssen Sie mit diesem Benutzer fortfahren.
  5. Richten Sie GitHub für Jenkins ein, indem Sie die Schritte unter [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generieren und Hinzufügen eines neuen SSH-Schlüssels zum SSH-Agent) ausführen.
        * Generieren Sie mithilfe der Anweisungen von GitHub den SSH-Schlüssel, und fügen Sie ihn dem GitHub-Konto hinzu, unter dem das Repository gehostet wird.
        * Führen Sie die unter dem obigen Link beschriebenen Befehle in der Jenkins-Docker-Shell aus (nicht auf Ihrem Host).
        * Verwenden Sie die folgenden Befehle, um sich über Ihren Host an der Jenkins-Shell anzumelden:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Stellen Sie sicher, dass der Cluster oder der Computer, in bzw. auf dem das Jenkins-Containerimage gehostet wird, über eine öffentliche IP-Adresse verfügt. Die Jenkins-Instanz kann dann Benachrichtigungen von GitHub empfangen.

## <a name="install-the-service-fabric-jenkins-plug-in-from-the-portal"></a>Installieren des Service Fabric-Jenkins-Plug-Ins über das Portal

1. Besuchen Sie ``http://PublicIPorFQDN:8081``.
2. Wählen Sie im Jenkins-Dashboard die Optionen **Manage Jenkins (Jenkins verwalten)** > **Manage Plugins (Plug-Ins verwalten)** > **Advanced (Erweitert)**.
Hier können Sie ein Plug-In hochladen. Wählen Sie die Option **Choose file** (Datei auswählen) und anschließend die Datei **serviceFabric.hpi** aus, die Sie im Rahmen der Vorbereitung heruntergeladen haben. Nach dem Auswählen von **Upload** wird das Plug-In von Jenkins automatisch installiert. Lassen Sie einen Neustart zu, falls die entsprechende Bestätigungsmeldung angezeigt wird.

## <a name="create-and-configure-a-jenkins-job"></a>Erstellen und Konfigurieren eines Jenkins-Auftrags

1. Erstellen Sie über das Dashboard ein **neues Element**.
2. Geben Sie einen Elementnamen ein (z.B. **MyJob**). Wählen Sie die Option **free-style project** (Freestyleprojekt), und klicken Sie auf **OK**.
3. Navigieren Sie zur Auftragsseite, und klicken Sie auf **Configure** (Konfigurieren).

   a. Geben Sie im Abschnitt „General“ (Allgemein) unter **GitHub project** (GitHub-Projekt) die URL Ihres GitHub-Projekts ein. Mit dieser URL wird die Service Fabric-Java-Anwendung gehostet, die Sie in den Jenkins-CI/CD-Flow (Continuous Integration, Continuous Deployment) integrieren möchten (z.B. ``https://github.com/sayantancs/SFJenkins``).

   b. Wählen Sie im Abschnitt **Quellcodeverwaltung** die Option **Git**. Geben Sie die Repository-URL an, unter der die Service Fabric-Java-Anwendung gehostet wird, die Sie in den Jenkins-CI/CD-Flow integrieren möchten (z.B. ``https://github.com/sayantancs/SFJenkins.git``). Hier können Sie auch die zu erstellende Verzweigung angeben (z.B. **/master**).
4. Konfigurieren Sie Ihre *GitHub*-Instanz (in der das Repository gehostet wird), um die Kommunikation mit Jenkins zu ermöglichen. Führen Sie die folgenden Schritte aus:

   a. Wechseln Sie zur Seite Ihres GitHub-Repositorys. Navigieren Sie zu **Settings (Einstellungen)** > **Integrations and Services (Integrationen und Dienste)**.

   b. Wählen Sie **Add Service** (Dienst hinzufügen), geben Sie **Jenkins** ein, und wählen Sie das **Jenkins-GitHub-Plug-In** aus.

   c. Geben Sie Ihre Jenkins-Webhook-URL ein. (Diese lautet standardmäßig ``http://<PublicIPorFQDN>:8081/github-webhook/``.) Klicken Sie auf die Option zum Hinzufügen/Aktualisieren des Diensts****.

   d. An die Jenkins-Instanz wird ein Testereignis gesendet. Für den Webhook in GitHub sollte ein grünes Häkchen angezeigt werden, und das Projekt sollte erstellt werden.

   e. Wählen Sie unter dem Abschnitt **Build Triggers** (Buildtrigger) die gewünschte Buildoption. In diesem Beispiel soll jeweils ein Build ausgelöst werden, wenn ein Pushvorgang zum Repository durchgeführt wird. Sie wählen also die Option **GitHub hook trigger for GITScm polling** (GitHub-Hooktrigger für GITScm-Abruf). (Früher hatte diese Option die Bezeichnung **Build when a change is pushed to GitHub** (Build bei Push einer Änderung auf GitHub durchführen).)

   f. Wählen Sie im Abschnitt **Build** in der Dropdownliste **Buildschritt hinzufügen** die Option **Invoke Gradle Script** (Gradle-Skript aufrufen). Geben Sie im daraufhin angezeigten Widget den Pfad zu **Root build script** (Stammerstellungsskript) für Ihre Anwendung an. „build.gradle“ wird am angegebenen Pfad abgerufen und entsprechend verwendet. Wenn Sie mithilfe des Eclipse-Plug-Ins oder des Yeoman-Generators ein Projekt namens ``MyActor`` erstellen, muss das Stammerstellungsskript Folgendes enthalten: ``${WORKSPACE}/MyActor``. Der folgende Screenshot enthält hierzu ein Beispiel:

    ![Service Fabric-Jenkins-Buildvorgang][build-step]

   g. Wählen Sie in der Dropdownliste **Post-Build Actions** (Aktionen nach dem Erstellen) die Option **Deploy Service Fabric Project** (Service Fabric-Projekt bereitstellen). Hier müssen Sie Clusterdetails dazu angeben, wo die von Jenkins kompilierte Service Fabric-Anwendung bereitgestellt werden soll. Sie können auch zusätzliche Anwendungsdetails angeben, die zum Bereitstellen der Anwendung verwendet werden. Der folgende Screenshot enthält hierzu ein Beispiel:

    ![Service Fabric-Jenkins-Buildvorgang][post-build-step]

   > [!NOTE]
   > Hier kann der gleiche Cluster angegeben werden, der auch als Host für die Jenkins-Containeranwendung fungiert, falls Sie das Jenkins-Containerimage mithilfe von Service Fabric bereitstellen.
   >

## <a name="next-steps"></a>Nächste Schritte
GitHub und Jenkins sind jetzt konfiguriert. Sie können erwägen, in Ihrem ``MyActor``-Projekt im Repositorybeispiel unter „https://github.com/sayantancs/SFJenkins“ einige Änderungen vorzunehmen. Senden Sie Ihre Änderungen per Pushvorgang an eine ``master``-Remoteverzweigung (oder eine beliebige andere Verzweigung, die Sie zur Verwendung konfiguriert haben). Hierdurch wird der Jenkins-Auftrag ``MyJob`` ausgelöst, den Sie konfiguriert haben. Die Änderungen werden von GitHub abgerufen, die Erstellung wird durchgeführt, und die Anwendung wird für den Clusterendpunkt bereitgestellt, den Sie in Aktionen nach der Erstellung angegeben haben.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

