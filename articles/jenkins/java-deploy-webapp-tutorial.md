---
title: Verwenden von Jenkins zum Bereitstellen Ihrer Web-Apps in Azure | Microsoft-Dokumentation
description: "Richten Sie Continuous Integration aus GitHub in Azure App Service für Ihre Java-Web-Apps mithilfe von Jenkins und Docker ein."
author: rloutlaw
manager: douge
ms.service: jenkins
ms.search.scope: 
ms.devlang: java
ms.topic: article
ms.workload: web
ms.date: 08/02/2017
ms.author: routlaw
ms.custom: Jenkins
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 22288cd1468b410df77b27721ccda32c3d033e47
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---

# <a name="set-up-continuous-integration-and-deployment-to-azure-app-service-with-jenkins"></a>Einrichten von Continuous Integration und Continuous Deployment in Azure App Service mit Jenkins

Dieses Tutorial richtet Continuous Integration und Continuous Deployment (CI/CD) einer Java-Beispiel-Web-App, die mit dem [Spring Boot](http://projects.spring.io/spring-boot/)-Framework entwickelt wurde, mithilfe von Jenkins in einer [Azure App Service-Web-App unter Linux](/azure/app-service-web/app-service-linux-intro) ein.

In diesem Tutorial führen Sie die folgenden Aufgaben aus:

> [!div class="checklist"]
> * Installieren der Jenkins-Plug-Ins, die für die Bereitstellung in Azure App Service benötigt werden
> * Definieren eines Jenkins-Auftrags zum Erstellen von Docker-Images aus einem GitHub-Repository, wenn ein neuer Commit gepusht wird
> * Definieren einer neuen Azure-Web-App für Linux und Konfigurieren der App für die Bereitstellung von Docker-Images, die in die Azure-Containerregistrierung gepusht wurden
> * Konfigurieren des Jenkins-Plug-Ins für Azure App Service
> * Bereitstellen der Beispielanwendung in Azure App Service mit einem manuellen Build
> * Auslösen eines Jenkins-Builds und Aktualisieren der Web-App durch Pushen von Änderungen an GitHub

## <a name="before-you-begin"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Jenkins](https://jenkins.io/) mit konfigurierten Tools JDK und Maven. Wenn Sie kein Jenkins-System besitzen, erstellen Sie jetzt eins in Azure mithilfe der [Jenkins-Lösungsvorlage](/azure/jenkins/install-jenkins-solution-template).
* Ein [GitHub](https://github.com)-Konto.
* [Azure CLI 2.0](/cli/azure/overview), entweder über Ihre lokale Befehlszeilenschnittstelle oder in der [Azure Cloud Shell](/azure/cloud-shell/overview).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-jenkins-plug-ins"></a>Installieren von Jenkins-Plug-Ins

1. Öffnen Sie Ihre Jenkins-Webkonsole in einem Webbrowser, und wählen Sie im linken Menü die Option **Manage Jenkins** und dann die Option **Manage Plugins** aus.
2. Wählen Sie die Registerkarte **Available** aus.
3. Aktivieren Sie die Kontrollkästchen neben den folgenden Plug-Ins:   

    - [Azure App Service Plug-In](https://plugins.jenkins.io/azure-app-service)
    - [GitHub Branch Source Plug-In](https://plugins.jenkins.io/github-branch-source)

    Wenn die Plug-Ins nicht angezeigt werden, überprüfen Sie auf der Registerkarte **Installed**, ob sie bereits installiert sind.

1. Wählen Sie **Download now and install after restart** aus, um die Plug-Ins in Ihrer Jenkins-Konfiguration zu aktivieren.

## <a name="configure-github-and-jenkins"></a>Konfigurieren von GitHub und Jenkins

Richten Sie Jenkins so ein, dass [GitHub-Webhooks](https://developer.github.com/webhooks/) empfangen werden, wenn neue Commits in ein Repository in Ihrem Konto gepusht werden.

1. Wählen Sie **Manage Jenkins** und dann **Configure System** aus. Stellen Sie im Abschnitt **GitHub** sicher, dass **Manage hooks** ausgewählt ist, und wählen Sie dann **Manage additional GitHub actions** und anschließend **Convert login and password to token** aus.
2. Aktivieren Sie das Optionsfeld **From login and password**, und geben Sie Ihren Benutzernamen und Ihr Kennwort für GitHub ein. Wählen Sie **Create token credentials** aus, um ein neues [GitHub Personal Access Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) zu erstellen.   
   ![Erstellen eines persönlichen Zugriffstokens für GitHub aus dem Benutzernamen und dem Kennwort](media/jenkins-java-quickstart/create_github_credentials.png)
3.  Wählen Sie in der Dropdownliste **Credentials** in der Konfiguration der GitHub-Server das neu erstellte Token aus. Wählen Sie **Test connection** aus, um zu überprüfen, ob die Authentifizierung funktioniert.   
   ![Überprüfen der Verbindung mit GitHub nach Konfiguration des persönlichen Zugriffstokens](media/jenkins-java-quickstart/verify_github_connection.png)

> [!NOTE]
> Wenn für Ihr GitHub-Konto die zweistufige Authentifizierung aktiviert ist, erstellen Sie in GitHub ein Token und konfigurieren Sie Jenkins für die Verwendung des Tokens. Weitere Einzelheiten finden Sie in der Dokumentation zum [Jenkins GitHub-Plug-In](https://wiki.jenkins.io/display/JENKINS/Github+Plugin).

## <a name="fork-the-sample-repo-and-create-a-jenkins-job"></a>Forken des Beispielrepositorys und Erstellen eines Jenkins-Auftrags 

1. Öffnen Sie das [Spring Boot-Beispielanwendungsrepository](https://github.com/spring-guides/gs-spring-boot-docker), und forken Sie es in Ihr eigenes GitHub-Konto, indem Sie in der oberen rechten Ecke **Fork** auswählen.   
    ![Forken aus GitHub](media/jenkins-java-quickstart/fork_github_repo.png)
1. Wählen Sie in der Jenkins-Webkonsole **New Item**, benennen Sie das Element **MyJavaApp** und wählen Sie **Freestyle project** und dann **OK** aus.   
    ![Neues Jenkins Freestyle Project](media/jenkins-java-quickstart/jenkins_freestyle.png)
2. Wählen Sie im Abschnitt **Allgemein** als Projekt **GitHub** aus, und geben Sie die URL Ihres geforkten Repositorys ein, z.B. https://github.com/raisa/gs-spring-boot-docker.
3. Wählen Sie im Abschnitt **Source code management** die Option **Git** aus, und geben Sie die `.git`-URL Ihres geforkten Repositorys ein, z.B. https://github.com/raisa/gs-spring-boot-docker.git.
4. Wählen Sie im Bereich **Build Triggers** (Trigger erstellen) die Option **GitHub hook trigger for GITScm polling** (GitHub-Hooktrigger für GITScm-Abruf) aus.
5. Wählen Sie im Abschnitt **Build** die Option **Add build step** und dann **Invoke top-level Maven targets** aus. Geben Sie `package` in das Feld **Goals** ein.
6. Wählen Sie **Speichern** aus. Sie können Ihren Auftrag testen, indem Sie auf der Projektseite **Build Now** auswählen.

## <a name="configure-azure-app-service"></a>Konfigurieren von Azure App Service 

1. Erstellen Sie mithilfe der Azure CLI oder der [Cloud Shell](/azure/cloud-shell/overview) eine neue [Web-App unter Linux](/azure/app-service-web/app-service-linux-intro). In diesem Tutorial lautete der Name der Web-App `myJavaApp`, für Ihre eigene App müssen Sie allerdings einen eigenen eindeutigen Namen verwenden.
   
    ```azurecli-interactive
    az group create --name myResourceGroupJenkins --location westus
    az appservice plan create --is-linux --name myLinuxAppServicePlan --resource-group myResourceGroupJenkins 
    az webapp create --name myJavaApp --resource-group myResourceGroupJenkins --plan myLinuxAppServicePlan
    ```

2. Erstellen Sie eine [Azure-Containerregistrierung](/azure/container-registry/container-registry-intro), um die von Jenkins erstellten Docker-Images zu speichern. Der in diesem Tutorial verwendete Name der Containerregistrierung lautet `jenkinsregistry`, für Ihre eigene Containerregistrierung müssen Sie allerdings einen eigenen eindeutigen Namen verwenden. 

    ```azurecli-interactive
    az acr create --name jenkinsregistry --resource-group myResourceGroupJenkins --sku Basic --admin-enabled
    ```
3. Konfigurieren Sie die Web-App so, dass Docker-Images ausgeführt werden, die in die Containerregistrierung gepusht wurden, und geben Sie an, dass die im Container ausgeführte App an Port 8080 auf Anforderungen lauscht.   

    ```azurecli-interactive
    az webapp config container set -c jenkinsregistry/webapp --resource-group myResourceGroupJenkins --name myJavaApp
    az webapp config appsettings set --resource-group myResourceGroupJenkins --name myJavaApp --settings PORT=8080
    ```

## <a name="configure-the-azure-app-service-jenkins-plug-in"></a>Konfigurieren des Jenkins-Plug-Ins für Azure App Service

1. Wählen Sie in der Jenkins-Webkonsole den erstellten Auftrag **MyJavaApp** aus, und wählen Sie dann links auf der Seite die Option **Configure** aus.
2. Scrollen Sie zu **Post-build Actions**, und wählen Sie **Add post-build action** und dann **Publish an Azure Web App** aus.
3. Wählen Sie unter **Azure Profile Configuration** neben **Azure Credentials** die Option **Add**, und wählen Sie dann **Jenkins** aus.
4. Wählen Sie im Dialogfeld **Add Credentials** aus der Dropdownliste **Kind** die Option **Microsoft Azure Service Principal** aus.
5. Erstellen Sie über die Azure CLI oder die [Cloud Shell](/azure/cloud-shell/overview) einen Active Directory-Dienstprinzipal.
    
    ```azurecli-interactive
    az ad sp create-for-rbac --name jenkins_sp --password secure_password
    ```

    ```json
    {
        "appId": "BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBB",
        "displayName": "jenkins_sp",
        "name": "http://jenkins_sp",
        "password": "secure_password",
        "tenant": "CCCCCCCC-CCCC-CCCC-CCCCCCCCCCC"
    }
    ```
6. Geben Sie die Anmeldeinformationen des Dienstprinzipals im Dialogfeld **Add credentials** ein. Wenn Sie Ihre Azure-Abonnement-ID nicht kennen, können Sie sie mithilfe der CLI abfragen:
     
     ```azurecli-interactive
     az account list
     ```

     ```json
        {
            "cloudName": "AzureCloud",
            "id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA",
            "isDefault": true,
            "name": "Visual Studio Enterprise",
            "state": "Enabled",
            "tenantId": "CCCCCCCC-CCCC-CCCC-CCCC-CCCCCCCCCCC",
            "user": {
            "name": "raisa@fabrikam.com",
            "type": "user"
            }
     ```

    ![Konfigurieren eines Azure-Dienstprinzipals](media/jenkins-java-quickstart/azure_service_principal.png)
6. Überprüfen Sie mithilfe von **Verify Service Principal**, ob sich der Dienstprinzipal bei Azure authentifizieren kann. 
7. Wählen Sie **Add** aus, um die Anmeldeinformationen zu speichern.
8. Wählen Sie die Anmeldeinformationen des Dienstprinzipals aus, die Sie gerade aus der Dropdownliste **Azure Credentials** hinzugefügt haben, wenn Sie wieder zur Konfiguration **Publish an Azure Web App** zurückgekehrt sind.
9. Wählen Sie unter **App Configuration** den Namen Ihrer Ressourcengruppe und Ihrer Web-App aus der Dropdownliste aus.
10. Aktivieren Sie das Optionsfeld **Publish via Docker**.
11. Geben Sie `complete/Dockerfile` unter **Dockerfile path** ein.
12. Geben Sie `https://jenkinsregistry.azurecr.io` in das Feld **Docker registry URL** ein.
13. Wählen Sie neben **Registry Credentials** die Option **Add** aus. 
14. Geben Sie als **Username** den Benutzernamen des Administrators für die von Ihnen erstellte Azure-Containerregistrierung ein.
15. Geben Sie im Feld **Password** das Kennwort für die Azure-Containerregistrierung ein. Sie können den Benutzernamen und das Kennwort aus dem Azure-Portal oder mithilfe des folgenden CLI-Befehls abrufen:

    ```azurecli-interactive
    az acr credential show -n jenkinsregistry
    ```
    ![Hinzufügen der Anmeldeinformationen für Ihre Containerregistrierung](media/jenkins-java-quickstart/enter_acr_credentials.png)
15. Wählen Sie **Add** aus, um die Anmeldeinformationen zu speichern.
16. Wählen Sie die neu erstellten Anmeldeinformationen aus der Dropdownliste **Registry credentials** im Bereich **App Configuration** für **Publish an Azure Web App** aus. Die fertig gestellte Postbuildaktion sollte in etwa wie folgt aussehen:   
    ![Konfiguration der Postbuildaktion für die Azure App Service-Bereitstellung](media/jenkins-java-quickstart/appservice_plugin_configuration.png)
17. Klicken Sie zum Speichern der Auftragskonfiguration auf **Save**.

## <a name="deploy-the-app-from-github"></a>Bereitstellen der App über GitHub

1. Wählen Sie im Jenkins-Projekt die Option **Build Now**, um die Beispiel-App in Azure bereitzustellen.
2. Sobald der Build abgeschlossen ist, steht die App unter der Veröffentlichungs-URL – z.B. http://myjavaapp.azurewebsites.net – live in Azure bereit.   
   ![Anzeigen Ihrer bereitgestellten App in Azure](media/jenkins-java-quickstart/hello_docker_world_unedited.png)

## <a name="push-changes-and-redeploy"></a>Pushen von Änderungen und erneutes Bereitstellen

1. Wechseln Sie aus Ihrem Github-Fork im Web zu `complete/src/main/java/Hello/Application.java`. Wählen Sie rechts auf der GitHub-Benutzeroberfläche den Link **Edit this file**.
2. Nehmen Sie folgende Änderung an der `home()`-Methode vor, und committen Sie die Änderung in den Masterbranch Ihres Repositorys.
   
    ```java
    return "Hello Docker World on Azure";
    ```
3. In Jenkins wird ein neuer Build gestartet, ausgelöst durch den neuen Commit im `master`-Branch des Repositorys. Wenn der Build abgeschlossen ist, laden Sie Ihre App erneut in Azure.     
      ![Anzeigen Ihrer bereitgestellten App in Azure](media/jenkins-java-quickstart/hello_docker_world.png)
  
## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Azure-VMs als Build-Agents](/azure/jenkins/jenkins-azure-vm-agents)
- [Verwalten von Ressourcen in Aufträgen und Pipelines mit der Azure CLI](/azure/jenkins/execute-cli-jenkins-pipeline)
 

