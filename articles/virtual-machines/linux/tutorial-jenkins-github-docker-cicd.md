---
title: Erstellen einer Entwicklungspipeline in Azure mit Jenkins | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie in Azure einen virtuellen Computer von Jenkins erstellen können, der bei jedem Codecommit Pullvorgänge aus GitHub durchführt und einen neuen Docker-Container zum Ausführen Ihrer Anwendung erstellt."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: d9849b5e061dd7f2ae0744a3522dc2eb1fb37035
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="how-to-create-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>So erstellen Sie in Azure eine Entwicklungsinfrastruktur auf einem virtuellen Linux-Computer mit Jenkins, GitHub und Docker
Sie können zum Automatisieren der Erstellungs- und Testphase der Anwendungsentwicklung eine Pipeline für Continuous Integration und Deployment (CI/CD) verwenden. In diesem Tutorial erstellen Sie eine CI/CD-Pipeline auf einer Azure-VM und erfahren, wie Sie:

> [!div class="checklist"]
> * eine Jenkins-VM erstellen können
> * Jenkins installieren und konfigurieren können
> * eine Webhookintegrations zwischen GitHub und Jenkins erstellen können
> * Jenkins-Buildauftrag aus GitHub-Commits erstellen und auslösen können
> * ein Docker-Image für Ihre Anwendung erstellen können
> * überprüfen können, ob GitHub-Commits neue Docker-Images und -Updates für die ausgeführte Anwendung erstellen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-jenkins-instance"></a>Erstellen einer Jenkins-Instanz
In einem vorherigen Tutorial zum [Anpassen eines virtuellen Linux-Computers beim ersten Start](tutorial-automate-vm-deployment.md) haben Sie erfahren, wie die Anpassung für virtuelle Computer mit cloud-init automatisiert wird. In diesem Tutorial wird eine cloud-init-Datei verwendet, um Jenkins und Docker auf einer VM zu installieren. 

Erstellen Sie in der aktuellen Shell eine Datei namens *cloud-init.txt*, und fügen Sie die folgende Konfiguration ein. Erstellen Sie die Datei beispielsweise in Cloud Shell, nicht auf dem lokalen Computer. Geben Sie `sensible-editor cloud-init-jenkins.txt` ein, um die Datei zu erstellen und eine Liste der verfügbaren Editoren anzuzeigen. Stellen Sie sicher, dass die gesamte Datei „cloud-init“ ordnungsgemäß kopiert wird, insbesondere die erste Zeile:

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

Vor der Erstellung eines virtuellen Computers müssen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. In folgendem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupJenkins* am Standort *eastus* erstellt:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Jetzt können Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer erstellen. Verwenden Sie den `--custom-data`-Parameter, um Ihre cloud-init-Konfigurationsdatei zu übergeben. Geben Sie den vollständigen Pfad von *cloud-init-jenkins.txt* an, wenn Sie die Datei außerhalb Ihres vorhandenen Arbeitsverzeichnisses gespeichert haben.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Die Erstellung und Konfigurieren der VM dauert einige Minuten.

