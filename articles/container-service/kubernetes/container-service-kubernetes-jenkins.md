---
title: Jenkins CI/CD mit Kubernetes in Azure Container Service | Microsoft-Dokumentation
description: Automatisieren eines CI/CD-Prozesses mit Jenkins zum Bereitstellen und Aktualisieren einer Container-App auf Kubernetes in Azure Container Service
services: container-service
documentationcenter: 
author: chzbrgr71
manager: johny
editor: 
tags: acs, azure-container-service, jenkins
keywords: Docker, Container, Kubernetes, Azure, Jenkins
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 85ac34444cc54b9c6fe9d5d3fcd1ab53bf1dc0d9
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---

# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Jenkins-Integration mit Azure Container Service und Kubernetes 
In diesem Tutorial werden Sie durch die Schritte zum Einrichten der Continuous Integration einer Anwendung mit mehreren Container in Azure Container Service Kubernetes unter Verwendung der Jenkins-Plattform geführt. Der Workflow aktualisiert das Containerimage in Docker Hub sowie die Kubernetes-Pods mittels eines Bereitstellungsrollouts. 

## <a name="high-level-process"></a>Der Prozess im Überblick
Die grundlegenden Schritte, die in diesem Artikel beschrieben werden, sind: 
- Installieren eines Kubernetes-Clusters in Container Service
- Einrichten von Jenkins und Konfigurieren des Zugriffs auf Container Service
- Erstellen eines Jenkins-Workflows
- Durchführen eines Ende-zu-Ende-Tests des CI/CD-Prozesses

## <a name="install-a-kubernetes-cluster"></a>Installieren eines Kubernetes-Clusters
    
