---
title: Bereitstellen in Azure App Service per Jenkins-Plug-In | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie das Azure App Service-Jenkins-Plug-In zum Bereitstellen einer Java-Web-App für Azure in Jenkins verwenden."
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.translationtype: HT
ms.sourcegitcommit: 9633e79929329470c2def2b1d06d95994ab66e38
ms.openlocfilehash: dfb1abd4eefdb2baea8cdbd497bc8fcc95d200e6
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="deploy-to-azure-app-service-with-jenkins-plugin"></a>Bereitstellen in Azure App Service per Jenkins-Plug-In 
Zum Bereitstellen einer Java-Web-App in Azure können Sie Azure CLI in der [Jenkins-Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) verwenden oder das [Azure App Service-Jenkins-Plug-In](https://plugins.jenkins.io/azure-app-service) nutzen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren von Jenkins zum Bereitstellen in Azure App Service per FTP 
> * Konfigurieren von Jenkins zum Bereitstellen in Azure App Service in Linux per Docker 

## <a name="create-and-configure-jenkins-instance"></a>Erstellen und Konfigurieren einer Jenkins-Instanz
Wenn Sie noch nicht über einen Jenkins-Master verfügen, sollten Sie mit der [Projektmappenvorlage](install-jenkins-solution-template.md) beginnen, in der JDK8 und die folgenden erforderlichen Plug-Ins enthalten sind:

* [Jenkins Git-Client-Plug-In](https://plugins.jenkins.io/git-client) v.2.4.6 
* [Docker Commons-Plug-In](https://plugins.jenkins.io/docker-commons) v.1.4.0
* [Azure-Anmeldeinformationen](https://plugins.jenkins.io/azure-credentials) v.1.2
* [Azure App Service](https://plugins.jenkins.io/azure-app-server) v.0.1

Sie können das App Service-Plug-In zum Bereitstellen von Web-Apps in allen Sprachen (z.B. C#, PHP, Java und Node.js usw.) verwenden, die von Azure App Service unterstützt werden. In diesem Tutorial verwenden wir die folgende Java-Beispiel-App: [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) (Einfache Java-Web-App für Azure). Klicken Sie auf die Schaltfläche **Fork** (Verzweigen) in der rechten oberen Ecke, um das Repository in Ihr GitHub-Konto zu verzweigen.  

Das Java JDK und Maven sind zum Erstellen des Java-Projekts erforderlich. Stellen Sie sicher, dass Sie die Komponenten auf dem Jenkins-Master oder dem VM-Agent installieren, wenn Sie diesen für Continuous Integration-Zwecke nutzen. 

Melden Sie sich zum Installieren mithilfe von SSH an der Jenkins-Instanz an, und führen Sie die folgenden Befehle aus:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Für die Bereitstellung in App Service unter Linux müssen Sie Docker auch auf dem Jenkins-Master oder dem für den Buildvorgang verwendeten VM-Agent installieren. Informationen zur Installation von Docker finden Sie in diesem Artikel: „https://docs.docker.com/engine/installation/linux/ubuntu/“.

## <a name="add-azure-service-principal-to-jenkins-credential"></a>Hinzufügen des Azure-Dienstprinzipals zu Jenkins-Anmeldeinformationen

Ein Azure-Dienstprinzipal ist erforderlich, um Azure bereitzustellen. 

<ol>
<li>Verwenden Sie [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) oder das [Azure-Portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal), um einen Azure-Dienstprinzipal zu erstellen.</li>
<li>Klicken Sie im Jenkins-Dashboard auf **Credentials -> System ->** (Anmeldeinformationen -> System ->). Klicken Sie auf **Global credentials (unrestricted)** (Globale Anmeldeinformationen (uneingeschränkt)).</li>
<li>Klicken Sie auf **Add Credentials** (Anmeldeinformationen hinzufügen), um einen Microsoft Azure-Dienstprinzipal hinzuzufügen, indem Sie die Abonnement-ID, die Client-ID, den geheimen Clientschlüssel und den OAuth 2.0-Token-Endpunkt angeben. Stellen Sie eine ID (**mySp**) zur Verwendung in den nächsten Schritten bereit.</li>
</ol>

## <a name="azure-app-service-plugin"></a>Azure App Service-Plug-In

Das Azure App Service-Plug-In v1.0 unterstützt Continuous Deployment für Azure-Web-Apps mit folgenden Verfahren:

* Git und FTP
* Docker für Web-App unter Linux

## <a name="configure-jenkins-to-deploy-web-app-through-ftp-using-the-jenkins-dashboard"></a>Konfigurieren von Jenkins für die Bereitstellung der Web-App per FTP mit dem Jenkins-Dashboard

Zum Bereitstellen Ihres Projekts in der Azure-Web-App können Sie Ihre Buildartefakte (z.B. eine WAR-Dateien in Java) per Git oder FTP hochladen.

Vor dem Einrichten des Auftrags in Jenkins benötigen Sie einen Azure App Service-Plan und eine Web-App zum Ausführen der Java-App.


1. Erstellen Sie mit dem CLI-Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen Azure App Service-Plan mit dem Tarif **FREE**. Der App Service-Plan definiert die physischen Ressourcen, die zum Hosten Ihrer Apps verwendet werden. Alle einem App Service-Plan zugewiesenen Anwendungen teilen sich diese Ressourcen. Das spart Kosten, wenn Sie mehrere Apps hosten.
2. Erstellen Sie eine Web-App. Sie können entweder das [Azure-Portal](/azure/app-service-web/web-sites-configure) oder den folgenden Azure CLI-Befehl verwenden:
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. Stellen Sie sicher, dass Sie die Java-Laufzeitkonfiguration einrichten, die für Ihre App erforderlich ist. Mit dem folgenden Azure CLI-Befehl wird die Web-App für die Ausführung mit einer aktuellen Java 8 JDK-Version und [Apache Tomcat](http://tomcat.apache.org/) 8.0 konfiguriert.
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-the-jenkins-job"></a>Einrichten des Jenkins-Auftrags


1. Erstellen Sie im Jenkins-Dashboard ein neues **Freestyle**-Projekt.
2. Konfigurieren Sie die **Quellcodeverwaltung** so, dass Ihre lokale Verzweigung von [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) (Einfache Java-Web-App für Azure) verwendet wird, indem Sie die **Repository-URL** angeben. Beispiel: http://github.com/&lt;IhreID>/javawebappsample.
3. Fügen Sie einen Buildschritt hinzu, um das Projekt mit Maven zu erstellen. Fügen Sie hierzu eine **Execute shell** (Shell für die Ausführung) hinzu. Für dieses Beispiel benötigen wir einen zusätzlichen Schritt, um die WAR-Datei im Zielordner in „ROOT.war“ umzubenennen.   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. Fügen Sie eine Postbuildaktion hinzu, indem Sie **Publish an Azure Web App** (Azure-Web-App veröffentlichen) wählen.
5. Geben Sie den Azure-Dienstprinzipal „mySp“ an, den Sie im vorherigen Schritt gespeichert haben.
6. Wählen Sie im Abschnitt **App-Konfiguration** die Ressourcengruppe und die Web-App Ihres Abonnements aus. Das Plug-In erkennt automatisch, ob die Web-App auf Windows oder Linux basiert. Für eine Windows-basierte Web-App wird die Option „Dateien veröffentlichen“ angezeigt.
7. Fügen Sie die Dateien ein, die Sie bereitstellen möchten (z.B. ein WAR-Paket bei Verwendung von Java). Die Angabe des Quell- und Zielverzeichnisses ist optional. Mit den Parametern können Sie beim Hochladen von Dateien die Quell- und Zielordner angeben. Die Java-Web-App in Azure wird auf einem Tomcat-Server ausgeführt. Sie laden Ihr WAR-Paket also in den Ordner „webapps“ hoch. Legen Sie für dieses Beispiel **Quellverzeichnis** auf „target“ fest, und geben Sie „webapps“ als **Zielverzeichnis** an.
8. Wenn Sie die Bereitstellung für einen anderen Slot als den Produktionsslot durchführen möchten, können Sie unter **Slot** den entsprechenden Namen festlegen.
9. Speichern Sie das Projekt, und führen Sie den Buildvorgang durch. Ihre Web-App wird in Azure bereitgestellt, wenn der Buildvorgang abgeschlossen ist.

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a>Bereitstellen der Web-App per FTP mit der Jenkins-Pipeline

Das Plug-In ist für die Pipeline vorbereitet. Ein Beispiel hierfür finden Sie im GitHub-Repository.

1. Öffnen Sie auf der GitHub-Webbenutzeroberfläche die Datei **Jenkinsfile_ftp_plugin**. Klicken Sie auf das Stiftsymbol, um diese Datei zu bearbeiten und die Ressourcengruppe und den Namen Ihrer Web-App in Zeile 11 bzw. 12 zu aktualisieren.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Ändern Sie Zeile 14, um die Anmeldeinformationen-ID in Ihrer Jenkins-Instanz zu aktualisieren.    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a>Erstellen einer Jenkins-Pipeline

1. Öffnen Sie Jenkins in einem Webbrowser, und klicken Sie auf **New Item** (Neues Element).
2. Geben Sie einen Namen für den Auftrag an, und wählen Sie **Pipeline** aus. Klicken Sie auf **OK**.
3. Klicken Sie dann auf die Registerkarte **Pipeline**.
4. Wählen Sie als **Definition** die Option **Pipeline script from SCM** (Pipeline-Skript von SCM) aus.
5. Wählen Sie als **SCM** die Option **Git** aus. Geben Sie die GitHub-URL für Ihr verzweigtes Repository ein: „https:&lt;Ihr verzweigtes Repository>.git“.
6. Aktualisieren Sie **Skriptpfad** auf „Jenkinsfile_ftp_plugin“.
7. Klicken Sie auf **Speichern**, und führen Sie den Auftrag aus.

## <a name="configure-jenkins-to-deploy-web-app-on-linux-through-docker"></a>Konfigurieren von Jenkins zum Bereitstellen von Web-Apps unter Linux mit Docker

Neben Git/FTP wird für Web-Apps unter Linux auch die Bereitstellung mit Docker unterstützt. Für die Bereitstellung mit Docker müssen Sie eine Dockerfile-Datei angeben, mit der Ihre Web-App mit Dienstlaufzeit als Docker-Image verpackt wird. Das Plug-In erstellt dann das Image, übermittelt es per Push an eine Docker-Registrierung und stellt das Image für Ihre Web-App bereit.

Für Web-App unter Linux werden auch herkömmliche Vorgehensweisen wie Git und FTP unterstützt, aber dies gilt nur für integrierte Sprachen (.NET Core, Node.js, PHP und Ruby). Für andere Sprachen müssen Sie Ihren Anwendungscode und die Dienstlaufzeit zusammen in einem Docker-Image verpacken und Docker zum Bereitstellen verwenden.

Vor dem Einrichten des Auftrags in Jenkins benötigen Sie eine Azure App Service-Instanz unter Linux. Außerdem ist eine Containerregistrierung zum Speichern und Verwalten Ihrer privaten Docker-Containerimages erforderlich. Sie können DockerHub verwenden. Wir verwenden für dieses Beispiel Azure Container Registry.

* Sie können [diese Schritte](/azure/app-service-web/app-service-linux-how-to-create-web-app) ausführen, um unter Linux eine Web-App zu erstellen. 
* Azure Container Registry ist ein verwalteter Dienst vom Typ [Docker-Registrierung] (https://docs.docker.com/registry/), der auf Version 2.0 der Open Source-Docker-Registrierung basiert. Führen Sie [diese Schritte] (/azure/container-registry/container-registry-get-started-azure-cli) aus, um weitere Informationen zur Vorgehensweise zu erhalten. Sie können auch DockerHub verwenden.

### <a name="to-deploy-using-docker"></a>Gehen Sie wie folgt vor, um die Bereitstellung mit Docker durchzuführen:

1. Erstellen Sie im Jenkins-Dashboard ein neues Freestyle-Projekt.
2. Konfigurieren Sie die **Quellcodeverwaltung** so, dass Ihre lokale Verzweigung von [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) (Einfache Java-Web-App für Azure) verwendet wird, indem Sie die **Repository-URL** angeben. Beispiel: http://github.com/&lt;IhreID>/javawebappsample.
Fügen Sie einen Buildschritt hinzu, um das Projekt mit Maven zu erstellen. Fügen Sie hierzu eine **Execute shell** (Shell für die Ausführung) und dann unter **Befehl** die folgende Zeile hinzu:    
```bash
mvn clean package
```

3. Fügen Sie eine Postbuildaktion hinzu, indem Sie **Publish an Azure Web App** (Azure-Web-App veröffentlichen) wählen.
4. Geben Sie den Azure-Dienstprinzipal **mySp** an, den Sie im vorherigen Schritt als Element der Azure-Anmeldeinformationen gespeichert haben.
5. Wählen Sie im Abschnitt **App-Konfiguration** die Ressourcengruppe und eine Linux-Web-App Ihres Abonnements aus.
6. Wählen Sie die Option „Publish via Docker“ (Per Docker veröffentlichen).
7. Fügen Sie den **Dockerfile**-Pfad ein. Sie können die Standardeinstellung „/Dockerfile“ beibehalten. Geben Sie die **Docker registry URL** (URL für Docker-Registrierung) im Format „https://&lt;meineRegistrierung>.azurecr.io“ an, wenn Sie Azure Container Registry verwenden. Lassen Sie das Feld leer, wenn Sie DockerHub nutzen.
8. Fügen Sie unter **Registry credentials** (Anmeldeinformationen der Registrierung) die Anmeldeinformationen für Azure Container Registry hinzu. Sie können die Benutzer-ID und das Kennwort abrufen, indem Sie in Azure CLI die folgenden Befehle ausführen. Mit dem ersten Befehl wird das Administratorkonto aktiviert.    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. Der Name und das Tag des Docker-Image auf der Registerkarte **Erweitert** sind optional. Der Imagename wird standardmäßig von dem Imagenamen abgeleitet, den Sie im Azure-Portal konfiguriert haben (in der Docker-Container-Einstellung). Das Tag wird mithilfe von $BUILD_NUMBER generiert. Stellen Sie sicher, dass Sie entweder den Imagenamen im Azure-Portal angeben oder auf der Registerkarte **Erweitert** einen Wert für **Docker-Image** bereitstellen. Geben Sie für dieses Beispiel „&lt;IhreRegistrierung>.azurecr.io/calculator“ für **Docker-Image** an, und lassen Sie **Docker Image Tag** (Docker-Image-Tag) leer.
10. Beachten Sie, dass die Bereitstellung fehlschlägt, wenn Sie die integrierte Docker-Imageeinstellung verwenden. Ändern Sie die Docker-Konfiguration so, dass das benutzerdefinierte Image in der Docker-Container-Einstellung im Azure-Portal verwendet wird. Verwenden Sie für das integrierte Image den Dateiuploadansatz, um die Bereitstellung durchzuführen.
11. Ähnlich wie beim Dateiuploadansatz können Sie einen anderen Slot als den Produktionsslot wählen.
12. Speichern und erstellen Sie das Projekt. Sie sehen, dass Ihr Containerimage per Pushvorgang in Ihre Registrierung übermittelt und die Web-App bereitgestellt wird.

### <a name="deploy-to-web-app-on-linux-through-docker-using-jenkins-pipeline"></a>Bereitstellen für Web-App unter Linux per Docker mit der Jenkins-Pipeline

1. Öffnen Sie auf der GitHub-Webbenutzeroberfläche die Datei **Jenkinsfile_container_plugin**. Klicken Sie auf das Stiftsymbol, um diese Datei zu bearbeiten und die Ressourcengruppe und den Namen Ihrer Web-App in Zeile 11 bzw. 12 zu aktualisieren.    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. Ändern Sie Zeile 13 in Ihren Server für die Containerregistrierung.    
```java
def registryServer = '<registryURL>'
```    

3. Ändern Sie Zeile 16, um die Anmeldeinformationen-ID in Ihrer Jenkins-Instanz zu aktualisieren.    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a>Erstellen einer Jenkins-Pipeline    

1. Öffnen Sie Jenkins in einem Webbrowser, und klicken Sie auf **New Item** (Neues Element).
2. Geben Sie einen Namen für den Auftrag an, und wählen Sie **Pipeline** aus. Klicken Sie auf **OK**.
3. Klicken Sie dann auf die Registerkarte **Pipeline**.
4. Wählen Sie als **Definition** die Option **Pipeline script from SCM** (Pipeline-Skript von SCM) aus.
5. Wählen Sie als **SCM** die Option **Git** aus.
6. Geben Sie die GitHub-URL für Ihr verzweigtes Repository ein: „https:&lt;Ihr verzweigtes Repository>.git“.</li>
Aktualisieren Sie den **Skriptpfad** in „Jenkinsfile_container_plugin“.
8. Klicken Sie auf **Speichern**, und führen Sie den Auftrag aus.

## <a name="verify-your-web-app"></a>Überprüfen Ihrer Web-App

1. Zur Überprüfung, ob die WAR-Datei erfolgreich an Ihre Web-App bereitgestellt wurde, Öffnen Sie einen Webbrowser.
2. Navigieren Sie zu „http://&lt;App_Name>.azurewebsites.net/api/calculator/ping. Folgendes wird angezeigt:    
     Welcome to Java Web App!!! This is updated!
   Sun Jun 17 16:39:10 UTC 2017
3. Gehen Sie zu http://&lt;App_Name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (ersetzen Sie &lt;x> und &lt;y> durch beliebige Zahlen), um die Summe aus x und y zu erhalten.        
    ![Rechner: Addieren](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a>Für App Service unter Linux

* Führen Sie zur Überprüfung in der Azure CLI Folgendes aus:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Sie erhalten folgendes Ergebnis:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Gehen Sie zu http://&lt;App_Name>.azurewebsites.net/api/calculator/ping. Es wird folgende Nachricht angezeigt: 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Gehen Sie zu http://&lt;App_Name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (ersetzen Sie &lt;x> und &lt;y> durch beliebige Zahlen), um die Summe aus x und y zu erhalten.
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie das Azure App Service-Plug-In für die Bereitstellung in Azure verwendet.

Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Konfigurieren von Jenkins zum Bereitstellen in Azure App Service per FTP 
> * Konfigurieren von Jenkins zum Bereitstellen in Azure App Service in Linux mit Docker 