Verwenden Sie [az vm open-port](/cli/azure/vm#open-port), um Port *8080* für den Jenkins-Datenverkehr zu öffnen, und verwenden Sie Port *1337* für die Anwendung „node.js“, die zum Ausführen einer Beispiel-App verwendet wird. Damit lassen Sie den Webdatenverkehr zu:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Konfigurieren von Jenkins
Rufen Sie die öffentliche IP-Adresse Ihrer VM ab, um auf die Jenkins-Instanz zuzugreifen:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Aus Sicherheitsgründen müssen Sie das ursprüngliche Administratorkennwort eingeben, dass in einer Textdatei auf Ihrer VM gespeichert ist, um mit der Installation von Jenkins beginnen zu können. Verwenden Sie die öffentliche IP-Adresse, die Sie im vorherigen Schritt abgerufen haben, um eine SSH-Verbindung mit Ihrer VM herzustellen:

```bash
ssh azureuser@<publicIps>
```

Lassen Sie das `initialAdminPassword` für Ihre Installation anzeigen, und kopieren Sie dieses:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Sollte die Datei noch nicht verfügbar sein, warten Sie noch einige Minuten, bis die Installation von Jenkins und Docker abgeschlossen ist.

Öffnen Sie anschließend einen Webbrowser, und gehen Sie zu `http://<publicIps>:8080`. Schließen Sie das anfängliche Jenkins-Setup wie folgt ab:

- Geben Sie das *anfängliche Administratorkennwort* ein, das Sie im vorherigen Schritt von der VM abgerufen haben.
- Klicken Sie auf **Select plugins to install** (Zu installierende Plugins auswählen)
- Suchen Sie nach *GitHub* über das Textfeld am oberen Rand, wählen Sie *GitHub-Plugin* aus, und klicken Sie anschließend auf **Installieren**
- Füllen Sie das Formular wie gewünscht aus, um ein Jenkins-Benutzerkonto zu erstellen. Es ist sicherer, diesen ersten Jenkins-Benutzer zu erstellen, statt mit dem Standardadministratorkonto fortzufahren.
- Wenn der Vorgang abgeschlossen ist, klicken Sie auf **Start using Jenkins** (Jenkins verwenden)


## <a name="create-github-webhook"></a>Erstellen eines GitHub-Webhooks
Öffnen Sie die [node.js-Beispielapp „Hello World“](https://github.com/Azure-Samples/nodejs-docs-hello-world) aus dem Azure-Beispielrepository, um die Integration in GitHub zu konfigurieren. Klicken Sie auf die Schaltfläche **Fork** (Verzweigen) in der rechten oberen Ecke, um das Repository in Ihr GitHub-Konto zu verzweigen.

Erstellen eines Webhooks in der von Ihnen erstellten Verzweigung:

- Klicken Sie auf **Einstellungen**, und wählen Sie anschließend **Integrations & Services** (Integrationen und Dienste) auf der linken Seite.
- Klicken Sie auf **Add service** (Dienst hinzufügen), und geben Sie anschließend *Jenkins* in das Filterfeld ein.
- Wählen Sie *Jenkins (GitHub-Plugin)* aus
- Geben Sie für die **Jenkins-Hook-URL** `http://<publicIps>:8080/github-webhook/` ein. Stellen Sie sicher, dass Sie den nachstehenden Schrägstrich (/) hinzufügen
- Klicken Sie auf **Dienst hinzufügen**

![Hinzufügen eines GitHub-Webhooks in Ihr verzweigtes Repository](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Erstellen eines Jenkins-Auftrags
Erstellen Sie einen Jenkins-Auftrag, damit Jenkins auf ein Ereignis in GitHub, wie z.B. das Entwickeln von Code, reagieren kann. 

Klicken Sie auf Ihrer Jenkins-Website auf der Startseite auf **Create new jobs** (Neue Aufträge erstellen):

- Geben Sie *HelloWorld* als den Namen des Auftrags ein. Wählen Sie **Freestyle Project** und anschließend **OK** aus.
- Wählen Sie im Bereich **Allgemein** das Projekt **GitHub** aus, und geben Sie die URL Ihres verzweigten Repositorys ein. Eine derartige URL kann z.B. so aussehen: *https://github.com/iainfoulds/nodejs-docs-hello-world*
- Wählen Sie im Bereich **Quellcodeverwaltung** **Git** aus, und geben Sie die *git*-URL Ihres verzweigten Repositorys ein. Eine derartige URL kann z.B. so aussehen: *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- Wählen Sie im Bereich **Build Triggers** (Trigger erstellen) die Option **GitHub hook trigger for GITScm polling** (GitHub-Hooktrigger für GITScm-Abruf) aus.
- Wählen Sie im Abschnitt **Build** die Option **Buildschritt hinzufügen** aus. Wählen Sie **Execute shell** (Shell ausführen) aus, und geben Sie anschließend `echo "Testing"` im Befehlsfenster ein.
- Klicken Sie unten auf der Auftragsseite auf **Speichern** .


## <a name="test-github-integration"></a>Testen der GitHub-Integration
Übernehmen Sie eine Änderung in Ihrer Verzweigung, um die GitHub-Integration mit Jenkins zu testen. 

Kehren Sie zur GitHub-Web-UI zurück, und wählen Sie Ihr verzweigtes Repository aus. Klicken Sie anschließend auf die Datei **index.js**. Klicken Sie auf das Stiftsymbol, um diese Datei zu bearbeiten, sodass in der sechsten Zeile Folgendes steht:

```nodejs
response.end("Hello World!");
```

Klicken Sie auf die Schaltfläche **Commit Changes** (Änderungen übernehmen) am unteren Rand, um die Änderungen zu übernehmen.

In Jenkins können Sie mit einem neuen Build im Bereich **Build history** (Buildverlauf) in der unteren linken Ecke der Auftragsseite beginnen. Klicken Sie auf den Link der Buildnummer, und wählen Sie auf der linken Seite **Konsolenausgabe** aus. Sie können sich die von Jenkins durchgeführten Schritte anschauen, während ein Pullvorgang für Ihren Code aus GitHub durchgeführt wird. Der Buildvorgang gibt die Meldung `Testing` an die Konsole aus. Jedes Mal, wenn ein Commit in GitHub durchgeführt wird, wendet sich der Webhook an Jenkins und löst so einen neuen Build aus.


## <a name="define-docker-build-image"></a>Definieren eines Docker-Buildimages
Erstellen Sie ein Docker-Image zum Ausführen der Anwendung, um das auf Ihren GitHub-Commits basierte Ausführen der node.js-App zu sehen. Das Image wird aus einer Dockerfile-Datei erstellt, die die Konfiguration des Containers definiert, der die App ausführt. 

Wechseln Sie von der SSH-Verbindung mit Ihrer VM zum Jenkins-Arbeitsbereichsverzeichnis, das nach dem von Ihnen in einem vorherigen Schritt erstellten Auftrag benannt ist. In Ihrem Beispiel hieß dieses Verzeichnis *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Erstellen Sie in diesem Arbeitsbereichsverzeichnis mithilfe von `sudo sensible-editor Dockerfile` eine Datei, und fügen Sie den folgenden Inhalt ein. Stellen Sie sicher, dass die gesamte Dockerfile-Datei ordnungsgemäß kopiert wird, insbesondere die erste Zeile:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

Diese Dockerfile-Datei verwendet das node.js-Basisimage mithilfe von Alpine Linux, macht Port 1337 verfügbar, auf dem die Anwendung „Hello World“ ausgeführt wird, und kopiert anschließend die Anwendungsdateien und initialisiert sie.


## <a name="create-jenkins-build-rules"></a>Erstellen von Jenkins-Buildregeln
Sie haben in einem vorherigen Schritt eine einfache Jenkins-Buildregel erstellt, die eine Meldung an die Konsole ausgegeben hat. Erstellen Sie die Buildschritte, mit denen Sie die Dockerfile-Datei verwenden und die Anwendung ausführen können.

Kehren Sie zu Ihrer Jenkins-Instanz zurück, und wählen Sie den Auftrag aus, den Sie in einem vorherigen Schritt erstellt haben. Klicken Sie links auf **Configure** (Konfigurieren), und scrollen Sie nach unten zum Bereich **Build**:

- Entfernen Sie den vorhandenen Buildschritt `echo "Test"`. Klicken Sie dazu auf das rote Kreuz in der oberen rechten Ecke des Felds des vorhandenen Buildschritts.
- Klicken Sie auf **Add build step** (Buildschritt hinzufügen), und wählen Sie anschließend **Execute shell** (Shell ausführen)
- Geben Sie im Feld **Befehl** die folgenden Docker-Befehle ein, und wählen Sie anschließend **Speichern** aus:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

Die Docker-Buildschritte erstellen ein Image und markieren es mit der Jenkins-Buildnummer, damit Sie einen Verlauf der Images erhalten. Alle vorhandenen Container, die die Anwendung ausführen, werden beendet und entfernt. Ein neuer Container wird dann mithilfe des Images gestartet. Er führt die node.js-Anwendung auf Grundlage der letzten Commits in GitHub aus.


## <a name="test-your-pipeline"></a>Testen Ihrer Pipeline
Bearbeiten Sie erneut die Datei *index.js* in Ihrem verzweigten GitHub-Repository, und klicken Sie auf **Änderungen übernehmen**. In Jenkins wird ein neuer Auftrag auf Grundlage des Webhooks für GitHub gestartet. Das Erstellen des Docker-Images und das Starten Ihrer Anwendung in einem neuen Container nimmt einige Sekunden in Anspruch.

Falls dies vonnöten ist, können Sie die öffentliche IP-Adresse Ihrer VM erneut abrufen:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Öffnen Sie einen Webbrowser, und geben Sie `http://<publicIps>:1337` ein. Ihre node.js-Anwendung wird wie folgt mit den letzten Änderungen in Ihrer GitHub-Verzweigung angezeigt:

![Ausgeführte Node.js-App](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Bearbeiten Sie anschließend die Datei *index.js* noch einmal in GitHub, und übernehmen Sie die Änderungen. Das Abschließen des Auftrags nimmt einige Sekunden in Anspruch. Aktualisieren Sie danach Ihren Webbrowser, um die aktualisierte Version Ihrer in einem neuen Container ausgeführten App wie folgt anzuzeigen:

![Ausführen der node.js-Anwendung nach einem weiteren GitHub-Commitvorgang](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie GitHub so konfiguriert, dass es einen Jenkins-Buildauftrag bei jedem Code-Commit ausführt und anschließend einen Docker-Container zum Testen der Anwendung bereitstellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * eine Jenkins-VM erstellen können
> * Jenkins installieren und konfigurieren können
> * eine Webhookintegrations zwischen GitHub und Jenkins erstellen können
> * Jenkins-Buildauftrag aus GitHub-Commits erstellen und auslösen können
> * ein Docker-Image für Ihre Anwendung erstellen können
> * überprüfen können, ob GitHub-Commits neue Docker-Images und -Updates für die ausgeführte Anwendung erstellen

Im nächsten Tutorial erhalten Sie Informationen zum Integrieren von Jenkins in Visual Studio Team Services.

> [!div class="nextstepaction"]
> [Bereitstellen Ihrer App auf virtuellen Linux-Computern mithilfe von Jenkins und Team Services](tutorial-build-deploy-jenkins.md)