Bereitstellen des Kubernetes-Clusters in Azure Container Service mithilfe der folgenden Schritte. Die vollständige Dokumentation finden Sie [hier](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>Schritt 1: Erstellen einer Ressourcengruppe
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-the-cluster"></a>Schritt 2: Bereitstellen des Clusters
> [!NOTE]
> Die folgenden Schritte erfordern einen lokalen öffentlichen SSH-Schlüssel im Ordner „~/.ssh“.
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-to-container-service"></a>Einrichten von Jenkins und Konfigurieren des Zugriffs auf Container Service

### <a name="step-1-install-jenkins"></a>Schritt 1: Installieren von Jenkins
1. Erstellen Sie eine Azure-VM mit Ubuntu 16.04 LTS.  Da Sie in den späteren Schritten über Bash auf Ihrem lokalen Computer eine Verbindung mit dieser VM herstellen müssen, legen Sie den „Authentifizierungstyp“ auf „öffentlicher SSH-Schlüssel“ fest, und fügen Sie den öffentlichen SSH-Schlüssel ein, der lokal in Ihrem Ordner „~/.ssh“ gespeichert ist.  Notieren Sie sich auch den angegebenen Benutzernamen, da dieser Benutzername in den späteren Schritten benötigt wird, um das Jenkins-Dashboard anzuzeigen und eine Verbindung mit der Jenkins-VM herzustellen.
2. Installieren Sie Jenkins mithilfe dieser [Anweisungen](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). Ein ausführlicheres Tutorial befindet sich auf [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. Um das Jenkins-Dashboard auf Ihrem lokalen Computer anzuzeigen, aktualisieren Sie die Azure-Netzwerksicherheitsgruppe, sodass Port 8080 zugelassen wird, indem Sie eine eingehende Regel hinzufügen, die den Zugriff auf Port 8080 erlaubt.  Alternativ dazu können Sie auch eine Portweiterleitung einrichten, indem Sie diesen Befehl ausführen: `ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Stellen Sie eine Verbindung mit Ihrem Jenkins-Server her, indem Sie im Browser zur öffentlichen IP (http://<Ihre_öffentliche_Jenkins_IP>:8080) navigieren und das Jenkins-Dashboard beim ersten Zugriff mit dem anfänglichen Administratorkennwort entsperren.  Das Administratorkennwort ist auf der Jenkins-VM unter „/var/lib/jenkins/secrets/initialAdminPassword“ gespeichert.  Eine einfache Möglichkeit, dieses Kennwort abzurufen, ist die Herstellung einer SSH-Verbindung mit der Jenkins-VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Führen Sie als Nächstes folgenden Befehl aus: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Installieren Sie Docker mithilfe dieser [Anweisungen](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts) auf dem Jenkins-Computer. Dadurch können die Docker-Befehle in Jenkins-Aufträgen ausgeführt werden.
6. Konfigurieren Sie Docker-Berechtigungen, damit Jenkins auf den Docker-Endpunkt zugreifen kann.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Installieren Sie die `kubectl` CLI auf Jenkins. Weitere Informationen finden Sie unter [Installieren und Einrichten von kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).  Jenkins-Aufträge verwenden „kubectl“, um den Kubernetes-Cluster zu verwalten und für Bereitstellungen zu verwenden.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-to-the-kubernetes-cluster"></a>Schritt 2: Einrichten des Zugriffs auf den Kubernetes-Cluster

> [!NOTE]
> Es gibt mehrere Ansätze zur Ausführung der folgenden Schritte. Verwenden Sie den Ansatz, der für Sie am einfachsten ist.
>

1. Kopieren Sie die `kubectl`-Konfigurationsdatei auf den Jenkins-Computer, sodass Jenkins-Aufträge auf den Kubernetes-Cluster zugreifen können. Bei diesen Anweisungen wird davon ausgegangen, dass Sie Bash auf einem anderen Computer als der Jenkins-VM verwenden und dass ein lokaler öffentlicher SSH-Schlüssel im „~/.ssh“-Ordner des Computers gespeichert ist.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. Überprüfen Sie unter Jenkins, ob der Zugriff auf den Kubernetes-Cluster möglich ist.  Stellen Sie zu diesem Zweck eine SSH-Verbindung mit der Jenkins-VM her: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Überprüfen Sie dann, ob Jenkins erfolgreich eine Verbindung mit Ihrem Cluster herstellen kann: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Erstellen eines Jenkins-Workflows

### <a name="prerequisites"></a>Voraussetzungen

- GitHub-Konto für Code-Repository.
- Docker Hub-Konto zum Speichern und Aktualisieren von Images.
- Containeranwendung, die neu erstellt und aktualisiert werden kann. Sie können diese in Golang geschriebene Beispiel-Container-App verwenden: „https://github.com/chzbrgr71/go-web“. 

> [!NOTE]
> Die folgenden Schritte müssen in Ihrem eigenen GitHub-Konto ausgeführt werden. Gerne können Sie das Repository oben klonen, aber Sie müssen Ihr eigenes Konto verwenden, um die Webhooks und den Jenkins-Zugriff zu konfigurieren.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>Schritt 1: Bereitstellen der ersten v1 der Anwendung
1. Erstellen Sie die App auf dem Entwicklercomputer mit den folgenden Befehlen. Ersetzen Sie `myrepo` durch das eigene.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Übertragen Sie das Image auf Docker Hub.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Stellen Sie es auf dem Kubernetes-Cluster bereit.
    
    > [!NOTE] 
    > Bearbeiten Sie die `go-web.yaml`-Datei, um Ihr Containerimage und -repository zu aktualisieren.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>Schritt 2: Konfigurieren des Jenkins-Systems
1. Klicken Sie auf **Manage Jenkins** (Jenkins verwalten) > **Configure System** (System konfigurieren).
2. Wählen Sie unter **GitHub** die Option **Add GitHub Server** (GitHub-Server hinzufügen) aus.
3. Lassen Sie **API URL** als Standardeinstellung stehen.
4. Fügen Sie unter **Credentials** (Anmeldeinformationen) Jenkins-Anmeldeinformationen mit **Secret text** (Geheimer Schlüsseltext) hinzu. Es empfiehlt sich, persönliche GitHub-Zugriffstoken zu verwenden, die in Ihren GitHub-Benutzerkontoeinstellungen konfiguriert werden. Weitere Informationen hierzu finden Sie [hier](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).
5. Klicken Sie auf **Test connection** (Verbindung testen), um sicherzustellen, dass die Verbindung ordnungsgemäß konfiguriert ist.
6. Fügen Sie unter **Global Properties** (Globale Eigenschaften) eine Umgebungsvariable `DOCKER_HUB` hinzu, und geben Sie Ihr Docker Hub-Kennwort an. (Dies ist in dieser Demo hilfreich, in einem Produktionsszenario müsste jedoch ein sichererer Ansatz gewählt werden.)
7. Speichern Sie.

![Jenkins GitHub-Zugriff](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-the-jenkins-workflow"></a>Schritt 3: Erstellen des Jenkins-Workflows
1. Erstellen Sie ein Jenkins-Element.
2. Geben Sie einen Namen (z.B. „Go-Web“) ein, und wählen Sie **Freestyle Project** aus. 
3. Aktivieren Sie **GitHub Project**, und geben Sie die URL zu Ihrem GitHub-Repository an.
4. Geben Sie in **Source Code Management** (Quellcodeverwaltung) die URL des GitHub-Repositorys und Anmeldeinformationen an. 
5. Fügen Sie einen **Buildschritt** vom Typ **Execute shell** (Shell ausführen) hinzu, und verwenden Sie den folgenden Text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Fügen Sie einen weiteren **Buildschritt** vom Typ **Execute shell** (Shell ausführen) hinzu, und verwenden Sie den folgenden Text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins-Buildschritte](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Speichern Sie das Jenkins-Element, und testen Sie es mit **Build Now** (Jetzt erstellen).

### <a name="step-4-connect-github-webhook"></a>Schritt 4: Herstellen der Verbindung mit dem GitHub-Webhook
1. Klicken Sie in dem erstellten Jenkins-Element auf **Configure** (Konfigurieren).
2. Wählen Sie unter **Build Triggers** (Trigger erstellen) die Option **GitHub hook trigger for GITScm polling** (GitHub-Hooktrigger für GITScm-Abruf) und **Save** (Speichern). Damit wird der GitHub-Webhook automatisch konfiguriert.
3. Klicken Sie in Ihrem GitHub-Repository für Go-Web auf **Settings > Webhooks** (Einstellungen > Webhooks).
4. Stellen Sie sicher, dass die Jenkins-Webhook-URL erfolgreich hinzugefügt wurde. Die URL sollte auf „github-webhook“ enden.

![Jenkins-Webhook-Konfiguration](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-the-cicd-process-end-to-end"></a>Durchführen eines Ende-zu-Ende-Tests des CI/CD-Prozesses

1. Aktualisieren Sie Code für das Repository und führen Sie eine Pushübertragung/Synchronisierung mit dem GitHub-Repository aus.
2. Überprüfen Sie auf der Jenkins-Konsole den **Buildverlauf**, und vergewissern Sie sich, dass der Auftrag ausgeführt wurde. Öffnen Sie die Konsolenausgabe, um Details anzuzeigen.
3. Zeigen Sie aus Kubernetes die Details zu der aktualisierten Bereitstellung an:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Nächste Schritte

- Stellen Sie Azure Container Registry bereit, und speichern Sie die Images in einem sicheren Repository. Weitere Informationen finden Sie in der [Dokumentation zu Azure Container Registry](https://docs.microsoft.com/azure/container-registry).
- Erstellen Sie einen komplexeren Workflow, der parallele Bereitstellung und automatisierte Tests in Jenkins umfasst.
- Weitere Informationen zu CI/CD mit Jenkins und Kubernetes finden Sie im [Jenkins-Blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).

