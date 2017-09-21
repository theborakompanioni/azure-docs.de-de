---
title: Erstellen von Linux-Hosts in Azure mithilfe von Docker Machine | Microsoft-Dokumentation
description: Beschreibt die Verwendung von Docker Machine zum Erstellen von Docker-Hosts in Azure.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
ms.assetid: 164b47de-6b17-4e29-8b7d-4996fa65bea4
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 7772381e9796ddc2e0db215bab4f230473eaa462
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="how-to-use-docker-machine-to-create-hosts-in-azure"></a>Erstellen von Hosts in Azure mithilfe von Docker Machine
In diesem Artikel erfahren Sie, wie Sie mithilfe von [Docker Machine](https://docs.docker.com/machine/) Hosts in Azure erstellen. Der Befehl `docker-machine` erstellt in Azure einen virtuellen Linux-Computer und installiert anschließend Docker. Danach können Sie Ihre Docker-Hosts in Azure mit den gleichen lokalen Tools und Workflows verwalten. Um Docker-Computer unter Windows 10 zu verwenden, müssen Sie Linux-Bash verwenden.

## <a name="create-vms-with-docker-machine"></a>Erstellen von virtuellen Computern mit dem Befehl „docker- machine“
Rufen Sie zunächst mit [az account show](/cli/azure/account#show) Ihre Azure-Abonnement-ID ab:

```azurecli
sub=$(az account show --query "id" -o tsv)
```

Sie erstellen virtuelle Docker-Hostcomputer in Azure mit `docker-machine create`, indem Sie *azure* als Treiber angeben. Weitere Informationen finden Sie in der [Dokumentation für den Azure-Treiber für Docker](https://docs.docker.com/machine/drivers/azure/).

Im folgenden Beispiel werden eine VM namens *myVM* basierend auf dem Plan „Standard D2 v2“ und ein Benutzerkonto namens *azureuser* erstellt. Außerdem wird der Port *80* auf der Host-VM geöffnet. Befolgen Sie alle angezeigten Anweisungen, um sich bei Ihrem Azure-Konto anzumelden und Docker Machine Berechtigungen zum Erstellen und Verwalten von Ressourcen zu erteilen.

```bash
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-size "Standard_D2_v2 \
    myvm
```

Die Ausgabe sieht in etwa wie im folgenden Beispiel aus:

```bash
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(myvmdocker) Completed machine pre-create checks.
Creating machine...
(myvmdocker) Querying existing resource group.  name="docker-machine"
(myvmdocker) Creating resource group.  name="docker-machine" location="westus"
(myvmdocker) Configuring availability set.  name="docker-machine"
(myvmdocker) Configuring network security group.  name="myvmdocker-firewall" location="westus"
(myvmdocker) Querying if virtual network already exists.  rg="docker-machine" location="westus" name="docker-machine-vnet"
(myvmdocker) Creating virtual network.  name="docker-machine-vnet" rg="docker-machine" location="westus"
(myvmdocker) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(myvmdocker) Creating public IP address.  name="myvmdocker-ip" static=false
(myvmdocker) Creating network interface.  name="myvmdocker-nic"
(myvmdocker) Creating storage account.  sku=Standard_LRS name="vhdski0hvfazyd8mn991cg50" location="westus"
(myvmdocker) Creating virtual machine.  location="westus" size="Standard_A2" username="azureuser" osImage="canonical:UbuntuServer:16.04.0-LTS:latest" name="myvmdocker"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env myvmdocker
```

## <a name="configure-your-docker-shell"></a>Konfigurieren Ihrer Docker-Shell
Definieren Sie die Verbindungseinstellungen, um eine Verbindung mit Ihrem Docker-Host in Azure herzustellen. Wie am Ende der Ausgabe angegeben, können Sie die Verbindungsinformationen für Ihren Docker-Host wie folgt anzeigen: 

```bash
docker-machine env myvmdocker
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://40.68.254.142:2376"
export DOCKER_CERT_PATH="/Users/user/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env myvmdocker)
```

Zum Definieren der Verbindungsinformationen können Sie entweder den empfohlenen Konfigurationsbefehl (`eval $(docker-machine env myvmdocker)`) ausführen oder die Umgebungsvariablen manuell festlegen. 

## <a name="run-a-container"></a>Ausführen eines Containers
Um einen Container in Aktion zu sehen, führen wir einen einfachen NGINX-Webserver aus. Erstellen Sie einen Container mit `docker run`, und machen Sie den Port 80 für Webdatenverkehr verfügbar:

```bash
docker run -d -p 80:80 --restart=always nginx
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
ff3d52d8f55f: Pull complete
226f4ec56ba3: Pull complete
53d7dd52b97d: Pull complete
Digest: sha256:41ad9967ea448d7c2b203c699b429abe1ed5af331cd92533900c6d77490e0268
Status: Downloaded newer image for nginx:latest
675e6056cb81167fe38ab98bf397164b01b998346d24e567f9eb7a7e94fba14a
```

Zeigen Sie ausgeführte Container mit `docker ps` an. Die folgende Beispielausgabe zeigt den ausgeführten NGINX-Container mit verfügbar gemachtem Port 80:

```bash
CONTAINER ID    IMAGE    COMMAND                   CREATED          STATUS          PORTS                          NAMES
d5b78f27b335    nginx    "nginx -g 'daemon off"    5 minutes ago    Up 5 minutes    0.0.0.0:80->80/tcp, 443/tcp    festive_mirzakhani
```

## <a name="test-the-container"></a>Testen des Containers
Rufen Sie die öffentliche IP-Adresse des Docker-Hosts ab:


```bash
docker-machine ip myvmdocker
```

Um den Container in Aktion zu sehen, öffnen Sie einen Webbrowser, und geben Sie die öffentliche IP-Adresse aus der Ausgabe des vorherigen Befehls ein:

![Ausführen des ngnix-Containers](./media/docker-machine/nginx.png)

## <a name="next-steps"></a>Nächste Schritte
Sie können Hosts auch mit der [Docker-VM-Erweiterung](dockerextension.md) erstellen. Beispiele zur Verwendung von Docker Compose finden Sie unter [Erste Schritte mit Docker und Compose zum Definieren und Ausführen einer Anwendung mit mehreren Containern in Azure](docker-compose-quickstart.md).

