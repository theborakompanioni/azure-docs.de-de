---
title: "Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps für Container | Microsoft-Dokumentation"
description: "Informationen zum Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps für Container."
keywords: Azure App Service, Web-App, Linux, Docker, Container
services: app-service
documentationcenter: 
author: SyntaxC4
manager: SyntaxC4
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/03/2017
ms.author: cfowler
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 2a14c9539e46bffbe3e11ff372f37190a562e199
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="use-a-custom-docker-image-for-azure-web-app-for-containers"></a>Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps für Container

[Introduction to Azure Web App for Containers](app-service-linux-intro.md) (Einführung zu Azure-Web-App für Container) bietet integrierte Docker-Images unter Linux mit Unterstützung für bestimmte Versionen wie PHP 7.0 und Node.js 4.5. „Web-App für Container“ nutzt die Containertechnologie von Docker, um sowohl integrierte als auch benutzerdefinierte Images als „Platform as a Service“ zu hosten. In diesem Tutorial erfahren Sie, wie Sie ein benutzerdefiniertes Docker-Image für die Verwendung in „Web-App für Container“ erstellen, das einem gängigen Muster entspricht, wenn kein integriertes Image für Ihre Sprache vorhanden ist, oder Ihre Anwendung eine bestimmte Konfiguration erfordert, die innerhalb der integrierten Images nicht bereitgestellt wird.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Git](https://git-scm.com/downloads)
* Ein aktives [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/)
* [Docker](https://docs.docker.com/get-started/#setup)
* Ein [Docker-Hub-Konto](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen, und navigieren Sie dann zum Verzeichnis mit dem Beispielcode.

```bash
git clone https://github.com/Azure-Samples/use-custom-docker-image.git
cd use-custom-docker-image
```

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="build-the-image-from-the-docker-file"></a>Erstellen des Images aus der Docker-Datei

Die folgende Docker-Datei beschreibt die Python-Umgebung, die zur Ausführung Ihrer Anwendung erforderlich ist. Darüber hinaus richtet das Image einen [SSH](https://www.ssh.com/ssh/protocol/)-Server für eine sichere Kommunikation zwischen dem Container und dem Host ein.

```docker
# Use an official Python runtime as a parent image
FROM python

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Run python's package manager and install the flask package
RUN pip install flask

# Configure ports
EXPOSE 2222 80

# Run apt-get, to install the SSH server
RUN apt-get update \ 
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "root:Docker!" | chpasswd

#Copy the sshd_config file to its new location
COPY sshd_config /etc/ssh/

# Start the SSH service
RUN service ssh start

# Copy init_container.sh to the /bin directory
COPY init_container.sh /bin/
    
# Run the chmod command to change permissions on above file in the /bin directory
RUN chmod 755 /bin/init_container.sh 

# run commands in init_container.sh
CMD ["/bin/init_container.sh"]
```

Um das Docker-Image zu erstellen, führen Sie den `docker build`-Befehl aus, und geben Sie einen Namen (`mydockerimage`) und das Tag (`v1.0.0`) ein. Ersetzen Sie `<docker-id>` durch Ihre Docker Hub-Konto-ID.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Der Befehl erzeugt eine Ausgabe ähnlich der Folgenden:

```bash
# The output from the commands in this article has been shortened for brevity.

Sending build context to Docker daemon  23.04kB
Step 1/13 : FROM python
 ---> 968120d8cbe8
Step 2/13 : WORKDIR /app
 ---> Using cache
 ---> dd6fdca5aa65
Step 3/13 : ADD . /app
 ---> e05c8f4beeae
Get:1 http://security.debian.org jessie/updates InRelease [63.1 kB]
Ign http://deb.debian.org jessie InRelease
Get:2 http://deb.debian.org jessie-updates InRelease [145 kB]
Get:3 http://deb.debian.org jessie Release.gpg [2373 B]
Fetched 9988 kB in 7s (1266 kB/s)
Reading package lists...
Building dependency tree...
Reading state information...
The following extra packages will be installed:
  init-system-helpers libwrap0 openssh-sftp-server
Suggested packages:
  ssh-askpass rssh molly-guard ufw monkeysphere
Recommended packages:
  tcpd xauth ncurses-term
The following NEW packages will be installed:
  init-system-helpers libwrap0 openssh-server openssh-sftp-server
0 upgraded, 4 newly installed, 0 to remove and 3 not upgraded.
Need to get 442 kB of archives.
After this operation, 1138 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian/ jessie/main libwrap0 amd64 7.6.q-25 [58.5 kB]
Creating SSH2 RSA key; this may take some time ...
2048 f0:e9:fb:69:de:62:a4:5c:a3:7c:b3:41:e9:2e:96:a3 /etc/ssh/ssh_host_rsa_key.pub (RSA)
Creating SSH2 DSA key; this may take some time ...
1024 4a:5e:89:bd:aa:2d:71:bb:0e:3a:32:94:fb:c0:b1:4d /etc/ssh/ssh_host_dsa_key.pub (DSA)
Processing triggers for systemd (215-17+deb8u7) ...
 ---> 5b416a7dcdca
Removing intermediate container 283b3b4623d7
Step 13/13 : CMD python app.py
 ---> Running in 1c776e5e0772
 ---> 1bfc1bbc968d
Removing intermediate container 1c776e5e0772
Successfully built 1bfc1bbc968d
Successfully tagged <docker-id>/myDockerImage:v1.0.0
```

Testen Sie, ob der Build funktioniert, indem Sie den Docker-Container ausführen. Rufen Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) auf, und übergeben Sie ihm Namen und Tag des Images. Sie müssen auch den Port mit dem `-p`-Argument angeben.

```bash
docker run -p 80:2222 <docker-ID>/mydockerimage:v1.0.0
```

Überprüfen Sie, ob Web-App und Container ordnungsgemäß funktionieren, indem Sie die Web-App lokal aufrufen.

![Lokales Testen der Web-App](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

## <a name="push-a-docker-image-to-docker-hub"></a>Pushübertragung eines Docker-Images auf den Docker-Hub

Eine Registrierung ist eine Anwendung, die Images hostet und Image- sowie Container-Dienste für Dienste bereitstellt. Damit Ihr Image gemeinsam verwendet werden kann, müssen Sie es per Pushübertragung an eine Registrierung senden. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Pushübertragung an eine private Docker-Registrierung? Die optionalen Anweisungen hierzu finden Sie unter [Pushübertragung eines Docker-Images an eine private Registrierung (optional)](#push-a-docker-image-to-private-registry-optional).

<!--## [Docker Hub](#tab/docker-hub)-->

Docker Hub ist eine Registrierung für Docker-Images, die Ihnen ermöglicht, Ihre eigenen Repositorys – entweder öffentlich oder privat – zu hosten. Verwenden Sie für die Pushübertragung eines benutzerdefinierten Docker-Images an den öffentlichen Docker-Hub den [docker push](https://docs.docker.com/engine/reference/commandline/push/)-Befehl, und geben Sie Image-Name und -Tag vollständig an. Ein vollständiger Name und ein vollständiges Tag des Images sehen wie folgt aus:

```bash
<docker-id>/image-name:tag
```

Wenn Sie sich noch nicht bei Docker-Hub angemeldet haben, tun Sie dies mit dem [docker login](https://docs.docker.com/engine/reference/commandline/login/)-Befehl, bevor Sie die Pushübertragung eines Images versuchen.

```bash
docker login --username <docker-id> --password <docker-hub-password>
```

Eine „login succeeded“-Meldung bestätigt Ihre erfolgreiche Anmeldung. Wenn Sie angemeldet sind, können Sie das Image mit dem [docker Push](https://docs.docker.com/engine/reference/commandline/push/)-Befehl per Pushübertragung an Docker-Hub senden.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Überprüfen Sie anhand der Befehlsausgabe, ob der Pushvorgang erfolgreich war.

```bash
The push refers to a repository [docker.io/<docker-id>/python-flask]
e9aa2c6d0f34: Pushed
0fdcb490aeec: Pushed
08ae61c7869c: Pushed
2548e7db2a94: Mounted from library/python
325b9d6f2920: Pushed
815acdffadff: Mounted from library/python
97108d083e01: Mounted from library/python
5616a6292c16: Mounted from library/python
f3ed6cb59ab0: Mounted from library/python
654f45ecb7e3: Mounted from library/python
2c40c66f7667: Mounted from library/python
v1: digest: sha256:a910d5b77e6960c01745a87c35f3d1a13ba73231ac9a4664c5011b1422d59b60 size: 2632
```

<!--
# [Private Registry](#tab/private-registry)

// Move Private Registry instructions here when Tabbed Conceptual bug is fixed

---
-->

## <a name="create-web-app-for-containers"></a>Erstellen einer Web-App für Container

Mithilfe von Azure-Web-Apps können Sie native Linux-Anwendungen in der Cloud hosten. Um eine Web-App für Container zu erstellen, müssen Sie Azure CLI-Befehle ausführen, die eine Gruppe erstellen, dann einen Dienstplan und schließlich die Web-App selbst. Führen Sie zunächst den [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create)-Befehl aus, und übergeben Sie ihm einen Speicherort und einen eindeutigen Namen.

```azurecli-interactive
az group create --location "West Europe" --name myResourceGroup
```

Daraufhin wird etwa folgende Ausgabe angezeigt:

```json
{
  "id": "/subscriptions/432849d3e4-4f90-a782-87c11e-5e59d6dd/resourceGroups/myResourceGroup",
  "location": "westeurope",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Erstellen Sie mit dem Namen der Gruppe mit dem Befehl [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) einen App Service-Plan. Sie müssen ihn auch mit einem eindeutigen Namen benennen und das `--is-linux`-Flag festlegen.

```azurecli-interactive
az appservice plan create --name myServicePlan --resource-group myResourceGroup --is-linux
```

Das Erstellen eines Dienstplans führt zu Ergebnissen, die dem folgenden Beispiel ähneln:

```json
  {- Starting...
  "adminSiteName": null,
  "appServicePlanName": "myServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/resourceGroups/myResourceGroup/provide
rs/Microsoft.Web/serverfarms/myServicePlan",
  "kind": "linux",
  "location": "West Europe", 
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "tier": "Basic"
  },
  "status": "Ready",
  "subscription": "",
  "tags": null,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

Da Sie nun Ressourcengruppe und Dienstplan erstellt haben, können Sie den [az webapp create](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create)-Befehl ausführen, um die Web-App zu erstellen. Beachten Sie, dass der Laufzeitstapel Python 3.4 ist, und die Web-App die Ressourcengruppe und das Dienstplansetup aus den vorherigen Schritten verwendet.

```azurecli-interactive
az webapp create -g myResourceGroup -p myServicePlan -n <web-app-name> --runtime "python|3.4" 
```

Der Befehl zum Erstellen einer Web-App erzeugt die hier gezeigte Ausgabe:

```json
{- Starting ..
  "availabilityState": "Normal",
   "enabled": true,
  "enabledHostNames": [
    "<web-app-name>.azurewebsites.net",
    "<web-app-name>.scm.azurewebsites.net"
  ],
  "ftpPublishingUrl": "ftp://waws-prod-am2-085.ftp.azurewebsites.windows.net/site/wwwroot",
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<web-app-name>.azurewebsites.net",
    },
  ],
  "hostNames": [
    "<web-app-name>.azurewebsites.net"
  ],
  "hostNamesDisabled": false,
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/5e59d6dd-d3e4-4f90-a782-43284987c11e/resourceGroups/myResourceGroup/providers/Microsoft.
Web/sites/<web-app-name>",
  "lastModifiedTimeUtc": "2017-08-08T21:09:33.693333",
  "location": "West Europe",
  "name": "<web-app-name>",
  "outboundIpAddresses": "13.81.108.99,52.232.76.83,52.166.73.203,52.233.173.39,52.233.159.48",
  "resourceGroup": "myResourceGroup"
}

```

Bei den meisten Web-Apps müssen Anwendungseinstellungen konfiguriert werden. Wenn Sie ein vorhandenes Docker-Image nutzen, das von einer anderen Person erstellt wurde, erfordert das Image unter Umständen einen anderen Port als Port 80 für die Anwendung. Um den `WEBSITES_PORT` festzulegen, führen Sie den [az webapp config](https://docs.microsoft.com/cli/azure/webapp/config/appsettings)-Befehl wie im folgenden Codebeispiel gezeigt aus:

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <web-app-name> --settings WEBSITES_PORT=2222
```

> [!NOTE]
> Bei Anwendungseinstellungen wird die Groß-/Kleinschreibung beachtet.
>

Stellen Sie sicher, dass die Web-App funktioniert, indem Sie sie aufrufen. Vergessen Sie die Portnummer nicht.

![Testen der Konfiguration des Web-App-Ports](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="configure-web-app-to-use-docker-container-from-docker-hub"></a>Konfigurieren der Web-App zur Verwendung des Docker-Containers aus Docker-Hub

Der [az webapp config](https://docs.microsoft.com/cli/azure/webapp/config)-Befehl ermöglicht Ihnen die Verwendung eines benutzerdefinierten Docker-Images.

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Bereitstellung aus einer privaten Docker-Registrierung? Die optionalen Anweisungen hierzu finden Sie unter [Konfigurieren einer Web-App zur Verwendung eines Docker-Containers aus einer privaten Registrierung (optional)](#configure-web-app-to-use-docker-container-from-a-private-registry-optional).

<!-- # [Docker Hub](#tab/docker-hub)-->

Um die Web-App zur Verwendung einer öffentlichen Docker-Registrierung zu konfigurieren, übergeben Sie den Namen der App, die Ressourcengruppe und den Imagenamen sowie die URL dem [az webapp config container set](https://docs.microsoft.com/cli/azure/webapp/config/container#az_webapp_config_container_set)-Befehl.

```azurecli-interactive
az webapp config container set --name <web-app-name> --resource-group myResourceGroup --docker-custom-image-name mydockerimage
--docker-registry-server-url <docker-id>/myContainerRegistry
```

Eine erfolgreiche Konfigurationsänderung gibt allgemeine Informationen über den Container zurück.

```bash
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "slotSetting": false,
    "value": "{docker-id}/mydockerimage:v1.0.0"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "{docker-id}"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  }
]
```

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

## <a name="test-the-application-in-azure"></a>Testen der Anwendung in Azure

Bevor Sie testen, müssen Sie die Web-App mit dem [az webapp restart](https://docs.microsoft.com/cli/azure/webapp#az_webapp_restart)-Befehl neu starten, damit die Konfigurationsänderungen wirksam werden.

```azurecli-interactive
az webapp restart --name <web-app-name> --resource-group myResourceGroup
```

Der Neustartbefehl startet die Web-App ohne Meldung neu, sodass Sie kein Feedback am Terminal erhalten. Sobald die Web-App ausgeführt wird, testen Sie die Web-App, indem Sie ihre URL unter `http://<username>.azurewebsites.net` aufrufen. Stellen Sie sicher, dass die App die neue Willkommensnachricht angezeigt.

![Testen der Web-App in Azure](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Ändern der Web-App und erneutes Bereitstellen

Eine Python-Datei mit dem Namen `app.py` ist im `using-custom-docker-image`-Verzeichnis vorhanden. Diese Datei enthält eine Codezeile, die eine `Hello World!`-Meldung anzeigt. Ändern Sie die Zeile, sodass sie die Meldung `Hello World of Web Apps running in Docker Containers!` anzeigt.

```python
return "Hello World of Web Apps running in Docker Containers!"
```

Nachdem Sie die Python-Datei geändert und gespeichert haben, müssen Sie sie neu erstellen und das neue Docker-Image per Pushübertragung senden. Starten Sie dann die Web-App neu, damit die Änderungen wirksam werden. Verwenden Sie die gleichen Befehle, die Sie zuvor in diesem Tutorial verwendet haben. Sie können die Abschnitte zum [Erstellen des Images aus der Docker-Datei](#build-the-image-from-the-docker-file) und zur [Pushübertragung des Docker-Images](#push-docker-image) nutzen. Testen Sie die Web-App, indem Sie die Anweisungen in [Testen der Anwendung in Azure](#tTest-the-application-in-azure) befolgen.

## <a name="connect-to-web-app-for-containers-using-ssh"></a>Herstellen einer Verbindung mit der Web-App für Container mithilfe von SSH

SSH ermöglicht die sichere Kommunikation zwischen einem Container und einem Client. Damit ein benutzerdefiniertes Docker-Image SSH unterstützen kann, muss es in einer Docker-Datei erstellt werden. Sie aktivieren SSH in der Docker-Datei selbst. Die SSH-Anweisungen wurden bereits der Docker-Beispieldatei hinzugefügt, sodass Sie diese Anweisungen mit Ihrem eigenen benutzerdefinierten Image ausführen können:

* Eine [RUN](https://docs.docker.com/engine/reference/builder/#run)-Anweisung, die `apt-get` aufruft und dann das Kennwort für das Stammkonto auf `"Docker!"` festlegt.

    ```docker
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

    > [!NOTE]
    > Diese Konfiguration erlaubt keine externen Verbindungen zum Container. SSH ist nur über die Kudu/SCM-Website verfügbar. Die Kudu/SCM-Website wird durch die Anmeldeinformationen für die Veröffentlichung authentifiziert.

* Eine [COPY](https://docs.docker.com/engine/reference/builder/#copy)-Anweisung, die das Docker-Modul anweist, die [sshd_config](http://man.openbsd.org/sshd_config)-Datei in das */etc/ssh/*-Verzeichnis zu kopieren. Die Konfigurationsdatei sollte auf der [sshd_config](https://github.com/Azure-App-Service/node/blob/master/6.11/sshd_config)-Datei im Azure-App-Service-GitHub-Repository basieren.

    > [!NOTE]
    > Die *sshd_config*-Datei muss Folgendes enthalten: 
    > * `Ciphers` muss mindestens ein Element aus dieser Liste enthalten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` muss mindestens ein Element aus dieser Liste enthalten: `hmac-sha1,hmac-sha1-96`.

    ```docker
    #Copy the sshd_config file to its new location
    COPY sshd_config /etc/ssh/
    ```

* Eine [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose)-Anweisung, die Port 2222 im Container verfügbar macht. Selbst wenn das Root-Kennwort bekannt ist, kann auf Port 2222 nicht aus dem Internet zugegriffen werden. Es handelt sich um einen internen Port, auf den nur von Containern innerhalb des Brückennetzwerks eines privaten virtuellen Netzwerks zugegriffen werden kann. Danach kopieren Befehle Details der SSH-Konfiguration und starten den `ssh`-Dienst.

    ```docker
    # Configure ports
    EXPOSE 2222 80

    #Copy the sshd_config file to its new location
    COPY sshd_config /etc/ssh/

    # Start the SSH service
    RUN service ssh start
    ```

Die `init_container.sh`-Datei im Beispielcode enthält Anweisungen zum Initialisieren des Containers, wenn er ausgeführt wird. Die Docker-Datei verwendet die Anweisungen [COPY](https://docs.docker.com/engine/reference/builder/#copy), [RUN](https://docs.docker.com/engine/reference/builder/#run) und [CMD](https://docs.docker.com/engine/reference/builder/#cmd) zum ordnungsgemäßen Starten des `init_container.sh`-Skripts.

```docker
# Copy init_container.sh to the /bin directory
COPY init_container.sh /bin/

# Run the chmod command to change permissions on above file in the /bin directory
RUN chmod 755 /bin/init_container.sh

# run commands in init_container.sh
CMD ["/bin/init_container.sh"]
```

Sie möchten möglicherweise sicherstellen, dass bestimmte Anwendungen im Container ausgeführt werden. Um den Container und die ausgeführten Prozesse zu überprüfen, beginnen Sie, indem Sie einen Browser öffnen und zu `https://<app name>.scm.azurewebsites.net/webssh/host` navigieren. Sie werden dann zu einer Seite umgeleitet, die eine interaktive Konsole anzeigt. Geben Sie den `top`-Befehl an der Eingabeaufforderung ein.

```bash
top
```

Der `top`-Befehl macht alle aktiven Prozesse in einem Container verfügbar.

```bash
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Glückwunsch! Sie haben ein benutzerdefiniertes Docker-Image für eine Azure-Web-App für Container konfiguriert.

## <a name="push-a-docker-image-to-private-registry-optional"></a>Pushübertragung eines Docker-Images an eine private Registrierung (optional)

Azure Container Registry ist ein verwalteter Docker-Dienst von Azure zum Hosten von privaten Images. Die Bereitstellungen können beliebigen Typs sein, einschließlich [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/) und Azure App Service-Containern. Die Verwendung der Azure Container Registry ist wie die Verwendung einer privaten Registrierung. Wenn Sie also Ihre eigene private Registrierung verwenden müssen, sind die Schritte zur Ausführung dieser Aufgabe ähnlich.

Erstellen Sie mit dem Befehl [az acr create](https://docs.microsoft.com/cli/azure/acr#az_acr_create) eine Azure Container Registry-Instanz. Übergeben Sie Namen, Ressourcengruppe und `Basic` für die SKU. Verfügbare SKUs sind `Classic`, `Basic`, `Standard` und `Premium`.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Das Erstellen eines Containers erzeugt die folgende Ausgabe:

```bash
 - Finished ..
Create a new service principal and assign access:
  az ad sp create-for-rbac --scopes /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --password <password>

Use an existing service principal and assign access:
  az role assignment create --scope /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --assignee <app-id>
{
  "adminUserEnabled": false,
  "creationDate": "2017-08-09T04:21:09.654153+00:00",
  "id": "/subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/{azure-container-registry-name>",
  "location": "westeurope",
  "loginServer": "<azure-container-registry-name>.azurecr.io",
  "name": "<azure-container-registry-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myazurecontainerre042025"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Zur Pushübertragung eines Images an die Registrierung müssen Sie Anmeldeinformationen angeben, damit die Registrierung den Pushvorgang akzeptiert. Sie können diese Anmeldeinformationen mit dem [az acr show](https://docs.microsoft.com/cli/azure/acr/credential#az_acr_credential_show)-Befehl abrufen. 

```azurecli-interactive
az acr credential show --name {azure-container-registry-name}
```

Der Befehl zeigt zwei Kennwörter, die mit dem Benutzernamen verwendet werden können.

```json
<
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<azure-container-registry-name>"
}
```

Nun verfügen Sie über die erforderlichen Anmeldeinformationen und können sich mit dem `docker login`-Befehl bei der Azure Container Registry anmelden. Die URL für die Registrierung ist zur Anmeldung erforderlich. Verwenden Sie das Format `http://{azure-container-registry-name>.azureacr.io`.

```bash
docker login <azure-container-registry-name>.azureacr.io --username <azure-container-registry-name> --password <password> 
```

Vergewissern Sie sich, dass die Anmeldung erfolgreich war. Senden Sie das Image mithilfe des `docker push`-Befehls per Pushübertragung, und kennzeichnen Sie das Image mit der vollständigen URL der Registrierung, gefolgt von Name und Tag des Images.

```bash
docker push http://<azure-container-registry-name>.azureacr.io/mydockerimage:v1.0.0
```

Überprüfen Sie durch Auflisten der ACR-Repositorys, ob die Pushübertragung der Registrierung erfolgreich einen Container hinzugefügt hat. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Durch Auflisten der Images in der Registrierung wird sichergestellt, dass `mydockerimage` sich in der Registrierung befindet.

```json
[
  "mydockerimage"
]
```

## <a name="configure-web-app-to-use-docker-container-from-a-private-registry-optional"></a>Konfigurieren einer Web-App zur Verwendung eines Docker-Containers aus einer privaten Registrierung (optional)

Sie können die Web-Apps unter Linux so konfigurieren, dass sie einen in der Azure Container Registry gespeicherten Container ausführen. Die Verwendung der Azure Container Registry ist wie die Verwendung einer privaten Registrierung. Wenn Sie also Ihre eigene private Registrierung verwenden müssen, sind die Schritte zur Ausführung dieser Aufgabe ähnlich.

Der Befehl [az acr credential show](https://docs.microsoft.com/cli/azure/acr/credential#az_acr_credential_show) zeigt die Kennwörter für die Containerregistrierung an. Kopieren Sie den Benutzernamen und eines der Kennwörter zur Verwendung für die Konfiguration der Web-App im nächsten Schritt.

```bash
az acr credential show --name <azure-container-registry-name>
```

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "password"
    },
    {
      "name": "password2",
      "value": "password2"
    }
  ],
  "username": "<azure-container-registry-name>"
}
```

Führen Sie den [az webapp config container set](https://docs.microsoft.com/cli/azure/webapp/config/container#az_webapp_config_container_set)-Befehl aus. Mit diesem Befehl wird das benutzerdefinierte Docker-Image der Web-App zugeordnet. Beachten Sie, dass Sie eine URL im Format `https://{your-registry-username}.azurecr.io` benötigen. Darüber hinaus benötigt die Web-App den Benutzernamen und das Kennwort, die in einem vorherigen Schritt abgerufen wurden, zum Zugriff auf die Containerregistrierung.

```azurecli-interactive
az webapp config container set --name <web-app-name> --resource-group myResourceGroup --docker-custom-image-name mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

> [!NOTE]
> `https` in der Registrierungs-URL ist erforderlich.
>

Die Ausgabe des Befehls ähnelt der folgenden JSON-Zeichenfolge und zeigt an, dass die Konfigurationsänderung erfolgreich war:

```bash
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "slotSetting": false,
    "value": "mydockerimage"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_URL",
    "slotSetting": false,
    "value": "mycontainerregistry.azurecr.io"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "{azure-container-registry-name}"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  }
]
```

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

[Azure App Service-Web-App für Container – FAQs](app-service-linux-faq.md)

